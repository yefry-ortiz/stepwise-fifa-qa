---
when_to_read: |
  Read FIRST, before locating code or planning any change — to resolve which project, namespace, key type, or command applies without crawling the tree.
---

# Project Inventory

## Purpose
ROUTING: dense lookup table for locating the right project, namespace, class, or file without crawling the tree — covers the four bounded contexts (SVC-001..004), the delivery-scope inventory (EPIC/ENABLER/SPIKE), and the stable architectural invariants (ADR-001..004) derived from the analyzed inputs.

## Scope
All bounded contexts covered by this inventory (SVC-001..004) and the delivery-scope inventory (EPIC-001..007, ENABLER-001..003, SPIKE-001..003).

## Mandatory Rules
- Include ONLY verified rows below; mark unknown cells `<unverified>`. Naming convention: `camelCase` (values/functions), `PascalCase` (types/classes), `kebab-case` (filenames) (origin: blueprint `node-typescript` §5).

### Bounded Context Table
| Service | Responsibility | Slices | Source |
|---|---|---|---|
| SVC-001 Test Execution Engine | core service running web automation tests | Test runner, Browser management, Test orchestration | `architecture/ARCH-SPEC.md` line 5-8 |
| SVC-002 Page Object Management | manages page object models and element interactions | Element locators, Page actions, Validation helpers | `architecture/ARCH-SPEC.md` line 10-13 |
| SVC-003 Test Configuration Management | handles test data, environments, language configs | Data loading, Environment switching, Language management | `architecture/ARCH-SPEC.md` line 15-18 |
| SVC-004 Test Results and Reporting | generates test reports, captures execution metrics | Report generation, Screenshot capture, Metrics collection | `architecture/ARCH-SPEC.md` line 20-23 |

### Delivery-Scope Inventory
| ID range | Kind | Source |
|---|---|---|
| EPIC-001..007 | Epics | `epics.md` line 10-152 |
| ENABLER-001..003 | Enablers | `epics.md` line 10-152 |
| SPIKE-001..003 | Spikes | `epics.md` line 10-152 |

### Key Types per Bounded Context
| Bounded Context | Key Types | Path |
|---|---|---|
| SVC-001..004 | `<unverified>` | `<unverified>` |
Status: pending - no evidence in inputs (no file/directory paths or type/class names given).

### Build and Test Commands
| Command | Value |
|---|---|
| build | `<unverified>` |
| test | `<unverified>` |
| lint | `<unverified>` |
| typecheck | `<unverified>` |
Status: pending - no evidence in inputs; see `validation-tools.md`.

### Domain to Path Lookup
| Domain / Service | Path |
|---|---|
| SVC-001..004 | `<unverified>` |
Status: pending - no evidence in inputs.

### Stable Invariants
| Invariant | ADR | Status |
|---|---|---|
| Playwright is the fixed test automation framework | ADR-001 | Accepted |
| Page Object Model is the fixed architectural pattern | ADR-002 | Accepted |
| JSON is the fixed test-data configuration format | ADR-003 | Accepted |
| Playwright HTML Reporter is the fixed reporting mechanism | ADR-004 | Accepted |

## Recommended Rules
- Group rows by bounded context; naming pattern hints above help quick navigation.

## Restrictions and Prohibitions
- Do not duplicate the narrative structure from `codebase-map.md`.
- Do not invent commands, project types, or ownership mappings not present in the excerpt.

## Examples
- Valid: `<unverified>` cell left blank rather than filled with a guessed path.
- Invalid: inventing a directory path for SVC-001 that is not in any analyzed input.

## Exceptions
- Partial rows are allowed only when source evidence is incomplete; uncertainty stays explicit via `<unverified>` / pending markers.

## Traceability
- Naming Conventions and Patterns: blueprint `node-typescript` §5 (not stated in the four project inputs analyzed).
- Project and Bounded Context Inventory: `architecture/ARCH-SPEC.md` line 5-23; `epics.md` line 10-152.
- Key Types per Bounded Context: pending — `architecture/ARCH-SPEC.md` open_questions.
- Build and Test Commands: pending — only tool names (npm, Playwright Test, ESLint, TypeScript) stated, no literal commands.
- Domain to Path Lookup: pending — `architecture/ARCH-SPEC.md` open_questions, line 134-135.
- Stable Invariants: `adrs/ADR-SPEC.md` line 5-35 (ADR-001 through ADR-004).

## Version and Owner
- Version: 1.0
- Owner: Project Team
