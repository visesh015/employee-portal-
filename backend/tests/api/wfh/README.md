# API Tests: Wfh

## 1. What Is This Folder?

Black-box HTTP API tests for the Wfh module routes.

## 2. Why Do We Need It?

It validates that Wfh HTTP endpoints correctly adhere to status codes, request schemas, and error contracts.

## 3. Main Responsibility

Testing HTTP requests against Wfh endpoints using FastAPI TestClient.

## 4. How It Works

API tests invoke endpoints via HTTPX `TestClient`. They pass valid and invalid payloads, testing 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, and 422 Validation Error status responses.

## 5. What This Folder Contains

| Item     | Type | Purpose                            | Required/Optional |
|----------|------|------------------------------------|-------------------|
| .gitkeep | File | Directory preservation placeholder | Required          |

## 6. File Responsibilities

| File     | What It Does                                              | What It Contains        | Input | Output | Called By | Calls |
|----------|-----------------------------------------------------------|-------------------------|-------|--------|-----------|-------|
| .gitkeep | Preserves the wfh API tests directory in version control. | Empty placeholder file. | None  | None   | Git VCS   | None  |

## 7. Allowed Responsibilities

FastAPI route test functions for wfh.

## 8. Boundaries

Must not test internal repository SQL directly (delegated to integration tests).

## 9. Connection With Other Layers

- Targets: Wfh controllers.

## 10. Data Flow

HTTP requests → Controller → Mocked Service → HTTP Response.

## 11. Validation Responsibility

Validates HTTP request and response schema formats.

## 12. Authentication and Authorization

Tests authentication headers and role authorization checks.

## 13. Error Handling

Tests that exceptions are correctly converted to standard `ErrorResponse` envelopes.

## 14. Transaction Responsibility

No database transaction responsibility.

## 15. Logging and Auditing

Captured by pytest.

## 16. Naming Convention

Files: `test_<feature>_api.py`.

## 17. When to Add a New File

When adding or modifying API endpoints in wfh.

## 18. Common Mistakes

Testing internal service algorithms in API tests instead of focusing on HTTP status codes and contracts.

## 19. Related Folders

- [API Tests Root](../README.md)
- [Wfh Module](../../../modules/wfh/README.md)

## 20. Review Checklist

- [ ] Are all standard HTTP status codes (2xx, 4xx, 5xx) covered for each route?
- [ ] Are authentication and role-based permissions tested?
