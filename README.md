# TP Job Application Form — Next.js

## How it works

### Logo → Base64 (build time)
`getStaticProps` runs on the server at build time. It reads `public/tp-logo.png`,
converts it to a base64 string, and passes it to the page as a prop.
The logo is baked into the HTML — no extra HTTP request needed.

```js
export async function getStaticProps() {
  const logoBuffer = fs.readFileSync(path.join(process.cwd(), 'public', 'tp-logo.png'))
  const logoBase64 = `data:image/jpeg;base64,${logoBuffer.toString('base64')}`
  return { props: { logoBase64 } }
}
```

### Power Automate connection
On form submit, the browser POSTs a JSON payload directly to your
Power Automate HTTP trigger URL (stored in `.env.local`).

In Power Automate, the trigger is:
**"When an HTTP request is received"**

It receives the JSON, then the next action is "Create item" in SharePoint.

No backend needed — browser → Power Automate directly.

### JSON payload sent on every submission
```json
{
  "name": "Ahmad bin Razak",
  "email": "ahmad@gmail.com",
  "phone": "60-123456789",
  "nationality": "Malaysian",
  "city": "Kuala Lumpur",
  "dob": "1995-06-15",
  "language": "Mandarin",
  "other_language": "",
  "work_type": "Full-time",
  "start_date": "2025-04-01",
  "has_cv": false,
  "consent_data_processing": true,
  "consent_future_contact": true,
  "submitted_at": "2025-03-19T08:42:11.000Z",
  "page_url": "https://yourform.com?utm_source=whatsapp",
  "utm_source": "whatsapp",
  "utm_medium": "outreach",
  "utm_campaign": "mandarin_batch1",
  "source": "tp_job_application_form"
}
```

## Setup

### 1. Install dependencies
```bash
npm install
```

### 2. Set environment variables
Copy `.env.local` and fill in your values:
```
NEXT_PUBLIC_PAD_WEBHOOK_URL=https://prod-xx.westeurope.logic.azure.com:443/workflows/...
NEXT_PUBLIC_GA_MEASUREMENT_ID=G-XXXXXXXXXX
```

### 3. Run locally
```bash
npm run dev
```

### 4. Deploy to Vercel
```bash
# Option A — Vercel CLI
npx vercel --prod

# Option B — Push to GitHub
# Connect your repo to Vercel, it deploys automatically on push.
# Add env vars in Vercel dashboard → Settings → Environment Variables.
```

### 5. Add env vars on Vercel
Go to your Vercel project → Settings → Environment Variables → add:
- `NEXT_PUBLIC_PAD_WEBHOOK_URL`
- `NEXT_PUBLIC_GA_MEASUREMENT_ID`

