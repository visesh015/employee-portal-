# Documentation Root

## 1. What Is This Folder?

The central architecture, API standards, and development guidelines repository for the backend.

## 2. Why Do We Need It?

It provides authoritative engineering specifications, preventing architecture drift and onboarding new developers rapidly.

## 3. Main Responsibility

Housing architectural blueprints, database ownership rules, integration contracts, and the documentation manifest.

## 4. How It Works

Documentation files specify coding rules, architectural boundaries, API conventions, and database ownership principles. The `documentation-manifest.md` catalogs every folder and file in the project with its review status.

## 5. What This Folder Contains

| Item                      | Type | Purpose                                                                         | Required/Optional |
|---------------------------|------|---------------------------------------------------------------------------------|-------------------|
| api-standards.md          | File | RESTful API design standards, status codes, and error formatting rules          | Required          |
| architecture.md           | File | High-level architecture overview, modular monolith design, and request journeys | Required          |
| database-ownership.md     | File | Data isolation principles, single-database modular ownership rules              | Required          |
| development-rules.md      | File | Strict developer guidelines, boundary rules, and non-negotiables                | Required          |
| documentation-manifest.md | File | Comprehensive documentation inventory cataloging all folders and files          | Required          |
| integration-contracts.md  | File | Contracts and aggregation specifications for the Integration Gateway            | Required          |

## 6. File Responsibilities

| File                      | What It Does                                                                     | What It Contains   | Input           | Output          | Called By             | Calls |
|---------------------------|----------------------------------------------------------------------------------|--------------------|-----------------|-----------------|-----------------------|-------|
| api-standards.md          | Documents REST API design standards (Placeholder — Implementation Pending).      | Empty placeholder. | None            | Documentation   | Developers            | None  |
| architecture.md           | Documents backend system architecture (Placeholder — Implementation Pending).    | Empty placeholder. | None            | Documentation   | Developers            | None  |
| database-ownership.md     | Documents database table ownership rules (Placeholder — Implementation Pending). | Empty placeholder. | None            | Documentation   | Developers            | None  |
| development-rules.md      | Documents strict engineering boundaries (Placeholder — Implementation Pending).  | Empty placeholder. | None            | Documentation   | Developers            | None  |
| documentation-manifest.md | Catalogs all project folders, files, README locations, and statuses.             | Inventory table.   | Repository scan | Status manifest | Developers, Reviewers | None  |
| integration-contracts.md  | Documents gateway aggregation contracts (Placeholder — Implementation Pending).  | Empty placeholder. | None            | Documentation   | Developers            | None  |

## 7. Allowed Responsibilities

Authoring and maintaining Markdown technical specifications.

## 8. Boundaries

Must not contain executable source code, credentials, or internal secrets.

## 9. Connection With Other Layers

- Referenced by: All module READMEs and developers.
- References: Entire repository structure.

## 10. Data Flow

Static architectural guidance.

## 11. Validation Responsibility

Documentation review against repository reality.

## 12. Authentication and Authorization

No security decisions.

## 13. Error Handling

Not applicable.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Not applicable.

## 16. Naming Convention

Files: `kebab-case.md`.

## 17. When to Add a New File

When documenting a new system-wide design pattern, standard, or integration contract.

## 18. Common Mistakes

Letting documentation diverge from the actual repository folder structure and code.

## 19. Related Folders

- [Backend Root](../README.md)
- [Attendance Canonical Reference](../modules/attendance/README.md)
- [Integration Gateway](../integration/README.md)

## 20. Review Checklist

- [ ] Are all document links valid relative links?
- [ ] Does `documentation-manifest.md` include every tracked file and folder?
