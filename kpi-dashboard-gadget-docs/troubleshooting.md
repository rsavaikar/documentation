---
layout: default
title: Troubleshooting
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Troubleshooting

*Last updated: 30 October 2026 · Rahul Savaikar*

## Messages

| Message | What to do |
| --- | --- |
| Choose the tickets for this gadget: open the gadget menu (⋯), select Edit, and enter a JQL query or saved filter. | The gadget has no query or filter yet. Open its settings and save one. |
| Searching tickets failed: Jira rejected the query. Check the JQL or filter in the gadget settings. | The query is invalid, or the saved filter was deleted or isn't shared with you. Open the settings and click **Check query**: it shows Jira's reason to you as the editor. Share the filter with the dashboard's viewers, or use a JQL query instead. |
| Searching tickets failed: you do not have permission. | Your Jira account can't run this search. Ask a Jira administrator for access to the project. |
| Searching tickets failed: Jira is busy. Try again in a minute. | Jira limited the number of requests. Click **Reload** after a minute. |
| These Jira fields were not found, so related figures show as empty: … | The field names in the gadget's settings don't match your site. Correct them under **Fields, organization groups and SLA targets**, or leave optional fields blank. |
| Only the first 3,000 tickets are included. Narrow the JQL query or filter for complete figures. | The query matches more than 3,000 tickets. Add a project or date condition. |
| No tickets match this query for your account. | The query finds nothing you may see. Check it with **Open this query in Jira**. |
| More than 400 tickets, so Jira opened the dashboard's query … | A link carries up to 400 ticket keys. Jira opened the gadget's query; the filters named in the message aren't applied there. |
| Jira opened the first 400 of *N* tickets, because a link cannot carry more. | Narrow the selection to open the rest. |
| Enter a JQL query. · The JQL query is too long. · Choose a saved filter. | Shown in the settings: enter a query of up to 4,000 characters, or select a filter. |

## Common situations

**The gadget is blank or keeps loading.**
Reload the dashboard (Ctrl+F5). If you changed settings, check that you clicked **Save settings**.

**Response times or SLA show "—".**
The first response date field isn't found or is empty on these tickets. Check its name in the settings. With an assignment date field set, response time also needs that field or falls back to the created date.

**Organisations or CSAT are empty.**
These use Jira Service Management's **Organizations** and **Satisfaction** fields. Projects without them, or tickets without values, show nothing there.

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
