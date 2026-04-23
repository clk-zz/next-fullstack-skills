---
name: next-frontend-standards
description: Next.js frontend standards for Server and Client Component boundaries, page-level data fetching, forms, loading and error states, routing and search params, auth UX, and component structure. Use when building or reviewing Next.js pages, layouts, components, forms, lists, filters, dashboards, and interactive UI in App Router projects. Do not use for route handlers, database code, or backend-only implementation unless the task is primarily about frontend behavior.
user-invocable: false
---

# Next.js Frontend Standards

Apply these rules when writing or reviewing frontend code in a Next.js App Router project.

## Component Boundaries

See [component-boundaries.md](./component-boundaries.md) for:
- Server Component first rules
- When Client Components are justified
- Keeping browser-only logic isolated

## Data Fetching

See [data-fetching.md](./data-fetching.md) for:
- Page-level data ownership
- Reading from services or route handlers
- Avoiding duplicate requests and waterfalls

## Forms

See [forms.md](./forms.md) for:
- Form state and validation
- Mutation handling
- Success and error feedback

## UI States

See [ui-states.md](./ui-states.md) for:
- Loading, empty, error, and success states
- Segment-level UX in App Router
- Retry and recovery patterns

## Routing

See [routing.md](./routing.md) for:
- URL-driven filters and tabs
- Search param conventions
- Navigation and reset behavior

## Auth UX

See [auth-ux.md](./auth-ux.md) for:
- Unauthenticated vs unauthorized behavior
- Redirects vs inline state
- Session-aware rendering

## Structure

See [structure.md](./structure.md) for:
- Page, section, and UI component organization
- Hook placement
- Reusable table and form patterns
