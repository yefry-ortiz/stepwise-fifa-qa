---
when_to_read: |
  Read before making any design or scoping decision — to check hard compliance, hosting, geography, and performance limits before committing.
---

# Constraints

## Purpose
This file governs the hard technical, environmental, quality-gate, and organizational-dependency constraints that bound test design, scoping, and delivery decisions for the FIFA insidefifa test automation project, as derived from the PRD, architecture spec, ADR spec, and epics documentation.

## Scope
Applies to teams and areas involved in test planning, test automation development, CI/CD pipeline configuration, and epic/feature scoping for the insidefifa web project — including QE/automation engineers, architecture owners, and epic/release planners.

## Mandatory Rules
- Website `https://inside.fifa.com/` must be available during test execution (ASM-001, high confidence) (source: `prd.md` line 103-105).
- Test execution environment must have stable internet connectivity (ASM-002, high confidence) (source: `prd.md` line 107-109).
- Page structure is assumed to remain consistent during the development phase (ASM-004, medium confidence) (source: `prd.md` line 115-117).
- Browser-specific configurations must be externalized to support future browser expansion (source: `prd.md` FR-005, line 55).
- Code coverage must exceed 80% (Gate-001 Code Quality) (source: `architecture/ARCH-SPEC.md` line 97).
- Tests must pass in the CI/CD pipeline and staging environment validation must succeed before production readiness (Gate-003) (source: `architecture/ARCH-SPEC.md` line 103-106).
- EPIC-005 (Multi-language) depends on all of EPIC-001, EPIC-002, EPIC-003, EPIC-004 completing first (source: `epics.md` line 81, 162).
- ENABLER-003 (Reporting Dashboard) depends on all epics (source: `epics.md` line 135, 144-145).
- Priority distribution constrains scope: 4 Must Have (EPIC-001..004), 1 Should Have (EPIC-005), 2 Could Have (EPIC-006, EPIC-007) (source: `epics.md` line 166-172).

## Recommended Rules
- Treat test data management via a JSON config strategy with care, given its Medium risk rating and the associated data-maintenance overhead (source: `adrs/ADR-SPEC.md` Impact Matrix, line 54).
- [INFERRED] Maintain language-specific test configurations proactively, since different language versions may have different page structures (RSK-003) (source: `prd.md` line 91-94).
- [INFERRED] Plan for regular updates to version-specific test configurations, since tests may fail on different browser versions (RSK-004) (source: `prd.md` line 96-99).
- [INFERRED] Account for complexity distribution when sequencing work: 3 High-complexity epics (EPIC-001, EPIC-005, EPIC-006) and 4 Medium-complexity epics (EPIC-002, EPIC-003, EPIC-004, EPIC-007) (source: `epics.md` line 174-179).

## Restrictions and Prohibitions
- Production readiness cannot proceed without Gate-003 (CI/CD pipeline pass and staging environment validation success) being satisfied (source: `architecture/ARCH-SPEC.md` line 103-106).
- Code that does not exceed 80% coverage cannot pass Gate-001 Code Quality (source: `architecture/ARCH-SPEC.md` line 97).
- EPIC-005 (Multi-language) and ENABLER-003 (Reporting Dashboard) work cannot be considered complete or fully scoped ahead of their epic dependencies finishing (source: `epics.md` line 81, 135, 144-145, 162).
- Browser-specific settings must not be hardcoded, since they must be externalized to support future browser expansion (source: `prd.md` FR-005, line 55).

## Examples
- Valid: A test suite assumes `https://inside.fifa.com/` is reachable and the CI environment has stable internet connectivity before execution begins (source: `prd.md` line 103-109).
- Invalid: Marking a release production-ready when CI/CD tests have not passed or staging validation has not succeeded, violating Gate-003 (source: `architecture/ARCH-SPEC.md` line 103-106).

## Traceability
- Hard Constraints Found — sourced from `prd.md` (lines 103-109, 115-117, 55), `architecture/ARCH-SPEC.md` (lines 97, 103-106), and `adrs/ADR-SPEC.md` Impact Matrix (line 54), per `analysis.md` section "For constraints.md".
- Known Organizational Limits — sourced from `epics.md` (lines 81, 135, 144-145, 162, 166-172, 174-179) and `prd.md` (lines 91-94, 96-99), per `analysis.md` section "For constraints.md".

## Version and Owner
- Version: 1.0
- Owner: Project Team
