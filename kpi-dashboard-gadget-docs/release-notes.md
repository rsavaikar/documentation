---
layout: default
title: Release notes
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Release notes](./release-notes.html) · [Support](./support.html)

# KPI Dashboard Gadget – Release notes

*Last updated: 23 November 2026 · Rahul Savaikar*

Updates install automatically. If a dashboard still shows the previous version, press Ctrl+F5. None of these releases needed new permissions.

## 23 November 2026

**SLA Overview: choose where the clocks start.** A new setting, **Start the SLA clocks from**, chooses **Created** (default) or **Assignment Date/Time (else Created)** for the gadget's own first response and resolution clocks:

- From the assignment date, first response stops at the first reply the customer could see after it, and resolution runs to the resolved date. Tickets without an assignment date start from created, and the gadget says how many.
- **Response after assignment** stays available whenever an assignment date field is set, also next to Jira SLA fields, with its own targets. It is hidden only when the gadget's own first response already starts from the assignment date, because it would be the same measure. (An earlier update today removed it; this restores it. Check its targets on gadgets saved in between.)
- The settings describe what each SLA actually measures, including when first response and resolution come from Jira SLA fields.
- A line above the figures says where each SLA's clock starts, or that it comes from Jira's own SLA, with a **How SLAs are measured** link to this documentation. The settings have the same link.
- Jira SLA fields still set their own start; the setting is greyed out when both SLAs use them.

**New: SLA Overview gadget.** A third gadget for service-level results:

- **First response** and **Resolution** SLAs, either from your Jira Service Management SLA fields (with Jira's own goals per customer, calendars, pauses and stop conditions) or from targets per priority on a 24/7 or business-hours clock.
- **Response after assignment:** time from the assignment date to the first reply the customer could see, with its own targets. Shown when an assignment date field is set.
- Targets per priority use the priorities of your projects, fetched from Jira.
- Met, breached and median times; open tickets **at risk**, **about to breach** within 4, 8 or 24 hours, and **past target**.
- An SLA trend by month, or by week when a month is selected.
- SLA by priority, by organisation and by engineer, and a list of open tickets past or near their target, with each ticket's Jira status.
- CSV and PNG exports; long lists show 10 rows with **Load more**.

**All gadgets**

- The gadgets now load up to **4,000** tickets, up from 3,000.
- Fixed: choosing the first filter in the **Saved filter** list didn't register, so the settings couldn't be saved with it.

**Fixes in SLA Overview, same day**

- Fixed: the gadget failed to display when a first response or resolution SLA was left as **Not used**.
- Fixed: CSV exports were named, for example, `sla-at-risk.csv-2026-11-23.csv`; they are now `sla-at-risk-2026-11-23.csv`.
- Clicking a point on the SLA trend now also opens months or weeks with no breaches.
- The **Month** filter lists the newest month first, as in KPI Dashboard.

## 19 November 2026

**KPI Dashboard: Black hole tickets.** A new figure for open tickets parked in waiting statuses, such as Waiting for Development, without an update for a set number of days, and how many of them have no linked issue. Set the statuses and days in the gadget's settings; the figure stays hidden until statuses are set.

## 31 October 2026

**KPI Dashboard:** **Tickets by engineer** is split by status.

## 30 October 2026

- Choose each gadget's fields from lists of your own Jira fields, saved by field ID.
- **Tickets by engineer** follows the **Assignment time only** option.
- Use Jira Service Management's own SLA results for first response and resolution in KPI Dashboard.
- The key figures fit on one row on wide gadgets.
- The set-up message points to **Configure**.

## 29 October 2026

Security review fixes, and settings that no longer depend on one Jira site.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
