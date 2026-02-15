# NativeForm Capabilities Map (API / CLI / MCP)

This is a compact inventory of what NativeForm exposes for developer and agent workflows, mapped across:

- HTTP API (canonical)
- CLI (`nativeform ...`) wrapper
- MCP tools (typed tool surface)

## Base URLs

- Web UI + docs: `https://www.nativeform.app`
- Management API base: `https://www.nativeform.app/api`
- Public agent-fill base: `https://www.nativeform.app/api/public`
- MCP endpoint: `https://www.nativeform.app/mcp`

## Auth + identity

Management API:

- Header: `x-nativeform-api-key: <api-key>`
- Alternative: `Authorization: Bearer <oauth-access-token>` (MCP-linked sessions)

Public agent-fill:

- No API key
- Agent attribution headers:
- `x-nativeform-agent` (required)
- `x-nativeform-agent-version` (optional)
- `x-nativeform-agent-channel` (optional; `api|cli|mcp|skill|other`)

CLI:

- `nativeform login` stores a key locally
- Env vars:
- `NATIVEFORM_API_KEY`
- `NATIVEFORM_BASE_URL`

## API keys

HTTP API:

- `GET /api/api-keys`
- `POST /api/api-keys`
- `POST /api/api-keys/{keyId}/revoke`
- `POST /api/api-keys/{keyId}/rotate`

CLI:

- `nativeform api-keys ls`
- `nativeform api-keys create --name "Automation key"`
- `nativeform api-keys revoke <keyId> --yes`
- `nativeform api-keys rotate <keyId>`

MCP tools:

- `list_api_keys`
- `create_api_key`
- `revoke_api_key`
- `rotate_api_key`

## Forms

HTTP API:

- `GET /api/forms`
- `POST /api/forms`
- `GET /api/forms/{formId}`
- `PATCH /api/forms/{formId}`
- `DELETE /api/forms/{formId}`
- `POST /api/forms/{formId}/publish`
- `POST /api/forms/{formId}/pause`
- `POST /api/forms/{formId}/duplicate`
- `POST /api/forms/{formId}/pin`
- `POST /api/forms/{formId}/unpin`

CLI:

- `nativeform forms ls`
- `nativeform forms create --name "..." --visibility PUBLIC|PRIVATE`
- `nativeform forms update <formId> ...`
- `nativeform forms publish <formId>`
- `nativeform forms pause <formId>`
- `nativeform forms duplicate <formId>`
- `nativeform forms pin <formId>`
- `nativeform forms unpin <formId>`
- `nativeform forms delete <formId> --yes`

MCP tools:

- `list_forms`
- `get_form_snapshot`
- `get_form_settings`
- `init_form` (create a full form in one call; may return a widget)
- `update_form` (batch edits for sections/fields/smart fields; may return a widget)
- `set_form_name`
- `publish_form`
- `pause_form`
- `duplicate_form`
- `pin_form`
- `unpin_form`
- `delete_form`

## Form AI settings (public agent-fill policy)

HTTP API:

- `PATCH /api/forms/{formId}/ai-settings`

CLI:

- `nativeform forms ai update <formId> --agent-filling-enabled true|false --allow-direct-agent-submit true|false`

MCP tools:

- `update_form_ai_config`

## Sections

HTTP API:

- `GET /api/forms/{formId}/sections`
- `POST /api/forms/{formId}/sections`
- `PATCH /api/forms/{formId}/sections/{sectionId}`
- `DELETE /api/forms/{formId}/sections/{sectionId}`
- `POST /api/forms/{formId}/sections/reorder`

CLI:

- `nativeform forms sections ls <formId>`
- `nativeform forms sections add <formId> --slug ... --title ...`
- `nativeform forms sections update <formId> <sectionId> ...`
- `nativeform forms sections reorder <formId> --section-ids id1,id2,id3`
- `nativeform forms sections delete <formId> <sectionId> --yes`

MCP tools:

- `create_form_section`
- `update_form_section`
- `reorder_form_sections`
- `delete_form_section`
- `move_section`

## Fields

HTTP API:

- `GET /api/forms/{formId}/fields`
- `POST /api/forms/{formId}/fields`
- `PATCH /api/forms/{formId}/fields/{fieldId}`
- `DELETE /api/forms/{formId}/fields/{fieldId}`
- `POST /api/forms/{formId}/fields/reorder`

