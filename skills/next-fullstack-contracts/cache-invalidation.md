# Cache Invalidation

Coordinate mutation results with UI refresh behavior.

## Rules

- Backend mutations should declare what data becomes stale
- Frontend flows should know whether to refresh, revalidate, navigate, or update optimistically
- Use one consistent rule per mutation type

## Recommended Patterns

- Create flow: redirect to the canonical detail page or refresh the list view
- Update flow: optimistic UI for low-risk fields, revalidate for authoritative fields
- Delete flow: remove from current list and revalidate affected summaries

## Next.js Coordination

- Server Actions should call `revalidatePath()` or `revalidateTag()` when they own the mutation
- Route Handler based mutations should trigger a client refresh strategy explicitly
- Do not assume the UI will reflect new data without an invalidation plan

## Quick Reference

| Mutation | Typical Follow-Up |
|----------|-------------------|
| Create | Redirect or refresh |
| Update | Optimistic update plus revalidate when needed |
| Delete | Remove locally plus revalidate summaries |
| Bulk change | Revalidate tags or paths |
