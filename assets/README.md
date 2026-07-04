# Assets

Files placed here are served at `/assets/*` on the deployed site. Add the
following (referenced by `config.yaml`):

| File | Referenced by | Notes |
| ---- | ------------- | ----- |
| `logo.svg` | `org.logo_url` → `/assets/logo.svg` | μLearn logo. SVG or PNG (update the extension in `config.yaml` if PNG). |
| `favicon.ico` | `meta.favicon_url` → `/assets/favicon.ico` | Browser tab icon. `.ico` or `.png`. |
| `og-image.png` | `meta.image_url` → `/assets/og-image.png` | Social/OpenGraph preview (recommended 1200×630). |

Optional: `badges/*.svg` if you enable the `badges:` block in `config.yaml`.

> Until these files exist, those references will 404 on the live site. You can
> instead point the `config.yaml` fields at full URLs (e.g. the gtech-mulearn
> GitHub org avatar) as a temporary fallback.
