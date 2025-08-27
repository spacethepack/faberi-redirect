# GitHub Pages Static Redirect

This repository implements a simple **client-side redirect** suitable for GitHub Pages.
It sends visitors to a full URL (including path), which native DNS cannot do.

> If you need a true HTTP 302/301 status code, use a proxy/edge platform
> (e.g., Cloudflare Workers, Netlify, or Vercel). GitHub Pages serves static files only.

---

## Quick Start

1. **Edit your destination URL**
   - Open `index.html`
   - Find the line with `const DESTINATION_URL = "...";`
   - Replace it with your target, e.g.:
     ```js
     const DESTINATION_URL = "https://service.example.com/some/path?x=1#hash";
     ```
   - Also update the meta-refresh fallback near the top:
     ```html
     <meta http-equiv="refresh" content="0; url=https://service.example.com/some/path?x=1#hash" />
     ```

2. **(Optional) Use a custom domain**
   - Edit the `CNAME` file (all caps, no extension) to contain your domain only:
     ```
     redirect.example.com
     ```
   - In your DNS, add a **CNAME record** like:
     ```
     host:  redirect
     type:  CNAME
     value: <your-username>.github.io
     ```
   - Commit and push.

3. **Enable GitHub Pages**
   - In your repo: **Settings → Pages**
   - **Build and deployment**: *Deploy from branch*
   - **Branch**: `main` (or `master`) ; **Folder**: `/ (root)`
   - Save and wait for deploy to finish.

4. **(Optional) Add `.nojekyll`**
   - We include `.nojekyll` to prevent GitHub Pages from running Jekyll.
     This keeps files/folders beginning with underscores from being filtered.

---

## Files

- `index.html` — Performs the redirect using JavaScript + meta refresh fallback.
- `CNAME` — (Optional) Declares the custom domain that GitHub Pages should serve this repo on.
- `.nojekyll` — Disables Jekyll processing on GitHub Pages.
- `README.md` — These instructions.

---

## Notes

- This is **client-side** redirection; GitHub Pages cannot emit a real 301/302.
- If you require a real HTTP 302:
  - **Cloudflare Workers** (very flexible; free tier available)
  - **Netlify** (`_redirects` file) or **Vercel** (config or serverless function).
