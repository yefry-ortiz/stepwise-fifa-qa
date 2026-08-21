# Cross-Cutting Concern: Security Baseline

> **Thesis** — The security invariants every project must satisfy, stated once, independent
> of platform. Each blueprint's §8 states how *that* platform satisfies them.
> **Applies to** — every L1, L2, and L3 blueprint.
> **Does not cover** — verification and pre-ship gates; see `pre-ship-checklist.md`.

---

## 1. Executive Summary

Six blueprints describe six different ways to satisfy the same small set of security
invariants. Those invariants do not vary by cloud, and repeating them per platform invites
drift. This document states them once as normative rules; the per-platform §8 sections state
the mechanism. Neither replaces the other: an invariant without a mechanism is unenforceable,
and a mechanism without an invariant is unjustified.

The organizing principle: **security is a property of the shipping ritual, not a release
milestone.** Controls that are not wired into CI or infrastructure-as-code do not survive
contact with delivery pressure.

## 2. Trust Boundaries

Every architecture in this library has the same four boundaries. Name them explicitly before
designing controls.

* **Client -> Edge.** The mobile binary and the web bundle are public. Anything shipped to
  them is disclosed, including anything in an environment variable that reaches the client
  build. Treat the client as an untrusted input generator, never as an enforcement point.
* **Edge -> Application.** The WAF and CDN tier. Volumetric and signature-based defense lives
  here, not in application code.
* **Application -> Data.** Authorization decisions live here or in the data tier. This is the
  boundary most often left implicit, and the one that produces the worst failures.
* **Application -> Third party.** Model providers, payment processors, and analytics. Every
  crossing is a data-disclosure decision and a spend decision simultaneously.

## 3. Identity and Authorization

* **Mandatory:** Delegate authentication to a managed identity provider. Do not implement
  password storage, reset flows, session issuance, or MFA in application code.
* **Mandatory:** Authorization is enforced server-side or in the data tier. Client-side role
  checks are presentation logic and carry no security weight.
* **Mandatory:** Where the data tier supports row-level authorization, enable it on every
  table holding user data — not only the tables currently queried from the client. A table
  without a policy is readable by anyone holding the public key.
* **Mandatory:** Test authorization policies by attempting access as a different user, and as
  an unauthenticated caller. An untested policy is a false-confidence generator.
* **Recommended:** Externalize authorization rules from application code once they exceed
  simple ownership checks.

## 4. Input Handling and Output Safety

* **Mandatory:** Validate and sanitize every input server-side. Schema validation in the
  client is a user-experience feature and provides no protection.
* **Mandatory:** Validate type, length, range, and format — not only shape.
* **Mandatory:** Sanitize error responses. Stack traces, table names, column names, query
  fragments, and internal hostnames must never reach a client. Log detail server-side; return
  a generic message and a correlation identifier.
* **Restriction:** Never interpolate untrusted input into a query, a shell command, a
  template, or a model prompt without an explicit escaping or parameterization step.

## 5. Secrets

* **Mandatory:** Publishable keys may ship to clients. Secret keys — service-role, payment
  secret, model-provider keys — remain server-side, injected from a managed secret store.
* **Mandatory:** No secret is committed to version control, at any point in history.
* **Mandatory:** A key suspected of exposure is rotated immediately. Assessment does not
  precede rotation.
* **Mandatory:** Model-provider credentials never reach a mobile binary or web bundle. Client
  AI calls route through a server-side proxy that holds the credential and applies quota.
* **Recommended:** Prefer workload identity federation over static keys for service-to-service
  authentication wherever the platform supports it.

## 6. Perimeter and Abuse Control

* **Mandatory:** Rate-limit every endpoint that reaches a metered third-party API. An
  unthrottled model endpoint is an unbounded liability, not merely a performance risk.
* **Mandatory:** Set hard spend caps and alert thresholds on every metered provider, with an
  alert well below the cap. See §11 of each blueprint for where the control lives.
* **Mandatory:** Restrict CORS to known origins in production.
* **Mandatory:** Place a WAF with managed rule sets in front of every public entry point.
* **Recommended:** Apply a challenge (CAPTCHA or equivalent) to unauthenticated public forms.

## 7. Data Protection

* **Mandatory:** Encrypt in transit and at rest. On every platform in this library this is
  default behavior — the rule exists so that deviations are deliberate and documented.
* **Mandatory:** Know where user data physically resides, including data held by third-party
  processors. This is a §13 question in every blueprint and it must have an answer before
  first launch, not before first audit.
* **Mandatory:** Publish a privacy policy before collecting any personal data, including an
  email address.
* **Restriction:** Never export production personal data to a personal account, a local
  machine, or an unmanaged analytics destination.

## 8. Supply Chain

* **Mandatory:** Review dependencies introduced by AI coding assistants before merge. License
  contamination is silent, and copyleft obligations attach regardless of how the code arrived.
* **Mandatory:** Establish the project's position on authorship and copyright of
  AI-generated code before it becomes commercially load-bearing. In some jurisdictions
  purely machine-generated code attracts no copyright protection, which affects what can be
  licensed, assigned, or defended. This is a legal determination, not an engineering one —
  the engineering obligation is to raise it early and record the answer.
* **Mandatory:** Run dependency vulnerability scanning in CI, and fail the build on
  known-exploited vulnerabilities.
* **Recommended:** Pin dependency versions and update deliberately.

## 9. AI and Agent-Specific Controls

Applies to every L3 blueprint and to §7 of every L1/L2 blueprint.

* **Mandatory:** Treat model output as untrusted input. Output that reaches a shell, a query,
  a filesystem path, or a rendered page passes through the same validation as user input.
