# DTO Shape

Keep API DTOs independent from ORM entities and page-local view models.

## Rules

- Do not expose raw ORM rows directly as public API contracts by default
- Shape API responses around resource contracts, not database convenience
- Allow page-level view models to differ from API DTOs when the UI needs derived fields

```ts
// Bad: route returns raw model with internal fields
return ok(userRecord)

// Good: route returns a deliberate DTO
return ok({
  id: user.id,
  email: user.email,
  displayName: user.name,
  createdAt: user.createdAt.toISOString(),
})
```

## Mapping Rules

- Map `Date` to ISO strings at API boundaries
- Remove internal flags, audit fields, and secrets unless explicitly needed
- Keep DTO names stable even if the database schema changes

## Quick Reference

| Shape | Owner |
|-------|-------|
| ORM model | Repository |
| Domain object | Service |
| API DTO | Route contract |
| View model | Page or component |
