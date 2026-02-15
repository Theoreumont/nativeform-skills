# Agents MCP

Web docs: `https://www.nativeform.app/docs/agents/mcp`

NativeForm exposes an MCP server at:

- `https://www.nativeform.app/mcp`

MCP is a good fit when your agent platform needs typed tools around NativeForm operations.

## Recommended runtime flow

MCP tool call -> internal service endpoint -> NativeForm API -> normalized tool response.

Keep the MCP server stateless where possible and keep secrets at the service layer.

## Tool contract (example shape)

```json
{
  "name": "nativeform_submit_response",
  "description": "Submit a free-form response to a NativeForm form",
  "input_schema": {
    "type": "object",
    "properties": {
      "formId": { "type": "string" },
      "text": { "type": "string" }
    },
    "required": ["formId", "text"]
  }
}
```

## Hosted tool inventory (high-level)

NativeForm's hosted MCP tools cover:

- Forms: create/update/publish/pause/duplicate/pin/unpin/delete, list, snapshot
- Schema: sections, fields, smart fields (create/update/reorder/delete)
- Responses: list/get/create/update/delete + smart-field processing
- Webhooks: list/create/update/delete/test + deliveries
- API keys: list/create/revoke/rotate
- Public agent-fill: manifest, draft, submit, confirm

See the full mapping (API + CLI + MCP tool names):

- [Capabilities map](../capabilities.md)

## Hardening checklist

- Validate every tool input (types, lengths, required fields).
- Scope tools by tenant/environment/user role.
- Rate limit tool calls per session to prevent runaway loops.
- Return structured errors so the model can recover safely.

