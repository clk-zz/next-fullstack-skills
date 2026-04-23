# Database

Default to the Node.js runtime and use a singleton database client.

## Runtime Choice

Use the Node.js runtime for database-backed route handlers unless the driver is explicitly edge-safe.

```tsx
// app/api/users/route.ts
export const runtime = 'nodejs'
```

Use Edge only when:
- The database driver is HTTP-based and edge-compatible
- Low-latency global reads are worth the tradeoff
- You have verified connection, timeout, and feature limits

## Singleton Client

Create the database client once and reuse it across hot reloads in development.

```ts
// Bad: new client inside a route
export async function GET() {
  const db = new PrismaClient()
  const users = await db.user.findMany()
  return NextResponse.json({ success: true, data: users })
}
```

```ts
// Good: singleton client in server/db/client.ts
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as {
  prisma?: PrismaClient
}

export const db =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['warn', 'error'] : ['error'],
  })

if (process.env.NODE_ENV !== 'production') {
  globalForPrisma.prisma = db
}
```

## Environment Safety

- Read credentials from environment variables only
- Validate required env vars during startup
- Keep environment parsing in one module such as `server/env.ts`
- Do not scatter `process.env.*` reads across route handlers

## Transaction Boundary

Open transactions in the service layer, not in route handlers.

```ts
// Good: transaction groups one business operation
export async function createOrder(input: CreateOrderInput) {
  return db.$transaction(async (tx) => {
    const order = await tx.order.create({ data: input.order })
    await tx.orderItem.createMany({ data: input.items.map((item) => ({ ...item, orderId: order.id })) })
    return order
  })
}
```

## Query Rules

- Select only the fields the endpoint needs
- Do not expose database records directly when API shape differs
- Keep raw SQL in repository modules
- Prefer cursor columns that are indexed and stable

## Quick Reference

| Rule | Default |
|------|---------|
| Runtime for DB routes | `nodejs` |
| Client creation | Singleton |
| Env access | Centralized |
| Transaction owner | Service layer |
| Query owner | Repository layer |
