# Employee Management Portal Root

## 1. What Is This Folder?

The root workspace directory of the Employee Management Portal, structured as a Service-Based Modular Monorepo with an Integration Gateway, Shared Technical Components, and One Primary Database.

## 2. Why Do We Need It?

It houses the entire enterprise employee portal system, encompassing the frontend application and backend modular services in a single coordinated repository.

## 3. Main Responsibility

Providing repository-level configuration, workspace orchestration, dependency segregation, and architectural integrity.

## 4. How It Works

The repository is partitioned into `frontend/` (user interface presentation layer) and `backend/` (FastAPI modular backend). Requests originate from the frontend, traverse the backend Integration Gateway (`backend/integration`), delegate to autonomous domain modules (`backend/modules`), utilize shared infrastructure (`backend/shared`), and persist to a single primary PostgreSQL database.

## 5. What This Folder Contains

| Item       | Type   | Purpose                                                                         | Required/Optional |
|------------|--------|---------------------------------------------------------------------------------|-------------------|
| .gitignore | File   | Git ignore patterns for Python, node_modules, IDEs, and local environment files | Required          |
| frontend   | Folder | Presentation layer application for web and mobile browsers                      | Required          |
| backend    | Folder | Service-based modular monorepo backend application                              | Required          |

## 6. File Responsibilities

| File       | What It Does                                       | What It Contains                                                | Input        | Output            | Called By | Calls |
|------------|----------------------------------------------------|-----------------------------------------------------------------|--------------|-------------------|-----------|-------|
| .gitignore | Specifies intentionally untracked files to ignore. | Ignore patterns for caches, virtualenvs, .env, build artifacts. | Git commands | Ignored file list | Git VCS   | None  |

## 7. Allowed Responsibilities

- Repository-level version control configuration (`.gitignore`).
- Root-level documentation and architectural specifications.
- Workspace-level orchestration scripts.

## 8. Boundaries

- Must not contain application source code directly in the root.
- Must not contain committed secrets, passwords, or local `.env` files.

## 9. Connection With Other Layers

- Contains: `frontend/`, `backend/`.
- Outward: Deployed to cloud infrastructure.

## 10. Data Flow

Frontend UI → Integration Gateway → Domain Modules → PostgreSQL Database.

## 11. Validation Responsibility

Segregated by layer (schemas at boundary, business rules in services).

## 12. Authentication and Authorization

Firebase / JWT authentication enforced at backend gateway and modules.

## 13. Error Handling

Handled uniformly across layers using standard JSON error envelopes.

## 14. Transaction Responsibility

Managed at backend service boundaries. Primary database for V1.

## 15. Logging and Auditing

Technical logging via shared middleware; business auditing via shared audit service.

## 16. Naming Convention

Directories: lowercase standard names (`frontend`, `backend`).

## 17. When to Add a New File

Root-level additions require enterprise architectural approval.

## 18. Common Mistakes

Placing loose script or configuration files directly into the repository root.

## 19. Related Folders

- [Backend Root](backend/README.md)
- [Frontend Root](frontend/README.md)
- [Documentation Manifest](backend/docs/documentation-manifest.md)

## 20. Review Checklist

- [ ] Are all source files located strictly within `frontend/` or `backend/`?
- [ ] Is `.gitignore` properly protecting against accidental commit of secrets?
