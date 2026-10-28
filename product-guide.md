---
layout: default
title: Product guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Technical reference](./technical-reference.html) · [Support](./support.html)

# Cloud Connector for Jira – Product guide

*Last updated: 28 October 2026 · Rahul Savaikar*

Cloud Connector for Jira keeps one project on your Jira Cloud site in sync with one project on a customer's or partner's Jira Cloud site: issues, fields, statuses, comments and files, both ways, without sharing any credentials. This guide explains what the app does, how it protects your data and how to get started.

## Highlights

**Work with customers in your own Jira.** Each team stays in its own project and workflow. New issues, updates, comments and files usually reach the other site within a minute.

**Nothing private crosses over.** Restricted comments, Jira Service Management internal notes and issues with a security level never leave your site. No passwords or API tokens are exchanged: each site writes only to its own Jira.

**You decide what syncs.** Map issue types, fields, option values and statuses per partner, choose the direction for each field, and connect several partners, each kept fully separate.

## Overview

When you work with a customer or partner who runs their own Jira Cloud, issues often live twice and are copied by hand. Cloud Connector for Jira links one project on your site with one project on theirs, so each team works in its own Jira and both stay up to date.

Both sites install the same app. One admin creates a pairing code and sends it privately to the other admin, who pastes it into their own Cloud Connector page. From then on the two sites exchange signed messages; each writes only to its own Jira, as the app.

Typical uses:

- **Customer escalations:** your support team works in Jira Service Management, the customer's engineers in their own Jira project.
- **Outsourced development:** a vendor's backlog mirrors the client's project, with statuses mapped between different workflows.
- **Several partners at once:** connect a separate project to each partner; one partner never sees another's issues.

Issues that existed before you connected can be brought over in one go, chosen with JQL. On every synced issue, a panel shows the linked partner issue, the last sync and any error, with a **Resync now** button.

## What syncs

Everything below works in both directions; each site chooses what it sends and receives.

| Change on one site | Result on the partner's site |
| --- | --- |
| Issue created (mapped issue type; optionally only with a sync label) | Matching issue created, with web links between the two |
| Summary or description edited | Same change on the linked issue (description sync can be switched off) |
| Status changed | Linked issue moves to the mapped status, if its workflow allows |
| Mapped field changed | Field updated; option values translated through a value map |
| Labels added or removed | Only those labels are added or removed; the partner's own labels stay |
| Public comment added | Comment added with "*Name* wrote on *site*:"; an internal note in Service Management projects by default |
| File attached (both sites opt in) | Same file attached, up to a size limit each site sets (max. 25 MB) |
| Issue deleted | Partner's copy is unlinked and gets a comment saying why; it is not deleted |

**Mappable fields:** text, paragraph, number, date, date and time, single select, radio, multi-select, checkboxes, priority and labels, both system and custom fields.

**Service Management:** issues created from a partner get the request type you choose per issue type, so they appear in queues and on the portal.

## How it works

A change usually reaches the partner's site within a minute, on this path:

1. **Jira event.** Someone creates or edits an issue, or adds a comment, in a connected project. Changes the app makes itself are ignored, so nothing echoes back.
2. **Queue.** The change is queued on Atlassian's Forge platform; if the partner is busy or briefly unreachable, it is retried automatically.
3. **Signed message.** Your site sends the change to the partner's Cloud Connector, signed with a secret only the two of you hold. It uses shared keys (for example `bug`, `priority`), not site-specific ids.
4. **Partner applies it.** The partner checks the signature, translates the keys into its own issue types, fields and statuses, and writes to its own Jira.

A change that still cannot be delivered is kept under **Activity → Failed changes**, retried every hour for a day, and can be retried by hand. Sites on different app versions keep syncing at the newest version both understand.

## Security, privacy and data

Only issues in a connected project leave your site, and only to the one partner paired with that connection.

**What is sent:** summary; description (if enabled); issue type and status as shared keys; values of fields you chose to send; new public comments with the author's display name; files, only if you switch attachments on; the issue key and a link for the web link.

**What is never sent, whatever the settings:**

- comments restricted to a group or project role
- internal notes in Jira Service Management projects
- issues with an issue security level, with their comments and files
- user accounts, email addresses, assignee and reporter

