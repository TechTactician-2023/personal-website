# Asghar Portfolio — React + Vite + Cloudflare Pages

A multi-page personal portfolio implemented with React. The first page is Contact (`/`); there is intentionally no Home page. Pages: Contact, About, Expertise, Experience, and Projects. Languages: English, Persian, Dutch, Hindi, German, and Austrian German. Persian uses right-to-left layout. The About page includes animated counters and a profile placeholder on the right on desktop.

## Option A — Direct upload to Cloudflare Pages (no build on your computer)

Use the separate archive `asghar-portfolio-react-cloudflare.zip`. Extract it and upload the **contents** of its `cloudflare-upload` folder using Cloudflare Pages Direct Upload. This version loads React/ReactDOM as ES modules from `esm.sh`, so the CDN must be reachable by site visitors.

## Option B — Build the project yourself (recommended)

Requirements: Node.js 20.19+ or 22.12+ and npm.

```bash
npm install
npm run dev
npm run build
npm run preview
```

Vite writes the production website to `dist/`. For Cloudflare Pages with Git integration, use:
- Build command: `npm run build`
- Build output directory: `dist`

For a manual upload after building, upload the **contents of `dist/`**, not the source folder.

## Project structure

- `src/main.js` — React app, navigation, language selector, route/page loading, and animated counters.
- `public/content/*.html` — content templates for each page.
- `public/content/assets/style.css` — styling.
- `public/content/assets/app.js` — translation dictionary and translation helper.
- `public/_redirects` — SPA route fallback for Cloudflare Pages.
- `public/_headers` — basic security/cache headers.

## Before publishing

1. Replace `asghar.network@example.com` in `public/content/index.html` with your actual email.
2. Replace the LinkedIn URL in `public/content/index.html` with your real profile URL.
3. Replace `public/content/assets/profile-placeholder.svg` with your own image and update the path in `public/content/about.html` if needed.
4. Verify employment history, dates, and metrics before sharing with recruiters.

## Validation notes

The source uses local npm dependencies for React and Vite; the production build must be run after `npm install`. The direct-upload archive uses CDN imports and does not need a build step. No backend or database is required.


## Admin panel and editable posts (Cloudflare Pages Functions + KV)

The management panel is available at `/admin.html` (or `/admin`). Posts are shown on `/posts`. For security and cross-visitor persistence, the admin panel requires Cloudflare Pages Functions and a KV namespace; a static drag-and-drop upload alone cannot provide secure login or shared post storage. Deploy through Git integration or Wrangler Pages so the `functions/` directory is included.

### Configure Cloudflare
1. Create a Cloudflare KV namespace and bind it to the Pages project as `PORTFOLIO_CONTENT`.
2. In Pages > Settings > Variables and Secrets, set `ADMIN_USERNAME`, `ADMIN_PASSWORD`, and a long random `SESSION_SECRET` (at least 32 random characters). Keep these as secrets, never as frontend variables. Never put credentials in frontend files.
3. Deploy the project with build command `npm run build` and output directory `dist`. Ensure the Pages project includes the root `functions/` directory.
4. Set the KV namespace binding named exactly `PORTFOLIO_CONTENT`. `wrangler.toml` contains a placeholder namespace ID; replace it if deploying with Wrangler.

### Build
```bash
npm install
npm run build
```
The admin backend runs on Cloudflare Pages Functions. A plain static upload does not execute Functions and therefore will not enable login or persistent post editing.
