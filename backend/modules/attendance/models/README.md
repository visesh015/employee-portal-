# Attendance Models

## 1. What Is This Folder?

The models layer defines the SQLAlchemy ORM declarative models representing the database tables logically owned by the Attendance module.

## 2. Why Do We Need It?

It provides a code-first, strongly-typed schema definition for PostgreSQL database tables, relationships, and constraints.

## 3. Main Responsibility

Declaring database table structures, column types, primary keys, foreign keys, indexes, and table-level constraints for attendance data.

## 4. How It Works

Models inherit from the shared `BaseModel` declarative class. They define column mappings (`id`, `employee_id`, `punch_in`, `punch_out`, `status`, `created_at`, `updated_at`) using SQLAlchemy column types and declare table indexes to support efficient timesheet and date-range queries.

## 5. What This Folder Contains

| Item                    | Type | Purpose                                                    | Required/Optional |
|-------------------------|------|------------------------------------------------------------|-------------------|
| __init__.py             | File | Package initialization marker                              | Required          |
| attendance_model.py     | File | SQLAlchemy ORM model for the attendance_records table      | Required          |
| regularization_model.py | File | SQLAlchemy ORM model for the regularization_requests table | Required          |

## 6. File Responsibilities

| File                    | What It Does                                             | What It Contains                              | Input                 | Output                              | Called By                         | Calls            |
|-------------------------|----------------------------------------------------------|-----------------------------------------------|-----------------------|-------------------------------------|-----------------------------------|------------------|
| __init__.py             | Initializes the models package.                          | Empty package marker.                         | None                  | None                                | Python runtime                    | None             |
| attendance_model.py     | Defines the database schema for attendance punches.      | AttendanceRecord SQLAlchemy model class.      | None (ORM definition) | Table metadata and entity instances | AttendanceRepository, Alembic     | Shared BaseModel |
| regularization_model.py | Defines the database schema for regularization requests. | RegularizationRequest SQLAlchemy model class. | None (ORM definition) | Table metadata and entity instances | RegularizationRepository, Alembic | Shared BaseModel |

## 7. Allowed Responsibilities

- Defining table column specifications, data types, nullability, and defaults.
- Defining table-level constraints (unique indexes, check constraints).
- Specifying relationships between tables strictly within the Attendance module.

## 8. Boundaries

- Must not define foreign key constraints to tables owned by other modules without architectural approval.
- Must not contain business methods or data processing logic.
- Must not be returned directly in API controller responses.

## 9. Connection With Other Layers

- Called by: Attendance repositories (`backend/modules/attendance/repositories`) and Alembic migrations.
- Calls: Shared `BaseModel` (`backend/shared/database/base_model.py`).
- Allowed: Declarative definitions inheriting from shared base.
- Prohibited: Cross-module model inheritance or cross-module direct foreign key imports.

## 10. Data Flow

Enters: Raw column attributes from repositories during object instantiation.
Leaves: Persisted table rows via SQLAlchemy session state.

## 11. Validation Responsibility

Enforces database-level constraints (not null, column data types, field length limits).

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Raises database integrity errors when schema constraints are violated during flush/commit.

## 14. Transaction Responsibility

Represents state within active database transactions.

## 15. Logging and Auditing

Model structures support audit logging by providing `created_at`, `updated_at`, and audit foreign keys.

## 16. Naming Convention

Files: `<entity>_model.py`. Classes: `PascalCase` (e.g. `AttendanceRecordModel`). Tables: `snake_case_plural`.

## 17. When to Add a New File

When a new physical table is required for Attendance persistence.

## 18. Common Mistakes

- Adding foreign keys pointing directly to foreign module tables, violating modular autonomy.
- Adding business logic or API serialization methods inside model classes.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Repositories](../repositories/README.md)
- [Shared Database Base Model](../../../shared/database/README.md)
- [Database Migrations](../../../database/migrations/README.md)

## 20. Review Checklist

- [ ] Do all models inherit from `BaseModel`?
- [ ] Are indexes added for frequently queried fields (e.g. `employee_id`, `punch_time`)?
- [ ] Is modular table ownership strictly respected?
