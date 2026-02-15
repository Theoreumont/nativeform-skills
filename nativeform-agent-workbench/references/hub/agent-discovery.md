# Agent discovery

Web docs: `https://www.nativeform.app/docs/guides/agent-discovery`

Make your pages discoverable so AI agents can figure out how to submit to an embedded NativeForm without needing to expand iframes.

## Problem

Many crawlers/agents fetch a page once and do not expand `<iframe>` contents. If your discovery signals only live inside the iframe document, they can be missed.

## Recommended fix: add companion metadata next to your iframe

Put a small, machine-readable snippet near your embed:

```html
<!-- NativeForm agent discovery (recommended) -->
<meta name="nativeform:agent-fill-protocol" content="nativeform-agent-fill-v1" />
<link
  rel="alternate"
  type="application/vnd.nativeform.ai+json"
  href="https://www.nativeform.app/api/public/forms/<slug>/ai-manifest"
/>
<script type="application/json" id="nativeform-agent-hints">
  {
    "protocol": "nativeform-agent-fill-v1",
    "manifest": "https://www.nativeform.app/api/public/forms/<slug>/ai-manifest",
    "embed": "https://www.nativeform.app/f/<slug>/embed"
  }
</script>
```

Replace `<slug>` with your public form slug.

## Optional: add hints on the iframe tag

Some scanners inspect attributes even if they do not follow iframe sources:

```html
<iframe
  src="https://www.nativeform.app/f/<slug>/embed"
  data-nativeform-agent-fill-protocol="nativeform-agent-fill-v1"
  data-nativeform-ai-manifest="https://www.nativeform.app/api/public/forms/<slug>/ai-manifest"
></iframe>
```

## Security notes

- Never expose account-level API keys in HTML.
- Treat public agent instructions as public content: guidance, not secrets.

## Acceptance criteria

A generic bot fetching only the parent HTML (no iframe expansion) can determine:

- This page embeds a NativeForm.
- Which protocol to follow.
- Where to fetch the manifest (`/ai-manifest`).

