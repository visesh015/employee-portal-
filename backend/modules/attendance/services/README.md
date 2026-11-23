# Attendance Services

## 1. What Is This Folder?

The services layer contains the core business logic, workflow orchestration, and domain rules for the Attendance module.

## 2. Why Do We Need It?

It guarantees that business rules (punch validation, shift alignment, regularization approval conditions) are executed consistently regardless of the entry mechanism.

## 3. Main Responsibility

Implementing attendance rules, orchestrating repositories and external clients, enforcing resource authorization, and managing transactional boundaries.

## 4. How It Works

Services receive validated domain data from controllers. They interact with `repositories` to fetch existing records, call external module `clients` to verify employee active status and shift assignments, apply business calculations using `helpers`, enforce state transition rules (e.g., cannot clock in twice without clocking out), trigger database writes, and emit audit logs.

## 5. What This Folder Contains

| Item                      | Type | Purpose                                                         | Required/Optional |
|---------------------------|------|-----------------------------------------------------------------|-------------------|
| __init__.py               | File | Package initialization marker                                   | Required          |
| clock_service.py          | File | Business logic for clock-in, clock-out, and punch integrity     | Required          |
| regularization_service.py | File | Business logic for regularization workflows and approvals       | Required          |
| report_service.py         | File | Business logic for aggregating hours, overtime, and punctuality | Required          |

## 6. File Responsibilities

| File                      | What It Does                                                    | What It Contains                                  | Input                                  | Output                      | Called By                | Calls                                  |
|---------------------------|-----------------------------------------------------------------|---------------------------------------------------|----------------------------------------|-----------------------------|--------------------------|----------------------------------------|
| __init__.py               | Initializes the services package.                               | Empty package marker.                             | None                                   | None                        | Python runtime           | None                                   |
| clock_service.py          | Executes punch logic, shift matching, and duplicate prevention. | ClockService class and business workflow methods. | Employee ID, timestamp, punch metadata | AttendanceRecord entity     | ClockController          | AttendanceRepository, EmployeeClient   |
| regularization_service.py | Processes regularization requests and approval state machines.  | RegularizationService class and approval methods. | Request ID, decision, reviewer ID      | RegularizationRecord entity | RegularizationController | RegularizationRepository, AuditService |
| report_service.py         | Computes attendance statistics and timesheet aggregations.      | ReportService class and calculation methods.      | Employee/Dept ID, date range           | Aggregated report data      | ReportController         | AttendanceRepository, AttendanceHelper |

## 7. Allowed Responsibilities

- Implementing business validation rules and state machine logic.
- Coordinating repositories and client invocations.
- Raising domain exceptions when business constraints are violated.
- Requesting audit log creation for sensitive modifications.

## 8. Boundaries

- Must not define FastAPI routes or inspect raw HTTP requests.
- Must not write raw SQL queries (delegated to repositories).
- Must not access foreign module tables directly.

## 9. Connection With Other Layers

- Called by: Attendance controllers (`backend/modules/attendance/controllers`).
- Calls: Attendance repositories, clients, helpers, and shared audit services.
- Allowed: Orchestrating data access and client calls.
- Prohibited: Calling controllers or returning raw HTTP responses.

## 10. Data Flow

Enters: Validated Pydantic request models, authenticated user context, business parameters.
Leaves: Domain entity objects, calculation results, or raised domain exceptions.

## 11. Validation Responsibility

Handles all domain business validation (e.g. punch interval rules, shift window validity, approval rights).

## 12. Authentication and Authorization

Enforces resource-level authorization (e.g. verifying an employee only regularizes their own records, or a manager approves their direct report).

## 13. Error Handling

Detects invalid state or violated rules and raises explicit domain exceptions defined in `exceptions.py`.

## 14. Transaction Responsibility

Coordinates transactional units of work. Ensures database writes and audit records commit atomically.

## 15. Logging and Auditing

Initiates business audit events for regularization approvals and manual punch adjustments via shared audit services.

## 16. Naming Convention

Files: `<feature>_service.py`. Classes: `<Feature>Service`. Methods: `record_clock_in`, `approve_regularization`.

## 17. When to Add a New File

When a new attendance business workflow or capability is added (e.g. `overtime_service.py`).

## 18. Common Mistakes

- Bypassing the service layer by calling repositories directly from controllers.
- Placing HTTP status codes or FastAPI dependencies inside service classes.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Controllers](../controllers/README.md)
- [Attendance Repositories](../repositories/README.md)
- [Attendance Clients](../clients/README.md)
- [Attendance Helpers](../helpers/README.md)

## 20. Review Checklist

- [ ] Is all domain logic isolated within services?
- [ ] Are business errors raised as domain exceptions?
- [ ] Are cross-module checks handled via clients?
