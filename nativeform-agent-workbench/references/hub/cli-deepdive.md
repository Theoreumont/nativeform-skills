# `nativeform` CLI deep dive

The CLI is a wrapper around the NativeForm HTTP API.

If anything in this page is out of date, the CLI help is canonical:

```bash
nativeform --help
```

## Install

```bash
# npm
npm i -g @theodevs/nativeform-cli

# bun
bun add -g @theodevs/nativeform-cli
```

## Auth

Recommended:

```bash
nativeform login
```

Alternatives:

- Env var: `NATIVEFORM_API_KEY`
- Per-command: `--token <api-key>`

## Global options

- `--token <api-key>`: API key (defaults to `NATIVEFORM_API_KEY`)
- `--base-url <url>`: defaults to `NATIVEFORM_BASE_URL` or `https://www.nativeform.app`
- `--json`: raw JSON output
- `--yes`: skip confirmation prompts
- `--debug`: request debug details

## Command catalog

### Session

- `nativeform login`
- `nativeform logout`

### Forms

- `nativeform forms ls [--limit N] [--status STATUS] [--search TEXT]`
- `nativeform forms create --name NAME [--slug SLUG] [--description TEXT] [--visibility VISIBILITY] [--status STATUS]`
- `nativeform forms update <formId> [--name NAME] [--slug SLUG] [--description TEXT] [--visibility VISIBILITY] [--status STATUS]`
- `nativeform forms publish <formId>`
- `nativeform forms pause <formId>`
- `nativeform forms duplicate <formId>`
- `nativeform forms pin <formId>`
- `nativeform forms unpin <formId>`
- `nativeform forms delete <formId> [--yes]`

### Form AI settings

- `nativeform forms ai update <formId> [--introduction-message TEXT] [--instructions TEXT] [--context TEXT]`
- `nativeform forms ai update <formId> --agent-filling-enabled true|false --allow-direct-agent-submit true|false`

### Sections

- `nativeform forms sections ls <formId>`
- `nativeform forms sections add <formId> --slug SLUG --title TITLE [--description TEXT] [--ai-instructions TEXT]`
- `nativeform forms sections update <formId> <sectionId> [--slug SLUG] [--title TITLE] [--description TEXT]`
- `nativeform forms sections reorder <formId> --section-ids id1,id2,id3`
- `nativeform forms sections delete <formId> <sectionId> [--yes]`

### Fields

- `nativeform forms fields ls <formId>`
- `nativeform forms fields add <formId> --key KEY --label LABEL --type TYPE [--section-id SECTION_ID]`
- `nativeform forms fields update <formId> <fieldId> [--key KEY] [--label LABEL] [--type TYPE] [--section-id SECTION_ID]`
- `nativeform forms fields reorder <formId> --items JSON`
- `nativeform forms fields delete <formId> <fieldId> [--yes]`

### Smart fields

- `nativeform forms smart-fields ls <formId>`
- `nativeform forms smart-fields add <formId> --key KEY --label LABEL --prompt TEXT [--complexity LEVEL] [--output-type TYPE] [--options JSON]`
- `nativeform forms smart-fields update <formId> <smartFieldId> --key KEY --label LABEL --prompt TEXT [--complexity LEVEL] [--output-type TYPE] [--options JSON]`
- `nativeform forms smart-fields delete <formId> <smartFieldId> [--yes]`

### Responses

- `nativeform responses ls <formId> [--limit N] [--search TEXT] [--status STATUS]`
- `nativeform responses get <formId> <responseId>`
- `nativeform responses create <formId> --text TEXT [--respondent-name NAME] [--respondent-email EMAIL] [--respondent-external-id ID]`
- `nativeform responses update <formId> <responseId> [--status STATUS] [--metadata JSON] [--answers JSON]`
- `nativeform responses delete <formId> <responseId> [--yes]`
- `nativeform responses process-smart-field <formId> <responseId>`

### Webhooks

- `nativeform webhooks ls <formId>`
- `nativeform webhooks create <formId> --url URL [--name NAME] [--integration-type MANUAL|MAKE|N8N|ZAPIER]`
- `nativeform webhooks update <formId> <webhookId> [--url URL] [--name NAME] [--is-active true|false]`
- `nativeform webhooks test <formId> <webhookId>`
- `nativeform webhooks deliveries <formId> <webhookId> [--limit N] [--offset N]`
- `nativeform webhooks delete <formId> <webhookId> [--yes]`

### API keys

- `nativeform api-keys ls`
- `nativeform api-keys create [--name NAME]`
- `nativeform api-keys revoke <keyId> [--yes]`
- `nativeform api-keys rotate <keyId>`

### Public agent fill (no API key)

- `nativeform public manifest <slug>`
- `nativeform public draft <slug> --answers JSON [--respondent JSON] [--metadata JSON]`
- `nativeform public submit <slug> [--draft-id ID] [--answers JSON] [--respondent JSON] [--metadata JSON]`
- `nativeform public confirm get <token>`
- `nativeform public confirm submit <token>`

### Escape hatch (call any API path)

- `nativeform api call <METHOD> <PATH> [--body JSON]`

Example:

```bash
nativeform api call GET /api/forms --json
```

