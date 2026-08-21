---
when_to_read: |
  Read before writing or reviewing tests, setting coverage gates, or classifying test severity/priority.
---

# Test Standards

## Purpose
This file governs the testing approach, quality gates, performance/coverage targets, and test framework/patterns to be used for the FIFA QA automation project, as defined by the PRD, architecture spec, and ADRs.

## Scope
Applies to all teams and contributors writing, reviewing, or executing automated tests for the FIFA website project, including homepage load validation, navigation coverage, cross-browser/multi-viewport testing, and CI/CD quality gates.

## Mandatory Rules
- Homepage must load within 3 seconds; all visible links must be clickable; no broken images or missing content; must be responsive on desktop viewport (source: `prd.md` FR-001, line 14-18).
- A 95% test pass rate is required in a stable environment, measured as pass/fail ratio over 100 test executions (NFR-002 Reliability) (source: `prd.md` line 64-67).
- 100% of defined navigation paths must be covered by tests (KPI-002 Test Coverage) (source: `prd.md` line 126-129).
- Tests must be designed to detect 90% of navigation issues before production (KPI-003 Defect Detection Rate) (source: `prd.md` line 131-134).
- Average page load time must be under 3 seconds, measured across 5 test runs (NFR-001 Performance) (source: `prd.md` line 59-62).
- Gate-001 Code Quality: TypeScript compilation must complete without errors, ESLint compliance is required, and code coverage must exceed 80% (source: `architecture/ARCH-SPEC.md` line 92-97).
- Gate-002 Test Execution: all tests must pass in the local environment, performance benchmarks must be met, and there must be no critical test failures (source: `architecture/ARCH-SPEC.md` line 98-102).
- Gate-003 Deployment Readiness: tests must pass in the CI/CD pipeline, and staging environment validation must be successful (source: `architecture/ARCH-SPEC.md` line 103-106).
- Desktop viewport (1920x1080) is the initial supported compatibility target (NFR-004) (source: `prd.md` line 74-77).
- Type checking must run as a CI gate separate from the build (origin: blueprint `node-typescript` §6).
- Playwright Test 1.40+ must be used as the test runner (source: `adrs/ADR-SPEC.md` line 44; `architecture/ARCH-SPEC.md` line 85).
- Tests must follow the Page Object Model pattern, separating element locators, page actions, and validation helpers per SVC-002 (source: `architecture/ARCH-SPEC.md` line 10-13).
- Order-dependent tests must run sequentially, independent multi-language tests may run in parallel, and flaky network/timing conditions must use retry with exponential backoff (source: `architecture/ARCH-SPEC.md` line 51-63).

## Recommended Rules
- [INFERRED] Extend viewport coverage to mobile (375px), tablet (768px), and large desktop (1440px) as planned multi-viewport testing matures (source: `epics.md` EPIC-007, line 107-119).
- [INFERRED] Plan for cross-browser expansion to Firefox, Safari, and Edge beyond the current Chrome-only implementation (source: `prd.md` FR-005, line 49-55; `epics.md` EPIC-006, line 90-104).
- [INFERRED] Allocate spike time for selector strategy research on dynamic FIFA website content (SPIKE-001, 2 days), performance testing integration (SPIKE-002, 3 days), and visual testing framework evaluation (SPIKE-003, 2 days) before committing to related standards (source: `epics.md` line 139-152).

## Restrictions and Prohibitions
- [TO BE COMPLETED] No explicit prohibited testing shortcuts stated in the inputs beyond the quality-gate pass conditions above.

## Examples
- Valid: A test suite run reports 96/100 executions passing in the stable environment, satisfying the 95% pass rate requirement (NFR-002), and CI reports TypeScript compiling without errors with 82% code coverage, satisfying Gate-001.
- Invalid: A test suite is merged with TypeScript compilation errors or code coverage at 70%, violating Gate-001, or navigation path coverage is only 80%, falling short of the 100% KPI-002 requirement.

## Traceability
- Testing Approach rules: `prd.md` FR-001 (line 14-18), NFR-002 (line 64-67), KPI-002 (line 126-129), KPI-003 (line 131-134), NFR-001 (line 59-62), NFR-004 (line 74-77); `architecture/ARCH-SPEC.md` Gate-001/002/003 (line 92-106); blueprint `node-typescript` §6.
- Test Frameworks and Patterns rules: `adrs/ADR-SPEC.md` (line 44); `architecture/ARCH-SPEC.md` (line 10-13, 51-63, 85); `epics.md` EPIC-006/EPIC-007 (line 90-119), SPIKE-001/002/003 (line 139-152); `prd.md` FR-005 (line 49-55).
- Source analysis section: `artifacts/outputs/context-pack-analysis/analysis.md` § "For test-standards.md".

## Version and Owner
- Version: 1.0
- Owner: Project Team