* **Mandatory:** Tools exposed to an agent carry their own authorization. An agent inherits
  the permissions of its tools, so a broadly-scoped tool grants a broadly-scoped agent.
* **Mandatory:** Any agent action that is irreversible, outward-facing, or spend-incurring
  requires either a human approval gate or a hard, tested budget bound.
* **Mandatory:** Log every tool invocation with its arguments and outcome. An agent without a
  tool-call audit trail cannot be incident-reviewed.
* **Restriction:** Never place untrusted retrieved content and privileged instructions in the
  same trust context without a clear delimiter and an explicit instruction-precedence rule.

## 10. Playbook

1. **Delegate identity on day one.** Retrofitting a managed IdP after launch means migrating
   live credentials — the most expensive avoidable migration in this library.
2. **Enforce authorization at the data tier.** Application-tier-only authorization fails open
   the moment a second consumer of the database appears.
3. **Assume the client is disclosed.** Design so that full knowledge of the client binary
   yields no privileged capability.
4. **Cap spend before shipping the feature.** Rate limits and budget alerts are part of the
   feature, not follow-up work.
5. **Wire controls into CI and IaC.** A control enforced by human discipline degrades; a
   control enforced by a failing build does not.

---

## When This Baseline Is Insufficient

1. Regulated workloads (health, payment, government) — the baseline is a floor, and the
   applicable regime adds mandatory controls it does not enumerate.
2. Multi-tenant platforms where tenants are mutually adversarial — tenancy isolation needs
   dedicated design beyond §12 of a platform blueprint.
3. Threat models involving insiders or supply-chain compromise of the build system.

---

## 14. Normative Profile

```yaml
blueprint_id: security-baseline
blueprint_version: 1.0
layer: L4

tech_signals:
  - signal: "Managed identity provider (platform-specific; see each blueprint §8)"
    evidence: "§3"
  - signal: "Managed secret store (platform-specific; see each blueprint §8)"
    evidence: "§5"
  - signal: "WAF with managed rule sets at every public entry point"
    evidence: "§6"

standards:
  - area: security
    rule: "Delegate authentication to a managed identity provider; never implement password storage, reset, or session issuance in application code."
    evidence: "§3"
  - area: security
    rule: "Enforce authorization server-side or in the data tier; client-side role checks carry no security weight."
    evidence: "§3"
  - area: security
    rule: "Enable row-level authorization on every table holding user data, not only tables currently queried from the client."
    evidence: "§3"
  - area: security
    rule: "Validate and sanitize every input server-side for type, length, range, and format."
    evidence: "§4"
  - area: security
    rule: "Return generic error messages with a correlation id; never expose stack traces, table names, column names, or query fragments to a client."
    evidence: "§4"
  - area: security
    rule: "Keep secret keys server-side, injected from a managed secret store; only publishable keys may ship to clients."
    evidence: "§5"
  - area: security
    rule: "Rotate any key suspected of exposure immediately, before assessing impact."
    evidence: "§5"
  - area: security
    rule: "Route all client AI calls through a server-side proxy holding the model credential; model keys never reach a mobile binary or web bundle."
    evidence: "§5"
  - area: security
    rule: "Rate-limit every endpoint reaching a metered third-party API, and set hard spend caps with alerts below the cap."
    evidence: "§6"
  - area: security
    rule: "Restrict CORS to known origins in production."
    evidence: "§6"
  - area: security
    rule: "Treat model output as untrusted input; validate before it reaches a shell, query, filesystem path, or rendered page."
    evidence: "§9"
  - area: security
    rule: "Require a human approval gate or a tested budget bound for any agent action that is irreversible, outward-facing, or spend-incurring."
    evidence: "§9"
  - area: security
    rule: "Log every agent tool invocation with arguments and outcome."
    evidence: "§9"
  - area: test
    rule: "Test authorization policies by attempting access as a different user and as an unauthenticated caller."
    evidence: "§3"
  - area: automation
    rule: "Run dependency vulnerability scanning in CI and fail the build on known-exploited vulnerabilities."
    evidence: "§8"
  - area: automation
    rule: "Review dependencies introduced by AI coding assistants before merge, for license contamination."
    evidence: "§8"
  - area: automation
    rule: "Pin dependency versions and update deliberately."
    evidence: "§8"

constraints:
  - constraint: "The project's position on authorship and copyright of AI-generated code must be determined and recorded before that code becomes commercially load-bearing; in some jurisdictions purely machine-generated code attracts no copyright protection."
    source: "§8"
  - constraint: "A privacy policy must exist before any personal data — including an email address — is collected."
    source: "§7"
  - constraint: "Data residency for user data, including third-party processors, must be documented before first launch."
    source: "§7"
  - constraint: "No secret may exist anywhere in version-control history."
    source: "§5"

infra:
  - item: "Secret store"
    detail: "Managed; secrets injected at runtime, never committed. Platform-specific implementation in each blueprint §8."
    evidence: "§5"
  - item: "WAF / edge"
    detail: "Managed rule sets in front of every public entry point."
    evidence: "§6"
  - item: "Spend controls"
    detail: "Hard cap plus alert threshold on every metered provider."
    evidence: "§6"

commands:
  - purpose: security
    command: "<project dependency-audit command> — wired into CI, non-zero exit fails the build"
    evidence: "§8"

open_questions:
  - question: "Which regulatory regime applies, and what does it add beyond this baseline?"
    why: "Determined by customer jurisdiction and sector, not by platform choice."
  - question: "Are tenants mutually adversarial?"
    why: "Changes the tenancy isolation requirement in §12 of the selected platform blueprint."
  - question: "What are the actual spend caps per metered provider?"
    why: "Derived from project budget, not from platform capability."
```
