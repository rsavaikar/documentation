---
layout: default
title: Admin guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For portal customers](./for-customers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Portal Fields for JSM Cloud – Admin guide

*Last updated: 28 October 2026 · Rahul Savaikar*

This guide is for Jira administrators and service project administrators. The app is **off by default**: nothing appears on any portal until you switch it on for a project.

## 1. Install

1. In Jira, go to **Apps → Explore more apps**, search for **Portal Fields for JSM Cloud** and click **Get app**.
2. Review the permissions and confirm. A Jira administrator is needed for this step.

The app then adds two settings pages: one for defaults across all projects, and one in each service project.

## 2. Set defaults (optional)

**Jira settings → Apps → Portal Fields for JSM Cloud** (Jira administrators). These defaults apply to every service project until a project administrator changes that project's settings.

| Setting | What it does |
| --- | --- |
| Emergency: Turn off Portal Fields for JSM Cloud on all portals | Removes the section, report and charts from every portal at once, overriding every project's own settings. Use it in an incident; projects' settings are kept and apply again when you untick it and save. Off by default. |
| Enable the request report on all service project portals by default | Switches the app on for every project that hasn't saved its own settings. Off by default. |
| Allow CSV and Excel export by default | Shows the export buttons to customers. On by default. |
| Default extra columns | Fields shown after the standard columns, for projects that use the defaults |
| Default charts | Charts customers can open, for projects that haven't chosen their own |

## 3. Switch on a service project

**Space settings → Apps → Portal Fields for JSM Cloud** in the service project (project administrators).

Tick **Show the "Advanced request report" section on this project's customer portal** and click **Save**. Until a project is saved, the page shows whether the report is currently on or off for it, based on the defaults.

**Save** is greyed out until you change something, and a yellow note reminds you of unsaved changes. Settings take effect only after you save. **Reset to defaults** removes the project's own settings again.

## 4. Choose the columns

Customers always see the standard columns: Type, Reference, Summary, Status, Service project and Requester. Under **Extra columns**, choose either:

- **Use the default columns from Jira settings → Apps**, or
- **Choose columns for this project**, then pick up to 30 fields.

In the field picker:

- **Available fields** lists the fields the app supports; search by name. **Only fields used on this project's request types** narrows the list to fields on your portal's request forms; untick it to see every field, including those used only by agents.
- **Selected columns** shows the order customers see; move fields with the arrows or remove them.
- Labels show which fields are **Used in this project** and which are a **Custom field**.

**Choose only fields your customers may see.** Customers see the value of every chosen field, including internal ones such as Assignee, which shows agents' names.

Supported fields: text, numbers, dates, users, select lists (including cascading), labels, organisations, priority, SLA fields and other simple fields. Assets objects, attachments, comments, work logs, issue links, time tracking and description aren't offered.

## 5. Choose the charts

Under **Charts**, tick the charts customers can open:

| Chart | Needs |
| --- | --- |
| Requests by status | Nothing extra |
| Created vs resolved | Nothing extra |
| SLA met vs breached | At least one SLA field (for example Time to resolution) among the extra columns |
| Requests by field | At least one groupable field among the extra columns: a select list, user, organisation or label field |

The SLA chart counts a cycle as **met** if it finished within its goal and **breached** if it finished late, or is still running but already past its goal. Cycles still running within their goal aren't counted yet.

If no chart is ticked, the **Charts** button doesn't appear on the portal.

## 6. Options

| Option | What it does |
| --- | --- |
| Allow customers to export the report as CSV or Excel | Shows the export buttons in the report and the download buttons for charts |
| Show all requests in this project to every customer who can open its portal | **Off by default.** When on, every customer of this project sees all of its requests, including other customers' and other organisations'. Turn it on only if that's intended. |

## 7. Check on the portal

1. Open the customer portal and go to **Requests**.
2. The **Advanced request report** section shows under the page title, with **Open report** and, if any charts are on, **Charts**.
3. Check the columns and charts. To see exactly what a customer sees, log in with a test customer account; agents and administrators may see more requests.

The section shows only on the **Requests** page, not on request pages, the portal home or Approvals.

## 8. Run and maintain

- **Turning a project off:** untick the first box and save. The section then disappears for that project's customers. Customers with requests in it see a note in the report that those requests aren't shown.
- **Turning everything off at once:** in Jira settings → Apps → Portal Fields for JSM Cloud, tick **Turn off Portal Fields for JSM Cloud on all portals** and save. This overrides every project; untick it and save to restore them as they were.
- **Updates** are installed by Atlassian automatically. If an update needs new permissions, a Jira administrator approves it under **Apps → Manage apps**.
- **Customers' column layouts** are saved per account. Customers can reset their own layout; layouts of closed Atlassian accounts are erased automatically.
- **Uninstalling** under **Apps → Manage apps** removes all of the app's settings and stored layouts from your site.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
