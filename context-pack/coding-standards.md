---
when_to_read: |
  Read before writing or modifying any source file — choosing names, structuring modules/imports, handling errors, or adding logging.
---

# Coding Standards

## Purpose
This file governs naming conventions, code organization, and linting/formatting enforcement for the project's source code, as derived from the analysis of project inputs (PRD, epics, ADR-SPEC, ARCH-SPEC) and the staged `node-typescript` L2 blueprint.

## Scope
Applies to all automation and application source code contributors, including engineers implementing page objects, element locators, page actions, validation helpers, and any TypeScript/Node.js code in the project.

## Mandatory Rules
- Values and functions must be `camelCase`, types and classes must be `PascalCase`, and filenames must be `kebab-case` (origin: blueprint `node-typescript` §5).
- Follow the Page Object Model (POM) pattern: separate page objects, element locators, page actions, and validation helpers from test logic (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19; `architecture/ARCH-SPEC.md` SVC-002, line 10-13).
- Organize code by feature, not by technical layer (origin: blueprint `node-typescript` §5).
- The `any` type is prohibited; use `unknown` and narrow instead (origin: blueprint `node-typescript` §5, §10.1).
- Default exports are prohibited (origin: blueprint `node-typescript` §5).
- TypeScript strict mode is required (origin: blueprint `node-typescript` §2, §10.1).
- ESLint compliance is a Gate-001 (Code Quality) pass condition (source: `architecture/ARCH-SPEC.md` line 92-97).
- Lint and format checks are enforced in CI and are never reviewed by humans; formatting is machine-enforced (origin: blueprint `node-typescript` §6; blueprint `pre-ship-checklist` Gate 0, §2).

## Recommended Rules
- [INFERRED] Typed error hierarchy mapped centrally to responses, and structured JSON logging with redaction (origin: blueprint `node-typescript` §3, §5) — a blueprint default, not a project-stated requirement; see Open Questions OQ-05 in analysis.md.

## Restrictions and Prohibitions
- Top-level `controllers/`, `services/`, and `utils/` directories are prohibited; package by feature instead (origin: blueprint `node-typescript` §5).
- The `any` type is prohibited (origin: blueprint `node-typescript` §5, §10.1).
- Default exports are prohibited (origin: blueprint `node-typescript` §5).
- Non-strict TypeScript configuration is prohibited; strict mode is required (origin: blueprint `node-typescript` §2, §10.1).
- Human review of lint/format issues is prohibited; these checks must be machine-enforced in CI (origin: blueprint `node-typescript` §6; blueprint `pre-ship-checklist` Gate 0, §2).

## Examples
- Valid: A page object file named `login-page.ts` exporting a named `LoginPage` class with `camelCase` methods (e.g., `submitForm()`), organized under a feature directory rather than a generic `controllers/` or `utils/` folder.
- Invalid: A file named `LoginPage.ts` (not `kebab-case`) with a default export, methods typed as `any`, and validation logic mixed directly into test files instead of separated per the POM pattern.

## Traceability
- Naming Conventions: `origin: blueprint node-typescript §5` — no naming convention was stated in the four project inputs analyzed; supplied by the blueprint per the tech-stack-selection precedence rule (blueprint fills gaps where inputs are silent).
- Code Organization (POM pattern): `source: adrs/ADR-SPEC.md` ADR-002 (line 13-19) and `architecture/ARCH-SPEC.md` SVC-002 (line 10-13) — project-stated requirement.
- Code Organization (package by feature, no `any`, no default exports, strict mode): `origin: blueprint node-typescript §5, §2, §10.1` — blueprint-supplied, project inputs silent on these rules.
- Linting and Formatting: ESLint gate `source: architecture/ARCH-SPEC.md` line 92-97 (project-stated); CI enforcement and machine-only review `origin: blueprint node-typescript §6` and `blueprint pre-ship-checklist Gate 0, §2` (blueprint-supplied).
- Error Handling and Logging: status `pending — no evidence in inputs`; recommendation is `[INFERRED]` from `blueprint node-typescript §3, §5`, not a project-stated requirement; flagged in Open Questions (OQ-05) of analysis.md.

## Version and Owner
- Version: 1.0
- Owner: Project Team
