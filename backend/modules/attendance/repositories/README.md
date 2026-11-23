# Attendance Repositories

## 1. What Is This Folder?

The repositories layer encapsulates all database persistence, retrieval, and query logic for the Attendance module.

## 2. Why Do We Need It?

It decouples data persistence technologies (PostgreSQL, SQLAlchemy) from domain business logic, ensuring testability and clean architecture.

## 3. Main Responsibility

Executing database queries, managing CRUD operations for attendance entities, filtering records, and applying pagination.

## 4. How It Works

Repositories receive database sessions via dependency injection. They construct and execute SQLAlchemy queries against `AttendanceModel` and `RegularizationModel`, handling record lookups, insertions, updates, and soft deletions. Repositories return ORM model instances or collections to the calling service.

## 5. What This Folder Contains

| Item                         | Type | Purpose                                                      | Required/Optional |
|------------------------------|------|--------------------------------------------------------------|-------------------|
| __init__.py                  | File | Package initialization marker                                | Required          |
| attendance_repository.py     | File | Database operations for clock punches and attendance records | Required          |
| regularization_repository.py | File | Database operations for regularization request records       | Required          |

## 6. File Responsibilities

| File                         | What It Does                                             | What It Contains                                  | Input                                     | Output                          | Called By                   | Calls                           |
|------------------------------|----------------------------------------------------------|---------------------------------------------------|-------------------------------------------|---------------------------------|-----------------------------|---------------------------------|
| __init__.py                  | Initializes the repositories package.                    | Empty package marker.                             | None                                      | None                            | Python runtime              | None                            |
| attendance_repository.py     | Performs database operations for attendance punches.     | AttendanceRepository class and query methods.     | Employee ID, date, punch data, DB session | AttendanceModel entity/list     | ClockService, ReportService | SQLAlchemy, AttendanceModel     |
| regularization_repository.py | Performs database operations for regularization records. | RegularizationRepository class and query methods. | Request ID, status filter, DB session     | RegularizationModel entity/list | RegularizationService       | SQLAlchemy, RegularizationModel |

## 7. Allowed Responsibilities

- Building and executing SQLAlchemy queries for attendance tables.
- Implementing record filtering, sorting, and pagination.
- Mapping raw query results to ORM model entities.

## 8. Boundaries

- Must not contain business decision rules (e.g. determining if overtime is payable).
- Must not access tables owned by other modules (e.g. employee tables).
- Must not handle HTTP requests or raise HTTP exceptions.

## 9. Connection With Other Layers

- Called by: Attendance services (`backend/modules/attendance/services`).
- Calls: Attendance models (`backend/modules/attendance/models`) and SQLAlchemy.
- Allowed: Database communication via SQLAlchemy session.
- Prohibited: Calls to controllers, external clients, or foreign repositories.

## 10. Data Flow

Enters: Query filters, entity attribute dictionaries, database session instances.
Leaves: SQLAlchemy ORM model instances, record lists, row counts.

## 11. Validation Responsibility

Database constraint adherence (primary keys, foreign keys within module, unique punch constraints).

## 12. Authentication and Authorization

No security decisions. Repositories execute queries based on parameters passed by authorized services.

## 13. Error Handling

Catches SQLAlchemy database exceptions (integrity errors, timeouts) and translates them to domain persistence exceptions.

## 14. Transaction Responsibility

Participates in active session transactions. Requires Team Confirmation for commit and rollback conventions.

## 15. Logging and Auditing

Technical query execution logging via database connection pool; no direct business auditing.

## 16. Naming Convention

Files: `<entity>_repository.py`. Classes: `<Entity>Repository`. Methods: `get_by_id`, `create`, `list_by_employee`.

## 17. When to Add a New File

When a new database entity table is added to the Attendance module.

## 18. Common Mistakes

- Embedding business logic or calculation rules inside query methods.
- Committing transactions prematurely within repository methods instead of service boundaries.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Services](../services/README.md)
- [Attendance Models](../models/README.md)
- [Shared Database](../../../shared/database/README.md)

## 20. Review Checklist

- [ ] Do all methods query exclusively through SQLAlchemy session?
- [ ] Are queries parameterized to prevent SQL injection?
- [ ] Is there zero business logic in repository methods?
