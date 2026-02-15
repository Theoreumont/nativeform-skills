# Agents CLI

Web docs: `https://www.nativeform.app/docs/agents/cli`

The `nativeform` CLI is the fastest way to validate payloads, reproduce issues, and script one-off operations from a terminal.

## Install

Choose one:

```bash
# npm
npm i -g @theodevs/nativeform-cli

# bun
bun add -g @theodevs/nativeform-cli
```

## Authenticate

```bash
nativeform login
```

Alternatives:

- Set `NATIVEFORM_API_KEY` in your environment
- Or pass `--token` per command

## Common commands

```bash
nativeform forms ls --json
nativeform responses ls <formId>
nativeform responses create <formId> --text "Ada from ACME, email ada@acme.dev"
nativeform webhooks ls <formId>
nativeform api-keys ls
```

## Safety checklist

- Prefer environment variables over passing keys inline (inline keys may show in process lists).
- Rotate keys after any screen share / recording / log leak.

## Full command list

- See: [nativeform CLI deep dive](cli-deepdive.md)
- Or run: `nativeform --help`

