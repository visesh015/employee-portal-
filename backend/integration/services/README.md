# Integration Services

## 1. What Is This Folder?

The orchestration and aggregation service layer for the Integration Gateway.

## 2. Why Do We Need It?

It houses the logic for fanning out requests to multiple downstream modules, merging responses, handling partial failures, and computing composite view metrics.

## 3. Main Responsibility

Coordinating internal client calls, assembling composite DTOs, and managing degradation when optional downstream services are unavailable.

## 4. How It Works

Integration services receive requests from controllers. They invoke multiple downstream clients concurrently (e.g. using `asyncio.gather`), collect domain responses, handle downstream errors gracefully, filter data according to the user's role and tenancy, and assemble the final composite response schema.

## 5. What This Folder Contains

| Item                            | Type | Purpose                                                                           | Required/Optional |
|---------------------------------|------|-----------------------------------------------------------------------------------|-------------------|
| __init__.py                     | File | Package initialization marker                                                     | Required          |
| admin_integration_service.py    | File | Aggregates organization-wide administrative and compliance metrics                | Required          |
| dashboard_service.py            | File | Aggregates employee and manager dashboard data across Attendance, Leave, Holidays | Required          |
| employee_integration_service.py | File | Aggregates complete employee profile, shift, and status views                     | Required          |

## 6. File Responsibilities

| File                            | What It Does                                                                          | What It Contains                  | Input                       | Output                        | Called By           | Calls                                        |
|---------------------------------|---------------------------------------------------------------------------------------|-----------------------------------|-----------------------------|-------------------------------|---------------------|----------------------------------------------|
| __init__.py                     | Initializes the integration services package.                                         | Empty package marker.             | None                        | None                          | Python runtime      | None                                         |
| admin_integration_service.py    | Orchestrates admin composite queries (Placeholder — Implementation Pending).          | AdminIntegrationService class.    | Admin filters, auth context | Admin composite DTO           | AdminController     | All Module Clients                           |
| dashboard_service.py            | Orchestrates dashboard aggregation (Placeholder — Implementation Pending).            | DashboardService class.           | Employee ID, auth context   | DashboardResponse DTO         | DashboardController | AttendanceClient, LeaveClient, HolidayClient |
| employee_integration_service.py | Orchestrates unified employee profile queries (Placeholder — Implementation Pending). | EmployeeIntegrationService class. | Employee ID, auth context   | EmployeeCompositeResponse DTO | EmployeeController  | EmployeeClient, AttendanceClient             |

## 7. Allowed Responsibilities

- Coordinating concurrent HTTP client requests to internal modules.
- Aggregating, combining, and shaping domain data into composite structures.
- Handling partial degradation (e.g. returning dashboard even if optional holiday service is temporarily slow).

## 8. Boundaries

- Must not connect directly to the database or execute SQL.
- Must not import module repositories or ORM models.
- Must not enforce domain business rules (e.g. whether a punch is valid is decided by AttendanceService, not DashboardService).

## 9. Connection With Other Layers

- Called by: Integration controllers (`backend/integration/controllers`).
- Calls: Integration clients (`backend/integration/clients`).
- Allowed: Invocations to internal module clients.
- Prohibited: Direct database queries or cross-module model imports.

## 10. Data Flow

Enters: Validated request parameters and user authorization context.
Leaves: Assembled composite DTOs ready for controller serialization.

## 11. Validation Responsibility

Validates composite data integrity before assembly.

## 12. Authentication and Authorization

Enforces tenant isolation and passes caller context down to client headers.

## 13. Error Handling

Catches downstream client exceptions; applies fallback or raises gateway-level errors.

## 14. Transaction Responsibility

Must not manage transactions.

## 15. Logging and Auditing

Technical logging of downstream call fan-out latency and aggregation performance.

## 16. Naming Convention

Files: `<feature>_service.py`. Classes: `<Feature>Service`.

## 17. When to Add a New File

When a new composite aggregation workflow is required.

## 18. Common Mistakes

- Attempting to implement domain business rules in integration services.
- Serializing downstream calls sequentially when they could run concurrently.

## 19. Related Folders

- [Integration Gateway Root](../README.md)
- [Integration Controllers](../controllers/README.md)
- [Integration Clients](../clients/README.md)
- [Integration Schemas](../schemas/README.md)

## 20. Review Checklist

- [ ] Are independent downstream calls executed concurrently?
- [ ] Are partial failure policies explicitly designed and documented?
- [ ] Is there zero direct database access?
