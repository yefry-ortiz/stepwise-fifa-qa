# Context Pack Input Analysis

**Session:** ANALYZE-STEPWISE-FIFA-QA-20260821
**Date:** 2026-08-21
**Scope:** This run analyzed ONLY the following selected inputs (per caller scope directive) -
everything else under `artifacts/inputs/` is out of scope and was not read, enumerated, or cited:
- `artifacts/inputs/documentation/adrs/` (ADR-SPEC.md)
- `artifacts/inputs/documentation/architecture/` (ARCH-SPEC.md)
- `artifacts/inputs/documentation/epics.md`
- `artifacts/inputs/documentation/prd.md`

Plus the tech-blueprints staged in Step 0: `node-typescript` (L2), `security-baseline` (L4),
`pre-ship-checklist` (L4). See `artifacts/inputs/tech-stack-selection.md` for the selection
rationale.

## For domain.md
### Domain Terms and Entities
status: complete
- **InsideFIFA-WEB**: project name for the web automation test project targeting `inside.fifa.com` (source: `prd.md` line 4; `epics.md` line 4).
- **Application under test**: `https://inside.fifa.com/`, a public FIFA website; this project validates its navigation and functionality rather than building it (source: `prd.md` line 5).
- **"What FIFA Does" subpages / topic areas**: 7 subpages reachable from `/all-topics` — Legal, Transfer system, Women's Football, Advancing football, Refereeing, Innovation, Talent development (source: `prd.md` line 20-30; `epics.md` line 42-54).
- **"Inside FIFA" top navigation menu**: top navigation button with sub-items requiring dropdown/click functionality and sub-item navigation validation (source: `prd.md` line 32-39; `epics.md` line 58-70).
- **MoSCoW priority** (Must Have / Should Have / Could Have): priority classification used for functional requirements and epics (source: `prd.md` line 12; `epics.md` line 12, 28, 44, 60, 76, 92, 109).
- **Multi-language support (EN, ES, FR)**: framework readiness requirement for testing English, Spanish, and French versions with language-specific selectors (source: `prd.md` line 41-47; `epics.md` line 74-86).
- **Page Object Model (POM)**: design pattern adopted for maintainable test code and separation of concerns (source: `artifacts/inputs/documentation/adrs/ADR-SPEC.md` line 13-18).
- **Slices**: sub-components composing a service catalog entry, e.g. Test runner, Browser management, Test orchestration for the Test Execution Engine (source: `artifacts/inputs/documentation/architecture/ARCH-SPEC.md` line 8).
- **Quality Gate**: named checkpoint (Gate-001/002/003) with pass/fail criteria that must be satisfied to progress code quality, test execution, and deployment readiness (source: `ARCH-SPEC.md` line 92).

### Business Rules and Invariants
status: complete
- Homepage must load within 3 seconds; all visible links clickable; no broken images or missing content; page responsive on desktop viewport (source: `prd.md` FR-001, line 14-18).
- All 7 "What FIFA Does" topic pages must be accessible and load content correctly (source: `prd.md` FR-002, line 20-30; `epics.md` EPIC-003, line 42-54).
- The "Inside FIFA" top navigation button and all sub-items must be accessible and navigable (source: `prd.md` FR-003, line 32-39).
- English (en) must be fully tested; Spanish (es) and French (fr) must be switchable and testable (source: `prd.md` FR-004, line 41-47).
- Current implementation targets Chrome only; architecture must support future extension to Firefox, Safari, Edge via externalized browser-specific configuration (source: `prd.md` FR-005, line 49-55).
- No authentication is assumed required to access the public pages under test — stated as a project assumption with High confidence (source: `prd.md` ASM-003, line 111-113).

## For arch-standards.md
### Architecture Patterns Observed
status: complete
- Page Object Model (POM) pattern adopted in TypeScript for maintainable test code and separation of concerns (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).
- Sequential Test Execution pattern: tests run sequentially to avoid resource conflicts, used for navigation testing where order matters (source: `architecture/ARCH-SPEC.md` line 53-55).
- Parallel Test Execution pattern: independent tests run in parallel for faster execution, used for multi-language testing scenarios (source: `architecture/ARCH-SPEC.md` line 57-59).
- Retry Mechanism pattern: failed tests automatically retry with exponential backoff, for handling network instability and timing issues (source: `architecture/ARCH-SPEC.md` line 61-63).
- Containerized Test Execution strategy: tests run in Docker containers for consistency across environments (source: `architecture/ARCH-SPEC.md` line 67-73).
- Service catalog organizes the framework into four logical services: Test Execution Engine (test runner, browser management, test orchestration), Page Object Management (element locators, page actions, validation helpers), Test Configuration Management (data loading, environment switching, language management), and Test Results and Reporting (report generation, screenshot capture, metrics collection) (source: `architecture/ARCH-SPEC.md` line 5-23).

