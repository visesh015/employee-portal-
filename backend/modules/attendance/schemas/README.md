# Attendance Schemas

## 1. What Is This Folder?

The schemas layer contains Pydantic models defining the data contracts for Attendance requests and responses.

## 2. Why Do We Need It?

It provides automated request validation, type enforcement, data sanitization, and structured API response serialization.

## 3. Main Responsibility

Defining validation rules for incoming payloads (deserialization) and formatting outgoing data structures (serialization).

## 4. How It Works

Pydantic models in `requests.py` inspect incoming HTTP request bodies, ensuring required fields are present, dates are valid, and values match expected formats. Pydantic models in `responses.py` serialize internal service and repository outputs into consistent, frontend-ready JSON structures, stripping internal database fields.

## 5. What This Folder Contains

| Item         | Type | Purpose                                                          | Required/Optional |
|--------------|------|------------------------------------------------------------------|-------------------|
| __init__.py  | File | Package initialization marker                                    | Required          |
| requests.py  | File | Pydantic models for incoming request bodies and query parameters | Required          |
| responses.py | File | Pydantic models for serialized outgoing API responses            | Required          |

## 6. File Responsibilities

| File         | What It Does                                                       | What It Contains           | Input                      | Output                            | Called By              | Calls    |
|--------------|--------------------------------------------------------------------|----------------------------|----------------------------|-----------------------------------|------------------------|----------|
| __init__.py  | Initializes the schemas package.                                   | Empty package marker.      | None                       | None                              | Python runtime         | None     |
| requests.py  | Defines request validation models for clocking and regularization. | Pydantic request classes.  | Incoming JSON / Query dict | Validated Pydantic request object | Attendance controllers | Pydantic |
| responses.py | Defines response serialization models for attendance data.         | Pydantic response classes. | Domain entity / Dict       | Serialized JSON-compatible dict   | Attendance controllers | Pydantic |

## 7. Allowed Responsibilities

- Defining field types, constraints, default values, and custom validators.
- Declaring request payload contracts (`ClockInRequest`, `RegularizationSubmitRequest`).
- Declaring response structure contracts (`AttendanceResponse`, `RegularizationResponse`).

## 8. Boundaries

- Must not execute database queries or call external APIs.
- Must not contain business state transitions or authorization logic.
- Must not import SQLAlchemy ORM models directly into request schemas.

## 9. Connection With Other Layers

- Called by: Attendance controllers (`backend/modules/attendance/controllers`).
- Calls: Pydantic framework; shared base schemas (`backend/shared/schemas`).
- Allowed: Schema inheritance and field validation.
- Prohibited: Dependencies on services, repositories, or database models.

## 10. Data Flow

Deserialization Flow: Frontend JSON → validated Python Pydantic request object → Controller.
Serialization Flow: Service result entity → Pydantic response schema → Frontend JSON.

## 11. Validation Responsibility

Strict format, type, and field-level constraint validation (e.g. date format, string length, enum values).

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Raises Pydantic `ValidationError` on malformed inputs, which FastAPI converts to HTTP 422 Unprocessable Entity.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Schemas do not perform logging or auditing.

## 16. Naming Convention

Files: `requests.py`, `responses.py`. Classes: `<Action>Request`, `<Entity>Response`.

## 17. When to Add a New File

When adding new endpoint contracts or modifying existing request/response structures.

## 18. Common Mistakes

- Putting business validation rules inside Pydantic field validators.
- Exposing internal database column names or sensitive audit keys directly in response schemas.

## 19. Related Folders

- [Attendance Module Root](../README.md)
- [Attendance Controllers](../controllers/README.md)
- [Shared Schemas](../../../shared/schemas/README.md)

## 20. Review Checklist

- [ ] Are all schema fields strongly typed?
- [ ] Do response schemas use `from_attributes = True` for ORM compatibility?
- [ ] Are sensitive internal fields excluded from response schemas?
