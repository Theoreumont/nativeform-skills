# NativeForm Contracts (API / CLI / MCP)

This reference captures stable, public-facing contracts that apply across NativeForm integration surfaces.

## Surfaces

- HTTP API (canonical): `https://www.nativeform.app/api`
- CLI (wrapper): `nativeform ...` (calls the HTTP API)
- MCP (typed tools): `https://www.nativeform.app/mcp`

## Build order (parity principle)

1. HTTP API first (canonical contract)
2. CLI wraps the same behavior
3. MCP exposes typed tools around the same behavior
4. Skills orchestrate the above (instruction layer, not logic)

## Management auth (private API)

Send one of:

- `x-nativeform-api-key: <api-key>` (recommended)
- `Authorization: Bearer <oauth-access-token>` (used by MCP-linked sessions)

Key hygiene:

- Keep keys server-side (backend/proxy), not in the browser.
- Rotate keys if exposed.

## Agent attribution headers

For agent-originated requests (especially public agent-fill), include:

- Required: `x-nativeform-agent: <agent-name>`
- Optional: `x-nativeform-agent-version: <version>`
- Optional: `x-nativeform-agent-channel: api|cli|mcp|skill|other`
- Recommended: align `User-Agent` with agent name/version

Note:

- `metadata.nativeform` is reserved and server-controlled. Callers should not try to override it.

## Public agent-fill endpoints (no API key)

Canonical endpoints:

- `GET /api/public/forms/{slug}/ai-manifest`
- `POST /api/public/forms/{slug}/drafts`
- `POST /api/public/forms/{slug}/submissions`
- `GET /api/public/confirm/{token}`
- `POST /api/public/confirm/{token}`

Expected outcomes:

- Success: `201` with IDs (`draftId`, `responseId`)
- Validation: `422` with `error.fieldErrors`
- Policy disabled: `403` with `error.code=POLICY_DISABLED`
- Rate limited: `429` with `Retry-After`

## Rate limiting

Expect (and honor) these headers:

- `X-RateLimit-Limit`
- `X-RateLimit-Remaining`
- `X-RateLimit-Reset`
- `Retry-After` on `429`

## Error envelope

Errors return a structured envelope:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Human-readable error",
    "fieldErrors": {
      "email": ["Invalid email"]
    },
    "hint": "Actionable next step",
    "docs_url": "https://www.nativeform.app/docs/..."
  }
}
```

## Scope naming (v1)

- API key scopes: `nf.<resource>.<action>`
- OAuth scopes: `nativeform.<resource>.<action>`

Common resources/actions:

- Forms: `read`, `write`, `publish`
- Responses: `read`, `delete`, `process`
- Webhooks: `read`, `write`, `test`
- API keys: `read`, `create`, `revoke`, `rotate`

