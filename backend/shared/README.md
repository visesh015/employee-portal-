# Shared Components Root

## 1. What Is This Folder?

The `shared` package contains domain-agnostic, reusable technical infrastructure components used across the Employee Management Portal backend.

## 2. Why Do We Need It?

Centralizing technical cross-cutting concerns (database sessions, authentication verification, configuration, middleware, standard exception handling) eliminates boilerplate and ensures uniform security and error standards.

## 3. Main Responsibility

Providing foundational technical services, base classes, middleware, security utilities, and common schemas.

## 4. How It Works

Shared packages are imported by domain modules and the integration gateway. Modules depend inward on Shared, while Shared maintains zero dependencies on business modules.

## 5. What This Folder Contains

| Item        | Type   | Purpose                                                                             | Required/Optional |
|-------------|--------|-------------------------------------------------------------------------------------|-------------------|
| __init__.py | File   | Package initialization marker                                                       | Required          |
| audit       | Folder | Business auditing infrastructure (audit service, audit repository)                  | Required          |
| config      | Folder | Application settings and environment variable loading                               | Required          |
| database    | Folder | SQLAlchemy database engine, async session factory, and declarative base model       | Required          |
| exceptions  | Folder | Shared base exceptions and global FastAPI exception handlers                        | Required          |
| helpers     | Folder | Generic datetime and string validation utility functions                            | Required          |
| middleware  | Folder | HTTP request processing middleware (request ID, access logging, error containment)  | Required          |
| schemas     | Folder | Standard API response envelopes (success, error, pagination)                        | Required          |
| security    | Folder | Firebase/JWT verification, token parsing, and role/scope authorization dependencies | Required          |

## 6. File Responsibilities

| File        | What It Does                    | What It Contains      | Input | Output | Called By      | Calls |
|-------------|---------------------------------|-----------------------|-------|--------|----------------|-------|
| __init__.py | Initializes the shared package. | Empty package marker. | None  | None   | Python runtime | None  |

## 7. Allowed Responsibilities

- Technical infrastructure, base classes, and configuration.
- Standard API response envelopes and error handling.
- Cross-cutting security and database session providers.

## 8. Boundaries

- Must not contain domain business logic for Attendance, Leave, Employees, or WFH.
- Must not import any code from `backend/modules` or `backend/integration`.
- Must not become a catch-all dumping ground for unclassified code.

## 9. Connection With Other Layers

- Called by: All domain modules (`backend/modules/*`), Integration Gateway (`backend/integration`).
- Calls: External libraries (FastAPI, SQLAlchemy, Pydantic, HTTPX, PyJWT).
- Allowed: Inward dependency from modules to Shared.
- Prohibited: Outward dependency from Shared to modules.

## 10. Data Flow

Receives configuration values and technical context; provides database sessions, user identity, and standard envelopes.

## 11. Validation Responsibility

Shared validation helpers validate common technical formats (ISO dates, email strings).

## 12. Authentication and Authorization

Houses authentication token verification and role parsing dependencies used by all routers.

## 13. Error Handling

Houses global exception handlers that translate domain exceptions into standard error envelopes.

## 14. Transaction Responsibility

Houses the SQLAlchemy session factory and session lifecycle management (`session.py`).

## 15. Logging and Auditing

Provides the distinction and infrastructure for both technical logging and business auditing.

## 16. Naming Convention

Files: `snake_case.py`. Classes: `PascalCase`. Variables: `snake_case`.

## 17. When to Add a New File

When a genuinely project-wide technical capability is required by multiple independent modules.

## 18. Common Mistakes

- Placing business rules inside shared helpers.
- Importing domain modules into shared components, creating circular dependencies.

## 19. Related Folders

- [Backend Root](../README.md)
- [Audit](audit/README.md)
- [Config](config/README.md)
- [Database](database/README.md)
- [Exceptions](exceptions/README.md)
- [Middleware](middleware/README.md)
- [Security](security/README.md)

## 20. Review Checklist

- [ ] Are there zero imports of business modules anywhere in shared?
- [ ] Is all code strictly domain-agnostic?
