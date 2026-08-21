---
when_to_read: |
  Read before running against an environment or setting up test accounts — to pick the right env/URL, browser matrix, and feature-flag config.
---

# Environment Configuration

## Purpose
This file governs which test environments exist, their browser and language matrix, and how environment-specific and test-data configuration is managed and loaded across those environments.

## Scope
Applies to any team or process that runs tests against Local Development, CI/CD Pipeline, Staging Validation, or Production Monitoring environments, and to any component responsible for loading/switching environment configuration and language (e.g., the Configuration Service, SVC-003).

## Mandatory Rules
- Four environments are defined: Local Development (single machine, Chrome desktop, English), CI/CD Pipeline (containerized, Chrome headless, English), Staging Validation (cloud-based, Chrome desktop, EN/ES/FR), and Production Monitoring (production environment, Chrome desktop, English) (source: `architecture/ARCH-SPEC.md` line 25-49).
- Test data and environment-specific configuration must be managed via JSON configuration files (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27).
- The Configuration Service (SVC-003) is responsible for data loading, environment switching, and language management (source: `architecture/ARCH-SPEC.md` line 15-18).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No explicitly prohibited environments or configuration patterns stated in the inputs.

## Examples
- Valid: Running the CI/CD Pipeline environment in Chrome headless with English, consistent with its defined containerized/Chrome-headless/English configuration.
- Invalid: Running the CI/CD Pipeline environment in Spanish (ES) or French (FR), since only Staging Validation is defined to support EN/ES/FR — the CI/CD Pipeline environment is defined as English only.

## Traceability
- Environment definitions (Local Development, CI/CD Pipeline, Staging Validation, Production Monitoring) — `architecture/ARCH-SPEC.md` line 25-49, via analysis.md "Environment Configuration" section.
- JSON-based test data/environment configuration management — `adrs/ADR-SPEC.md` ADR-003, line 21-27, via analysis.md "Environment Configuration" section.
- Configuration Service (SVC-003) responsibilities — `architecture/ARCH-SPEC.md` line 15-18, via analysis.md "Environment Configuration" section.

## Version and Owner
- Version: 1.0
- Owner: Project Team
