# Shared Schemas

Share schemas deliberately, not blindly.

## Safe Sharing

Share schemas for:
- Public request payloads
- Filter params
- Form payloads
- Public response DTOs

Do not share schemas that are tightly coupled to:
- Database persistence shape
- Internal service-only objects
- Secrets or privileged fields

```ts
// Good: public create-user payload schema
export const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(50),
})
```

## Rules

- Keep public schemas in a boundary-safe module
- Derive TypeScript types from the schema
- Extend or transform schemas per layer instead of reusing the wrong one

## Quick Reference

| Schema Type | Share Across UI and API |
|-------------|-------------------------|
| Request payload | Yes |
| Query params | Yes |
| Public DTO | Yes |
| ORM create input | No |
| Internal service object | Usually no |
