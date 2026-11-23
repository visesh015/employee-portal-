# Backend Scripts

## 1. What Is This Folder?

The scripts directory contains automation, database seeding, migration, and development utilities.

## 2. Why Do We Need It?

It provides safe, automated commands for bootstrapping development environments, running migrations, and launching applications.

## 3. Main Responsibility

Executing maintenance, initialization, and local runner workflows.

## 4. How It Works

`run_migrations.py` wraps Alembic CLI execution to apply database migrations programmatically. `seed_dev_data.py` populates a local or DEV database with mock employee profiles, shifts, and test punches. `start_all.py` launches backend application servers locally using Uvicorn.

## 5. What This Folder Contains

| Item              | Type | Purpose                                                                | Required/Optional |
|-------------------|------|------------------------------------------------------------------------|-------------------|
| run_migrations.py | File | Automated script to apply Alembic database migrations programmatically | Required          |
| seed_dev_data.py  | File | Development utility to seed realistic mock data into the database      | Required          |
| start_all.py      | File | Local development runner launching all API services concurrently       | Required          |

## 6. File Responsibilities

| File              | What It Does                                                                         | What It Contains              | Input                | Output                         | Called By         | Calls               |
|-------------------|--------------------------------------------------------------------------------------|-------------------------------|----------------------|--------------------------------|-------------------|---------------------|
| run_migrations.py | Executes Alembic migrations programmatically (Placeholder — Implementation Pending). | Migration runner script.      | Environment settings | Database migration execution   | Developers, CI/CD | Alembic             |
| seed_dev_data.py  | Populates mock records for local testing (Placeholder — Implementation Pending).     | Data generation script.       | Database session     | Inserted mock database records | Developers        | SQLAlchemy, Modules |
| start_all.py      | Starts all backend services locally (Placeholder — Implementation Pending).          | Uvicorn launch orchestration. | Port settings        | Running HTTP servers           | Developers        | Uvicorn, FastAPI    |

## 7. Allowed Responsibilities

- Automating repetitive developer workflows.
- Seeding non-production databases with test fixtures.
- Wrapping Alembic commands with safety environment checks.

## 8. Boundaries

- Must never execute seeding scripts against production or UAT environments.
- Must not contain hardcoded administrative credentials or database passwords.
- Must not be imported by application runtime code.

## 9. Connection With Other Layers

- Called by: Developers from the command line or CI/CD runner pipelines.
- Calls: Shared database, Alembic, Uvicorn, and domain repositories.

## 10. Data Flow

CLI arguments / environment settings → script execution → database / server state.

## 11. Validation Responsibility

Validates that the target environment is not production before executing destructive actions.

## 12. Authentication and Authorization

Uses environment-provided database and service credentials.

## 13. Error Handling

Catches CLI execution exceptions and prints user-friendly diagnostic messages to stdout/stderr.

## 14. Transaction Responsibility

Manages database transactions during data seeding, rolling back on error.

## 15. Logging and Auditing

Logs progress and errors to the terminal console.

## 16. Naming Convention

Files: `snake_case.py`.

## 17. When to Add a New File

When automating a new development, maintenance, or data migration task.

## 18. Common Mistakes

- Running `seed_dev_data.py` in shared QA or UAT environments, overwriting real test data.
- Hardcoding passwords or API keys inside script files.

## 19. Related Folders

- [Backend Root](../README.md)
- [Database Migrations](../database/migrations/README.md)
- [Shared Config](../shared/config/README.md)

## 20. Review Checklist

- [ ] Does `seed_dev_data.py` contain an explicit safety guard blocking execution in production?
- [ ] Are all scripts idempotent where possible?
