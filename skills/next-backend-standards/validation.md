# Validation

Validate params, query, and body before calling service functions.

## Default Rule

Use one schema source for each route:
- Params schema
- Query schema
- Body schema

Prefer `zod` for request validation because it is explicit and easy to share between API and UI code.

## Body Validation

```tsx
import { z } from 'zod'

const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(50),
})

export async function POST(request: Request) {
  const json = await request.json()
  const input = createUserSchema.parse(json)
  const user = await createUser(input)

  return NextResponse.json({ success: true, data: user }, { status: 201 })
}
```

## Params and Query Validation

```tsx
import { z } from 'zod'

const paramsSchema = z.object({
  id: z.string().uuid(),
})

const querySchema = z.object({
  limit: z.coerce.number().int().min(1).max(100).default(20),
  cursor: z.string().optional(),
})

export async function GET(
  request: Request,
  context: { params: Promise<{ id: string }> }
) {
  const params = paramsSchema.parse(await context.params)
  const query = querySchema.parse(
    Object.fromEntries(new URL(request.url).searchParams)
  )

  const result = await listUserSessions(params.id, query)
  return NextResponse.json({ success: true, data: result })
}
```

## Malformed JSON

Differentiate malformed JSON from schema validation failure.

```tsx
// Bad: invalid JSON falls through as a 500
const body = await request.json()
const input = schema.parse(body)

// Good: malformed JSON becomes a client error
let body: unknown

try {
  body = await request.json()
} catch {
  return fail(400, 'BAD_REQUEST', 'Request body must be valid JSON')
}

const parsed = schema.safeParse(body)

if (!parsed.success) {
  return fail(422, 'VALIDATION_ERROR', 'Request validation failed', parsed.error.flatten())
}
```

## Placement

- Keep schemas in `server/validations/` or beside the route for small features
- Export inferred types from schemas
- Reuse the same schema in form validation when practical
- Validate external webhook payloads even when the provider documents them

## Quick Reference

| Input | Validate In | Typical Failure |
|-------|-------------|-----------------|
| Path params | Route handler | 400 |
| Query params | Route handler | 400 |
| JSON body syntax | Route handler | 400 |
| JSON body shape | Route handler | 422 |
| Cross-field business rule | Service | 409 or 422 |