### Architecture Anti-Patterns Found
status: pending - no evidence in inputs

## For tech-policy.md
### Tech Stack Detected
status: complete
- Test/automation framework: Playwright, selected for cross-browser support, modern API, and performance (source: `adrs/ADR-SPEC.md` ADR-001, line 5-11; `architecture/ARCH-SPEC.md` line 85).
- Language: TypeScript 5.0+ for type-safe development (source: `adrs/ADR-SPEC.md` line 19, 42; `architecture/ARCH-SPEC.md` line 86).
- Package manager / build tool: npm 10.0+ (source: `adrs/ADR-SPEC.md` line 43; `architecture/ARCH-SPEC.md` line 87).
- Test runner: Playwright Test 1.40+ (source: `adrs/ADR-SPEC.md` line 44).
- Reporting: Playwright HTML Reporter 1.40+, with screenshot capture on failures (source: `adrs/ADR-SPEC.md` ADR-004, line 29-35, 45; `architecture/ARCH-SPEC.md` line 90).
- Test data configuration: JSON configuration files, environment-specific, to support multi-language testing (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27, 46).
- Container platform: Docker 24.0+ for consistent, isolated test execution (source: `architecture/ARCH-SPEC.md` line 67-73, 88).
- CI/CD platform: GitHub Actions (latest), for automated test triggers, result artifact storage, and failure notifications (source: `architecture/ARCH-SPEC.md` line 75-79, 89).
- L2 blueprint `node-typescript` (staged per `tech-stack-selection.md`) supplies the TypeScript/Node.js coding, testing, and validation-tooling conventions that the project inputs do not otherwise specify in full (origin: blueprint, `blueprint_id: node-typescript`, `blueprint_version: 1.0`).

### Approved Libraries and Versions
status: complete
- Playwright v1.40+ (source: `adrs/ADR-SPEC.md` line 11, 41; `architecture/ARCH-SPEC.md` line 85).
- TypeScript 5.0+ (source: `adrs/ADR-SPEC.md` line 42; `architecture/ARCH-SPEC.md` line 86).
- npm 10.0+ (source: `adrs/ADR-SPEC.md` line 43; `architecture/ARCH-SPEC.md` line 87).
- Docker 24.0+ (source: `architecture/ARCH-SPEC.md` line 88).
- GitHub Actions - Latest (source: `architecture/ARCH-SPEC.md` line 89).

### Prohibited or Deprecated Dependencies
status: pending - no evidence in inputs

## For coding-standards.md
### Naming Conventions
status: complete
- Values and functions are `camelCase`, types and classes `PascalCase`, filenames `kebab-case` (origin: blueprint `node-typescript` §5, `blueprint_version: 1.0`). No naming convention is stated in the four project inputs analyzed; this rule is supplied by the staged L2 blueprint per the tech-stack-selection precedence rule (blueprint fills gaps where inputs are silent).

### Code Organization Rules
status: complete
- Page Object Model (POM) pattern: separate page objects, element locators, page actions, and validation helpers from test logic (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19; `architecture/ARCH-SPEC.md` SVC-002, line 10-13).
- Package by feature, not by technical layer; top-level `controllers/`, `services/`, `utils/` directories are prohibited (origin: blueprint `node-typescript` §5).
- The `any` type is prohibited; use `unknown` and narrow (origin: blueprint `node-typescript` §5, §10.1).
- Default exports are prohibited (origin: blueprint `node-typescript` §5).
- TypeScript strict mode is required; non-strict configuration is prohibited (origin: blueprint `node-typescript` §2, §10.1).

### Linting and Formatting Config
status: complete
- ESLint compliance is a Gate-001 (Code Quality) pass condition (source: `architecture/ARCH-SPEC.md` line 92-97).
- Lint and format are enforced in CI and never reviewed by humans; formatting is machine-enforced (origin: blueprint `node-typescript` §6; blueprint `pre-ship-checklist` Gate 0, §2).

### Error Handling and Logging Patterns
status: pending - no evidence in inputs
- The four project inputs analyzed (PRD, epics, ADR-SPEC, ARCH-SPEC) do not describe error-handling or logging conventions for the automation code itself. [INFERRED — supplied by blueprint, not a project fact] The `node-typescript` blueprint recommends a typed error hierarchy mapped centrally to responses, and structured JSON logging with redaction (origin: blueprint `node-typescript` §3, §5); this is a blueprint default, not a project-stated requirement, and is flagged in Open Questions.

