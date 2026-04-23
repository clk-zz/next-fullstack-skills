# Component Boundaries

Default to Server Components and isolate interactivity in small Client Components.

## Default Rule

Use Server Components for:
- Page shells
- Data reads
- Layout composition
- SEO-visible content

Use Client Components only for:
- Event handlers
- Browser APIs
- Local UI state
- Realtime subscriptions

```tsx
// Bad: page becomes a Client Component just to handle one button
'use client'

export default function UsersPage() {
  const [open, setOpen] = useState(false)
  return <UsersTable onInviteClick={() => setOpen(true)} />
}

// Good: page stays on the server, interactivity is isolated
export default async function UsersPage() {
  const users = await listUsers()
  return <UsersView users={users} />
}
```

```tsx
'use client'

export function InviteUserButton() {
  const [open, setOpen] = useState(false)
  return <Button onClick={() => setOpen(true)}>Invite user</Button>
}
```

## Rules

- Do not add `'use client'` at the page level unless most of the tree truly needs it
- Pass server-fetched data into Client Components instead of re-fetching by default
- Keep browser-only libraries behind Client Component boundaries
- Do not import server-only modules into Client Components

## Quick Reference

| Concern | Server Component | Client Component |
|---------|------------------|------------------|
| Read database-backed data | Yes | No |
| Use `useState` or `useEffect` | No | Yes |
| Access `window` or `localStorage` | No | Yes |
| Render page shell | Yes | Sometimes |
