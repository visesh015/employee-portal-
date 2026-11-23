# Shared Audit

## 1. What Is This Folder?

The audit package provides the infrastructure for recording immutable business audit trails.

## 2. Why Do We Need It?

Enterprise compliance, security forensics, and operational integrity require an immutable record of sensitive business actions (who modified a record, what changed, when, and why).

## 3. Main Responsibility

Providing `audit_service.py` and `audit_repository.py` to record structured business audit entries.

## 4. How It Works

When a service performs a sensitive state modification (e.g. approving regularization or modifying employee data), it invokes `AuditService.record_event(...)`. The audit service constructs an audit event DTO with actor identity, target entity, timestamp, before/after state diff, and correlation ID, and writes it to the central audit log table via `AuditRepository`.

Technical Logging vs Business Auditing:
- Technical Logging: Captures runtime diagnostics, query latency, stack traces, and system health for engineers.
- Business Auditing: Captures legally binding historical event trails (actor, action, entity, before/after diff) for compliance and business users.

## 5. What This Folder Contains

| Item                | Type | Purpose                                                          | Required/Optional |
|---------------------|------|------------------------------------------------------------------|-------------------|
| __init__.py         | File | Package initialization marker                                    | Required          |
| audit_repository.py | File | Database queries and persistence for the central audit log table | Required          |
| audit_service.py    | File | Business audit recording service interface and event dispatcher  | Required          |

## 6. File Responsibilities

| File                | What It Does                                                                       | What It Contains       | Input                          | Output                 | Called By       | Calls           |
|---------------------|------------------------------------------------------------------------------------|------------------------|--------------------------------|------------------------|-----------------|-----------------|
| __init__.py         | Initializes the audit package.                                                     | Empty package marker.  | None                           | None                   | Python runtime  | None            |
| audit_repository.py | Persists audit log records to the database (Placeholder — Implementation Pending). | AuditRepository class. | AuditEvent model, DB session   | Persisted AuditRecord  | AuditService    | SQLAlchemy      |
| audit_service.py    | Dispatches business audit events (Placeholder — Implementation Pending).           | AuditService class.    | Actor ID, action, entity, diff | None / Acknowledgement | Domain Services | AuditRepository |

## 7. Allowed Responsibilities

- Creating and querying audit trail records.
- Serializing entity before/after state differences.
- Guaranteeing audit trail immutability (insert-only operations).

## 8. Boundaries

- Must not allow updating or deleting existing audit logs.
- Must not store sensitive credentials, plaintext passwords, or raw payment tokens.
- Must not replace standard technical application logging.

## 9. Connection With Other Layers

- Called by: Domain services (`backend/modules/*/services`).
- Calls: Shared database session.
- Allowed: Inbound calls from any service requiring audit recording.
- Prohibited: Importing business modules.

## 10. Data Flow

Enters: Audit event metadata (actor, action, resource, diff). Leaves: Persisted immutable audit records.

## 11. Validation Responsibility

Validates audit event schema format and required fields (actor, action, timestamp).

## 12. Authentication and Authorization

Captures authenticated actor identity from the current security context.

## 13. Error Handling

Ensures audit write failures are captured without silently corrupting business transactions.

## 14. Transaction Responsibility

Participates in the surrounding database transaction or writes via an independent audit transaction. Requires Team Confirmation.

## 15. Logging and Auditing

Dedicated strictly to business auditing.

## 16. Naming Convention

Files: `audit_<role>.py`. Classes: `AuditService`, `AuditRepository`.

## 17. When to Add a New File

When extending audit recording with async event publishing or compliance export utilities.

## 18. Common Mistakes

- Treating technical application logs as a substitute for business audit trails.
- Adding update or delete methods to the audit repository, violating immutability.

## 19. Related Folders

- [Shared Root](../README.md)
- [Attendance Services](../../modules/attendance/services/README.md)
- [Shared Database](../database/README.md)

## 20. Review Checklist

- [ ] Is the audit repository strictly insert-only (no update or delete methods)?
- [ ] Are sensitive values masked or excluded before audit serialization?
