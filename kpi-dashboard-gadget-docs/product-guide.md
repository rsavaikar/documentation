---
layout: default
title: Product guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Release notes](./release-notes.html) · [Support](./support.html)

# KPI Dashboard Gadget – Product guide

*Last updated: 23 November 2026 · Rahul Savaikar*

KPI Dashboard Gadget adds three gadgets to Jira dashboards: **KPI Dashboard** for support performance, **Partner Case Overview** for open cases per partner and customer, and **SLA Overview** for service-level results. You choose the tickets with a JQL query or saved filter; Jira decides which of them each viewer may see. This guide explains what the gadgets show, how they protect your data and how to get started.

## Highlights

**Support KPIs without exports.** Tickets, closed tickets, average response and resolution time, first-response and resolution SLA, customer satisfaction and engineer performance, calculated live in the dashboard.

**Jira's own SLA results.** Choose a Jira Service Management SLA and the gadget shows Jira's met or breached result, on the SLA's own calendar and pause rules, or measure against simple elapsed-time targets if you prefer.

**SLAs you can act on.** SLA Overview shows met and breached shares, median times, tickets about to breach in the next hours and tickets already past target, by priority, organisation and engineer. It can also time the first reply after a ticket is assigned.

**Every viewer sees only their own tickets.** The gadgets read Jira as the person viewing the dashboard, so project permissions, issue security levels and filter sharing always apply.

**Fits your site.** Choose each gadget's fields from lists of your own Jira fields, and set partner and customer organisation prefixes and SLA targets, so nothing is tied to one Jira site.

**Nothing leaves Atlassian.** The app runs entirely on Atlassian's Forge platform, stores no ticket data and is eligible for Atlassian's "Runs on Atlassian" programme.

## Overview

| Gadget | Shows |
| --- | --- |
| KPI Dashboard | Key figures, a monthly ticket trend, tickets by engineer and by organisation, engineer performance, recently resolved tickets and customer satisfaction (CSAT), with filters for month, engineer, organisation, status and request scope |
| Partner Case Overview | Open cases per customer and by status, open cases, high and critical priority cases, stale cases and recently resolved cases, filtered by partner or organisation |
| SLA Overview | First response and resolution SLAs, from the created date or the assignment date, and response after assignment: met and breached, median and 90th-percentile times, open tickets about to breach or past target, an SLA trend by month or by week, and SLA by priority, organisation and engineer |

Typical uses:

- **Team reviews:** response and resolution times, SLA and CSAT per month and per engineer.
- **Account and partner meetings:** open and stale cases for one partner and its customers, with security levels shown where tickets have them.
- **Daily triage:** high and critical open cases, oldest first, one click from Jira; tickets about to breach their SLA in the next few hours.
- **Customer and contract reviews:** SLA met and breached per organisation, against each customer's own Jira SLA goals.

## What the gadgets show

**KPI Dashboard**

| Feature | Details |
| --- | --- |
| Key figures | Tickets, closed tickets, average response time, average resolution time, first-response SLA met, resolution SLA met (if a resolution SLA is chosen), CSAT satisfied, open tickets, stale tickets, tickets resolved in the last 7 days, and **Black hole tickets** (if black hole statuses are set). The response figure also says how many tickets were measured, excluded or have no first response date. |
| Charts | Monthly ticket trend; tickets by engineer, split by status; tickets by organisation, with a share-of-tickets pie; CSAT score distribution and monthly CSAT trend |
| Tables | Engineer performance; recently resolved (last 7 days); CSAT by engineer |
| Filters | Month; engineers; organisations (tickets with any or all of them); status; request scope, if a scope field is set |
| Drill-down | Click a bar, segment, point, slice, engineer or key to open those tickets in Jira's issue search |
| Exports | Each chart as PNG (with its filters and date); engineer performance and recently resolved as CSV |

**Partner Case Overview**

| Feature | Details |
| --- | --- |
| Key figures | Open cases and customers, high or critical open cases, average age of open cases, stale cases, cases resolved in the last 7 days |
| Charts | Open cases per customer (pie); open cases per customer by status |
| Tables | Open cases (sort by oldest, newest or least recently updated); high and critical priority cases; stale cases (no update in 14 days or more); recently resolved; a **Security level** column when tickets have one |
| Filter | Partner or organisations (tickets with any or all of them) |
| Exports | Each chart as PNG |

**SLA Overview**

