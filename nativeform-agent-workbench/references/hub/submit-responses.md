# Submit Responses

Web docs: `https://www.nativeform.app/docs/api/submit`

Submit a new response to a NativeForm form via the management API. This endpoint accepts free-form `text` and extracts structured fields.

## Endpoint

- `POST /api/forms/{formId}/responses`

Base URL:

- `https://www.nativeform.app/api`

## Auth

- Required header: `x-nativeform-api-key: $NATIVEFORM_API_KEY`

## Request body

- `text` (string, required): free-form content to extract from
- `respondent` (object, optional): `{ externalId?, email?, name? }`
- `metadata` (object, optional): any JSON object (stored with the response)
- `dateContext` (string, optional): ISO timestamp used to interpret relative times ("next Tuesday")

## Example

```bash
FORM_ID="your-form-id"
curl -X POST "https://www.nativeform.app/api/forms/$FORM_ID/responses" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "I want to report an issue with my order #12345. It arrived damaged.",
    "respondent": { "externalId": "user_abc123", "email": "customer@example.com", "name": "Jane Smith" },
    "metadata": { "source": "support-chat", "sessionId": "sess_xyz789" }
  }'
```

## Success response (high-level)

- HTTP `201`
- `data.responseId`
- `data.submittedAt`
- `data.channel` (typically `"API"`)

## Errors

- `400`: invalid JSON or schema validation errors
- `401`: missing/invalid key
- `404`: form not found
- `422`: missing required fields (the response is not written)

## Best practices

- Call from your backend only. Never expose the key in the browser.
- Validate user input before sending to NativeForm.
- Store `responseId` in your system so you can correlate downstream webhooks and retries.

