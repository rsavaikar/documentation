---
layout: default
title: For dashboard viewers
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Release notes](./release-notes.html) · [Support](./support.html)

# KPI Dashboard Gadget – For dashboard viewers

*Last updated: 23 November 2026 · Rahul Savaikar*

This page explains how to read and use the gadgets on a dashboard. The figures include only the tickets your own Jira permissions and issue security allow you to see, so a colleague may see different numbers on the same dashboard.

## KPI Dashboard

**Filters** at the top narrow every figure, chart and table below them:

| Filter | What it does |
| --- | --- |
| Month | One month, by the date your administrator chose or, by default, the assignment date or, without one, the created date. The filter's label shows which. |
| Engineer(s) | One or more assignees |
| Organization(s) | One or more organisations; choose whether tickets need **any** or **all** of them |
| Status | One or more statuses |
| Request scope | One scope; shown only if your administrator set a scope field |
| Response time measured from | Assignment time, else created time; or assignment time only. With assignment time only, response figures and the **Tickets by engineer** chart leave out tickets with no assignment date. Shown only if an assignment date field is set. |

**Reset filters** clears them. **Reload** fetches the tickets again; the gadget also refreshes on its own schedule.

**Key figures:**

| Figure | Meaning |
| --- | --- |
| Tickets in selected data | Tickets that match the filters |
| Closed tickets | Of those, closed, with the share. Closed means the status name contains closed, resolved, completed or done. |
| Resolved, last 7 days | Ignores the Month filter |
| Average response time | First response minus assignment (or created) time, for tickets with both dates. The text below it says how many tickets were measured and from which date, how many were left out because the response is earlier than the start date, and how many have no first response date. |
| Average resolution time | Resolved minus created time |
| First response SLA met | If your administrator chose a Jira SLA: Jira's own result, met ÷ (met + breached); SLAs still running or paused aren't counted. Otherwise: share of tickets whose response time was within the target for their priority. |
| Resolution SLA met | Jira's own result, counted the same way. Shown only if your administrator chose a resolution SLA. |
| CSAT satisfied | Share of ratings that are 4 or 5 |
| Open / not closed | All other tickets, which includes statuses such as Canceled or Declined |
| Stale tickets | Open and not updated for 14 days or more; ignores the Month filter |
| Black hole tickets | Open tickets in the waiting statuses your administrator chose, such as Waiting for Development, not updated for the chosen number of days, out of all open tickets in those statuses; and how many have no issue link, for example to a development ticket. Click either number to open those tickets. Shown only if black hole statuses are set; ignores the Month filter. |
| Tickets with response data | Tickets with a measured response time; negative durations are left out |

Response time and a Jira SLA can differ on the same ticket: response time is elapsed time, while the SLA follows its calendar and pauses.

The figures fit on one row on a wide gadget and wrap onto more rows on a narrower one.

**Charts and tables:**

- **Tickets by engineer:** each engineer's bar is split by status, with the least common statuses grouped as **Other statuses**. Click a segment to open that engineer's tickets in that status in Jira.
- **Monthly ticket trend** and **Tickets by organization**: click a point or bar to open those tickets in Jira. With partner and customer prefixes set, **Show** limits the organisation chart to partners, customers or others.
- **Share of tickets by organization:** a pie of the organisations you choose.
- **Engineer performance:** tickets, closed, average response and resolution time, SLA met (with a resolution SLA column if one is chosen) and CSAT per engineer. Search by name; **Export CSV** saves the table.
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

## SLA Overview

**Filters:** Month, Engineer(s), Priority and Organization(s); choose whether tickets need **any** or **all** of the organisations. **Reset filters** clears them.

The footer says how each SLA is measured: from a Jira SLA (**Jira SLA**), or against the gadget's own targets on a **24/7** or **business hours** clock.

| Figure or section | Meaning |
| --- | --- |
| *SLA* met | Share of measured tickets within target, with met and breached counts. Measured means the SLA's clock has stopped. |
| *SLA* breached | Share of measured tickets that missed the target (met + breached = 100%). Click to open them. Open tickets already past target are noted below it. |
| Median time to … | Half of the measured tickets took less than this; **90% within** shows the slowest tenth. Times on business hours or a Jira SLA calendar stay in hours. |
| Open, past target | Open tickets already over a target; they will breach. Ignores the Month filter. |
| Open, at risk | Open tickets that have used the set share of a target (80% by default). Ignores the Month filter. |
| SLA met per month / per week | One line per SLA; the other filters apply. With **All months**, one point per month. With a month chosen, one point per week of that month: days 1–7, 8–14, 15–21, 22–28 and 29 to the end. Click a point to open that month's or week's measured tickets for that SLA. |
| SLA by priority | Met, breached and median per priority, with each priority's target. For an SLA from Jira, the goals Jira applied to those tickets, for example **1–4 h** when customers have different goals. |
| SLA by organization | Per organisation: the **Goal** applied to its tickets (from Jira's SLA data for a Jira SLA), met, breached and median for the selected month, plus **About to breach** (open, less than 4, 8 or 24 hours left; choose under the table's title) and **Past target**. Worst first. A ticket with several organisations counts under each. Shows 10 organisations; **Load more** adds 10. |
| Open tickets past or near their target | One row per ticket and SLA, worst first: priority, Jira status, engineer, which SLA, time so far against the target, and **SLA status**: *Over by …*, or the share of the target used. *Paused* marks a Jira SLA that Jira has paused. Shows 10 rows; **Load more** adds 10. |
| SLA by engineer | Met, breached and median per SLA for each engineer (the current assignee), and their open tickets past target |

**Where the clocks start:** a line above the figures says whether each SLA comes from Jira's own SLA or from the gadget's clock, and whether that clock starts from the created date or the assignment date. With the assignment date, first response stops at the first reply the customer could see after it, by someone other than the reporter; tickets without an assignment date start from created, and the line says how many. **Response after assignment**, shown when your administrator set an assignment date field, is the time from the assignment date to the first reply the customer could see, by someone other than the reporter; tickets without an assignment date aren't measured. **How SLAs are measured** opens this page.

Tables that follow the Month filter say which month and how many tickets they cover; choose **All months** for the whole query. **Export CSV**, beside **Export PNG** at the top right of a table, saves SLA by priority, SLA by organisation, SLA by engineer and the at-risk list, with every row, not only those shown; **Export PNG** saves a section as an image.

## Opening tickets in Jira

Clicking a chart or key opens Jira's issue search in a new tab, with exactly those tickets. A link carries up to 400 ticket keys; for more, Jira opens the gadget's query instead, and the gadget tells you which filters aren't applied there.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
