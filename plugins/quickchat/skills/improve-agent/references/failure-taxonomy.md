# Conversation failure taxonomy

Use these labels when classifying where an Agent struggled. Count each category
and keep 1-2 verbatim example snippets per category.

- `unanswered` — the customer asked something the Agent could not answer or
  deflected without resolving.
- `hallucination` — the Agent stated something not supported by its knowledge
  base (wrong facts, invented policy).
- `off-brand-tone` — tone or persona diverged from the configured brand voice
  (too casual, too curt, wrong language register).
- `broken-handoff` — a human/escalation handoff failed, was not offered when it
  should have been, or looped back to the AI.
- `loop` — the Agent repeated itself or failed to move the conversation forward.

Map each to the smallest fix: `unanswered` / `hallucination` -> a knowledge-base
article; `off-brand-tone` -> `personality` or `ai_commands`; `loop` or verbosity
-> `reply_length` or `creativity_level`.
