---
layout: default
redirect_from: /admin-guide.html
title: Admin guide
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Cloud Connector for Jira – Admin guide

*Last updated: 28 October 2026 · Rahul Savaikar*

This guide is for the Jira administrators who set up Cloud Connector for Jira on their site. Each partner needs the same steps on its own site; plan about 15 minutes with the partner's admin.

## 1. Install and pair

<p>
  <a href="./install.html" style="display: inline-block; padding: 12px 20px; background-color: #0052cc; color: #ffffff; font-size: 16px; font-weight: bold; text-decoration: none; border-radius: 6px;">Install Cloud Connector for Jira</a>
</p>

1. [**Install Cloud Connector for Jira**](./install.html) on your Jira Cloud site. Sign in with the Atlassian account that administers your site, select the site, review the requested permissions and complete the installation. Your partner installs the app on their site too.
2. **Open the admin page:** Jira settings → Apps → **Cloud Connector**.
3. **Create a connection:** click **New connection** and name it after the partner, for example "Acme Corp". Your partner creates a connection named after you.
4. **Generate a pairing code** on the **Connection** tab and send it **privately** to the partner's admin, for example in a direct message. It contains a shared secret.
5. **The partner pastes the code** into **Or paste the code you received** on their connection and clicks **Pair with this code**.
6. Both pages now show **Paired**. Click **Test connection** on either side; it should report that the partner answered.

A pairing code is valid for 24 hours and works once. **Cancel pairing** revokes a pending code; **Generate new code** replaces it.

## 2. Configure your side

Each admin configures only their own side. Agree with the partner on a few **shared keys** (for example `task`, `bug`, `severity`); each site maps those keys to its own issue types and fields. Every tab has a **Save settings** button.

### Project tab

| Setting | What it does |
| --- | --- |
| Project key | The project this connection syncs; a project can belong to one connection only |
| Sync label | Optional: only issues with this label are sent |
| Sync description / status / comments | Send and accept these changes |
| Add incoming comments as internal notes | Service Management projects only, on by default: keeps the partner's comments off your portal |
| Sync attachments | Off by default; files flow only when both sites switch it on |
| Largest file to send or accept | 1–25 MB, default 10 MB |

### Issue types tab

Map each shared key to one of your issue types and tick **Sync**. In a Service Management project, also choose the **request type** incoming issues get, so they appear in queues. Optionally pick a fallback type for unmapped keys.

### Fields tab

1. **Add field**, pick a field on your site and give it a shared key, for example `severity`.
2. Choose the direction: **Both ways**, **Send only** or **Receive only**.
3. For select-type fields, click **Values** to map options whose names differ, for example your **Critical** to the partner's **Highest**.
4. Under **Fields per issue type**, choose which fields sync for each issue type.

Supported fields: text, paragraph, number, date, date and time, select, radio, multi-select, checkboxes, priority and labels. A value the partner sends that your field doesn't have is skipped and listed under **Activity**; the rest of the update still applies.

### Statuses tab

Map the partner's statuses to yours, one per line, for example `Resolved = Done`. **Load partner statuses** fetches their names for you. An issue only moves if your workflow allows that transition.

## 3. Check and go live

**Check contract** (Connection tab) asks the partner what it has mapped and shows both sides: issue types and fields marked *mapped* or *missing*, which way each field's values flow, and whether both sites accept files. Fix anything flagged before you start; keys mapped on only one side don't sync.

From now on, new and changed issues sync on their own. To bring over **existing issues**:

1. Under **Sync existing issues**, enter JQL to choose them, for example `status != Done AND created >= -90d`. It is always limited to this connection's project; leave it empty for all issues.
2. Click **Count issues** to check the JQL and see how many match.
3. Click **Start initial sync** and confirm. Issues are created on the partner's site in the background, oldest first, with their files and public comments. **Refresh** shows progress; **Cancel** stops it.

Issues already linked are left alone.

## 4. Run and maintain

- **Activity tab:** the latest 30 events of the connection, kept for 30 days, including warnings such as skipped values or missing transitions. Use **What do these messages mean?** below the log, or see [Troubleshooting](./troubleshooting.html).
- **Failed changes:** issues whose change the partner never received are listed above the activity log with the last error. **Retry** sends the whole issue again; temporary problems, such as the partner being down, are retried automatically every hour for a day.
- **Several partners:** create one connection per partner, each with its own project. Partners never see each other's issues.
- **Updates:** the two sites may run different app versions for a while; they keep syncing at the newest version both understand, and the Connection tab says if a feature waits for the partner to update.
- **Disconnect** stops all exchange immediately and tells the partner. Issue links are kept, so pairing again resumes the same issues. **Delete** removes the connection and its stored data.
- **Uninstalling** the app tells paired partners and removes its stored data from your site.

---

[Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#cloud-connector-for-jira) · [End-user terms](./end-user-terms.html)
