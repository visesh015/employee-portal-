# Shared Security

## 1. What Is This Folder?

The security package provides authentication, authorization, and cryptographic token verification infrastructure.

## 2. Why Do We Need It?

It provides a centralized, hardened implementation of identity verification (Firebase/JWT) and role-based access control.

## 3. Main Responsibility

Extracting bearer tokens, verifying signatures, creating authenticated user context objects, and enforcing role/scope requirements.

## 4. How It Works

`jwt_handler.py` verifies Firebase/JWT tokens, decodes claims, and validates expiration. `authentication.py` provides `get_current_user` FastAPI dependency for injecting authenticated principal details. `authorization.py` provides `require_role(allowed_roles)` and `require_scope(required_scopes)` dependencies.

Three-Level Security Architecture:
1. Middleware Level: Correlation ID injection, request context, and common transport validation.
2. Dependency Level: Token signature verification, tenant extraction, and route-level role/scope checks (401 for invalid auth, 403 for insufficient roles).
3. Service Level: Resource ownership and domain authorization (e.g. verifying employee owns the record being regularized).

## 5. What This Folder Contains

| Item              | Type | Purpose                                                                  | Required/Optional |
|-------------------|------|--------------------------------------------------------------------------|-------------------|
| __init__.py       | File | Package initialization marker                                            | Required          |
| authentication.py | File | FastAPI dependencies for extracting and verifying the authenticated user | Required          |
| authorization.py  | File | Role-based and scope-based access control dependencies                   | Required          |
| jwt_handler.py    | File | Cryptographic verification and decoding of JWT and Firebase auth tokens  | Required          |

## 6. File Responsibilities

| File              | What It Does                                                                          | What It Contains                      | Input                            | Output                             | Called By                 | Calls                  |
|-------------------|---------------------------------------------------------------------------------------|---------------------------------------|----------------------------------|------------------------------------|---------------------------|------------------------|
| __init__.py       | Initializes the shared security package.                                              | Empty package marker.                 | None                             | None                               | Python runtime            | None                   |
| authentication.py | Provides get_current_user dependency (Placeholder — Implementation Pending).          | FastAPI authentication dependencies.  | HTTP Authorization header        | AuthenticatedUser context object   | Controllers, Dependencies | jwt_handler.py         |
| authorization.py  | Provides role and scope checking dependencies (Placeholder — Implementation Pending). | Role and scope enforcement functions. | AuthenticatedUser, Allowed roles | AuthenticatedUser or 403 exception | Controllers, Routers      | Shared Exceptions      |
| jwt_handler.py    | Decodes and verifies JWT tokens (Placeholder — Implementation Pending).               | Token verification utilities.         | Raw JWT token string             | Decoded token claims dictionary    | authentication.py         | PyJWT / Firebase Admin |

## 7. Allowed Responsibilities

- Cryptographic token decoding and signature verification.
- Extracting user identity, tenant ID, and assigned roles from token claims.
- Providing route-level authorization guards for controllers.

## 8. Boundaries

- Must not query domain-specific business tables.
- Must not hardcode private cryptographic keys or Firebase credentials.
- Must not make domain business decisions (e.g. approving a leave request).

## 9. Connection With Other Layers

- Called by: All controller route dependencies across modules and Integration.
- Calls: Shared config (`backend/shared/config`), shared exceptions.
- Allowed: Inbound security dependency calls from all controllers.
- Prohibited: Dependencies on domain business modules.

## 10. Data Flow

Enters: Authorization HTTP headers, bearer tokens. Leaves: `AuthenticatedUser` principal context objects.

## 11. Validation Responsibility

Validates token structure, cryptographic signatures, issuer, audience, and expiration.

## 12. Authentication and Authorization

- Authentication: Who is the caller? (401 Unauthorized if token is missing, expired, or invalid).
- Authorization: What may the caller do? (403 Forbidden if user lacks required role/scope).

## 13. Error Handling

Raises `UnauthorizedException` (401) on invalid tokens and `ForbiddenException` (403) on permission failure.

## 14. Transaction Responsibility

No transaction responsibility.

## 15. Logging and Auditing

Technical logging of authentication failures and expired token attempts. Never logs raw token strings.

## 16. Naming Convention

Classes: `AuthenticatedUser`, `Role`, `Scope`. Dependencies: `get_current_user`, `require_role`.

## 17. When to Add a New File

When adding new authentication providers (e.g. SSO/SAML) or fine-grained policy evaluation engines.

## 18. Common Mistakes

- Conflating 401 Unauthorized (unauthenticated) with 403 Forbidden (authenticated but not allowed).
- Hardcoding secret keys or ignoring token expiration checks.

## 19. Related Folders

- [Shared Root](../README.md)
- [Shared Config](../config/README.md)
- [Shared Exceptions](../exceptions/README.md)
- [Integration Gateway](../../integration/README.md)

## 20. Review Checklist

- [ ] Are token expiration and audience claims strictly verified?
- [ ] Are raw tokens and secrets masked in all log outputs?
