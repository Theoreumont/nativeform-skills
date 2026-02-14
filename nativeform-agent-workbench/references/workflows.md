# NativeForm Agent Workflows

## Workflow A: Public form fill

1. Read manifest from `GET /api/public/forms/{slug}/ai-manifest`.
2. Validate answers against field types and validation constraints.
3. Submit draft to `POST /api/public/forms/{slug}/drafts` when iterative collection is needed.
4. Submit final payload to `POST /api/public/forms/{slug}/submissions`.
5. If confirmation is required, present tokenized confirm flow:
   - `GET /api/public/confirm/{token}`
   - `POST /api/public/confirm/{token}`

Done when:

- HTTP status is `201`.
- Response includes `responseId`.
- Validation failures are surfaced as typed field errors.

## Workflow B: Management actions with parity discipline

For any requested action (publish, pause, responses, webhooks, keys, fields):

1. Check the API endpoint contract.
2. Prefer the equivalent `nativeform` CLI command.
3. Confirm matching MCP tool exists for parity.
4. Confirm parity test exists in `tests/parity`.

Done when:

- API + CLI + MCP + parity test all align.

## Workflow C: Robust retries and support output

1. Parse normalized error body.
2. On `422`, return exact field-level fixes.
3. On `429`, apply backoff using `Retry-After`.
4. On auth errors, call out missing or invalid scope.
5. Always provide next action in plain language.

## Workflow D: Agent attribution and analytics hygiene

1. Send `x-nativeform-agent` on agent requests.
2. Include version/channel headers when available.
3. Preserve server-side attribution (`source=agent`, agent metadata).
4. Record transport context (`api`, `cli`, `mcp`, `skill`) in logs and reporting.
