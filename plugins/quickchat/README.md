# Quickchat AI

Build, deploy, analyze, and improve customer support AI Agents without leaving Claude.

Quickchat AI connects Claude to your Quickchat account. Ask Claude to build an AI Agent from your website, put it live on your site or in Discord, Telegram, Instagram, Slack or email, review how it performs, triage the conversations that need a human, and improve its knowledge and settings. It works in Claude on the web and desktop, in Cowork, and in Claude Code.

## Get started

1. Connect the plugin's Quickchat AI connector. In Claude, open Customize > Plugins > Quickchat AI > Connectors. In Claude Code, run `/mcp` and choose the Quickchat server.
2. Sign in to Quickchat in the browser window that opens. There is no API key to paste. New to Quickchat? Signing up during this step creates your account and a free AI Agent.
3. Ask Claude: "Build me an AI Agent from my website."

## What's inside

**Connector:** the hosted Quickchat AI MCP server at `https://app.quickchat.ai/v1/api/mcp/rpc`, also listed in the Claude Directory as Quickchat AI.

**Skills,** which Claude uses on its own when a request fits:

| Skill | What it does |
|---|---|
| `launch-agent` | Sets up an Agent from a website or a short interview |
| `deploy-agent` | Reports which channels are live and connects new ones |
| `performance-review` | Turns analytics into a prioritized review |
| `conversation-triage` | Finds the conversations that need a human, then assigns or resolves the ones you approve |
| `improve-agent` | Audits settings and knowledge, then makes the edits you approve |
| `setup` | Connects the server and confirms it works |

**Commands,** when you'd rather be explicit:

```
/quickchat:agent-launch   https://example.com
/quickchat:agent-deploy   [agent] [channel]
/quickchat:agent-review   [agent] [period]
/quickchat:agent-triage   [agent]
/quickchat:agent-improve  [agent]
/quickchat:agent-topic    <topic> [agent]
```

## Try it

```
Build a customer support AI Agent from my website and prepare it for launch.
Put my AI Agent on my website and connect it to Telegram.
Review my AI Agent's last 7 days and tell me the three highest-impact improvements.
Find unresolved and low-rated conversations, group the causes, and recommend fixes.
```

## What this plugin runs, sends and fetches

- **Nothing runs on your computer.** The plugin has no hooks, scripts or local servers. Its skills and commands are instructions for Claude.
- **One connector, over HTTPS.** Claude signs in to Quickchat's hosted MCP server with OAuth and PKCE and keeps the token. The plugin stores no credentials and never asks for an API key or password in chat.
- **Your permissions, nothing more.** Every tool enforces the same per-Agent roles as the Quickchat dashboard, so the connector reaches only the Agents and conversations your account can already reach.
- **Reads:** Agent settings, knowledge-base articles and website sources, analytics, conversation transcripts and diagnostics, CSAT and in-chat feedback, insights, AI Action configuration and call logs, simulation datasets and results, channel and deployment status, and your plan and AI credits.
- **Writes, when you ask:** creates and configures Agents, including the profile picture; edits knowledge and website sources; manages AI Actions, including remote MCP servers; assigns and resolves Inbox conversations; connects channels; and runs simulations.
- **Confirms first:** rebuilding an Agent from a website (it overwrites the persona and settings), creating an Agent, deleting knowledge-base articles or AI Actions, removing website sources, resolving conversations, and running simulations.
- **Spends AI credits:** test messages to an Agent, simulation runs and generated test datasets.
- **Reaches outside Quickchat only on request:** fetching the public website you name, sending a test request to an HTTP endpoint you configured, and connecting a remote MCP server you add.
- **Exports** arrive as short-lived signed URLs. Quickchat does not access your Claude memory, chat history or files.

## Privacy and support

- Privacy policy: https://quickchat.ai/privacy
- Terms: https://quickchat.ai/terms
- Documentation: https://docs.quickchat.ai/manage-with-ai/
- Support: support@quickchat.ai or https://quickchat.ai/contact

## License

MIT. See [LICENSE](./LICENSE).
