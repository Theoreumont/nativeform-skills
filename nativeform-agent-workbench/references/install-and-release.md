# Install and Release Guide

## Goal

Keep this skill easy to install and safe to share publicly.

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
- Keep `SKILL.md` focused on user-facing usage of `nativeform.app`.
- Keep language beginner-friendly and practical.
- Do not include internal stack or system details.
- Do not include secrets or private IDs.
