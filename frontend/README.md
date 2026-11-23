# Frontend Root

## 1. What Is This Folder?

The presentation layer workspace containing the client-side user interface application for the Employee Management Portal.

## 2. Why Do We Need It?

It provides the graphical user interface for employees, managers, HR specialists, and system administrators.

## 3. Main Responsibility

Rendering portal UI screens, handling user input, managing client-side navigation, and calling the backend Integration Gateway.

## 4. How It Works

The frontend application interacts exclusively with the backend Integration Gateway (`backend/integration`). It does not communicate directly with internal business domain modules or the primary database.

## 5. What This Folder Contains

| Item     | Type | Purpose                            | Required/Optional |
|----------|------|------------------------------------|-------------------|
| .gitkeep | File | Directory preservation placeholder | Required          |

## 6. File Responsibilities

| File     | What It Does                                         | What It Contains        | Input | Output | Called By | Calls |
|----------|------------------------------------------------------|-------------------------|-------|--------|-----------|-------|
| .gitkeep | Preserves the frontend directory in version control. | Empty placeholder file. | None  | None   | Git VCS   | None  |

## 7. Allowed Responsibilities

Client-side UI components, state management, styling, and API client consumption.

## 8. Boundaries

- Must not connect directly to internal module databases.
- Must not bypass the backend Integration Gateway.
- Must not embed server-side secrets or private API keys.

## 9. Connection With Other Layers

- Called by: End-user web browsers.
- Calls: Backend Integration Gateway (`backend/integration`).

## 10. Data Flow

Enters: User interactions and clicks. Leaves: API calls to `/api/v1/integration/*`.

## 11. Validation Responsibility

Client-side form validation for user convenience (server-side schemas remain authoritative).

## 12. Authentication and Authorization

Manages user login state and attaches Firebase/JWT bearer tokens to outbound HTTP requests.

## 13. Error Handling

Displays user-friendly notification banners and form errors based on backend `ErrorResponse` payloads.

## 14. Transaction Responsibility

No database transaction responsibility.

## 15. Logging and Auditing

Client-side console telemetry and operational error reporting.

## 16. Naming Convention

Follows frontend framework standards.

## 17. When to Add a New File

When adding UI components and client-side view logic.

## 18. Common Mistakes

Attempting to bypass the Integration gateway to query internal modules directly.

## 19. Related Folders

- [Repository Root](../README.md)
- [Backend Integration Gateway](../backend/integration/README.md)

## 20. Review Checklist

- [ ] Does the frontend communicate strictly through the Integration Gateway?
- [ ] Are all user inputs validated on the client before submission?
