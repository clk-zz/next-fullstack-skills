# API Structure

Structure route handlers as thin transport layers over backend services.

## Default Folder Layout

```text
app/
  api/
    users/
      route.ts
    users/
      [id]/
        route.ts
lib/
  api/
    response.ts
    errors.ts
server/
  auth/
    session.ts
  db/
    client.ts
  repositories/
    user-repository.ts
  services/
    user-service.ts
  validations/
    user.ts
```

## Route Handler Responsibilities

Keep route handlers responsible for:
- Parsing the request
- Validating params, query, and body
- Reading auth context
- Calling one service function
- Returning a consistent HTTP response

Do not put raw database queries or multi-step business rules directly in `route.ts`.

```tsx
// Bad: route owns transport, validation, and data access
export async function POST(request: Request) {
  const body = await request.json()

  if (!body.email) {
    return Response.json({ error: 'email is required' }, { status: 400 })
  }

  const user = await db.user.create({ data: body })
  return Response.json(user, { status: 201 })
}

// Good: route coordinates helpers and delegates business logic
export async function POST(request: Request) {
  const body = await request.json()
  const input = createUserSchema.parse(body)
  const result = await createUser(input)

  return NextResponse.json(
    { success: true, data: result },
    { status: 201 }
  )
}
```

## Route Handlers vs Server Actions

Use Route Handlers for:
- Public or partner-facing REST APIs
- Mobile clients
- Webhooks
- GET endpoints that need HTTP semantics

Use Server Actions for:
- UI-triggered mutations inside the same Next.js app
- Forms that do not need a public API contract

Do not fetch your own route handlers from Server Components or Server Actions when direct function calls are possible.

## Naming and Resource Rules

- Use plural resources for collections: `/api/users`
- Use nested segments only when there is a real parent-child relation: `/api/users/[id]/sessions`
- Use one route per resource path and HTTP method
- Use `GET`, `POST`, `PATCH`, `DELETE` as the default CRUD surface
- Add `OPTIONS` only when the integration requires it

## Quick Reference

| Concern | Route Handler | Service | Repository |
|---------|---------------|---------|------------|
| Parse request | Yes | No | No |
| Validate input | Yes | Sometimes | No |
| Auth context | Yes | Sometimes | No |
| Business rules | No | Yes | No |
| Transactions | No | Yes | No |
| Raw queries | No | No | Yes |
