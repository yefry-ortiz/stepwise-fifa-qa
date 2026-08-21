---
when_to_read: |
  Read before provisioning resources, writing IaC, configuring deployment/networking, or choosing a cloud service/region.
---

# Infrastructure

## Purpose
This file governs the infrastructure topology, containerization strategy, and deployment environments for the test automation suite that validates an externally-owned website. It defines how and where tests execute (local, CI/CD, staging, production monitoring) and clarifies that no hosting/deployment surface is owned by this project.

## Scope
Applies to QE/automation engineers, DevOps/CI configuration owners, and anyone setting up or modifying test execution environments (local development, CI/CD pipeline, staging validation, production monitoring) for this test suite project.

## Mandatory Rules
- Test execution MUST run in Docker 24.0+ containers, for environment consistency, easy scaling, isolation from the host system, and simplified dependency management (source: `architecture/ARCH-SPEC.md` line 65-73, 88).
- This project does NOT own or require a dedicated hosting/deployment surface — it is a test suite validating an externally-owned website, not a hosted service; no L1 cloud platform was selected (source: `tech-stack-selection.md` Binding constraint, derived from `prd.md` line 5 and `architecture/ARCH-SPEC.md` line 25-49).
- Test execution MUST target the four defined deployment environments: Local Development (ENV-001, single machine, Chrome desktop, English), CI/CD Pipeline (ENV-002, containerized environment, Chrome headless, English), Staging Validation (ENV-003, cloud-based test environment, Chrome desktop, English/Spanish/French), and Production Monitoring (ENV-004, production environment, Chrome desktop, English — production smoke tests and monitoring) (source: `architecture/ARCH-SPEC.md` line 27-49).
- GitHub Actions MUST be used as the CI/CD platform integrating these environments (source: `architecture/ARCH-SPEC.md` line 75-79, 89).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No IaC tooling evidence found in the inputs (status: pending - no evidence in inputs); no prohibited cloud services/regions stated since no cloud platform is in scope.

## Examples
- Valid: Running the automated test suite inside a Docker 24.0+ container as part of the CI/CD Pipeline (ENV-002), using Chrome headless and English locale, orchestrated by GitHub Actions.
- Invalid: Provisioning a dedicated hosted service or cloud infrastructure for this project's own deliverable, since this project is a test suite validating an externally-owned website and no L1 cloud platform was selected.

## Traceability
- Infrastructure Topology: `architecture/ARCH-SPEC.md` line 65-73, 88; `tech-stack-selection.md` Binding constraint, derived from `prd.md` line 5 and `architecture/ARCH-SPEC.md` line 25-49.
- Infrastructure-as-Code Tooling: status pending - no evidence in inputs; `tech-stack-selection.md` (no IaC blueprint selected).
- Deployment Targets and Environments: `architecture/ARCH-SPEC.md` line 27-49 (ENV-001 through ENV-004); line 75-79, 89 (GitHub Actions).

## Version and Owner
- Version: 1.0
- Owner: Project Team
