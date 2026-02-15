# Get an API Key

Web docs: `https://www.nativeform.app/docs/api-key` (keys UI: `https://www.nativeform.app/docs/my-api-keys`)

NativeForm API keys authenticate management API requests (forms, responses, webhooks, keys).

## Where to create/manage keys

- NativeForm UI: open `https://www.nativeform.app/docs/my-api-keys`
- Or from any form: Export tab -> API access

## Key format + required header

- Keys look like: `nfk_<prefix>_<secret>`
- Send this header on every management API request:

```text
x-nativeform-api-key: nfk_xxxxxx_secret
```

## Safety rules (must follow)

- Never put API keys in frontend code.
- Never paste API keys into chat, tickets, or logs.
- Store keys in your server secret manager (Vercel env vars, etc.).
- Create separate keys per integration and per environment.
- Rotate keys after any accidental exposure.

## CLI alternative (stores a local key)

If you use the `nativeform` CLI:

```bash
nativeform login
```

This opens the keys page in your browser and prompts you to paste a key locally.

## Troubleshooting

- `401 Unauthorized`: header missing, wrong header name, key revoked/rotated, or wrong account.
- Works locally but not in prod: verify prod environment variables are set (and not set to empty).

