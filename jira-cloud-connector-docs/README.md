# Cloud Connector for Jira – documentation site

Source of the public documentation for **Cloud Connector for Jira**, published with GitHub Pages at
<https://rsavaikar.github.io/documentation/jira-cloud-connector-docs/>.

| Page | File |
| --- | --- |
| Home | `index.md` |
| Product guide | `product-guide.md` |
| Admin guide | `admin-guide.md` |
| Install | `install.md` |
| For Jira users | `for-jira-users.md` |
| Troubleshooting | `troubleshooting.md` |
| Support | `support.md` |
| Support SLA | `sla.md` |
| Privacy policy | `privacy-policy.md` |
| End-user terms | `end-user-terms.md` |

Each page is a Markdown file with a `layout: default` front matter. The site title, theme and plugins are shared by all apps and set in `_config.yml` at the repository root, because GitHub Pages reads it only there. Pushing to `main` republishes the site within a few minutes.

The pages used to live at the repository root. Each page's `redirect_from` line sends the old address (for example `/documentation/product-guide.html`) to the new one, so links already in the app and on the Marketplace keep working. Keep those lines. The old home address, `/documentation/`, is now the Wolf Apps landing page, which links here.

Support: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com)
