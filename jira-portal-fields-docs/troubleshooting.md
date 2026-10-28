---
layout: default
title: Troubleshooting
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For portal customers](./for-customers.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Troubleshooting

*Last updated: 28 October 2026 · Rahul Savaikar*

## Messages

| Message | What to do |
| --- | --- |
| Something went wrong. Please try again. If it keeps happening, give your Jira admin this reference: … | Try again. If it repeats, send the reference to [support](./support.html); it points to the details in the app's logs. |
| This took too long to load. Please try again, or narrow the filters. | Try again, or choose fewer requests, for example **Created by me** or **Open requests**. |
| Loading took too long, so only some requests are shown. | The report shows what it loaded in time. Narrow the filters, for example to **Created by me**, one service project or **Open requests**, to see a complete list. |
| Showing the first 1000 requests. Narrow the filters to see the others. | The report loads up to 1,000 requests at a time. Narrow the filters, or pick one service project. |
| *N* matching requests from *project* aren't shown, because this report is turned off for that service project. | Working as intended: the app is off for that project. A project administrator can switch it on under **Space settings → Apps → Portal Fields for JSM Cloud**. |
| The request report isn't enabled for any portal you can access. | The app is off for every portal this user can open. Switch it on for a project, and save. |
| No SLA fields are available. … | Add an SLA field (for example Time to resolution) to the project's extra columns. |
| No fields are available to group by. … | Add a select list, user, organisation or label field to the project's extra columns. |
| No requests to show for these filters. | Change the filters, for example to **Any status** or **All requests I can see**. |
| Please log in to the portal to see your requests. | The user isn't logged in. The report isn't available to anonymous visitors. |
| You need Jira administrator permission for this page. | The defaults page under Jira settings → Apps is for Jira administrators only. |
| You need project administrator permission for this page. | The project's settings page is for that project's administrators. |

## Common situations

**The "Advanced request report" section doesn't appear.**

1. Check that you're on the portal's **Requests** page. The section doesn't show on request pages, the portal home or Approvals.
2. Check that the user is logged in.
3. Check that the app is switched on for at least one service project the user can open, and that its settings were **saved**: the project's settings page says "Not saved yet" otherwise.
4. Reload the page (Ctrl+F5) after changing settings or after an app update.

**A customer can't find a request in the report.**
Check the filters: **Open requests** hides resolved requests, and **Created by me** hides requests others created. If the request is in a project where the app is off, the report says so at the top.

**A field I want isn't in the column picker.**
The standard columns (Type, Reference, Summary, Status, Service project, Requester) are always shown, so they aren't in the list. Untick **Only fields used on this project's request types** to see fields that aren't on a portal form. Assets fields and some system fields, such as attachments, comments and description, aren't supported.

**My settings don't take effect.**
Click **Save**; settings apply only once saved. **Save** stays greyed out until something has changed.

**The Charts button is missing.**
No chart is ticked under **Charts** in the project's settings.

**Downloads don't start.**
The browser may be blocking downloads from the portal. Allow downloads for your Jira site and try again.

**A status name differs from the portal's.**
For agents and administrators, the report may show the Jira status name rather than the name the portal shows customers.

---

[Support](./support.html) · [Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
