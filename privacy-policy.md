---
layout: default
title: Privacy policy
redirect_from:
  - /jira-cloud-connector-docs/privacy-policy.html
  - /jira-portal-fields-docs/privacy-policy.html
---

[Home](./) · [Cloud Connector for Jira](./jira-cloud-connector-docs/) · [Portal Fields for JSM Cloud](./jira-portal-fields-docs/) · [KPI Dashboard Gadget](./kpi-dashboard-gadget-docs/) · [Support](./support.html) · [Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html)

# Privacy policy – Fox Apps

*Last updated: 30 October 2026 · Rahul Savaikar*

This policy covers the free Atlassian Cloud apps published by Rahul Savaikar ("we"), each built on Atlassian Forge:

- [Cloud Connector for Jira](#cloud-connector-for-jira) keeps a Jira project on your site in sync with a project on a partner's Jira site.
- [Portal Fields for JSM Cloud](#portal-fields-for-jsm-cloud) shows request fields in a report and charts on your Jira Service Management customer portal.
- [KPI Dashboard Gadget](#kpi-dashboard-gadget) shows support figures and partner case overviews in gadgets on your Jira dashboards.

It explains what data each app handles, where it goes and how long it is kept. The principles below apply to every app; each app's section lists its own data.

## Summary

- The apps run entirely on Atlassian's Forge platform. We operate no servers and use no third-party services.
- We do not receive your Jira data. We do not sell data, show ads, track users or use cookies.
- No app processes passwords, API tokens or email addresses.

## Cloud Connector for Jira

Issue content is sent only to the partner site your administrator paired with, and only from projects your administrator connected.

| Data | Purpose | Where it goes | Kept for |
| --- | --- | --- | --- |
| Issue content of connected projects: summary, description, status, issue type, chosen fields, labels | Keep the partner's copy in sync | Your site and the paired partner site | Not stored by the app; it lives in Jira on both sites |
| New public comments, with the author's display name | Show the comment on the partner's issue | Your site and the paired partner site | Not stored by the app |
| Attached files (only if your administrator switches attachments on) | Copy files to the partner's issue | The paired partner site | Pieces of a file in transfer: up to 1 day |
| Connection settings, mappings and the pairing secret | Run the connection | App storage on your site (the secret in encrypted storage) | Until the connection is deleted or the app is uninstalled |
| Issue links (issue ids and keys on both sites) and each issue's last sync result | Know which issues belong together; show the issue panel | App storage on your site | Until the issue or connection is deleted, or the app is uninstalled |
| Activity log: issue keys, file names, comment authors' display names, error messages, including Jira's reason when it rejects a change | Let your administrators see what happened | App storage on your site, shown only to your Jira administrators | 30 days |
| Ids of processed messages, with their result | Avoid duplicates when a message is retried | App storage on your site | 2 days |
| Operational log lines: issue keys, ids, what failed and the status code; never people's names, file names, queries or Jira's error text | Diagnose problems | Atlassian's Forge logging | As set by Atlassian |

The app never sends comments restricted to a group or role, internal notes in Jira Service Management projects, or issues with an issue security level. It does not process user account details.

**The paired partner site** receives data only through the partner's own installation of the app. Your organisation is responsible for having the right to share issue data with the partner it pairs with, for example under its agreement with that partner.

**Your choices:** administrators decide which project, issue types, fields, comments and files are synced, per partner. **Disconnect** stops all exchange with a partner at once. Deleting a connection removes its stored settings and links.

**Security:** messages between the two sites are sent over HTTPS and signed with a secret unique to each pairing, and only to the partner's Cloud Connector address on Atlassian's Forge platform. When your Jira rejects a change from the partner, the partner learns only that it failed and the status code; the reason stays on your Activity tab. Pairing secrets are kept in Forge's encrypted storage. Only Jira administrators can change the app's settings, and the issue panel shows information only to users who may view the issue.

## Portal Fields for JSM Cloud

The app reads request data from Jira when someone opens the report and shows it to them. It does not keep a copy and does not change anything in Jira. It sends no data outside Atlassian; only files your users choose to export are saved to their own devices.

| Data | Purpose | Where it goes | Kept for |
| --- | --- | --- | --- |
| Request details shown in the report: reference, summary, status, request type, service project, reporter's display name, created and resolved dates | Show the user their requests | Read from your Jira site and shown to the user who opened the report | Not stored by the app; it lives in Jira |
| Extra fields your administrators enabled, for example priority, assignee, organisations, SLA times or custom fields | Show the extra columns and charts | Read from your Jira site and shown to the same user | Not stored by the app |
| Exports (CSV, Excel, PDF, PNG, JPG) | Let the user save the report or a chart | Created in the user's browser and saved to their device; not sent anywhere | On the user's device, under their organisation's control |
| App settings: which projects use the app, chosen fields and charts, export and visibility options | Run the app as your administrators configured it | App storage on your site | Until changed, or the app is uninstalled |
| Column layout: the user's Atlassian account ID, column order, hidden columns and when it was last changed | Keep each user's layout across browsers and devices | App storage on your site | While the app is installed; see below |
| Operational log lines: an error reference, which part of the app failed and the status code, never Jira's error text; load times and counts | Diagnose problems | Atlassian's Forge logging | As set by Atlassian |

**Who sees what:** your users see only requests that Jira already lets them see on the portal, and only the fields your administrators enabled. If a project administrator switches on "Show all requests", every customer who can open that project's portal sees all of its requests, with the reporter's name, except requests with an issue security level, which stay visible only to those Jira allows. This setting is off by default.

**Personal data:** the column layouts are the only personal data the app stores. As Atlassian requires, the app reports the stored account IDs to Atlassian every week and erases the layouts of accounts that have been closed. Users can also remove their own layout at any time with **Reset to default columns**.

**Your choices:** administrators decide which projects use the app and which fields, charts and exports customers get. Users choose their own columns and can reset them.

**Security:** which requests a user sees is always decided by Jira, for that user. Only Jira and project administrators can change the app's settings, and the app checks their permission on every change. Error messages shown to users contain no technical details, only a reference.

## KPI Dashboard Gadget

The gadgets read tickets from your Jira site as the person viewing the dashboard and show the figures in that person's browser.

| Data | Purpose | Where it goes | Kept for |
| --- | --- | --- | --- |
| Ticket details: key, summary, status, priority, assignee's display name, organisations, security level name, created, updated and resolved dates | Calculate and show the figures, charts and tables | Read from your Jira site and shown only to the user viewing the dashboard | Not stored by the app; it lives in Jira |
| Values of the fields set in the gadget: first response date, satisfaction rating, and optionally an assignment date and a request scope | Calculate response times, SLA, CSAT and the scope filter | Read from your Jira site and shown to the same user | Not stored by the app |
| Gadget settings: the JQL query or saved filter, refresh interval, field names, organisation prefixes and SLA targets | Run each gadget as its dashboard owner configured it | Saved by Jira with the dashboard, not in app storage | Until changed, or the gadget or app is removed |
| Exports (PNG charts, CSV tables) | Let the user save a chart or table | Created in the user's browser and saved to their device; not sent anywhere | On the user's device, under their organisation's control |
| Operational log lines: which part of the app failed and the status code, never the query or Jira's error text | Diagnose problems | Atlassian's Forge logging | As set by Atlassian |

**Who sees what:** every Jira call is made as the person viewing the dashboard. Each viewer sees only the tickets their own Jira permissions, issue security levels and filter sharing allow; the app never reads Jira with permissions of its own.

**Personal data:** the app stores no personal data. Assignees' display names are shown only to viewers who may already see those tickets in Jira.

**Your choices:** dashboard owners decide which tickets each gadget includes and which fields it uses; Jira administrators decide who can view each dashboard.

**Security:** the app has one permission, `read:jira-work`, and only reads from Jira. Error messages show no technical details, and CSV exports neutralise cells that a spreadsheet could run as formulas.

## Who receives data

- **Atlassian.** The apps run on Atlassian's Forge platform, which hosts their functions, storage and logs under [Atlassian's privacy policy](https://www.atlassian.com/legal/privacy-policy).
- **Us.** We do not receive your Jira data. We may read operational logs to help with a support request.
- **Others.** Only where an app's section above says so, such as the paired partner site for Cloud Connector for Jira.

## Where data is stored

Data the apps store is kept in Forge hosted storage on your Atlassian site and follows your site's data residency location. Data that Cloud Connector for Jira sends to a partner is stored in the partner's Jira, under the partner's settings.

## Security

Atlassian's Forge platform encrypts data in transit and at rest. Each app checks Jira permissions before it shows or changes anything; see each app's section above.

## Deleting data

Uninstalling an app removes its stored data on your site, following Atlassian's Forge data deletion. To ask about, access or delete personal data an app holds about you, contact us at the address below.

## Changes and contact

We will post changes to this policy on this page with a new "Last updated" date. Questions or requests: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com).

---

[Support SLA](./sla.html) · End-user terms: [Cloud Connector for Jira](./jira-cloud-connector-docs/end-user-terms.html) · [Portal Fields for JSM Cloud](./jira-portal-fields-docs/end-user-terms.html) · [KPI Dashboard Gadget](./kpi-dashboard-gadget-docs/end-user-terms.html)
