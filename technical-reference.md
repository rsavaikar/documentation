---
layout: default
title: Technical reference
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Technical reference](./technical-reference.html) · [Support](./support.html)

# Cloud Connector for Jira – Technical reference

*Last updated: 28 October 2026 · Rahul Savaikar*

This reference is for administrators, security reviewers and developers who want the details behind the guides: architecture, protocol, storage and how the app is built and tested. For setup, the [Admin guide](./admin-guide.html) is the shorter path.

A [Forge](https://developer.atlassian.com/platform/forge/) app that connects **Jira Cloud sites** to each other, for example your site and a customer's, and keeps issues, fields, statuses and comments in sync between one project on each side.

The same app is installed on every participating site. Two installations **pair** once with a short-lived pairing code and then exchange signed messages. Each side only reads from and writes to its **own** Jira, as the app, so no Jira credentials or API tokens are ever shared between organizations.

A site can be paired with **several partners at once**. Each pairing is an isolated *connection* with its own project, settings and secret.

---

## Contents

1. [How it works](#how-it-works)
2. [What syncs](#what-syncs)
3. [Installation](#installation)
4. [Pairing two sites](#pairing-two-sites)
5. [Configuration guide](#configuration-guide)
6. [Several partners and isolation](#several-partners-and-isolation)
7. [Security and data flow](#security-and-data-flow)
8. [Reliability: queue, retries, idempotency](#reliability-queue-retries-idempotency)
9. [Troubleshooting](#troubleshooting)
10. [Development](#development)
11. [Reference](#reference)
12. [Limitations and roadmap](#limitations-and-roadmap)
13. [FAQ](#faq)

---

## How it works

```
 Site A (e.g. your Jira)                                Site B (e.g. the customer's Jira)
 ┌──────────────────────────────┐                        ┌──────────────────────────────┐
 │ Jira event                   │                        │                              │
 │   └─► trigger ─► queue ─► worker ──HTTPS + HMAC──► web trigger ─► Jira REST (asApp) │
 │                              │                        │                              │
 │ Jira REST (asApp) ◄─ web trigger ◄──HTTPS + HMAC── worker ◄─ queue ◄─ trigger       │
 │                              │                        │                   ▲          │
 │ Forge storage (per site):    │                        │               Jira event     │
 │ connections, secrets,        │                        │                              │
 │ settings, issue links, log   │                        │                              │
 └──────────────────────────────┘                        └──────────────────────────────┘
```

1. **Trigger.** A Jira event (issue created or updated, comment added) in a synced project fires a Forge trigger. Changes made by the app itself are filtered out (`ignoreSelf`), which prevents echo loops.
2. **Queue.** The trigger only enqueues a small job (issue id, changed fields, connection). Nothing is sent from the trigger itself.
3. **Worker.** The queue consumer reads the current issue, converts it into a neutral message (shared keys and plain values, never site-specific ids), signs it and sends it to the partner's web trigger.
4. **Web trigger.** The partner verifies the signature with the secret of the named connection, translates the message into its own project, issue types, fields and statuses, and writes to Jira as the app.

Each site stores its own state in Forge storage. Nothing is stored outside Atlassian's platform, and the app has no servers of its own.

Example: a story created on site A.

1. A user on site A creates **SUP-6** (a Story).
2. Site A's connector runs trigger → queue → worker.
3. Site A sends `issue.created` to site B, signed and addressed to B's connection.
4. Site B verifies the signature and maps the shared key `story` to its Story type.
5. Site B creates **ENG-4** in its project, as the app, and answers with `{ issue: ENG-4 }`.
6. Site A stores the link SUP-6 ↔ ENG-4 and adds web links on both issues.

---

## What syncs

| Change on one side | Result on the other side |
|---|---|
| Issue of a **mapped issue type** created in the connection's project (optionally only issues with the sync label) | Issue of the matching type created in the partner's project. Both issues get a web link to each other. |
| Sync label added to an existing, not-yet-linked issue | Same as above |
| Summary edited | Summary updated on the linked issue |
| Description edited (if enabled) | Description updated. Attachments and @mentions can't cross sites, so they become a placeholder and plain text. |
| Status changed (if enabled) | Linked issue moved to the mapped status, if the workflow allows that transition |
| **Mapped field** changed | Matching field updated, with option values translated through the value map |
| Comment added (if enabled) | Comment added with a bold "*Name* wrote on *site*:" line. In Jira Service Management projects it is added as an **internal note** by default. |
| File attached (if switched on **on both sites**) | Same file attached to the linked issue, up to the size limit. Files already on an issue when it is first synced are sent too. |

**Never sent, whatever the settings:**

- comments restricted to a group or project role
- internal notes in Jira Service Management projects
- issues with an **issue security level**, including their comments and files. If a level is added to an issue that already syncs, syncing stops from then on.

Each case is logged under **Activity** on the sending site.

**Mappable field types:** text, paragraph (rich text), number, date, date & time, single select, radio, multi-select, checkboxes, priority and labels, both custom and system fields.

**Not synced yet:** user, cascading-select, version and component fields; attachment deletions, and images embedded in descriptions (they show a placeholder); comment edits and deletes; assignee and reporter; changing the issue type of an already-linked issue. Deleting a synced issue doesn't delete the partner's copy; it unlinks it (see [Reliability](#reliability-queue-retries-idempotency)). See [Limitations and roadmap](#limitations-and-roadmap).

**Who appears as the author?** Issues and comments created by the connector show the app, **Cloud Connector for Jira**, as reporter or author. The original author's name is kept in the comment's attribution line.

---

## Installation

### Prerequisites

| Requirement | Notes |
|---|---|
| Jira Cloud admin rights | On every site that installs the app |
| Node.js **22.23.4 or newer** | Required by Forge CLI 15. With nvm-windows: `nvm install 22.23.4` and `nvm use 22.23.4` |
| Forge CLI | `npm install -g @forge/cli`, then `forge login` |

### Register and deploy

1. Install the dependencies:

   ```bash
   npm install
   ```

2. Register the app once. This writes the app id into `manifest.yml`. (Already done for this repository; only needed for a fresh copy under another Atlassian account.)

   ```bash
   forge register
   ```

3. Deploy:

   ```bash
   forge deploy -e production
   ```

   Use `-e development` for testing. Development builds show a **DEV** tag in Jira and are meant for test sites.

### Install on your own site

```bash
forge install --site your-site.atlassian.net --product jira --environment production
```

The installer asks you to approve these permissions:

| Permission | Why |
|---|---|
| `read:jira-work` | Read issues, comments, fields, issue types and statuses of the synced project |
| `write:jira-work` | Create and update issues, add comments, move issues between statuses, add web links |
| `read:servicedesk-request` | List a Service Management project's request types, so incoming issues get one |
| `storage:app` | Store connections, settings, issue links and the activity log |
| Outbound calls to `*.hello.atlassian-dev.net` | Send messages to the partner's web trigger (Forge-hosted) |

### Install on a partner's site

1. In the [developer console](https://developer.atlassian.com/console/myapps/), open **Cloud Connector for Jira**, go to **Distribution** and enable sharing.
2. Send the installation link to the partner's Jira admin. They install the app on their own site and approve the same permissions.
3. The two sites don't need the same app version. Forge applies minor updates automatically, but a version that needs new permissions waits until each site's admin approves it (**Apps › Manage apps**). Until then the sites sync at the newer protocol both understand, and a newer feature (for example attachments) waits for the older side. See [Protocol versions](#protocol-versions).

### Where to find the app

The connector is an admin page: **⚙ Settings → Jira settings → Apps**, then **Cloud Connector** in the left sidebar. It is not listed in the top **Apps** menu or under **Manage apps**.

---

## Pairing two sites

Pairing connects one *connection* on site A with one *connection* on site B.

1. **Site A:** on the Cloud Connector page click **New connection**, name it after the partner (for example "Acme Corp") and click **Create**.
2. **Site A:** on the **Connection** tab click **Generate pairing code**. Copy the code.
3. Send the code **privately** to site B's admin, for example in a direct message. It contains a shared secret.
4. **Site B:** create a connection for site A the same way, paste the code into **Or paste the code you received** and click **Pair with this code**.
5. Both connections now show **Paired**. Click **Test connection** on either side; it should report "Peer … answered."

About pairing codes:

- A code is valid for **24 hours**. The page shows when the pending code expires.
- **Cancel pairing** revokes a pending code immediately. **Generate new code** replaces it, and the old one stops working.
- **Disconnect** first tells the partner (its connection shows **Disconnected by partner**), then forgets the secret. Anything the partner still sends is refused (HTTP 403) until the connections are paired again. Issue links are kept on both sides, so re-pairing resumes syncing the same issues. Deleting a connection, and uninstalling the app, tell the partner the same way (best effort: a partner before protocol v6, or one that can't be reached, isn't told).

---

## Configuration guide

Each admin configures **their own side** of a connection. The two sides agree on a small set of **shared keys** (for example `task`, `bug`, `severity`) and each side maps those keys to its own Jira objects. Messages only carry shared keys and plain values, so neither site needs to know the other's ids.

Every settings tab has a **Save settings** button. An **Unsaved changes** label shows until you save, and switching tabs keeps unsaved edits. Saving validates against Jira, refuses invalid settings with an error, and saves settings with minor problems while listing them as warnings.

### Connection tab

- Pairing (see above), **Test connection** and **Disconnect**.
- **Check contract** asks the partner which shared keys it has mapped and shows two tables:
  - **Issue types:** each shared key, with *mapped* or *missing* for this site and for the partner.
  - **Fields:** the same, plus a **Values flow** column: *both ways*, *to peer*, *from peer* or *none*.
  - **Attachments:** whether each site has attachments switched on and its size limit, with *files sync* only when both are on.
  - Keys that are missing on one side, or fields where neither side sends what the other receives, don't sync. A warning above the tables counts them.
- **Sync existing issues** (initial sync), see below.

### Sync existing issues

New and changed issues sync on their own; issues that existed before the connection don't. To bring them over:

1. On the **Connection** tab, under **Sync existing issues**, enter JQL to choose them, e.g. `status != Done AND created >= -90d`. It is always limited to the connection's project (`project = "KEY" AND (your JQL)`), oldest first; your own `ORDER BY` is ignored. Leave it empty for all issues of the project.
2. **Count issues** shows roughly how many match and checks the JQL.
3. **Start initial sync** asks for confirmation, then works through the matches in the background, 50 per page. **Refresh** updates the progress; **Cancel** stops it after the current page (issues already queued still sync).

Each matching issue that isn't linked yet is created on the partner site, with its files (if attachments are on) and its public comments. The usual rules apply: only mapped issue types, only issues with the sync label if one is set, never issues with a security level, never restricted comments. Issues that are already linked are counted and left alone; use **Resync now** in the issue panel for those.

### Project tab

| Setting | Meaning |
|---|---|
| Project key | The project on this site that this connection syncs. A project can belong to only one connection. |
| Sync label | Optional. Only issues with this label are sent. Leave empty to send every issue of a mapped type. Incoming issues get the label too, and mapped label updates never remove it. |
| Sync description | Send and accept description changes |
| Sync status | Send and accept status changes |
| Sync comments | Send and accept new comments |
| Add incoming comments as internal notes | Jira Service Management projects only, on by default. Keeps the partner's comments off your customer portal. |
| Sync attachments | **Off by default.** Copies files added to synced issues to the partner and accepts the partner's files. Files only flow when both sites switch it on. |
| Largest file to send or accept | 1–25 MB, default 10 MB. Applies in both directions: larger files aren't sent, and larger incoming files are refused. Skipped files are listed under **Activity**. |

### Issue types tab

Map each shared key to an issue type of your project. Each row has a **Sync** checkbox; unticked types don't sync.

| Shared key | Type here (Software) | Type at partner (Service Management) |
|---|---|---|
| `task` | Task | [System] Service request |
| `bug` | Bug | [System] Incident |

- Choosing a type suggests a key (for example "Service Request" → `service-request`), which you can change. Both sides must use the same key.
- Each issue type can be mapped only once.
- **Request type for incoming issues** (Service Management projects only): the request type that issues created from the partner get, chosen per issue type. Without one, such issues are missing from queues and the customer portal, and **Activity** warns about each one. Issues created through the fallback type get no request type.
- **Incoming issues whose shared key is not mapped here:** pick a fallback type, or "None" to skip them. Skipped issues are listed under **Activity** on the receiving site.

### Fields tab

1. **Add field** and pick a field on this site. The list comes from the create screens of your mapped issue types and shows each field's type, for example "Severity · Select".
2. Set the **shared key**, for example `severity`.
3. Set the **direction**:

   | Direction | This site sends changes | This site accepts changes |
   |---|---|---|
   | ⇄ Both ways | yes | yes |
   | → Send only | yes | no |
   | ← Receive only | no | yes |

4. For select-type fields (select, radio, multi-select, checkboxes, priority), click **Values** to map options whose names differ between the sites. The dialog lists this site's options; enter the shared value for each. An empty box means "same name".

   Example: your priority has no "Highest", the partner's does. On your side map the option **Critical** to the shared value `Highest`, and the partner's "Highest" arrives as Critical.

5. Under **Fields per issue type**, choose which fields sync for each issue type. Fields not chosen there are left alone.

How values are handled:

- **Unknown values are skipped, not fatal.** An incoming option with no match, or a field that isn't on the issue's screen, is skipped with a warning under **Activity**. The rest of the update still applies. For multi-selects, matching options are kept and only unknown ones are dropped.
- New issues receive every mapped field that has a value. Updates send only the mapped fields that actually changed.
- Labels can't contain spaces in Jira, so spaces in incoming labels become underscores.
- **Labels sync as changes.** A new issue gets the partner's full set; after that only the labels added or removed are applied, so labels that exist only on your side stay. Your own sync label is never removed by the partner. With a partner on a connector before protocol v5, updates fall back to replacing the whole set.
- A field must be on the **edit** screen to receive updates on existing issues.
- Summary, description, status, issue type, assignee, reporter and comments are handled by the connector itself and can't be added on this tab.

### Statuses tab

The status map says how the **partner's** statuses map to **this site's** statuses, one per line:

```
Waiting for customer = In Review
Resolved = Done
Closed = Done
```

- Each site maps only its incoming direction; the partner's admin maps the other direction on their site.
- Several partner statuses can point to the same status here. Names are not case-sensitive.
- Partner statuses that aren't listed move to a status with the same name, if there is one.
- **Load partner statuses** fetches the partner's status names, and **Add N unmapped to the map** adds a `Name = Name` line for each one, ready to edit. The tab also lists your own project's statuses.
- An issue only moves if your workflow has a transition from its current status to the target. Otherwise its status stays and a warning appears under **Activity**. Transitions that need a screen filled in can't be completed automatically.
- **New issues keep their own starting status.** When a partner issue is still in a "to do" category status (for example "To Do" or "Backlog"), the new issue isn't moved, unless that status is listed in the map. Later status changes always sync.

### Issue panel

Every issue of a connected project has a **Cloud Connector** panel in the issue view (the *Partner sync* glance on the right). It shows:

- the connection and partner site,
- the linked partner issue, as a link,
- or why the issue doesn't sync (issue type not mapped, sync label missing, security level), or that it will be created with its next change,
- the last sync of this issue: sent or received, when, and what, or the error if the last attempt failed.

Users who may edit the issue also get a button:

- **Sync now** on an issue that isn't on the partner site yet creates it there, with files and comments.
- **Resync now** on a linked issue sends its summary, description, status, mapped fields, files and public comments again. Labels are only added, never removed, and comments the partner already has are skipped. Use it after fixing a mapping, or when an update failed.

### Activity tab

The last 30 events for this connection (kept for 30 days): issues created and updated, comments sent and received, pairing changes, and warnings such as skipped values or missing transitions. See [Troubleshooting](#troubleshooting) for what they mean.

Above the log, **Failed changes** lists issues with a change the partner never received, with the last error. **Retry** (or **Retry all**) resyncs the whole issue; a record disappears once a resync succeeds.

---

## Several partners and isolation

A site can have one connection per partner. Use the **Partner connection** picker at the top of the page to switch between them, and **New connection**, **Rename** and **Delete** to manage them.

Each connection has its own pairing secret, project, issue type/field/status mappings, issue links and activity log, stored under the connection's own key prefix. Isolation is enforced in code:

- **One project, one partner.** Saving a project that another connection already uses is refused ("Project X is already used by connection Y"). Otherwise one partner's comments and updates would reach the other through shared issues.
- **Outgoing:** a change is sent only to the connection that owns the issue's *current* project. An issue moved out of that project stops syncing to its former partner, even if the change was queued before the move.
- **Incoming:** every message names the receiving connection and must be signed with *that* connection's secret. It can only create issues in that connection's project, and only update or comment on issues linked under that connection that are still in its project. Unknown and unpaired connections get the same refusal, so ids can't be probed.
- **Partners never see each other.** Each pairing has its own secret, and partner A never receives partner B's address or key.

**Deleting a connection** removes its pairing, settings, issue links and log. Jira issues are not deleted on either site, but they stop syncing.

**Upgrading from a single-partner version:** an existing pairing is migrated automatically into a connection named **Partner 1**, with its settings and issue links. Older partners that don't send a connection id are routed to that connection.

---

## Security and data flow

### What leaves your site

Only the content of issues in a connection's project, of mapped issue types (and with the sync label, if set):

- summary, and the description if enabled
- the shared issue type key, and the status name if enabled
- values of mapped fields that this site *sends*
- new comments, if enabled, with the author's display name
- attached files, only if **Sync attachments** is switched on, and only up to the size limit
- the issue key and a browse link, used for the web link on the partner's issue

It goes **only** to the partner site paired with that connection, through that site's Forge web trigger. There are no third-party servers.

Anything Jira restricts on your site stays there: comments limited to a group or role, Service Management internal notes, and issues protected by an issue security level (with their comments and files) are never sent.

### Protection

| Mechanism | Details |
|---|---|
| No shared credentials | Each site writes only to its own Jira as the app (`api.asApp()`). No API tokens or user accounts cross sites. |
| Signed messages | Every message is signed with HMAC-SHA256 over `timestamp.body` using a 256-bit secret unique to the pairing. Bad signatures get HTTP 401. |
| Replay window | Messages with a timestamp more than 5 minutes off are rejected. |
| Secret storage | Pairing secrets are kept in Forge's encrypted secret storage (`kvs.setSecret`) and never shown again after pairing. |
| Pairing codes | Expire after 24 hours, can be cancelled, are replaced by a new code, and can be used once. |
| Disconnect | Tells the partner, then deletes the secret. Further messages are refused with HTTP 403. |
| Admin actions | Every admin-page action checks that the caller is a Jira administrator (the `ADMINISTER` permission), not only that Jira showed them the page. |
| Issue panel | The panel checks, as the user, that they may view the issue (`BROWSE_PROJECTS`) before the app reads it; **Sync now** requires edit permission (`EDIT_ISSUES`). |
| No personal data in logs | Forge's operational logs get issue keys, ids and error messages only; people's names and file names appear only in the admin's Activity tab, which is app storage. |
| Per-connection isolation | See [Several partners and isolation](#several-partners-and-isolation) |
| No echo loops | Changes made by the app don't fire its own triggers (`ignoreSelf`). |

### Runs on Atlassian

The connector is **not eligible** for Atlassian's "Runs on Atlassian" program. `forge eligibility` reports:

```
- App is egressing data
- App is using a webtrigger module that can egress data
```

The program requires that customer data never leaves the site the app is installed on. Sending issues to a partner's site is that transfer, even though the partner site also runs on Atlassian's cloud. It is the purpose of the app, so this can't be changed without removing the sync.

---

## Reliability: queue, retries, idempotency

- **Queued work.** Triggers only enqueue jobs; the worker does the slow part, with up to 900 seconds per job (room for large files). One job at a time per issue keeps creates and updates for the same issue in order.
- **Retries.** If the partner is unreachable, rate-limited (HTTP 429), not ready yet (409, for example an update that arrives before the issue's create) or failing (5xx), or Jira itself rate-limits or fails, the job is queued again with a delay of 30s, 60s, 120s, 240s and 480s, so up to **5 retries** over about 15 minutes. After that the change is listed under **Activity › Failed changes** (see below).
- **Permanent failures aren't retried.** If the partner rejects a message for a reason that won't fix itself (for example an unmapped issue type, or Jira refusing a field), the receiver answers 422 and the sender logs it once.
- **Failed changes are kept, not dropped.** A change that still fails after its retries, or fails for good, is recorded for its issue and listed under **Activity › Failed changes** with **Retry**. Retrying resyncs the whole issue (fields, status, files, public comments), so it also covers earlier changes that were lost. Failures that may pass on their own (the partner was down, rate limits, 5xx) are retried automatically **every hour, for up to a day** (scheduled trigger `retry-failures`).
- **Deleted issues.** When a synced issue is deleted, its records here are removed and the partner is told (protocol v6). The partner's copy is not deleted: it is unlinked, gets a comment saying why, and stays unlinked, so editing it never creates a new copy here.
- **Storage stays bounded.** Activity entries expire after 30 days, processed-message ids after 2 days and attachment chunks after 1 day; a deleted issue's links, state, failure record and file records are removed with it. Connections, routes and log entries each have their own key, so simultaneous writes (two syncs logging at once, two admins saving) can't overwrite each other.
- **Idempotency.** Each message has an id. The receiver remembers processed ids for 2 days and answers repeats from cache. Creates check the issue link first, and comments check the comment link, so a retried message never creates duplicates.
- **Attachments travel in chunks.** A file is downloaded on the sending site and sent in 150 KB chunks (about 200 KB as base64), because the receiving site keeps chunks in Forge storage between requests and a stored value may not exceed 240 KiB. The last chunk makes the receiver assemble the file, check its size and upload it. Before sending, the sender asks which chunks the receiver already has, so an interrupted transfer resumes instead of starting over. Short bursts of rate limiting are absorbed by retrying a chunk up to 4 times with 2–16 s pauses; after that the whole job is retried later. Each file is sent once: the sender remembers sent and skipped files, and the receiver remembers uploaded ones. Files uploaded by the connector itself are never sent on, because they came from a partner; otherwise a copy would bounce back to the site it came from.
- **Nothing is lost silently.** If a change can't even be queued, an error with the issue key is logged under **Activity** (or, when the event didn't say which project it belongs to, in the backend log).

---

## Troubleshooting

### Common Activity messages

| Message | Meaning and fix |
|---|---|
| `No transition to "X" on KEY (partner status "Y")` | Your workflow can't move the issue to X from its current status. Add a line for Y on the **Statuses** tab, or add the transition to your workflow. |
| `KEY: skipped priority: "Highest" has no matching option here.` | The partner sent a value your field doesn't have. Map it under **Fields → Values**. |
| `KEY: skipped severity: Severity is not on this issue type's screen` | Add the field to the issue type's create or edit screen, or remove it from **Fields per issue type**. |
| `KEY has no request type, so it is missing from queues and the portal.` | Choose a request type for that issue type on the **Issue types** tab. |
| `Skipped incoming issue: issue type "x" is not mapped on this site.` | Map the shared key `x` on the **Issue types** tab, or set a fallback type. |
| `Sync of issue N failed: Peer rejected … (403): Unknown or unpaired connection.` | The partner disconnected or deleted its connection. Pair again. |
| `Partner's connector speaks protocol v4 (this site v5)` | The partner hasn't approved an app update yet. Sync continues; newer features are held back. The Connection tab says which. |
| `Not sending files of …: the partner's connector (protocol v3) can't receive attachments` | As above: files go across with the next change to the issue once the partner has updated. |
| `The two connectors share no protocol version …` | One side is too old for the other. Update the installation the message names. |
| `Could not queue issue sync for KEY: …` | Forge refused the queue write. Edit the issue again to retry, and check `forge logs`. |
| `Pairing code expired and was discarded.` | The pending code was older than 24 hours. Generate a new one. |
| `Skipped report.pdf on KEY: 12.3 MB is over the 10 MB limit.` | Raise **Largest file to send or accept** on the Project tab (up to 25 MB), on both sites. Already-skipped files aren't retried. |
| `Skipped report.pdf on KEY: Peer rejected attachment.chunk (422): Attachments are switched off on the receiving site.` | The partner hasn't switched attachments on. **Check contract** shows both sides' settings. |
| `Sent report.pdf (2.4 MB, 17 chunks) from KEY.` / `Attached report.pdf … to KEY` | A file was transferred (sender / receiver). |
| `Not sending comment N on KEY: it is an internal note.` / `… visible only to role "X"` | Working as intended: restricted comments stay on your site. Post a public comment to share something with the partner. |
| `Not syncing KEY: it has the security level "X".` | Working as intended: issues with a security level never leave your site. Remove the level to sync the issue. |

### Admin page errors

Forge prefixes errors on the admin page with "There was an error invoking the function -". The text after the dash is the actual reason, for example:

- `Project X is already used by connection "Y"`: each project can belong to one connection only.
- `Field "…" is not on the create screen of any mapped issue type`: add the field to a create screen, or pick another field.
- `This pairing code has expired`: ask the other admin for a new code.

### Other checks

- **Nothing syncs:** both connections paired? **Check contract** all green? Is the issue's type mapped and ticked, and does it carry the sync label (if one is set)?
- **Status doesn't move:** see the transition message above. Jira only allows moves the workflow permits.
- **A partner's comment is visible to customers:** turn on **Add incoming comments as internal notes** (Service Management projects only).
- **Backend details:** run `forge logs -e production --since 1h`. Each log line shows the connection id in brackets.

---

## Development

### Project layout

```
manifest.yml               Forge modules, scopes, egress, runtime
src/index.js               function handler exports
src/protocol.js            message envelope, HMAC signing, pairing codes, connection routing, version negotiation (pure)
src/config.js              settings defaults and validation, shared-key contract, status map (pure)
src/fields.js              field kinds, value conversion to and from the wire format (pure)
src/attachments.js         attachment chunking, reassembly, multipart body, size limits (pure)
src/privacy.js             what never leaves the site: restricted comments, security levels (pure)
src/adf.js                 Atlassian Document Format sanitising and attribution (pure)
src/store.js               Forge KVS: connections, routing, secrets, settings, links, activity, migration
src/jira.js                Jira REST helpers (asApp): issues, metadata, transitions, comments, links
src/sync/triggers.js       Jira event triggers → queue (routed by project)
src/sync/queue.js          queue push with per-issue ordering
src/sync/worker.js         queue consumer → partner, with delayed retries
src/sync/peerClient.js     signed HTTP calls to the partner's web trigger
src/sync/inbound.js        web trigger receiving partner messages
src/admin/resolver.js      admin page backend (connections, pairing, settings, contract check, initial sync)
src/issue/resolver.js      issue panel backend (sync status, Sync now)
src/frontend/index.jsx     admin page (Forge UI Kit)
src/frontend/issue.jsx     issue panel (Forge UI Kit)
test/                      unit tests for the pure modules
assets/avatar/             app icon (SVG source and PNG renders)
```

### Commands

| Command | What it does |
|---|---|
| `npm install` | Install dependencies |
| `npm test` | Unit tests for protocol and version negotiation, settings, field conversion, privacy rules and ADF handling |
| `forge lint` | Validate the manifest and code |
| `forge deploy -e development` | Deploy the development build (takes several minutes) |
| `forge install --site <site> --product jira -e development` | Install the development build on a test site |
| `forge tunnel` | Run local code against a development installation |
| `forge logs -e development --since 30m` | Backend logs |
| `forge eligibility -e development` | Runs on Atlassian check |

### Testing end to end

You need two installations. A free Forge demo site works for one side (`forge site provision`, one per Atlassian account). With a single site, create **two connections on the same site** and pair them with each other, each with its own project. That exercises pairing, routing, isolation and mappings exactly as between two sites.

Test checklist used for this app:

| Scenario | How |
|---|---|
| Create both ways | Create an issue of a mapped type on each side |
| Field, label and value-map sync | Edit mapped fields; map a differently named option |
| Status | Move an issue; check the partner follows |
| Comments | Comment on each side; check attribution and no duplicates |
| Attachments | Attach a small and a multi-chunk file; compare checksums on both sides; attach a file over the limit and check it is skipped |
| Unmapped type | Create an unmapped type; nothing should sync |
| Isolation | With two connections, each project syncs only to its own partner |
| JSM internal note | Comment towards a Service Management project; check the lock icon |
| Disconnect | Disconnect one side; the other's Test connection gets 403 |
| Retry | Fault injection, below |

### Testing the retry path

A development-only switch makes the receiving site answer HTTP 429 to the first message about an issue whose summary contains `[simulate-429]`, so the sender has to retry:

```bash
forge variables set -e development CONNECTOR_FAULT_INJECTION true
```

Deploy, create an issue with `[simulate-429]` in its summary, and check `forge logs` for "Retrying … in 30s" followed by the sync. Afterwards remove the variable and deploy again. **Never set it in production.**

```bash
forge variables unset -e development CONNECTOR_FAULT_INJECTION
```

### Testing an older partner

A second development-only variable makes one connection behave like an older connector, on both its sending and its receiving side. The value is a comma-separated list of `connectionId:version` (the id is in the connection's storage keys and in `forge logs` lines such as `[c1a2b3c4d5e]`):

```bash
forge variables set -e development CONNECTOR_SIMULATE_PROTOCOL c1a2b3c4d5e:3
```

Deploy, then on the partner connection: **Test connection** shows the "older connector" warning, issues and comments still sync, and a file added to an issue is held back with "Not sending files of …". Remove the variable and deploy again; the next change to that issue sends the file. **Never set it in production.**

```bash
forge variables unset -e development CONNECTOR_SIMULATE_PROTOCOL
```

### Forge pitfalls (learned the hard way)

- **Don't set `"type": "module"` in `package.json`.** The Forge bundler then uses strict ES-module interop, and default imports of the CommonJS `@forge/*` packages break ("… is not a constructor").
- **Don't return `InvocationError` from the queue consumer.** With a plain function consumer it crashed the Forge runtime, and switching to a resolver-based consumer made every `queue.push` fail with HTTP 400. Retries are done by re-queuing with `delayInSeconds` instead.
- **Queue pushes with a concurrency key** were once rejected with a bare 400. `enqueue` falls back to pushing without the key.
- **Forge variables** only take effect after the next `forge deploy`.
- Repositories inside OneDrive make `npm install` and `forge deploy` slow (10–20 minutes for packaging).

---

## Reference

### Protocol versions

Every message carries the protocol version it is written in (`v`). The two sites can run different app versions, because a partner's admin approves updates that need new permissions on their own schedule. Negotiation works like this:

1. Each site accepts messages from its oldest supported version (`MIN_PROTOCOL_VERSION`, now 3) up to its own (`PROTOCOL_VERSION`, now 6), and names both in the `x-connector-protocol` header of every answer to an authenticated sender, e.g. `6;min=3`.
2. The sender remembers what the partner advertised, with the pairing, and writes every later message at the newest version both speak. A partner stored as older is asked again with a `ping` before a message relies on that, at most once a minute, so an update on its side takes effect with the next message.
3. Before it knows, the sender uses its own version. A partner that can't read it answers 426 with its versions; the sender stores them and sends the same message once more at the agreed version. A partner from before negotiation sends no header, so its versions are read from the 426 text instead.
4. A message type or feature the agreed version doesn't have is not used: below v4 files are held back, below v5 labels are sent as a full set. Files are held back, not marked as skipped, so they go across after the partner updates. The Connection tab shows the partner's version and what is held back.
5. If the sites share no version at all, nothing syncs, and the activity log and Connection tab say which site to update.

| Version | Change |
|---|---|
| 2 | Issue types travel as shared keys instead of local names |
| 3 | Issues carry mapped field values (`mapped`) — **oldest accepted** |
| 4 | Attachments travel in chunks (`attachment.status`, `attachment.chunk`) |
| 5 | Label updates travel as changes (`labelChanges: { sharedKey: { add, remove } }` in `issue.updated`) |
| 6 | Notices: `connection.closed` (with `reason`: disconnected, deleted or uninstalled) and `issue.deleted` — **current** |

When adding a version: raise `PROTOCOL_VERSION`; register new message types in `TYPE_VERSIONS` and new features in `FEATURE_VERSIONS`; keep reading older payloads; and raise `MIN_PROTOCOL_VERSION` only when it is safe to stop talking to installations that old.

Pairing codes have their own version: v2 adds the issuing connection's id.

### Message envelope

```json
{
  "v": 5,
  "id": "6f1c…",
  "type": "issue.updated",
  "sentAt": "2026-10-27T17:34:42.000Z",
  "payload": { "source": { "id": "10042", "key": "ENG-4" }, "fields": {}, "statusName": null, "mapped": { "priority": "Highest" } }
}
```

| Type | Purpose | Answer |
|---|---|---|
| `pair.complete` | Second admin finishes pairing; carries its inbound URL and connection id | `{ ok, siteUrl }` |
| `ping` | Test connection | `{ ok, siteUrl }` |
| `contract.describe` | Check contract and partner statuses | `{ contract, statuses }` |
| `issue.created` | New issue with summary, description, type key, status, mapped fields | `{ issue: { id, key, url } }` |
| `issue.updated` | Changed summary, description, status, mapped fields | `{ ok }` |
| `comment.created` | New comment with author name and body | `{ ok, commentId }` |
| `attachment.status` | Is this file already here, and which chunks are stored? | `{ done, attachmentId }` or `{ done: false, have: [indices] }` |
| `attachment.chunk` | One 150 KB chunk (base64) with file name, type, size, index and total; the last one triggers upload | `{ stored }` or `{ done, attachmentId }` |
| `connection.closed` | The partner disconnected, deleted the connection or uninstalled the app (`reason`) | `{ ok }` |
| `issue.deleted` | A synced issue was deleted on the partner site; the linked issue here is unlinked | `{ ok }` |

### HTTP headers

| Header | Content |
|---|---|
| `x-connector-timestamp` | Milliseconds since epoch; must be within 5 minutes |
| `x-connector-signature` | Hex HMAC-SHA256 of `timestamp.body` |
| `x-connector-connection` | The receiving connection's id |
| `x-connector-protocol` | On answers only: the receiver's protocol versions, `version;min=oldest` |

### Web trigger responses

| Status | Meaning | Sender retries? |
|---|---|---|
| 200 | Processed (or answered from cache for a repeated message) | — |
| 400 | Malformed message | no |
| 401 | Invalid or expired signature | no |
| 403 | Unknown or unpaired connection, or the issue left the connection's project | no |
| 405 | Not a POST | no |
| 409 | Not ready yet (issue not linked yet, or no pairing pending) | yes |
| 412 | The receiving connection has no project | no |
| 422 | Permanent rejection (unmapped type, Jira refused the write) | no |
| 426 | The message's version is outside the receiver's range; the answer names the range | once, at the agreed version |
| 429 / 5xx | Rate limited or temporary error | yes |

### Pairing code

A base64url-encoded JSON object:

```json
{ "v": 2, "url": "<issuer's web trigger URL>", "secret": "<256-bit secret>", "siteUrl": "https://a.atlassian.net", "expiresAt": "<ISO time>", "connectionId": "c1a2b3c4d5" }
```

### Storage layout (Forge KVS, per site)

| Key | Content |
|---|---|
| `connection:<id>` | `{ id, name, createdAt, legacy? }` |
| `route:<PROJECTKEY>` | `{ connectionId }`: the connection that owns the project |
| `site` | This site's URL |
| `app-account` | The app user's account id, learned from the first comment or file it adds; its comments and files are never sent on |
| `schema` | Storage layout version (3). Older layouts are migrated automatically on first use. |
| `conn:<id>:peer` *(secret)* | Pairing state, partner URL, secret, partner connection id |
| `conn:<id>:config` | Settings of the connection |
| `conn:<id>:link:local:<issueId>` / `link:remote:<partnerIssueId>` | Issue links in both directions; a local link marked `unlinked` after the partner's issue was deleted |
| `conn:<id>:comment:remote:<commentId>` | Comment links (duplicate protection) |
| `conn:<id>:state:<issueId>` | Last sync of the issue, for the issue panel |
| `conn:<id>:backfill` | Progress of the last initial sync |
| `conn:<id>:failed:<issueId>` | A change not delivered: last error, whether it is retried hourly, how often it was |
| `conn:<id>:att:out:<issueId>:<attachmentId>` | Sender: file sent, skipped (and why), or received from the partner (never sent back). Older records without the issue id are still read. |
| `conn:<id>:att:in:<partnerAttachmentId>` | Receiver: file uploaded here |
| `conn:<id>:att:chunk:<partnerAttachmentId>:<n>` | Receiver: chunks waiting for assembly, kept 1 day |
| `conn:<id>:msg:<messageId>` | Processed messages, kept 2 days |
| `conn:<id>:log:<newest-first key>` | One activity entry, kept 30 days; the page shows the latest 30 |

### Manifest modules

| Module | Key | Purpose |
|---|---|---|
| `jira:adminPage` | `connector-admin` | The Cloud Connector page |
| `jira:issueContext` | `connector-issue` | The issue panel (sync status, Sync now) |
| `trigger` | `issue-events`, `comment-events`, `attachment-events` | Issue created/updated, comment added, file attached |
| `trigger` | `issue-deleted-events`, `uninstall-events` | Issue deleted; app uninstalled (tell the partner) |
| `scheduledTrigger` | `retry-failures` | Hourly retry of failed changes |
| `consumer` | `sync-consumer` | Queue worker (`sync-queue`), up to 900 s per job for large files |
| `webtrigger` | `inbound` | Receives partner messages |

---

## Limitations and roadmap

| Area | Status |
|---|---|
| User fields, assignee, reporter | Not synced. Account ids and emails often aren't visible across organizations; needs a user map. |
| Cascading select, version, component fields | Not supported yet |
| Attachments | Synced when switched on, up to 25 MB per file. Deleting a file doesn't sync, and images embedded in descriptions still show "[attachment not synced]". |
| Comment edits and deletes | Not synced; only new comments |
| Issue type change on linked issues | Not synced |
| Issue deletion | Unlinks the partner's copy (with a comment) instead of deleting it |
| Status map per issue type | One map per connection; workflows that differ by issue type may need per-type maps |
| Narrow windows | The Fields table is cramped at small widths |

---

## FAQ

**Can I connect more than one customer?**
Yes. Create one connection per customer, each with its own project. See [Several partners and isolation](#several-partners-and-isolation).

**Can two partners share one project?**
No. That would let one partner's comments and updates reach the other, so it's refused on save.

**Do partners see each other?**
No. Each pairing has its own secret and address, and a partner only receives issues from its own connection's project.

**Does the partner need an account on my site?**
No. Each side only writes to its own site, as the app.

**Why are Summary and Description not on the Fields tab?**
The connector handles them itself: summary always syncs, description is a switch on the Project tab.

**Can I rename the app user?**
The name comes from the app name in the developer console. Renaming it there updated the reporter shown on existing installations.

**Is the app eligible for Runs on Atlassian?**
No; see [Runs on Atlassian](#runs-on-atlassian).

**What happens if the partner site is down?**
Changes are retried with increasing delays for about 15 minutes. Anything still undelivered is listed under **Activity › Failed changes**, retried automatically every hour for up to a day, and can be retried by hand with **Retry**.

---

[Support SLA](./sla.html) · [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
