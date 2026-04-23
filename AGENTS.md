# Agent Instructions

Rules for contributing to this repository.

## Style Guidelines

- **No emojis** in any markdown files or code comments
- Use `Yes/No` in tables instead of checkmarks or emoji
- Keep examples concise and focused
- Use `// Bad:` and `// Good:` comments in code examples
- Prefer `NextResponse.json()` examples for API responses
- Follow existing file patterns in `skills/next-backend-standards/`

## File Structure

Each skill topic should have:
- Clear heading with brief description
- Decision rules or patterns (if applicable)
- Code examples showing bad vs good patterns
- Quick reference table at the end

## Code Examples

```tsx
// Bad: route mixes validation, auth, business logic, and database details
export async function POST(request: Request) {
  const body = await request.json()
  const record = await db.item.create({ data: body })
  return Response.json(record)
}

// Good: route coordinates helpers and returns a consistent response
export async function POST(request: Request) {
  const body = await request.json()
  const result = await createItem(body)
  return NextResponse.json({ success: true, data: result }, { status: 201 })
}
```
