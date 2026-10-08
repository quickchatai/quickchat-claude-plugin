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
---

# Connect Quickchat AI

The plugin ships a remote MCP server at `https://app.quickchat.ai/v1/api/mcp/rpc`.
It authenticates with OAuth, so there is no API key to paste.

## Connect

- **Claude on the web or desktop, and Cowork:** open Customize > Plugins, select
  Quickchat AI and open its Connectors tab. If the connector shows Not added, add
  it, then connect it. The same connector is listed in the Claude Directory as
  Quickchat AI, where the button reads **Connect to Claude**.
- **Claude Code:** run `/mcp`, choose the Quickchat server and sign in.

Either way a Quickchat sign-in page opens in the browser. Sign in with a
Quickchat account, or create one: signing up here also creates a free AI Agent.
Approve, and the browser returns to Claude.

## Confirm it worked

Call `whoami`, then `list_scenarios`. A successful `list_scenarios` returns the
AI Agents the account can reach, with the user's role on each.

- An entry with `configured: false` is the empty Agent the account came with.
  Offer the `launch-agent` skill to set it up from a website or a short
  interview.
- An empty list means the account can't reach any Agent yet. Ask the user to get
  a role from an admin of their Quickchat organization.
- An authentication error means the OAuth flow didn't complete. Ask the user to
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
| Put an Agent live on a website, chat page, Discord, Telegram, Instagram, Slack or email | `deploy-agent` |
| See how an Agent is performing | `performance-review` |
| Find conversations that need a human | `conversation-triage` |
| Audit and improve settings or knowledge | `improve-agent` |
