# Auth

Resolve identity once and keep authorization decisions close to business logic.

## Authentication Rules

- Read the authenticated user from server-side session or token helpers
- Fail fast with `401` when no valid identity exists
- Do not trust `userId`, `role`, or `tenantId` from client input
- Pass actor context into service functions explicitly

```tsx
// Bad: trust userId from the request body
const body = await request.json()
await updateProfile(body.userId, body)

// Good: derive actor from the session
const session = await requireSession()
const body = await request.json()
await updateProfile(session.user.id, body)
```

## Authorization Rules

Check permissions in services so the same rules apply across routes, jobs, and actions.

```ts
export async function deleteUser({
  actor,
  userId,
}: {
  actor: SessionUser
  userId: string
}) {
  if (actor.role !== 'admin') {
    throw new HttpError(403, 'FORBIDDEN', 'Admin access required')
  }

  return userRepository.delete(userId)
}
```

## Multi-Tenant Safety

- Scope reads and writes by tenant or organization id
- Validate ownership before returning a resource
- Do not expose sequential ids across tenants when avoidable

## Quick Reference

| Concern | Rule |
|---------|------|
| Identity source | Session or verified token |
| Missing identity | 401 |
| Missing permission | 403 |
| Ownership check | Service layer |
| Client-provided actor ids | No |