## For automation-standards.md
### CI/CD Pipeline Details
status: complete
- GitHub Actions (latest) used for test execution, with automated test triggers on code changes, test result artifact storage, and a failure notification system (source: `architecture/ARCH-SPEC.md` line 75-79, 89).
- Containerized Test Execution: tests run in Docker 24.0+ containers for environment consistency, easy scaling, isolation from host system, and simplified dependency management (source: `architecture/ARCH-SPEC.md` line 65-73, 88).
- Four defined execution environments: Local Development (single machine, Chrome desktop, English), CI/CD Pipeline (containerized, Chrome headless, English), Staging Validation (cloud-based, Chrome desktop, EN/ES/FR), Production Monitoring (production environment, Chrome desktop, English) (source: `architecture/ARCH-SPEC.md` line 25-49).
- ENABLER-001 (Test Infrastructure Setup): establish CI/CD pipeline integration and test execution environment, depends on EPIC-001 (source: `epics.md` line 125-127).
- Test suite updates must be completed within 2 hours for UI changes (NFR-003 Maintainability) (source: `prd.md` line 69-72).
- Full test suite execution time target under 10 minutes (KPI-001) (source: `prd.md` line 121-124).

### Test Automation Framework
status: complete
- Playwright selected as the primary test automation framework for cross-browser support, modern API, and performance (source: `adrs/ADR-SPEC.md` ADR-001, line 5-11).
- Page Object Model (POM) pattern for maintainable test code (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).
- Playwright HTML Reporter with screenshot capture on failures (source: `adrs/ADR-SPEC.md` ADR-004, line 29-35).
- JSON configuration files, environment-specific, for test data supporting multi-language testing (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27).
- Sequential execution for order-dependent navigation tests; parallel execution for independent multi-language scenarios; automatic retry with exponential backoff for network instability and timing issues (source: `architecture/ARCH-SPEC.md` line 51-63).
- EPIC-001 (Core Navigation Framework, Must Have, High complexity) establishes the foundational framework: Chrome integration, basic POM, and test reporting (source: `epics.md` line 10-23).

## For test-standards.md
### Testing Approach
status: complete
- Homepage load within 3 seconds; all visible links clickable; no broken images/missing content; responsive on desktop viewport (source: `prd.md` FR-001, line 14-18).
- 95% test pass rate required in stable environment, measured as pass/fail ratio over 100 test executions (NFR-002 Reliability) (source: `prd.md` line 64-67).
- 100% of defined navigation paths must be covered by tests (KPI-002 Test Coverage) (source: `prd.md` line 126-129).
- Detect 90% of navigation issues before production (KPI-003 Defect Detection Rate) (source: `prd.md` line 131-134).
- Average page load time under 3 seconds, measured across 5 test runs (NFR-001 Performance) (source: `prd.md` line 59-62).
- Gate-001 Code Quality: TypeScript compilation without errors, ESLint compliance, code coverage > 80% (source: `architecture/ARCH-SPEC.md` line 92-97).
- Gate-002 Test Execution: all tests pass in local environment, performance benchmarks met, no critical test failures (source: `architecture/ARCH-SPEC.md` line 98-102).
- Gate-003 Deployment Readiness: tests pass in CI/CD pipeline, staging environment validation successful (source: `architecture/ARCH-SPEC.md` line 103-106).
- Desktop viewport (1920x1080) is the initial supported compatibility target (NFR-004) (source: `prd.md` line 74-77).
- Type checking must run as a CI gate separate from the build (origin: blueprint `node-typescript` §6).

### Test Frameworks and Patterns
status: complete
- Playwright Test 1.40+ as the test runner (source: `adrs/ADR-SPEC.md` line 44; `architecture/ARCH-SPEC.md` line 85).
- Page Object Model pattern: element locators, page actions, validation helpers separated per SVC-002 (source: `architecture/ARCH-SPEC.md` line 10-13).
- Sequential execution for order-dependent tests, parallel execution for independent multi-language tests, retry with exponential backoff for flaky network/timing conditions (source: `architecture/ARCH-SPEC.md` line 51-63).
- Multi-viewport testing planned: mobile (375px), tablet (768px), large desktop (1440px) (source: `epics.md` EPIC-007, line 107-119).
- Cross-browser expansion planned (Firefox, Safari, Edge) beyond current Chrome-only implementation (source: `prd.md` FR-005, line 49-55; `epics.md` EPIC-006, line 90-104).
- SPIKE-001 Selector Strategy Research (dynamic FIFA website content, 2 days), SPIKE-002 Performance Testing Integration (3 days), SPIKE-003 Visual Testing Framework evaluation (2 days) (source: `epics.md` line 139-152).

