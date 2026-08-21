---
when_to_read: |
  Read before adding a dependency, choosing a framework/library, or picking a runtime/version — to stay within the approved stack and licensing.
---

# Tech Policy

## Purpose
This file governs the approved test/automation technology stack, language, tooling, and dependency versions for the project, as detected from architecture and ADR specifications, ensuring consistency and traceability for all automation and infrastructure choices.

## Scope
Applies to the automation engineering team and any contributors building, extending, or maintaining the test/automation framework, CI/CD pipelines, and containerized test execution environments for this project.

## Mandatory Rules
- Use Playwright as the test/automation framework, selected for cross-browser support, modern API, and performance (source: `adrs/ADR-SPEC.md` ADR-001, line 5-11; `architecture/ARCH-SPEC.md` line 85).
- Use TypeScript 5.0+ for all automation code to ensure type-safe development (source: `adrs/ADR-SPEC.md` line 19, 42; `architecture/ARCH-SPEC.md` line 86).
- Use npm 10.0+ as the package manager / build tool (source: `adrs/ADR-SPEC.md` line 43; `architecture/ARCH-SPEC.md` line 87).
- Use Playwright Test 1.40+ as the test runner (source: `adrs/ADR-SPEC.md` line 44).
- Use Playwright HTML Reporter 1.40+ for reporting, with screenshot capture on failures (source: `adrs/ADR-SPEC.md` ADR-004, line 29-35, 45; `architecture/ARCH-SPEC.md` line 90).
- Use JSON configuration files, environment-specific, for test data configuration to support multi-language testing (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27, 46).
- Use Docker 24.0+ as the container platform for consistent, isolated test execution (source: `architecture/ARCH-SPEC.md` line 67-73, 88).
- Use GitHub Actions (latest) as the CI/CD platform for automated test triggers, result artifact storage, and failure notifications (source: `architecture/ARCH-SPEC.md` line 75-79, 89).
- Approved library versions: Playwright v1.40+, TypeScript 5.0+, npm 10.0+, Docker 24.0+, GitHub Actions (latest) (source: `adrs/ADR-SPEC.md` line 11, 41-43; `architecture/ARCH-SPEC.md` line 85-89).

## Recommended Rules
- [INFERRED] Follow the TypeScript/Node.js coding, testing, and validation-tooling conventions supplied by the staged L2 blueprint `node-typescript` (version 1.0) to fill gaps not otherwise specified by the project inputs (origin: blueprint, `blueprint_id: node-typescript`, `blueprint_version: 1.0`).

## Restrictions and Prohibitions
- [TO BE COMPLETED] No prohibited or deprecated dependencies evidence found in the inputs (status: pending - no evidence in inputs).

## Examples
- Valid: Using Playwright v1.40+ with TypeScript 5.0+, npm 10.0+, running inside Docker 24.0+, orchestrated via GitHub Actions (latest).
- Invalid: Introducing a different test framework (e.g., Cypress or Selenium) not named in the approved stack, or using a language other than TypeScript 5.0+ for automation code.

## Traceability
- Tech Stack Detected — Playwright, TypeScript, npm, Playwright Test, Playwright HTML Reporter, JSON test data config, Docker, GitHub Actions: sourced from `adrs/ADR-SPEC.md` (ADR-001 line 5-11; line 19, 42-46; ADR-003 line 21-27; ADR-004 line 29-35) and `architecture/ARCH-SPEC.md` (line 67-73, 85-90) — from `artifacts/outputs/context-pack-analysis/analysis.md`, section "For tech-policy.md".
- Node.js/TypeScript conventions rule — origin: blueprint (`blueprint_id: node-typescript`, `blueprint_version: 1.0`), staged per `tech-stack-selection.md`; this fills gaps not otherwise specified by project inputs rather than being a project-stated fact.
- Approved Libraries and Versions — sourced from `adrs/ADR-SPEC.md` (line 11, 41-43) and `architecture/ARCH-SPEC.md` (line 85-89).
- Prohibited or Deprecated Dependencies — status: pending, no evidence in inputs.

## Version and Owner
- Version: 1.0
- Owner: Project Team
