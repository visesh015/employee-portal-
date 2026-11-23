# Attendance Module Root

## 1. What Is This Folder?

The Attendance module is the core domain service responsible for employee timekeeping, clock events, attendance regularization, shift adherence tracking, and timesheet reporting. It acts as the canonical reference module for the entire Employee Management Portal backend.

## 2. Why Do We Need It?

Accurate timekeeping is critical for enterprise operations, payroll calculation, labor law compliance, and employee accountability. Isolating attendance into a standalone module guarantees that timekeeping business rules remain decoupled from employee profile management and leave workflows.

## 3. Main Responsibility

Encapsulating all business rules, database tables, and API contracts related to employee clock punches, shift matching, work hour calculations, attendance regularization approvals, and attendance reporting.

## 4. How It Works

The Attendance module operates as a self-contained FastAPI sub-application. Incoming HTTP requests pass through shared middleware for context and correlation ID assignment, enter module route handlers defined in `controllers`, resolve dependencies defined in `dependencies.py`, validate payload schemas defined in `schemas`, execute business logic and shift checks in `services`, communicate with external modules (Employees, Leave) via `clients`, and persist data to dedicated tables via `repositories` and `models`. Domain exceptions defined in `exceptions.py` are raised on validation or business rule failures and translated into standardized HTTP responses.

## 5. What This Folder Contains

| Item            | Type   | Purpose                                                                           | Required/Optional |
|-----------------|--------|-----------------------------------------------------------------------------------|-------------------|
| __init__.py     | File   | Package initialization marker                                                     | Required          |
| dependencies.py | File   | FastAPI dependency injection providers (db session, user context, clients)        | Required          |
| exceptions.py   | File   | Attendance domain-specific exception definitions                                  | Required          |
| main.py         | File   | Module router definition, sub-app setup, and route registration                   | Required          |
| clients         | Folder | Downstream internal HTTP clients for calling Employees, Leave, and Shift APIs     | Required          |
| controllers     | Folder | FastAPI route controllers receiving HTTP requests for clocking and regularization | Required          |
| helpers         | Folder | Stateless domain calculation utilities (hours, grace periods, penalty windows)    | Required          |
| models          | Folder | SQLAlchemy ORM models defining the attendance and regularization database tables  | Required          |
| repositories    | Folder | Data access layer executing PostgreSQL queries via SQLAlchemy                     | Required          |
| schemas         | Folder | Pydantic request and response schemas for serialization and validation            | Required          |
| services        | Folder | Business logic layer enforcing attendance rules, validation, and workflow state   | Required          |

## 6. File Responsibilities

| File            | What It Does                                                         | What It Contains                         | Input                 | Output                   | Called By                          | Calls                        |
|-----------------|----------------------------------------------------------------------|------------------------------------------|-----------------------|--------------------------|------------------------------------|------------------------------|
| __init__.py     | Marks the attendance directory as a Python package.                  | Package marker.                          | None                  | None                     | Python runtime                     | None                         |
| dependencies.py | Provides FastAPI dependencies for route handlers.                    | Session and client dependency factories. | HTTP Request          | Session/Client instances | Attendance controllers             | Shared database and security |
| exceptions.py   | Defines custom domain exceptions for attendance rules.               | Domain exception classes.                | Error context         | Exception instances      | Services and repositories          | Shared base exceptions       |
| main.py         | Initializes the module router and mounts all attendance controllers. | FastAPI APIRouter registration.          | Application bootstrap | APIRouter instance       | Backend main / Integration gateway | Attendance controllers       |

## 7. Allowed Responsibilities

- Defining Attendance domain business logic, data models, schemas, and endpoints.
- Providing dependency injection helpers tailored to Attendance operations.
- Raising Attendance-specific domain exceptions on rule violations.

## 8. Boundaries

- Must not directly query or modify tables owned by other modules (Employees, Leave).
- Must not import ORM models or repositories from external modules.
- Must not parse or generate frontend-specific composite dashboard payloads (owned by Integration).

## 9. Connection With Other Layers

- Called by: Integration Gateway (`backend/integration`) and frontend routing.
- Calls: External module APIs via internal `clients`; shared utilities (`backend/shared`).
- Allowed: Downstream calls to Employees and Leave modules via clients.
- Prohibited: Cross-module direct database imports; circular calls back to Integration.

## 10. Data Flow

Information entering: Clock-in/out payloads, date ranges, regularization requests, authentication tokens, employee IDs.
Information leaving: Punch confirmation records, attendance history summaries, regularization approval statuses.

## 11. Validation Responsibility

Two-stage validation:
1. Request format validation: Enforced by Pydantic schemas in `schemas/requests.py`.
2. Business validation: Enforced by service layer in `services/clock_service.py` (e.g. shift timing, duplicate punch checks).

## 12. Authentication and Authorization

Authentication is verified via shared security dependencies (`get_current_user`). Authorization is verified at the controller level (role checks like Employee vs Manager) and service level (verifying caller owns the punch record).

## 13. Error Handling

Exceptions originate in repositories (db errors) or services (rule violations). They are mapped to custom exceptions in `exceptions.py` and converted to standard error envelopes (`ErrorResponse`) by shared exception handlers.

## 14. Transaction Responsibility

Transactions are started per HTTP request via the database session dependency and committed or rolled back in the repository or service boundaries. Requires Team Confirmation for cross-operation atomic commitments.

## 15. Logging and Auditing

- Technical logging: Tracing punch latency, client timeouts, and DB queries via shared logger.
- Business auditing: Recording punch modifications, regularization approvals, and manager overrides via shared audit service.

## 16. Naming Convention

- Files: `snake_case.py` (e.g., `clock_controller.py`).
- Classes: `PascalCase` (e.g., `ClockService`, `AttendanceRepository`).
- Functions: `snake_case` (e.g., `process_clock_in`, `calculate_overtime`).

## 17. When to Add a New File

When introducing new attendance sub-domains such as biometric sync, geofencing rules, or overtime policies.

## 18. Common Mistakes

- Placing business logic inside controllers.
- Directly querying the employee database table instead of calling `employee_client.py`.
- Returning raw SQLAlchemy models instead of Pydantic response schemas.

## 19. Related Folders

- [Modules Root](../README.md)
- [Controllers](controllers/README.md)
- [Services](services/README.md)
- [Repositories](repositories/README.md)
- [Models](models/README.md)
- [Schemas](schemas/README.md)
- [Clients](clients/README.md)
- [Helpers](helpers/README.md)
- [Integration Gateway](../../integration/README.md)

## 20. Review Checklist

- [ ] Does the module rely exclusively on its own models?
- [ ] Are all cross-module interactions handled via clients?
- [ ] Are all responses serialized using Pydantic schemas?
