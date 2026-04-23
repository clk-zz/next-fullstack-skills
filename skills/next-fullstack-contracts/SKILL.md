---
name: next-fullstack-contracts
description: Shared frontend and backend contracts for Next.js projects, including DTO shape, API envelope consumption, shared validation schemas, pagination contracts, error-code handling, and cache invalidation coordination. Use only when the task explicitly spans both frontend and backend behavior, such as aligning App Router pages with route handlers, defining typed client-to-server flows, standardizing DTOs, mapping backend error codes to UI behavior, or coordinating cache invalidation after mutations. Do not use for frontend-only or backend-only tasks unless cross-layer contracts are part of the request.
user-invocable: false
---

# Next.js Fullstack Contracts

Apply these rules when defining the contract between frontend code and backend code in a Next.js App Router project.

## DTO Shape

See [dto-shape.md](./dto-shape.md) for:
- API DTO vs database model boundaries
- Resource shaping for screens
- Avoiding direct ORM leakage

## Error Contracts

See [error-contracts.md](./error-contracts.md) for:
- Stable error codes
- UI handling rules by error category
- When to show field vs page vs toast feedback

## Pagination Contracts

See [pagination-contracts.md](./pagination-contracts.md) for:
- Request param naming
- Response metadata shape
- Cursor behavior across UI and API

## Shared Schemas

See [shared-schemas.md](./shared-schemas.md) for:
- What schemas can be shared
- Boundary-safe zod usage
- UI schema vs persistence schema rules

## Client Consumption

See [client-consumption.md](./client-consumption.md) for:
- Reading `success / data / error / meta`
- Centralized API client helpers
- Consistent failure handling

## Cache Invalidation

See [cache-invalidation.md](./cache-invalidation.md) for:
- Revalidate and refresh coordination
- When UI should navigate, refresh, or optimistically update
- Tag and path invalidation expectations
