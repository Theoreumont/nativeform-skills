# NativeForm Workflows (API / CLI / MCP)

Use these playbooks to guide a user (or an agent runtime) through NativeForm operations across the same underlying capabilities.

## Workflow 0: Pick the right surface

- Web UI (`nativeform.app`): best for interactive setup and non-technical users.
- CLI (`nativeform ...`): best for debugging, reproducible steps, and quick iteration.
- HTTP API (`/api/...`): best for production server-to-server integrations.
- MCP (`/mcp`): best for tool-calling agents with OAuth linking and typed schemas.

Default decision:

- If the user says "I want to test quickly": start with CLI.
- If the user says "I need to ship this in production": design around HTTP API + a server proxy.
- If the user says "I want ChatGPT/Claude to operate my account": use MCP.

## Workflow A: Public agent-fill (no API key)

Goal: submit a response to a public form using the agent-fill protocol.

Steps:

1. Fetch the manifest: `GET /api/public/forms/{slug}/ai-manifest`
2. Build answers using `fields[]` (types, required flags, validations).
3. Create a draft when you need iterative collection: `POST /api/public/forms/{slug}/drafts`
4. Submit the final payload: `POST /api/public/forms/{slug}/submissions`
5. If confirmation is required, use the confirm flow:

- `GET /api/public/confirm/{token}` (inspect / display for the user)
- `POST /api/public/confirm/{token}` (finalize)

CLI equivalents:

```bash
nativeform public manifest <slug>
nativeform public draft <slug> --answers '[{"fieldKey":"email","value":"ada@acme.dev"}]'
nativeform public submit <slug> --draft-id <draftId>
nativeform public confirm get <token>
nativeform public confirm submit <token>
```

Success looks like:

- HTTP `201` and a `responseId` (and often a `draftId` when drafts are used).
- On `422`, you can map `error.fieldErrors` back to the manifest fields and request corrections.

## Workflow B: Submit a response to a private form (management API)

Goal: convert free-form text into a structured response for a specific `formId`.

HTTP API:

```bash
FORM_ID="your-form-id"
curl -X POST "https://www.nativeform.app/api/forms/$FORM_ID/responses" \
  -H "x-nativeform-api-key: $NATIVEFORM_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text":"Ada from ACME, email ada@acme.dev"}'
```

CLI equivalent:

```bash
nativeform responses create <formId> --text "Ada from ACME, email ada@acme.dev"
```

Success looks like:

- HTTP `201` with a `responseId`.
- Missing required fields return a typed error; ask the user for the missing pieces and retry with an updated `text` or `answers`.

## Workflow C: Form lifecycle (create, publish, pause, duplicate, pin)

Goal: manage a form programmatically and keep changes reproducible.

CLI-first steps:

```bash
# Create
nativeform forms create --name "Customer intake" --visibility PUBLIC

# Publish / pause
nativeform forms publish <formId>
nativeform forms pause <formId>

# Duplicate / pin / unpin
nativeform forms duplicate <formId>
nativeform forms pin <formId>
nativeform forms unpin <formId>

# Delete (destructive)
nativeform forms delete <formId> --yes
```

MCP equivalents (tool names):

- `init_form`, `update_form`, `publish_form`, `pause_form`, `duplicate_form`, `pin_form`, `unpin_form`, `delete_form`

Success looks like:

- The form appears in `nativeform forms ls` (or via `GET /api/forms`).

## Workflow D: Form schema edits (sections, fields, smart fields)

Goal: evolve a form schema safely while keeping ordering explicit.

Sections (CLI):

```bash
nativeform forms sections ls <formId>
nativeform forms sections add <formId> --slug contact --title "Contact details"
nativeform forms sections update <formId> <sectionId> --title "Contact"
nativeform forms sections reorder <formId> --section-ids id1,id2,id3
nativeform forms sections delete <formId> <sectionId> --yes
```

Fields (CLI):

```bash
nativeform forms fields ls <formId>
nativeform forms fields add <formId> --key email --label "Email" --type EMAIL --required true
nativeform forms fields update <formId> <fieldId> --label "Work email"
nativeform forms fields reorder <formId> --items '[{"key":"email","order":1}]'
nativeform forms fields delete <formId> <fieldId> --yes
```

Smart fields (CLI):

```bash
nativeform forms smart-fields ls <formId>
nativeform forms smart-fields add <formId> --key sentiment --label "Sentiment" --prompt "Classify sentiment"
nativeform forms smart-fields update <formId> <smartFieldId> --prompt "Classify sentiment (positive/neutral/negative)"
nativeform forms smart-fields delete <formId> <smartFieldId> --yes
```

MCP equivalents (tool names):

- Sections: `create_form_section`, `update_form_section`, `reorder_form_sections`, `delete_form_section`, `move_section`
- Fields: `create_form_field`, `update_form_field`, `reorder_form_fields`, `delete_form_field`, `move_field`
- Smart fields: `create_smart_field`, `update_smart_field`, `delete_smart_field`

Success looks like:

- `get_form_snapshot` (MCP) or the dashboard shows the updated schema and ordering.

## Workflow E: Webhooks (create, test, deliveries)

Goal: deliver submissions to external systems reliably.

CLI-first steps:

```bash
nativeform webhooks ls <formId>
nativeform webhooks create <formId> --url https://example.com/hook --integration-type MANUAL
nativeform webhooks test <formId> <webhookId>
nativeform webhooks deliveries <formId> <webhookId> --limit 20
nativeform webhooks update <formId> <webhookId> --is-active false
nativeform webhooks delete <formId> <webhookId> --yes
```

MCP equivalents (tool names):

- `list_form_webhooks`, `create_form_webhook`, `update_form_webhook`, `delete_form_webhook`, `test_form_webhook`, `list_form_webhook_deliveries`

Success looks like:

- A `test` delivery succeeds (or shows a clear error response to debug downstream).

## Workflow F: API key hygiene (create, revoke, rotate)

Goal: isolate integrations and minimize blast radius.

CLI steps:

```bash
nativeform api-keys ls
nativeform api-keys create --name "CI debugging key"
nativeform api-keys rotate <keyId>
nativeform api-keys revoke <keyId> --yes
```

MCP equivalents (tool names):

- `list_api_keys`, `create_api_key`, `rotate_api_key`, `revoke_api_key`

Success looks like:

- The old key stops working immediately after revoke.

## Workflow G: Troubleshooting by status code

- `401`: key missing/invalid/revoked. Verify header name and that the key belongs to the correct account.
- `403` on public fill: public agent filling is disabled for the form; the form owner must enable it.
- `404`: wrong `formId`/`slug`, or the form is not active.
- `422`: validation failure. Use `error.fieldErrors` and the manifest/schema to prompt the user for corrections.
- `429`: rate limited. Respect `Retry-After` and back off.

