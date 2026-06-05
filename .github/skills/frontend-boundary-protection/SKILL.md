---
name: frontend-boundary-protection
description: Prevents logic contamination in the frontend; business logic belongs in the API, FE is presentation only
type: agent
---

# Skill: frontend-boundary-protection

# Core Directive

Prevent logic contamination in the frontend and preserve API contracts.
The frontend is a presentation layer — business logic belongs in the backend.

# Hard Boundaries

## Frontend MUST NOT
- Implement business rules that should be server-enforced
- Duplicate backend validation logic (validation is UX-only in FE)
- Store or cache sensitive data (tokens, PII) in insecure storage
- Build URLs or API paths manually — use services
- Compute derived data that the API already provides
- Call multiple endpoints to aggregate what a single endpoint should return
- Hardcode values that come from the API (enums, constants, configurations)

## Frontend MUST
- Display server errors as-is (with i18n if available)
- Show loading states for all async operations
- Handle network failures gracefully (retry, timeout error UI)
- Use TypeScript interfaces matching the API response shape
- Validate form inputs for UX purposes only (required fields, format hints)

# API Contract Alignment

- Every API call goes through a service layer (API service, feature-specific services)
- Response types mirror the API contract (DTOs/interfaces in a shared types file)
- Request types mirror the API input DTOs
- Never assume a field will be present — handle optional fields with `?`
- Never assume a field exists in the response — use optional chaining `?.`

# State Management Rules

- Server state belongs in API calls, not in client stores
- Cache invalidation: re-fetch after mutations
- Optimistic updates are opt-in and must include rollback on error
- URL/search params are the source of truth for page state

# Component Architecture

- Components receive data via inputs, emit events via outputs (container/presentational pattern)
- Pages/smart components orchestrate data fetching
- Presentational components are pure — they do not call services
- Shared components belong in a `shared/` module, feature components in `features/`

# Testing Boundaries

- Mock API responses, never call real endpoints in unit tests
- Test UI states: loading, empty, error, success
- Test form validation feedback, not the validation logic itself
- Test that error messages from the API are displayed correctly
