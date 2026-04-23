# Pagination

Prefer cursor pagination for lists that change frequently.

## Default Query Parameters

Use:
- `limit`
- `cursor`
- `sort`
- `order`

Keep parameter names consistent across resources.

## Cursor-First Pattern

```tsx
const querySchema = z.object({
  limit: z.coerce.number().int().min(1).max(100).default(20),
  cursor: z.string().optional(),
})

export async function GET(request: Request) {
  const query = querySchema.parse(
    Object.fromEntries(new URL(request.url).searchParams)
  )

  const result = await listUsers(query)

  return NextResponse.json({
    success: true,
    data: result.items,
    meta: {
      pagination: {
        nextCursor: result.nextCursor,
        hasMore: result.hasMore,
      },
    },
  })
}
```

## Offset Pagination

Use offset pagination only when:
- The dataset is small
- Stable ordering is guaranteed
- The endpoint is primarily for internal admin tooling

If you use offset pagination, keep the response envelope the same and place values under `meta.pagination`.

## Sorting Rules

- Whitelist sortable fields
- Default to descending creation time for activity feeds
- Use unique tie-breakers for stable cursor order

## Quick Reference

| Case | Preferred Pattern |
|------|-------------------|
| Feed or timeline | Cursor |
| Large mutable table | Cursor |
| Small admin list | Offset is acceptable |
| Pagination metadata | `meta.pagination` |
| Client default limit | 20 |
