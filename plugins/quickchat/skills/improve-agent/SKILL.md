---
name: improve-agent
description: >-
  Audit a Quickchat AI Agent and improve it: read its current settings and
  knowledge base, mine recent conversations for gaps (unanswered questions, wrong
  answers, off-brand tone, missing knowledge), then propose specific
  configuration edits and knowledge-base additions. Apply changes only after the
  user approves each one. Use when the user asks to improve, tune, optimize, fix,
  or update an Agent, add knowledge, or change its persona, greeting, or language.
metadata:
  author: quickchat-ai
  version: "1.0"
---

# Improve an Agent

Read first, change second. Diagnose from real data, propose the smallest fix, and
never write without explicit approval.

## Before you start
- Call `list_scenarios` to resolve the Agent. Config tools require EDITOR-or-above
  on that Agent; if the user lacks it, say so and stop.
- Ask whether to PROPOSE changes only (default) or also APPLY approved ones.

## Diagnose
1. `get_assistant_settings` — persona, profession, creativity, greeting, language,
   reply length, KB descriptions.
2. `list_knowledge_base_articles` — what the Agent already knows.
3. `get_insights` plus a `list_conversations` sample read with
   `get_conversation_detail` to find where it struggled. Classify with
   `references/failure-taxonomy.md`.

## Propose
For each recurring problem, propose the SMALLEST change that fixes it:
- Missing knowledge -> a new KB article (draft the exact text).
- Wrong tone/persona -> a specific `personality` (an enum id, not a scale) or an
  `ai_commands` guideline.
- Too long / short / robotic -> `reply_length` or `creativity_level`.
Present proposals as a before -> after diff, then STOP for approval.

## Apply (only after approval)
- `update_assistant_settings` — pass ONLY the fields that change; effective
  immediately, no retrain.
- `add_knowledge_base_article` — content required; embedded automatically in the
  background, no retrain.
- After each write, re-read with `get_assistant_settings` /
  `list_knowledge_base_articles` and confirm the change landed.

## Guardrails
- Never call a write tool before the user approves that specific change.
- `personality` and similar fields are enum ids — use the ids from the tool
  schema, not free text.
- Some fields (e.g. `ai_commands`) need a plan feature; if a write is rejected for
  that reason, report it plainly instead of retrying.

## Output
A short report: top issues (ranked, one example each), the proposed changes as a
diff, and — if you applied any — a confirmation of what changed.
