---
layout: default
title: Privacy policy
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For portal customers](./for-customers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Privacy policy – Portal Fields for JSM Cloud

*Last updated: 28 October 2026 · Rahul Savaikar*

Portal Fields for JSM Cloud ("the app") is a free app for Atlassian Jira Service Management Cloud, built on Atlassian Forge and published by Rahul Savaikar ("we"). It shows request fields chosen by your administrators in a request report and charts on your customer portal. This policy explains what data the app handles, where it is kept and for how long.

## Summary

- The app runs entirely on Atlassian's Forge platform. We operate no servers and use no third-party services. The app sends no data outside Atlassian; only files your users choose to export are saved to their own devices.
- The app reads request data from Jira when someone opens the report and shows it to them. It does not keep a copy.
- The only personal data the app stores is each user's column layout, linked to their Atlassian account ID.
- We do not sell data, show ads, track users or use cookies.

## Data the app processes

| Data | Purpose | Where it goes | Kept for |
| --- | --- | --- | --- |
| Request details shown in the report: reference, summary, status, request type, service project, reporter's display name, created and resolved dates | Show the user their requests | Read from your Jira site and shown to the user who opened the report | Not stored by the app; it lives in Jira |
| Extra fields your administrators enabled, for example priority, assignee, organisations, SLA times or custom fields | Show the extra columns and charts | Read from your Jira site and shown to the same user | Not stored by the app |
| Exports (CSV, Excel, PDF, PNG, JPG) | Let the user save the report or a chart | Created in the user's browser and saved to their device; not sent anywhere | On the user's device, under their organisation's control |
| App settings: which projects use the app, chosen fields and charts, export and visibility options | Run the app as your administrators configured it | App storage on your site | Until changed, or the app is uninstalled |
| Column layout: the user's Atlassian account ID, column order, hidden columns and when it was last changed | Keep each user's layout across browsers and devices | App storage on your site | While the app is installed; see "Personal data" below |
| Operational log lines: an error reference, which part of the app failed, the status code and Jira's error message with quoted names and values removed; load times and counts | Diagnose problems | Atlassian's Forge logging; we may read them to provide support | As set by Atlassian |

The app does not process passwords, API tokens or email addresses, and it does not change anything in Jira.

## Who sees what

- **Your users** see only requests that Jira already lets them see on the portal, and only the fields your administrators enabled.
- **Your administrators** decide which fields appear. If a project administrator switches on "Show all requests", every customer who can open that project's portal sees all of its requests. This setting is off by default.
- **Atlassian** hosts the app's functions, storage and logs on its Forge platform, under [Atlassian's privacy policy](https://www.atlassian.com/legal/privacy-policy).
- **Us.** We do not receive your request data. We may see operational logs to help with a support request.

## Where data is stored

All stored data (settings and column layouts) is kept in Forge hosted storage on your Atlassian site and follows your site's data residency location.

## Personal data

The column layouts are the only personal data the app stores. As Atlassian requires, the app reports the stored account IDs to Atlassian every week and erases the layouts of accounts that have been closed. Users can also remove their own layout at any time with **Reset to default columns**.

To ask about, access or delete personal data the app holds about you, contact us at the address below.

## Your choices

- Administrators decide which projects use the app and which fields, charts and exports customers get.
- Users choose their own columns and can reset them.
- Uninstalling the app removes all of its stored data on your site, following Atlassian's Forge data deletion.

## Security

The app runs on Atlassian's Forge platform, which handles encryption in transit and at rest. Which requests a user sees is always decided by Jira, for that user. Only Jira and project administrators can change the app's settings, and the app checks their permission on every change. Error messages shown to users contain no technical details, only a reference.

## Changes and contact

We will post changes to this policy on this page with a new "Last updated" date. Questions or requests: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com).

---

[Support SLA](./sla.html) · [End-user terms](./end-user-terms.html)
