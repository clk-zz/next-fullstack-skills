# UI States

Every page and major panel should define loading, empty, error, and success behavior.

## Loading

Use segment-level loading UI where App Router supports it.

- Use `loading.tsx` for route-level loading
- Use Suspense fallbacks for independent subtrees
- Keep skeletons shape-matched to the final content

## Empty

Empty state should explain:
- What the user is seeing
- Why it might be empty
- What action they can take next

```tsx
// Bad: blank state with no guidance
if (users.length === 0) return null

// Good: explicit empty state
if (users.length === 0) {
  return <EmptyState title="No users yet" description="Invite your first team member to get started." />
}
```

## Error

- Use `error.tsx` for route-segment failures
- Use inline error components for local widget failures
- Offer retry when the action is safe to repeat

## Success

- Show a visible confirmation after mutations when the result is not otherwise obvious
- Prefer inline confirmation over toast-only feedback for important flows

## Quick Reference

| State | Expectation |
|-------|-------------|
| Loading | Skeleton or fallback |
| Empty | Explanation plus next action |
| Error | Recoverable message plus retry when safe |
| Success | Explicit feedback when meaningful |
