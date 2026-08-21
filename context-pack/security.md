---
when_to_read: |
  Read before implementing auth, handling secrets/tokens, validating input, exposing an endpoint, or touching encryption.
---

# Security

## Purpose
This file governs the security posture for the FIFA QA automation project: what security patterns, gaps, and risks apply to testing the target site, and what baseline controls (where they exist) come from staged blueprints rather than project-specific requirements.

## Scope
Applies to anyone writing or maintaining automated tests against the target FIFA public-facing site, and to anyone setting up CI/CD checks (dependency audits, secret scans) for this project.

## Mandatory Rules
- No project-stated authentication/authorization requirement exists: the target site under test requires no authentication for the public pages tested (source: `prd.md` ASM-003).
- [TO BE COMPLETED] No project-stated secrets-management, encryption, or input-validation rules found in the four analyzed inputs (status: pending - no evidence in inputs).

## Recommended Rules
- [INFERRED] Identity delegation, input validation, secrets management, and dependency supply-chain review per the `security-baseline` L4 blueprint (origin: blueprint `security-baseline` §3-§9) — a blueprint floor, not a project-stated requirement (see Open Questions OQ-03 in analysis.md).
- [INFERRED] Dependency audit and secret scan as a Gate-0 CI check (origin: blueprint `pre-ship-checklist` §2; `node-typescript` §8 proposes `pnpm audit --audit-level=high`) — no dependency-audit command is named in the project inputs; flagged as a gap.
- Mitigate RSK-002 (rate limiting/blocking) via test delays and possibly rotating IP addresses (source: `prd.md` line 86-89).
- Mitigate RSK-001 (website structure changes) via robust selectors and regular test maintenance (source: `prd.md` line 81-84).

## Restrictions and Prohibitions
- [TO BE COMPLETED] No explicit prohibited security patterns stated in the four analyzed project inputs.

## Examples
- Valid: Running automated navigation and content-verification tests against public FIFA pages without supplying any credentials, consistent with ASM-003 (no authentication required for pages under test).
- Invalid: Hardcoding or storing credentials/secrets for the target site in test code or config, when the project's own assumption (ASM-003) is that no authentication is required.

## Traceability
- Mandatory rule on no-auth requirement: project-stated, `source: prd.md` ASM-003, line 111-113.
- Recommended rule on identity delegation/input validation/secrets/dependency review: `origin: blueprint` `security-baseline`, §3-§9 — not project-stated.
- Recommended rule on dependency audit / secret scan Gate-0: `origin: blueprint` `pre-ship-checklist` §2 and `node-typescript` §8 — not project-stated; gap noted since no project input names a dependency-audit tool.
- Recommended mitigation for RSK-002 (rate limiting/blocking): `source: prd.md` line 86-89.
- Recommended mitigation for RSK-001 (website structure changes): `source: prd.md` line 81-84.

## Version and Owner
- Version: 1.0
- Owner: Project Team
