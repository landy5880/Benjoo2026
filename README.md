# Depot — Asset Inventory System

A single-page asset inventory app (Dashboard, Assets, Categories, Locations,
Employees, Reports, Users, Settings) with login, role-based access
(Admin/Staff/Viewer), QR codes, CSV/Excel import-export, and a Supabase-backed
database for shared, persistent data across devices.

## Files
- `index.html` — the entire app (HTML/CSS/vanilla JS, no build step required)
- `netlify.toml` — tells Netlify to publish this directory as-is

## Stack
- Plain HTML/CSS/JS, no framework, no bundler
- Supabase (Postgres + REST API) for data storage, connected via `@supabase/supabase-js`
  loaded from a CDN, using a **publishable/anon key embedded directly in the file**
- QR codes via `qrcodejs` (CDN)
- Excel import/export via `SheetJS` / `xlsx` (CDN)

## Supabase project
- Project ref: `bgciayhxvkqhmgcdfvco`
- URL: `https://bgciayhxvkqhmgcdfvco.supabase.co`
- Tables: `categories`, `locations`, `employees`, `app_users`, `assets`, `asset_history`, `app_meta`
- RLS is enabled on every table with a single permissive `"public full access"` policy
  (`using (true) with check (true)`) — this is intentionally open since the app
  connects directly from the browser with no separate backend. If this app will
  ever hold real/sensitive data, tighten these policies and move to real
  Supabase Auth instead of the current plaintext `app_users` table.

## Demo accounts (in `app_users` table)
- `admin` / `admin123` — Admin (full access)
- `staff` / `staff123` — Staff (no Users/Settings access)
- `viewer` / `viewer123` — Viewer (Dashboard/Assets/Reports only, read-focused)

## Known issue
Deploys to Netlify (site: `depot-asset-inventory`, site ID
`c7f09769-f789-41d6-81f9-2c3962aca8f1`) started failing with `403 Forbidden`
after a few earlier deploy attempts accidentally uploaded multi-gigabyte
payloads (from deploying out of a directory that included `node_modules`).
The site itself still exists and looks healthy, but pushing a new deploy was
blocked at the time this was handed off — worth checking the Netlify
dashboard's Deploys tab for the specific quota/error message, or just
re-deploying from a clean directory (this one) after some time has passed.

## Local development
No build step — just open `index.html` directly in a browser, or serve it
with any static file server, e.g.:
```
npx serve .
```
