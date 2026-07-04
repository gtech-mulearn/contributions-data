# μLearn Contributions Data

Data repository for the μLearn contributions leaderboard, deployed at
**[contributions.mulearn.org](https://contributions.mulearn.org)**.

This repo holds muLearn's config, branding, and scraped contribution data. The
app engine ([ohcnetwork/leaderboard](https://github.com/ohcnetwork/leaderboard))
is pulled at build time — it is **not** forked. Only this repo, the scraper
plugin ([gtech-mulearn/mulearn-github-plugin](https://github.com/gtech-mulearn/mulearn-github-plugin)),
the Cloudflare Pages project, and the domain belong to muLearn.

## What's here

| Path | Purpose |
| ---- | ------- |
| `config.yaml` | Org info, roles, plugin config, branding pointers |
| `theme.css` | μLearn brand colors (overrides the engine's CSS variables) |
| `assets/` | Logo, favicon, OG image, badge SVGs (see `assets/README.md`) |
| `.github/workflows/scrape.yml` | Cron (every 6h): scrape the org → commit data → upload DB to R2 |
| `.github/workflows/deploy.yml` | On push: build the static site → deploy to Cloudflare Pages |
| `contributors/` | Generated + human-editable contributor profiles |
| `activities/`, `aggregates/` | Generated scrape output (committed by the workflow) |

## How it runs

1. **`scrape.yml`** runs every 6 hours: authenticates as the `mulearn-leaderboard-bot`
   GitHub App, scrapes every repo in `gtech-mulearn` via the plugin, commits the
   updated data back to `main`, and uploads the SQLite DB to Cloudflare R2.
2. The data commit triggers **`deploy.yml`**, which builds the static site from
   the committed data and deploys it to Cloudflare Pages.

## Required GitHub Actions secrets / variables

Set under **Settings → Secrets and variables → Actions**:

**Secrets:** `GH_APP_PRIVATE_KEY` (PKCS#8), `CLOUDFLARE_API_TOKEN`,
`CLOUDFLARE_ACCOUNT_ID`
**Variables:** `GH_APP_ID`, `CLOUDFLARE_PAGES_PROJECT_NAME`,
`LEADERBOARD_DATA_DB_URL`

Also enable **Settings → Actions → General → Workflow permissions → Read and
write**.

## Assign a role

Everyone defaults to `contributor`. To mark someone core team, add
`contributors/<github-username>.md`:

```markdown
---
username: someusername
role: core
---
```

Commit → the next deploy shows the new role. Human-edited profiles always win
over scraped data.
