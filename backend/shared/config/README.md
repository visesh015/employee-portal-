# Shared Config

## 1. What Is This Folder?

The configuration package manages application settings, environment variables, and operational flags.

## 2. Why Do We Need It?

It centralizes environment configuration using strongly typed Pydantic models, preventing runtime crashes due to missing or invalid settings.

## 3. Main Responsibility

Loading, validating, and exposing application settings (database URLs, secret keys, API ports, CORS rules).

## 4. How It Works

`settings.py` defines a `Settings` class inheriting from Pydantic `BaseSettings`. It automatically reads environment variables from the host OS or `.env` files, validates data types, and provides a singleton `settings` instance for the application.

## 5. What This Folder Contains

| Item        | Type | Purpose                                                               | Required/Optional |
|-------------|------|-----------------------------------------------------------------------|-------------------|
| __init__.py | File | Package initialization marker                                         | Required          |
| settings.py | File | Pydantic BaseSettings class declaring typed environment configuration | Required          |

## 6. File Responsibilities

| File        | What It Does                                                                 | What It Contains                             | Input                        | Output                      | Called By                          | Calls                 |
|-------------|------------------------------------------------------------------------------|----------------------------------------------|------------------------------|-----------------------------|------------------------------------|-----------------------|
| __init__.py | Initializes the config package.                                              | Empty package marker.                        | None                         | None                        | Python runtime                     | None                  |
| settings.py | Defines typed configuration settings (Placeholder — Implementation Pending). | Settings class inheriting from BaseSettings. | Environment variables / .env | Settings singleton instance | Application bootstrap, clients, DB | Pydantic BaseSettings |

## 7. Allowed Responsibilities

- Declaring required and optional environment variables.
- Providing default values for local development.
- Validating configuration formats (e.g. Postgres DSN, URL formats).

## 8. Boundaries

- Must not hardcode production secrets, passwords, or Firebase keys.
- Must not execute database queries or business operations.
- Must not modify environment variables at runtime.

## 9. Connection With Other Layers

- Called by: Database connection, security handlers, clients, main applications.
- Calls: Operating system environment variables.
- Allowed: Providing configuration across the backend.
- Prohibited: Reverse dependencies on domain modules.

## 10. Data Flow

Enters: Raw OS environment variables. Leaves: Validated, strongly typed `Settings` attributes.

## 11. Validation Responsibility

Validates environment variable types, formats, and required presence at startup.

## 12. Authentication and Authorization

Holds configuration values for authentication (JWT algorithms, secret keys, Firebase project IDs).

## 13. Error Handling

Raises Pydantic validation errors during application startup if required settings are missing.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Configures application logging levels (`LOG_LEVEL`). Must not log sensitive secret values.

## 16. Naming Convention

Classes: `Settings`. Attributes: uppercase snake_case (e.g. `DATABASE_URL`, `JWT_SECRET`).

## 17. When to Add a New File

When a new global service, timeout, or external integration credential is introduced.

## 18. Common Mistakes

- Hardcoding default secrets in `settings.py`.
- Reading `os.environ` directly in domain modules instead of accessing `settings`.

## 19. Related Folders

- [Shared Root](../README.md)
- [Shared Database](../database/README.md)
- [Shared Security](../security/README.md)
- [Environment Example](../../README.md)

## 20. Review Checklist

- [ ] Are all sensitive environment variables declared without plaintext defaults?
- [ ] Is all configuration access routed through the typed `settings` object?
