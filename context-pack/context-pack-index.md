---
when_to_read: |
  Read FIRST on entering the project — to discover which context-pack files exist and when each one applies.
---

# Context Pack Index

## Purpose
ROUTING: Entry index for the InsideFIFA-WEB context pack — one line per generated file, what it governs, and when to read it. Generated from `artifacts/outputs/context-pack-analysis/analysis.md` (session `ANALYZE-STEPWISE-FIFA-QA-20260821`) by session `CTXPACK-STEPWISE-FIFA-QA-20260821`.

## Scope
All context-pack files produced in this run, covering the InsideFIFA-WEB test-automation project (Playwright + TypeScript, validating `https://inside.fifa.com/`).

## Mandatory Rules

| File | Governs | Read when |
|---|---|---|
| `project-inventory.md` **(ROUTING)** | Dense lookup: bounded contexts, naming conventions, stable invariants, build/test commands | FIRST — before locating code or planning any change |
| `codebase-map.md` **(ROUTING)** | Narrative solution structure, bounded contexts, layer conventions, where-to-look guidance | FIRST — before scanning the tree or planning where a change goes |
| `ux-design.md` **(ROUTING)** | User journeys, interactive components, responsive breakpoints/viewports under test | Before writing/reviewing UI/navigation test scenarios or choosing viewports |
| `domain.md` | Domain terms, entities, business rules and invariants | Before modeling entities, naming domain concepts, or writing business logic |
| `arch-standards.md` | Architecture patterns (POM, sequential/parallel/retry execution, service catalog) | Before adding a component/service or wiring an integration |
| `tech-policy.md` | Approved stack: Playwright, TypeScript, npm, Docker, GitHub Actions and versions | Before adding a dependency or choosing a framework/library/runtime version |
| `coding-standards.md` | Naming conventions, code organization, POM structure, lint/format rules | Before writing or modifying any source file |
| `constraints.md` | Hard limits: site availability, coverage gate, epic dependencies, priority/complexity distribution | Before making any design or scoping decision |
| `security.md` | Auth posture (none required), blueprint-inherited security floor, risk mitigations | Before implementing auth, handling secrets, or validating input |
| `customer-background.md` | Project identity, primary/secondary users, target-system ownership boundary | Before prioritizing work or making a product trade-off |
| `test-standards.md` | Pass-rate/coverage thresholds, quality gates Gate-001/002/003, test framework | Before writing or reviewing tests, or setting coverage gates |
| `automation-standards.md` | Automation framework (Playwright, POM, reporting), CI/CD pipeline details | Before authoring automated tests or CI test steps |
| `test-data-policy.md` | Test data format (JSON, ADR-003), medium-risk data maintenance note | Before creating or loading test data |
| `env-config.md` | Four execution environments (Local, CI/CD, Staging, Production) and their browser/language matrix | Before running against an environment or setting up test accounts |
| `config-management-rules.md` | Config-file conventions (JSON), Configuration Service (SVC-003), secret-management gap | Before adding config/environment variables or injecting secrets |
| `best-practices.md` | Cross-cutting practices: robust selectors, risk mitigations, POM separation of concerns | When a cross-cutting question arises not covered by a more specific file |
| `infrastructure.md` | Docker containerized execution, no dedicated hosting surface, deployment environments (ENV-001..004) | Before provisioning resources or configuring deployment/networking |
| `validation-tools.md` | Quality-gate order (Gate-001→002→003) and gate contents; literal commands pending | Before running build/test/lint/typecheck/security checks or wiring CI |

## Traceability
- All 18 files above derive from `artifacts/outputs/context-pack-analysis/analysis.md` (produced by `analyze-context-pack-inputs`, session `ANALYZE-STEPWISE-FIFA-QA-20260821`), which itself derives from `artifacts/inputs/documentation/prd.md`, `epics.md`, `adrs/ADR-SPEC.md`, `architecture/ARCH-SPEC.md`, and the staged blueprints `node-typescript` (L2), `security-baseline` (L4), `pre-ship-checklist` (L4) per `artifacts/inputs/tech-stack-selection.md`.
- Several files carry `[TO BE COMPLETED]` or `<unverified>` / `<unknown — verify>` markers where the four analyzed project inputs had no evidence (see analysis.md `## Open Questions` OQ-01 through OQ-06 for the tracked gaps: literal build/test/lint/security commands, staging-vs-production test target, secrets existence, repository paths for SVC-001..004, and error-handling/logging conventions).

## Version and Owner
- Version: 1.0
- Owner: Project Team