| Feature | Details |
| --- | --- |
| SLAs | **First response** and **Resolution**, each from a Jira Service Management SLA field or from the gadget's own targets per priority. The gadget's own clocks start from the created date, or from the assignment date if you choose. **Response after assignment** is added whenever an assignment date field is set, including next to Jira SLA fields. A line above the figures says where each clock starts, with a link to this documentation. |
| Key figures | For each SLA: met %, breached %, median time with the 90th percentile; open tickets past target; open tickets at risk (a set share of the target used, 80% by default) |
| Charts | SLA met per month, one line per SLA; with a month selected, SLA met per week of that month (days 1–7, 8–14, 15–21, 22–28 and 29 to the end). Three more charts for the SLA chosen under **Charts show**: open tickets by SLA status (past target, about to breach, at risk, on track); how close measured tickets came to the target; and met vs breached by organisation (the 10 busiest, with **Load more**, or the organisations you choose) |
| Tables | SLA by priority (your Jira priorities, with the goals applied); SLA by organisation, with each customer's **Goal**, **About to breach** within 4, 8 or 24 hours and **Past target**; SLA by engineer; open tickets past or near their target, worst first, with each ticket's Jira status. SLA by organisation and the at-risk list show 10 rows at a time, with **Load more**. |
| Filters | Month; engineers; priorities; organisations (tickets with any or all of them) |
| Drill-down | Click a count, a point, an organisation, an engineer or a key to open those tickets in Jira |
| Exports | Each section as PNG; SLA by priority, SLA by organisation, SLA by engineer and the at-risk list as CSV, with **Export CSV** beside **Export PNG** |

All three gadgets load up to 4,000 tickets and refresh every 15, 30 or 60 minutes, or on **Reload**.

## How the figures are calculated

| Figure | Rule |
| --- | --- |
| Response time | First response date minus assignment date. Without an assignment date field, or if a ticket has none, from the created date. Tickets whose response is earlier than the start date are left out, and the gadget says how many. |
| Resolution time | Resolved date minus created date |
| Closed, Open / not closed | Closed means the status name contains closed, resolved, completed or done; every other status counts as not closed |
| First-response SLA, with an SLA chosen | Jira's own result: met ÷ (met + breached). A ticket that breached in any cycle counts as breached. SLAs still running or paused aren't counted. |
| First-response SLA, without one | Response time within the target for the ticket's priority: Highest, Critical, High and Medium use the urgent target (2 hours by default); Lowest, Low and Trivial use the other target (24 hours by default). Elapsed time, not business hours. |
| Resolution SLA | Jira's own result from the chosen SLA, counted the same way. Shown only when one is chosen. |
| CSAT | Ratings 1–5 only; 4 or 5 is satisfied, 1 or 2 dissatisfied |
| Month | The month of the **Month based on** field if one is chosen, else the assignment month, else the created month |
| Stale | Open, by Jira's status category, and not updated for 14 days or more |
| Customer (Partner Case Overview) | Each organisation on the ticket that doesn't start with the partner prefix |
| Black hole tickets | Open tickets in the chosen black hole statuses (for example Waiting for Development) not updated for the chosen number of days (90 by default), out of all open tickets in those statuses, and how many of them have no issue link. Ignores the Month filter. |

Response and resolution times are elapsed time from Jira fields, while a Jira SLA follows its calendar and pauses, so the two can differ on the same ticket. Without an SLA chosen, check the SLA figures against your Jira SLA configuration before using them in contracts.

**SLA Overview**

| Figure | Rule |
| --- | --- |
| SLA from a Jira SLA field | Jira's own result for each ticket, with its goals per organisation and priority, calendars, pauses and start and stop conditions. A finished cycle counts as met or breached; a running one as running, at risk or past target, and paused when Jira has paused it. A ticket with no cycle on that SLA has no target and isn't counted. Times are on Jira's SLA calendar. |
| Start of the gadget's clocks | **Created** (default), or **Assignment Date/Time (else Created)**, chosen in the settings. With the assignment date, tickets without one start from created, and the gadget says how many. |
| First response, gadget targets | From created: to the first response date field. From the assignment date: to the first comment visible to the customer made after it by someone other than the reporter; internal notes, automation and app comments, and comments by portal customers don't count. |
| Response after assignment | Shown whenever an assignment date field is set, also next to Jira SLA fields, with its own targets and clock. From the assignment date to the first comment visible to the customer made after it by someone other than the reporter; internal notes, automation and app comments, and comments by portal customers don't count. Tickets without an assignment date aren't measured. Hidden when the gadget's own first response already starts from the assignment date, because it would be the same measure. |
| Resolution, gadget targets | From the created date, or the assignment date, to the resolved date. Resolution from the assignment date is an internal measure; customer contracts usually count from creation. |
| Targets | Hours per priority, for the priorities of the projects in the query, as Jira lists them. A blank target means no SLA for that priority. |
| Clock | Per SLA: every hour (24/7), or business hours only (one time zone, working hours and weekdays; daylight saving is followed, public holidays are not excluded) |
| Met %, breached % | Met ÷ (met + breached), over tickets whose clock has stopped; met % + breached % = 100% |
| At risk | Open, within target, with at least the set share of the target used (80% by default) |
| About to breach | Open, within target, with less than 4, 8 or 24 hours left on the SLA's own clock |
| Past target | Open and already over the target; it will breach |
| Month | As in KPI Dashboard. Met, breached and median follow it. The trend shows every month with **All months**, and the weeks of the chosen month otherwise, by the same date. At-risk, about-to-breach and past-target figures ignore it. |

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

