# Shared Schemas

## 1. What Is This Folder?

The shared schemas package defines universal API response envelope contracts and pagination data models.

## 2. Why Do We Need It?

It guarantees that every API response across all modules follows a uniform, predictable JSON envelope structure.

## 3. Main Responsibility

Defining generic models for success responses, error envelopes, and paginated collection results.

## 4. How It Works

`success_response.py` defines `SuccessResponse[T]`, wrapping data payloads with status and timestamp. `error_response.py` defines `ErrorResponse`, standardizing error codes, messages, details, and correlation IDs. `pagination.py` defines `PaginatedResponse[T]` and `PaginationParams` (page, page_size, total_records, total_pages).

## 5. What This Folder Contains

| Item                | Type | Purpose                                                                       | Required/Optional |
|---------------------|------|-------------------------------------------------------------------------------|-------------------|
| __init__.py         | File | Package initialization marker                                                 | Required          |
| error_response.py   | File | Standard JSON error envelope model with error codes and correlation ID        | Required          |
| pagination.py       | File | Reusable pagination query parameters and paginated collection response models | Required          |
| success_response.py | File | Generic success response envelope model wrapping API payloads                 | Required          |

## 6. File Responsibilities

| File                | What It Does                                                                         | What It Contains                            | Input                           | Output                         | Called By                 | Calls    |
|---------------------|--------------------------------------------------------------------------------------|---------------------------------------------|---------------------------------|--------------------------------|---------------------------|----------|
| __init__.py         | Initializes the shared schemas package.                                              | Empty package marker.                       | None                            | None                           | Python runtime            | None     |
| error_response.py   | Defines the standard error response envelope (Placeholder — Implementation Pending). | ErrorResponse Pydantic model.               | Error details, code, request ID | Serialized error JSON schema   | Exception handlers        | Pydantic |
| pagination.py       | Defines pagination parameters and envelope (Placeholder — Implementation Pending).   | PaginationParams, PaginatedResponse models. | Page, page_size, items list     | Paginated JSON schema          | Repositories, controllers | Pydantic |
| success_response.py | Defines generic success envelope (Placeholder — Implementation Pending).             | SuccessResponse generic model.              | Data payload, message           | Serialized success JSON schema | Controllers               | Pydantic |

## 7. Allowed Responsibilities

- Declaring generic response envelope models.
- Standardizing pagination metadata models.
- Declaring error envelope structures.

## 8. Boundaries

- Must not contain module-specific domain attributes (e.g. `punch_in` belongs in Attendance schemas).
- Must not execute database queries or call services.
- Must not import SQLAlchemy models.

## 9. Connection With Other Layers

- Called by: All module controllers, integration controllers, and exception handlers.
- Calls: Pydantic framework.
- Allowed: Imported by all application layers for response typing.
- Prohibited: Reverse dependencies on domain models.

## 10. Data Flow

Enters: Raw domain payloads and metadata. Leaves: Structured, serialized response schemas.

## 11. Validation Responsibility

Validates pagination parameter bounds (e.g. `page >= 1`, `1 <= page_size <= 100`).

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Defines the data contract used by all exception handlers for error serialization.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Schemas do not perform logging or auditing.

## 16. Naming Convention

Classes: `SuccessResponse`, `ErrorResponse`, `PaginatedResponse`, `PaginationParams`.

## 17. When to Add a New File

When introducing a new standard API wrapper pattern across all endpoints.

## 18. Common Mistakes

- Adding domain-specific fields directly to shared response schemas.
- Returning un-enveloped raw dictionaries from controllers.

## 19. Related Folders

- [Shared Root](../README.md)
- [Shared Exceptions](../exceptions/README.md)
- [Attendance Schemas](../../modules/attendance/schemas/README.md)

## 20. Review Checklist

- [ ] Are response models generic where appropriate (`SuccessResponse[T]`)?
- [ ] Are pagination parameters constrained with sensible minimums and maximums?
