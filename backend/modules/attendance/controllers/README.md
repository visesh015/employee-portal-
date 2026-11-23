# Attendance Controllers

## 1. What Is This Folder?

The controllers layer contains FastAPI route handlers responsible for accepting HTTP requests for attendance actions.

## 2. Why Do We Need It?

It provides a clear separation between HTTP protocol concerns (request parsing, status codes, routing) and business logic.

## 3. Main Responsibility

Receiving HTTP requests, validating request bodies via Pydantic schemas, invoking service methods, and returning HTTP responses.

## 4. How It Works

Controllers define router endpoints using FastAPI decorators. They declare parameters for path variables, query strings, request bodies (Pydantic schemas), and dependencies (current user, DB session, service instances). Controllers delegate all processing to services and return structured response schemas with appropriate HTTP status codes (200, 201, 204).

## 5. What This Folder Contains

| Item                         | Type | Purpose                                                                     | Required/Optional |
|------------------------------|------|-----------------------------------------------------------------------------|-------------------|
| __init__.py                  | File | Package initialization marker                                               | Required          |
| clock_controller.py          | File | HTTP endpoints for clock-in, clock-out, and active punch status             | Required          |
| regularization_controller.py | File | HTTP endpoints for submitting, reviewing, and approving regularizations     | Required          |
| report_controller.py         | File | HTTP endpoints for generating daily, weekly, and monthly attendance reports | Required          |

## 6. File Responsibilities

| File                         | What It Does                                                    | What It Contains                                 | Input                                      | Output                                      | Called By                | Calls                 |
|------------------------------|-----------------------------------------------------------------|--------------------------------------------------|--------------------------------------------|---------------------------------------------|--------------------------|-----------------------|
| __init__.py                  | Initializes the controllers package.                            | Empty package marker.                            | None                                       | None                                        | Python runtime           | None                  |
| clock_controller.py          | Handles clock-in, clock-out, and status endpoints.              | FastAPI router and endpoint handler definitions. | HTTP Request, ClockInRequest schema        | SuccessResponse with ClockResponse schema   | FastAPI Router / Clients | ClockService          |
| regularization_controller.py | Handles regularization request creation and approval endpoints. | FastAPI router and endpoint handler definitions. | HTTP Request, RegularizationRequest schema | SuccessResponse with RegularizationResponse | FastAPI Router / Clients | RegularizationService |
| report_controller.py         | Handles attendance report generation and summary endpoints.     | FastAPI router and endpoint handler definitions. | HTTP Request, DateRange query params       | SuccessResponse with ReportResponse         | FastAPI Router / Clients | ReportService         |

## 7. Allowed Responsibilities

- Extracting HTTP headers, query params, path variables, and JSON bodies.
- Invoking service methods.
- Returning Pydantic response models and setting HTTP status codes.

## 8. Boundaries

- Must not execute database queries or construct SQL.
- Must not contain business rules or validation calculations.
- Must not return raw SQLAlchemy ORM models to callers.

## 9. Connection With Other Layers

- Called by: FastAPI routing engine / Integration gateway.
- Calls: Attendance services (`backend/modules/attendance/services`).
- Allowed: Invocations to services and dependency injection providers.
- Prohibited: Direct calls to repositories, models, or foreign module controllers.

## 10. Data Flow

Enters: HTTP requests, JSON payloads, authentication tokens, query parameters.
Leaves: HTTP status codes (200 OK, 201 Created), serialized JSON response envelopes.

## 11. Validation Responsibility

Format and type validation only, handled automatically via Pydantic request models in schemas.

## 12. Authentication and Authorization

Enforces authentication and role verification (e.g. Employee vs HR/Manager) via FastAPI `Depends()`.

## 13. Error Handling

Does not catch business exceptions; permits exceptions to bubble up to shared global exception handlers.

## 14. Transaction Responsibility

Does not manage transactions. Sessions are provided by dependencies and committed/rolled back by services.

## 15. Logging and Auditing

Technical access logging occurs via middleware; controllers do not write business audit logs directly.

## 16. Naming Convention

Files: `<feature>_controller.py`. Handlers: `get_<resource>`, `create_<resource>`, `update_<resource>`.

## 17. When to Add a New File

When a new attendance API endpoint group is approved (e.g. `biometric_controller.py`).

## 18. Common Mistakes

- Writing business logic or SQL inside controller endpoints.
- Manually handling try/except and creating inconsistent error dictionaries.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Services](../services/README.md)
- [Attendance Schemas](../schemas/README.md)
- [Attendance Dependencies](../README.md)

## 20. Review Checklist

- [ ] Are all route handlers thin and delegating immediately to services?
- [ ] Are response schemas explicitly declared in route decorators?
- [ ] Is there zero direct database query code?
