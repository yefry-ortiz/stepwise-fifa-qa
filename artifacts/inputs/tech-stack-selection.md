# Tech Stack Selection

## Selected blueprints

| Layer | blueprint_id | Why selected |
|---|---|---|
| L1 (exactly one) | none | No hosting or deployment surface in scope — see Binding constraint below |
| L2 | `node-typescript` | Project inputs (ADR-SPEC, ARCH-SPEC) state TypeScript 5.0+, npm 10.0+, Playwright as the stack; `node-typescript` is the closest-fitting L2 backend/tooling blueprint for a TypeScript-native, npm-managed automation codebase |
| L4 | `security-baseline` | Always selected |
| L4 | `pre-ship-checklist` | Always selected |

No L3 blueprint selected — the project has no agentic component in any of the four analyzed inputs.

## Binding constraint

InsideFIFA-WEB is a web **test automation** project (Playwright + TypeScript) that validates navigation and functionality of an external, third-party website (`https://inside.fifa.com/`). The deliverable itself is a test suite/framework, not a hosted application or service — it has no cloud platform to select, host, or deploy to. Per Selection rule 1, zero L1 is the correct answer for a deliverable with no hosting or deployment surface, not a gap. Execution environments named in the inputs (local dev, CI/CD pipeline, staging validation, production monitoring — `ARCH-SPEC.md` lines 27-49) describe *where the tests run*, not a platform this project builds or owns.

## Accepted trade-offs

- `node-typescript`: the runtime is single-threaded for CPU-bound work and the ecosystem churns faster than JVM/.NET stacks. Accepted — this project is I/O-bound test orchestration (browser automation, HTTP calls), never CPU-bound processing, so this cost does not apply in practice.
- `node-typescript`: dependency-tree depth makes supply-chain review a standing obligation. Accepted — `security-baseline`'s supply-chain controls apply.
- `node-typescript`: not intended for a system needing decade-scale stability with minimal maintenance. Accepted — a test-automation project tracking a live external website already requires ongoing maintenance regardless of stack (see `RSK-001`, `prd.md` line 81-84).

## Deviations from blueprint mandates

| Blueprint | Rule being overridden | Justification |
|---|---|---|
| — | — | — |

None recorded. The project inputs state Playwright 1.40+/TypeScript 5.0+/npm 10.0+ (`ADR-SPEC.md` lines 39-46, `ARCH-SPEC.md` lines 83-90), which is consistent with the `node-typescript` blueprint's normative profile rather than in conflict with it.

## Project-specific resolutions

| Question (from blueprint `open_questions`) | Resolution |
|---|---|
| (none surfaced by `node-typescript`, `security-baseline`, or `pre-ship-checklist` that require a project-specific answer beyond what the inputs already state) | unresolved — see `analysis.md` Open Questions for gaps the four selected inputs leave unaddressed (e.g., no CI/CD pipeline definition file, no explicit secrets-management approach) |

## Not covered here

`domain.md` and `customer-background.md` derive from project and customer inputs — the project brief, requirements, transcripts, and source code — not from this file or from any blueprint.
