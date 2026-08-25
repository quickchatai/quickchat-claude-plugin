# Analytics field glossary

Reference for the fields returned by `get_analytics_overview` and the related
analytics tools. Load this when you need to explain or report a specific metric.

## Volume
- `total_conversations` — conversations in the range.
- `total_messages` — messages across those conversations.
- `avg_messages_per_conversation` — mean messages per conversation.
- `messages_used_this_billing_period` — messages counted against the plan limit
  (billing-period-to-date, not the selected range).

## Resolution
- `total_resolved` — confirmed plus assumed resolutions.
- `total_confirmed_resolutions` — the customer or agent confirmed resolution.
- `total_assumed_resolutions` — inferred resolved (e.g. the customer stopped).
- `resolution_rate` — `total_resolved` as a percentage of conversations.
- `resolution_by_day` — daily resolution breakdown for trend lines.

## Handoffs
- `total_handoffs` — conversations handed to a human.

## Themes & sentiment
- `topics_by_day` — free-text topic labels per day (use for "what's trending").
- sentiment — aggregate sentiment signal over the range.

## Related tools
- `get_topics` — customer INTENT split into 7 fixed buckets (purchase, support,
  inquiry, complaint, feedback, subscription, other). Different from the
  free-text `topics_by_day`.
- `get_csat` — external help-desk CSAT (1-5). Empty = none received, NOT low.
- `get_ratings` — in-chat widget thumbs/emoji, raw counts, no average.
- `get_ttfr` — human first-reply time after handoff, in seconds; prefer median.
