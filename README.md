# Quickchat AI plugin for Claude

Build, deploy, analyze, and improve customer support **AI Agents** from Claude.

This repository holds the [Quickchat AI](https://quickchat.ai) plugin for Claude. It works in Claude on the web and desktop, in Cowork, and in Claude Code, and bundles the hosted Quickchat AI connector with skills and commands for launching, deploying, reviewing and improving an Agent. The plugin lives in [`plugins/quickchat`](./plugins/quickchat), and its [README](./plugins/quickchat/README.md) covers everything it contains and what it reads, writes and sends.

## Install

**Claude and Cowork (paid plans):** open Customize > Plugins, find Quickchat AI under Discover and add it, then connect its Quickchat AI connector. The plugin syncs to Claude Code on the same account.

**Just the connector:** add [Quickchat AI from the Claude Directory](https://claude.ai/directory/connectors/app-quickchat-ai) and click **Connect to Claude**.

**Claude Code, from this repository:**

```
/plugin marketplace add quickchatai/quickchat-claude-plugin
/plugin install quickchat@quickchat
```

On first use, sign in to Quickchat in the browser. There is no API key to paste, and signing up during that step creates a free AI Agent. Existing Quickchat customers sign in with their usual account and see their Agents.

## Repository layout

| Path | What it is |
|---|---|
| `.claude-plugin/marketplace.json` | Marketplace manifest, for installing from this repository in Claude Code |
| `plugins/quickchat/` | The plugin: manifest, connector, skills, commands, README, license and icon |

## Contributing

The skills mirror the playbooks the Quickchat MCP server serves through its `get_playbook` tool, so they stay in step with the tools they describe. Please open an issue before a substantial change.

## Privacy, terms and support

Privacy policy: <https://quickchat.ai/privacy>. Terms: <https://quickchat.ai/terms>. Support: support@quickchat.ai.

## License

MIT. See [LICENSE](./LICENSE).
