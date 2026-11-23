---
layout: default
title: Admin guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Release notes](./release-notes.html) · [Support](./support.html)

# KPI Dashboard Gadget – Admin guide

*Last updated: 23 November 2026 · Rahul Savaikar*

This guide is for Jira administrators, who install the app, and for anyone who builds dashboards with its gadgets.

## 1. Install

1. In Jira, go to **Apps → Explore more apps**, search for **KPI Dashboard Gadget** and click **Get app**.
2. Review the permission and confirm. A Jira administrator is needed for this step.

The app adds three gadgets to Jira's gadget list: **KPI Dashboard**, **Partner Case Overview** and **SLA Overview**.

## 2. Add a gadget to a dashboard

1. Open a dashboard you can edit and click **Edit**.
2. Click **Add gadget**, search for **KPI Dashboard**, **Partner Case Overview** or **SLA Overview**, and click **Add**.
3. The new gadget opens its settings. Until a query or filter is saved, it asks to be set up.

## 3. Choose the tickets

Under **Tickets to include**, choose one:

- **JQL query:** enter a query, for example `project = ABC AND created >= startOfMonth(-2) ORDER BY created DESC` for KPI Dashboard, `project = ABC AND (statusCategory != Done OR resolved >= -7d) ORDER BY created DESC` for Partner Case Overview, or `project = ABC AND created >= startOfMonth(-5) ORDER BY created DESC` for SLA Overview.
- **Saved filter:** search for a filter by name and select it; **Selected:** under the list confirms your choice. Everyone who views the dashboard needs access to that filter; if they don't, they see an error instead of data. A JQL query avoids that. List people by account ID rather than with `currentUser()`, which means someone different for each viewer.

Keep the query bounded, for example by project and date, and add `ORDER BY created DESC` so the newest tickets come first: each gadget loads up to 4,000 tickets.

Click **Check query** to run it as yourself. The gadget shows the first matching ticket and roughly how many tickets match, or Jira's reason if the query is invalid. It also shows how many of the first 100 tickets have each chosen field filled, and how many have a first response before the assignment date, so you can see whether you picked the right fields. **Open this query in Jira** runs the exact query in Jira's issue search for comparison.

Under **Refresh data every**, choose 15, 30 or 60 minutes.

## 4. Fields, organization groups and SLA targets

Open **Fields, organization groups and SLA targets** to fit the gadget to your site. Each gadget has its own settings.

**KPI Dashboard**

| Setting | Default | What it does |
| --- | --- | --- |
| Assignment date field | Not used | Optional. A date field set when a ticket is assigned. Response time and the Month filter then start from it; tickets without it, or a gadget without this field, use the created date. |
| First response date field | `[CHART] Date of First Response` | When the first response was made. Jira Service Management creates this field. |
| Month based on | Same as the assignment date, else created | Optional. Which date decides the month a ticket counts under. |
| Satisfaction rating field | `Satisfaction` | The customer satisfaction rating (1–5) from Service Management surveys |
| Request scope field | Not used | Optional. A select, text or number field; when set, the gadget adds a **Request scope** filter. |
| Organizations field | `Organizations` | The field that holds a ticket's organisations |
| First response SLA | Not used | Optional. A Jira Service Management SLA. When chosen, Jira's met or breached result replaces the two response targets below. |
| Resolution SLA | Not used | Optional. A Jira Service Management SLA. When chosen, the gadget adds a **Resolution SLA met** figure and an engineer table column. |
| Partner organization prefix | Blank | Optional. Organisations whose names start with it, for example `PRT_`, are grouped as partners. |
| Customer organization prefix | Blank | Optional. Organisations whose names start with it, for example `CST_`, are grouped as customers. |
| Response target, Highest/High/Medium (hours) | 2 | First-response target for urgent priorities, including Critical. Used when no first response SLA is chosen. |
| Response target, Low/Lowest (hours) | 24 | First-response target for other priorities, including Trivial. Used when no first response SLA is chosen. |
| Black hole statuses | Blank | Optional. Statuses, separated by commas, where tickets wait on someone else, for example `Waiting for Development, Waiting for Partner`. Up to 10; names as shown in Jira, upper or lower case. Adds the **Black hole tickets** figure; leave blank to hide it. |
| Black hole after (days without an update) | 90 | Open tickets in the black hole statuses not updated for this many days count as black hole tickets. |

