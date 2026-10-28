---
layout: default
redirect_from: /troubleshooting.html
title: Troubleshooting
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [For Jira users](./for-jira-users.html) · [Troubleshooting](./troubleshooting.html) · [Support](./support.html)

# Troubleshooting

*Last updated: 28 October 2026 · Rahul Savaikar*

Most problems show up as a message on the connection's **Activity** tab. Find the message below.

| Message | What to do |
| --- | --- |
| No transition to "X" on KEY | Your workflow can't move the issue to X from its current status. Add a line on the **Statuses** tab or add the transition to your workflow. |
| skipped priority: "Highest" has no matching option here | The partner sent a value your field doesn't have. Map it under **Fields → Values**. |
| skipped severity: Severity is not on this issue type's screen | Add the field to the issue type's create or edit screen, or remove it from **Fields per issue type**. |
| KEY has no request type | Choose a request type for that issue type on the **Issue types** tab. |
| Skipped incoming issue: issue type "x" is not mapped | Map the shared key on the **Issue types** tab, or set a fallback type. |
| Peer rejected … (403): Unknown or unpaired connection | The partner disconnected or deleted its connection. Pair again. |
| The partner disconnected this connection | Nothing syncs until you pair again; issue links are kept. |
| Partner's connector speaks protocol vN (this site vM) | The two sites run different app versions. Sync continues; newer features wait until both have updated. |
| Skipped file.pdf: over the 10 MB limit | Raise **Largest file to send or accept** on the **Project** tab, on both sites. |
| Attachments are switched off on the receiving site | The partner hasn't switched attachments on; **Check contract** shows both settings. |
| Not sending comment …: it is an internal note | Working as intended: restricted comments stay on your site. |
| Not syncing KEY: it has the security level "X" | Working as intended: issues with a security level never leave your site. |
| Pairing code expired | Codes last 24 hours. Generate a new one. |

**Nothing syncs at all?** Check that both connections show **Paired**, that **Check contract** shows no missing keys, and that the issue's type is mapped and ticked (and carries the sync label, if you set one).

**A change didn't arrive?** Look under **Activity → Failed changes** and click **Retry**, or use **Resync now** in the issue's panel.

**Still stuck?** [Contact support](./support.html) with the exact message from the Activity tab.

---

[Support SLA](../sla.html) · [Privacy policy](../privacy-policy.html#cloud-connector-for-jira) · [End-user terms](./end-user-terms.html)
