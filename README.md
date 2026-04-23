# Next Fullstack Skills

[English](./README.md) | [中文](./README.zh-CN.md)

Agent skills for Next.js App Router projects covering frontend standards, backend API standards, and fullstack contracts.

## What This Repository Provides

This repository contains 3 background skills for building Next.js applications with clearer boundaries between frontend, backend, and shared contracts.

- `next-backend-standards`
  Backend rules for `route.ts`, API design, validation, database access, auth, pagination, and webhooks.
- `next-frontend-standards`
  Frontend rules for Server and Client Component boundaries, forms, routing, UI states, and page structure.
- `next-fullstack-contracts`
  Shared rules for DTOs, API envelopes, error-code handling, pagination contracts, shared schemas, and cache invalidation.

After installation, these skills are meant to work like background standards. The model should match them automatically based on whether the task is mainly backend, frontend, or cross-layer integration.

## Installation

Install all skills:

```bash
npx skills add clk-zz/next-fullstack-skills
```

Install a single skill:

```bash
npx skills add clk-zz/next-fullstack-skills --skill next-backend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-frontend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-fullstack-contracts
```

List available skills before installing:

```bash
npx skills add clk-zz/next-fullstack-skills --list
```

## How To Use

You do not need to manually invoke these skills in normal use. Once installed, the model should select the right skill from your request:

- If the task is about writing APIs, `route.ts`, database logic, auth, or webhooks, it should use `next-backend-standards`.
- If the task is about pages, forms, lists, filters, loading states, or interactive UI, it should use `next-frontend-standards`.
- If the task spans both frontend and backend, such as DTOs, unified response handling, pagination contracts, or integration flows, it should use `next-fullstack-contracts`.

Example requests:

- "Write a user API with validation and Prisma integration."
- "Build a dashboard page with filters, loading states, and pagination."
- "Standardize backend responses and make the frontend handle errors and pagination consistently."

## Included Skills

### `next-backend-standards`

Entry file: [skills/next-backend-standards/SKILL.md](./skills/next-backend-standards/SKILL.md)

Covers:

- Route handler structure
- Unified success and error responses
- Request validation
- Error handling
- Database client rules
- Service and repository layering
- Auth and authorization
- Pagination
- Webhook handling

### `next-frontend-standards`

Entry file: [skills/next-frontend-standards/SKILL.md](./skills/next-frontend-standards/SKILL.md)

Covers:

- Server and Client Component boundaries
- Page-level data fetching
- Forms and mutation UX
- Loading, empty, error, and success states
- URL-driven filters and routing
- Auth-related page behavior
- Frontend file and component structure

### `next-fullstack-contracts`

Entry file: [skills/next-fullstack-contracts/SKILL.md](./skills/next-fullstack-contracts/SKILL.md)

Covers:

- DTO shape
- Shared request and response contracts
- Error-code-driven UI behavior
- Pagination request and response shape
- Shared schemas
- API client consumption rules
- Cache invalidation coordination

## Recommended Use Cases

Use this repository if you want to:

- Keep Next.js frontend and backend responsibilities separate
- Standardize API response shape and validation rules
- Make frontend pages consume backend data more consistently
- Reduce ad hoc contract design between UI and API layers
- Reuse one set of engineering rules across multiple Next.js projects