**Partner Case Overview** shows only the Organizations field and the two organisation prefixes, under **Organizations**. Organisations that start with the partner prefix are partners; every other organisation on a ticket counts as a customer.

**SLA Overview** has its settings under **Fields and SLA targets**:

| Setting | Default | What it does |
| --- | --- | --- |
| First response SLA | Not used | Recommended with Jira Service Management. Choose your SLA field, for example *Time to first response*. Jira's own goals, calendars, pauses and stop conditions then apply, and the gadget's targets for it are greyed out. |
| Resolution SLA | Not used | The same for resolution, for example *Time to resolution* |
| Assignment date field | Not used | Optional. Used for the Month filter and, if you choose it below, as the start of the gadget's SLA clocks. |
| Start the SLA clocks from | Created | **Created**, or **Assignment Date/Time (else Created)**. Applies to the gadget's own first response and resolution targets; a Jira SLA field sets its own start. Needs an assignment date field. |
| First response date field | `[CHART] Date of First Response` | Where the gadget's own first response clock stops |
| Month based on | Same as the assignment date, else created | As in KPI Dashboard |
| Organizations field and prefixes | `Organizations`, blank | Used for the Organization(s) filter and SLA by organisation |
| SLA targets | Placeholder hours per priority | One row per priority of the projects in the query, fetched from Jira when the settings open; **Fetch priorities again** refreshes the list. One column per SLA. A blank target means no SLA for that priority. |
| Clock | Every hour (24/7) | Per SLA: 24/7 or business hours only |
| Business hours | Your browser's time zone, 08:00–16:00, Monday to Friday | Used by any SLA on business hours. Public holidays are not excluded. |
| At risk when this much of the target is used (%) | 80 | When an open ticket counts as at risk |

Tips for SLA Overview:

- Prefer the Jira SLA fields when you have them: the figures then match Jira exactly, including different goals and calendars per customer. For example, a response clock that Jira stops when a ticket enters Awaiting Closure is stopped in the gadget too.
- **Starting from the assignment date:** first response runs to the first public agent reply after the assignment date, and resolution to the resolved date. Tickets without an assignment date start from created, and the gadget says how many. First response then reads each ticket's comments, so the gadget loads more slowly. The settings and the gadget link to this guide under **How SLAs are measured**.
- For customers with different cover, such as 24/7 first response, add another SLA Overview gadget with its own query and settings.
- The priority list is read from the priorities a project allows when creating tickets, so the person editing the gadget needs to be able to create tickets in those projects. Otherwise every priority on the site is listed.

**Choosing fields.** Each setting is a list of your site's Jira fields, showing only fields that suit it, with each field's name and ID so you can tell apart fields with the same name. Choose **Not used** if your site has no such field.

- The date lists offer date fields, including system dates such as Created, but not Updated, which changes on every edit.
- A field with a date but no time can be chosen, with a warning: it reads as midnight, so response times are approximate.
- Fields are saved by their ID, so renaming a field in Jira breaks nothing. A gadget saved with typed field names keeps working and switches to IDs the next time someone saves its settings.
- A field Jira no longer offers is marked **not found in Jira**. It's also listed on the gadget, and its figures stay empty; nothing else breaks.
- The lists show the fields your own Jira account can see.

## 5. Save

Click **Save settings**, then close the gadget's settings. The gadget loads its tickets as you, and as each viewer when they open the dashboard.

## 6. Share the dashboard

Share the dashboard as usual in Jira. Each viewer sees only the tickets their own Jira permissions and issue security allow, so two viewers may see different figures on the same dashboard.

## 7. Run and maintain

- **Updates** are installed by Atlassian automatically. If an update needs new permissions, a Jira administrator approves it under **Apps → Manage apps**. After an update, press Ctrl+F5 on the dashboard if it still shows the old version.
- **Changing settings:** open the gadget's menu (⋯) and choose **Configure**. Changes apply after **Save settings**.
- **Uninstalling** under **Apps → Manage apps** removes the gadgets from your dashboards, together with their settings.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
