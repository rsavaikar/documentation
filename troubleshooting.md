---
layout: default
title: Troubleshooting
---
Oct 28, 2026 · @Rahul Savaikar

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [Support](./support.html)

# **Troubleshooting** 

Most problems show up as a message on the connection's **Activity** tab. Find the message below. 

|Message|What to do|
|---|---|
|No transition to "X" on KEY|Your workflow can't move the issue to X from its<br>current status. Add a line on the**Statuses**tab or add<br>the transition to your workflow.|
|skipped priority: "Highest" has no matching<br>option here|The partner sent a value your field doesn't have.<br>Map it under**Fields → Values**.|
|skipped severity: Severity is not on this issue<br>type's screen|Add the field to the issue type's create or edit<br>screen, or remove it from**Fields per issue type**.|
|KEY has no request type|Choose a request type for that issue type on the<br>**Issue types**tab.|
|Skipped incoming issue: issue type "x" is not<br>mapped|Map the shared key on the**Issue types**tab, or set a<br>fallback type.|
|Peer rejected … (403): Unknown or unpaired<br>connection|The partner disconnected or deleted its connection.<br>Pair again.|
|The partner disconnected this connection|Nothing syncs until you pair again; issue links are<br>kept.|
|Partner's connector speaks protocol vN (this<br>site vM)|The two sites run different app versions. Sync<br>continues; newer features wait until both have<br>updated.|
|Skipped file.pdf: over the 10 MB limit|Raise**Largest file to send or accept**on the Project<br>tab, on both sites.|
|Attachments are switched off on the receiving<br>site|The partner hasn't switched attachments on;**Check**<br>**contract**shows both settings.|
|Not sending comment …: it is an internal note|Working as intended: restricted comments stay on<br>your site.|
|Not syncing KEY: it has the security level "X"|Working as intended: issues with a security level<br>never leave your site.|
|Pairing code expired|Codes last 24 hours. Generate a new one.|



**Nothing syncs at all?** Check that both connections show **Paired** , that **Check contract** shows no missing keys, and that the issue's type is mapped and ticked (and carries the sync label, if you set one). 

**A change didn't arrive?** Look under **Activity → Failed changes** and click **Retry** , or use **Resync now** in the 

issue's panel.

See also: [Privacy policy](./privacy-policy.html) · [End-user terms](./end-user-terms.html)
