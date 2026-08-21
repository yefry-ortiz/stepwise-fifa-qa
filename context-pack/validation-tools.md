---
when_to_read: |
  Read before running build/test/lint/typecheck/security checks or wiring pre-commit/CI — to use the exact project commands and required pass order.
---

# Validation Tools

## Purpose
This file governs the validation commands (build, test, lint, typecheck, security scan) and the pre-commit/CI gate order that must be followed before code is considered ready to merge or deploy, based on the evidence in `analysis.md` (section "For validation-tools.md").

## Scope
Applies to all engineering work on this project that touches build, test, lint, typecheck, or security-scan tooling, and to anyone configuring pre-commit hooks or the GitHub Actions CI/CD pipeline.

## Mandatory Rules
- Gate-001 Code Quality -> Gate-002 Test Execution -> Gate-003 Deployment Readiness is the mandatory validation order (source: `architecture/ARCH-SPEC.md` line 92-106).
- Gate-001 requires: TypeScript compilation without errors, ESLint compliance, code coverage > 80% (source: `architecture/ARCH-SPEC.md` line 94-97).
- Gate-002 requires: all tests pass locally, performance benchmarks met, no critical test failures (source: `architecture/ARCH-SPEC.md` line 99-102).
- Gate-003 requires: tests pass in CI/CD pipeline, staging environment validation successful (source: `architecture/ARCH-SPEC.md` line 104-106).
- CI/CD platform is GitHub Actions with automated triggers, artifact storage, and failure notifications (source: `architecture/ARCH-SPEC.md` line 75-79, 89).
- [TO BE COMPLETED] No literal build, test, lint, typecheck, or security-scan command is stated in the four analyzed project inputs (status: pending - no evidence in inputs); only tool names are given (npm 10.0+, TypeScript 5.0+, Playwright Test 1.40+, ESLint). See analysis.md Open Questions OQ-01 for the tracked gap.

## Recommended Rules
- [INFERRED] `node-typescript` §6 suggests `pnpm build`, but this conflicts with the project's stated npm package manager — do not adopt as-is; flagged in Open Questions.
- [INFERRED] Conventional Playwright invocation `npx playwright test` is plausible given Playwright Test 1.40+ is the named runner, but does not appear in any analyzed input.
- [INFERRED] `node-typescript` §8 proposes `pnpm audit --audit-level=high` for a security scan; `pre-ship-checklist` Gate 0 requires a dependency-audit and secret-scan step generically without naming a tool.

## Restrictions and Prohibitions
- [TO BE COMPLETED] No tools or commands are explicitly prohibited in the four analyzed project inputs.

## Examples
- Valid: Running Gate-001 checks (TypeScript compilation, ESLint, coverage > 80%) locally and confirming they pass before triggering Gate-002 test execution.
- Invalid: Skipping the Gate-001 code-coverage check and proceeding straight to Gate-003 staging validation without first confirming Gate-002 test execution passed.

## Traceability
- Gate order and gate contents (Gate-001, Gate-002, Gate-003): `architecture/ARCH-SPEC.md` line 92-106 (project-stated, `source:`).
- CI/CD platform (GitHub Actions, triggers, artifact storage, failure notifications): `architecture/ARCH-SPEC.md` line 75-79, 89 (project-stated, `source:`).
- Named tools without literal commands (npm 10.0+, TypeScript 5.0+): `adrs/ADR-SPEC.md` line 42-43; `architecture/ARCH-SPEC.md` line 86-87 (project-stated, `source:`).
- Named test runner (Playwright Test 1.40+) without literal command: `adrs/ADR-SPEC.md` line 44 (project-stated, `source:`).
- ESLint compliance requirement without literal lint command: `architecture/ARCH-SPEC.md` line 96 (project-stated, `source:`).
- TypeScript compilation requirement without literal typecheck command: `architecture/ARCH-SPEC.md` line 95 (project-stated, `source:`).
- `pnpm build` suggestion: origin: blueprint, `node-typescript` §6 [Blueprint default].
- `pnpm audit --audit-level=high` suggestion: origin: blueprint, `node-typescript` §8 [Blueprint default]; generic dependency-audit/secret-scan requirement also from `pre-ship-checklist` Gate 0 [Blueprint default].

## Version and Owner
- Version: 1.0
- Owner: Project Team
