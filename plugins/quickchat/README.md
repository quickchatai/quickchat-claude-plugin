# Quickchat AI

Build, deploy, analyze, and improve customer support AI Agents from your AI app.

Quickchat AI connects Claude to your Quickchat account. Ask Claude to build an AI Agent from your website, try it with a real customer question, put it live on your site or in WhatsApp, Discord, Telegram, Instagram, Slack or email, review how it performs, triage the conversations that need a human, set when it hands them to your team, and improve its knowledge, settings and AI Actions. It works in Claude on the web and desktop, in Cowork, and in Claude Code.

The Cursor package reuses the same MCP configuration and skills. Its marketplace submission and in-app validation are pending. To test it, copy this entire directory, including hidden files, to `~/.cursor/plugins/local/quickchat` on the machine running Cursor. Restart Cursor or run **Developer: Reload Window**, then find Quickchat in **Customize**. On managed accounts, an administrator may need to allow local plugin imports. See [Cursor's local plugin instructions](https://cursor.com/docs/plugins#test-plugins-locally).

## Get started

1. Connect the plugin's Quickchat AI connector. In Claude, open Customize > Plugins > Quickchat AI > Connectors. In Claude Code, run `/mcp` and choose the Quickchat server. In Cursor, open Customize and connect the Quickchat MCP server from the locally installed plugin.
2. Sign in to Quickchat in the browser window that opens. There is no API key to paste. New to Quickchat? Signing up during this step creates your account and a free AI Agent.
3. Ask your AI app: "Build me an AI Agent from my website."

## What's inside

**Connector:** the hosted Quickchat AI MCP server at `https://app.quickchat.ai/v1/api/mcp/rpc`, also listed in the Claude Directory as Quickchat AI.

**Skills,** which your AI app uses when a request fits:

| Skill | What it does |
|---|---|
| `launch-agent` | Sets up an Agent from a website or a short interview, then tries it with a customer question you confirm |
| `deploy-agent` | Reports which channels are live and connects new ones |
| `performance-review` | Turns analytics into a prioritized review |
| `conversation-triage` | Finds the conversations that need a human, then assigns or resolves the ones you approve |
| `improve-agent` | Audits settings, knowledge and AI Actions, then makes the edits you approve |
| `setup` | Connects the server and confirms it works |

**Claude commands,** when you'd rather be explicit:

```
/quickchat:agent-launch   https://example.com
/quickchat:agent-deploy   [agent] [channel]
/quickchat:agent-review   [agent] [period]
/quickchat:agent-triage   [agent]
/quickchat:agent-improve  [agent]
/quickchat:agent-topic    <topic> [agent]
```

In Cursor, ask for these workflows in natural language or select a shared skill. The Cursor package does not load the Claude command files.

## Try it

```
Build my AI Agent from my website, then let me try one customer question here.
Put my AI Agent on my website and connect it to Telegram.
Review my AI Agent's last 7 days and tell me the three highest-impact improvements.
Find unresolved and low-rated conversations, group the causes, and recommend fixes.
```

## What this plugin runs, sends and fetches

- **No executable helpers.** The plugin has no hooks, scripts or local servers. Its skills and commands are instructions for your AI app.
- **One connector, over HTTPS.** Your AI app signs in to Quickchat's hosted MCP server with OAuth and PKCE and keeps the token. The plugin stores no credentials and never asks for an API key or password in chat.
- **Your permissions, nothing more.** Every tool enforces the same per-Agent roles as the Quickchat dashboard, so the connector reaches only the Agents and conversations your account can already reach.
- **Reads:** Agent settings, knowledge-base articles and website sources, analytics, conversation transcripts and diagnostics, CSAT and in-chat feedback, insights, AI Action configuration and call logs, simulation datasets and results, channel and deployment status, and your plan and AI credits.
- **Writes, when you ask:** creates and configures Agents, including the profile picture and Human Handoff (when to hand off, working hours, messages and handoff emails); edits knowledge and website sources; manages AI Actions, including remote MCP servers, knowledge-base search and Shopify Actions; assigns and resolves Inbox conversations; connects channels; and runs simulations.
- **Confirms first:** rebuilding an Agent from a website (it overwrites the persona and settings), creating an Agent, adding handoff email recipients, deleting knowledge-base articles or AI Actions, removing website sources, resolving conversations, running simulations, and sending any test request other than a GET.
- **Spends AI credits:** test messages to an Agent, simulation runs and generated test datasets.
- **Reaches outside Quickchat only on request:** fetching the public website you name, sending a test request to an HTTP endpoint you configured, and connecting a remote MCP server you add.
- **Exports** arrive as short-lived signed URLs. Quickchat receives the arguments your AI app sends to its tools; the connector has no capability to browse your AI app's memory, chat history or files.

## Privacy and support

- Privacy policy: https://quickchat.ai/privacy
- Terms: https://quickchat.ai/terms
- Documentation: https://docs.quickchat.ai/manage-with-ai/
- Support: support@quickchat.ai or https://quickchat.ai/contact

## License

MIT. See [LICENSE](./LICENSE).
