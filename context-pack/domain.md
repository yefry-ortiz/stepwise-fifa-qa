---
when_to_read: |
  Read before modeling entities, naming domain concepts in code, or writing business logic — to use the ubiquitous language and respect invariants.
---

# Domain

## Purpose
This file defines the ubiquitous language (domain terms and entities) and the business rules and invariants for the InsideFIFA-WEB test automation project, which validates the navigation and functionality of the public FIFA website `https://inside.fifa.com/`.

## Scope
Applies to the QE/automation team building and maintaining the InsideFIFA-WEB test project: anyone modeling domain entities (topic pages, navigation menus), naming test/domain concepts in code, or authoring test business logic for `inside.fifa.com` coverage (source: `prd.md` line 4-5; `epics.md` line 4).

## Mandatory Rules
- The application under test is `https://inside.fifa.com/`, a public FIFA website; this project validates its navigation and functionality rather than building it (source: `prd.md` line 5).
- Homepage must load within 3 seconds; all visible links clickable; no broken images or missing content; page responsive on desktop viewport (source: `prd.md` FR-001, line 14-18).
- All 7 "What FIFA Does" topic pages (Legal, Transfer system, Women's Football, Advancing football, Refereeing, Innovation, Talent development), reachable from `/all-topics`, must be accessible and load content correctly (source: `prd.md` FR-002, line 20-30; `epics.md` EPIC-003, line 42-54).
- The "Inside FIFA" top navigation button and all sub-items must be accessible and navigable, including dropdown/click functionality and sub-item navigation validation (source: `prd.md` FR-003, line 32-39; `epics.md` line 58-70).
- English (en) must be fully tested; Spanish (es) and French (fr) must be switchable and testable, with the framework supporting language-specific selectors (source: `prd.md` FR-004, line 41-47; `epics.md` line 74-86).
- Current implementation targets Chrome only; architecture must support future extension to Firefox, Safari, Edge via externalized browser-specific configuration (source: `prd.md` FR-005, line 49-55).
- No authentication is assumed required to access the public pages under test — stated as a project assumption with High confidence (source: `prd.md` ASM-003, line 111-113).
- Functional requirements and epics must be classified using MoSCoW priority (Must Have / Should Have / Could Have) (source: `prd.md` line 12; `epics.md` line 12, 28, 44, 60, 76, 92, 109).
- Code must follow the Page Object Model (POM) design pattern for maintainable test code and separation of concerns (source: `artifacts/inputs/documentation/adrs/ADR-SPEC.md` line 13-18).
- Code quality, test execution, and deployment readiness must satisfy the named Quality Gate checkpoints (Gate-001/002/003) with defined pass/fail criteria before progressing (source: `ARCH-SPEC.md` line 92).

## Recommended Rules
- [INFERRED] When modeling the Test Execution Engine, decompose it into its documented slices (Test runner, Browser management, Test orchestration) to keep the service catalog entry structure consistent with the architecture spec (source: `ARCH-SPEC.md` line 8).

## Restrictions and Prohibitions
- None derived from available evidence.

## Examples
- Valid: A test case verifies the "Inside FIFA" top navigation button opens a dropdown and each sub-item navigates correctly, matching FR-003 (source: `prd.md` line 32-39; `epics.md` line 58-70).
- Invalid: A test case requires user login/authentication to access a public "What FIFA Does" topic page, contradicting the no-authentication assumption ASM-003 (source: `prd.md` line 111-113).

## Traceability
- Domain Terms and Entities — `prd.md` line 4, 5, 12, 20-30, 32-39, 41-47; `epics.md` line 4, 12, 28, 42-54, 58-70, 74-86, 92, 109; `artifacts/inputs/documentation/adrs/ADR-SPEC.md` line 13-18; `artifacts/inputs/documentation/architecture/ARCH-SPEC.md` line 8, 92.
- Business Rules and Invariants — `prd.md` FR-001 (line 14-18), FR-002 (line 20-30), FR-003 (line 32-39), FR-004 (line 41-47), FR-005 (line 49-55), ASM-003 (line 111-113); `epics.md` EPIC-003 (line 42-54).

## Version and Owner
- Version: 1.0
- Owner: Project Team
