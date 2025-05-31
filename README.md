# GitHub Status Bot

A GitHub Actions workflow that monitors GitHub's status page and sends notifications to Slack when incidents are detected.

## Features

- 🔍 Monitors GitHub Status API every 20 minutes
- 🔔 Sends real-time notifications to Slack for:
  - 🚨 New incidents
  - ⚠️ Updated incidents
  - ✅ Resolved incidents
- 💾 Maintains a cache of incidents to prevent duplicate notifications
- 🔄 Persists cache between workflow runs
- ⏰ Configurable monitoring interval
- 🛠️ Manual trigger support

## Setup

1. **Fork this repository**

2. **Add Slack Webhook**
   - Go to your Slack workspace
   - Create a new app or use an existing one
   - Enable Incoming Webhooks
   - Create a new webhook URL
   - Add the webhook URL as a repository secret named `SLACK_WEBHOOK`

3. **Enable GitHub Actions**
   - Go to your repository settings
   - Navigate to Actions > General
   - Enable "Allow all actions and reusable workflows"

## How It Works

1. **Monitoring**
   - Runs every 20 minutes via cron schedule
   - Can be triggered manually via workflow_dispatch
   - Fetches incidents from GitHub Status API

2. **Incident Processing**
   - Checks for incidents within the last 20 minutes
   - Compares with cached incidents to detect changes
   - Categorizes incidents as new, updated, or resolved

3. **Notifications**
   - Sends formatted messages to Slack
   - Includes incident details:
     - Name and status
     - Creation/update/resolution times
     - Direct link to incident

4. **Cache Management**
   - Maintains a cache of all incidents
   - Persists between workflow runs
   - Prevents duplicate notifications
   - 7-day retention period

## Message Formats

### New Incident
```
🚨 *New GitHub Incident*
*[Incident Name]*
Status: *[Status]*
📅 Created At: [Timestamp]
🔗 [Shortlink]
```

### Updated Incident
```
⚠️ *Updated GitHub Incident*
*[Incident Name]*
Status: *[Status]*
📅 Updated At: [Timestamp]
🔗 [Shortlink]
```

### Resolved Incident
```
✅ *Resolved GitHub Incident*
*[Incident Name]*
Status: *[Status]*
📅 Resolved At: [Timestamp]
🔗 [Shortlink]
```

## Configuration

The workflow can be configured by modifying the following in `githubstatus.yaml`:

- `cron: '*/20 * * * *'` - Change the monitoring interval
- `retention-days: 7` - Adjust cache retention period
- Message formats in the Slack notification step

## Troubleshooting

1. **No Notifications**
   - Check if the workflow is running (Actions tab)
   - Verify Slack webhook URL is correct
   - Check workflow logs for any errors

2. **Duplicate Notifications**
   - Verify cache is being maintained
   - Check if previous run was successful
   - Look for any cache download errors

3. **Workflow Not Running**
   - Check repository permissions
   - Verify Actions are enabled
   - Check cron schedule syntax

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is licensed under the MIT License - see the LICENSE file for details. 