## For security.md
### Security Patterns Found
status: pending - no evidence in inputs
- None of the four analyzed project inputs (PRD, epics, ADR-SPEC, ARCH-SPEC) describe authentication, authorization, secrets handling, or other security patterns for this project. The project's own assumption is that no authentication is required for the public pages under test (source: `prd.md` ASM-003, line 111-113), which is a scope statement rather than a security pattern.
- Security controls are supplied entirely by the staged L4 blueprints (origin: blueprint): `security-baseline` states platform-neutral invariants (identity delegation, input validation, secrets management, dependency supply-chain review — `blueprint_id: security-baseline`, §3-§9); `pre-ship-checklist` states verification gates including a dependency audit and secret scan in Gate 0 (`blueprint_id: pre-ship-checklist`, §2).

### Security Gaps or Risks
status: complete
- RSK-002 Rate Limiting/Blocking: automated access to the target site may trigger rate limiting or IP blocking; mitigation is test delays and possibly rotating IP addresses (source: `prd.md` line 86-89).
- RSK-001 Website Structure Changes: FIFA may change website structure, breaking navigation tests; mitigation is robust selectors and regular test maintenance (source: `prd.md` line 81-84).
- [Blueprint gap, not a project fact] Dependency audit is a required Gate-0 CI check per `pre-ship-checklist` (§2) and `node-typescript` (§8, `pnpm audit --audit-level=high`), but no dependency-audit command or tool is named in any of the four analyzed project inputs — see Open Questions.

## For constraints.md
### Hard Constraints Found
status: complete
- Website `https://inside.fifa.com/` must be available during test execution (ASM-001, high confidence) (source: `prd.md` line 103-105).
- Test execution environment must have stable internet connectivity (ASM-002, high confidence) (source: `prd.md` line 107-109).
- Page structure assumed to remain consistent during development phase (ASM-004, medium confidence) (source: `prd.md` line 115-117).
- Browser-specific configurations must be externalized to support future browser expansion (source: `prd.md` FR-005, line 55).
- Code coverage must exceed 80% (Gate-001 Code Quality) (source: `architecture/ARCH-SPEC.md` line 97).
- Tests must pass in CI/CD pipeline and staging environment validation must succeed before production readiness (Gate-003) (source: `architecture/ARCH-SPEC.md` line 103-106).
- Test data management (JSON config strategy) carries Medium risk and requires careful data maintenance (source: `adrs/ADR-SPEC.md` Impact Matrix, line 54).

### Known Organizational Limits
status: complete
- EPIC-005 (Multi-language) depends on all of EPIC-001, EPIC-002, EPIC-003, EPIC-004 completing first (source: `epics.md` line 81, 162).
- ENABLER-003 (Reporting Dashboard) depends on all epics (source: `epics.md` line 135, 144-145).
- Priority distribution constrains scope: 4 Must Have (EPIC-001..004), 1 Should Have (EPIC-005), 2 Could Have (EPIC-006, EPIC-007) (source: `epics.md` line 166-172).
- Complexity distribution: 3 High-complexity epics (EPIC-001, EPIC-005, EPIC-006), 4 Medium-complexity epics (EPIC-002, EPIC-003, EPIC-004, EPIC-007) (source: `epics.md` line 174-179).
- Different language versions may have different page structures, requiring language-specific test configurations (RSK-003) (source: `prd.md` line 91-94).
- Tests may fail on different browser versions, requiring version-specific test configurations and regular updates (RSK-004) (source: `prd.md` line 96-99).

## For customer-background.md
### Customer Context
status: complete
- Project name: InsideFIFA-WEB, version 1.0.0, dated 2026-08-21 (source: `prd.md` line 4-7; `epics.md` line 4-6).
- Primary users: QA Engineer (executes and maintains the test suite), Test Automation Engineer (develops and enhances the test framework), Project Manager (reviews test results and quality metrics) (source: `prd.md` line 138-141).
- Secondary users: Development Team (uses test results for debugging) (source: `prd.md` line 143-144).
- The customer/target system is FIFA's public "Inside FIFA" content site (`https://inside.fifa.com/`), which this project does not own or build — it validates the site's navigation and functionality (source: `prd.md` line 5).

