# Attendance Clients

## 1. What Is This Folder?

The clients layer contains HTTP client wrappers used by the Attendance module to communicate with external domain modules.

## 2. Why Do We Need It?

It guarantees that Attendance does not directly query foreign database tables, preserving module autonomy and independent deployability.

## 3. Main Responsibility

Executing internal HTTP requests to foreign modules (Employees, Leave, Shift), managing timeouts, propagating correlation IDs, and converting responses.

## 4. How It Works

Clients encapsulate HTTP communication using an asynchronous HTTP client (e.g. HTTPX). They forward request headers (correlation ID, authentication context), call downstream module endpoints, handle network failures and timeouts gracefully, and map downstream JSON responses into typed domain objects for Attendance services.

## 5. What This Folder Contains

| Item               | Type | Purpose                                                                    | Required/Optional |
|--------------------|------|----------------------------------------------------------------------------|-------------------|
| __init__.py        | File | Package initialization marker                                              | Required          |
| employee_client.py | File | Client for fetching employee profile, status, and reporting manager info   | Required          |
| leave_client.py    | File | Client for verifying approved leave records during attendance checks       | Required          |
| shift_client.py    | File | Client for fetching shift schedules, start/end times, and grace thresholds | Required          |

## 6. File Responsibilities

| File               | What It Does                                               | What It Contains                        | Input                     | Output                            | Called By                           | Calls                  |
|--------------------|------------------------------------------------------------|-----------------------------------------|---------------------------|-----------------------------------|-------------------------------------|------------------------|
| __init__.py        | Initializes the clients package.                           | Empty package marker.                   | None                      | None                              | Python runtime                      | None                   |
| employee_client.py | Queries employee active status and profile info.           | EmployeeClient class and query methods. | Employee ID, auth context | EmployeeProfileDTO / ActiveStatus | ClockService, RegularizationService | HTTPX, Shared Settings |
| leave_client.py    | Queries employee leave balance and approved leave dates.   | LeaveClient class and check methods.    | Employee ID, date range   | LeaveStatusDTO                    | ClockService, RegularizationService | HTTPX, Shared Settings |
| shift_client.py    | Queries shift assignments and grace period configurations. | ShiftClient class and fetch methods.    | Employee ID, date         | ShiftDetailsDTO                   | ClockService                        | HTTPX, Shared Settings |

## 7. Allowed Responsibilities

- Building HTTP requests to external internal APIs.
- Injecting correlation IDs and service tokens into request headers.
- Handling HTTP connection timeouts, retries, and mapping downstream 4xx/5xx to domain exceptions.

## 8. Boundaries

- Must not connect directly to foreign module database tables.
- Must not make business decisions based on raw status codes (delegated to services).
- Must not expose downstream API keys or secrets in logs.

## 9. Connection With Other Layers

- Called by: Attendance services (`backend/modules/attendance/services`).
- Calls: Foreign module HTTP endpoints (Employees, Leave, Shift).
- Allowed: Outbound HTTP requests to approved internal services.
- Prohibited: Direct database connections to external databases.

## 10. Data Flow

Enters: Lookup IDs, date ranges, forwarded authentication context from Attendance services.
Leaves: Parsed downstream DTOs or downstream communication failure exceptions.

## 11. Validation Responsibility

Validates that downstream responses conform to expected contract schemas.

## 12. Authentication and Authorization

Forwards user or service authorization tokens in outbound request headers.

## 13. Error Handling

Translates downstream HTTP 404, 500, or network timeouts into `DownstreamServiceException`.

## 14. Transaction Responsibility

Does not manage database transactions. External calls occur outside local database transactions.

## 15. Logging and Auditing

Technical logging of outbound request latency, URL, status code, and correlation ID.

## 16. Naming Convention

Files: `<target_service>_client.py`. Classes: `<TargetService>Client`. Methods: `get_employee`, `check_leave`.

## 17. When to Add a New File

When Attendance requires data owned by another distinct domain service.

## 18. Common Mistakes

- Importing foreign module repositories or models instead of using HTTP clients.
- Failing to configure timeouts, leading to thread exhaustion during downstream outages.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Services](../services/README.md)
- [Employees Module](../../employees/README.md)
- [Leave Module](../../leave/README.md)

## 20. Review Checklist

- [ ] Are explicit timeouts configured on all client requests?
- [ ] Is the correlation ID forwarded in all outbound headers?
- [ ] Are downstream network exceptions mapped cleanly?
