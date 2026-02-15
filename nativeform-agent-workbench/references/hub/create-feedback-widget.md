# Create feedback widget

Web docs: `https://www.nativeform.app/docs/guides/create-feedback-widget`

Build an in-app feedback widget that submits to NativeForm via the Submit Response API.

## Architecture (recommended)

1. Frontend widget collects feedback (text + optional context).
2. Your backend receives widget payload (no NativeForm key).
3. Your backend calls NativeForm:
- `POST https://www.nativeform.app/api/forms/{formId}/responses`
- With `x-nativeform-api-key` from env vars

Never expose API keys in frontend code.

## Suggested form structure

Example fields:

- `user_email` (EMAIL, required)
- `user_fullname` (SHORT_TEXT, required)
- `feedback` (LONG_TEXT, required)

Example smart fields (optional):

- `view` (ENUM): where the feedback was submitted from
- `type` (ENUM): feature request vs bug, etc.
- `urgency` (ENUM): low/medium/high

## Backend example (Next.js route)

```ts
import { NextRequest, NextResponse } from "next/server"

const FORM_ID = process.env.NATIVEFORM_FORM_ID!
const API_KEY = process.env.NATIVEFORM_API_KEY!

export async function POST(req: NextRequest) {
  const { feedback, email, name, view, pageUrl } = await req.json()

  const res = await fetch(`https://www.nativeform.app/api/forms/${FORM_ID}/responses`, {
    method: "POST",
    headers: {
      "x-nativeform-api-key": API_KEY,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      text: feedback,
      respondent: { email, name },
      metadata: {
        source: "widget",
        view: view || "unknown",
        pageUrl: pageUrl || req.headers.get("referer") || null,
      },
    }),
  })

  const data = await res.json()
  return NextResponse.json(data, { status: res.status })
}
```

## Error handling

- Show a friendly error to the user; log the full response server-side.
- Treat `401` as a key/config problem and `422` as missing/invalid user data.

