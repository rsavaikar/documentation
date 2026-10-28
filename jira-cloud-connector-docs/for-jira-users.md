---
layout: default
redirect_from: /for-jira-users.html
title: For Jira users
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# For Jira users

*Last updated: 28 October 2026 · Rahul Savaikar*

If your project is connected to a partner, you keep working in Jira as usual: Cloud Connector for Jira copies your changes to the partner's linked issue, and theirs to yours, usually within a minute.

## The Cloud Connector panel

On every issue of a connected project, open **Partner sync** on the right of the issue view. It shows:

- the partner's linked issue, as a link you can open (if you have access to their site);
- when the issue last synced, in which direction, and any error;
- or why the issue doesn't sync, for example its type isn't mapped or it lacks the sync label.

If you can edit the issue, you also see a button:

- **Sync now** creates the issue on the partner's site if it isn't there yet.
- **Resync now** sends the issue again: fields, status, files and public comments. Use it if something looks out of date.

**Learn more about partner sync** at the bottom of the panel opens this page.

## What you'll notice

- Issues and comments that came from the partner show **Cloud Connector for Jira** as the author. A partner's comment starts with a bold line such as "**Ann Lee wrote on acme.atlassian.net:**".
- In Service Management projects, partner comments arrive as **internal notes** by default, so your customers don't see them.
- Both issues get a web link to each other.
- Assignee and reporter are not synced: each team assigns work on its own site.

## What stays private

Comments you restrict to a group or role, internal notes, and issues with a security level are **never** sent to the partner. Everything else in a connected project may be shared, so keep internal discussion in restricted comments or internal notes.

---

[Troubleshooting](./troubleshooting.html) · [Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
