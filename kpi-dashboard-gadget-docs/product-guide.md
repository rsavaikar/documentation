---
layout: default
title: Product guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# KPI Dashboard Gadget – Product guide

*Last updated: 30 October 2026 · Rahul Savaikar*

KPI Dashboard Gadget adds two gadgets to Jira dashboards: **KPI Dashboard** for support performance and **Partner Case Overview** for open cases per partner and customer. You choose the tickets with a JQL query or saved filter; Jira decides which of them each viewer may see. This guide explains what the gadgets show, how they protect your data and how to get started.

## Highlights

**Support KPIs without exports.** Tickets, closed tickets, average response and resolution time, first-response SLA, customer satisfaction and engineer performance, calculated live in the dashboard.

**Every viewer sees only their own tickets.** The gadgets read Jira as the person viewing the dashboard, so project permissions, issue security levels and filter sharing always apply.

**Fits your site.** Field names, partner and customer organisation prefixes and SLA targets are settings in each gadget, so nothing is tied to one Jira site.

**Nothing leaves Atlassian.** The app runs entirely on Atlassian's Forge platform, stores no ticket data and is eligible for Atlassian's "Runs on Atlassian" programme.

## Overview

| Gadget | Shows |
| --- | --- |
| KPI Dashboard | Key figures, a monthly ticket trend, tickets by engineer and by organisation, engineer performance, recently resolved tickets and customer satisfaction (CSAT), with filters for month, engineer, organisation, status and request scope |
| Partner Case Overview | Open cases per customer and by status, open cases, high and critical priority cases, stale cases and recently resolved cases, filtered by partner or organisation |

Typical uses:

- **Team reviews:** response and resolution times, SLA and CSAT per month and per engineer.
- **Account and partner meetings:** open and stale cases for one partner and its customers, with security levels shown where tickets have them.
- **Daily triage:** high and critical open cases, oldest first, one click from Jira.

## What the gadgets show

**KPI Dashboard**

| Feature | Details |
| --- | --- |
| Key figures | Tickets, closed tickets, average response time, average resolution time, first-response SLA met, CSAT satisfied, open tickets, stale tickets, tickets resolved in the last 7 days |
| Charts | Monthly ticket trend; tickets by engineer; tickets by organisation, with a share-of-tickets pie; CSAT score distribution and monthly CSAT trend |
| Tables | Engineer performance; recently resolved (last 7 days); CSAT by engineer |
| Filters | Month; engineers; organisations (tickets with any or all of them); status; request scope, if a scope field is set |
| Drill-down | Click a bar, point, slice, engineer or key to open those tickets in Jira's issue search |
| Exports | Each chart as PNG (with its filters and date); engineer performance and recently resolved as CSV |

**Partner Case Overview**

| Feature | Details |
| --- | --- |
| Key figures | Open cases and customers, high or critical open cases, average age of open cases, stale cases, cases resolved in the last 7 days |
| Charts | Open cases per customer (pie); open cases per customer by status |
| Tables | Open cases (sort by oldest, newest or least recently updated); high and critical priority cases; stale cases (no update in 14 days or more); recently resolved; a **Security level** column when tickets have one |
| Filter | Partner or organisations (tickets with any or all of them) |
| Exports | Each chart as PNG |

Both gadgets load up to 3,000 tickets and refresh every 15, 30 or 60 minutes, or on **Reload**.

## How the figures are calculated

| Figure | Rule |
| --- | --- |
| Response time | First response date minus assignment date. Without an assignment date field, or if a ticket has none, from the created date. |
| Resolution time | Resolved date minus created date |
| First-response SLA | Response time within the target for the ticket's priority: Highest, Critical, High and Medium use the urgent target (2 hours by default); Lowest, Low and Trivial use the other target (24 hours by default). Elapsed time, not business hours. |
| CSAT | Ratings 1–5 only; 4 or 5 is satisfied, 1 or 2 dissatisfied |
| Month | Assignment month, else created month |
| Stale | Open and not updated for 14 days or more |
| Customer (Partner Case Overview) | Each organisation on the ticket that doesn't start with the partner prefix |

These are elapsed-time figures from Jira fields. Check them against your Jira SLA configuration before using them in contracts.

## Security, privacy and data

| Measure | Detail |
| --- | --- |
| Jira decides visibility | Every Jira call is made as the person viewing the dashboard. The app never reads Jira as itself, so issue security levels, project permissions and filter sharing always apply. |
| No data stored | Ticket data is read when the gadget loads and kept only in the viewer's browser. The app stores nothing; the gadget's settings are saved by Jira with the dashboard. |
| Read-only | The app only reads from Jira; it never changes tickets. |
| Safe error messages | Errors show a plain message. The app's logs record which part failed and the status code, never the query or Jira's error text, which can quote names and values. |
| Safe exports | In CSV files, cells that a spreadsheet could run as a formula are written as plain text, including in spreadsheets that use `;` as the list separator. |
| No outside calls | The app has no servers of its own and calls nothing outside Atlassian. |

See the [privacy policy](../privacy-policy.html#kpi-dashboard-gadget) for details.

## Permissions and requirements

The gadgets need Jira Cloud and a Jira administrator to install the app. Organisation and satisfaction figures use Jira Service Management's **Organizations** and **Satisfaction** fields; on projects without them those figures stay empty. The app asks for one permission:

| Permission | Why |
| --- | --- |
| `read:jira-work` | Run the gadget's query or saved filter, list fields and search saved filters, all as the viewing user |

## Getting started

Setting up takes about 10 minutes.

1. **Install** KPI Dashboard Gadget from the Atlassian Marketplace.
2. On a Jira dashboard, click **Edit → Add gadget** and add **KPI Dashboard** or **Partner Case Overview**.
3. In the gadget's settings, enter a JQL query or choose a saved filter, click **Check query**, and set the field names and prefixes for your site under **Fields, organization groups and SLA targets**.
4. Click **Save settings** and close the settings.

The [Admin guide](./admin-guide.html) walks through each step.

## Current limitations

We list these openly so you can judge fit before installing.

| Area | Today |
| --- | --- |
| Number of tickets | Up to 3,000 per gadget; narrow the query for complete figures |
| SLA | Elapsed time against two targets by priority; not Jira's SLA calendars or goals |
| Links to Jira | A drill-down link carries up to 400 ticket keys; for more, Jira opens the gadget's query instead |
| Recently resolved | Adds tickets resolved in the last 7 days that match the gadget's query apart from its created-date conditions; needs a query with a project clause |
| Organisations | Uses the Jira Service Management Organizations field only |

## FAQ

**Do viewers see tickets they couldn't see in Jira?** No. The gadgets ask Jira as each viewer, so a viewer sees exactly the tickets Jira's issue search shows them, including issue security levels.

**Does the app store our tickets?** No. Ticket data is loaded into the viewer's browser and not kept. The app stores nothing; Jira keeps each gadget's settings with the dashboard.

**Does it work without Jira Service Management?** Yes, but organisation and satisfaction figures need Service Management's Organizations and Satisfaction fields.

**Our field names are different. Can we still use it?** Yes. Set your own field names in each gadget's settings. Optional fields can be left blank.

**Does the app change tickets?** No. It only reads from Jira.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
