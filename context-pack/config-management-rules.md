---
when_to_read: |
  Read before adding config or environment variables, injecting secrets, or writing environment-specific settings.
---

# Configuration Management Rules

## Purpose
This file governs environment-variable naming/injection, config-file conventions, and secret-injection patterns for the project, based on the "Environment Configuration" and "Secret and Config Management" evidence in `artifacts/outputs/context-pack-analysis/analysis.md`.

## Scope
Applies to any team or contributor adding, modifying, or consuming test data, environment-specific configuration, or secrets within the project's configuration layer (e.g. the Configuration Service, SVC-003).

## Mandatory Rules
- JSON configuration files are the project's config-file convention for test data and environment-specific configuration (source: `adrs/ADR-SPEC.md` ADR-003).
- Configuration Service (SVC-003) is the designated component for data loading, environment switching, and language management (source: `architecture/ARCH-SPEC.md` line 15-18).
- [TO BE COMPLETED] No project-stated secret-management approach exists in the four analyzed inputs (status: pending - no evidence in inputs); the project's stated assumption is that no application secrets exist since the target site is public/unauthenticated (source: `prd.md` ASM-003).

## Recommended Rules
- [INFERRED] Environment variables validated through a schema at startup, and secrets sourced from a managed secret store (origin: blueprint `node-typescript` §12; `security-baseline` §5) — a blueprint default, not a project-stated requirement; see Open Questions OQ-03 in analysis.md.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No explicitly prohibited config patterns (e.g. hardcoded values, committed secrets) stated in the four analyzed project inputs.

## Examples
- Valid: Storing environment-specific test data (e.g. per-environment URLs, language settings) in a JSON configuration file loaded by the Configuration Service (SVC-003).
- Invalid: Committing a secret value directly into a JSON config file instead of sourcing it from a managed secret store, as recommended by the blueprint default ([INFERRED], not project-stated).

## Traceability
- Environment Configuration (JSON config-file convention, Configuration Service SVC-003): analysis.md § "Environment Configuration", status: complete — sources: `adrs/ADR-SPEC.md` ADR-003 (line 21-27), `architecture/ARCH-SPEC.md` (line 15-18).
- Secret and Config Management (no project-stated approach; public/unauthenticated site assumption): analysis.md § "Secret and Config Management", status: pending - no evidence in inputs — source: `prd.md` ASM-003 (line 111-113).
- Recommended schema-validated env vars / managed secret store: `origin: blueprint` `node-typescript` §12 and `security-baseline` §5 — not project-stated; flagged in Open Questions (OQ-03).

## Version and Owner
- Version: 1.0
- Owner: Project Team
