# Backend Modules Root

## 1. What Is This Folder?

The `modules` directory contains all autonomous business domain modules of the Employee Management Portal.

## 2. Why Do We Need It?

It establishes a modular monolith architecture where each business capability is encapsulated within its own bounded context, preventing tight coupling and spaghetti code.

## 3. Main Responsibility

Hosting domain modules (Attendance, Employees, Leave, Holidays, WFH, Policies, Engagement) with strict logical isolation, explicit contracts, and independent layered structures.

## 4. How It Works

Each subdirectory in `modules` represents a distinct domain service containing its own controllers, services, repositories, models, schemas, clients, and helpers. Modules communicate with each other exclusively via internal HTTP client interfaces or integration aggregations, never through direct database queries or cross-module model imports.

## 5. What This Folder Contains

| Item        | Type   | Purpose                                                                                                 | Required/Optional |
|-------------|--------|---------------------------------------------------------------------------------------------------------|-------------------|
| __init__.py | File   | Python package initialization marker                                                                    | Required          |
| attendance  | Folder | Canonical reference module for time tracking, clock-in/out, and regularization                          | Required          |
| employees   | Folder | Domain module for employee profile management and organizational structure                              | Required          |
| leave       | Folder | Domain module for leave requests, quotas, and approval workflows                                        | Required          |
| holidays    | Folder | Domain module for official company holidays and regional calendars                                      | Required          |
| wfh         | Folder | Domain module for work-from-home requests, allocations, and approvals                                   | Required          |
| Policies    | Folder | Module 6: Company policy and compliance management (Pending Bunny Review / Requires Team Confirmation)  | Optional          |
| engagement  | Folder | Module 7: Employee engagement and survey management (Pending Bunny Review / Requires Team Confirmation) | Optional          |

## 6. File Responsibilities

| File        | What It Does                                           | What It Contains      | Input | Output | Called By            | Calls |
|-------------|--------------------------------------------------------|-----------------------|-------|--------|----------------------|-------|
| __init__.py | Initializes the modules directory as a Python package. | Empty package marker. | None  | None   | Python import system | None  |

## 7. Allowed Responsibilities

Adding new self-contained business domain modules following the established 7-layer architecture.

## 8. Boundaries

No direct cross-module database access. No shared business logic placed in the modules root. No circular dependencies.

## 9. Connection With Other Layers

- Called by: Integration gateway and main application assembly.
- Calls: Shared components (`backend/shared`).
- Allowed: Downstream calls from Integration to Modules, and Modules to Shared.
- Prohibited: Cross-module direct database imports; circular module-to-module dependencies.

## 10. Data Flow

Receives decomposed business requests from Integration or direct module routes; returns domain-specific entity responses.

## 11. Validation Responsibility

Format validation occurs in each module's schemas; business validation occurs in module services.

## 12. Authentication and Authorization

Authentication and authorization dependencies are applied per module router and service layer.

## 13. Error Handling

Each module defines custom domain exceptions in `exceptions.py` that map to shared standard HTTP error envelopes.

## 14. Transaction Responsibility

Managed within individual module services and repositories. Requires Team Confirmation for cross-module distributed transactions.

## 15. Logging and Auditing

Technical logging via shared middleware and logger; business auditing via shared audit service within module services.

## 16. Naming Convention

Folders: lowercase domain names (e.g. `attendance`, `employees`). Policies capitalized per existing scaffolding.

## 17. When to Add a New File

Only when an approved, distinct new business domain capability is introduced by architectural review.

## 18. Common Mistakes

Importing models or repositories across different modules; placing business logic directly in the root `modules` directory.

## 19. Related Folders

- [Backend Root](../README.md)
- [Attendance Module](attendance/README.md)
- [Integration Gateway](../integration/README.md)
- [Shared Components](../shared/README.md)
- [Documentation Manifest](../docs/documentation-manifest.md)

## 20. Review Checklist

- [ ] Does the new module adhere to the 7-layer structure?
- [ ] Are database models strictly isolated to this module?
- [ ] Is cross-module communication routed through clients?
