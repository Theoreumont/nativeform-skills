# Agents Overview

Web docs: `https://www.nativeform.app/docs/agents/overview`

NativeForm supports agent workflows for:

- Reading normalized responses
- Submitting structured responses from free-form text
- Operating forms via typed tools (MCP) or a CLI wrapper

## Choose the right surface

- HTTP API: production server-to-server integration (canonical contracts)
- CLI: fast local debugging and reproducible steps
- MCP: tool-calling agents with typed schemas + OAuth linking
- Public Fill: public forms/embeds with no API key

## Core capabilities

- Read submissions: `GET /api/forms/{formId}/responses` ([Read Responses](read-responses.md))
- Submit responses: `POST /api/forms/{formId}/responses` ([Submit Responses](submit-responses.md))
- Key management: create/rotate/revoke ([Get an API Key](get-api-key.md))

## Recommended production architecture

- Keep NativeForm keys server-side only.
- Put a thin proxy endpoint between agent clients and NativeForm.
- Validate inputs and tool arguments before forwarding.
- Capture request IDs + status codes for debugging and prompt improvement.
- Build deterministic fallbacks when required fields are missing.

## Continue with

- [Agents Auth](agents-auth.md)
- [Public Fill](public-fill.md)
- [Agents CLI](agents-cli.md)
- [Agents MCP](agents-mcp.md)
- [Agents Skills](agents-skills.md)