**How it is protected:**

| Measure | Detail |
| --- | --- |
| No shared credentials | Each site writes only to its own Jira, as the app. No API tokens or user accounts cross sites. |
| Signed messages | Every message is signed (HMAC-SHA256) with a 256-bit secret unique to the pairing; messages older than 5 minutes are rejected. |
| Encrypted secrets | Pairing secrets are kept in Forge's encrypted secret storage and never shown again. |
| Short-lived pairing codes | Codes expire after 24 hours, work once and can be cancelled. |
| Permission checks | Every admin action checks that the user is a Jira administrator; the issue panel checks that the user may view the issue, and "Sync now" that they may edit it. |
| Isolation | Each partner connection has its own project, settings and secret; partners never see each other's issues. |
| No personal data in logs | Operational logs contain issue keys, ids and error messages, never people's names or file names. |
| Disconnect any time | Disconnecting tells the partner and stops all exchange immediately. |

**Where data lives:** on Atlassian's Forge platform only. Settings and issue links are stored in the app's Forge storage on each site; there are no third-party servers. Because issue content is sent to the partner's site, the app is not eligible for Atlassian's "Runs on Atlassian" badge. See the [privacy policy](./privacy-policy.html) for details.

## Permissions and requirements

The app needs Jira Cloud on both sites, and a Jira administrator on each side to install and pair it. It asks for these permissions:

| Permission | Why |
| --- | --- |
| `read:jira-work` | Read issues, comments, fields, issue types and statuses of the connected project |
| `write:jira-work` | Create and update issues, add comments, move issues between statuses, add web links |
| `read:servicedesk-request` | List a Service Management project's request types, so incoming issues get one |
| `storage:app` | Store connections, settings, issue links and the activity log |
| Outbound calls to the partner's Forge web trigger | Send messages to the paired site; Atlassian hosts the web trigger |

The partner installs the same app on their own site; nothing needs to be installed outside Jira.

## Getting started

Setting up a partner takes about 15 minutes for the two admins together.

1. **Install** Cloud Connector for Jira on your site; your partner installs it on theirs.
2. **Create a connection** on the Cloud Connector admin page (Jira settings → Apps) and choose the project to sync.
3. **Pair:** generate a pairing code and send it privately to the partner's admin, who pastes it on their page.
4. **Map issue types** with shared keys you agree on (for example `bug`, `task`); each side maps the key to its own type.
5. **Map fields and statuses** where names differ, and choose the direction per field (both ways, send only, receive only).
6. **Check contract** shows both sites' mappings side by side and flags anything only one side has.
7. Optionally **sync existing issues** with a JQL filter, then work as usual: new and changed issues sync on their own.

The [Admin guide](./admin-guide.html) walks through each step.

## Current limitations

We list these openly so you can judge fit before installing.

| Area | Today |
| --- | --- |
| Assignee, reporter and user fields | Not synced: accounts aren't visible across organisations |
| Cascading select, version and component fields | Not supported yet |
| Comment edits and deletions | Only new comments sync |
| File deletions; images inside descriptions | Not synced; embedded images show a placeholder |
| Changing the issue type of a linked issue | Not synced |
| Status mapping | One status map per connection, not per issue type |
| Author shown on created issues and comments | The app; the original author's name is in the comment's first line |

## FAQ

**Does the partner need the app too?** Yes. Both sites install Cloud Connector for Jira; each side controls its own mapping and what it sends.

**Do we share passwords or API tokens?** No. The sites pair once with a code; after that each one writes only to its own Jira, as the app.

**Can we connect several customers?** Yes. Each connection links one of your projects with one partner, and partners never see each other's issues.

**Will our internal notes reach the customer?** No. Restricted comments, Service Management internal notes and issues with a security level always stay on your site.

**What if the partner is down?** Changes are retried automatically; anything still undelivered is listed for retry and tried again every hour for a day.

**What if we run different app versions?** The two sites agree on the newest version both understand and keep syncing; newer features wait until both have updated.

**What about issues created before we connected?** Bring them over with **Sync existing issues**, choosing them with JQL.

**How do we stop syncing?** Click **Disconnect**. The partner is told immediately, issue links are kept, and pairing again resumes where you left off.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
