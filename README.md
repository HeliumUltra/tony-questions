# T&R Website — Tony's Fact Check form

A self-contained single-page fact-check form for Tony, served as `index.html`.
Separate from the tandrauto website — this repo/project is **only** this form.

- No build step, no framework. One static `index.html` (plus Google Fonts).
- `<meta name="robots" content="noindex, nofollow">` is in the `<head>`, and
  `vercel.json` + `robots.txt` add belt-and-suspenders no-index headers so the
  URL never shows up in search.
- Deployment protection (Vercel Authentication) **must be OFF** so the client
  doesn't hit a login wall.

## How answers come back to you

When Tony taps **Finish**, the form:

1. **Emails the answers to Matthew automatically** via [Web3Forms](https://web3forms.com)
   (a free form-to-email relay — no backend, no account required), and
2. **Downloads a `tony-fact-check-answers.txt` copy** to his device as a backup.

### One-time setup — the Web3Forms access key (required for auto-email)

Until a real key is set, the form still works but only downloads the file.

1. Go to https://web3forms.com
2. Enter **matthew@heliumultra.com** as the destination email.
3. The access key is emailed to you instantly (no account needed).
4. In `index.html`, replace the placeholder:

   ```js
   const WEB3FORMS_ACCESS_KEY = "REPLACE_WITH_WEB3FORMS_ACCESS_KEY";
   ```

   with your real key, then redeploy. Every finished form then lands in the
   `matthew@heliumultra.com` inbox.

## Deploying to Vercel (project: `tandrauto-factcheck`)

### Option A — Import this repo (no CLI)
1. Vercel dashboard → **Add New… → Project**.
2. Import `heliumultra/tony-questions`, branch `claude/deploy-factcheck-form-8ch3rz`
   (or after merge, the default branch).
3. Framework preset: **Other**. Root directory: `/`. Deploy.
4. **Settings → Deployment Protection → turn Vercel Authentication OFF.**
5. Rename the project to `tandrauto-factcheck` if desired (Settings → General).

### Option B — Vercel CLI
```bash
npm i -g vercel
vercel --prod        # from the repo root; name the project tandrauto-factcheck
vercel project ...   # then disable Deployment Protection in the dashboard
```

## Verify (logged-out)
```bash
curl -sSI https://<your-deployment>.vercel.app/        # expect HTTP/2 200, not a login redirect
curl -sS  https://<your-deployment>.vercel.app/ | grep -i "Tony's Fact Check"
curl -sS  https://<your-deployment>.vercel.app/ | grep -i "Finish"
```
A 200 with the `<title>T&R Website — Tony's Fact Check</title>` and the
"Finish & Download Answers" button means protection is off and the form is live.
