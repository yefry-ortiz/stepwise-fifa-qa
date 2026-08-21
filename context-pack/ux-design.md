---
when_to_read: |
  Read before writing or reviewing UI/navigation test scenarios, choosing viewports, or validating a user journey or interactive component — to use the actual journeys, components, and breakpoints under test.
---

# UX Design

## Purpose
ROUTING: This file indexes the three validated user journeys (Homepage Navigation Validation, "What FIFA Does" Subpages Navigation, Top Navigation "Inside FIFA" Validation), the two interactive components under test (top navigation "Inside FIFA" menu, language switching mechanism), and the viewport/state matrix (responsive breakpoints at 375px/768px/1440px and the initial desktop viewport 1920x1080) that must be used when authoring or reviewing UI, navigation, or interaction test scenarios.

## Scope
The InsideFIFA-WEB navigation and interaction surface under test: homepage, "What FIFA Does" subpages, top navigation menu, and language switching.

## Mandatory Rules
- Validate the Homepage Navigation Validation journey: homepage loads correctly and all primary navigation elements are functional (source: `prd.md` FR-001, line 11-18; `epics.md` EPIC-002, line 26-38).
- Validate the "What FIFA Does" Subpages Navigation journey: navigate from `/all-topics` to Legal, Transfer system, Women's Football, Advancing football, Refereeing, Innovation, and Talent development (source: `prd.md` FR-002, line 20-30; `epics.md` EPIC-003, line 42-54).
- Validate the Top Navigation "Inside FIFA" Validation journey: the "Inside FIFA" button and all sub-items are accessible and navigable (source: `prd.md` FR-003, line 32-39; `epics.md` EPIC-004, line 58-70).
- Test the top navigation "Inside FIFA" menu component: button accessibility, sub-item discovery/testability, dropdown/click functionality, and sub-item navigation (source: `epics.md` EPIC-004, line 67-70).
- Test the language switching mechanism/interaction: switch between English (fully tested), Spanish and French (testable), with language-specific selectors managed appropriately (source: `prd.md` FR-004, line 41-47; `epics.md` EPIC-005, line 74-86).
- Validate responsive behavior at the stated breakpoints: mobile (375px), tablet (768px), and large desktop (1440px), each with responsive element validation (source: `epics.md` EPIC-007, line 107-119).
- Use 1920x1080 as the initial supported desktop viewport (source: `prd.md` NFR-004, line 74-77).

## Recommended Rules
- None derived from available evidence.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No brand guidelines, visual design tokens, or style-guide content are stated in any of the four analyzed project inputs (status: pending - no evidence in inputs).

## Examples
- Valid: Validating the "Inside FIFA" top navigation dropdown by opening the menu and asserting navigation to each discovered sub-item.
- Invalid: Testing a viewport size (e.g., 1024px) that is not part of the stated breakpoint set (375px, 768px, 1440px, 1920x1080).

## Traceability
- User Journeys: `prd.md` FR-001 (line 11-18), FR-002 (line 20-30), FR-003 (line 32-39); `epics.md` EPIC-002 (line 26-38), EPIC-003 (line 42-54), EPIC-004 (line 58-70).
- Components and Interactions: `epics.md` EPIC-004 (line 67-70); `prd.md` FR-004 (line 41-47); `epics.md` EPIC-005 (line 74-86).
- States and Viewports: `epics.md` EPIC-007 (line 107-119); `prd.md` NFR-004 (line 74-77).
- Brand: status pending - no evidence in inputs across the four analyzed project inputs.

## Version and Owner
- Version: 1.0
- Owner: Project Team
