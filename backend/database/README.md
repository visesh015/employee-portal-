# Database Root

## 1. What Is This Folder?

The central database configuration and migration orchestration directory for the backend.

## 2. Why Do We Need It?

It houses the Alembic migration environment and configuration required to manage PostgreSQL schema evolutions centrally.

## 3. Main Responsibility

Configuring Alembic database connection parameters and hosting migration scripts.

## 4. How It Works

Alembic reads configuration from `alembic.ini` and executes migration logic in `migrations/env.py`. Although migrations are run centrally, code-first development guarantees that each module defines its own models and maintains logical ownership over its tables.

## 5. What This Folder Contains

| Item        | Type   | Purpose                                                                           | Required/Optional |
|-------------|--------|-----------------------------------------------------------------------------------|-------------------|
| alembic.ini | File   | Alembic configuration file specifying database URL and migration script locations | Required          |
| migrations  | Folder | Alembic migration environment, templates, and versioned revisions                 | Required          |

## 6. File Responsibilities

| File        | What It Does                                                                              | What It Contains              | Input       | Output                   | Called By   | Calls |
|-------------|-------------------------------------------------------------------------------------------|-------------------------------|-------------|--------------------------|-------------|-------|
| alembic.ini | Configures Alembic migration environment settings (Placeholder — Implementation Pending). | INI configuration directives. | Alembic CLI | Configuration parameters | Alembic CLI | None  |

## 7. Allowed Responsibilities

Database migration configuration and migration script definitions.

## 8. Boundaries

- Must not contain application route handlers or business logic.
- Must not bypass Alembic to execute raw unversioned schema modifications.

## 9. Connection With Other Layers

- Called by: Alembic CLI and migration scripts (`backend/scripts/run_migrations.py`).
- Calls: PostgreSQL database.

## 10. Data Flow

Enters: Migration commands and model metadata. Leaves: DDL executions against PostgreSQL.

## 11. Validation Responsibility

Validates schema changes against existing database state during migration dry-runs.

## 12. Authentication and Authorization

Database administrative credentials used strictly during migration execution.

## 13. Error Handling

Alembic rolls back failed DDL migrations in databases supporting transactional DDL.

## 14. Transaction Responsibility

Manages transactional DDL execution during database upgrades.

## 15. Logging and Auditing

Logs migration upgrade/downgrade execution history in `alembic_version` table.

## 16. Naming Convention

INI files: lowercase. Migration files: `<timestamp_or_revision>_<description>.py`.

## 17. When to Add a New File

When configuring multi-database routing or custom migration hooks.

## 18. Common Mistakes

Hardcoding production database passwords inside `alembic.ini`.

## 19. Related Folders

- [Backend Root](../README.md)
- [Migrations](migrations/README.md)
- [Shared Database](../shared/database/README.md)

## 20. Review Checklist

- [ ] Does `alembic.ini` dynamically read `DATABASE_URL` from environment variables?
- [ ] Are all schema migrations strictly versioned?
