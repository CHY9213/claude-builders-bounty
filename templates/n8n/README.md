# 📬 GitHub Weekly Digest — n8n Workflow

Automatically generate a narrative weekly summary of your GitHub repo's activity using Claude API, delivered to your Discord/Slack.

## Setup (5 Steps)

### 1. Import the Workflow

- Open your n8n instance
- Go to **Workflows → Add Workflow → Import from File**
- Upload `github-weekly-digest.json`

### 2. Add Credentials

In n8n, create the following credentials:

| Node | Credential Type | Needed For |
|------|----------------|------------|
| GitHub API calls | **Header Auth** | GitHub API token with `repo` scope |
| Claude API Call | **Header Auth** | Anthropic API key (`sk-ant-...`) |

### 3. Configure Variables

Open the **GitHub Config** node and set:

| Variable | Example | Description |
|----------|---------|-------------|
| `repoOwner` | `vercel` | GitHub repository owner |
| `repoName` | `next.js` | GitHub repository name |
| `language` | `EN` | Output language (`EN` or `ZH`) |
| `webhookUrl` | `https://discord.com/...` | Discord/Slack webhook URL |

### 4. Activate

Click **Active** toggle to enable the weekly cron trigger (Fridays at 5PM).

### 5. Test

Click **Execute Workflow** to run a test immediately.

## Workflow Structure

```
Schedule (Cron: Fri 5PM)
  │
  └─ GitHub Config (repo, language, webhook)
       ├── Fetch Weekly Commits (GitHub API)
       ├── Fetch Closed Issues (GitHub API)
       └── Fetch Merged PRs (GitHub API)
              │
              └─ Merge & Format Data
                     │
                     └─ Claude API Call (claude-sonnet-4-20250514)
                            │
                            └─ Format Digest
                                   │
                                   └─ Send to Discord Webhook
```

## Output Example

On your Discord channel, you'll receive an embedded message like:

> **Weekly Digest: vercel/next.js**
>
> This week's focus was on optimizing the Turbopack integration...
>
> 📊 Commits: 45 | Issues Closed: 12 | PRs: 8

## Customization

- **Delivery channel**: Replace the Discord webhook node with n8n's Slack, Email, or Telegram nodes
- **Schedule**: Change the cron expression in the Schedule Trigger node
- **Language**: Set `language` to `ZH` for Chinese output

## Requirements

- n8n instance (self-hosted or n8n.cloud)
- GitHub personal access token (public repo access only needs `public_repo`)
- Anthropic API key (Claude Sonnet 4)
- Discord webhook URL (or alternative delivery channel)
