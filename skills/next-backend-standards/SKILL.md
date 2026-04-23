---
name: next-backend-standards
description: Next.js backend API and database standards for route handlers, route.ts files, input validation, unified API responses, error handling, database clients, auth guards, pagination, webhook handlers, and service or repository layering. Use when building or reviewing server-side backend code in App Router projects, especially API endpoints, database access, server auth logic, and backend integration handlers. Do not use for page composition or frontend UI behavior unless the task is explicitly backend-facing.
user-invocable: false
---

# Next.js Backend Standards

Apply these rules when writing or reviewing backend code in a Next.js App Router project.

## API Structure

See [api-structure.md](./api-structure.md) for:
- `app/api/**/route.ts` layout
- What belongs in a route handler
- When to use Route Handlers vs Server Actions

## Response Shape

See [response-shape.md](./response-shape.md) for:
- Unified success and error envelopes
- Status code mapping
- Pagination metadata shape

## Validation

See [validation.md](./validation.md) for:
- Body, params, and query validation
- Malformed JSON handling
- Schema placement and helper patterns

## Error Handling

See [error-handling.md](./error-handling.md) for:
- Expected vs unexpected errors
- `HttpError` style mapping
- Logging and sanitizing failures

## Database

See [database.md](./database.md) for:
- Node.js runtime as the default for database routes
- Singleton database clients
- Environment validation and transaction boundaries

## Layers

See [layers.md](./layers.md) for:
- Route, service, and repository separation
- File placement for server-side backend code
- Avoiding server-to-server self-fetches

## Auth

See [auth.md](./auth.md) for:
- Authentication guards
- Authorization checks
- Ownership and tenant boundaries

## Pagination

See [pagination.md](./pagination.md) for:
- Cursor-first pagination
- Query parameter conventions
- Response metadata

## Webhooks

See [webhooks.md](./webhooks.md) for:
- Signature verification
- Idempotency
- Safe acknowledgement patterns
