# Unit Tests Root

## 1. What Is This Folder?

The `unit` testing package. Isolated unit test suite verifying domain services, business logic, and helper algorithms.

## 2. Why Do We Need It?

It ensures high code quality, robust regression coverage, and architectural compliance for Unit Tests Root.

## 3. Main Responsibility

Testing business rules in isolation: punch validation, overtime calculations, grace period logic, and state transitions.

## 4. How It Works

Tests in this folder execute within the Pytest test runner framework. They target specific system boundaries using appropriate dependencies: Mocked repositories and mocked clients using `unittest.mock` (no database or network access).

## 5. What This Folder Contains

| Item       | Type   | Purpose                                        | Required/Optional |
|------------|--------|------------------------------------------------|-------------------|
| attendance | Folder | Unit tests for Attendance services and helpers | Required          |
| employees  | Folder | Unit tests for Employees services and helpers  | Required          |
| holidays   | Folder | Unit tests for Holidays services and helpers   | Required          |
| leave      | Folder | Unit tests for Leave services and helpers      | Required          |
| wfh        | Folder | Unit tests for WFH services and helpers        | Required          |

## 6. File Responsibilities

| File | What It Does | What It Contains | Input | Output | Called By | Calls |
|------|--------------|------------------|-------|--------|-----------|-------|

## 7. Allowed Responsibilities

Test cases, assertions, and test fixtures tailored to Unit Tests Root.

## 8. Boundaries

Must not be executed against production environments. Must not commit test secrets.

## 9. Connection With Other Layers

- Invoked by: Pytest.
- Targets: Relevant application layers.

## 10. Data Flow

Enters: Test inputs and fixtures. Leaves: Test assertion results.

## 11. Validation Responsibility

Validates system behavior against functional requirements.

## 12. Authentication and Authorization

Uses mock authorization contexts and test user principals.

## 13. Error Handling

Assertions detect behavioral regressions.

## 14. Transaction Responsibility

Participates in test transaction rollbacks where database interaction is involved.

## 15. Logging and Auditing

Test runner captures log streams.

## 16. Naming Convention

Files: `test_<feature>.py`. Functions: `test_<behavior>_<expected>`.

## 17. When to Add a New File

When adding automated tests for Unit Tests Root.

## 18. Common Mistakes

Mixing test boundaries (e.g. running slow database queries in unit test folders).

## 19. Related Folders

- [Tests Root](../README.md)
- [Backend Root](../../README.md)

## 20. Review Checklist

- [ ] Do tests in this folder strictly respect the defined testing scope?
- [ ] Are all tests isolated and deterministic?