## For env-config.md and config-management-rules.md
### Environment Configuration
status: complete
- Four environments defined: Local Development (single machine, Chrome desktop, English), CI/CD Pipeline (containerized, Chrome headless, English), Staging Validation (cloud-based, Chrome desktop, EN/ES/FR), Production Monitoring (production environment, Chrome desktop, English) (source: `architecture/ARCH-SPEC.md` line 25-49).
- Test data and environment-specific configuration is managed via JSON configuration files (source: `adrs/ADR-SPEC.md` ADR-003, line 21-27).
- Configuration Service (SVC-003) handles data loading, environment switching, and language management (source: `architecture/ARCH-SPEC.md` line 15-18).

### Secret and Config Management
status: pending - no evidence in inputs
- None of the four analyzed project inputs describe a secret-management approach; the project targets a public, unauthenticated site (ASM-003, `prd.md` line 111-113), so no application secrets are evidenced. [Blueprint default, not a project fact] `node-typescript` (§12) and `security-baseline` (§5) require environment variables to be validated through a schema at startup and secrets to come from a managed secret store — flagged in Open Questions since the project inputs do not confirm whether any secrets exist for this test-only project.

## For best-practices.md
### General Best Practices Observed
status: complete
- Robust selectors and regular test maintenance to withstand website structure changes (source: `prd.md` RSK-001, line 81-84).
- Test delays and considering rotating IP addresses to mitigate rate limiting/blocking risk (source: `prd.md` RSK-002, line 86-89).
- Language-specific test configurations to handle content localization variations (source: `prd.md` RSK-003, line 91-94).
- Version-specific test configurations and regular updates to handle browser compatibility issues (source: `prd.md` RSK-004, line 96-99).
- SPIKE-001 Selector Strategy Research: dedicated research effort for optimal selector strategies against dynamic FIFA website content (source: `epics.md` line 139-142).
- Separation of concerns via Page Object Model to keep test code maintainable (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).
- Treat the dependency tree as an attack surface: audit in CI, pin, and review additions (origin: blueprint `node-typescript` §10 Playbook item 5).

## For infrastructure.md
### Infrastructure Topology
status: complete
- Test execution runs in Docker 24.0+ containers for environment consistency, easy scaling, isolation from host system, and simplified dependency management (source: `architecture/ARCH-SPEC.md` line 65-73, 88).
- No dedicated hosting/deployment surface exists for this project's own deliverable — it is a test suite validating an externally-owned website, not a hosted service (source: `tech-stack-selection.md` Binding constraint, derived from `prd.md` line 5 and `architecture/ARCH-SPEC.md` line 25-49). Per the tech-stack selection, no L1 cloud platform was selected.

### Infrastructure-as-Code Tooling
status: pending - no evidence in inputs
- None of the four analyzed inputs mention an IaC tool (Terraform, Pulumi, CloudFormation, etc.). `iac-platform-engineering` (L4) was not selected as a blueprint since no infrastructure is being defined by this project beyond containerized test execution — see `tech-stack-selection.md`.

### Deployment Targets and Environments
status: complete
- Local Development (ENV-001): single machine, Chrome desktop, English (source: `architecture/ARCH-SPEC.md` line 27-31).
- CI/CD Pipeline (ENV-002): containerized environment, Chrome headless, English (source: `architecture/ARCH-SPEC.md` line 33-37).
- Staging Validation (ENV-003): cloud-based test environment, Chrome desktop, English/Spanish/French (source: `architecture/ARCH-SPEC.md` line 39-43).
- Production Monitoring (ENV-004): production environment, Chrome desktop, English — production smoke tests and monitoring (source: `architecture/ARCH-SPEC.md` line 45-49).
- GitHub Actions is the CI/CD platform integrating these environments (source: `architecture/ARCH-SPEC.md` line 75-79, 89).

## For validation-tools.md
### Build Commands
status: pending - no evidence in inputs
- None of the four analyzed project inputs state a literal build command; only tool names are given (npm 10.0+, TypeScript 5.0+) (source: `adrs/ADR-SPEC.md` line 42-43; `architecture/ARCH-SPEC.md` line 86-87). [Blueprint default] `node-typescript` §6 suggests `pnpm build`, but the project's stated package manager is npm, not pnpm — see Open Questions for this discrepancy.

### Test Commands
status: pending - no evidence in inputs
- No literal test command is stated. Playwright Test 1.40+ is the named test runner (source: `adrs/ADR-SPEC.md` line 44). The conventional invocation for this runner is `npx playwright test`, but this exact command does not appear in any analyzed input — flagged in Open Questions rather than asserted as fact.

### Lint and Format Commands
status: pending - no evidence in inputs
- ESLint compliance is required (Gate-001) but no literal lint command is stated (source: `architecture/ARCH-SPEC.md` line 96).

