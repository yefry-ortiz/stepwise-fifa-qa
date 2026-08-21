# ENGINEERING AUDIT - refining-artifact-consistency - 2026-05-18

## Summary

Built a reusable governance skill for Stepwise interactive artifact refinement.
The skill captures AI Pods framework patterns that matter during refinement:
source fidelity, zero invention, downstream contract preservation, surgical
edits, read-only canonical evidence, explicit unresolved items, and concise
rationale.

## Source Evidence

- Existing AI Pods skills use agent-native contracts, source fidelity gates, and
  structured output discipline.
- Representative skills reviewed: `researching-prd`, `planning-epics`,
  `implementing-user-stories`, `researching-code-design`, `planning-code-tasks`,
  `implementing-code`, `reviewing-code`, `human-quality-gate`, and
  `refining-iteratively`.
- Stepwise refinement sessions already provide sandbox targets and read-only
  context paths; this skill is designed to govern that existing runtime model.

## Architecture Decisions

| Decision | Outcome | Rationale |
|----------|---------|-----------|
| Skill name | `refining-artifact-consistency` | Describes artifact-focused refinement governance without tying it to one SDLC phase. |
| Runtime mode | Embedded Stepwise refinement first | Stepwise owns session lifecycle, diffing, approval, and apply-back. |
| Output pattern | No generated artifact by default | Refinement edits are applied directly to sandbox target artifacts. |
| Reference files | 3 files | Separates source/scope rules, edit protocol, and quick-action behavior. |
| Progress tracking | Explicit embedded-mode exception | The Stepwise runtime tracks refinement sessions; requiring `_progress.json` during a refinement turn would create unwanted sandbox noise. |

## Pattern Compliance

| Pattern | Status | Notes |
|---------|--------|-------|
| Source fidelity | present | Requires target and relevant read-only source loading before factual changes. |
| Zero invention | present | Missing evidence remains pending/open instead of fabricated. |
| Surgical repair | present | Smallest coherent edit is required. |
| Carry-forward contract | adapted | Stable artifact contracts, IDs, schema, and traceability serve as the carry-forward contract. |
| Write scope control | present | Canonical project files are read-only; sandbox targets are writable. |
| Status protocol | present | Unsupported items remain pending, assumptions, or open questions. |
| Progress tracker | intentionally omitted in embedded mode | Lifecycle belongs to Stepwise refinement state and transcript. |

## Files Written

- `SKILL.md`
- `references/source-and-scope.md`
- `references/edit-protocol.md`
- `references/quick-action-guidance.md`
- `evals/evals.json`
- `ENGINEERING-AUDIT-REFINING-ARTIFACT-CONSISTENCY-20260518.md`

## Open Questions

- None.
