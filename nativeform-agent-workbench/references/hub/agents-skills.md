# Agents Skills

Web docs: `https://www.nativeform.app/docs/agents/skills`

Skills are reusable instruction packs that keep agent behavior consistent across runs.

## Recommended structure

```text
my-skill/
  SKILL.md
  references/
    request-contracts.md
    troubleshooting.md
  scripts/
    validate-input.ts
```

Principles:

- Keep `SKILL.md` short and routing-focused.
- Link supporting references instead of embedding long dumps.
- Prefer deterministic scripts for repetitive transformations.
- Define success criteria and safety guardrails.

## Install this skill

```bash
npx skills add https://github.com/theoreumont/nativeform-skills --skill nativeform-agent-workbench
```

Manual fallback:

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench"
cp -R nativeform-agent-workbench/* "${CODEX_HOME:-$HOME/.codex}/skills/nativeform-agent-workbench/"
```

## Versioning tips

- Treat skills like interfaces: stable, explicit, and testable.
- Tag breaking prompt/output changes and keep a stable set of before/after tasks.

