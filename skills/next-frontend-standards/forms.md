# Forms

Make forms schema-driven and predictable for both success and failure paths.

## Rules

- Keep one schema per form payload
- Reuse schema-derived types when practical
- Show field-level errors for validation failures
- Show form-level errors for auth, conflict, or server failures
- Disable duplicate submission while a mutation is in flight

## Mutation Pattern

Use Server Actions for app-internal form mutations by default.
Use Route Handlers only when the form targets a stable public API contract.

```tsx
// Bad: form posts to an API route for an app-internal mutation by default
fetch('/api/users', {
  method: 'POST',
  body: JSON.stringify(values),
})

// Good: internal form uses a server action
<form action={createUserAction}>
  <input name="email" />
  <button type="submit">Create user</button>
</form>
```

## UX Rules

- Preserve user input after validation failure
- Reset only when the success flow clearly requires it
- Show success feedback close to the action that triggered it
- Redirect after success only when a new canonical page should own the resource

## Quick Reference

| Concern | Rule |
|---------|------|
| Internal mutation | Server Action first |
| Public contract mutation | Route Handler |
| Validation feedback | Field-level |
| Server failure feedback | Form-level |
| Duplicate submit | Prevent |
