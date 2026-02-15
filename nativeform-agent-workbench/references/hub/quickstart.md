# Quickstart

Web docs: `https://www.nativeform.app/docs/get-started` (index: `https://www.nativeform.app/docs`)

This is the shortest path to integrating NativeForm as a developer.

## 1) Get an API key

- Follow: [Get an API Key](get-api-key.md)
- You will use it as the `x-nativeform-api-key` header from your backend.

## 2) Find your `formId`

- Open a form in the NativeForm dashboard.
- The `formId` is in the URL: `/form/{formId}`.

## 3) Read responses (server-to-server)

- Guide: [Read Responses](read-responses.md)

Example:

```bash
FORM_ID="your-form-id"
curl -G "https://www.nativeform.app/api/forms/$FORM_ID/responses" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  --data-urlencode "limit=10"
```

## 4) Submit responses (server-to-server)

- Guide: [Submit Responses](submit-responses.md)

Example:

```bash
FORM_ID="your-form-id"
curl -X POST "https://www.nativeform.app/api/forms/$FORM_ID/responses" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text":"Ada from ACME, email ada@acme.dev"}'
```

## 5) Optional: create a feedback widget

- Guide: [Create feedback widget](create-feedback-widget.md)

## 6) Optional: agent integrations

- Start here: [Agents Overview](agents-overview.md)
- Public submissions without an API key: [Public Fill](public-fill.md)
- Tool calling (typed tools): [Agents MCP](agents-mcp.md)
- Reproducible debugging: [Agents CLI](agents-cli.md)

