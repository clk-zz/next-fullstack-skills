# Pagination Contracts

Frontend and backend should use the same request and response pagination shape.

## Request Contract

Use consistent query keys:
- `limit`
- `cursor`
- `sort`
- `order`

## Response Contract

Return items under `data` and pagination state under `meta.pagination`.

```ts
type PaginatedResponse<T> = {
  success: true
  data: T[]
  meta: {
    pagination: {
      nextCursor?: string | null
      prevCursor?: string | null
      hasMore?: boolean
      total?: number
    }
  }
}
```

## UI Rules

- Reset cursor when filters change
- Keep pagination state in the URL for list pages
- Treat missing `nextCursor` as the end of the list

## Quick Reference

| Concern | Rule |
|---------|------|
| Query key for size | `limit` |
| Query key for continuation | `cursor` |
| Response items | `data` |
| Pagination metadata | `meta.pagination` |
