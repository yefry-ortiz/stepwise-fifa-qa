---
when_to_read: |
  Read before creating or loading test data — choosing sources, applying anonymization, or setting reset/cleanup policy.
---

# Test Data Policy

## Purpose
This file governs test-data sources, format, environment-specific configuration, and associated risk management for the project, as derived from the ADR-SPEC.md test data configuration decision (ADR-003).

## Scope
Applies to all teams and areas involved in creating, loading, or maintaining test data used for multi-language testing across environments, per the JSON configuration strategy defined in ADR-003.

## Mandatory Rules
- JSON configuration files are the fixed (Accepted ADR) test-data format, environment-specific, supporting multi-language testing (source: `adrs/ADR-SPEC.md` ADR-003).
- Test data management via the JSON config strategy carries Medium risk per the project's own impact matrix and requires careful data maintenance (source: `adrs/ADR-SPEC.md` Impact Matrix, line 54).
- [TO BE COMPLETED] No anonymization requirements, data reset/cleanup policy, or privacy/compliance restrictions on test data are stated in any of the four analyzed project inputs (status: pending - no evidence in inputs).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No prohibited test-data sources are stated in the four analyzed project inputs.

## Examples
- Valid: Storing multi-language test fixtures in environment-specific JSON configuration files, consistent with the ADR-003 Accepted decision.
- Invalid: Using a non-JSON ad-hoc format (e.g., YAML or hardcoded inline data) for environment-specific test data, given JSON is the fixed ADR-mandated format.

## Traceability
- Mandatory Rule 1: `## For env-config.md and config-management-rules.md` (Environment Configuration, status: complete) and `## For project-inventory.md, Stable Invariants` — both citing `adrs/ADR-SPEC.md` ADR-003, line 21-27.
- Mandatory Rule 2: `## For constraints.md` — citing `adrs/ADR-SPEC.md` Impact Matrix, line 54.
- Mandatory Rule 3: No corresponding section/evidence found in analysis.md; flagged as a gap.

## Version and Owner
- Version: 1.0
- Owner: Project Team
