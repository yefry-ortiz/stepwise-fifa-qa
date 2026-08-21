---
when_to_read: |
  Read before prioritizing work or making a product trade-off — to align with the customer's business goals and stakeholder expectations.
---

# Customer Background

## Purpose
This file governs the identity, versioning, and stakeholder context of the InsideFIFA-WEB project, and clarifies the ownership boundary between this test project and the target system it validates.

## Scope
Applies to all teams and roles interacting with the InsideFIFA-WEB test suite and framework: QA Engineers, Test Automation Engineers, Project Managers, and the Development Team.

## Mandatory Rules
- The project is named InsideFIFA-WEB, version 1.0.0, dated 2026-08-21 (source: `prd.md` line 4-7; `epics.md` line 4-6).
- Primary users are: QA Engineer (executes and maintains the test suite), Test Automation Engineer (develops and enhances the test framework), and Project Manager (reviews test results and quality metrics) (source: `prd.md` line 138-141).
- Secondary users include the Development Team, who use test results for debugging (source: `prd.md` line 143-144).
- The customer/target system is FIFA's public "Inside FIFA" content site (`https://inside.fifa.com/`); this project does not own or build that site — it only validates the site's navigation and functionality (source: `prd.md` line 5).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- Do not treat this project as if it owns, builds, or is responsible for the content or infrastructure of `https://inside.fifa.com/` — it is strictly a validation/test project targeting that external site (source: `prd.md` line 5).
- Do not assign responsibilities of primary user roles (QA Engineer, Test Automation Engineer, Project Manager) to the secondary user role (Development Team), or vice versa (source: `prd.md` line 138-144).

## Examples
- Valid: A Test Automation Engineer enhances the test framework to better validate navigation on `https://inside.fifa.com/`, without proposing changes to that site's own codebase.
- Invalid: A contributor treats InsideFIFA-WEB as the owner/builder of `https://inside.fifa.com/` and plans feature development for the FIFA site itself.

## Traceability
- Project name/version/date: `analysis.md` § "For customer-background.md" → `prd.md` line 4-7; `epics.md` line 4-6.
- Primary users: `analysis.md` § "For customer-background.md" → `prd.md` line 138-141.
- Secondary users: `analysis.md` § "For customer-background.md" → `prd.md` line 143-144.
- Target system ownership boundary: `analysis.md` § "For customer-background.md" → `prd.md` line 5.

## Version and Owner
- Version: 1.0
- Owner: Project Team
