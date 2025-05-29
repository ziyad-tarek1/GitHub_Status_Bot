# 🛠️ GitHub Status Bot

This GitHub Action periodically checks the [GitHub Status API](https://www.githubstatus.com) for new or updated **incidents** and sends formatted alerts to a Slack channel via webhook.

Incidents include disruptions or outages across GitHub services like:
- GitHub Actions
- Git operations
- Codespaces
- API services
- Webhooks
- Pages
- And more...

## 📦 Features

- ✅ Detects newly created or recently updated incidents
- 🔁 Skips older incidents (>12 hours old)
- 🧠 Remembers what’s already sent (caching to prevent duplicates)
- 🚀 Posts alerts to Slack with direct incident links
- 🕰️ Scheduled to run every 20 minutes

---

## 🔧 Workflow Setup

This GitHub Actions workflow is scheduled and manually dispatchable:

```yaml
name: GitHub Status Multi-Incident Monitor

on:
  schedule:
    - cron: '*/20 * * * *'
  workflow_dispatch:
````

It runs a single job:

* **Checks out the repo**
* **Restores a local incident log cache**
* **Fetches live incidents from GitHub Status API**
* **Compares with previous log**
* **Posts new/resolved events to Slack**
* **Saves the updated log to cache**

---

## 💬 Slack Alerts

You’ll receive messages like:

```
✅ Resolved GitHub Incident: Delayed GitHub Actions Jobs
Status: resolved
📅 Resolved At: 2025-05-22T09:17:56.960Z
🔗 https://stspg.io/frxj4njt4g76
```

Or for new disruptions:

```
🚨 New GitHub Incident: Degraded GitHub API performance
Status: investigating
📅 Created At: 2025-05-28T14:12:03.452Z
🔗 https://stspg.io/7xj29k5
```
---

## 🧪 Environment Variables

You must define the following **repository secret**:

| Secret Name         | Description                         |
| ------------------- | ----------------------------------- |
| `SLACK_WEBHOOK_URL` | Slack webhook URL for notifications |

## 🗂️ Caching Behavior

The workflow uses GitHub Actions' cache to store a file `.gh-status/incident_log.json` that tracks which incidents (by `id` + `status`) have already been sent.

This avoids duplicate alerts.

---


### ⚠️ Known Limitation

If the cache fails to save (e.g. due to multiple concurrent runs), the next run may reprocess old incidents. To avoid this:

* You can improve the cache key to use a dynamic key per-run with a shared restore key (see Issues section below).
---

## 📁 File Structure

```text
.gh-status/
├── incidents.json         ← Fetched from GitHub Status API
└── incident_log.json      ← Tracks posted incident `id` + `status`
```
---

## 🛠️ Improvements & To-do

* [ ] Improve caching strategy with dynamic keys to avoid save conflicts
* [ ] Store incident log in repo or external DB to persist more reliably
* [ ] Enhance support for incident groups / subcomponents
* [ ] Add tests or local dry-run script

---

## 🧾 License

MIT License © 2025


