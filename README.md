[contributors-shield]: https://img.shields.io/github/contributors/jobmetric/github-notify.svg?style=for-the-badge
[contributors-url]: https://github.com/jobmetric/github-notify/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/jobmetric/github-notify.svg?style=for-the-badge&label=Fork
[forks-url]: https://github.com/jobmetric/github-notify/network/members
[stars-shield]: https://img.shields.io/github/stars/jobmetric/github-notify.svg?style=for-the-badge
[stars-url]: https://github.com/jobmetric/github-notify/stargazers
[license-shield]: https://img.shields.io/github/license/jobmetric/github-notify.svg?style=for-the-badge
[license-url]: https://github.com/jobmetric/github-notify/blob/master/LICENCE.md
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-blue.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/majidmohammadian

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![MIT License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

# GitHub Notify

**The easiest way to send GitHub repository notifications to Telegram!** 

Reusable GitHub Actions workflows that make it incredibly simple to get notified about Pull Requests, CI/CD status, releases, and other repository events. Just a few lines of code - no complex configuration needed!

## Documentation

GitHub Notify is a reusable GitHub Action that allows you to send notifications to Telegram when specific events occur in your repository. This action can be integrated into your GitHub Actions workflows to notify you about Pull Requests, CI/CD pipeline status, releases, and other repository events.

### How It Works

GitHub Notify provides **reusable workflows** that make it incredibly easy to send notifications to Telegram. Simply call our pre-built workflows with just a few lines of code - no need to write complex message templates or configure multiple steps.

The system uses the Telegram Bot API to send messages to your Telegram chat, group, or channel. When configured in your GitHub Actions workflow, it will automatically send beautifully formatted notifications whenever the workflow is triggered (e.g., on push, pull request, release, etc.).

**How Reusable Workflows Work:**
1. You create a simple workflow file in your repository
2. You call our reusable workflow with your chat ID and bot token
3. Our workflow handles all the formatting and message creation automatically
4. You receive a beautifully formatted notification in Telegram

**Coming Soon Features:**
- More pre-built reusable workflows for different event types (push, release, CI/CD, issues, etc.)
- Support for multiple notification channels (Slack, Discord, Email, etc.)
- Rich message formatting with attachments and media
- Customizable message templates for different event types
- Webhook support for real-time notifications
- Notification filtering and routing based on event types
- Integration with GitHub's native notification system
- Support for scheduled notifications and digest summaries

### Prerequisites

Before using this action, you need to:

1. Create a Telegram bot and obtain a bot token
2. Get the chat ID where you want to receive notifications
3. Configure GitHub secrets for secure token storage

---

## Step 1: Create a Telegram Bot

To create a Telegram bot, you need to interact with BotFather, Telegram's official bot for creating and managing bots.

### Getting Your Bot Token

1. **Open Telegram** and search for `@BotFather` in the search bar
2. **Start a conversation** with BotFather by clicking "Start" or sending `/start`
3. **Create a new bot** by sending the command `/newbot`
4. **Choose a name** for your bot (e.g., "My GitHub Notifier")
5. **Choose a username** for your bot (must end with `bot`, e.g., `my_github_notifier_bot`)
6. **Copy the bot token** that BotFather provides you. It will look something like:
   ```
   123456789:ABCdefGHIjklMNOpqrsTUVwxyz
   ```
   ⚠️ **Important:** Keep this token secure and never commit it to your repository!

### Bot Permissions

Your bot needs permission to send messages. If you're sending to a:
- **Private chat**: The bot can send messages directly to you
- **Group**: Add the bot to the group and make sure it has permission to send messages
- **Channel**: Add the bot as an administrator with permission to post messages

---

## Step 2: Get Your Telegram Chat ID

The chat ID identifies where the bot should send messages. The method to get the chat ID depends on whether you're using a private chat, group, or channel.

### Method 1: Get Your Personal Chat ID

1. **Start a conversation** with your bot by searching for it in Telegram and clicking "Start"
2. **Send any message** to your bot (e.g., "Hello")
3. **Visit this URL** in your browser (replace `YOUR_BOT_TOKEN` with your actual bot token):
   ```
   https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates
   ```
4. **Look for the `chat` object** in the JSON response. The `id` field is your chat ID:
   ```json
   {
     "ok": true,
     "result": [{
       "message": {
         "chat": {
           "id": 123456789,
           "first_name": "Your Name",
           "type": "private"
         }
       }
     }]
   }
   ```
5. **Copy the chat ID** (in this example: `123456789`)

### Method 2: Get a Group Chat ID

1. **Add your bot to the group** (search for your bot and add it)
2. **Send a message** in the group (any message will do)
3. **Visit the same URL** as above:
   ```
   https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates
   ```
4. **Find the group chat ID** in the response. Group IDs are negative numbers (e.g., `-1001234567890`)

### Method 3: Get a Channel Chat ID

1. **Create or select a channel** in Telegram
2. **Add your bot as an administrator**:
   - Go to channel settings
   - Click "Administrators"
   - Click "Add Administrator"
   - Select your bot
   - Make sure "Post Messages" permission is enabled
3. **Post a message** in the channel
4. **Get the channel ID** using one of these methods:

   **Option A: Using the API**
   - Visit: `https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates`
   - Look for the channel ID (it will be negative, e.g., `-1001234567890`)

   **Option B: Using a Telegram Bot**
   - Forward a message from your channel to `@userinfobot` or `@getidsbot`
   - The bot will reply with the channel ID

   **Option C: Using the Channel Username**
   - If your channel is public, you can use the channel username (e.g., `@my_channel`)
   - For private channels, you must use the numeric ID

### Quick Test

To verify everything is set up correctly, you can test sending a message using curl:

```bash
curl -X POST "https://api.telegram.org/botYOUR_BOT_TOKEN/sendMessage" \
  -d "chat_id=YOUR_CHAT_ID" \
  -d "text=Test message from GitHub Notify"
```

If successful, you should receive the message in your Telegram chat.

---

## Step 3: Configure GitHub Secrets

To securely store your bot token and chat ID, you need to add them as GitHub Secrets in your repository.

### Adding Secrets to Your Repository

1. **Go to your GitHub repository** on GitHub.com
2. **Click on "Settings"** (in the repository navigation bar)
3. **Click on "Secrets and variables"** → **"Actions"** (in the left sidebar)
4. **Click "New repository secret"**
5. **Add the following secrets:**

   **Secret 1: `TELEGRAM_BOT_TOKEN`**
   - Name: `TELEGRAM_BOT_TOKEN`
   - Value: Your bot token from BotFather (e.g., `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)
   - Click "Add secret"

   **Secret 2: `TELEGRAM_CHAT_ID`**
   - Name: `TELEGRAM_CHAT_ID`
   - Value: Your chat ID (e.g., `123456789` or `-1001234567890`)
   - Click "Add secret"

### Adding Secrets to an Organization

If you want to use the same secrets across multiple repositories in an organization:

1. **Go to your organization** on GitHub.com
2. **Click on "Settings"**
3. **Click on "Secrets and variables"** → **"Actions"**
4. **Click "New organization secret"**
5. **Add the secrets** as described above
6. **Configure repository access** to specify which repositories can use these secrets

---

## Step 4: Configure Your GitHub Actions Workflow

Now that you have your bot token and chat ID configured, you can use GitHub Notify in your workflows. 

**🎯 We highly recommend using our pre-built reusable workflows** - they're the easiest and fastest way to get started! Just a few lines of code and you're done. No need to write complex message templates or worry about formatting.

---

## 🚀 **Quick Start: Using Reusable Workflows (Recommended)**

**This is the easiest and fastest way to get started!** Simply create a workflow file and call our reusable workflow - no need to write complex message templates or configure multiple steps.

### **Pull Request Notifications**

Create a workflow file in your repository at `.github/workflows/notify-pr.yml`:

```yaml
name: Notify PR via Telegram

on:
  pull_request:
    types: [opened, reopened, ready_for_review]

jobs:
  notify:
    uses: jobmetric/github-notify/.github/workflows/telegram-pull-request-notify.yml@v1
    with:
      chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    secrets:
      bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
```

That's it! 🎉 This workflow will automatically send beautifully formatted notifications to your Telegram whenever:
- A pull request is opened
- A pull request is reopened
- A pull request is marked as ready for review

The notification includes:
- PR title and author
- Repository and branch information
- Draft status indicator
- Direct link to the pull request

### **How to Use Reusable Workflows**

1. **Create a workflow file** in your repository:
   - Go to your repository on GitHub
   - Click on the `.github` folder (create it if it doesn't exist)
   - Click on the `workflows` folder (create it if it doesn't exist)
   - Click "Add file" → "Create new file"
   - Name it something like `notify-pr.yml`

2. **Copy and paste the workflow code** (like the example above)

3. **Make sure your secrets are configured** (from Step 3 above)

4. **Commit the file** - GitHub Actions will automatically detect and run it

**Important Notes:**
- Use the version tag (e.g., `@v1`) for stability, or `@main` for the latest version
- The `chat_id` is passed as an input (can be a secret reference like `${{ secrets.TELEGRAM_CHAT_ID }}`)
- The `bot_token` must be passed as a secret (never hardcode it!)
- You can customize which events trigger the workflow (e.g., only on `opened` or also on `closed`, `merged`, etc.)

**Example: Customizing Events**

You can customize when notifications are sent by modifying the `types` array:

```yaml
on:
  pull_request:
    types: [opened, reopened, ready_for_review, closed, merged]
```

This will send notifications for all these PR events.

### **More Reusable Workflows Coming Soon**

We're working on more pre-built workflows for:
- Push notifications
- Release notifications
- CI/CD status notifications
- Issue notifications
- And more!

---

## 📝 **Advanced Usage: Using the Action Directly**

If you need more control over the message format or want to create custom notifications, you can use the Telegram action directly in your workflows.

### Basic Usage

Create a workflow file in your repository at `.github/workflows/notify.yml`:

```yaml
name: Notify on Push

on:
  push:
    branches: [ main, master ]
  pull_request:
    types: [ opened, closed, merged ]

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Send Telegram notification
        uses: jobmetric/github-notify/telegram@main
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          message: |
            🚀 New push to {{ repository.name }}
            
            Branch: {{ github.ref_name }}
            Author: {{ github.actor }}
            Commit: {{ github.sha }}
          parse_mode: 'Markdown'
```

### Advanced Usage Examples

#### Example 1: Notify on Release

```yaml
name: Notify on Release

on:
  release:
    types: [ published ]

jobs:
  notify-release:
    runs-on: ubuntu-latest
    steps:
      - name: Send release notification
        uses: jobmetric/github-notify/telegram@main
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          message: |
            🎉 New Release Published!
            
            **{{ repository.name }}** v{{ github.event.release.tag_name }}
            
            {{ github.event.release.body }}
            
            [View Release]({{ github.event.release.html_url }})
          parse_mode: 'Markdown'
```

#### Example 2: Notify on CI/CD Status

```yaml
name: CI/CD Notifications

on:
  workflow_run:
    workflows: ["CI"]
    types:
      - completed

jobs:
  notify-ci:
    runs-on: ubuntu-latest
    steps:
      - name: Send CI status
        uses: jobmetric/github-notify/telegram@main
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          message: |
            {{ workflow_run.conclusion == 'success' && '✅' || '❌' }} CI/CD {{ workflow_run.conclusion }}
            
            Workflow: {{ workflow_run.name }}
            Repository: {{ repository.name }}
            Branch: {{ workflow_run.head_branch }}
            
            [View Workflow Run]({{ workflow_run.html_url }})
          parse_mode: 'Markdown'
```

#### Example 3: Custom Message with HTML Formatting

```yaml
name: Custom Notification

on:
  pull_request:
    types: [ opened, closed ]

jobs:
  notify-pr:
    runs-on: ubuntu-latest
    steps:
      - name: Send PR notification
        uses: jobmetric/github-notify/telegram@main
        with:
          bot_token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
          message: |
            <b>Pull Request {{ github.event.action }}</b>
            
            <i>{{ github.event.pull_request.title }}</i>
            
            Author: <code>{{ github.event.pull_request.user.login }}</code>
            Repository: <code>{{ repository.name }}</code>
            
            <a href="{{ github.event.pull_request.html_url }}">View PR</a>
          parse_mode: 'HTML'
```

### Action Inputs

The Telegram action accepts the following inputs:

| Input | Required | Description | Default |
|-------|----------|-------------|---------|
| `bot_token` | Yes | Telegram bot token from BotFather | - |
| `chat_id` | Yes | Target Telegram chat ID (user, group, or channel) | - |
| `message` | Yes | Message text to send | - |
| `parse_mode` | No | Telegram parse mode: `Markdown`, `HTML`, or `MarkdownV2` | `Markdown` |

### Parse Modes

Telegram supports different parse modes for formatting messages:

- **Markdown**: Basic Markdown formatting (legacy)
  ```markdown
  *bold* _italic_ `code` [link](url)
  ```

- **HTML**: HTML tags for formatting
  ```html
  <b>bold</b> <i>italic</i> <code>code</code> <a href="url">link</a>
  ```

- **MarkdownV2**: Enhanced Markdown with better escaping
  ```markdown
  *bold* _italic_ `code` [link](url)
  ```

### Using GitHub Context Variables

You can use GitHub's context variables in your messages to include dynamic information:

- `{{ github.repository }}` - Repository name (e.g., `owner/repo`)
- `{{ github.ref_name }}` - Branch or tag name
- `{{ github.sha }}` - Commit SHA
- `{{ github.actor }}` - Username of the person who triggered the workflow
- `{{ github.event.* }}` - Event-specific data (varies by event type)
- `{{ github.workflow }}` - Workflow name
- `{{ github.run_id }}` - Unique run ID
- `{{ github.run_number }}` - Run number for this workflow

For more context variables, see the [GitHub Actions documentation](https://docs.github.com/en/actions/learn-github-actions/contexts).

---

## Troubleshooting

### Bot Not Sending Messages

1. **Verify bot token**: Make sure the token is correct and hasn't been revoked
2. **Check chat ID**: Ensure the chat ID is correct (positive for users, negative for groups/channels)
3. **Bot permissions**: For groups/channels, ensure the bot has permission to send messages
4. **Start the bot**: For private chats, make sure you've started a conversation with the bot

### Getting 401 Unauthorized Error

- Your bot token is incorrect or has been revoked
- Regenerate the token from BotFather and update your GitHub secret

### Getting 400 Bad Request Error

- Check that your chat ID is correct
- Verify the message format matches the parse mode you're using
- Ensure special characters are properly escaped (especially for MarkdownV2)

### Messages Not Appearing in Channel

- Make sure the bot is added as an administrator to the channel
- Verify the bot has "Post Messages" permission enabled
- Check that you're using the correct channel ID (not the username for private channels)

---

## Security Best Practices

1. **Never commit secrets**: Always use GitHub Secrets, never hardcode tokens in workflow files
2. **Use organization secrets**: For multiple repositories, use organization-level secrets
3. **Rotate tokens regularly**: Periodically regenerate your bot token for security
4. **Limit bot permissions**: Only grant the minimum permissions necessary
5. **Review workflow files**: Regularly audit your workflow files for security issues

---

## Examples Repository

For more examples and use cases, check out the [examples directory](https://github.com/jobmetric/github-notify/tree/main/examples) in the repository.

---

## Support

If you encounter any issues or have questions:

1. Check the [Troubleshooting](#troubleshooting) section above
2. Search existing [Issues](https://github.com/jobmetric/github-notify/issues)
3. Create a new [Issue](https://github.com/jobmetric/github-notify/issues/new) with details about your problem

---

## Roadmap

**Coming Soon:**
- Support for multiple notification channels (Slack, Discord, Email, etc.)
- Rich message formatting with attachments and media
- Customizable message templates for different event types
- Webhook support for real-time notifications
- Notification filtering and routing based on event types
- Integration with GitHub's native notification system
- Support for scheduled notifications and digest summaries
- Interactive buttons and inline keyboards in Telegram messages
- Support for sending files and media (images, documents, etc.)
- Rate limiting and retry mechanisms for reliable delivery

## Contributing

Thank you for considering contributing to GitHub Notify! See the [CONTRIBUTING.md](https://github.com/jobmetric/github-notify/blob/master/CONTRIBUTING.md) for details.

## License

The MIT License (MIT). Please see [License File](https://github.com/jobmetric/github-notify/blob/master/LICENCE.md) for more information.
