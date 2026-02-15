# Agents Auth

Web docs: `https://www.nativeform.app/docs/agents/auth`

Agent workflows should authenticate to NativeForm from trusted server environments only.

## Key management best practices

- Create separate keys per environment (dev/staging/prod).
- Name keys by integration so revocation is targeted.
- Store keys in your secret manager, never in git or frontend bundles.
- Rotate keys when ownership changes or exposure is suspected.

## Required header (management API)

```text
x-nativeform-api-key: nfk_xxxxxx_secret
```

## Secure proxy pattern (recommended)

Put a thin endpoint between your agent/browser and NativeForm.

```ts
// app/api/agent/nativeform/route.ts
import { NextResponse } from "next/server"

export async function POST(request: Request) {
  const body = await request.json()

  const res = await fetch("https://www.nativeform.app/api/forms/{formId}/responses", {
    method: "POST",
    headers: {
      "x-nativeform-api-key": process.env.NATIVEFORM_API_KEY!,
      "Content-Type": "application/json",
    },
    body: JSON.stringify(body),
  })

  const data = await res.json()
  return NextResponse.json(data, { status: res.status })
}
```

## Troubleshooting

- `401`: header missing/invalid/revoked key.
- Works locally but fails in prod: verify env vars exist and are not empty.
- Intermittent failures after rotation: restart workers or clear caches.