The gadgets need Jira Cloud and a Jira administrator to install the app. Satisfaction figures and SLA results use Jira Service Management's fields, and organisation figures use **Organizations** by default; on projects without them those figures stay empty. The app asks for one permission:

| Permission | Why |
| --- | --- |
| `read:jira-work` | Run the gadget's query or saved filter, list fields, search saved filters and read the priorities of the query's projects, all as the viewing user |

## Getting started

Setting up takes about 10 minutes.

1. **Install** KPI Dashboard Gadget from the Atlassian Marketplace.
2. On a Jira dashboard, click **Edit → Add gadget** and add **KPI Dashboard**, **Partner Case Overview** or **SLA Overview**.
3. In the gadget's settings, enter a JQL query or choose a saved filter, click **Check query**, and choose the fields and set the prefixes for your site under **Fields, organization groups and SLA targets** (in SLA Overview, **Fields and SLA targets**, where you also choose your Jira SLA fields or set targets).
4. Click **Save settings** and close the settings.

The [Admin guide](./admin-guide.html) walks through each step.

## Current limitations

We list these openly so you can judge fit before installing.

| Area | Today |
| --- | --- |
| Number of tickets | Up to 4,000 per gadget; narrow the query for complete figures |
| SLA (KPI Dashboard) | Jira's own result when an SLA is chosen; otherwise elapsed time against two targets by priority, not Jira's SLA calendars or goals |
| SLA (SLA Overview, gadget targets) | Business hours use one time zone and don't exclude public holidays. Use the Jira SLA fields for per-customer calendars and goals. |
| Response after assignment, or first response from the assignment date | Reads each ticket's comments, so the gadget loads more slowly. If Jira returns only some of a ticket's comments, the gadget says how many tickets it couldn't check in full. |
| Priority list (SLA Overview) | Read from the projects in up to 500 of the query's tickets, for up to 10 projects, from the priorities a project allows when creating tickets. If none can be read, every priority on the site is listed. |
| Links to Jira | A drill-down link carries up to 400 ticket keys; for more, Jira opens the gadget's query instead |
| Recently resolved | Adds tickets resolved in the last 7 days that match the gadget's query apart from its created-date conditions; needs a query with a project clause |
| Field lists | Show the fields visible to the person editing the gadget. A viewer who can't see a field gets no figures from it. |

## FAQ

**Do viewers see tickets they couldn't see in Jira?** No. The gadgets ask Jira as each viewer, so a viewer sees exactly the tickets Jira's issue search shows them, including issue security levels.

**Does the app store our tickets?** No. Ticket data is loaded into the viewer's browser and not kept. The app stores nothing; Jira keeps each gadget's settings with the dashboard.

**Does it work without Jira Service Management?** Yes, but satisfaction figures and Jira SLA results need Service Management's fields. Organisation figures can use another list field.

**Our field names are different. Can we still use it?** Yes. Choose your own fields from the lists in each gadget's settings. Optional fields can be left as **Not used**.

**Why do response time and SLA differ?** Response time is elapsed time between two dates. A Jira SLA follows its calendar and pauses, so it can record less time for the same ticket, for example outside business hours.

**Does the app change tickets?** No. It only reads from Jira.

**Can the gadget read our SLA settings (goals, calendars) from the project settings?** No. Jira Service Management has no public API for SLA configuration, and Forge apps can't use Jira's internal ones. It doesn't need to: with your Jira SLA fields chosen, each ticket's figures come from the SLA Jira applied to that ticket, with your goals per customer and priority, calendars, pauses and stop conditions. The **Goal** columns show the goals Jira applied.

**Should SLA Overview use our Jira SLAs or its own targets?** Use your Jira SLA fields (for example Time to first response and Time to resolution) when you have them: the gadget then shows exactly what Jira records, with your goals per customer, calendars, pauses and stop conditions. Its own targets suit projects without Jira SLAs, or a measure Jira can't express, such as measuring from the assignment date.

**Does SLA Overview read comment text?** Only to find the first reply after the assignment date, and only when an assignment date field is set (for Response after assignment, or a first response clock that starts from the assignment date). The time of that reply is the only thing passed on to the browser; comment text and authors are not, and nothing is stored.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
