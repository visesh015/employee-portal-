# Leave Module Root

## 1. What Is This Folder?

The `Leave` module encapsulates all capabilities and persistence for leave request applications, leave quota accrual, balance tracking, multi-tier manager approvals, and annual leave encashment.

## 2. Why Do We Need It?

It provides automated leave balance calculations and approval workflows, preventing unauthorized absences and ensuring payroll compliance. Keeping this logic isolated ensures modular cohesion and clean domain boundaries.

## 3. Main Responsibility

Managing domain rules, persistence, and external contracts for leave data.

## 4. How It Works

The Leave module operates as an independent domain service following the standard 7-layer architecture. HTTP requests enter via controllers, validate against schemas, execute business rules in services, and persist via repositories. For canonical architectural flow and implementation patterns, see [Attendance Module Reference](../attendance/README.md).

## 5. What This Folder Contains

| Item            | Type   | Purpose                                                                       | Required/Optional |
|-----------------|--------|-------------------------------------------------------------------------------|-------------------|
| __init__.py     | File   | Package initialization marker                                                 | Required          |
| dependencies.py | File   | FastAPI dependency injection providers (Placeholder — Implementation Pending) | Required          |
| exceptions.py   | File   | Domain-specific exception definitions (Placeholder — Implementation Pending)  | Required          |
| main.py         | File   | Module router and sub-app entry point (Placeholder — Implementation Pending)  | Required          |
| clients         | Folder | HTTP clients for inter-module communication                                   | Required          |
| controllers     | Folder | Route handlers receiving HTTP requests                                        | Required          |
| helpers         | Folder | Domain utility and calculation functions                                      | Required          |
| models          | Folder | SQLAlchemy ORM database models                                                | Required          |
| repositories    | Folder | Database query and persistence layer                                          | Required          |
| schemas         | Folder | Pydantic request and response schemas                                         | Required          |
| services        | Folder | Domain business logic and workflow orchestration                              | Required          |

## 6. File Responsibilities

| File            | What It Does                                                                             | What It Contains   | Input         | Output       | Called By           | Calls                      |
|-----------------|------------------------------------------------------------------------------------------|--------------------|---------------|--------------|---------------------|----------------------------|
| __init__.py     | Marks the directory as a Python package.                                                 | Package marker.    | None          | None         | Python runtime      | None                       |
| dependencies.py | Provides FastAPI dependencies for route handlers (Placeholder — Implementation Pending). | Empty placeholder. | HTTP Request  | Dependencies | Controllers         | Requires Team Confirmation |
| exceptions.py   | Defines custom domain exceptions (Placeholder — Implementation Pending).                 | Empty placeholder. | Error context | Exceptions   | Services            | Requires Team Confirmation |
| main.py         | Initializes the module router (Placeholder — Implementation Pending).                    | Empty placeholder. | Bootstrap     | APIRouter    | Integration Gateway | Requires Team Confirmation |

## 7. Allowed Responsibilities

Defining domain models, rules, and contracts for Leave.

## 8. Boundaries

Must not access other module tables directly; must not import foreign repositories or ORM models.

## 9. Connection With Other Layers

- Called by: Integration Gateway (`backend/integration`).
- Calls: Shared components (`backend/shared`); other modules via `clients`.
- Allowed: Inbound calls from Integration; outbound HTTP calls via clients.
- Prohibited: Cross-module direct database queries or cross-module model imports.

## 10. Data Flow

Enters: Validated requests from Integration. Leaves: Structured Leave domain responses.

## 11. Validation Responsibility

Schemas handle format validation; services handle business rule validation.

## 12. Authentication and Authorization

Enforces user identity and role authorization via shared security dependencies.

## 13. Error Handling

Domain exceptions map to standard shared error envelopes via global exception handlers.

## 14. Transaction Responsibility

Requires Team Confirmation. Transactions managed at the service boundary per unit of work.

## 15. Logging and Auditing

Technical logging via shared middleware; business auditing via shared audit service.

## 16. Naming Convention

Follows standard project conventions. See [Attendance Module Reference](../attendance/README.md#16-naming-convention).

## 17. When to Add a New File

When extending Leave functionality with new approved sub-features.

## 18. Common Mistakes

Cross-module database imports; bypassing services to query repositories directly.

## 19. Related Folders

- [Modules Root](../README.md)
- [Attendance Canonical Reference](../attendance/README.md)
- [Controllers](controllers/README.md)
- [Services](services/README.md)
- [Repositories](repositories/README.md)
- [Models](models/README.md)
- [Schemas](schemas/README.md)
- [Clients](clients/README.md)
- [Helpers](helpers/README.md)

## 20. Review Checklist

- [ ] Are all Leave data operations strictly confined to this module?
- [ ] Are external module communications routed exclusively through clients?
- [ ] Does the implementation match the canonical Attendance reference?