CLI:

- `nativeform forms fields ls <formId>`
- `nativeform forms fields add <formId> --key ... --label ... --type ...`
- `nativeform forms fields update <formId> <fieldId> ...`
- `nativeform forms fields reorder <formId> --items <json>`
- `nativeform forms fields delete <formId> <fieldId> --yes`

MCP tools:

- `create_form_field`
- `update_form_field`
- `reorder_form_fields`
- `delete_form_field`
- `move_field`

## Smart fields (AI-computed fields)

HTTP API:

- `GET /api/forms/{formId}/smart-fields`
- `POST /api/forms/{formId}/smart-fields`
- `PATCH /api/forms/{formId}/smart-fields/{smartFieldId}`
- `DELETE /api/forms/{formId}/smart-fields/{smartFieldId}`

CLI:

- `nativeform forms smart-fields ls <formId>`
- `nativeform forms smart-fields add <formId> --key ... --label ... --prompt ...`
- `nativeform forms smart-fields update <formId> <smartFieldId> ...`
- `nativeform forms smart-fields delete <formId> <smartFieldId> --yes`

MCP tools:

- `create_smart_field`
- `update_smart_field`
- `delete_smart_field`

## Responses

HTTP API:

- `GET /api/forms/{formId}/responses`
- `POST /api/forms/{formId}/responses`
- `GET /api/forms/{formId}/responses/{responseId}`
- `PATCH /api/forms/{formId}/responses/{responseId}`
- `DELETE /api/forms/{formId}/responses/{responseId}`
- `POST /api/forms/{formId}/responses/{responseId}/smart-fields/process`

CLI:

- `nativeform responses ls <formId>`
- `nativeform responses get <formId> <responseId>`
- `nativeform responses create <formId> --text "..."`
- `nativeform responses update <formId> <responseId> --status SUBMITTED`
- `nativeform responses delete <formId> <responseId> --yes`
- `nativeform responses process-smart-field <formId> <responseId>`

MCP tools:

- `get_form_responses`
- `get_form_response`
- `create_form_response`
- `update_form_response`
- `delete_form_response`
- `process_response_smart_fields`

## Webhooks

HTTP API:

- `GET /api/forms/{formId}/webhooks`
- `POST /api/forms/{formId}/webhooks`
- `PATCH /api/forms/{formId}/webhooks/{webhookId}`
- `DELETE /api/forms/{formId}/webhooks/{webhookId}`
- `POST /api/forms/{formId}/webhooks/{webhookId}/test`
- `GET /api/forms/{formId}/webhooks/{webhookId}/deliveries`

CLI:

- `nativeform webhooks ls <formId>`
- `nativeform webhooks create <formId> --url https://example.com/hook --integration-type MANUAL`
- `nativeform webhooks update <formId> <webhookId> --is-active false`
- `nativeform webhooks test <formId> <webhookId>`
- `nativeform webhooks deliveries <formId> <webhookId> --limit 20`
- `nativeform webhooks delete <formId> <webhookId> --yes`

MCP tools:

- `list_form_webhooks`
- `create_form_webhook`
- `update_form_webhook`
- `delete_form_webhook`
- `test_form_webhook`
- `list_form_webhook_deliveries`

## Public agent-fill protocol (no API key)

HTTP API:

- `GET /api/public/forms/{slug}/ai-manifest`
- `POST /api/public/forms/{slug}/drafts`
- `POST /api/public/forms/{slug}/submissions`
- `GET /api/public/confirm/{token}`
- `POST /api/public/confirm/{token}`

CLI:

- `nativeform public manifest <slug>`
- `nativeform public draft <slug> --answers <json>`
- `nativeform public submit <slug> --draft-id <draftId>`
- `nativeform public confirm get <token>`
- `nativeform public confirm submit <token>`

MCP tools:

- `get_public_form_manifest`
- `create_public_form_draft`
- `submit_public_form`
- `get_public_submission_confirmation`
- `confirm_public_submission`

## Escape hatch: call any API path

CLI:

- `nativeform api call <METHOD> <PATH> [--body JSON] [--json]`

