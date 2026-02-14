# NativeForm Contracts (Agent Skill)

## Build order

1. HTTP API (canonical)
2. CLI wrapper
3. MCP extension
4. Skill orchestration layer

## Public agent-fill endpoints

- `GET /api/public/forms/{slug}/ai-manifest`
- `POST /api/public/forms/{slug}/drafts`
- `POST /api/public/forms/{slug}/submissions`
- `GET /api/public/confirm/{token}`
- `POST /api/public/confirm/{token}`

## Manifest required keys

- `manifestVersion`
- `generatedAt`
- `form`
- `instructions`
- `fields`
- `submission`
- `rateLimits`
- `examples`

Each `fields[]` entry must include `type`, `required`, `options` when relevant, and `validation`.

## Instructions mapping

- `instructions.agentInstructions` <- `form.aiInstructions`
- `instructions.humanInstructions` <- `form.instructions`
- `instructions.context` <- `form.context`

## Agent identity headers

- Required: `x-nativeform-agent`
- Optional: `x-nativeform-agent-version`
- Optional: `x-nativeform-agent-channel`
- Recommended: aligned `User-Agent`

## Auth modes for management APIs

- API key header: `x-nativeform-api-key`
- OAuth header: `Authorization: Bearer <token>`

## Rate-limit contract

Always expect/emit:

- `X-RateLimit-Limit`
- `X-RateLimit-Remaining`
- `X-RateLimit-Reset`
- `Retry-After` on `429`

## Error contract

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

## Locked v1 scope names

API key scopes use `nf.<resource>.<action>`.
OAuth scopes use `nativeform.<resource>.<action>`.

Minimum set:

- Forms: `read`, `write`, `publish`
- Responses: `read`, `delete`, `process`
- Webhooks: `read`, `write`, `test`
- Keys: `read`, `create`, `revoke`, `rotate`
