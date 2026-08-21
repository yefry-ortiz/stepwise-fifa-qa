---
when_to_read: |
  Read before adding a component/service, wiring an integration, or placing code across layers — to respect layer boundaries and approved integration patterns.
---

# Architecture Standards

## Purpose
This file governs the architecture patterns adopted for the test automation framework, including test code organization, test execution strategies, and the logical service catalog that structures the framework's components.

## Scope
Applies to the test automation framework's architecture: TypeScript test code structure, test execution orchestration (sequential and parallel), test environment consistency (containerization), and the four logical services (Test Execution Engine, Page Object Management, Test Configuration Management, Test Results and Reporting).

## Mandatory Rules
- Test code MUST follow the Page Object Model (POM) pattern in TypeScript for maintainable test code and separation of concerns (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).
- Navigation tests where order matters MUST use Sequential Test Execution to avoid resource conflicts (source: `architecture/ARCH-SPEC.md` line 53-55).
- Independent multi-language test scenarios MUST use Parallel Test Execution for faster execution (source: `architecture/ARCH-SPEC.md` line 57-59).
- Failed tests MUST automatically retry using the Retry Mechanism pattern with exponential backoff to handle network instability and timing issues (source: `architecture/ARCH-SPEC.md` line 61-63).
- Tests MUST run in Docker containers under the Containerized Test Execution strategy to ensure consistency across environments (source: `architecture/ARCH-SPEC.md` line 67-73).
- The framework MUST be organized into four logical services: Test Execution Engine (test runner, browser management, test orchestration), Page Object Management (element locators, page actions, validation helpers), Test Configuration Management (data loading, environment switching, language management), and Test Results and Reporting (report generation, screenshot capture, metrics collection) (source: `architecture/ARCH-SPEC.md` line 5-23).

## Recommended Rules
None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No architecture anti-patterns evidence found in the inputs (status: pending - no evidence in inputs).

## Examples
- Valid: A new navigation test suite is added and configured to run sequentially, per the Sequential Test Execution pattern, to avoid resource conflicts (source: `architecture/ARCH-SPEC.md` line 53-55).
- Invalid: No documented invalid example is available; the "Architecture Anti-Patterns Found" section reports status: pending - no evidence in inputs.

## Traceability
- Page Object Model pattern: `adrs/ADR-SPEC.md` ADR-002, line 13-19.
- Sequential Test Execution pattern: `architecture/ARCH-SPEC.md` line 53-55.
- Parallel Test Execution pattern: `architecture/ARCH-SPEC.md` line 57-59.
- Retry Mechanism pattern: `architecture/ARCH-SPEC.md` line 61-63.
- Containerized Test Execution strategy: `architecture/ARCH-SPEC.md` line 67-73.
- Service catalog (four logical services): `architecture/ARCH-SPEC.md` line 5-23.
- Architecture Anti-Patterns Found: status pending, no source citation available.

## Version and Owner
- Version: 1.0
- Owner: Project Team
