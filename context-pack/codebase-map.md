---
when_to_read: |
  Read FIRST, before scanning the tree or planning where a change goes — to orient on solution structure, bounded contexts, layers, and where to look for X.
---

# Codebase Map

## Purpose
ROUTING: This test-automation solution is organized around four architectural service boundaries — Test Execution Engine, Page Object Management, Test Configuration Management, and Test Results and Reporting — with intra-suite execution patterns (sequential, parallel, retry) coordinating them. No repository source code was in scope for the analysis that produced this map (only documentation inputs — `adrs`, `architecture`, `epics.md`, `prd.md` — were analyzed), so directory-level structure, cross-service integration patterns, test-file layout, and "where to look for X" paths remain unstated and marked `<unknown — verify>` pending a source-code-inclusive analysis run.

## Scope
This test-automation solution: the four architectural service boundaries (Test Execution Engine, Page Object Management, Test Configuration Management, Test Results and Reporting) and their intra-suite execution.

## Mandatory Rules
### Solution Structure
<unknown — verify>: no repository source code was in scope for the analysis run that produced this map (only documentation inputs were analyzed); no directory structure is stated in any analyzed input.

### Bounded Contexts in Scope
- Test Execution Engine, Page Object Management, Test Configuration Management, Test Results and Reporting form the four architectural service boundaries (source: `architecture/ARCH-SPEC.md` line 3-23).

### Layer Conventions
- Package by feature, not by technical layer; route -> service -> repository style layering with domain logic independent of the HTTP framework (origin: blueprint `node-typescript` §3, §5).
- Project-stated convention: Page Object Model separates page objects from test logic (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).

### Cross-BC Integration Patterns
<unknown — verify>: only intra-suite execution patterns are documented (sequential, parallel, retry — `architecture/ARCH-SPEC.md` line 51-63); no integration pattern between SVC-001..004 themselves is stated.

### Frontend Layout
Not applicable: this project is a test-automation suite, not a frontend application.

### Test Layout
<unknown — verify>: no test-file directory layout is stated; only the logical service split (SVC-001..004) and the POM pattern are documented (source: `architecture/ARCH-SPEC.md` line 3-23; `adrs/ADR-SPEC.md` line 13-19).

### Where to Look for X
<unknown — verify>: cannot be populated — no file/directory paths exist in any analyzed input (source: `architecture/ARCH-SPEC.md` open_questions, line 134-135).

## Recommended Rules
- Keep this map concise and navigation-oriented; re-run analysis with `artifacts/inputs/source-code/` in scope to populate the pending sections.

## Restrictions and Prohibitions
- Do not turn this file into a raw inventory table dump — see `project-inventory.md` for that.
- Do not include roadmap/history.
- Do not invent paths or responsibilities not present in the excerpt.

## Examples
- Valid: marking Solution Structure `<unknown — verify>` rather than guessing a directory layout.
- Invalid: inventing a `src/pages/` directory path not stated in any analyzed input.

## Exceptions
- Sections with incomplete evidence keep explicit `<unknown — verify>` markers rather than being silently omitted.

## Traceability
- Bounded Contexts in Scope: `architecture/ARCH-SPEC.md` line 3-23.
- Layer Conventions: blueprint `node-typescript` §3, §5; `adrs/ADR-SPEC.md` ADR-002, line 13-19.
- Cross-BC Integration Patterns: `architecture/ARCH-SPEC.md` line 51-63.
- Test Layout: `architecture/ARCH-SPEC.md` line 3-23; `adrs/ADR-SPEC.md` line 13-19.
- Where to Look for X: `architecture/ARCH-SPEC.md` open_questions, line 134-135.

## Version and Owner
- Version: 1.0
- Owner: Project Team
