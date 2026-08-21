# Architectural Blueprint: Node.js / TypeScript (Backend)

> **Thesis** — One language across client and server: the type contract spans the whole system,
> and the cost is a runtime with weak CPU parallelism and an ecosystem that shifts underfoot.
> **Best fit** — I/O-bound APIs and BFFs · teams sharing types between web, mobile, and server ·
> agentic workloads, where the framework ecosystem is native.
> **Anti-fit** — CPU-bound workloads · long-lived enterprise systems needing decade-scale
> stability · domains requiring heavyweight transactional guarantees.
> **Layer** — L2 backend. Composes with any L1 platform; pairs naturally with `react-web`.

---

## 1. Executive Summary

The reason to choose this stack is end-to-end type safety. A schema defined once flows into the
server, the web client, and the mobile client, and a breaking change fails the build rather
than production. That property is worth more than raw runtime performance for most product
work.

The honest costs: the runtime is single-threaded for CPU work, the ecosystem churns faster than
any other stack here, and dependency-tree depth makes supply-chain review a standing obligation
rather than a one-time task.

## 2. Language, Runtime, and Framework Baseline

* **Language:** TypeScript in strict mode. Strict mode is not negotiable — the stack's entire
  value proposition is the type system, and a non-strict configuration forfeits most of it.
* **Runtime:** Node.js on an active LTS release. Alternative runtimes are viable where their
  constraints are understood, but Node remains the compatibility baseline.
* **Framework:** Fastify or Nest depending on the domain. Nest supplies dependency injection,
  module boundaries, and convention — worth its weight on large domains and a tax on small
  ones. Fastify is the right default for focused services.
* **Package manager:** pnpm with workspaces, for strict dependency resolution and monorepo
  support.

## 3. Application Architecture and Layering

* **Layering:** Route -> service -> repository, with domain logic independent of the HTTP
  framework so it is testable without a server.
* **Validation at the boundary.** A schema validator parses every inbound payload and derives
  the TypeScript type from the schema — never the reverse. Types are erased at runtime;
  validation is what actually protects the system.
* **Async and concurrency:** Everything is async by default. CPU-bound work belongs in worker
  threads or a different service — a synchronous loop blocks the event loop and stalls every
  concurrent request, which presents as total unresponsiveness rather than slowness.
* **Error handling:** A typed error hierarchy mapped centrally to responses. Unhandled promise
  rejections must crash the process rather than being suppressed; a process in an unknown state
  is more dangerous than a restarted one.

## 4. Data Access and Persistence

* **ORM / query builder:** A type-safe query builder or ORM where schema types are generated
  from the database schema and flow into application code.
* **Migrations:** Versioned, forward-only, checked in, applied as a deliberate step.
* **Connection pooling is the recurring failure on serverless L1 platforms.** Request-scoped
  compute multiplies connections until the database ceiling is hit. Use a pooler, and size it
  against the database, not the application.
* **Transactions:** Explicit and scoped to the use case; never spanning an HTTP call to another
  service.

## 5. Project Structure and Code Organization

```
apps/api/src/
  modules/<feature>/    routes, service, repository, schemas
  shared/               cross-cutting utilities
packages/
  core/                 domain logic and validators shared with clients
  db/                   schema and migrations
```

* **Package by feature, not by technical layer.** Top-level `controllers/`, `services/`,
  `utils/` directories obscure boundaries as the codebase grows.
* **Naming:** `camelCase` for values and functions, `PascalCase` for types and classes,
  `kebab-case` for filenames. One exported concern per file where practical.
* **`any` is prohibited.** Use `unknown` and narrow. An `any` in the diff is a defect, not a
  style preference — it silently disables checking for everything downstream.
* **No default exports** — they defeat rename refactoring and make imports inconsistent.
* **Logging:** Structured JSON via a fast logger, with redaction configured for sensitive
  fields. `console.log` is prohibited outside scripts.

## 6. Build, Test, and CI

* **Build:** TypeScript compilation or a bundler, with a task-graph runner in monorepos so only
  changed packages rebuild.
* **Test:** Vitest or the runtime's built-in test runner for unit tests; Supertest or the
  framework's injection API for HTTP-level tests; Testcontainers for real database engines.
* **Type checking is a separate CI gate from the build.** A bundler that strips types without
  checking them will happily ship type errors.
* **Lint and format** enforced in CI, never reviewed by humans.

## 7. AI and Agent Integration

