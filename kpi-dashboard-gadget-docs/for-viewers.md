---
layout: default
title: For dashboard viewers
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# KPI Dashboard Gadget – For dashboard viewers

*Last updated: 30 October 2026 · Rahul Savaikar*

This page explains how to read and use the gadgets on a dashboard. The figures include only the tickets your own Jira permissions and issue security allow you to see, so a colleague may see different numbers on the same dashboard.

## KPI Dashboard

**Filters** at the top narrow every figure, chart and table below them:

| Filter | What it does |
| --- | --- |
| Month | One month, by assignment date or, without one, by created date |
| Engineer(s) | One or more assignees |
| Organization(s) | One or more organisations; choose whether tickets need **any** or **all** of them |
| Status | One or more statuses |
| Request scope | One scope; shown only if your administrator set a scope field |
| Response time measured from | Assignment time, else created time; or assignment time only. Shown only if an assignment date field is set. |

**Reset filters** clears them. **Reload** fetches the tickets again; the gadget also refreshes on its own schedule.

**Key figures:**

| Figure | Meaning |
| --- | --- |
| Tickets in selected data | Tickets that match the filters |
| Closed tickets | Of those, closed, with the share |
| Resolved, last 7 days | Ignores the Month filter |
| Average response time | First response minus assignment (or created) time, for tickets with both dates |
| Average resolution time | Resolved minus created time |
| First response SLA met | Share of tickets whose response time was within the target for their priority |
| CSAT satisfied | Share of ratings that are 4 or 5 |
| Open / not closed | Tickets not yet closed |
| Stale tickets | Open and not updated for 14 days or more; ignores the Month filter |

**Charts and tables:**

- **Monthly ticket trend**, **Tickets by engineer** and **Tickets by organization**: click a point or bar to open those tickets in Jira. With partner and customer prefixes set, **Show** limits the organisation chart to partners, customers or others.
- **Share of tickets by organization:** a pie of the organisations you choose.
- **Engineer performance:** tickets, closed, average response and resolution time, SLA met and CSAT per engineer. Search by name; **Export CSV** saves the table.
- **Recently resolved (last 7 days):** tickets resolved in the last week, by resolved date. It also includes recently resolved tickets that the gadget's query leaves out only because of their created date. **Export CSV** saves the table.
- **Customer satisfaction (CSAT):** average score, satisfied and dissatisfied shares, survey responses, the score distribution, a monthly trend and CSAT by engineer.

**Export PNG** on a chart saves it as an image, with the active filters and the date.

## Partner Case Overview

**Partner / organization(s)** narrows everything to one or more organisations; choose whether tickets need **any** or **all** of them. **Reset filters** clears the choice.

| Figure or section | Meaning |
| --- | --- |
| Open cases | Open tickets, and how many customers they belong to |
| High / critical open cases | Open tickets with priority Highest, Critical or High |
| Average age of open cases | From created date to now |
| Stale cases | Open and not updated for 14 days or more |
| Resolved, last 7 days | Needs resolved tickets in the gadget's query, for example `resolved >= -7d` |
| Open cases per customer | Pie; a ticket with several customers counts under each. Click a slice to open those tickets in Jira. |
| Open cases per customer, by status | Busiest customers first. Click a segment to open those tickets. |
| Open cases, High and critical priority cases, Stale cases, Recently resolved | Tables; click a key to open the ticket. Sort open cases by oldest, newest or least recently updated. A **Security level** column appears when tickets have one. |

A customer is any organisation on the ticket that doesn't start with the partner prefix. Tickets without one show under **No customer organization**.

## Opening tickets in Jira

Clicking a chart or key opens Jira's issue search in a new tab, with exactly those tickets. A link carries up to 400 ticket keys; for more, Jira opens the gadget's query instead, and the gadget tells you which filters aren't applied there.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
