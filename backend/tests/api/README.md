# API Tests Root

## 1. What Is This Folder?

The `api` testing package. HTTP endpoint test suite verifying controllers, routing, request validation, and HTTP status codes.

## 2. Why Do We Need It?

It ensures high code quality, robust regression coverage, and architectural compliance for API Tests Root.

## 3. Main Responsibility

Testing HTTP status codes (200, 201, 400, 401, 403, 404, 422), headers, and error response envelopes.

## 4. How It Works

Tests in this folder execute within the Pytest test runner framework. They target specific system boundaries using appropriate dependencies: FastAPI `TestClient` (HTTPX) with mocked service dependencies.

## 5. What This Folder Contains

| Item        | Type   | Purpose                                        | Required/Optional |
|-------------|--------|------------------------------------------------|-------------------|
| attendance  | Folder | API endpoint tests for the Attendance module   | Required          |
| employees   | Folder | API endpoint tests for the Employees module    | Required          |
| holidays    | Folder | API endpoint tests for the Holidays module     | Required          |
| integration | Folder | API endpoint tests for the Integration Gateway | Required          |
| leave       | Folder | API endpoint tests for the Leave module        | Required          |
| wfh         | Folder | API endpoint tests for the WFH module          | Required          |

## 6. File Responsibilities

| File | What It Does | What It Contains | Input | Output | Called By | Calls |
|------|--------------|------------------|-------|--------|-----------|-------|

## 7. Allowed Responsibilities

Test cases, assertions, and test fixtures tailored to API Tests Root.

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

When adding automated tests for API Tests Root.

## 18. Common Mistakes

Mixing test boundaries (e.g. running slow database queries in unit test folders).

## 19. Related Folders

- [Tests Root](../README.md)
- [Backend Root](../../README.md)

## 20. Review Checklist

- [ ] Do tests in this folder strictly respect the defined testing scope?
- [ ] Are all tests isolated and deterministic?
