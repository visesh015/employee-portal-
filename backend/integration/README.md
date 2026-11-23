# Integration Gateway Root

## 1. What Is This Folder?

The `integration` layer acts as the Backend-for-Frontend (BFF) and API Gateway. It is the sole frontend-facing entry point for composite queries, cross-module aggregations, and portal dashboards.

## 2. Why Do We Need It?

Frontend clients often require composite views (such as employee dashboards combining attendance, leave balances, and upcoming holidays) that span multiple business domains. The Integration layer orchestrates these downstream calls, preventing frontend complexity and shielding internal module boundaries.

## 3. Main Responsibility

Routing frontend requests, orchestrating concurrent downstream module calls, aggregating responses into optimized composite DTOs, propagating authentication context, and translating downstream failures into safe frontend responses.

## 4. How It Works

When a request arrives from the frontend:
1. Common middleware attaches request IDs and measures latency.
2. Integration dependency providers authenticate and authorize the user token.
3. An Integration controller receives the request and validates query parameters.
4. The corresponding Integration service determines which downstream modules must be contacted.
5. Integration HTTP clients issue parallel calls to module APIs (Attendance, Employees, Leave, Holidays, WFH).
6. Downstream modules process requests within their bounded contexts and return domain responses.
7. The Integration service aggregates, filters, and formats the partial results.
8. The controller serializes the composite DTO into a frontend response schema.

Step-by-step Dashboard Aggregation Flow:
1. Frontend requests `/api/v1/dashboard/me`.
2. Integration authenticates caller (extracts employee_id and tenant context).
3. Dashboard controller invokes `DashboardService.get_employee_dashboard(employee_id)`.
4. Dashboard service initiates concurrent calls via clients: AttendanceClient (today's punch), LeaveClient (leave balance), HolidayClient (upcoming holidays).
5. Each downstream client queries its respective module's internal API.
6. Clients deserialize responses and return typed DTOs to the service.
7. Dashboard service combines approved data fields into a single `DashboardResponse`.
8. Integration returns HTTP 200 with the aggregated payload to the frontend.

## 5. What This Folder Contains

| Item            | Type   | Purpose                                                                          | Required/Optional |
|-----------------|--------|----------------------------------------------------------------------------------|-------------------|
| __init__.py     | File   | Package initialization marker                                                    | Required          |
| dependencies.py | File   | FastAPI dependencies for authentication, context, and client injection           | Required          |
| exceptions.py   | File   | Custom exceptions for gateway timeouts, aggregation failures, and routing errors | Required          |
| main.py         | File   | FastAPI application entrypoint for the integration gateway router                | Required          |
| clients         | Folder | HTTP client interfaces for calling internal domain modules                       | Required          |
| controllers     | Folder | Frontend-facing route controllers (Dashboard, Admin, HR, Employee)               | Required          |
| schemas         | Folder | Pydantic composite request and response schemas                                  | Required          |
| services        | Folder | Orchestration and composite business aggregation logic                           | Required          |

## 6. File Responsibilities

| File            | What It Does                                                                                 | What It Contains                   | Input                 | Output                | Called By               | Calls                   |
|-----------------|----------------------------------------------------------------------------------------------|------------------------------------|-----------------------|-----------------------|-------------------------|-------------------------|
| __init__.py     | Initializes the integration package.                                                         | Empty package marker.              | None                  | None                  | Python runtime          | None                    |
| dependencies.py | Provides dependency injection for integration routes (Placeholder — Implementation Pending). | Session and security dependencies. | HTTP Request          | User context, clients | Integration controllers | Shared Security         |
| exceptions.py   | Defines gateway and aggregation exceptions (Placeholder — Implementation Pending).           | Exception classes.                 | Error details         | Exception instances   | Integration services    | Shared base exceptions  |
| main.py         | Configures and mounts integration route controllers (Placeholder — Implementation Pending).  | FastAPI APIRouter setup.           | Application bootstrap | APIRouter instance    | Backend main            | Integration controllers |

## 7. Allowed Responsibilities

- Exposing frontend-optimized composite endpoints.
- Orchestrating calls to multiple internal domain modules.
- Combining, sorting, and transforming domain responses into UI-ready DTOs.

## 8. Boundaries

- Must not connect directly to PostgreSQL or execute database queries.
- Must not import module repositories or SQLAlchemy models.
- Must not own core business rules (business rules belong inside domain modules).
- Must not become a bloated god-service containing all application logic.

## 9. Connection With Other Layers

- Called by: Frontend application via HTTP/HTTPS.
- Calls: Domain modules (Attendance, Employees, Leave, Holidays, WFH) via internal clients; shared security and middleware.
- Allowed: Outbound HTTP calls to module APIs.
- Prohibited: Direct database access; circular calls.

## 10. Data Flow

Enters: Frontend HTTP requests, cookies/tokens, query parameters.
Leaves: Frontend JSON responses, composite views, dashboard metrics.

## 11. Validation Responsibility

Validates incoming frontend request schemas and verifies downstream responses conform to expected contract schemas.

## 12. Authentication and Authorization

Enforces user authentication, role checks (Employee vs HR vs Admin), and organization tenancy at the gateway boundary.

## 13. Error Handling

Translates downstream HTTP failures, timeouts, and connection errors into safe gateway responses. Never exposes internal downstream stack traces or database errors.

## 14. Transaction Responsibility

Must not manage transactions. The Integration layer has no database session.

## 15. Logging and Auditing

Technical access logging of incoming frontend requests and downstream client call latency.

## 16. Naming Convention

Files: `snake_case.py`. Classes: `PascalCase`. Endpoints: `/api/v1/<resource>`.

## 17. When to Add a New File

When adding new composite aggregation views required by frontend UI screens.

## 18. Common Mistakes

- Importing module database models directly into integration code.
- Implementing domain business logic inside integration services instead of delegating to modules.

## 19. Related Folders

- [Backend Root](../README.md)
- [Integration Clients](clients/README.md)
- [Integration Controllers](controllers/README.md)
- [Integration Services](services/README.md)
- [Integration Schemas](schemas/README.md)
- [Attendance Module](../modules/attendance/README.md)

## 20. Review Checklist

- [ ] Is there zero direct database connection or query code?
- [ ] Are all downstream calls routed through clients?
- [ ] Are appropriate timeouts configured for all concurrent downstream calls?
