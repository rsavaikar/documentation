---
layout: default
redirect_from: /privacy-policy.html
title: Privacy policy
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Privacy policy – Cloud Connector for Jira

*Last updated: 28 October 2026 · Rahul Savaikar*

Cloud Connector for Jira ("the app") is a free app for Atlassian Jira Cloud, built on Atlassian Forge and published by Rahul Savaikar ("we"). It keeps issues in a Jira project on your site in sync with a project on a partner's Jira site that your administrator pairs it with. This policy explains what data the app handles, where it goes and how long it is kept.

## Summary

- The app runs entirely on Atlassian's Forge platform. We operate no servers and use no third-party services.
- Issue content is sent only to the partner site your administrator paired with, and only from projects your administrator connected.
- We do not sell data, show ads, track users or use cookies.

## Data the app processes

| Data | Purpose | Where it goes | Kept for |
| --- | --- | --- | --- |
| Issue content of connected projects: summary, description, status, issue type, chosen fields, labels | Keep the partner's copy in sync | Your site and the paired partner site | Not stored by the app; it lives in Jira on both sites |
| New public comments, with the author's display name | Show the comment on the partner's issue | Your site and the paired partner site | Not stored by the app |
| Attached files (only if your administrator switches attachments on) | Copy files to the partner's issue | The paired partner site | Pieces of a file in transfer: up to 1 day |
| Connection settings, mappings and the pairing secret | Run the connection | App storage on your site (the secret in encrypted storage) | Until the connection is deleted or the app is uninstalled |
| Issue links (issue ids and keys on both sites) and each issue's last sync result | Know which issues belong together; show the issue panel | App storage on your site | Until the issue or connection is deleted, or the app is uninstalled |
| Activity log: issue keys, file names, comment authors' display names, error messages | Let your administrators see what happened | App storage on your site | 30 days |
| Ids of processed messages, with their result | Avoid duplicates when a message is retried | App storage on your site | 2 days |
| Operational log lines: issue keys, ids and error messages, never people's names or file names | Diagnose problems | Atlassian's Forge logging; we may read them to provide support | As set by Atlassian |

The app never sends comments restricted to a group or role, internal notes in Jira Service Management projects, or issues with an issue security level. It does not process passwords, API tokens, email addresses or user account details.

## Who receives data

- **The paired partner site.** Your Jira administrator chooses which project to connect and which partner to pair with. Data reaches only that partner's Jira site, through the partner's own installation of the app.
- **Atlassian.** The app runs on Atlassian's Forge platform, which hosts the app's functions, storage and logs under [Atlassian's privacy policy](https://www.atlassian.com/legal/privacy-policy).
- **Us.** We do not receive your issue content. We may see operational logs to help with a support request.

Your organisation is responsible for having the right to share issue data with the partner it pairs with, for example under its agreement with that partner.

## Your choices

- Administrators decide which project, issue types, fields, comments and files are synced, per partner.
- **Disconnect** stops all exchange with a partner at once. Deleting a connection removes its stored settings and links.
- Uninstalling the app removes its stored data on your site, following Atlassian's Forge data deletion.

## Security

Messages between the two sites are sent over HTTPS and signed with a secret unique to each pairing. Pairing secrets are kept in Forge's encrypted storage. Only Jira administrators can change the app's settings, and the issue panel shows information only to users who may view the issue.

## Changes and contact

We will post changes to this policy on this page with a new "Last updated" date. Questions or requests: [rahul.savaikar@gmail.com](mailto:rahul.savaikar@gmail.com).

---

[Support SLA](./sla.html) · [End-user terms](./end-user-terms.html)
