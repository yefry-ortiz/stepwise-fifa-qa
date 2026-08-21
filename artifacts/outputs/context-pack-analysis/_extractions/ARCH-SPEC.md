tech_signals:
  - signal: "Automation Framework: Playwright 1.40+"
    evidence: "| Automation Framework | Playwright | 1.40+ |"
    line: 85
    origin: input
  - signal: "Programming Language: TypeScript 5.0+"
    evidence: "| Programming Language | TypeScript | 5.0+ |"
    line: 86
    origin: input
  - signal: "Package Manager: npm 10.0+"
    evidence: "| Package Manager | npm | 10.0+ |"
    line: 87
    origin: input
  - signal: "Container Platform: Docker 24.0+"
    evidence: "| Container Platform | Docker | 24.0+ |"
    line: 88
    origin: input
  - signal: "CI/CD Platform: GitHub Actions (Latest)"
    evidence: "| CI/CD Platform | GitHub Actions | Latest |"
    line: 89
    origin: input
  - signal: "Reporting: Playwright HTML Reporter 1.40+"
    evidence: "| Reporting | Playwright HTML Reporter | 1.40+ |"
    line: 90
    origin: input

domain_terms:
  - term: "Slices"
    definition_or_usage: "Sub-components composing a service, e.g. Test runner, Browser management, Test orchestration for the Test Execution Engine"
    line: 8
    origin: input
  - term: "Quality Gate"
    definition_or_usage: "Named checkpoint (Gate-001/002/003) with pass/fail criteria that must be satisfied to progress code quality, test execution, and deployment readiness"
    line: 92
    origin: input

standards:
  - area: arch
    rule: "Sequential Test Execution pattern: tests run sequentially to avoid resource conflicts, used for navigation testing where order matters"
    line: 53
    origin: input
  - area: arch
    rule: "Parallel Test Execution pattern: independent tests run in parallel for faster execution, used for multi-language testing scenarios"
    line: 57
    origin: input
  - area: arch
    rule: "Retry Mechanism pattern: failed tests automatically retry with exponential backoff, used for handling network instability and timing issues"
    line: 61
    origin: input
  - area: arch
    rule: "Containerized Test Execution strategy: tests run in Docker containers for consistency across environments"
    line: 67
    origin: input
  - area: automation
    rule: "CI/CD Integration: GitHub Actions used for test execution, with automated test triggers on code changes, test result artifacts storage, and a failure notification system"
    line: 75
    origin: input
  - area: test
    rule: "Gate-001 Code Quality: TypeScript compilation without errors, ESLint compliance, code coverage > 80%"
    line: 94
    origin: input
  - area: test
    rule: "Gate-002 Test Execution: all tests pass in local environment, performance benchmarks met, no critical test failures"
    line: 99
    origin: input
  - area: test
    rule: "Gate-003 Deployment Readiness: tests pass in CI/CD pipeline, staging environment validation successful, production smoke tests pass"
    line: 104
    origin: input

constraints:
  - constraint: "Code coverage must exceed 80% (Gate-001: Code Quality)"
    source: "Quality Gates - Gate-001: Code Quality"
    line: 97
    origin: input
  - constraint: "All tests must pass in the local environment and performance benchmarks must be met before proceeding (Gate-002: Test Execution)"
    source: "Quality Gates - Gate-002: Test Execution"
    line: 100
    origin: input
  - constraint: "Tests must pass in the CI/CD pipeline and staging environment validation must succeed before production readiness (Gate-003: Deployment Readiness)"
    source: "Quality Gates - Gate-003: Deployment Readiness"
    line: 104
    origin: input

infra:
  - item: "Local Development (ENV-001)"
    detail: "Development and debugging of test cases; single machine scale; Chrome desktop browser; English (en) language"
    line: 27
    origin: input
  - item: "CI/CD Pipeline (ENV-002)"
    detail: "Automated test execution in build pipeline; containerized environment; Chrome headless browser; English (en) language"
    line: 33
    origin: input
  - item: "Staging Validation (ENV-003)"
    detail: "Pre-production validation against staging environment; cloud-based test environment; Chrome desktop browser; English (en), Spanish (es), French (fr) languages"
    line: 39
    origin: input
  - item: "Production Monitoring (ENV-004)"
    detail: "Production smoke tests and monitoring; production environment; Chrome desktop browser; English (en) language"
    line: 45
    origin: input
  - item: "Containerized Test Execution"
    detail: "Tests run in Docker containers for consistency across environments; benefits: environment consistency, easy scaling, isolation from host system, simplified dependency management"
    line: 67
    origin: input
  - item: "CI/CD Integration"
    detail: "GitHub Actions for test execution; automated test triggers on code changes; test result artifacts storage; failure notification system"
    line: 75
    origin: input

inventory:
  - project_or_type: "Test Execution Service (SVC-001) - Test Execution Engine"
    path: "N/A - no path specified in ARCH-SPEC.md"
    role: "Core service responsible for running web automation tests; slices: Test runner, Browser management, Test orchestration"
    line: 5
    origin: input
  - project_or_type: "Page Object Service (SVC-002) - Page Object Management"
    path: "N/A - no path specified in ARCH-SPEC.md"
    role: "Manages page object models and element interactions; slices: Element locators, Page actions, Validation helpers"
    line: 10
    origin: input
  - project_or_type: "Configuration Service (SVC-003) - Test Configuration Management"
    path: "N/A - no path specified in ARCH-SPEC.md"
    role: "Handles test data, environments, and language configurations; slices: Data loading, Environment switching, Language management"
    line: 15
    origin: input
  - project_or_type: "Reporting Service (SVC-004) - Test Results and Reporting"
    path: "N/A - no path specified in ARCH-SPEC.md"
    role: "Generates test reports and captures execution metrics; slices: Report generation, Screenshot capture, Metrics collection"
    line: 20
    origin: input

open_questions:
  - question: "No repository paths or module locations are given for the four cataloged services (SVC-001 to SVC-004) — is there a source mapping services to actual code folders?"
    why: "The Service Catalog only names services and their slices, without file/directory paths."
  - question: "No literal build/test/lint/typecheck/security commands are stated, only tool names (npm, ESLint, TypeScript, Playwright) — are the actual CLI commands documented elsewhere?"
    why: "The Technology Stack table and Quality Gates reference tools (ESLint, TypeScript compiler, npm) but no concrete command strings appear in this file, so the `commands` field is omitted rather than invented."
  - question: "No design artifacts (journeys, screens, components, brand, interaction) are described in this file — is UI/UX design documented in a separate spec?"
    why: "ARCH-SPEC.md is limited to services, environments, integration patterns, deployment, tech stack, and quality gates; no screen/journey/component content is present, so the `design` field is omitted rather than invented."
