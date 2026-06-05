---
name: backend-integrity
description: Enforces backend as source of truth; transactional integrity, security boundaries, API contract stability, proper layering
type: agent
---

# Skill: backend-integrity

# Core Directive

Preserve transactional integrity, backend ownership, and source of truth.
The backend is the authoritative layer — data validation, business logic, and permissions live here.

# Layer Boundaries

```
Client (FE) → Controller (routing/validation) → Service (business logic) → Repository (data access)
```

- **Controllers**: parse request, validate input (DTO), call service, return response
- **Services**: business logic, orchestration, transactional boundaries
- **Repositories**: data access, query building, ORM operations
- **Entities**: domain model, relationships, database constraints

# Source of Truth Rules

1. **Database is the source of truth for data**
   - Computed fields belong in queries or services, not in storage
   - Denormalization must be intentional and documented

2. **Backend is the source of truth for logic**
   - Business rules must be enforced server-side, never trust the client
   - FE validation is for UX, BE validation is for security

3. **DTOs are the source of truth for API contracts**
   - Never expose entities directly through the API
   - Use response DTOs to shape what the client receives
   - Use input DTOs to define what the client must send

# Transactional Integrity

- Use database transactions for multi-step operations
- If step 2 fails, step 1 must roll back
- Service methods handling multiple repository calls must be wrapped in a transaction
- Pessimistic locking for concurrent access to critical resources

# Security Constraints

- All endpoints require authentication unless explicitly marked public
- Authorization checks happen at the service layer, not the controller
- Input validation: whitelist allowed values, never blacklist
- SQL injection prevention: use parameterized queries / ORM (never string concatenation)
- Rate limiting on sensitive endpoints (auth, password reset, payment)

# API Contract Stability

- Version endpoints when breaking changes are unavoidable (`/api/v2/`)
- Never remove a field from a response DTO — deprecate first
- Never change the type of an existing field — add a new field instead
- Document all public endpoints with OpenAPI/Swagger

# Error Handling

- Use typed exceptions with HTTP status codes (4xx for client errors, 5xx for server errors)
- Never expose stack traces or internal details in production error responses
- Log errors server-side with context (request ID, user ID, action)
- External service failures must be caught and wrapped in domain-specific errors
