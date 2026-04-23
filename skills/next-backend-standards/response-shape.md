# Response Shape

Return a consistent envelope from every Route Handler.

## Success Envelope

```ts
type ApiSuccess<T> = {
  success: true
  data: T
  meta?: {
    requestId?: string
    pagination?: {
      nextCursor?: string | null
      prevCursor?: string | null
      hasMore?: boolean
      total?: number
    }
  }
}
```

## Error Envelope

```ts
type ApiError = {
  success: false
  error: {
    code:
      | 'BAD_REQUEST'
      | 'VALIDATION_ERROR'
      | 'UNAUTHORIZED'
      | 'FORBIDDEN'
      | 'NOT_FOUND'
      | 'CONFLICT'
      | 'RATE_LIMITED'
      | 'INTERNAL_ERROR'
    message: string
    details?: unknown
  }
  meta?: {
    requestId?: string
  }
}
```

## Standard Helpers

```tsx
// lib/api/response.ts
import { NextResponse } from 'next/server'

export function ok<T>(data: T, init?: ResponseInit) {
  return NextResponse.json({ success: true, data }, init)
}

export function created<T>(data: T) {
  return NextResponse.json({ success: true, data }, { status: 201 })
}

export function fail(
  status: number,
  code: string,
  message: string,
  details?: unknown
) {
  return NextResponse.json(
    {
      success: false,
      error: { code, message, details },
    },
    { status }
  )
}
```

## Rules

- Always return `success`, even for simple endpoints
- Put resource payloads under `data`
- Put pagination and request metadata under `meta`
- Put machine-readable identifiers under `error.code`
- Do not return raw database or library errors to clients
- Do not mix plain strings, arrays, and objects across endpoints

```tsx
// Bad: shape changes between endpoints
return NextResponse.json(users)
return NextResponse.json({ message: 'created' }, { status: 201 })
return NextResponse.json({ error: 'duplicate email' }, { status: 409 })

// Good: every endpoint uses the same top-level contract
return NextResponse.json({ success: true, data: users })
return NextResponse.json({ success: true, data: user }, { status: 201 })
return NextResponse.json(
  {
    success: false,
    error: { code: 'CONFLICT', message: 'Email already exists' },
  },
  { status: 409 }
)
```

## Suggested Status Mapping

| Case | Status | Code |
|------|--------|------|
| Malformed JSON or query | 400 | `BAD_REQUEST` |
| Validation failure | 422 | `VALIDATION_ERROR` |
| Missing session | 401 | `UNAUTHORIZED` |
| Permission denied | 403 | `FORBIDDEN` |
| Missing resource | 404 | `NOT_FOUND` |
| Duplicate resource | 409 | `CONFLICT` |
| Too many requests | 429 | `RATE_LIMITED` |
| Unexpected failure | 500 | `INTERNAL_ERROR` |
