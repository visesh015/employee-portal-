# Shared Helpers

## 1. What Is This Folder?

The shared helpers package contains stateless technical utility functions used across multiple modules.

## 2. Why Do We Need It?

It prevents duplicate implementations of generic operations (datetime formatting, timezone conversion, string sanitization).

## 3. Main Responsibility

Providing pure, reusable utility functions for common technical operations.

## 4. How It Works

`datetime_helper.py` provides standardized UTC timestamp generation, ISO parsing, and date arithmetic. `validation_helper.py` provides regex validation and string sanitization utilities. Functions are strictly pure and side-effect free.

## 5. What This Folder Contains

| Item                 | Type | Purpose                                                                | Required/Optional |
|----------------------|------|------------------------------------------------------------------------|-------------------|
| __init__.py          | File | Package initialization marker                                          | Required          |
| datetime_helper.py   | File | UTC timestamp helpers, ISO string parsers, and timezone converters     | Required          |
| validation_helper.py | File | Generic regex validators, email format checkers, and string sanitizers | Required          |

## 6. File Responsibilities

| File                 | What It Does                                                                     | What It Contains                 | Input                         | Output                           | Called By                       | Calls                     |
|----------------------|----------------------------------------------------------------------------------|----------------------------------|-------------------------------|----------------------------------|---------------------------------|---------------------------|
| __init__.py          | Initializes the shared helpers package.                                          | Empty package marker.            | None                          | None                             | Python runtime                  | None                      |
| datetime_helper.py   | Provides datetime manipulation functions (Placeholder — Implementation Pending). | Pure datetime utility functions. | Datetimes, strings, timezones | Formatted datetimes, UTC objects | Services, schemas, repositories | Python datetime, zoneinfo |
| validation_helper.py | Provides string and format validators (Placeholder — Implementation Pending).    | Pure validation functions.       | Raw strings, input values     | Booleans, sanitized strings      | Schemas, services               | Python re                 |

## 7. Allowed Responsibilities

- Pure mathematical, datetime, and string formatting calculations.
- Timezone normalization to UTC.
- Sanitizing user input strings.

## 8. Boundaries

- Must not contain business domain rules (Attendance/Leave rules belong in module helpers).
- Must not access databases or make network calls.
- Must not maintain mutable global state.

## 9. Connection With Other Layers

- Called by: All modules, services, repositories, and schemas.
- Calls: Python standard library.
- Allowed: Inbound utility calls from any layer.
- Prohibited: Outbound calls to databases or external APIs.

## 10. Data Flow

Enters: Raw values and parameters. Leaves: Normalized, validated, or transformed values.

## 11. Validation Responsibility

Validates primitive data formats and structures.

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Raises standard Python exceptions (ValueError, TypeError) on invalid inputs.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Helpers do not perform logging or auditing.

## 16. Naming Convention

Files: `<utility>_helper.py`. Functions: `snake_case`.

## 17. When to Add a New File

When a generic technical utility function is needed by two or more independent modules.

## 18. Common Mistakes

- Placing business rules (e.g. shift overtime calculations) in shared helpers instead of module helpers.
- Importing domain modules into shared helpers.

## 19. Related Folders

- [Shared Root](../README.md)
- [Attendance Helpers](../../modules/attendance/helpers/README.md)

## 20. Review Checklist

- [ ] Are all functions strictly stateless and pure?
- [ ] Is there zero domain-specific business logic in these utilities?
