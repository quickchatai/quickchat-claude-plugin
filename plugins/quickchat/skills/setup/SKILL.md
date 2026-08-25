---
name: setup
description: >-
  Connect this plugin's Quickchat AI MCP server and confirm it works. Use when
  the Quickchat plugin has just been installed, when a Quickchat tool fails with
  an authentication or "not connected" error, or when the user asks how to sign
  in to Quickchat, connect their account, or get started. Do NOT use for
  ordinary Quickchat work once the connection is already established.
metadata:
  author: quickchat-ai
  version: "1.0"
---

# Connect Quickchat AI

The plugin ships a remote MCP server at `https://app.quickchat.ai/v1/api/mcp/rpc`.
It authenticates with OAuth, so there is no API key to paste.

## Connect

1. The server is declared in the plugin, so Claude offers it automatically. When
   prompted, approve the connection.
2. A Quickchat consent screen opens in the browser. Sign in with a Quickchat
   account, or create one — signing up here also creates a free AI Agent.
3. Approve, and the browser returns to Claude.

If nothing happens, ask the user to check that the plugin is enabled and, in
Claude Code, that `/mcp` lists `quickchat` as connected.

## Confirm it worked

Call `whoami`, then `list_scenarios`. A successful `list_scenarios` returns the
AI Agents the account can reach, with the user's role on each.

- Empty list — the account has no Agents yet. Offer the `launch-agent` skill to
  build one from a website.
- Authentication error — the OAuth flow did not complete. Ask the user to
  reconnect rather than retrying the tool.

## What the account controls

Every tool enforces the same per-Agent roles as the Quickchat dashboard, so the
connection reaches only the Agents and conversations that account can already
reach. Some write operations need Editor or Support; a few also depend on the
plan's limits.

## Where to go next

| The user wants to | Skill |
|---|---|
| Build a new Agent from a website or an interview | `launch-agent` |
| See how an Agent is performing | `performance-review` |
| Find conversations that need a human | `conversation-triage` |
| Audit and improve settings or knowledge | `improve-agent` |
