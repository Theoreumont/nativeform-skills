# Form

Web docs: `https://www.nativeform.app/docs/api`

This page focuses on form-level operations: listing forms, creating forms, and finding the `formId` you need for other endpoints.

## Base URL

- Management API base: `https://www.nativeform.app/api`

## Find your `formId`

- Open a form in the dashboard.
- The `formId` is in the URL: `/form/{formId}`

## List forms

Endpoint:

- `GET /api/forms`

Query parameters:

- `limit` (1-200, default 50)
- `cursor` (string; use `meta.nextCursor`)
- `order` (`asc|desc`, default `desc`)
- `status` (optional)
- `visibility` (optional)
- `search` (optional; matches name/slug)

Example:

```bash
curl -G "https://www.nativeform.app/api/forms" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  --data-urlencode "limit=10" \
  --data-urlencode "search=contact"
```

## Create a form

Endpoint:

- `POST /api/forms`

Body fields:

- `name` (required)
- `slug` (optional; `^[a-z0-9-]+$`)
- `description` (optional)
- `visibility` (optional; `PUBLIC|PRIVATE`, default `PRIVATE`)
- `status` (optional; default `DRAFT`)
- `instructions` (optional)
- `context` (optional)

Example:

```bash
curl -X POST "https://www.nativeform.app/api/forms" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Customer intake",
    "visibility": "PUBLIC",
    "status": "DRAFT",
    "description": "Collect lead details for sales follow-up"
  }'
```

## Next steps

- Read data: [Read Responses](read-responses.md)
- Submit data: [Submit Responses](submit-responses.md)
- Prefer CLI/MCP for richer form lifecycle ops (publish/pause/duplicate/pin, schema edits):
- CLI: [nativeform CLI deep dive](cli-deepdive.md)
- Capability inventory: [Capabilities map](../capabilities.md)

