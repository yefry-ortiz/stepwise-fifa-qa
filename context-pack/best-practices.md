---
when_to_read: |
  Read when a cross-cutting question arises not covered by a more specific file — to follow team conventions and avoid known anti-patterns.
---

# Best Practices

## Purpose
This file governs the general engineering and quality-engineering best practices to be followed when building and maintaining automated tests against the FIFA website, based on risks and architectural decisions identified in the project documentation (PRD, epics, ADRs).

## Scope
Applies to QE/automation engineers and developers working on test suites and page-object infrastructure that target the FIFA website, including selector strategy, test scheduling/execution, localization handling, and cross-browser configuration.

## Mandatory Rules
- Use robust selectors and perform regular test maintenance to withstand website structure changes (source: `prd.md` RSK-001, line 81-84).
- Introduce test delays and consider rotating IP addresses to mitigate rate limiting/blocking risk (source: `prd.md` RSK-002, line 86-89).
- Use language-specific test configurations to handle content localization variations (source: `prd.md` RSK-003, line 91-94).
- Use version-specific test configurations and update them regularly to handle browser compatibility issues (source: `prd.md` RSK-004, line 96-99).
- Conduct dedicated research into optimal selector strategies against dynamic FIFA website content (SPIKE-001 Selector Strategy Research) (source: `epics.md` line 139-142).
- Apply separation of concerns via the Page Object Model to keep test code maintainable (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).

## Recommended Rules
- [INFERRED] Treat the dependency tree as an attack surface: audit in CI, pin, and review additions (origin: blueprint `node-typescript` §10 Playbook item 5).

## Restrictions and Prohibitions
- [TO BE COMPLETED] No explicit anti-patterns to avoid are stated beyond the risk mitigations above.

## Examples
- Valid: A test suite uses resilient, semantically-meaningful selectors (e.g., data attributes or accessible roles) combined with a Page Object Model layer, and schedules test runs with built-in delays to avoid triggering rate limiting on the FIFA website.
- Invalid: A test suite hardcodes brittle CSS selectors tied to volatile DOM structure, runs all requests back-to-back with no delay, and hardcodes a single language/browser version configuration, ignoring localization and version differences.

## Traceability
- Robust selectors + maintenance: `prd.md` RSK-001 (line 81-84).
- Test delays / IP rotation: `prd.md` RSK-002 (line 86-89).
- Language-specific configs: `prd.md` RSK-003 (line 91-94).
- Version-specific configs: `prd.md` RSK-004 (line 96-99).
- Selector strategy research (SPIKE-001): `epics.md` (line 139-142).
- Page Object Model separation of concerns: `adrs/ADR-SPEC.md` ADR-002 (line 13-19).
- Dependency tree as attack surface: `origin: blueprint` `node-typescript` §10 Playbook item 5 (not project-stated; kept as Recommended/[INFERRED]).

## Version and Owner
- Version: 1.0
- Owner: Project Team
