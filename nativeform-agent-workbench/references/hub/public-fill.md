# Public Fill

Web docs: `https://www.nativeform.app/docs/agents/public-fill`

NativeForm exposes a public agent-fill protocol for public forms/embeds.

Key points:

- No API key required.
- Agents should discover the schema from the manifest, optionally create drafts, then submit.
- Use agent attribution headers so NativeForm can attribute the submission.

## Endpoints

- `GET /api/public/forms/{slug}/ai-manifest`
- `POST /api/public/forms/{slug}/drafts`
- `POST /api/public/forms/{slug}/submissions`
- `GET /api/public/confirm/{token}`
- `POST /api/public/confirm/{token}`

## Required headers (agent attribution)

- `x-nativeform-agent: <agent-name>` (required)
- `x-nativeform-agent-version: <version>` (optional)
- `x-nativeform-agent-channel: api|cli|mcp|skill|other` (optional)

## Draft request

```http
POST /api/public/forms/customer-intake/drafts
Content-Type: application/json

{
  "answers": [
    { "fieldKey": "company_name", "value": "Acme Inc." }
  ],
  "respondent": {
    "externalId": "crm_123",
    "displayName": "Ada Lovelace",
    "contact": { "email": "ada@lovelace.ai" }
  },
  "metadata": {
    "campaign": "q1-launch"
  }
}
```

## Draft response (example)

```json
{
  "data": {
    "draftId": "drf_123",
    "responseId": "drf_123",
    "form": { "id": "frm_123", "slug": "customer-intake", "name": "Customer Intake" },
    "confirmation": {
      "token": "....",
      "expiresAt": "2026-02-14T12:00:00.000Z",
      "endpoint": "/api/public/confirm/....",
      "webUrl": "/public/confirm/...."
    }
  }
}
```

## Submit request

```http
POST /api/public/forms/customer-intake/submissions
Content-Type: application/json

{
  "draftId": "drf_123",
  "answers": [
    { "fieldKey": "company_name", "value": "Acme Inc." },
    { "fieldKey": "email", "value": "ada@lovelace.ai" }
  ]
}
```

## Discovery checklist

- Read the manifest from `/ai-manifest` to get field keys, types, and validations.
- Handle `422` `fieldErrors` by asking for corrections instead of retrying blindly.
- Honor `Retry-After` on `429`.

## Errors (common)

- `422 VALIDATION_ERROR`: fix `error.fieldErrors`
- `403 POLICY_DISABLED`: public fill disabled for the form
- `429 RATE_LIMITED`: back off using `Retry-After`
- `400 INVALID_CONFIRM_TOKEN`: token expired; create a new draft

