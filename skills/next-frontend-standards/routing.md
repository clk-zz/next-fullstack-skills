# Routing

Keep filter, search, tab, and pagination state in the URL.

## URL as Source of Truth

Store durable UI state in search params:
- Search keywords
- Sort order
- Tabs
- Filters
- Pagination cursor or page

Do not hide shareable state only inside React local state.

```tsx
// Bad: filter state cannot be shared or refreshed
const [status, setStatus] = useState('active')

// Good: filter state lives in search params
const searchParams = useSearchParams()
const status = searchParams.get('status') ?? 'active'
```

## Rules

- Use stable parameter names across pages
- Reset pagination when filters or sort change
- Preserve unrelated params when updating one filter
- Parse params once and pass typed values down

## Navigation Rules

- Use `Link` for navigable state changes when possible
- Use router mutations for imperative flows only
- Keep back-button behavior meaningful

## Quick Reference

| Concern | Rule |
|---------|------|
| Search and filters | URL-driven |
| Pagination reset on filter change | Yes |
| Preserve unrelated params | Yes |
| Local-only state for shareable filters | No |
