---
name: improve-agent
description: >-
  Audit a Quickchat AI Agent and improve it: read its current settings and
  knowledge base, mine recent conversations for gaps (unanswered questions,
  wrong answers, off-brand tone, missing knowledge), then propose specific
  configuration edits and knowledge-base additions. Apply changes only after the
  user approves each one. Use when the user asks to improve, tune, optimize,
  fix, or update an Agent, add knowledge, or change its persona, greeting,
  language, or profile picture.
metadata:
  author: quickchat-ai
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
2. `list_knowledge_base_articles` — what the Agent already knows. `content` here
   is a preview, not the article; `get_knowledge_base_article` returns the body.
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
  immediately, no retrain. `ai_commands` REPLACES every existing guideline, so
  read the current list first and send it back with the change. The list is
  capped at 10,000 characters in total: near the cap, merge or shorten
  guidelines rather than appending one more, which can only be rejected.
- `add_knowledge_base_article` — content required; embedded automatically in the
  background, no retrain.
- `update_knowledge_base_article` — replaces the article's WHOLE body, so read it
  with `get_knowledge_base_article` first and send the full edited text.
  `previous_title` + `previous_content` restore the previous title and
  body, unless `previous_content_truncated` is true.
- `delete_knowledge_base_article` — irreversible; read the article first, since
  the response echoes only the first 20,000 characters for recovery. It requires
  `expected_title` and `expected_added_at` from
  `list_knowledge_base_articles` (they fail the call if the article changed
  underneath you) plus confirm=true.
- `set_assistant_avatar` — profile picture from an image URL, a base64 data URI
  (png/jpg/gif/webp, max 5 MB), or `use_default=true` to reset. It permanently
  replaces the previous image and the legacy launcher icon, and needs a plan
  that allows widget customization. It returns the resulting `avatar_url` —
  there is no settings field to re-read it from.
- After each other write, re-read with `get_assistant_settings` /
  `list_knowledge_base_articles` and confirm the change landed.

## Guardrails
- Never call a write tool before the user approves that specific change.
- Never build an edit out of a preview — correcting one line still means sending
  the whole article, and a preview sent back silently drops the rest.
- `personality` and similar fields are enum ids — use the ids from the tool
  schema, not free text.
- Some fields (e.g. `ai_commands`) need a plan feature; if a write is rejected for
  that reason, report it plainly instead of retrying.

## Output
A short report: top issues (ranked, one example each), the proposed changes as a
diff, and — if you applied any — a confirmation of what changed.