### Typecheck Commands
status: pending - no evidence in inputs
- TypeScript compilation without errors is required (Gate-001) but no literal typecheck command is stated (source: `architecture/ARCH-SPEC.md` line 95).

### Security Scan Commands
status: pending - no evidence in inputs
- No dependency-audit or secret-scan command is named in any of the four analyzed inputs. [Blueprint default] `node-typescript` §8 proposes `pnpm audit --audit-level=high`; `pre-ship-checklist` Gate 0 requires a dependency-audit and secret-scan step generically without naming a tool.

### Pre-commit and CI Validation Order
status: complete
- Gate-001 Code Quality -> Gate-002 Test Execution -> Gate-003 Deployment Readiness, in that order (source: `architecture/ARCH-SPEC.md` line 92-106).
- Gate-001: TypeScript compilation without errors, ESLint compliance, code coverage > 80% (source: `architecture/ARCH-SPEC.md` line 94-97).
- Gate-002: all tests pass locally, performance benchmarks met, no critical test failures (source: `architecture/ARCH-SPEC.md` line 99-102).
- Gate-003: tests pass in CI/CD pipeline, staging environment validation successful (source: `architecture/ARCH-SPEC.md` line 104-106).
- CI/CD platform is GitHub Actions, with automated triggers on code changes, artifact storage, and failure notifications (source: `architecture/ARCH-SPEC.md` line 75-79, 89).

## For project-inventory.md
### Naming Conventions and Patterns
status: complete
- `camelCase` for values/functions, `PascalCase` for types/classes, `kebab-case` for filenames (origin: blueprint `node-typescript` §5) — not stated in the four project inputs analyzed; supplied by the staged L2 blueprint.

### Project and Bounded Context Inventory
status: complete
- SVC-001 Test Execution Engine: core service running web automation tests; slices — Test runner, Browser management, Test orchestration (source: `architecture/ARCH-SPEC.md` line 5-8).
- SVC-002 Page Object Management: manages page object models and element interactions; slices — Element locators, Page actions, Validation helpers (source: `architecture/ARCH-SPEC.md` line 10-13).
- SVC-003 Test Configuration Management: handles test data, environments, and language configurations; slices — Data loading, Environment switching, Language management (source: `architecture/ARCH-SPEC.md` line 15-18).
- SVC-004 Test Results and Reporting: generates test reports and captures execution metrics; slices — Report generation, Screenshot capture, Metrics collection (source: `architecture/ARCH-SPEC.md` line 20-23).
- EPIC-001..007, ENABLER-001..003, SPIKE-001..003 form the delivery-scope inventory (source: `epics.md` line 10-152; see Source Summary for full list).

### Key Types per Bounded Context
status: pending - no evidence in inputs
- No file/directory paths or concrete type/class names are given for SVC-001..004 in any of the four analyzed inputs (source: `architecture/ARCH-SPEC.md` open_questions).

### Build and Test Commands
status: pending - no evidence in inputs
- See `for-validation-tools.md` above — no literal commands are stated in the analyzed inputs; only tool names (npm, Playwright Test, ESLint, TypeScript).

### Domain to Path Lookup
status: pending - no evidence in inputs
- No repository paths are given for any of the four cataloged services (SVC-001 to SVC-004) in `architecture/ARCH-SPEC.md` (source: `architecture/ARCH-SPEC.md` open_questions, line 134-135).

### Stable Invariants
status: complete
- Playwright is the fixed test automation framework (ADR-001, Accepted) (source: `adrs/ADR-SPEC.md` line 5-11).
- Page Object Model is the fixed architectural pattern (ADR-002, Accepted) (source: `adrs/ADR-SPEC.md` line 13-19).
- JSON is the fixed test-data configuration format (ADR-003, Accepted) (source: `adrs/ADR-SPEC.md` line 21-27).
- Playwright HTML Reporter is the fixed reporting mechanism (ADR-004, Accepted) (source: `adrs/ADR-SPEC.md` line 29-35).

## For codebase-map.md
### Solution Structure
status: pending - no evidence in inputs
- No repository source code was in scope for this run (only `artifacts/inputs/documentation/adrs`, `architecture`, `epics.md`, `prd.md` were analyzed; `artifacts/inputs/source-code/` is out of scope). The architecture-level service catalog (SVC-001..004) is documented above under `for-project-inventory.md`, but no directory structure is stated in any analyzed input.

### Bounded Contexts in Scope
status: complete
- Test Execution Engine, Page Object Management, Test Configuration Management, Test Results and Reporting form the four architectural service boundaries (source: `architecture/ARCH-SPEC.md` line 3-23).

