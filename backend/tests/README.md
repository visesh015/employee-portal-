# Tests Root

## 1. What Is This Folder?

The centralized automated test suite for the Employee Management Portal backend.

## 2. Why Do We Need It?

It provides comprehensive quality assurance, regression prevention, and validation of business rules and API contracts.

## 3. Main Responsibility

Housing all test suites categorized by test level: unit, api, integration, contract, and end-to-end.

## 4. How It Works

Tests are organized into five distinct directories by testing scope:
- `unit/`: Fast, isolated tests for services and helpers with mocked dependencies.
- `api/`: Black-box HTTP tests verifying controller routes, status codes, and schemas.
- `integration/`: Multi-component tests verifying service + repository + real database interaction.
- `contract/`: Verification of client-provider schema contracts between modules.
- `end_to_end/`: Complete user workflows through the Integration gateway.

## 5. What This Folder Contains

| Item        | Type   | Purpose                                                                         | Required/Optional |
|-------------|--------|---------------------------------------------------------------------------------|-------------------|
| api         | Folder | HTTP-level endpoint tests covering status codes and request/response validation | Required          |
| contract    | Folder | Consumer-driven contract tests for inter-module HTTP client interfaces          | Required          |
| end_to_end  | Folder | Cross-service user journey tests through the Integration Gateway                | Required          |
| integration | Folder | Multi-component tests using real database sessions                              | Required          |
| unit        | Folder | Isolated business logic and calculation tests with mocked repositories          | Required          |

## 6. File Responsibilities

| File | What It Does | What It Contains | Input | Output | Called By | Calls |
|------|--------------|------------------|-------|--------|-----------|-------|

## 7. Allowed Responsibilities

Test cases, fixtures, mock factories, and test assertion helpers.

## 8. Boundaries

- Must not be imported by production application code.
- Must not execute destructive actions against shared or production databases.
- Must not include committed credentials or API keys.

## 9. Connection With Other Layers

- Called by: Pytest runner / CI/CD pipelines.
- Calls: Backend modules, integration, and shared code.
- Prohibited: Application code importing test code.

## 10. Data Flow

Enters: Test fixtures and mock inputs. Leaves: Assertions, test reports, and code coverage metrics.

## 11. Validation Responsibility

Validates system behavior against architectural and business specifications.

## 12. Authentication and Authorization

Uses mock authentication tokens and test user fixtures.

## 13. Error Handling

Pytest captures assertions and unexpected exceptions, generating detailed failure reports.

## 14. Transaction Responsibility

Tests typically run inside rolled-back database transactions to ensure test isolation.

## 15. Logging and Auditing

Test runner captures standard output and log streams for diagnostics.

## 16. Naming Convention

Files: `test_<feature>.py`. Functions: `test_<behavior>_<expected_result>`.

## 17. When to Add a New File

When adding new test levels or shared pytest plugins.

## 18. Common Mistakes

- Conflating unit tests with integration tests by querying real databases in unit tests.
- Writing tests that depend on state left behind by other tests.

## 19. Related Folders

- [Backend Root](../README.md)
- [Unit Tests](unit/README.md)
- [API Tests](api/README.md)
- [Integration Tests](integration/README.md)
- [Contract Tests](contract/README.md)
- [End-to-End Tests](end_to_end/README.md)

## 20. Review Checklist

- [ ] Are test suites strictly segregated by testing boundary?
- [ ] Are all tests independent and repeatable?
