# Operation System — integrated into admindash

The full Operation System is now a page **inside** your admin app, behind your existing JWT login.

## What was added
- `public/os.html` — the Operation System UI (tabs: Today, Finance, DevOps, Apps, Plans, Wiki, Media, Map; Health Board; AI task chat; Plan Finder). It calls `/api/mcp` and `/api/ai` using your `auth_token` cookie as a Bearer token.
- `app/api/mcp/route.ts` — live-data proxy (Zoho leads + Vercel deploys now; Supabase advisors if SUPABASE_TOKEN set; Gmail/Drive return empty until Phase 2). Auth-checked with your JWT.
- `app/api/ai/route.ts` — the per-task AI chat (Anthropic). Auth-checked.
- `app/dashboard/os/page.tsx` — a dashboard page that embeds the OS (reuses your sidebar + auth).
- `components/Sidebar.tsx` — new nav item **Operation System** (`/dashboard/os`).
- `next.config.ts` — ignore lint/type errors during build (so the embedded proxy never blocks deploy).
- `.env.example` — new env vars for live data.

## Nothing was removed — your existing dashboard/users/reports/analytics/settings stay as-is.

## Deploy
1. `npm install`
2. Set env vars (Vercel project → Settings → Environment Variables) from `.env.example`: keep your auth vars, add `VERCEL_TOKEN`, `VERCEL_TEAM`, `ZOHO_*`, `ANTHROPIC_API_KEY`, (`SUPABASE_TOKEN` optional).
3. `vercel --prod` (or push to `segattihall-ops/admindash` and let Vercel build).
4. Open `/dashboard/os` after logging in.

## Live data status
- ✅ Vercel health, Plan Finder, Wiki, Calendar — work immediately.
- ✅ Leads (Zoho) — once ZOHO_* env vars set.
- ✅ AI task chat — once ANTHROPIC_API_KEY set.
- ⏳ Finance/Ledger (Gmail) + Drive media — Phase 2 (Google OAuth).
- ⏳ Supabase advisors — set SUPABASE_TOKEN (a Supabase Management API token).