### Layer Conventions
status: complete
- Package by feature, not by technical layer; route -> service -> repository style layering with domain logic independent of the HTTP framework (origin: blueprint `node-typescript` §3, §5) — no project-stated layering convention exists in the four analyzed inputs beyond the POM separation of page objects from test logic (source: `adrs/ADR-SPEC.md` ADR-002, line 13-19).

### Cross-BC Integration Patterns
status: pending - no evidence in inputs
- The four analyzed inputs describe intra-suite execution patterns (sequential, parallel, retry — `architecture/ARCH-SPEC.md` line 51-63) but no integration pattern between the four cataloged services (SVC-001..004) themselves.

### Frontend Layout
status: pending - no evidence in inputs
- Not applicable / not stated: this project is a test-automation suite, not a frontend application; no frontend layout is described in any of the four analyzed inputs.

### Test Layout
status: pending - no evidence in inputs
- No test-file directory layout is stated in any of the four analyzed inputs; only the logical service split (SVC-001..004) and the POM pattern are documented (source: `architecture/ARCH-SPEC.md` line 3-23; `adrs/ADR-SPEC.md` line 13-19).

### Where to Look for X
status: pending - no evidence in inputs
- Cannot be populated: no file/directory paths exist in any of the four analyzed inputs (source: `architecture/ARCH-SPEC.md` open_questions, line 134-135).

## For ux-design.md
### User Journeys
status: complete
- Homepage Navigation Validation journey: homepage loads correctly, all primary navigation elements functional (source: `prd.md` FR-001, line 11-18; `epics.md` EPIC-002, line 26-38).
- "What FIFA Does" Subpages Navigation journey: navigate to Legal, Transfer system, Women's Football, Advancing football, Refereeing, Innovation, Talent development from `/all-topics` (source: `prd.md` FR-002, line 20-30; `epics.md` EPIC-003, line 42-54).
- Top Navigation "Inside FIFA" Validation journey: "Inside FIFA" button and all sub-items accessible and navigable (source: `prd.md` FR-003, line 32-39; `epics.md` EPIC-004, line 58-70).

### Components and Interactions
status: complete
- Top navigation "Inside FIFA" menu: button accessible, sub-items discovered/testable, dropdown/click functionality, sub-item navigation (source: `epics.md` EPIC-004, line 67-70).
- Language switching mechanism/interaction: switch between English (fully tested), Spanish and French (testable), with language-specific selectors managed (source: `prd.md` FR-004, line 41-47; `epics.md` EPIC-005, line 74-86).

### States and Viewports
status: complete
- Responsive breakpoint states: mobile (375px), tablet (768px), large desktop (1440px), each with responsive element validation (source: `epics.md` EPIC-007, line 107-119).
- Initial supported desktop viewport: 1920x1080 (source: `prd.md` NFR-004, line 74-77).

### Brand
status: pending - no evidence in inputs
- None of the four analyzed inputs describe brand guidelines, visual design tokens, or style-guide content.

## Source Summary
status: complete

This run's scope was restricted by the caller to four selected inputs; all other content under
`artifacts/inputs/` (e.g. `artifacts/inputs/source-code/`, `artifacts/inputs/meetings/`) was
explicitly out of scope and was not read, enumerated, or cited.

| Raw source | Category | Extraction |
|---|---|---|
| `artifacts/inputs/documentation/prd.md` (144 lines) | Documentation - PRD | `_extractions/prd.md` |
| `artifacts/inputs/documentation/epics.md` (179 lines) | Documentation - Epics | `_extractions/epics.md` |
| `artifacts/inputs/documentation/adrs/ADR-SPEC.md` (54 lines) | Documentation - ADR catalog | `_extractions/ADR-SPEC.md` |
| `artifacts/inputs/documentation/architecture/ARCH-SPEC.md` (106 lines) | Documentation - Architecture spec | `_extractions/ARCH-SPEC.md` |

Blueprint inputs (Step 0, `tech-blueprints`) — extractions lifted verbatim from each blueprint's
own §14 Normative Profile, not separately distilled:

| Blueprint | Layer | Staged path |
|---|---|---|
| `node-typescript` | L2 | `artifacts/inputs/tech-blueprints/l2-stacks/node-typescript.md` |
| `security-baseline` | L4 | `artifacts/inputs/tech-blueprints/l4-concerns/security-baseline.md` |
| `pre-ship-checklist` | L4 | `artifacts/inputs/tech-blueprints/l4-concerns/pre-ship-checklist.md` |

No L1 platform, L3 agentic blueprint, or additional L4/L2 blueprints were selected — see
`artifacts/inputs/tech-stack-selection.md` for the full rationale. `prd.md` and `epics.md`
together are the sole source for `domain.md` and `customer-background.md` content, as expected
(no blueprint produces those two files).

