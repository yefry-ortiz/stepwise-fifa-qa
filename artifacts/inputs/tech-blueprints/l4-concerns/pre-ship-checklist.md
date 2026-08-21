# Cross-Cutting Concern: Pre-Ship Checklist

> **Thesis** — Verification gates, not architecture. What must be *checked* before a release,
> as distinct from what must be *built*, which is the job of the blueprints.
> **Applies to** — every project, regardless of platform.
> **Artifact type** — checklist. Deliberately not merged into any blueprint; blueprints answer
> *what to build*, this answers *what to verify*.

---

## 1. Executive Summary

Every item here is a gate with a pass/fail outcome, ordered so that cheap checks run before
expensive ones and automated checks run before human ones. The three layers — manual probes,
assisted review, and automated scanning — are complementary. None is sufficient alone:
scanners miss business-logic and authorization-state bugs; humans miss cross-cutting
configuration drift; assisted review finds a wide but shallow band between them.

This document feeds `test-standards.md` (quality gates) and `validation-tools.md` (command
registry) in the generated context pack.

## 2. Gate 0 — Automated, Blocking

Runs in CI on every pull request. A failure blocks merge.

| Check | Pass condition |
|---|---|
| Build | Clean build for every target in the workspace |
| Typecheck | Zero errors; no suppression added in the diff without justification |
| Lint and format | Zero errors; formatting is machine-enforced, never reviewed by humans |
| Unit tests | Pass; coverage does not decrease against the base branch |
| Integration tests | Pass against ephemeral infrastructure, not a shared environment |
| Dependency audit | No known-exploited vulnerabilities; no new copyleft license without approval |
| Secret scan | No credential pattern anywhere in the diff or in history |
| IaC plan | Plan is clean and reviewed; no out-of-band resource drift |

## 3. Gate 1 — Auth Failure Paths (Manual, Per Release)

Happy-path authentication is covered by integration tests. These are the paths that are
routinely missed, each of which is an information-disclosure or availability defect.

- [ ] Repeated failed logins rate-limit or lock, and the response does **not** reveal whether
      the account exists.
- [ ] Password reset for a non-existent address returns the same response as for an existing
      one, with the same timing characteristics.
- [ ] Signing up with an already-registered address does not disclose that the address is
      registered.
- [ ] Double-clicking an email verification link degrades gracefully; the second use does not
      error visibly or invalidate the session.
- [ ] An expired or already-consumed reset token is rejected with a generic message.
- [ ] Session revocation takes effect immediately on the server, not merely in the client.
- [ ] Access as a *different authenticated user* is denied at the data tier, verified by
      direct query — not only through the application.
- [ ] Access as an *unauthenticated* caller to every endpoint that should require auth is
      denied.

## 4. Gate 2 — Assisted Review (Per Release)

Run against the codebase with a coding assistant. Each is a distinct pass; combining them
into one prompt reliably degrades recall.

1. Review security headers and baseline HTTP posture.
2. Review against OWASP Top 10, naming the specific file and line for each finding.
3. Search for credential or personal-data leakage in frontend code, API responses, and log
   output.
4. Confirm no API key, token, or connection string is reachable from client code or visible
   in a network call.
5. Review authorization: enumerate every data-tier policy and identify tables with none.
6. For AI features: review prompt-construction sites for untrusted-input interpolation, and
   tool definitions for over-broad scope.

Findings are triaged, not auto-applied. Assisted review produces false positives at a rate
that makes blind application harmful.

## 5. Gate 3 — Operational Readiness

- [ ] Rate limits are configured on every endpoint reaching a metered third-party API.
- [ ] Hard spend caps and alert thresholds are set on every metered provider, with the alert
      meaningfully below the cap.
- [ ] CORS is restricted to production origins.
- [ ] WAF is enabled with managed rules on every public entry point.
- [ ] Error responses in production are sanitized; verified by triggering a real failure.
- [ ] Logs contain no personal data or credentials; verified by sampling.
- [ ] Rollback path is tested, not merely documented.
- [ ] Alerting reaches a human who is on duty; verified by firing a test alert.

## 6. Gate 4 — Legal and Data

