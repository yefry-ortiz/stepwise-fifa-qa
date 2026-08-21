# InsideFIFA-WEB — Test Case Analysis Spec
version: 1.0.0
session: ANALYSIS-INSIDEFIFA-WEB-20260821
mode: BUILD
date: 2026-08-21T21:31:00Z
language: English
sources:
  test_cases: artifacts/outputs/quality-engineering-planning/e2e-test-cases (resolved — see path_resolution_note)
  test_strategy: artifacts/outputs/quality-engineering-planning/STRATEGY-SPEC-Test-Strategy-1.md (resolved — see path_resolution_note)
  test_inventory: artifacts/outputs/quality-engineering-web-automation/automation-code/test-inventory.md | N/A (no inventory found — first run)
spec_status: draft

## path_resolution_note

The declared parameters `test_cases_path` (`./artifacts/outputs/quality-engineering-planning/test-cases`)
and `test_strategy_path` (`./artifacts/outputs/quality-engineering-planning/qa-test-strategy`) point to
EMPTY directories. The actual upstream artifacts produced by the `generating-e2e-test-cases` and
`defining-qe-strategy` capabilities were written to sibling folders instead:
  - Test cases (367 TCs, 7 Gherkin suites): `artifacts/outputs/quality-engineering-planning/e2e-test-cases/suites/epic-0{1..7}-e2e-suite.md`
  - Test strategy: `artifacts/outputs/quality-engineering-planning/STRATEGY-SPEC-Test-Strategy-1.md`
This is a capability output-folder naming mismatch between the upstream `quality-engineering-planning`
capability config and the downstream `quality-engineering-web-automation` capability's declared input
parameters — NOT missing data. Per Zero-Invention, no content was fabricated: all TC IDs and strategy
content below are read verbatim from the real files above. This mismatch is logged as ASM-01 in
open_questions for pipeline-config correction.
status: assumption
source: directory listing of both declared and resolved paths

## executive_metrics

total_test_cases: 367
automatable: 213
partial: 152
manual_only: 2
simple: 136
medium: 187
complex: 44
new: 367          # no test inventory found — automation_code_path has no test-inventory.md (first run)
update: 0
duplicate: 0
completeness_score: 7.72
gaps_found: 9
clarifications_needed: 13
status: complete
source: aggregated from quality_assessment section (367/367 TCs) across e2e-test-cases/suites/epic-0{1..7}-e2e-suite.md

## original_test_cases

Source test cases are Gherkin feature files (367 scenarios across 7 epic suites), too large to
reproduce verbatim in full within this spec without exceeding the 3,000-line split threshold
(execution-protocol scaling rule; raw source totals 8,769 lines across 7 files). Per Zero-Invention,
no content was paraphrased or altered — the authoritative verbatim source remains on disk at the
paths below and is the source-of-record for every TC ID referenced in this spec.

| epic_id | suite_file | tc_count | tc_id_range |
|---------|-----------|----------|-------------|
| EPIC-001 | e2e-test-cases/suites/epic-01-e2e-suite.md | 40 | TC_EPIC01_001-040 |
| EPIC-002 | e2e-test-cases/suites/epic-02-e2e-suite.md | 45 | TC_EPIC02_001-045 |
| EPIC-003 | e2e-test-cases/suites/epic-03-e2e-suite.md | 60 | TC_EPIC03_001-060 |
| EPIC-004 | e2e-test-cases/suites/epic-04-e2e-suite.md | 42 | TC_EPIC04_001-042 |
| EPIC-005 | e2e-test-cases/suites/epic-05-e2e-suite.md | 48 | TC_EPIC05_001-048 |
| EPIC-006 | e2e-test-cases/suites/epic-06-e2e-suite.md | 62 | TC_EPIC06_001-062 |
| EPIC-007 | e2e-test-cases/suites/epic-07-e2e-suite.md | 70 | TC_EPIC07_001-070 |

status: complete
source: artifacts/outputs/quality-engineering-planning/e2e-test-cases/suites/ (resolved path — see path_resolution_note)

