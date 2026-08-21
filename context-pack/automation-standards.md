---
when_to_read: |
  Read before authoring automated tests or CI test steps — to use the approved framework, locator strategy, and wait/retry rules.
---

# Automation Standards

## Purpose
This file governs the test automation framework, execution strategy, reporting, and CI/CD pipeline standards for the project, ensuring consistent, maintainable, and performant automated testing across all environments.

## Scope
Applies to all teams and areas responsible for authoring, executing, and maintaining automated tests, including test infrastructure setup (ENABLER-001), the Core Navigation Framework (EPIC-001), CI/CD pipeline integration, and multi-language/multi-environment test execution (Local Development, CI/CD Pipeline, Staging Validation, Production Monitoring).

## Mandatory Rules
- Use Playwright as the primary test automation framework for cross-browser support, modern API, and performance (source: `adrs/ADR-SPEC.md` ADR-001, line 5-11).
- Follow the Page Object Model (POM) pattern for maintainable test code (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).
- Use the Playwright HTML Reporter with screenshot capture on failures for test reporting (source: `adrs/ADR-SPEC.md` ADR-004, line 29-35).
- Store test data in JSON configuration files, environment-specific, to support multi-language testing (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27).
- Execute order-dependent navigation tests sequentially; execute independent multi-language scenarios in parallel (source: `architecture/ARCH-SPEC.md` line 51-63).
- Apply automatic retry with exponential backoff for network instability and timing issues (source: `architecture/ARCH-SPEC.md` line 51-63).
- Run test execution through GitHub Actions (latest), with automated test triggers on code changes, test result artifact storage, and a failure notification system (source: `architecture/ARCH-SPEC.md` line 75-79, 89).
- Run tests in Docker 24.0+ containers for environment consistency, easy scaling, isolation from host system, and simplified dependency management (source: `architecture/ARCH-SPEC.md` line 65-73, 88).
- Support the four defined execution environments: Local Development (single machine, Chrome desktop, English), CI/CD Pipeline (containerized, Chrome headless, English), Staging Validation (cloud-based, Chrome desktop, EN/ES/FR), Production Monitoring (production environment, Chrome desktop, English) (source: `architecture/ARCH-SPEC.md` line 25-49).
- Establish CI/CD pipeline integration and test execution environment as part of ENABLER-001 (Test Infrastructure Setup), which depends on EPIC-001 (source: `epics.md` line 125-127).
- Complete test suite updates within 2 hours for UI changes, per NFR-003 Maintainability (source: `prd.md` line 69-72).
- Keep full test suite execution time under 10 minutes, per KPI-001 (source: `prd.md` line 121-124).
- Establish the foundational framework via EPIC-001 (Core Navigation Framework, Must Have, High complexity): Chrome integration, basic POM, and test reporting (source: `epics.md` line 10-23).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No prohibited locator strategies or test patterns evidence found in the inputs.

## Examples
- Valid: A navigation test suite structured with Page Object Model classes, executed sequentially for order-dependent flows, using Playwright's HTML Reporter with screenshots on failure (source: `adrs/ADR-SPEC.md` ADR-001, ADR-002, ADR-004).
- Invalid: A test suite that runs order-dependent navigation tests in parallel without retry/backoff handling for network instability, contradicting the sequential/parallel execution and retry strategy (source: `architecture/ARCH-SPEC.md` line 51-63).

## Traceability
- CI/CD Pipeline Details: `architecture/ARCH-SPEC.md` (line 25-49, 51-63, 65-73, 75-79, 88-89), `epics.md` (line 125-127), `prd.md` (line 69-72, 121-124).
- Test Automation Framework: `adrs/ADR-SPEC.md` ADR-001 (line 5-11), ADR-002 (line 13-19), ADR-003 (line 21-27), ADR-004 (line 29-35); `architecture/ARCH-SPEC.md` (line 51-63); `epics.md` (line 10-23).

## Version and Owner
- Version: 1.0
- Owner: Project Team
