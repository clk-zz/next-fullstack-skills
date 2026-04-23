# Auth UX

Separate unauthenticated state from unauthorized state.

## Rules

- Unauthenticated means the user must log in
- Unauthorized means the user is logged in but lacks permission
- Do not show the same message for both

## Pattern

Use server-side session checks for protected pages.

```tsx
// Good: session is checked before rendering the page
export default async function BillingPage() {
  const session = await requireSession()

  if (!session.permissions.includes('billing:read')) {
    forbidden()
  }

  return <BillingDashboard />
}
```

## UX Guidance

- Redirect to login when authentication is required to continue the flow
- Show a permission state when the user is logged in but blocked
- Keep permission-denied components explicit and non-generic

## Quick Reference

| Case | Recommended UX |
|------|----------------|
| No session | Login redirect or unauthorized page |
| Logged in, no permission | Forbidden page or inline permission state |
| Partial feature access | Disable action with explanation |
