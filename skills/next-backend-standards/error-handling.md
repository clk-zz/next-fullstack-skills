# Error Handling

Handle expected errors explicitly and unexpected errors defensively.

## Error Categories

- Expected errors: validation, auth, not found, conflict, rate limiting
- Unexpected errors: driver crashes, programming bugs, upstream outages

Expected errors should map to stable HTTP status codes and error codes.
Unexpected errors should be logged and returned as sanitized 500 responses.

## Minimal Error Type

```ts
export class HttpError extends Error {
  constructor(
    public status: number,
    public code: string,
    message: string,
    public details?: unknown
  ) {
    super(message)
  }
}
```

## Route-Level Mapping

```tsx
export async function PATCH(request: Request) {
  try {
    const body = await request.json()
    const input = updateUserSchema.parse(body)
    const user = await updateUser(input)

    return ok(user)
  } catch (error) {
    if (error instanceof HttpError) {
      return fail(error.status, error.code, error.message, error.details)
    }

    console.error('PATCH /api/users failed', error)
    return fail(500, 'INTERNAL_ERROR', 'Internal server error')
  }
}
```

## Business Errors Belong in Services

```ts
// Bad: route owns domain rules
if (existingUser) {
  return fail(409, 'CONFLICT', 'Email already exists')
}

// Good: service throws a domain-level HTTP error
if (existingUser) {
  throw new HttpError(409, 'CONFLICT', 'Email already exists')
}
```

## Logging Rules

- Log unexpected errors with route context and request id
- Do not log secrets, tokens, passwords, or full webhook payloads
- Do not return stack traces to clients
- Prefer structured logging when the project already has a logger

## Quick Reference

| Error Type | Throw Where | Return To Client |
|------------|-------------|------------------|
| Validation | Route or service | 400 or 422 |
| Unauthorized | Auth helper | 401 |
| Forbidden | Service or auth helper | 403 |
| Not found | Service | 404 |
| Conflict | Service | 409 |
| Unexpected error | Any layer | 500 |
