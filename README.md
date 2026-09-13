# FateForge V2.0

FateForge V2.0 keeps the original character-generation core and adds the first clean Visual Generator foundation.

## GitHub Pages files

Keep these five files in the GitHub Pages repository root:

- `index.html`
- `manifest.json`
- `service-worker.js`
- `icon.svg`
- `README.md`

There is **no AppDeploy backend, test site, `backend-index.ts`, or Cloudflare Worker source in the GitHub Pages repository**.

## Visual Generator architecture

```text
FateForge GitHub Pages
        ↓
Cloudflare Worker
        ↓
Google Gemini Image API
        ↓
Generated character image
```

The Gemini API key must never be placed in `index.html` or committed to GitHub. Store it as a Cloudflare Worker secret named `GEMINI_API_KEY`.

## V2.0 visual generator

The new `Generating` tab reads the saved Character Card directly and sends the character profile to the Worker. The profile is treated as canonical for visual generation.

The current frontend contains a placeholder endpoint:

```text
https://YOUR-FATEFORGE-VISUAL-WORKER.workers.dev/api/generate-character
```

After deploying the Worker, replace `VISUAL_API_URL` in `index.html` with the real Worker URL.

## Cloudflare Worker

The separate `cloudflare-worker/` folder contains the production Worker source and deployment instructions.

Create the Worker, then add the Gemini API key as a secret:

```bash
npx wrangler secret put GEMINI_API_KEY
```

Deploy:

```bash
npx wrangler deploy
```

Cloudflare Worker secrets are encrypted and are intended for sensitive API keys. Never put the Gemini key in public frontend code.

## Image model

The Worker uses Google's current Gemini native image-generation endpoint with `gemini-3.1-flash-image` (Nano Banana 2). The API is called server-side so the browser never receives the Gemini API key.
