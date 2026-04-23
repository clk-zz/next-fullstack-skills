# Data Fetching

Fetch page data on the server and keep data ownership clear.

## Preferred Pattern

Read data in Server Components by calling server-side functions directly.

```tsx
// Good: page reads from a server-side service
export default async function OrdersPage() {
  const orders = await listOrders()
  return <OrdersPageView orders={orders} />
}
```

## Avoid Self-Fetching

Do not call your own route handlers from Server Components unless you are intentionally exercising a public API contract.

```tsx
// Bad: server-to-server self-fetch
const orders = await fetch(`${process.env.NEXT_PUBLIC_APP_URL}/api/orders`).then((r) => r.json())

// Good: direct server call
const orders = await listOrders()
```

## Client Fetching

Use client-side fetching only when the data must change after hydration because of:
- Polling
- Realtime streams
- User-driven refetch without navigation
- Browser-only dependencies

If a Client Component fetches from an API, keep the response envelope handling centralized.

## Rules

- Page-level reads belong to the closest Server Component that owns the screen
- Parallelize independent reads with `Promise.all`
- Use Suspense boundaries for independently loading panels
- Do not fetch the same collection in both page and child component without reason

## Quick Reference

| Situation | Preferred Pattern |
|-----------|-------------------|
| Initial page render | Server Component read |
| Interactive refreshable widget | Client fetch or server action result |
| Public third-party consumer | Route Handler |
| Internal page data | Server-side service call |
