# Fox Apps documentation

Source of the public documentation for Fox Apps, published with GitHub Pages at
<https://rsavaikar.github.io/documentation/>.

| Path | Contents |
| --- | --- |
| `_config.yml` | Site title, theme and plugins for all apps. GitHub Pages reads it only at the root. |
| `index.md` | Landing page that lists the apps and the shared policies |
| `support.md` | Support page for all apps, linking each app's troubleshooting and support details |
| `sla.md` | Support SLA for all apps, with app-specific notes |
| `privacy-policy.md` | Privacy policy for all apps, with a section per app |
| `assets/brand/` | Vendor logo: `vendor-logo.svg` (source) and PNGs at 512 and 144 px; the 144 px one is the site's header logo (`logo` in `_config.yml`) |
| `favicon.ico` | Browser tab icon, made from the vendor logo |
| `jira-cloud-connector-docs/` | [Cloud Connector for Jira](https://rsavaikar.github.io/documentation/jira-cloud-connector-docs/) |
| `jira-portal-fields-docs/` | [Portal Fields for JSM Cloud](https://rsavaikar.github.io/documentation/jira-portal-fields-docs/) |
| `kpi-dashboard-gadget-docs/` | [KPI Dashboard Gadget](https://rsavaikar.github.io/documentation/kpi-dashboard-gadget-docs/) |

## Adding an app

1. Create a folder named `<app>-docs/` with an `index.md` and the app's pages, each starting with `layout: default` front matter. Use relative links (`./admin-guide.html`) between pages.
2. Add a line for the app under **Apps** in the root `index.md`, and to the app lists and navigation at the top of `support.md`, `sla.md` and `privacy-policy.md`, and a row to the table in `support.md`.
3. Add the app's section to `privacy-policy.md` (its data table, who sees what, its choices and security), and any app-specific notes to `sla.md`. Link the app's pages to `../sla.html` and `../privacy-policy.html#<app-section>`.
4. Commit and push to `main`; the site republishes within a few minutes.

Support: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com)
