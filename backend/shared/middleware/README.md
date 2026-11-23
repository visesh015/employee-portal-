# Shared Middleware

## 1. What Is This Folder?

The middleware package contains HTTP request interceptors that process cross-cutting concerns for all incoming requests.

## 2. Why Do We Need It?

It decouples transport-level concerns (request correlation IDs, timing, global access logging, unhandled exception safety) from application endpoints.

## 3. Main Responsibility

Intercepting HTTP requests, assigning correlation IDs, logging request/response metrics, and catching unhandled server panics.

## 4. How It Works

`request_context.py` extracts or generates an `X-Request-ID` and stores it in context variables for downstream logging. `access_logging.py` records incoming method, path, client IP, latency, and response status code. `error_middleware.py` acts as a fail-safe boundary, intercepting unexpected 500 exceptions and returning sanitized error envelopes.

Key Architectural Principle:
`Middleware processes the request journey. Service performs the business job.`

## 5. What This Folder Contains

| Item                | Type | Purpose                                                                      | Required/Optional |
|---------------------|------|------------------------------------------------------------------------------|-------------------|
| __init__.py         | File | Package initialization marker                                                | Required          |
| access_logging.py   | File | HTTP access logger recording request method, path, duration, and status      | Required          |
| error_middleware.py | File | Top-level exception boundary converting unhandled server errors to safe JSON | Required          |
| request_context.py  | File | Request correlation ID management using Python contextvars                   | Required          |

## 6. File Responsibilities

| File                | What It Does                                                                                | What It Contains                    | Input                | Output                          | Called By        | Calls             |
|---------------------|---------------------------------------------------------------------------------------------|-------------------------------------|----------------------|---------------------------------|------------------|-------------------|
| __init__.py         | Initializes the middleware package.                                                         | Empty package marker.               | None                 | None                            | Python runtime   | None              |
| access_logging.py   | Logs HTTP request timing and access details (Placeholder — Implementation Pending).         | FastAPI/Starlette middleware class. | HTTP Request         | HTTP Response with timing       | FastAPI pipeline | Shared Logger     |
| error_middleware.py | Catches unhandled errors and ensures safe responses (Placeholder — Implementation Pending). | FastAPI/Starlette middleware class. | HTTP Request         | Safe HTTP Response              | FastAPI pipeline | Shared Exceptions |
| request_context.py  | Attaches correlation IDs to requests (Placeholder — Implementation Pending).                | FastAPI/Starlette middleware class. | HTTP Request headers | ContextVar set, response header | FastAPI pipeline | contextvars       |

## 7. Allowed Responsibilities

- Injecting and forwarding `X-Request-ID` and correlation headers.
- Measuring request execution time and logging access metrics.
- Enforcing CORS headers and security headers.

## 8. Boundaries

- Must not execute database queries or call repositories.
- Must not enforce domain business rules or state transitions.
- Must not perform module-specific authorization decisions.

## 9. Connection With Other Layers

- Called by: ASGI server (Uvicorn) / FastAPI application pipeline.
- Calls: Shared logger and context storage.
- Allowed: Intercepting incoming requests and outgoing responses.
- Prohibited: Calling domain services, repositories, or models.

## 10. Data Flow

Enters: Raw HTTP request stream. Leaves: Processed request with context, outgoing response with headers.

## 11. Validation Responsibility

Validates the presence and format of required request headers.

## 12. Authentication and Authorization

Extracts raw bearer tokens into context, but does not perform domain permission checks.

## 13. Error Handling

Catches unhandled runtime panics and converts them into standardized 500 responses with correlation IDs.

## 14. Transaction Responsibility

Must not manage database transactions.

## 15. Logging and Auditing

Technical access logging of every HTTP request, latency, and status code.

## 16. Naming Convention

Files: `<concern>_middleware.py` or `<concern>.py`. Classes: `<Concern>Middleware`.

## 17. When to Add a New File

When introducing a new global HTTP transport concern (e.g. rate limiting headers, compression).

## 18. Common Mistakes

- Attempting to perform database queries or user permission checks in middleware.
- Modifying request payload bodies in a way that breaks downstream Pydantic validation.

## 19. Related Folders

- [Shared Root](../README.md)
- [Shared Exceptions](../exceptions/README.md)
- [Shared Config](../config/README.md)

## 20. Review Checklist

- [ ] Does middleware strictly avoid database queries and business rules?
- [ ] Is `X-Request-ID` propagated onto all outgoing response headers?
