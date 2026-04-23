# Webhooks

Treat webhook endpoints as public, untrusted integrations.

## Core Rules

- Verify the provider signature before processing the event
- Keep webhook handlers idempotent
- Acknowledge quickly, then offload slow work
- Log provider event ids for replay analysis

## Signature Verification

Some providers require the raw request body.
Do not parse JSON before verification when the provider signs the raw payload.

```tsx
// Bad: JSON parse changes the signed payload
const body = await request.json()
verifySignature(JSON.stringify(body), request.headers)

// Good: verify raw text first
const payload = await request.text()
verifySignature(payload, request.headers)
const body = JSON.parse(payload)
```

## Idempotency

```ts
// Good: skip duplicate provider events
const alreadyProcessed = await webhookEventRepository.exists(event.id)

if (alreadyProcessed) {
  return ok({ received: true })
}

await webhookEventRepository.record(event.id)
await handleWebhookEvent(event)
```

## Response Rules

- Return `2xx` only after signature verification passes
- Return `400` for invalid payloads
- Return `401` or `403` for failed signature verification
- Return `202` when you accept the event and process it asynchronously

## Quick Reference

| Concern | Rule |
|---------|------|
| Trust level | Untrusted input |
| Verification timing | Before business logic |
| Payload format | Raw body first when required |
| Duplicate delivery | Must be safe |
| Slow downstream work | Queue or defer |
