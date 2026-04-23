# Error Contracts

Error codes should drive predictable frontend behavior.

## Rules

- Use stable machine-readable `error.code` values
- Do not force the UI to parse human-readable messages
- Map each error code family to a default UI treatment

## Suggested UI Mapping

| Code | Default UI Behavior |
|------|---------------------|
| `BAD_REQUEST` | Inline request-level error |
| `VALIDATION_ERROR` | Field or form errors |
| `UNAUTHORIZED` | Redirect to login or show auth prompt |
| `FORBIDDEN` | Permission state |
| `NOT_FOUND` | 404 state or empty detail view |
| `CONFLICT` | Inline conflict message |
| `RATE_LIMITED` | Retry-later message |
| `INTERNAL_ERROR` | Generic failure state |

## Rules for Forms

- Prefer field-level mapping only for validation errors
- Do not show raw backend text directly when the UI already has a better localized message
- Keep fallback handling for unknown error codes

## Quick Reference

| Concern | Rule |
|---------|------|
| UI logic source | `error.code` |
| Field-level errors | `VALIDATION_ERROR` |
| Unknown code | Generic fallback |
| Message parsing by string contains | No |
