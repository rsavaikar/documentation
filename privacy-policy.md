---
layout: default
title: Privacy policy
---

[Home](./) · [Product guide](./product-guide.html) · [Admin guide](./admin-guide.html) · [Support](./support.html)

# **Privacy policy – Jira Cloud Connector** 

Jira Cloud Connector ("the app") is a free app for Atlassian Jira Cloud, built on Atlassian Forge and published by Rahul Savaikar ("we"). It keeps issues in a Jira project on your site in sync with a project on a partner's Jira site that your administrator pairs it with. This policy explains what data the app handles, where it goes and how long it is kept. 

## **Summary** 

- The app runs entirely on Atlassian's Forge platform. We operate no servers and use no thirdparty services. 

- Issue content is sent only to the partner site your administrator paired with, and only from projects your administrator connected. 

- We do not sell data, show ads, track users or use cookies. 

## **Data the app processes** 

|Data|Purpose|Where it goes|Kept for|
|---|---|---|---|
|Issue content of connected<br>projects: summary, description,<br>status, issue type, chosen fields,<br>labels|Keep the partner's<br>copy in sync|Your site and the<br>paired partner site|Not stored by the<br>app; it lives in Jira<br>on both sites|
|New public comments, with the<br>author's display name|Show the comment<br>on the partner's<br>issue|Your site and the<br>paired partner site|Not stored by the<br>app|
|Attached files (only if your<br>administrator switches attachments<br>on)|Copy files to the<br>partner's issue|The paired partner<br>site|Pieces of a file in<br>transfer: up to 1<br>day|
|Connection settings, mappings and<br>the pairing secret|Run the connection|App storage on your<br>site (the secret in<br>encrypted storage)|Until the<br>connection is<br>deleted or the<br>app is uninstalled|
|Issue links (issue ids and keys on<br>both sites) and each issue's last sync<br>result|Know which issues<br>belong together;<br>show the issue<br>panel|App storage on your<br>site|Until the issue or<br>connection is<br>deleted, or the<br>app is uninstalled|
|Activity log: issue keys, file names,<br>comment authors' display names,<br>error messages|Let administrators<br>see what happened|App storage on your<br>site|30 days|



|Data|Purpose|Where it goes|Kept for|
|---|---|---|---|
|Ids of processed messages, with<br>their result|Avoid duplicates<br>when a message is<br>retried|App storage on your<br>site|2 days|
|Operational log lines (similar to the<br>activity log)|Diagnose problems|Atlassian's Forge<br>logging; we may<br>read them to<br>provide support|As set by Atlassian|



The app never sends comments restricted to a group or role, internal notes in Jira Service Management projects, or issues with an issue security level. It does not process passwords, API tokens, email addresses or user account details. 

## **Who receives data** 

- **The paired partner site.** Your Jira administrator chooses which project to connect and which partner to pair with. Data reaches only that partner's Jira site, through the partner's own installation of the app. 

- **Atlassian.** The app runs on Atlassian's Forge platform, which hosts the app's functions, storage and logs under Atlassian's privacy policy. 

- **Us.** We do not receive your issue content. We may see operational logs to help with a support request. 

Your organisation is responsible for having the right to share issue data with the partner it pairs with, for example under its agreement with that partner. 

## **Your choices** 

- Administrators decide which project, issue types, fields, comments and files are synced, per partner. 

- **Disconnect** stops all exchange with a partner at once. Deleting a connection removes its stored settings and links. 

- Uninstalling the app removes its stored data on your site, following Atlassian's Forge data deletion. 

## **Security** 

Messages between the two sites are sent over HTTPS and signed with a secret unique to each pairing. Pairing secrets are kept in Forge's encrypted storage. Only Jira administrators can change the app's settings. 

## **Changes and contact** 

We will post changes to this policy on this page with a new effective date. Questions or requests: <u>rahul.savaikar@gmail.com.</u>

See also: [End-user terms](./end-user-terms.html)
