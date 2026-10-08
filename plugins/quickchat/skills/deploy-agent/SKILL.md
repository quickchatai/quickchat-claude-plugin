---
name: deploy-agent
description: >-
  Put a configured Quickchat AI Agent in front of real users: report which
  channels are live, then connect the ones the user wants. The website widget,
  public chat page, the Agent's own MCP endpoint, Discord, Telegram, Instagram,
  Slack and Email all connect through the tools here, without sending the user
  to the dashboard. Use when the user asks to deploy, publish, go live, share
  the Agent, embed it on a site, or connect or integrate it with Telegram,
  Discord, Instagram, Slack, email, WhatsApp or any other channel. Not for
  building or configuring the Agent itself (use launch-agent or improve-agent).
metadata:
  author: quickchat-ai
---

# Deploy an Agent to its channels

Connecting a channel is tool work, not dashboard homework. Every channel below
has a first-class path through `connect_agent_channel`; only the ones under
"Dashboard-only channels" need the Quickchat dashboard, and you say so exactly
once instead of guessing.

## Before you start
1. Resolve the Agent: call `list_scenarios` and match the name to its
   `scenario_id`. If several qualify, ask which one.
2. Call `get_deployment_info` and read `channels` — that is the verified state.
   A channel listed in `connected` is live; `not_configured` is a definite no;
   `not_checked` (or absent from all four lists) means you could not verify it,
   so say that rather than guessing.
3. A channel name is never a website. When the user says "connect Telegram" or
   "put it on Instagram", that is a `connect_agent_channel` call — do NOT
   scrape telegram.com or instagram.com as a knowledge source, and do NOT
   describe dashboard menus for a channel this tool connects.

## Connecting each channel
All of these are `connect_agent_channel` with the right `channel` value. Relay
the returned `instructions` faithfully — they say who may open a link and for
how long it works.

- **Telegram** (`channel`=telegram): switches on the Agent's direct-message
  link and returns it. The link is durable and shareable — ideal for a personal
  assistant. Telegram groups are the one Telegram case that needs the
  dashboard.
- **Discord** (`channel`=discord, optional `nickname`): returns a one-time
  link. The user opens it in a browser where they are signed in to Quickchat,
  picks the server, and approves. It works once, only for them, and expires in
  about 10 minutes (`expires_in_seconds` is authoritative) — if it lapses,
  mint a fresh one instead of resending the old link.
- **Instagram** (`channel`=instagram): returns a one-time link like Discord's.
  It forwards to Instagram, which always asks the user to sign in (even with a
  session already open) so they can choose the professional (business or
  creator) account the Agent should answer DMs for; they sign in with that
  account and approve message access. This cannot be completed for them: give
  them the link and tell them which account to sign in with.
- **Slack** (`channel`=slack): returns a workspace-install link. It needs no
  Quickchat sign-in and does not expire, but anyone who opens it can connect
  their own workspace to this Agent — pass it only to the intended person.
- **Email** (`channel`=email): provisions the Agent's dedicated inbound
  address, switches the channel on, and returns `inbound_address`. Mail sent
  or forwarded there is answered by the Agent; to cover an existing support
  inbox, the user forwards that inbox to this address.
- **The Agent's own MCP endpoint** (`channel`=mcp with `is_active`=true):
  lets ChatGPT, Claude or Cursor users add the Agent as a connector.
  `visibility` public means anyone with the URL, private requires signing in.
  Switching it OFF (`is_active`=false) disconnects existing users, so ask
  before doing that. Plans without the MCP channel are refused — relay the
  upgrade message rather than retrying.
- **Website widget and public chat page**: no connect call — read
  `website_widget` and `public_chat_page` from `get_deployment_info` and hand
  over `embed_snippet` (paste before `</body>`) or the page URL, plus any
  `note` saying the surface is switched off and where to enable it.

## Dashboard-only channels
WhatsApp, Facebook Messenger, Intercom, Zendesk and HubSpot are set up on the
Quickchat dashboard's Integrations page, and Shopify from the Shopify App
Store or that same page — their setup needs a Meta business login, a
page-selection step, a store install, or API credentials that must not travel
through chat. Say where they are set up, and read `channels` for whether they
already are; never present a dashboard path for the channels listed above.

## Verify and close
1. After the user says they finished a link flow, call `get_deployment_info`
   again and confirm the channel moved to `connected` before declaring
   victory. Discord and Instagram confirm within moments of approval.
2. Offer to test with `send_message_to_agent` — one call sends a real visitor
   message and returns the reply. It spends an AI credit and can fire the
   Agent's live AI Actions, so ask before sending.
3. Close with what is now live and one next step (another channel, or
   improve-agent for tuning).

## Guardrails
- Mint links on demand, one per request: connect links are throttled per user,
  so never loop `connect_agent_channel` retrying the same channel.
- If the tool answers that a channel is temporarily unavailable, relay that
  message and try again in a few minutes; do not fall back to inventing
  dashboard steps or scraping websites.
- A lapsed or refused link is replaced by calling the tool again, never by
  editing the URL.
