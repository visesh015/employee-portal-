# Backend Root

## 1. What Is This Folder?

The core server-side API application root for the Employee Management Portal.

## 2. Why Do We Need It?

It orchestrates the entire server-side architecture, encapsulating business modules, integration gateways, shared libraries, database migrations, and automated tests.

## 3. Main Responsibility

Housing all server-side Python components, configuration files, and application entry points.

## 4. How It Works

The backend follows a service-based modular monorepo architecture. Incoming requests enter through `integration/` or module endpoints in `modules/`, leverage shared utilities in `shared/`, and persist state to PostgreSQL using SQLAlchemy models and Alembic migrations in `database/`.

Environment Tiering:
- Local: Individual developer machine (SQLite/local Postgres, debug logging).
- DEV: Shared development environment (continuous integration deployments).
- QA: Quality assurance environment (automated regression and QA testing).
- UAT: Business acceptance testing environment (client validation).

## 5. What This Folder Contains

| Item           | Type   | Purpose                                                                      | Required/Optional |
|----------------|--------|------------------------------------------------------------------------------|-------------------|
| .env.example   | File   | Template declaring all required environment variables with safe placeholders | Required          |
| pyproject.toml | File   | Python project build configuration, dependencies, and tool settings          | Required          |
| README.md      | File   | Backend documentation and architecture guide                                 | Required          |
| database       | Folder | Central database configuration and Alembic migration files                   | Required          |
| docs           | Folder | Architectural specifications, standards, and documentation manifest          | Required          |
| integration    | Folder | Frontend-facing Integration Gateway and composite aggregation service        | Required          |
| modules        | Folder | Autonomous business domain modules (Attendance, Employees, Leave, etc.)      | Required          |
| scripts        | Folder | Operational, migration, and development bootstrap scripts                    | Required          |
| shared         | Folder | Reusable cross-cutting technical components (security, db, config)           | Required          |
| tests          | Folder | Automated test suite (unit, api, integration, contract, end-to-end)          | Required          |

## 6. File Responsibilities

| File           | What It Does                                                                            | What It Contains                              | Input                  | Output                 | Called By         | Calls |
|----------------|-----------------------------------------------------------------------------------------|-----------------------------------------------|------------------------|------------------------|-------------------|-------|
| .env.example   | Declares all required environment variables (Placeholder — Implementation Pending).     | Safe variable templates without real secrets. | None                   | Template reference     | Developers, CI/CD | None  |
| pyproject.toml | Project build specification and dependency list (Placeholder — Implementation Pending). | Package metadata and tool configs.            | Python packaging tools | Installed dependencies | Poetry / Pip      | None  |
| README.md      | Documents the backend architecture and directory structure.                             | Markdown documentation.                       | None                   | Documentation          | Developers        | None  |

## 7. Allowed Responsibilities

Declaring backend-wide dependencies, project metadata, and top-level directory layout.

## 8. Boundaries

- Must not contain business domain logic directly in the backend root.
- Must not contain real passwords, Firebase keys, or production database URLs.

## 9. Connection With Other Layers

- Contains: `modules/`, `integration/`, `shared/`, `database/`, `tests/`, `docs/`, `scripts/`.
- Called by: Frontend and external API callers.

## 10. Data Flow

HTTP Request → Integration Gateway → Domain Modules → PostgreSQL Primary Database.

## 11. Validation Responsibility

Request schemas at boundaries; business validation in domain services.

## 12. Authentication and Authorization

Token verification via shared security; route authorization per endpoint.

## 13. Error Handling

Centralized exception handling producing standard `ErrorResponse` envelopes.

## 14. Transaction Responsibility

One primary PostgreSQL database for V1. Logical table ownership per module.

## 15. Logging and Auditing

Technical logging for engineers; business auditing for compliance.

## 16. Naming Convention

Packages: `snake_case`. Classes: `PascalCase`. Modules: `snake_case`.

## 17. When to Add a New File

Only during major project-wide architectural refactoring.

## 18. Common Mistakes

Mixing domain business logic into the root backend package.

## 19. Related Folders

- [Modules Root](modules/README.md)
- [Integration Gateway](integration/README.md)
- [Shared Components](shared/README.md)
- [Database Migrations](database/migrations/README.md)
- [Documentation Manifest](docs/documentation-manifest.md)

## 20. Review Checklist

- [ ] Are all dependencies cleanly partitioned in `pyproject.toml`?
- [ ] Are secrets strictly omitted from `.env.example`?
