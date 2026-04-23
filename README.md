# Next Fullstack Skills

Agent skills for Next.js App Router projects covering frontend standards, backend API standards, and fullstack contracts.

## Quick Install

```bash
npx skills add clk-zz/next-fullstack-skills
```

## Available Skills

### `next-backend-standards`

Backend standards for Next.js:

- [API Structure](./skills/next-backend-standards/api-structure.md) - route handler responsibilities and folder layout
- [Response Shape](./skills/next-backend-standards/response-shape.md) - unified success and error envelopes
- [Validation](./skills/next-backend-standards/validation.md) - params, query, and body validation with schemas
- [Error Handling](./skills/next-backend-standards/error-handling.md) - expected vs unexpected failures
- [Database](./skills/next-backend-standards/database.md) - singleton clients, env safety, runtime choice
- [Layers](./skills/next-backend-standards/layers.md) - route, service, and repository separation
- [Auth](./skills/next-backend-standards/auth.md) - authentication and authorization guards
- [Pagination](./skills/next-backend-standards/pagination.md) - cursor-first list endpoints
- [Webhooks](./skills/next-backend-standards/webhooks.md) - signature verification and idempotency

### `next-frontend-standards`

Frontend standards for Next.js:

- [Component Boundaries](./skills/next-frontend-standards/component-boundaries.md) - Server vs Client Component rules
- [Data Fetching](./skills/next-frontend-standards/data-fetching.md) - how pages consume backend data
- [Forms](./skills/next-frontend-standards/forms.md) - validation, submission, and mutation UX
- [UI States](./skills/next-frontend-standards/ui-states.md) - loading, empty, error, and success behavior
- [Routing](./skills/next-frontend-standards/routing.md) - pathname, search params, and filter state
- [Auth UX](./skills/next-frontend-standards/auth-ux.md) - login state, permission state, and redirects
- [Structure](./skills/next-frontend-standards/structure.md) - page, section, ui, and hooks organization

### `next-fullstack-contracts`

Shared contracts between frontend and backend:

- [DTO Shape](./skills/next-fullstack-contracts/dto-shape.md) - resource and view-model boundaries
- [Error Contracts](./skills/next-fullstack-contracts/error-contracts.md) - error code handling and UI mapping
- [Pagination Contracts](./skills/next-fullstack-contracts/pagination-contracts.md) - request and response consistency
- [Shared Schemas](./skills/next-fullstack-contracts/shared-schemas.md) - zod reuse and boundary rules
- [Client Consumption](./skills/next-fullstack-contracts/client-consumption.md) - how frontend should consume API envelopes
- [Cache Invalidation](./skills/next-fullstack-contracts/cache-invalidation.md) - revalidate and refresh coordination

## Installation

```bash
# Install one skill
npx skills add clk-zz/next-fullstack-skills --skill next-backend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-frontend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-fullstack-contracts

# Or install all skills from this repository
npx skills add clk-zz/next-fullstack-skills
```

## Usage

Use `next-backend-standards` when you need to:

- Build `app/api/**/route.ts` endpoints
- Add database access in Next.js
- Standardize response envelopes and error handling
- Introduce request validation and auth guards
- Review backend code for structure and consistency

Use `next-frontend-standards` when you need to:

- Build pages, forms, and list views in App Router
- Decide between Server and Client Components
- Standardize loading, error, empty, and permission states
- Keep filter and pagination state in the URL
- Review page and component organization

Use `next-fullstack-contracts` when you need to:

- Define shared request and response contracts
- Keep frontend DTO usage aligned with backend envelopes
- Reuse schemas safely across UI and API layers
- Standardize error-code-driven UI handling
- Coordinate pagination and cache invalidation behavior

These three skills are intended to behave like background skills:

- `next-backend-standards` should auto-match backend requests such as writing `route.ts`, connecting a database, adding auth guards, or standardizing API responses.
- `next-frontend-standards` should auto-match frontend requests such as building pages, forms, filters, dashboards, loading states, or interactive UI.
- `next-fullstack-contracts` should auto-match cross-layer requests such as frontend/backend integration, DTO design, shared schemas, unified pagination, or error-code-to-UI mapping.

The trigger signal mainly comes from each `SKILL.md` frontmatter `description`, and all three skills are marked with `user-invocable: false` so they behave more like automatically applied standards than manual slash-command skills.

## Contributing

Each skill follows the same pattern as `vercel-labs/next-skills`:

1. Create a directory under `skills/` with the skill name (prefix with `next-`)
2. Add a `SKILL.md` file with YAML frontmatter:
   ```yaml
   ---
   name: next-skill-name
   description: Brief description
   ---
   ```
3. For complex skills, add additional `.md` files and reference them from `SKILL.md`
