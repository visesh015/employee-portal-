# Integration Controllers

## 1. What Is This Folder?

FastAPI route controllers exposing frontend-facing endpoints for composite workflows.

## 2. Why Do We Need It?

It provides clean RESTful entry points tailored specifically to portal UI views (dashboard, employee portal, admin portal, HR portal).

## 3. Main Responsibility

Accepting frontend requests, enforcing route authentication and role permissions, and delegating to integration services.

## 4. How It Works

Controllers define router endpoints with FastAPI decorators. They declare query parameters, route paths, and response schemas, delegating all aggregation work to integration services and returning composite response schemas.

## 5. What This Folder Contains

| Item                    | Type | Purpose                                                                          | Required/Optional |
|-------------------------|------|----------------------------------------------------------------------------------|-------------------|
| __init__.py             | File | Package initialization marker                                                    | Required          |
| admin_controller.py     | File | Endpoints for organization-wide administration and audit overviews               | Required          |
| dashboard_controller.py | File | Endpoints for composite employee and manager dashboard cards                     | Required          |
| employee_controller.py  | File | Composite endpoints for unified employee profile and status views                | Required          |
| hr_controller.py        | File | Composite endpoints for HR management, attendance overviews, and leave approvals | Required          |

## 6. File Responsibilities

| File                    | What It Does                                                                             | What It Contains           | Input                            | Output                    | Called By      | Calls                      |
|-------------------------|------------------------------------------------------------------------------------------|----------------------------|----------------------------------|---------------------------|----------------|----------------------------|
| __init__.py             | Initializes the integration controllers package.                                         | Empty package marker.      | None                             | None                      | Python runtime | None                       |
| admin_controller.py     | Handles portal administration composite routes (Placeholder — Implementation Pending).   | FastAPI route definitions. | HTTP Request, Admin query params | Admin composite response  | FastAPI router | AdminIntegrationService    |
| dashboard_controller.py | Handles employee and manager dashboard routes (Placeholder — Implementation Pending).    | FastAPI route definitions. | HTTP Request, Auth token         | DashboardResponse schema  | FastAPI router | DashboardService           |
| employee_controller.py  | Handles unified employee profile composite views (Placeholder — Implementation Pending). | FastAPI route definitions. | HTTP Request, Employee ID        | EmployeeCompositeResponse | FastAPI router | EmployeeIntegrationService |
| hr_controller.py        | Handles HR portal overview composite routes (Placeholder — Implementation Pending).      | FastAPI route definitions. | HTTP Request, HR query params    | HRCompositeResponse       | FastAPI router | HRIntegrationService       |

## 7. Allowed Responsibilities

- Handling frontend HTTP routes, status codes, and headers.
- Declaring role requirements via FastAPI dependencies.
- Returning serialized composite schemas.

## 8. Boundaries

- Must not contain business aggregation logic (delegated to services).
- Must not query databases or execute SQL.
- Must not invoke downstream module repositories or models.

## 9. Connection With Other Layers

- Called by: Frontend via FastAPI routing.
- Calls: Integration services (`backend/integration/services`).
- Allowed: Delegating to integration services.
- Prohibited: Direct calls to downstream module repositories.

## 10. Data Flow

Enters: Frontend HTTP requests and tokens. Leaves: HTTP responses with composite JSON envelopes.

## 11. Validation Responsibility

Validates incoming query and path parameter types via Pydantic.

## 12. Authentication and Authorization

Enforces user role authorization (e.g. `Role.ADMIN`, `Role.HR`, `Role.EMPLOYEE`) via FastAPI dependencies.

## 13. Error Handling

Allows domain and gateway exceptions to be caught by global exception handlers.

## 14. Transaction Responsibility

Must not manage transactions.

## 15. Logging and Auditing

Technical access logging handled by middleware.

## 16. Naming Convention

Files: `<role_or_feature>_controller.py`. Classes: none (FastAPI router functions).

## 17. When to Add a New File

When introducing a new major UI portal persona or composite dashboard section.

## 18. Common Mistakes

Putting aggregation logic or multiple downstream client calls directly inside controller functions.

## 19. Related Folders

- [Integration Gateway Root](../README.md)
- [Integration Services](../services/README.md)
- [Integration Schemas](../schemas/README.md)

## 20. Review Checklist

- [ ] Are all controller functions thin delegators to services?
- [ ] Are role requirements explicitly declared via dependencies?
