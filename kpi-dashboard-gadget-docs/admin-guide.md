---
layout: default
title: Admin guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For dashboard viewers](./for-viewers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# KPI Dashboard Gadget – Admin guide

*Last updated: 30 October 2026 · Rahul Savaikar*

This guide is for Jira administrators, who install the app, and for anyone who builds dashboards with its gadgets.

## 1. Install

1. In Jira, go to **Apps → Explore more apps**, search for **KPI Dashboard Gadget** and click **Get app**.
2. Review the permission and confirm. A Jira administrator is needed for this step.

The app adds two gadgets to Jira's gadget list: **KPI Dashboard** and **Partner Case Overview**.

## 2. Add a gadget to a dashboard

1. Open a dashboard you can edit and click **Edit**.
2. Click **Add gadget**, search for **KPI Dashboard** or **Partner Case Overview**, and click **Add**.
3. The new gadget opens its settings. Until a query or filter is saved, it asks to be set up.

## 3. Choose the tickets

Under **Tickets to include**, choose one:

- **JQL query:** enter a query, for example `project = ABC AND created >= startOfMonth(-2) ORDER BY created DESC` for KPI Dashboard, or `project = ABC AND (statusCategory != Done OR resolved >= -7d) ORDER BY created DESC` for Partner Case Overview.
- **Saved filter:** search for a filter by name and select it. Everyone who views the dashboard needs access to that filter; if they don't, they see an error instead of data. A JQL query avoids that.

Keep the query bounded, for example by project and date: each gadget loads up to 3,000 tickets.

Click **Check query** to run it as yourself. The gadget shows the first matching ticket and roughly how many tickets match, or Jira's reason if the query is invalid. **Open this query in Jira** runs the exact query in Jira's issue search for comparison.

Under **Refresh data every**, choose 15, 30 or 60 minutes.

## 4. Fields, organization groups and SLA targets

Open **Fields, organization groups and SLA targets** to fit the gadget to your site. Each gadget has its own settings.

**KPI Dashboard**

| Setting | Default | What it does |
| --- | --- | --- |
| Assignment date field | Blank | Optional. A date-time field set when a ticket is assigned. Response time and the Month filter then start from it; blank means they use the created date. |
| First response date field | `[CHART] Date of First Response` | When the first response was made. Jira Service Management creates this field. |
| Satisfaction rating field | `Satisfaction` | The customer satisfaction rating (1–5) from Service Management surveys |
| Request scope field | Blank | Optional. A select field; when set, the gadget adds a **Request scope** filter. |
| Partner organization prefix | Blank | Optional. Organisations whose names start with it, for example `PRT_`, are grouped as partners. |
| Customer organization prefix | Blank | Optional. Organisations whose names start with it, for example `CST_`, are grouped as customers. |
| Response target, Highest/High/Medium (hours) | 2 | First-response target for urgent priorities, including Critical |
| Response target, Low/Lowest (hours) | 24 | First-response target for other priorities, including Trivial |

**Partner Case Overview** shows only the two organisation prefixes, under **Organization groups**. Organisations that start with the partner prefix are partners; every other organisation on a ticket counts as a customer.

Enter field names exactly as Jira shows them. Fields the gadget can't find are listed on the gadget, and their figures stay empty; nothing else breaks. Leave a field blank if your site has no such field.

## 5. Save

Click **Save settings**, then close the gadget's settings. The gadget loads its tickets as you, and as each viewer when they open the dashboard.

## 6. Share the dashboard

Share the dashboard as usual in Jira. Each viewer sees only the tickets their own Jira permissions and issue security allow, so two viewers may see different figures on the same dashboard.

## 7. Run and maintain

- **Updates** are installed by Atlassian automatically. If an update needs new permissions, a Jira administrator approves it under **Apps → Manage apps**.
- **Changing settings:** open the gadget's menu (⋯) and choose **Configure**. Changes apply after **Save settings**.
- **Uninstalling** under **Apps → Manage apps** removes the gadgets from your dashboards, together with their settings.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#kpi-dashboard-gadget) · [End-user terms](./end-user-terms.html)
