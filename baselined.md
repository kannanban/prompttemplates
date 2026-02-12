Phase 0: Repository Baseline (Run once per repo)
0.1 Repo Orientation (what is this system?)
Role: You are a senior Java maintainer onboarding to this repository.
Task: Identify what this system is, how it is structured, and how it runs.
Constraints:
Do not refactor or change code
Only observe and summarize
Input: Repository source code (already loaded)
Output:
System purpose (best inference)
Repo structure (modules/services)
Tech stack (Spring Boot, frameworks, build tool)
How to run locally (from docs or inference)
How tests are organized
External dependencies (DB, Kafka, external services)
Missing docs / gaps [Unclear]

0.2 Build & Run Baseline (safe commands)
Role: You are a maintainer preparing the repo for local execution.
Task: Identify the correct build + test commands and validate the repo builds.
Constraints:
Do not run destructive commands
Ask for approval before running terminal commands
Input: Repository
Output:
Exact commands to build
Commands to run unit tests
Commands to run integration tests
Expected artifacts (jar/docker image)
Known build issues and how to fix

0.3 Architecture & Layering Baseline (brownfield rules)
Role: You are a staff engineer extracting architecture rules from the codebase.
Task: Infer the architectural style and layering rules used in this repo.
Constraints:
Only infer from existing code
Don’t propose improvements
Input: Repository
Output:
Layering model (Controller → Service → Repo etc.)
Package conventions
Dependency direction rules (who can call whom)
DTO mapping approach
Transaction patterns
Error handling approach
Logging + correlation approach
Security/auth patterns

0.4 Domain & Data Baseline
Role: You are a Java maintainer mapping the business domain.
Task: Identify the core domain objects and persistence model.
Constraints:
No schema changes
No refactor
Input: Repository
Output:
Main entities/aggregates
Key tables and relationships (inferred from migrations/entities)
Core workflows (top 5)
Where business rules live
Risky coupling points

0.5 API Baseline (REST contracts)
Role: You are an API maintainer.
Task: Inventory all externally exposed APIs and their contracts.
Constraints:
Don’t change API behavior
Input: Repository
Output:
Endpoint list grouped by module
Request/response DTOs
Validation rules
Error response conventions
Auth requirements
Versioning strategy (if any)

0.6 Integration Baseline
Role: You are an integration owner.
Task: Identify all external integrations and how they’re implemented.
Constraints:
No redesign
Input: Repository
Output:
Outbound calls (REST/SOAP)
Messaging (Kafka/Rabbit) topics/queues
Batch jobs / schedulers
File-based integrations
Retry + circuit breaker patterns
Where configs are stored

0.7 Testing Baseline (how this repo expects tests)
Role: You are the test lead for this codebase.
Task: Extract the repo’s test strategy and conventions.
Constraints:
Follow existing patterns only
Input: Repository
Output:
Unit test framework + style
Integration test framework + style
Test data strategy
Mocking patterns
Coverage expectations (if any)
How to run tests locally + in CI

0.8 CI/CD Baseline
Role: You are a release engineer.
Task: Identify CI/CD pipelines and release flow.
Constraints:
Don’t change pipelines
Input: Repository
Output:
CI tool (GitHub Actions/Jenkins/GitLab)
Build stages
Quality gates
Deployment strategy
Environment configs
Rollback strategy (if documented)

Phase 0 Output: “Repo Baseline Document”
After you run the above, you should run this:
0.9 Consolidate Baseline into a Single Maintenance Guide
Role: You are a tech lead creating an onboarding + maintenance guide.
Task: Consolidate findings from 0.1–0.8 into a single baseline document.
Constraints:
Must be concise but complete
Must highlight constraints and golden paths
Input: Outputs from baseline steps
Output:
Repo overview
Architecture rules
Golden path examples
Do-not-touch zones
Standard dev workflow (build/test/run)
Change implementation checklist
0.1 Baseline from Business Ticket
Role: Senior maintainer for a production Java app
Task: From this ticket, locate where the current behavior is implemented and summarize the as-is flow.
Constraints:
Do not redesign or refactor
Follow existing patterns
If multiple candidates exist, rank confidence
Input: <paste business ticket>
Output:
Likely entry points (Controller, Scheduler, Listener, Batch job)
Key classes involved
Current flow summary
Dependencies (DB, Kafka, external APIs)
Unknowns [Unclear]

1) Requirement Clarification
1.1 Convert Ticket into Implementable Scope
Role: BA + Tech Lead
Task: Convert the ticket into acceptance criteria + edge cases suitable for implementation.
Constraints:
Preserve backward compatibility unless stated
Keep scope minimal
Input: <ticket>
Output:
Interpreted requirement
Assumptions
Acceptance criteria
Edge cases
Non-goals
Test expectations

2) Impact Analysis (core for maintenance)
2.1 Impact Analysis (Java + DB + Integrations)
Role: Maintenance lead
Task: Identify all impacted layers and ripple effects for implementing the requirement.
Constraints:
Minimal diff
No refactor
Preserve existing architecture
Input: <ticket + baseline summary if available>
Output:
Layers impacted (Controller/Service/Repo/DB/Integration)
Candidate files/classes to change
Contract impacts (API payloads, status codes)
DB impacts (schema, queries, migration needs)
Integration impacts (Kafka topics, downstream APIs)
Performance risks
Security risks
Regression risks + tests

