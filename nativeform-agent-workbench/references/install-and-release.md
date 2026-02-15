# Install and Release Guide

## Goal

Keep this skill easy to install and safe to share publicly while documenting NativeForm's public integration surfaces (API, CLI, MCP).

## Public repository

Public repo URL:

- `https://github.com/Theoreumont/nativeform-skills`

## Install examples

Install this skill from a user machine:

```bash
npx skills add https://github.com/Theoreumont/nativeform-skills --skill nativeform-agent-workbench
```

Manual fallback (Codex-compatible):

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench"
cp -R nativeform-agent-workbench/* "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench/"
```

After install, restart the agent client to reload the skill catalog.

## Release checklist

- Confirm frontmatter has `name` and `description`.
- Keep `SKILL.md` short and "routing-focused" (how to pick the right surface).
- Put long reference material under `references/` and link to it from `SKILL.md`.
- Do not include internal repository paths, internal architecture notes, or private system identifiers.
- Do not include secrets. Use placeholders like `$NATIVEFORM_API_KEY` and `<token>`.
- Treat signing secrets as "shown once" values and warn users to store them securely.
