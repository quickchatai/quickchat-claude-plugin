---
name: conversation-triage
description: >-
  Find and prioritize the customer conversations a teammate should follow up on:
  ones that are still unresolved and ones the AI escalated for the team to
  review. Read a sample, summarize what happened, note any that got a negative
  rating, and suggest a next step for each. Use when the user asks what
  needs a reply, which chats to review, what to follow up on, or to sort the
  support inbox. Do NOT use for aggregate stats (use performance-review) or to
  change the Agent (use improve-agent).
metadata:
  author: quickchat-ai
  version: "1.0"
---

# Conversation triage

Surface the customer conversations a teammate should follow up on, and make each
one actionable.

## Before you start
- Call `list_scenarios` to resolve the Agent. One Agent -> use it; else ask.
- Default window: the last 7 days.

## Steps
1. Use `get_insights` with insight_type="flagged" for the conversations the AI
   escalated for the team to review. NOTE: this tool is NOT date-filtered — it
   returns the most recent matches across all time, so filter by timestamp
   yourself for the window and page with `next_cursor` if needed.
2. Use `list_conversations` with `resolution_status="open"` plus
   `start_date`/`end_date` for the window to find open conversations.
3. Open `get_conversation_detail` on the most important few to read the
   transcript, see what happened, and note any negative rating the customer left.
4. Cross-reference: a conversation that is both escalated and unresolved is the
   top priority.

## Guardrails
- This connection can READ conversations but cannot resolve, reassign, or change
  the status of a conversation over MCP. Never claim to have changed a
  conversation's status — recommend the next step and tell the user to take it in
  the Quickchat Inbox.

## Output
A prioritized list, most important first. For each: the customer's request in one
line, why it is on the list (unresolved or escalated for review), any negative
rating it received, and a suggested next step. End with the single top item to
handle first.