3) Existing Design Rule Extraction (prevents “inventing architecture”)
3.1 Extract Repo Conventions
Role: Staff engineer capturing implicit design rules
Task: Infer conventions used in this codebase for implementing similar features.
Constraints:
Only infer from existing code
Don’t propose improvements
Input: <ticket + identified module>
Output:
Layering style and package structure
DTO patterns
Validation patterns
Exception handling patterns
Logging patterns (MDC, correlation IDs)
Transaction patterns
Repository/query patterns
Testing patterns (JUnit5/Mockito/Testcontainers)
“Golden files” to follow

4) Minimal Design Proposal
4.1 Brownfield Design Proposal
Role: Senior engineer
Task: Propose the minimal design change that fits the existing system.
Constraints:
Follow conventions from 3.1
Backward compatible
Avoid touching unrelated code
Input:
Ticket
Impact analysis
Conventions
Output:
Proposed approach
Changes per layer
API contract changes (if any)
DB change strategy (if any)
Rollout approach (config/feature flag if needed)
Test plan

5) Implementation Plan (PR-sized)
5.1 PR Breakdown Plan
Role: Tech lead
Task: Break the design into PR-sized tasks.
Constraints:
Each PR independently testable
No mixed refactor + feature
Input: <design>
Output:
PR list
For each PR: scope, files, tests, risks
Branch names + commit messages

6) Implementation Prompts (safe patch)
6.1 Implement Controller Layer Change
Role: Java maintainer
Task: Implement only the Controller/API layer changes for the requirement.
Constraints:
No service refactor
Preserve existing request/response patterns
Keep diff minimal
Input: <design + PR scope>
Output:
Patch + tests for controller (MockMvc/WebTestClient)
6.2 Implement Service Layer Change
Role: Java maintainer
Task: Implement only the service-layer logic.
Constraints:
Keep existing method signatures unless required
Follow existing transaction patterns
Input: <design + PR scope>
Output:
Patch + unit tests (JUnit5 + Mockito)
6.3 Implement Repository / DB Change
Role: Java maintainer with DB awareness
Task: Implement repository/query changes and DB migration if needed.
Constraints:
Backward compatible migration
Avoid risky table locks
Input: <design>
Output:
Migration script (Flyway/Liquibase)
Repository changes
Integration test (Testcontainers if see pattern)

7) Testing Prompts (maintenance-grade)
7.1 Unit Tests Generator
Role: Test-focused engineer
Task: Add unit tests for the changed logic.
Constraints:
Follow existing test style
Cover negative cases
Input: <diff or changed classes>
Output:
Unit tests + rationale
7.2 Integration Tests Generator
Role: Integration test engineer
Task: Add integration tests covering the end-to-end flow.
Constraints:
Reuse existing test infra
Avoid brittle tests
Input: <design + changed endpoints>
Output:
Integration tests + test data
7.3 Regression Test Checklist
Role: QA-minded engineer
Task: Produce a regression checklist for this ticket.
Constraints:
Focus on likely breakpoints
Input: <impact analysis>
Output:
Manual + automated regression list

8) Reviewer Prompts (very important)
8.1 General PR Review
Role: Senior maintainer reviewer
Task: Review the PR for correctness and brownfield safety.
Constraints:
Minimal diff preferred
No hidden breaking changes
Input: <PR diff + ticket>
Output:
Blockers, risks, missing tests
8.2 Backward Compatibility Review
Role: API stability reviewer
Task: Check for breaking changes in API/DB/integrations.
Constraints:
Default = backward compatible
Input: <PR diff>
Output:
Compatibility verdict + issues
8.3 Test Quality Review
Role: QA reviewer
Task: Review test completeness and regression coverage.
Input: <PR diff>
Output:
Missing cases + recommended tests
8.4 Performance Review
Role: Performance reviewer
Task: Identify any performance regressions (DB calls, loops, N+1).
Input: <PR diff>
Output:
Findings + fixes
8.5 Security Review
Role: Security reviewer
Task: Identify authz/authn, data leakage, injection, sensitive logging.
Input: <PR diff>
Output:
Findings + severity + fix

9) Release / Ops Prompts
9.1 Release Notes + Rollback Plan
Role: On-call owner
Task: Prepare release notes and rollback steps.
Constraints:
Rollback within 15 minutes
DB changes must be safe
Input: <PR diff + deployment approach>
Output:
Release notes, rollback, monitoring
9.2 Production Verification Checklist
Role: Release engineer
Task: Provide post-deploy verification steps.
Input: <ticket + impacted flows>
Output:
Smoke tests + metrics/logs to watch

Bonus: “Strict brownfield mode” prefix (use in every prompt)
Paste this at the top of any Kiro request:
Strict Brownfield Mode:
Do not refactor unrelated code
Do not introduce new frameworks
Keep diff minimal
Preserve backward compatibility
Follow existing patterns in this repo
If uncertain, mark [Unclear] and ask questions

Practical “One Prompt to Rule Them All” (end-to-end)
If you want one standard team prompt:
Role: You are a maintenance lead for a production Java application.
Task: For this ticket, do baseline discovery → impact analysis → minimal design → PR breakdown → implementation plan → test plan → review checklist.
Constraints: Strict Brownfield Mode.
Input: <ticket>
Output:
Baseline summary
Impact analysis
Minimal design
PR plan
Test plan
Reviewer checklist
Release + rollback notes

