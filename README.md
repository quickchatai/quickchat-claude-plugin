# Quickchat AI — Claude plugin

Build, analyze, test, and improve customer support **AI Agents** from Claude.

This plugin connects Claude to [Quickchat AI](https://quickchat.ai) and adds the workflow
knowledge to use it well: launching an Agent from a website, reviewing performance, triaging
conversations, and improving knowledge and settings.

Works in **Claude Code** and **Cowork**.

## Install

```
/plugin marketplace add quickchatai/quickchat-claude-plugin
/plugin install quickchat@quickchat
```

On first use Claude connects the Quickchat MCP server and opens an OAuth sign-in. There is no API
key to paste. Signing up during that flow also creates a free AI Agent, so you can start from
nothing.

Already a Quickchat customer? Sign in with your normal credentials and your existing Agents appear.

## What you get

**A connector** — the hosted Quickchat MCP server at `https://app.quickchat.ai/v1/api/mcp/rpc`,
exposing tools for analytics, conversation search and transcripts, Agent settings, knowledge-base
management, AI Actions, simulation testing, exports, and deployment info.

**Five skills**, which Claude activates on its own when they fit:

| Skill | Job |
|---|---|
| `launch-agent` | Create and configure an Agent, from a URL or a short interview |
| `performance-review` | Turn analytics into a prioritized review |
| `conversation-triage` | Find and group the conversations needing a human |
| `improve-agent` | Audit settings and knowledge, then make safe edits |
| `setup` | Connect the server and confirm it works |

**Five commands**, when you'd rather be explicit:

```
/quickchat:agent-launch   https://example.com
/quickchat:agent-review   [agent] [period]
/quickchat:agent-triage   [agent]
/quickchat:agent-improve  [agent]
/quickchat:agent-topic    <topic> [agent]
```

## Try it

```
Review my AI Agent's last 7 days and tell me the three highest-impact improvements.
Find unresolved and low-rated conversations, group the causes, and recommend fixes.
Build a customer support AI Agent from my website and prepare it for launch.
```

## Permissions and safety

Every tool enforces the same per-Agent role checks as the Quickchat dashboard. The connector
reaches only the Agents and conversations your own account can already reach, and write operations
follow your existing Quickchat role.

Three tools have real side effects and ask for confirmation first:

- `onboard_assistant_from_url` overwrites an Agent's persona, settings and knowledge
- `start_simulation_run` spends AI credits and can fire the Agent's active AI Actions
- `test_http_request_action` sends a real request to the URL you configured

## Privacy Policy

Full policy: **<https://quickchat.ai/privacy>**. Terms: <https://quickchat.ai/terms>.

**What the connector reads.** Agent settings, knowledge-base article metadata and content,
analytics, customer-conversation transcripts, in-chat feedback and CSAT, conversation insights,
AI Action configuration and call logs, simulation datasets and results, and deployment
information — all scoped to the authenticated Quickchat account's own permissions.

**What it writes.** Authorized users can create and configure Agents, update knowledge, manage
AI Actions and simulation datasets, and start or cancel simulation runs.

**How data is handled.** Authentication is OAuth 2.0 with PKCE; no credentials are stored in this
repository or on your machine beyond the token Claude manages. Conversation exports are delivered
as short-lived signed URLs. Quickchat does not access Claude memory, chat history, or your files.
Retention, third-party processors, and contact details are covered in the privacy policy linked
above.

**Support:** support@quickchat.ai

## Contributing

The four workflow skills are maintained alongside the MCP server. Please open an issue before a
substantial change so it can be kept in sync with the tools it describes.

## License

MIT — see [LICENSE](./LICENSE).
