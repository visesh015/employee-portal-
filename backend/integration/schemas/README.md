# Integration Schemas

## 1. What Is This Folder?

Pydantic models defining composite data contracts for frontend requests and responses.

## 2. Why Do We Need It?

It provides strong typing, automatic documentation (OpenAPI), and consistent serialization for aggregated UI views.

## 3. Main Responsibility

Declaring data models for composite dashboard payloads, cross-module filters, and portal responses.

## 4. How It Works

`requests.py` defines incoming composite query parameters and filter payloads. `responses.py` defines outgoing aggregated DTOs combining attendance status, leave balances, employee metadata, and holiday lists.

## 5. What This Folder Contains

| Item         | Type | Purpose                                                                | Required/Optional |
|--------------|------|------------------------------------------------------------------------|-------------------|
| __init__.py  | File | Package initialization marker                                          | Required          |
| requests.py  | File | Pydantic models for composite incoming requests and date-range filters | Required          |
| responses.py | File | Pydantic models for serialized composite frontend views and dashboards | Required          |

## 6. File Responsibilities

| File         | What It Does                                                                                    | What It Contains           | Input                        | Output                            | Called By               | Calls    |
|--------------|-------------------------------------------------------------------------------------------------|----------------------------|------------------------------|-----------------------------------|-------------------------|----------|
| __init__.py  | Initializes the integration schemas package.                                                    | Empty package marker.      | None                         | None                              | Python runtime          | None     |
| requests.py  | Defines request validation models for composite filters (Placeholder — Implementation Pending). | Pydantic request classes.  | Incoming JSON / Query dict   | Validated Pydantic request object | Integration controllers | Pydantic |
| responses.py | Defines composite response serialization models (Placeholder — Implementation Pending).         | Pydantic response classes. | Composite service DTO / Dict | Serialized JSON payload           | Integration controllers | Pydantic |

## 7. Allowed Responsibilities

- Defining strongly typed fields, default values, and serialization rules for composite DTOs.
- Structuring dashboard cards, summary metrics, and combined status objects.

## 8. Boundaries

- Must not execute network calls or database operations.
- Must not import SQLAlchemy models.
- Must not contain business aggregation logic.

## 9. Connection With Other Layers

- Called by: Integration controllers and services.
- Calls: Pydantic framework; shared base schemas.
- Allowed: Schema definition and serialization.
- Prohibited: Dependencies on repositories or database models.

## 10. Data Flow

Enters: Raw composite data from services. Leaves: Validated, sanitized JSON-ready objects for the frontend.

## 11. Validation Responsibility

Input format and type validation; outgoing schema serialization.

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Pydantic validation errors map to standard 422 HTTP responses.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Schemas do not perform logging.

## 16. Naming Convention

Files: `requests.py`, `responses.py`. Classes: `<View>Request`, `<View>Response`.

## 17. When to Add a New File

When adding new composite UI views requiring structured data contracts.

## 18. Common Mistakes

Mixing raw internal module schemas directly into frontend response schemas without clean abstraction.

## 19. Related Folders

- [Integration Gateway Root](../README.md)
- [Integration Controllers](../controllers/README.md)
- [Shared Schemas](../../shared/schemas/README.md)

## 20. Review Checklist

- [ ] Are all schema fields strongly typed and documented?
- [ ] Do composite response models prevent leakage of internal IDs and credentials?