- [ ] Privacy policy is published and accurate about what is collected and where it resides.
- [ ] Data residency is documented for the primary datastore and every third-party processor.
- [ ] Dependency licenses are reviewed, including transitive dependencies added by AI
      assistants during the release window.
- [ ] Data deletion and export paths exist if the applicable regime requires them.

## 7. Gate 5 — Final Automated Scan

A dedicated security scanner as the last step before deploy. This catches cross-cutting
issues that per-file review misses. It does **not** replace Gates 1-4: a clean scan alongside
an untested authorization policy is a false-confidence result, which is worse than no scan.

## 8. Playbook

1. **Automate every gate that can be automated.** A checklist item enforced by human
   discipline degrades within three releases.
2. **Run failure paths, not just happy paths.** Every item in Gate 1 exists because the happy
   path passed while the failure path disclosed information.
3. **Triage assisted-review findings; never auto-apply.** False-positive rates make blind
   application a net negative.
4. **Treat a clean scan as necessary, not sufficient.** Business-logic and authorization-state
   defects are invisible to scanners by construction.
5. **Gate the release, not the developer.** Gates belong in CI and in the release process,
   where they are uniform, not in individual review habits.

---

## When This Checklist Is Insufficient

1. Regulated releases requiring formal evidence, sign-off, and retention.
2. Systems where an outage is a safety event rather than an availability event.
3. First release of a multi-tenant platform — tenancy isolation needs dedicated adversarial
   testing beyond Gate 1.

---

## 14. Normative Profile

```yaml
blueprint_id: pre-ship-checklist
blueprint_version: 1.0
layer: L4

standards:
  - area: test
    rule: "Every pull request must pass build, typecheck, lint, unit tests, integration tests, dependency audit, secret scan, and IaC plan review before merge."
    evidence: "§2"
  - area: test
    rule: "Coverage must not decrease relative to the base branch."
    evidence: "§2"
  - area: test
    rule: "Integration tests run against ephemeral infrastructure, never a shared environment."
    evidence: "§2"
  - area: test
    rule: "Every release must verify the eight auth failure paths in Gate 1, including data-tier access as a different authenticated user."
    evidence: "§3"
  - area: test
    rule: "Assisted-review findings are triaged by a human and never auto-applied."
    evidence: "§4"
  - area: test
    rule: "Rollback path must be tested before release, not only documented."
    evidence: "§5"
  - area: test
    rule: "A final automated security scan runs as the last step before deploy and does not substitute for Gates 1-4."
    evidence: "§7"
  - area: automation
    rule: "Formatting is machine-enforced and never subject to human review."
    evidence: "§2"
  - area: automation
    rule: "All release gates execute in CI, not as individual reviewer habits."
    evidence: "§8"
  - area: security
    rule: "Authentication failure responses must not disclose account existence, including via response timing."
    evidence: "§3"

constraints:
  - constraint: "A failing Gate 0 check blocks merge; gates are not advisory."
    source: "§2"
  - constraint: "Privacy policy, data residency documentation, and license review must complete before release."
    source: "§6"

commands:
  - purpose: build
    command: "<project build command>"
    evidence: "§2"
  - purpose: typecheck
    command: "<project typecheck command>"
    evidence: "§2"
  - purpose: lint
    command: "<project lint command>"
    evidence: "§2"
  - purpose: test
    command: "<project unit test command>"
    evidence: "§2"
  - purpose: test
    command: "<project integration test command, targeting ephemeral infrastructure>"
    evidence: "§2"
  - purpose: security
    command: "<project dependency audit command>"
    evidence: "§2"
  - purpose: security
    command: "<project secret scan command>"
    evidence: "§2"
  - purpose: security
    command: "<final pre-deploy security scan>"
    evidence: "§7"

open_questions:
  - question: "What are the concrete commands for build, test, lint, typecheck, and security in this project?"
    why: "Stack- and repository-specific; resolved from the selected L2 blueprint and the project's manifests."
  - question: "What coverage threshold applies?"
    why: "Set per project; this document mandates only that coverage must not regress."
  - question: "Does the applicable regime require formal release evidence and retention?"
    why: "Determined by customer sector and jurisdiction."
```
