# Noel Mathews — Systems Status

Live uptime monitor and status page for [noelmc.online](https://noelmc.online), powered by [Upptime](https://upptime.js.org).

| Surface               | URL                                          |
| --------------------- | -------------------------------------------- |
| Status site           | https://status.noelmc.online                 |
| GitHub Pages fallback | https://noel9812.github.io/Website-Uptime    |
| JSON API              | https://status.noelmc.online/api/status.json |

[![Uptime CI](https://github.com/Noel9812/Website-Uptime/workflows/Uptime%20CI/badge.svg)](https://github.com/Noel9812/Website-Uptime/actions/workflows/uptime.yml)

## What this repo does

| Mechanism         | Role                                        |
| ----------------- | ------------------------------------------- |
| GitHub Actions    | Synthetic HTTPS checks every 5 minutes      |
| Issues            | Opened/closed automatically on outages      |
| `gh-pages` branch | Generated status website + graphs + history |
| `api/`            | JSON consumed by the portfolio footer       |

This repository deliberately uses Upptime's maintained monitor action rather than committing generated history. The first successful workflow creates the status history, graphs, and JSON API on `gh-pages`.

## Portfolio integration

```
content/settings/footer.yml → statusUrl: https://status.noelmc.online
```

The portfolio composable `useOperationalStatus` fetches `/api/status.json`. Never hardcode green.

## Monitored endpoints

Defined in `.upptimerc.yml`:

- `https://noelmc.online/`
- `https://www.noelmc.online/`

Icons use `https://noelmc.online/favicon.svg`.

## Manual GitHub configuration (do in order)

### 1. Repository settings

1. Open https://github.com/Noel9812/Website-Uptime/settings
2. **General → Features**: enable Issues (required for incident issues)
3. **General → Pull Requests**: leave defaults
4. Default branch: `master`
5. Set repository metadata:
   - Description: `Synthetic uptime monitoring and public status page for noelmc.online`
   - Website: `https://status.noelmc.online`
   - Topics: `upptime`, `uptime`, `status-page`, `monitoring`, `github-actions`

### 2. Personal Access Token secret

1. GitHub → **Settings → Developer settings → Fine-grained** (or classic) PAT
   - Classic: scopes `repo` + `workflow`
   - Fine-grained: this repo — Contents R/W, Issues R/W, Metadata R, Actions R/W, Pages R/W
2. Repo → **Settings → Secrets and variables → Actions → New repository secret**
3. Name: `GH_PAT`
4. Value: the token

### 3. Actions permissions

1. **Settings → Actions → General**
2. Allow Actions: **Allow all actions and reusable workflows**
3. Workflow permissions: **Read and write permissions**
4. Check **Allow GitHub Actions to create and approve pull requests** (Upptime updates content)
5. Save

### 4. Pages

1. After the **first successful Upptime run**, a `gh-pages` branch will exist
2. **Settings → Pages**
3. Source: **Deploy from a branch**
4. Branch: `gh-pages` / `/ (root)`
5. Save
6. Custom domain: `status.noelmc.online`
7. Enforce HTTPS: enable once DNS propagates

### 5. DNS (Cloudflare)

1. Cloudflare DNS for `noelmc.online`
2. Add CNAME: `status` → `noel9812.github.io` (proxied **or** DNS-only; HTTPS via GitHub Pages works best **DNS-only**/grey cloud initially, then orange-cloud if you prefer CF SSL)
3. Wait for GitHub Pages to show domain active + certificate

### 6. First run

1. **Actions → Upptime → Run workflow**
2. Confirm jobs succeed
3. Confirm `gh-pages` has `index.html`, `api/`, `graphs/`, `history/`
4. Open https://status.noelmc.online

### 7. Branding verification

In `.upptimerc.yml` already set:

- `owner` / `repo`
- Dark theme
- Navbar → Portfolio + GitHub
- Favicon + logo from `noelmc.online`

### 8. Badges (README)

The Actions badge at the top of this README auto-updates. Optional Upptime shields appear on the generated status site.

## Schedule (from config)

| Job              | Cron          |
| ---------------- | ------------- |
| Uptime           | `*/5 * * * *` |
| Response time    | hourly        |
| Static site      | hourly        |
| Graphs / summary | daily         |

## License

- Powered by: [Upptime](https://github.com/upptime/upptime)
- Code: [MIT](./LICENSE) © [Noel Mathews](https://noelmc.online)
- Data in the `./history` directory: [Open Database License](https://opendatacommons.org/licenses/odbl/1-0/)
