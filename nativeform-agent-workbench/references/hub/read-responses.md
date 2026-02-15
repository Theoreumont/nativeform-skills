# Read Responses

Web docs: `https://www.nativeform.app/docs/api`

Fetch and filter form submissions from the NativeForm management API.

## Endpoint

- `GET /api/forms/{formId}/responses`

Base URL:

- `https://www.nativeform.app/api`

## Auth

- Required header: `x-nativeform-api-key: $NATIVEFORM_API_KEY`

## Common query parameters

- Pagination:
- `limit` (1-200, default 50)
- `cursor` (string; use `meta.nextCursor`)
- `order` (`asc|desc`, default `desc`)
- `sort` (`submittedAt|createdAt`, default `submittedAt`)

- Filtering:
- `status` (optional)
- `channel` (`WEB|API`, optional)
- `search` (optional; matches answers + respondent)
- `submittedAfter` / `submittedBefore` (ISO timestamp, optional)
- `embedOrigin` (optional)
- `excludeFreeEmailProviders` (boolean, optional)

- Field filters:
- Add `field.<fieldKey>=<value>` parameters (example: `field.email=acme.dev`)

## Example

```bash
FORM_ID="your-form-id"
curl -G "https://www.nativeform.app/api/forms/$FORM_ID/responses" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  --data-urlencode "limit=50" \
  --data-urlencode "status=SUBMITTED" \
  --data-urlencode "channel=API" \
  --data-urlencode "submittedAfter=2024-01-01T00:00:00Z" \
  --data-urlencode "search=acme"
```

## Response shape (high-level)

- `data[]`: responses with:
- `answers[]`: normalized `value` + `rawText` + field metadata
- `smartFields[]`: computed values (when present)
- `form`: `{ id, name, slug }`
- `meta`: `{ limit, nextCursor, count, filters }`

## Troubleshooting

- Empty `data[]`: verify `formId`, relax filters, and check form status.
- Cursor loops: stop when `meta.nextCursor` is `null`.
- `401`: missing/invalid/revoked key (see [Get an API Key](get-api-key.md)).

