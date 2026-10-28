---
layout: default
title: Product guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For portal customers](./for-customers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Portal Fields for JSM Cloud – Product guide

*Last updated: 28 October 2026 · Rahul Savaikar*

Portal Fields for JSM Cloud adds a request report and charts to the Jira Service Management customer portal, showing the request fields your customers ask about: priority, SLA times, organisations, custom select lists and more. Your administrators choose the fields; Jira decides which requests each customer may see. This guide explains what the app does, how it protects your data and how to get started.

## Highlights

**The fields your customers ask about.** The portal's own Requests list has a fixed set of columns. Your project administrators choose extra fields to show next to them, per service project.

**Charts customers can download.** Requests by status, created vs resolved, SLA met vs breached, and requests by any chosen field. Each chart downloads as PNG, JPG, PDF, CSV or Excel.

**Each customer's own layout.** Customers reorder and hide columns, and their layout follows them to every browser and device.

**Nothing leaves Atlassian.** The app runs entirely on Atlassian's Forge platform and is eligible for Atlassian's "Runs on Atlassian" programme.

## Overview

On the customer portal's **Requests** page, the app adds an **Advanced request report** section with two buttons:

- **Open report** shows the customer's requests in a table with the standard columns (Type, Reference, Summary, Status, Service project, Requester) plus the extra fields your administrators chose. Customers can search, filter, sort, choose columns and export to CSV or Excel.
- **Charts** shows four charts about the same requests, each with a table view and downloads.

The section appears only on the Requests page, only for logged-in customers, and only where an administrator switched the app on for a service project they can open.

Typical uses:

- **SLA transparency:** show customers their time to first response and time to resolution, and how often SLAs were met.
- **Account management:** let a customer's organisation see all its requests with priority and request scope, and export them for their own reporting.
- **Fewer status questions:** customers see priority, assignee or custom status fields themselves instead of asking.

## What customers see

| Feature | Details |
| --- | --- |
| Request report | Standard columns plus up to 30 extra fields; 50 requests per page; click a reference to open the request in a new tab |
| Filters | Search text; open, closed or any status; all requests I can see, created by me, where I'm a participant, shared with my organisations; service project; request type |
| Sorting | Click any column heading; click again to reverse |
| Columns | Show, hide and reorder columns by dragging or with arrows; saved for each customer's account |
| Export | CSV and Excel of the filtered list, if the project allows exports |
| Charts | Requests by status; created vs resolved over the last 12 weeks, 6 months or 12 months; SLA met vs breached; requests by field (top 12 values, then "Other" and "(none)") |
| Chart downloads | PNG, JPG and PDF (chart, filters, date and data table), CSV and Excel (the numbers), if the project allows exports |
| Languages | English, Norwegian, German and Arabic (right to left), following the user's Jira language |

**Supported fields:** text, numbers, dates, users, select lists (including cascading), labels, organisations, priority, SLA fields and other simple fields, both system and custom. Assets objects aren't supported.

## Who sees which requests

Customers see exactly the requests Jira lets them see on the portal: their own, those they take part in and those shared with their organisations. The app asks Jira as that customer, every time.

The only exception is a project setting, **Show all requests in this project to every customer who can open its portal**. It's off by default; when a project administrator switches it on, every customer of that project sees all of its requests.

## Security, privacy and data

| Measure | Detail |
| --- | --- |
| Jira decides visibility | Which requests a customer sees is always looked up in Jira as that customer. |
| Only chosen fields | Only the fields your administrators allowed are read, and only as plain text; email addresses and other account details never reach the browser. |
| Permission checks | Every settings change checks that the user is a Jira administrator or a project administrator. |
| No data stored from requests | Request data is read when the report opens and not kept. The app stores only its settings and each user's column layout. |
| Personal data reporting | Column layouts are linked to Atlassian account IDs, reported to Atlassian weekly; layouts of closed accounts are erased. |
| Safe error messages | Users see a plain message with a reference; details go to the app's logs only, with Jira's quoted names and values removed. |
| No outside calls | The app has no servers of its own and calls nothing outside Atlassian. Stored data follows your site's data residency location. |

See the [privacy policy](../privacy-policy.html#portal-fields-for-jsm-cloud) for details.

## Permissions and requirements

The app needs Jira Service Management Cloud and a Jira administrator to install it. It asks for these permissions:

| Permission | Why |
| --- | --- |
| `read:jira-work` | Read the requests a user may see, the chosen field values, the list of fields, and administrators' permissions |
| `read:servicedesk-request` | List service projects and request types, and the requests a portal customer may see |
| `storage:app` | Store the app's settings and each user's column layout |
| `report:personal-data` | Report stored account IDs to Atlassian weekly, as Atlassian requires |

The app only reads from Jira; it never changes requests.

## Getting started

Setting up takes about 10 minutes.

1. **Install** Portal Fields for JSM Cloud from the Atlassian Marketplace.
2. Optionally set **defaults** for all projects under Jira settings → Apps → Portal Fields for JSM Cloud.
3. In a service project, go to **Space settings → Apps → Portal Fields for JSM Cloud**, tick **Show the "Advanced request report" section**, choose the extra columns and charts, and click **Save**.
4. Open the customer portal's **Requests** page and try **Open report** and **Charts**.

The [Admin guide](./admin-guide.html) walks through each step.

## Current limitations

We list these openly so you can judge fit before installing.

| Area | Today |
| --- | --- |
| The portal's own Requests list | Can't be changed by apps; the report opens next to it, in a pop-up |
| Number of requests | Up to 1,000 per load, most recent first; narrow the filters to see others |
| Very large or slow sites | If loading takes too long, the report shows what it loaded, with a note |
| Assets fields | Not supported |
| Anonymous portal visitors | Don't see the section; customers need to log in |
| Status names | For agents and admins, the report may show the Jira status name rather than the portal's name for it |
| Charts | Built from the same requests as the report (up to 1,000); the SLA chart uses SLA fields among the chosen columns |

## FAQ

**Do customers see requests they couldn't see before?** No, unless an administrator switches on "Show all requests" for a project. Otherwise customers see exactly what Jira shows them on the portal.

**Can customers see internal fields?** Only if an administrator adds them as columns. Choose only fields your customers may see; Assignee, for example, shows agents' names.

**Does the app change our requests?** No. It only reads from Jira.

**Do customers need a Jira licence?** No. Portal customers use the report without a Jira licence.

**Where is the data stored?** Only on Atlassian's Forge platform, on your site: the app's settings and each user's column layout. Request data isn't stored.

**Can we use it on some projects only?** Yes. The app is off by default and is switched on per service project.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#portal-fields-for-jsm-cloud) · [End-user terms](./end-user-terms.html)
