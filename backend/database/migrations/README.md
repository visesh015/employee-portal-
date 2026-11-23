# Database Migrations

## 1. What Is This Folder?

The Alembic migration environment containing the runtime runner, migration template, and version revisions.

## 2. Why Do We Need It?

It provides code-first database versioning, enabling repeatable, automated database upgrades and rollbacks across environments.

## 3. Main Responsibility

Executing schema migration scripts against PostgreSQL and providing revision script templates.

## 4. How It Works

`env.py` connects to the database, imports the shared `BaseModel` metadata (which aggregates all module models), compares the current database schema against the model metadata, and applies unapplied revision scripts from `versions/`.

## 5. What This Folder Contains

| Item           | Type   | Purpose                                                                     | Required/Optional |
|----------------|--------|-----------------------------------------------------------------------------|-------------------|
| env.py         | File   | Alembic Python environment script configuring migration engine and metadata | Required          |
| script.py.mako | File   | Mako template used by Alembic to generate new migration revision files      | Required          |
| versions       | Folder | Versioned migration revision script files                                   | Required          |

## 6. File Responsibilities

| File           | What It Does                                                                          | What It Contains                | Input                      | Output                              | Called By   | Calls                       |
|----------------|---------------------------------------------------------------------------------------|---------------------------------|----------------------------|-------------------------------------|-------------|-----------------------------|
| env.py         | Configures the Alembic migration runtime (Placeholder — Implementation Pending).      | Alembic migration runner logic. | Alembic context, DB URL    | Database connection, schema updates | Alembic CLI | SQLAlchemy, Shared Database |
| script.py.mako | Template for generating new migration scripts (Placeholder — Implementation Pending). | Mako template syntax.           | Alembic revision generator | New revision file                   | Alembic CLI | None                        |

## 7. Allowed Responsibilities

- Defining migration execution hooks and engine setup.
- Aggregating declarative model metadata for autogeneration.
- Customizing migration script templates.

## 8. Boundaries

- Must not contain domain business logic.
- Must not execute migrations automatically in production without review.

## 9. Connection With Other Layers

- Called by: Alembic CLI.
- Calls: Shared `BaseModel` (`backend/shared/database/base_model.py`) and PostgreSQL.
- Prohibited: Dependencies on controller or service logic.

## 10. Data Flow

Model definitions → Alembic autogenerate → Revision scripts → PostgreSQL DDL.

## 11. Validation Responsibility

Verifies that migration scripts apply cleanly in linear revision order.

## 12. Authentication and Authorization

Uses database credentials provided by environment settings.

## 13. Error Handling

Halts migration on DDL error and logs failed revision ID.

## 14. Transaction Responsibility

Wraps each migration script in an atomic DDL transaction.

## 15. Logging and Auditing

Records migration execution timestamps in technical logs and database tracking table.

## 16. Naming Convention

Revisions: `<hash>_<description>.py`.

## 17. When to Add a New File

When configuring Alembic custom comparison types or multi-schema hooks.

## 18. Common Mistakes

Modifying existing, already-applied migration files instead of generating new ones.

## 19. Related Folders

- [Database Root](../README.md)
- [Migration Versions](versions/README.md)
- [Shared Database Base Model](../../shared/database/README.md)

## 20. Review Checklist

- [ ] Does `env.py` import all module models to ensure comprehensive metadata coverage?
- [ ] Are all migrations written to be backward-compatible where feasible?
