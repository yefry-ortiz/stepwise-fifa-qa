tech_signals:
  - signal: "Playwright selected as primary test automation framework"
    evidence: "Selected Playwright for web automation due to cross-browser support, modern API, and excellent performance"
    line: 10
    origin: input
  - signal: "Playwright v1.40+"
    evidence: "Technology: Playwright v1.40+"
    line: 11
    origin: input
  - signal: "TypeScript with POM pattern"
    evidence: "Technology: TypeScript with POM pattern"
    line: 19
    origin: input
  - signal: "JSON configuration with environment-specific files"
    evidence: "Technology: JSON configuration with environment-specific files"
    line: 27
    origin: input
  - signal: "Playwright HTML Reporter"
    evidence: "Technology: Playwright HTML Reporter"
    line: 35
    origin: input
  - signal: "Test Framework: Playwright 1.40+ (Web automation)"
    evidence: "| Test Framework | Playwright | 1.40+ | Web automation |"
    line: 41
    origin: input
  - signal: "Language: TypeScript 5.0+ (Type-safe development)"
    evidence: "| Language | TypeScript | 5.0+ | Type-safe development |"
    line: 42
    origin: input
  - signal: "Build Tool: npm 10.0+ (Package management)"
    evidence: "| Build Tool | npm | 10.0+ | Package management |"
    line: 43
    origin: input
  - signal: "Test Runner: Playwright Test 1.40+ (Test execution)"
    evidence: "| Test Runner | Playwright Test | 1.40+ | Test execution |"
    line: 44
    origin: input
  - signal: "Reporting: Playwright HTML 1.40+ (Test results)"
    evidence: "| Reporting | Playwright HTML | 1.40+ | Test results |"
    line: 45
    origin: input
  - signal: "Configuration: JSON (Test data management)"
    evidence: "| Configuration | JSON | - | Test data management |"
    line: 46
    origin: input

domain_terms:
  - term: "Page Object Model (POM)"
    definition_or_usage: "Design pattern adopted for maintainable test code and separation of concerns"
    line: 14
    origin: input

standards:
  - area: test
    rule: "Adopt Playwright as the primary test automation framework (ADR-001)"
    line: 10
    origin: input
  - area: arch
    rule: "Adopt Page Object Model (POM) pattern for maintainable test code and separation of concerns (ADR-002)"
    line: 18
    origin: input
  - area: test
    rule: "Use JSON configuration files for test data to support multi-language testing (ADR-003)"
    line: 26
    origin: input
  - area: test
    rule: "Implement Playwright HTML Reporter with screenshot capture on failures (ADR-004)"
    line: 34
    origin: input

constraints:
  - constraint: "Test data management (JSON config strategy) carries Medium risk and requires careful data maintenance"
    source: "Impact Matrix - ADR-003 row"
    line: 54
    origin: input

infra:
  - item: "Build Tool"
    detail: "npm 10.0+ used for package management"
    line: 43
    origin: input

open_questions:
  - question: "No CI/CD, environment/deployment, or hosting infrastructure details are stated in this ADR file beyond the build tool (npm) — is that covered in another source document?"
    why: "The Technology Stack Matrix only lists npm as a build tool; no pipeline, environment, or hosting infra is documented here."
  - question: "No explicit coding style/lint standards, security standards, or automation (CI trigger) standards are stated — are these covered elsewhere?"
    why: "The ADR catalog only covers framework, POM, test data, and reporting decisions (ADR-001 to ADR-004); no security or CI automation ADRs are present."
