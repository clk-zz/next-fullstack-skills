# Layers

Separate transport, business logic, and persistence concerns.

## Recommended Layers

- `route.ts`: HTTP transport
- `service.ts`: business workflow and transactions
- `repository.ts`: persistence details
- `validation.ts`: schemas and inferred types

## Example

```tsx
// app/api/users/route.ts
export async function POST(request: Request) {
  const body = await request.json()
  const input = createUserSchema.parse(body)
  const session = await requireSession()
  const user = await createUser({ input, actorId: session.user.id })

  return created(user)
}
```

```ts
// server/services/user-service.ts
export async function createUser({
  input,
  actorId,
}: {
  input: CreateUserInput
  actorId: string
}) {
  await assertCanManageUsers(actorId)

  const existing = await userRepository.findByEmail(input.email)
  if (existing) {
    throw new HttpError(409, 'CONFLICT', 'Email already exists')
  }

  return userRepository.create(input)
}
```

```ts
// server/repositories/user-repository.ts
export const userRepository = {
  findByEmail(email: string) {
    return db.user.findUnique({ where: { email } })
  },
  create(data: CreateUserInput) {
    return db.user.create({ data })
  },
}
```

## Anti-Patterns

```tsx
// Bad: server code fetches its own API
const users = await fetch('http://localhost:3000/api/users').then((r) => r.json())

// Good: server code calls the same service directly
const users = await listUsers()
```

```tsx
// Bad: repository returns HTTP responses
return NextResponse.json({ success: true, data: user })

// Good: repository returns plain data
return db.user.findMany()
```

## Quick Reference

| Layer | Input | Output |
|-------|-------|--------|
| Route | Request details | HTTP response |
| Service | Validated domain input | Domain result |
| Repository | Query params | Persisted data |
| Validation | Unknown input | Typed input |
