# Wolf Apps documentation

Source of the public documentation for Wolf Apps, published with GitHub Pages at
<https://rsavaikar.github.io/documentation/>.

| Path | Contents |
| --- | --- |
| `_config.yml` | Site title, theme and plugins for all apps. GitHub Pages reads it only at the root. |
| `index.md` | Landing page that lists the apps |
| `jira-cloud-connector-docs/` | [Cloud Connector for Jira](https://rsavaikar.github.io/documentation/jira-cloud-connector-docs/) |

## Adding an app

1. Create a folder named `<app>-docs/` with an `index.md` and the app's pages, each starting with `layout: default` front matter. Use relative links (`./admin-guide.html`) between pages.
2. Add a line for the app under **Apps** in the root `index.md`.
3. Commit and push to `main`; the site republishes within a few minutes.

Support: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com)
