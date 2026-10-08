---
name: launch-agent
description: >-
  Configure a Quickchat AI Agent from scratch, end to end: set up the empty
  Agent the account already has (or provision one when every Agent is already
  configured), build it from a website URL or from a short interview when there
  is no site, then show the generated configuration and how to test it. Use when
  the user asks to create, build, launch, set up, or spin up a new Agent, to
  make an Agent from a website or URL, or when they have just connected
  Quickchat and have nothing set up yet. Not for editing an established Agent
  (use improve-agent).
metadata:
  author: quickchat-ai
---

# Launch a new Agent

Stand up a working Agent in one guided flow. Most people running this have just
connected Quickchat and already have an empty Agent, so configure THAT one and
build something before you report anything.

## Before you start
1. Call `list_scenarios`. Connecting Quickchat already created an Agent, so if any
   entry has `configured: false` AND a `role` of EDITOR or above, use that
   `scenario_id` for everything below and do NOT call `create_assistant` (creating
   another leaves theirs blank forever). Ignore unconfigured Agents you only have
   VIEWER on: they are someone else's and onboarding them will fail. Only when
   every Agent you can edit is already configured, or the user asked for an extra
   one, call `create_assistant` with confirm=true and use the scenario_id it
   returns. If more than one Agent qualifies, ask which to set up.
2. Ask whether they have a website to build from. That answer picks the path below.
3. Their Agent is on the FREE tier (no charge, no card); say so if you create one.

## Path A — they have a website
1. Use the scenario_id from "Before you start". Set the display name with
   `update_assistant_settings` if they gave one.
2. Call `onboard_assistant_from_url` with that scenario_id, the URL and
   confirm=true — all three are required, and the call is rejected without the
   confirm. Say out loud that this takes 20-60 seconds while it scrapes the
   site, writes the persona and embeds the content — silence here is where
   people give up. It OVERWRITES persona and settings and adds the scraped
   content to any existing knowledge, so run it on an Agent that holds nothing
   yet; on an already-configured Agent, ask first.
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

Then configure their Agent in a single `update_assistant_settings` call on the
scenario_id from "Before you start". Do this on every no-website run, including
when you had to create the Agent: creation happens before this interview, so it
cannot have carried answers you had not collected yet, and skipping the write
leaves the new Agent blank.
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
Never ask for an API key, token or password in this chat. If the API needs one,
create the action without it and have the user add the credential to the action
in the Quickchat dashboard (Actions page), then test it there.

## Close by making it real
1. `get_deployment_info` — give them the widget embed snippet and the public chat
   link.
2. Tell them they can talk to it right now at
   `https://app.quickchat.ai/i/<scenario_id>/ai-preview`.
3. Offer one concrete next step (add knowledge, tune the persona via
   improve-agent).

## Guardrails
- Configure the Agent the account already has. Create at most ONE Agent per
  request, only when none is unconfigured or the user asked; never loop
  `create_assistant`. If its result carries a `warning` naming an Agent that
  is still blank, tell the user and offer to configure that one instead.
- If initial settings are rejected, the Agent still exists — adjust and apply with
  `update_assistant_settings` on the returned scenario_id; do NOT call
  `create_assistant` again.
- If a configuration call returns an error, re-read with `get_assistant_settings`
  before retrying; the write may already have landed.

## Output
Confirm the Agent's name and scenario_id, summarize the configuration you set,
and give the preview link plus 1-2 next steps.
