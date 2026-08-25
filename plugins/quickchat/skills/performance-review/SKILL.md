---
name: performance-review
description: >-
  Produce a performance review of a Quickchat AI Agent: pull conversation
  volume, resolution rate, CSAT, handoff response time, and topic trends over a
  period and turn them into a prioritized set of recommendations. Use when the
  user asks how an Agent is doing, for a weekly or monthly review, a health
  check, KPIs, or to compare one period against another. Do NOT use for reading
  individual conversations (use conversation-triage) or changing settings (use
  improve-agent).
metadata:
  author: quickchat-ai
  version: "1.0"
---

# Performance review

Turn a Quickchat AI Agent's analytics into a short, decision-ready review.

## Before you start
- Call `list_scenarios` to resolve which Agent the user means. Exactly one Agent
  -> use it; otherwise ask which one.
- Confirm the period. Default: the last 7 days vs the 7 days before it.

## Steps
1. Use `compare_periods` to compare the current period with the previous one of
   equal length. Pass the PREVIOUS (older) window as `period_a` and the CURRENT
   window as `period_b`: deltas are computed as period_b minus period_a, so this
   makes a positive delta mean "up versus the previous period." Returns both
   overviews plus per-metric deltas.
2. Use `get_topics` for the customer-intent split, and read `topics_by_day` from
   the overview for free-text themes that are rising.
3. Use `get_csat` for satisfaction and `get_ttfr` for human handoff speed (only
   if the Agent hands off).
4. For any count, rate, total, or trend, ALWAYS use the analytics tools above.
   Never page through `list_conversations` to compute an aggregate.

## Read the numbers correctly
- `resolution_rate` already combines confirmed and assumed resolutions; report it
  as a percentage of conversations.
- `get_csat` empty means no CSAT was received, NOT a low score — say "no CSAT
  data" rather than implying dissatisfaction.
- `get_ttfr` measures HUMAN responders after a handoff, in seconds, not
  business-hours adjusted; prefer the median and note overnight gaps inflate the
  average.
- See `references/metrics-glossary.md` for the full field reference.

## Output
1. One-line headline: better or worse, and why.
2. A compact table: this period vs last, with deltas, for volume, resolution
   rate, CSAT, and handoffs.
3. Rising topics worth attention.
4. 2-3 concrete, prioritized recommendations.
Keep it scannable; lead with the metric that moved most.
