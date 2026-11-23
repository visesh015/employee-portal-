# Integration Clients

## 1. What Is This Folder?

HTTP client wrappers used by the Integration Gateway to query internal domain modules over HTTP.

## 2. Why Do We Need It?

It enforces architectural decoupling between the Integration gateway and downstream modules, ensuring modules are treated as black-box services.

## 3. Main Responsibility

Calling module endpoints, forwarding headers (authorization, correlation ID), enforcing timeouts, and parsing responses.

## 4. How It Works

Clients use an asynchronous HTTP client to send requests to module endpoints. They forward the incoming user's JWT or a service-to-service authorization token and correlation ID, receive JSON responses, and deserialize them into strongly typed DTOs.

## 5. What This Folder Contains

| Item                 | Type | Purpose                                                                      | Required/Optional |
|----------------------|------|------------------------------------------------------------------------------|-------------------|
| __init__.py          | File | Package initialization marker                                                | Required          |
| attendance_client.py | File | Client for calling Attendance module endpoints (punches, status, reports)    | Required          |
| employee_client.py   | File | Client for calling Employees module endpoints (profiles, roles, departments) | Required          |
| holiday_client.py    | File | Client for calling Holidays module endpoints (calendars, upcoming holidays)  | Required          |
| leave_client.py      | File | Client for calling Leave module endpoints (balances, active leave requests)  | Required          |
| wfh_client.py        | File | Client for calling WFH module endpoints (WFH status, quotas, requests)       | Required          |

## 6. File Responsibilities

| File                 | What It Does                                                         | What It Contains                   | Input                                 | Output          | Called By            | Calls                  |
|----------------------|----------------------------------------------------------------------|------------------------------------|---------------------------------------|-----------------|----------------------|------------------------|
| __init__.py          | Initializes the integration clients package.                         | Empty package marker.              | None                                  | None            | Python runtime       | None                   |
| attendance_client.py | Calls Attendance module APIs (Placeholder — Implementation Pending). | AttendanceClient class definition. | Employee ID, date range, auth context | Attendance DTOs | Integration Services | HTTPX / Attendance API |
| employee_client.py   | Calls Employees module APIs (Placeholder — Implementation Pending).  | EmployeeClient class definition.   | Employee ID, auth context             | Employee DTOs   | Integration Services | HTTPX / Employees API  |
| holiday_client.py    | Calls Holidays module APIs (Placeholder — Implementation Pending).   | HolidayClient class definition.    | Year, location, auth context          | Holiday DTOs    | Integration Services | HTTPX / Holidays API   |
| leave_client.py      | Calls Leave module APIs (Placeholder — Implementation Pending).      | LeaveClient class definition.      | Employee ID, auth context             | Leave DTOs      | Integration Services | HTTPX / Leave API      |
| wfh_client.py        | Calls WFH module APIs (Placeholder — Implementation Pending).        | WfhClient class definition.        | Employee ID, auth context             | WFH DTOs        | Integration Services | HTTPX / WFH API        |

## 7. Allowed Responsibilities

- Constructing HTTP requests with correlation IDs and auth headers.
- Handling HTTP connection pooling, timeouts, and network retry policies.
- Deserializing module responses into typed client DTOs.

## 8. Boundaries

- Must not make business decisions based on returned data.
- Must not connect directly to module databases.
- Must not leak tokens or passwords in client logs.

## 9. Connection With Other Layers

- Called by: Integration services (`backend/integration/services`).
- Calls: Internal module HTTP endpoints.
- Allowed: Outbound HTTP calls to registered internal modules.
- Prohibited: Direct database imports or connections.

## 10. Data Flow

Enters: Query parameters, entity IDs, forwarded headers. Leaves: Typed domain DTOs or downstream exceptions.

## 11. Validation Responsibility

Validates response payloads against expected client schema contracts.

## 12. Authentication and Authorization

Forwards authenticated user context and service identity in request headers.

## 13. Error Handling

Translates downstream HTTP 4xx/5xx status codes into `DownstreamServiceException`.

## 14. Transaction Responsibility

Must not manage transactions.

## 15. Logging and Auditing

Technical latency and error logging for downstream calls.

## 16. Naming Convention

Files: `<module>_client.py`. Classes: `<Module>Client`.

## 17. When to Add a New File

When the Integration layer needs to communicate with a new domain module.

## 18. Common Mistakes

Failing to set request timeouts; leaking raw downstream error tracebacks to the service layer.

## 19. Related Folders

- [Integration Gateway Root](../README.md)
- [Integration Services](../services/README.md)
- [Shared Config](../../shared/config/README.md)

## 20. Review Checklist

- [ ] Are explicit connection and read timeouts configured on all clients?
- [ ] Is correlation ID propagated in all outbound request headers?
