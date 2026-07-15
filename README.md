# Status monitor (Upptime)

This folder is a **starter for a separate GitHub repository** that publishes
a live status page and a machine-readable `status.json` for the portfolio footer.

## Why a separate repo?

- Isolates monitoring workflows from the portfolio app
- Publishes cleanly to GitHub Pages at `https://<user>.github.io/<status-repo>/`
- Keeps secrets and cron schedules out of the marketing site build

## Setup

1. Create a new GitHub repository (e.g. `status` or `portfolio-status`).
2. Copy the contents of this folder into that repository root.
3. Enable **GitHub Pages** from the `gh-pages` branch (Upptime creates it).
4. In repository **Settings → Secrets**, add:
   - `GH_PAT` — personal access token with `repo` scope (Upptime requirement)
5. Edit `.upptimerc.yml`:
   - Set `owner` / `repo`
   - Set `sites[].url` to your live portfolio / API endpoints
6. Push to `main`. Upptime Actions will generate the status site.

## Portfolio integration

In Decap CMS → **Footer** → **Status URL**, set either:

- Status site root: `https://<user>.github.io/<status-repo>/`
  (portfolio fetches `/api/status.json`)
- Or a direct JSON URL if you host a custom endpoint

The portfolio footer (`useOperationalStatus`) maps responses to:

| Level | UI label |
|-------|----------|
| operational | Systems operational |
| degraded | Partial degradation |
| down | Service disruption |
| unknown | Status unknown / View operational status |

Leave `statusUrl` empty until the status site is live — the footer shows “Status unavailable” without hardcoding green.

## Recommended endpoints to monitor

- Portfolio home (Cloudflare Pages URL)
- `/admin/` (optional — CMS shell availability)
- Cusdis / Formspree if you want third-party dependency visibility

## Local mock (optional)

For local development without Upptime, you can temporarily point `statusUrl` at
any JSON that matches:

```json
{ "status": "operational" }
```

or an array of `{ "status": "up" | "down" }` entries (Upptime live format).
