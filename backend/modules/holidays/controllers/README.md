# Holidays Controllers

## 1. What Is This Folder?

The `controllers` layer for Holidays. FastAPI route handlers receiving HTTP requests and delegating to services.

## 2. Why Do We Need It?

It isolates controllers concerns for the Holidays domain following the project's standard layered architecture.

## 3. Main Responsibility

Handling controllers responsibilities for Holidays.

## 4. How It Works

This layer follows the canonical design established in the Attendance module. For a detailed, complete explanation of how this layer works, responsibilities, and data flow, see the [Canonical Controllers Reference](../../attendance/controllers/README.md).

## 5. What This Folder Contains

| Item        | Type | Purpose                       | Required/Optional |
|-------------|------|-------------------------------|-------------------|
| __init__.py | File | Package initialization marker | Required          |

## 6. File Responsibilities

| File        | What It Does                                                                         | What It Contains      | Input | Output | Called By      | Calls |
|-------------|--------------------------------------------------------------------------------------|-----------------------|-------|--------|----------------|-------|
| __init__.py | Initializes the Holidays controllers package (Placeholder — Implementation Pending). | Empty package marker. | None  | None   | Python runtime | None  |

## 7. Allowed Responsibilities

Implementing controllers components strictly for Holidays.

## 8. Boundaries

Must follow standard controllers layer boundaries. See [Canonical Reference](../../attendance/controllers/README.md#8-boundaries).

## 9. Connection With Other Layers

- Follows standard layer hierarchy.
- See [Canonical Controllers Reference](../../attendance/controllers/README.md#9-connection-with-other-layers).

## 10. Data Flow

See [Canonical Controllers Reference](../../attendance/controllers/README.md#10-data-flow).

## 11. Validation Responsibility

See [Canonical Controllers Reference](../../attendance/controllers/README.md#11-validation-responsibility).

## 12. Authentication and Authorization

See [Canonical Controllers Reference](../../attendance/controllers/README.md#12-authentication-and-authorization).

## 13. Error Handling

See [Canonical Controllers Reference](../../attendance/controllers/README.md#13-error-handling).

## 14. Transaction Responsibility

Requires Team Confirmation.

## 15. Logging and Auditing

See [Canonical Controllers Reference](../../attendance/controllers/README.md#15-logging-and-auditing).

## 16. Naming Convention

See [Canonical Controllers Reference](../../attendance/controllers/README.md#16-naming-convention).

## 17. When to Add a New File

When implementing domain-specific controllers files for Holidays.

## 18. Common Mistakes

See [Canonical Controllers Reference](../../attendance/controllers/README.md#18-common-mistakes).

## 19. Related Folders

- [Holidays Module Root](../README.md)
- [Canonical Attendance Controllers](../../attendance/controllers/README.md)
- [Architecture Overview](../../../docs/architecture.md)

## 20. Review Checklist

- [ ] Does this layer strictly follow the canonical Attendance pattern?
- [ ] Are all dependencies cleanly mapped and decoupled?
