# Shared Database

## 1. What Is This Folder?

The database package provides SQLAlchemy database engine setup, session factories, and base model classes.

## 2. Why Do We Need It?

It ensures that all modules connect to the primary PostgreSQL database using consistent connection pooling, async sessions, and shared audit columns.

## 3. Main Responsibility

Managing the database connection pool, providing session generators for dependency injection, and defining `BaseModel`.

## 4. How It Works

`connection.py` instantiates the SQLAlchemy engine using `DATABASE_URL` from settings. `session.py` creates an async sessionmaker and provides a `get_db()` generator for FastAPI dependency injection. `base_model.py` defines the declarative base containing standard columns (`id`, `created_at`, `updated_at`).

## 5. What This Folder Contains

| Item          | Type | Purpose                                                                       | Required/Optional |
|---------------|------|-------------------------------------------------------------------------------|-------------------|
| __init__.py   | File | Package initialization marker                                                 | Required          |
| base_model.py | File | SQLAlchemy DeclarativeBase model with standard metadata and timestamp columns | Required          |
| connection.py | File | Database engine creation, connection pool configuration, and health checks    | Required          |
| session.py    | File | Async sessionmaker and get_db() session generator for FastAPI dependencies    | Required          |

## 6. File Responsibilities

| File          | What It Does                                                                              | What It Contains                                 | Input                      | Output                 | Called By                  | Calls                   |
|---------------|-------------------------------------------------------------------------------------------|--------------------------------------------------|----------------------------|------------------------|----------------------------|-------------------------|
| __init__.py   | Initializes the shared database package.                                                  | Empty package marker.                            | None                       | None                   | Python runtime             | None                    |
| base_model.py | Defines the declarative base class for all models (Placeholder — Implementation Pending). | BaseModel class inheriting from DeclarativeBase. | None                       | Declarative base class | Module models, Alembic     | SQLAlchemy              |
| connection.py | Configures the SQLAlchemy engine and pool (Placeholder — Implementation Pending).         | Engine instantiation and ping utilities.         | Database URL from settings | SQLAlchemy Engine      | session.py, Alembic        | SQLAlchemy              |
| session.py    | Provides the get_db session dependency (Placeholder — Implementation Pending).            | sessionmaker and get_db generator.               | Database Engine            | AsyncSession generator | FastAPI route dependencies | SQLAlchemy AsyncSession |

## 7. Allowed Responsibilities

- Configuring engine connection pool parameters (pool size, max overflow, recycle time).
- Providing session generators that handle clean session closure.
- Defining declarative base classes for domain models.

## 8. Boundaries

- Must not define domain-specific tables (Attendance, Employee tables belong in modules).
- Must not execute business queries.
- Must not hardcode database connection credentials.

## 9. Connection With Other Layers

- Called by: Module models, module repositories, Alembic migrations (`backend/database/migrations`).
- Calls: Shared config (`backend/shared/config`), PostgreSQL.
- Allowed: Providing database infrastructure to all modules.
- Prohibited: Dependencies on domain modules.

## 10. Data Flow

Enters: Database configuration. Leaves: Active SQLAlchemy sessions and declarative model base.

## 11. Validation Responsibility

Validates database connectivity during application startup health checks.

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Catches connection and pool exhaustion errors and raises `DatabaseConnectionException`.

## 14. Transaction Responsibility

Provides transactional session context. Individual commit/rollback managed by services/repositories.

## 15. Logging and Auditing

Technical logging of connection pool checkout and slow queries when query debugging is enabled.

## 16. Naming Convention

Classes: `BaseModel`, `AsyncSessionLocal`. Functions: `get_db`.

## 17. When to Add a New File

When adding global database features such as read-replica routing or connection metrics.

## 18. Common Mistakes

- Defining domain business tables inside shared database models.
- Forgetting to close or yield database sessions properly, leaking connections.

## 19. Related Folders

- [Shared Root](../README.md)
- [Database Migrations](../../database/migrations/README.md)
- [Attendance Models](../../modules/attendance/models/README.md)
- [Shared Config](../config/README.md)

## 20. Review Checklist

- [ ] Does `session.py` guarantee proper session cleanup in a `finally` block?
- [ ] Does `BaseModel` include standard audit columns (`id`, `created_at`, `updated_at`)?
