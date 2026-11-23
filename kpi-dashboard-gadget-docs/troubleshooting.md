---
layout: default
title: Troubleshooting
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Troubleshooting

*Last updated: 23 November 2026 · Rahul Savaikar*

## Messages

| Message | What to do |
| --- | --- |
| Choose the tickets for this gadget: edit the dashboard, open the gadget menu (⋯), select Configure, and enter a JQL query or saved filter. | The gadget has no query or filter yet. Open its settings and save one. |
| Searching tickets failed: Jira rejected the query. Check the JQL or filter in the gadget settings. | The query is invalid, or the saved filter was deleted or isn't shared with you. Open the settings and click **Check query**: it shows Jira's reason to you as the editor. Share the filter with the dashboard's viewers, or use a JQL query instead. |
| Searching tickets failed: you do not have permission. | Your Jira account can't run this search. Ask a Jira administrator for access to the project. |
| Searching tickets failed: Jira is busy. Try again in a minute. | Jira limited the number of requests. Click **Reload** after a minute. |
| These Jira fields were not found, so related figures show as empty: … | A chosen field was deleted, you can't see it, or a typed field name doesn't match your site. Choose another under **Fields, organization groups and SLA targets**, or choose **Not used** for optional fields. |
| These fields hold a date with no time, so response times are approximate: … | The assignment or first response field has a date but no time, so it reads as midnight. Choose a date-time field if your site has one. |
| Only the first 4,000 tickets are included. Narrow the JQL query or filter for complete figures. | The query matches more than 4,000 tickets. Add a project or date condition, and `ORDER BY created DESC` so the newest are kept. |
| No tickets match this query for your account. | The query finds nothing you may see. Check it with **Open this query in Jira**. |
| More than 400 tickets, so Jira opened the dashboard's query … | A link carries up to 400 ticket keys. Jira opened the gadget's query; the filters named in the message aren't applied there. |
| Jira opened the first 400 of *N* tickets, because a link cannot carry more. | Narrow the selection to open the rest. |
| Enter a JQL query. · The JQL query is too long. · Choose a saved filter. | Shown in the settings: enter a query of up to 4,000 characters, or select a filter. |
| Could not fetch priorities; the saved list is kept. · Jira returned no priorities for this query; the saved list is kept. | SLA Overview settings. Check the query, then click **Fetch priorities again**. The saved priorities and targets stay in use. |
| Tickets with no SLA target (no target for their priority, or no Jira SLA on the ticket) are not counted | SLA Overview. Set a target for that priority, or check that the Jira SLA applies to those tickets. |

## Common situations

**The gadget is blank or keeps loading.**
Reload the dashboard (Ctrl+F5). If you changed settings, check that you clicked **Save settings**.

**The gadget still looks like the old version after an update.**
Your browser may be showing a saved copy. Press Ctrl+F5 on the dashboard, or open it in a private window.

**Response times or SLA show "—".**
The first response date field isn't found or is empty on these tickets. Check it in the settings, and use **Check query** to see how many tickets have it filled. With an assignment date field set, response time also needs that field or falls back to the created date.

**Many tickets have no first response date.**
In our testing, Jira's `[CHART] Date of First Response` filled in when someone other than the reporter added a public comment, and stayed empty when the reporter commented. A ticket raised and answered by the same person shows no first response.

**Many tickets are left out because the response is before the start date.**
The first response is earlier than the assignment date, for example because agents reply before a ticket is assigned, or because the assignment date is set late or changes on reassignment. Those tickets aren't in the average, so check how the assignment field is filled.

**Response time and SLA don't match.**
Working as intended: response time is elapsed time, while a Jira SLA follows its calendar and pauses. To measure against simple targets instead, choose **Not used** for the first response SLA.

**SLA figures are empty, or fewer than the ticket count.**
Only SLAs that have finished or breached are counted, not those still running or paused. Check that the right SLA is chosen, and that the tickets' project uses it.

**The key figures take more than one row.**
They fit on one row on a wide gadget and wrap on a narrower one, such as a half-width dashboard column. Use a one-column layout, or maximise the gadget.

**Organisations or CSAT are empty.**
By default these use Jira Service Management's **Organizations** and **Satisfaction** fields. Check the fields chosen in the settings. Projects without them, or tickets without values, show nothing there.

**Canceled or declined tickets count as open.**
The KPI Dashboard's **Closed tickets** and **Open / not closed** figures go by the status name: closed means the name contains closed, resolved, completed or done. Other statuses, such as Canceled or Declined, count as not closed, even though Jira treats them as done. **Stale tickets** follows Jira's status category, so those tickets are not stale.

**SLA Overview doesn't match Jira's SLA.**
Choose your Jira SLA fields under **First response SLA** and **Resolution SLA**. With the gadget's own targets, it measures between two dates on its own clock and doesn't know Jira's pauses, stop conditions (such as a move to Awaiting Closure) or per-customer goals and calendars.

**A ticket is listed as a first response past target, but it has been answered.**
With the gadget's own targets, first response stops at the first response date field, `[CHART] Date of First Response`, which fills only on a public comment by someone other than the reporter. Choose your Jira *Time to first response* SLA field to use Jira's stop conditions instead.

**SLA by priority shows fewer tickets than Jira's search.**
It follows the Month filter, which starts on the latest month. Choose **All months** to cover the whole query.

**Response after assignment is missing, or shows "not measured".**
It appears only when an **Assignment date field** is set, and measures only tickets that have an assignment date. Reading comments makes the gadget load more slowly.

**The SLA Overview priorities don't match our project.**
Open the settings and click **Fetch priorities again**. The list comes from the priorities the query's projects allow when creating tickets; if you can't create tickets there, every priority on the site is listed.

**I can't choose a saved filter.**
Click the filter in the list until **Selected:** shows its name below the list, then **Save settings**. If it still fails, press Ctrl+F5 to load the latest version of the gadget.

**Partners count as customers.**
Set the **Partner organization prefix** in the gadget's settings.

**There's no Request scope filter.**
It appears only when a request scope field is set in the settings and the tickets have values in it.

**Two people see different numbers.**
Working as intended: each viewer sees only the tickets their own Jira permissions and issue security allow.

**Recently resolved shows few tickets in Partner Case Overview.**
That gadget uses only the tickets its query returns. Include resolved tickets, for example `(statusCategory != Done OR resolved >= -7d)`.

---

[Support](./support.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
