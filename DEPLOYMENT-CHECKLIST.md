# Cloudflare Pages deployment checklist

## Required for secure admin login and persistent posts

1. Push this project to a GitHub repository and connect it to Cloudflare Pages, or deploy with Wrangler. A drag-and-drop static upload cannot run Pages Functions.
2. Build command: `npm run build`
3. Build output directory: `dist`
4. Create a Cloudflare KV namespace and bind it in Pages Settings > Functions > KV namespace bindings as `PORTFOLIO_CONTENT`.
5. Add these variables/secrets under Pages Settings > Variables and Secrets:
   - `ADMIN_USERNAME`: desired admin username
   - `ADMIN_PASSWORD`: strong, unique password
   - `SESSION_SECRET`: at least 32 random characters
6. Redeploy after configuring bindings/secrets.
7. Visit `/admin.html` or `/admin`, sign in, create a post, then verify it appears at `/posts`.
8. Replace `asghar.network@example.com` in `public/content/index.html` with your real email address before publishing.

The admin session uses a signed HttpOnly, Secure, SameSite cookie with an 8-hour expiry. Posts are stored in KV. Keep credentials only in Cloudflare environment secrets; never put them in frontend files or commit them to Git.
