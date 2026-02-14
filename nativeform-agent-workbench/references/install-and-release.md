# Install and Release Guide

## Goal

Make this skill consumable from public repositories so people can install it using tools such as:

- `npx skills add <public-repo-url> --skill nativeform-agent-workbench`

## Public repository requirement

This repository is private. To distribute publicly:

1. Create a separate public repository (example: `nativeform/skills`).
2. Copy the `nativeform-agent-workbench` folder to that repository.
3. Keep `SKILL.md` at `nativeform-agent-workbench/SKILL.md` in the public repo.

Recommended public repo layout:

```text
nativeform-skills/
  nativeform-agent-workbench/
    SKILL.md
    references/
      contracts.md
      workflows.md
      install-and-release.md
    agents/
      openai.yaml
```

## Install examples

From a user machine:

```bash
npx skills add https://github.com/<org>/<repo> --skill nativeform-agent-workbench
```

Manual Codex-compatible install (fallback):

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench"
cp -R nativeform-agent-workbench/* "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench/"
```

After install, restart the agent client to reload the skill catalog.

## Release checklist

- Confirm frontmatter has `name` and `description`.
- Keep `SKILL.md` concise and actionable.
- Ensure references are relative and one level deep.
- Remove internal-only URLs, private IDs, and secrets.
- Tag release notes when behavior contracts change.
