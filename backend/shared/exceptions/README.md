# Shared Exceptions

## 1. What Is This Folder?

The exceptions package defines the standard domain exception hierarchy and global FastAPI exception handlers.

## 2. Why Do We Need It?

It guarantees that errors occurring anywhere in the application are translated into uniform, predictable, and secure JSON responses.

## 3. Main Responsibility

Defining `BaseCustomException` and registering global exception handlers that output standard `ErrorResponse` schemas.

## 4. How It Works

`base_exception.py` defines standard exception classes (`EntityNotFoundException`, `ConflictException`, `UnauthorizedException`, `ForbiddenException`, `ValidationException`). `exception_handlers.py` registers handlers on the FastAPI app instance to catch these exceptions and convert them into structured HTTP response envelopes with sanitized error messages and correlation IDs.

## 5. What This Folder Contains

| Item                  | Type | Purpose                                                                          | Required/Optional |
|-----------------------|------|----------------------------------------------------------------------------------|-------------------|
| __init__.py           | File | Package initialization marker                                                    | Required          |
| base_exception.py     | File | Root domain exception classes with error codes and HTTP status mappings          | Required          |
| exception_handlers.py | File | FastAPI global exception handlers converting exceptions to ErrorResponse schemas | Required          |

## 6. File Responsibilities

| File                  | What It Does                                                                      | What It Contains                             | Input                               | Output                                 | Called By                    | Calls            |
|-----------------------|-----------------------------------------------------------------------------------|----------------------------------------------|-------------------------------------|----------------------------------------|------------------------------|------------------|
| __init__.py           | Initializes the exceptions package.                                               | Empty package marker.                        | None                                | None                                   | Python runtime               | None             |
| base_exception.py     | Defines the base domain exception classes (Placeholder — Implementation Pending). | BaseCustomException and standard subclasses. | Error message, status code, details | Exception class instances              | Domain services, controllers | Python Exception |
| exception_handlers.py | Registers global exception handlers (Placeholder — Implementation Pending).       | FastAPI exception handler functions.         | Raised exceptions, HTTP request     | JSONResponse with ErrorResponse schema | FastAPI app main             | Shared Schemas   |

## 7. Allowed Responsibilities

- Declaring application-wide exception types with standardized error codes.
- Converting caught exceptions into structured HTTP JSON responses.
- Sanitizing unexpected exceptions to prevent internal implementation leaks.

## 8. Boundaries

- Must not expose raw SQL errors, stack traces, or internal file paths in responses.
- Must not leak passwords, tokens, or personal identifiers in error details.
- Must not handle business workflow state transitions.

## 9. Connection With Other Layers

- Called by: Domain services, controllers, middleware, and FastAPI runtime.
- Calls: Shared schemas (`backend/shared/schemas/error_response.py`).
- Allowed: Raising standard exceptions across all layers.
- Prohibited: Importing business modules.

## 10. Data Flow

Enters: Raised exception instances and request context. Leaves: Serialized HTTP error response JSON.

## 11. Validation Responsibility

Translates Pydantic `RequestValidationError` into user-friendly validation error envelopes.

## 12. Authentication and Authorization

Translates authentication failures into HTTP 401 and permission denials into HTTP 403.

## 13. Error Handling

The central destination for all application error translation.

## 14. Transaction Responsibility

Triggers automatic rollback in database session dependencies when an exception bubbles to handlers.

## 15. Logging and Auditing

Logs technical error details, stack traces, and correlation IDs to the technical log.

## 16. Naming Convention

Classes: `<Name>Exception` (e.g. `RecordNotFoundException`, `BusinessRuleException`).

## 17. When to Add a New File

When a new standard category of technical or domain error is identified.

## 18. Common Mistakes

- Returning custom error JSON dictionaries directly from endpoints instead of raising exceptions.
- Exposing internal database error messages to frontend callers.

## 19. Related Folders

- [Shared Root](../README.md)
- [Shared Schemas](../schemas/README.md)
- [Attendance Exceptions](../../modules/attendance/exceptions.py)

## 20. Review Checklist

- [ ] Do all custom exceptions inherit from `BaseCustomException`?
- [ ] Are stack traces strictly excluded from HTTP response envelopes?
