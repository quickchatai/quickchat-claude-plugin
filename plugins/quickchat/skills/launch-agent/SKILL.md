---
name: launch-agent
description: >-
  Create and configure a brand-new Quickchat AI Agent from scratch, end to end:
  provision it, configure it from a website URL or from a short interview when
  there is no site, then show the generated configuration and how to test it. Use
  when the user asks to create, build, launch, set up, or spin up a new Agent, to
  make an Agent from a website or URL, or when they have just connected Quickchat
  and have nothing set up yet. Not for editing an established Agent (use
  improve-agent).
metadata:
  author: quickchat-ai
  version: "2.0"
---

# Launch a new Agent

Stand up a working Agent in one guided flow. Most people running this have just
connected Quickchat and have an empty Agent, so build something before you report
anything.

## Before you start
- Ask whether they have a website to build from. That answer picks the path below.
- Explain this creates a real Agent on the FREE tier (no charge, no card).

## Path A — they have a website
1. Call `create_assistant` with confirm=true (and `name` if given). Creation is
   guarded by confirm=true and capped at 5/hour. Tell the user the new
   `scenario_id`.
2. Call `onboard_assistant_from_url` with that scenario_id and the URL. Say out
   loud that this takes 20-60 seconds while it scrapes the site, writes the
   persona and embeds the content — silence here is where people give up. It
   OVERWRITES configuration, so run it only on the freshly created Agent, never
   an established one.
3. Poll `get_assistant_settings` until `onboarding_from_url_completed` is true.
   If it reports `onboarding_progress_tracked` false there is no completion
   signal — read the settings once after ~60s instead.
4. Show what was generated: the name, the main prompt, the guidelines and the
   language it picked.

If the URL is rejected as a placeholder or marketplace domain, ask for their own
business website rather than retrying the same URL.

## Path B — no website
Do NOT create an empty Agent and stop. Interview first, in one short round of
questions:
- What does the business do, and what is it called?
- Who will be talking to the Agent (customers, members, players, staff)?
- What tone should it take?
- What are the top 3 questions it must answer?

Then create a configured Agent in a single call — `create_assistant` accepts the
settings inline:
- `name` and `one_word_description` — what the Agent is called
- `short_description` — the main prompt: role, what it helps with, what it must
  not guess at
- `ai_commands` — one short rule per item, from their must-answer questions
- `greeting` — the first line a visitor sees
- `language_chosen` — the language they answered in

Then offer `add_knowledge_base_article` for any facts they can give you now
(hours, pricing, policies). Content is embedded automatically; there is no
retrain step.

## Optional — connect a system
If they name something the Agent should reach (an order lookup, a booking system,
an internal API), offer `create_http_request_action`, then
`test_http_request_action` to prove it works. It takes no confirm argument. Skip
this unless they raise it, and never invent an endpoint.

## Close by making it real
1. `get_deployment_info` — give them the widget embed snippet and the public chat
   link.
2. Tell them they can talk to it right now at
   `https://app.quickchat.ai/i/<scenario_id>/ai-preview`.
3. Offer one concrete next step (add knowledge, tune the persona via
   improve-agent).

## Guardrails
- Create ONE Agent per request unless the user explicitly asks for more; never
  loop `create_assistant`.
- If initial settings are rejected, the Agent still exists — adjust and apply with
  `update_assistant_settings` on the returned scenario_id; do NOT call
  `create_assistant` again.
- If a configuration call returns an error, re-read with `get_assistant_settings`
  before retrying; the write may already have landed.

## Output
Confirm the Agent's name and scenario_id, summarize the configuration you set,
and give the preview link plus 1-2 next steps.
