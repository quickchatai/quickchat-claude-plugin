# Quickchat AI plugins

Build, deploy, analyze, and improve customer support **AI Agents** from your AI app.

This repository holds the [Quickchat AI](https://quickchat.ai) plugin for Claude. It works in Claude on the web and desktop, in Cowork, and in Claude Code, and bundles the hosted Quickchat AI connector with skills and commands for launching, deploying, reviewing and improving an Agent. The plugin lives in [`plugins/quickchat`](./plugins/quickchat), and its [README](./plugins/quickchat/README.md) covers everything it contains and what it reads, writes and sends.

The Cursor package uses the same hosted connector and skills. Its marketplace submission and in-app validation are pending. The local preview below is available for testing.

## Install

**Claude and Cowork (paid plans):** open Customize > Plugins, find Quickchat AI under Discover and add it, then connect its Quickchat AI connector. The plugin syncs to Claude Code on the same account.

**Just the connector:** add [Quickchat AI from the Claude Directory](https://claude.ai/directory/connectors/app-quickchat-ai) and click **Connect to Claude**.

**Claude Code, from this repository:**

```
/plugin marketplace add quickchatai/quickchat-claude-plugin
/plugin install quickchat@quickchat
```

On first use, sign in to Quickchat in the browser. There is no API key to paste, and signing up during that step creates a free AI Agent. Existing Quickchat customers sign in with their usual account and see their Agents.

## Test the Cursor package

Copy `plugins/quickchat` into `~/.cursor/plugins/local/quickchat` on the machine running Cursor. Copy the directory itself, including its hidden manifest and MCP files; a symlink to an external checkout will not load.

Restart Cursor or run **Developer: Reload Window**, then open **Customize** and find the Quickchat plugin. Connect its MCP server and sign in to Quickchat. Ask: "List my Quickchat agents, then summarize the settings of the agent I choose." Verify the returned data against the dashboard.

The package provides the six shared skills. The Claude slash commands remain available in Claude; Cursor uses the skills and natural-language requests. On managed Cursor accounts, an administrator may need to allow local plugin imports. See [Cursor's local plugin instructions](https://cursor.com/docs/plugins#test-plugins-locally).

Grok distribution is a separate review and compatibility test. Adding this package to Cursor does not publish it in Grok's connector catalog.

## Repository layout

| Path | What it is |
|---|---|
| `.claude-plugin/marketplace.json` | Marketplace manifest, for installing from this repository in Claude Code |
| `.cursor-plugin/marketplace.json` | Cursor marketplace manifest, pointing to the same plugin directory |
| `plugins/quickchat/` | The plugin: manifest, connector, skills, commands, README, license and icon |

## Contributing

The skills mirror the playbooks the Quickchat MCP server serves through its `get_playbook` tool, so they stay in step with the tools they describe. Please open an issue before a substantial change.

## Privacy, terms and support

Privacy policy: <https://quickchat.ai/privacy>. Terms: <https://quickchat.ai/terms>. Support: support@quickchat.ai.

## License

MIT. See [LICENSE](./LICENSE).
