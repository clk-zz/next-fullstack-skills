# Client Consumption

Frontend code should consume API envelopes consistently.

## Default Rule

Do not scatter `if (!response.ok)` and ad hoc JSON parsing across components.
Centralize API response parsing in one helper or client module.

```ts
// Good: one API client interprets the envelope
export async function apiFetch<T>(input: RequestInfo, init?: RequestInit): Promise<T> {
  const response = await fetch(input, init)
  const json = await response.json()

  if (!response.ok || !json.success) {
    throw new ApiClientError(json.error?.code ?? 'INTERNAL_ERROR', json.error?.message ?? 'Request failed', json.error?.details)
  }

  return json.data as T
}
```

## Rules

- Treat `response.ok` and envelope `success` together
- Throw typed client errors with `code`, `message`, and optional `details`
- Keep UI components focused on rendering, not transport parsing

## Quick Reference

| Concern | Rule |
|---------|------|
| JSON parsing | Centralized |
| Error normalization | Centralized |
| Component-level response parsing | Avoid |
| Success payload access | `data` |