This is the stack's strongest differentiator relative to the JVM and .NET. The major agent
frameworks and both vendor agent SDKs ship first-class TypeScript support, MCP servers are
straightforward to author here, and the same validators used for HTTP boundaries define tool
schemas. Agentic control flow can run **in-process** alongside domain logic rather than behind
an HTTP hop.

Where the project is agentic, this is a significant argument for the stack — see the L3
blueprints for framework selection.

## 8. Security

Inherits the L4 baseline. Stack specifics:

* **Dependency depth is the primary risk.** The transitive graph is deeper than any other stack
  here. Audit in CI, pin versions, and review what AI assistants add.
* Validate and narrow every external input at the boundary; a parsed schema is the trust
  boundary.
* Never interpolate into SQL; use the query builder's parameterization.
* Prototype-pollution-prone patterns (deep merges of untrusted objects) are prohibited.
* Set security headers explicitly through middleware.

## 9. Observability

OpenTelemetry auto-instrumentation covers the common frameworks and database drivers with
minimal wiring. Structured JSON logs with a correlation identifier propagated through async
context. Event-loop lag is a first-class metric on this stack and should be alerted on — it is
the leading indicator of the blocking failure described in §3.

## 10. Playbook

1. **TypeScript strict mode, and `any` is prohibited.** The type system is the reason to be
   here; weakening it forfeits the choice.
2. **Validate at the boundary and derive types from schemas.** Types vanish at runtime;
   validators do not.
3. **Never block the event loop.** CPU-bound work goes to a worker thread or another service.
4. **Pool database connections against the database ceiling,** especially on request-scoped
   serverless compute.
5. **Treat the dependency tree as an attack surface.** Audit in CI, pin, and review additions.

## 11. Cost and Performance Posture

* **What scales the cost:** Instance count under concurrent I/O. Memory per instance is low,
  which makes this stack cheap to scale horizontally.
* **The trap:** A single synchronous CPU-bound operation stalling all concurrent requests. It
  looks like an outage, not a slowdown, and load tests that measure throughput alone miss it.
* **Controls:** Event-loop-lag alerting, worker threads for CPU work, connection pooling.

## 12. Environments and Configuration

Environment variables validated through a schema at startup, so a missing or malformed value
fails immediately rather than at first use. Never read raw `process.env` outside that module.
Secrets from the platform secret store.

## 13. Compliance and Exit Path

* **Exit cost:** Low. TypeScript and the frameworks are open source and portable across every
  L1 platform in this library. The real coupling is ecosystem churn — the maintenance cost is
  ongoing rather than at exit.

---

## When Not to Choose Node.js / TypeScript

1. The workload is CPU-bound — image, video, or numerical processing belongs elsewhere.
2. The system must remain stable for a decade with minimal maintenance; ecosystem churn makes
   that expensive.
3. The domain needs heavyweight transactional semantics that the JVM and .NET ecosystems
   support more maturely.

---

## 14. Normative Profile