- `prd.md` — Product Requirements Document: functional/non-functional requirements, risks,
  assumptions, KPIs, personas for the InsideFIFA-WEB test-automation project.
- `epics.md` — Epic breakdown: 7 epics, 3 enablers, 3 spikes, priority/complexity distributions,
  and an epic dependency matrix.
- `adrs/ADR-SPEC.md` — 4 Architecture Decision Records (Playwright framework, POM pattern, JSON
  test-data strategy, Playwright HTML reporting), a technology stack matrix, and an impact
  matrix.
- `architecture/ARCH-SPEC.md` — 4-service architecture catalog, 4 execution environments,
  3 integration patterns, deployment strategy (Docker + GitHub Actions), technology stack table,
  and 3 quality gates.

## Open Questions
status: complete

- **OQ-01** (question, subject: tech-stack, blocking: false): What specific automation stack details beyond Playwright/TypeScript/npm (e.g. exact `package.json` scripts for build, test, lint, typecheck, security scan) does the actual repository use? Why it matters: `for-validation-tools.md` and `for-project-inventory.md` cannot state literal commands from the four analyzed inputs alone — only tool names are given (`adrs/ADR-SPEC.md` line 39-46; `architecture/ARCH-SPEC.md` line 81-90). Assumption if unanswered: the `node-typescript` blueprint's illustrative commands (`pnpm build`, `pnpm vitest run`, etc.) do not apply as-is since the project's stated package manager is npm, not pnpm; validation-tools sections remain `pending`.
- **OQ-02** (question, subject: scope, blocking: false): Is there a dedicated non-production/staging test environment separate from the live `https://inside.fifa.com/`, or do all automated tests run against the live production site? Why it matters: RSK-002 (rate limiting/blocking) and ASM-001 (`prd.md` line 86-89, 103-105) imply testing against the live site; `architecture/ARCH-SPEC.md` ENV-003 names a "Staging Validation" environment (line 39-43) but does not clarify whether it targets a separate host or the same production site. Assumption if unanswered: tests run against the live production site as implied by ASM-001.
- **OQ-03** (question, subject: security, blocking: false): Does this test-automation project handle any secrets or credentials (e.g. API keys, service accounts) despite targeting an unauthenticated public site? Why it matters: `security-baseline` (L4 blueprint) mandates a managed secret store and startup-schema-validated environment variables, but none of the four analyzed project inputs mention any secret. Assumption if unanswered: no secrets exist for this project; the security-baseline secret-management rules are inherited as a floor but have nothing to apply to yet.
- **OQ-04** (question, subject: architecture, blocking: false): What are the actual repository paths/directory structure for the four cataloged services (SVC-001 Test Execution Engine, SVC-002 Page Object Management, SVC-003 Test Configuration Management, SVC-004 Test Results and Reporting)? Why it matters: `for-codebase-map.md` and `for-project-inventory.md` (Domain to Path Lookup, Key Types per Bounded Context) cannot be populated without this — `architecture/ARCH-SPEC.md` names services and slices but never a file/directory path (line 5-23). Assumption if unanswered: those sections remain `pending — no evidence in inputs` in the generated context pack.
- **OQ-05** (question, subject: coding-standards, blocking: false): Are there project-specific error-handling and logging conventions, or does the project intend to adopt the `node-typescript` blueprint's typed-error-hierarchy and structured-JSON-logging defaults as-is? Why it matters: none of the four analyzed inputs state an error-handling/logging pattern for the automation code. Assumption if unanswered: the blueprint defaults apply (typed error hierarchy, structured JSON logs with redaction — origin: blueprint `node-typescript` §3, §5).
- **OQ-06** (assumption, subject: tech-stack, blocking: false): [INFERRED] `epics.md` implies a specific test framework/reporting tool for "Chrome browser integration" and "test reporting mechanism" (EPIC-001, line 20-22) without naming one directly in that file; `adrs/ADR-SPEC.md` and `architecture/ARCH-SPEC.md` independently confirm this is Playwright with its HTML reporter. This is consistent across the four inputs, not a contradiction, and is recorded here only because `epics.md` in isolation does not name the tool.

All items above are non-blocking: this analysis proceeded on the stated assumptions and marked
every affected section `pending - no evidence in inputs` rather than fabricating detail. No
blueprint-selection outcome blocked this run (Step 0 completed with a full non-empty selection:
`node-typescript`, `security-baseline`, `pre-ship-checklist`; no L1 or L3 blueprint applies to a
test-automation-only project with no agentic component and no hosting surface of its own).
