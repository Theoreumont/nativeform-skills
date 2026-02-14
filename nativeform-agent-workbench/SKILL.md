---
name: nativeform-agent-workbench
description: Runs NativeForm agent workflows using CLI-first execution with API fallback, including public form fill, management operations, parity checks, and normalized error handling. Use when users ask to operate NativeForm via agents, MCP, CLI, or installable skill flows.
metadata:
  tags: nativeform, agent, cli, mcp, api, forms
---

# NativeForm Agent Workbench

## When to use

Use this skill when the task is about NativeForm agent operations, especially:

- Running form management from agent tooling.
- Executing public AI form-fill flows.
- Keeping API, CLI, MCP, and parity behavior aligned.
- Preparing workflows for Codex and `skills` ecosystem installs.

## Execution policy

Always use this order:

1. CLI (`nativeform ...`) when available.
2. HTTP API endpoint fallback when CLI is not available.
3. Confirm-link fallback for constrained environments.

Do not invent hidden/private behavior. Use documented contracts only.

## Required request identity

For agent-originated HTTP calls, include:

- `x-nativeform-agent: <agent-name>` (required)
- `x-nativeform-agent-version: <version>` (optional)
- `x-nativeform-agent-channel: api|cli|mcp|skill|other` (optional)
- `User-Agent` aligned with agent name/version (recommended)

For management APIs, use one auth mode:

- `x-nativeform-api-key: <key>`
- `Authorization: Bearer <token>`

## Core workflows

### 1) Public agent fill

1. Fetch manifest: `GET /api/public/forms/{slug}/ai-manifest`.
2. Build answers from `fields[]` + `validation`.
3. Optionally create draft: `POST /api/public/forms/{slug}/drafts`.
4. Submit: `POST /api/public/forms/{slug}/submissions`.
5. If confirmation required, guide user through `/api/public/confirm/{token}`.

Success target: `201` with `responseId`.

Validation target: `422` with typed `fieldErrors`.

### 2) Form management parity

For each action, confirm parity mapping exists:

- Web action
- API endpoint
- CLI command
- MCP tool
- Parity test

Treat parity as complete only when all five exist.

### 3) Error handling and retries

Expect normalized error body:

- `error.code`
- `error.message`
- `error.fieldErrors` (when validation related)
- `error.hint`
- `error.docs_url`

On `429`, respect `Retry-After` and rate-limit headers.

## Safety and guardrails

- Never include secrets in docs, examples, or committed output.
- Never bypass scope checks.
- Never change business logic in the skill itself; orchestrate existing surfaces.
- Keep output explicit and beginner-friendly.

## Success checklist

- Uses documented endpoint contracts.
- Applies required agent identity headers.
- Returns actionable remediation when failing.
- Preserves CLI-first/API-fallback behavior.

## References

- [references/contracts.md](references/contracts.md)
- [references/workflows.md](references/workflows.md)
- [references/install-and-release.md](references/install-and-release.md)