```yaml
blueprint_id: node-typescript
blueprint_version: 1.0
layer: L2
role: backend

tech_signals:
  - signal: "TypeScript in strict mode"
    evidence: "§2"
  - signal: "Node.js on an active LTS release"
    evidence: "§2"
  - signal: "Fastify for focused services, or NestJS for large domains needing DI and module boundaries"
    evidence: "§2"
  - signal: "pnpm with workspaces for dependency resolution and monorepo support"
    evidence: "§2"
  - signal: "Zod or an equivalent schema validator at every boundary, with types derived from schemas"
    evidence: "§3"
  - signal: "Drizzle or Prisma with schema types generated from the database"
    evidence: "§4"
  - signal: "Vitest for unit tests; Testcontainers for real database engines"
    evidence: "§6"
  - signal: "Pino or an equivalent structured JSON logger with field redaction"
    evidence: "§5"
  - signal: "OpenTelemetry auto-instrumentation"
    evidence: "§9"
  - signal: "First-class TypeScript support in L3 agent frameworks and vendor agent SDKs; MCP servers authored in-stack"
    evidence: "§7"

standards:
  - area: coding
    rule: "Enable TypeScript strict mode; non-strict configuration is prohibited."
    evidence: "§2, §10.1"
  - area: coding
    rule: "The any type is prohibited; use unknown and narrow. An any in a diff is a defect, not a style preference."
    evidence: "§5, §10.1"
  - area: coding
    rule: "Derive TypeScript types from validation schemas, never define schemas from types."
    evidence: "§3, §10.2"
  - area: coding
    rule: "Package by feature; top-level technical-layer directories (controllers/, services/, utils/) are prohibited."
    evidence: "§5"
  - area: coding
    rule: "Values and functions are camelCase, types and classes PascalCase, filenames kebab-case."
    evidence: "§5"
  - area: coding
    rule: "Default exports are prohibited; they defeat rename refactoring and make imports inconsistent."
    evidence: "§5"
  - area: coding
    rule: "Use a structured JSON logger with redaction; console.log outside scripts is prohibited."
    evidence: "§5"
  - area: coding
    rule: "Read environment variables only through a startup-validated schema module; raw process.env access elsewhere is prohibited."
    evidence: "§12"
  - area: arch
    rule: "Validate and parse every inbound payload at the boundary; the parsed schema is the trust boundary."
    evidence: "§3, §10.2"
  - area: arch
    rule: "Never block the event loop; CPU-bound work belongs in a worker thread or a separate service."
    evidence: "§3, §10.3"
  - area: arch
    rule: "Let unhandled promise rejections crash the process; suppressing them is prohibited."
    evidence: "§3"
  - area: arch
    rule: "Keep domain logic independent of the HTTP framework so it is testable without a server."
    evidence: "§3"
  - area: arch
    rule: "Scope transactions to a use case; a transaction must never span an HTTP call to another service."
    evidence: "§4"
  - area: arch
    rule: "Pool database connections and size the pool against the database ceiling, especially on request-scoped serverless compute."
    evidence: "§4, §10.4"
  - area: arch
    rule: "Map errors through a typed error hierarchy to responses centrally."
    evidence: "§3"
  - area: test
    rule: "Run type checking as a CI gate separate from the build; a bundler that strips types without checking them will ship type errors."
    evidence: "§6"
  - area: test
    rule: "Test persistence against the real database engine using Testcontainers."
    evidence: "§6"
  - area: test
    rule: "Load tests must exercise concurrency, not throughput alone, to surface event-loop blocking."
    evidence: "§11"
  - area: security
    rule: "Audit the dependency tree in CI, pin versions, and review dependencies added by AI assistants; transitive depth is this stack's primary security risk."
    evidence: "§8, §10.5"
  - area: security
    rule: "Use query-builder parameterization; SQL string interpolation is prohibited."
    evidence: "§8"
  - area: security
    rule: "Deep merges of untrusted objects are prohibited (prototype pollution)."
    evidence: "§8"
  - area: security
    rule: "Set security headers explicitly through middleware."
    evidence: "§8"
  - area: automation
    rule: "Enforce lint and format in CI; style is never reviewed by humans."
    evidence: "§6"
  - area: automation
    rule: "Use a task-graph runner in monorepos so only changed packages rebuild."
    evidence: "§6"
  - area: automation
    rule: "Alert on event-loop lag as a leading indicator of blocking failures."
    evidence: "§9, §11"

constraints:
  - constraint: "Migrations are versioned, forward-only, checked in, and applied as a deliberate step."
    source: "§4"
  - constraint: "Secrets come from the platform secret store, never from committed files."
    source: "§12"
  - constraint: "CPU-bound workloads are out of scope for this stack."
    source: "§11, When Not to Choose"

infra:
  - item: "Connection pooling"
    detail: "External pooler required on request-scoped serverless compute; sized against the database ceiling."
    evidence: "§4"
  - item: "Observability"
    detail: "OpenTelemetry auto-instrumentation; structured JSON logs with correlation id through async context; event-loop lag as a first-class metric."
    evidence: "§9"
  - item: "Agent hosting"
    detail: "Agentic control flow can run in-process alongside domain logic; no HTTP hop required."
    evidence: "§7"

commands:
  - purpose: build
    command: "pnpm build"
    evidence: "§6"
  - purpose: typecheck
    command: "pnpm tsc --noEmit"
    evidence: "§6"
  - purpose: test
    command: "pnpm vitest run"
    evidence: "§6"
  - purpose: lint
    command: "pnpm lint"
    evidence: "§6"
  - purpose: security
    command: "pnpm audit --audit-level=high"
    evidence: "§8"

open_questions:
  - question: "Fastify or NestJS?"
    why: "Determined by domain size; Nest's conventions are a tax on small services."
  - question: "Which ORM or query builder?"
    why: "Team preference; the requirement is that schema types are generated from the database."
  - question: "Is an external connection pooler required?"
    why: "Determined by whether the L1 compute tier is request-scoped."
```
