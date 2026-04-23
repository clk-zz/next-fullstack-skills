# Structure

Organize frontend code by screen ownership and reuse level.

## Suggested Layout

```text
app/
  dashboard/
    page.tsx
    loading.tsx
    error.tsx
components/
  dashboard/
    dashboard-view.tsx
    metric-card.tsx
  ui/
    button.tsx
    empty-state.tsx
hooks/
  use-debounced-search.ts
```

## Rules

- Keep route-specific view composition close to the route
- Move broadly reusable primitives into `components/ui`
- Keep feature-specific UI in feature folders
- Put client hooks in `hooks/` or feature-local folders
- Do not turn every small JSX fragment into a separate component

## Quick Reference

| File Type | Best Placement |
|-----------|----------------|
| Route shell | `app/**` |
| Feature view | `components/<feature>` |
| Primitive UI | `components/ui` |
| Reusable client hook | `hooks/` |
