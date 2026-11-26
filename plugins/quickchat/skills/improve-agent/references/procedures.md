# Procedures

A Procedure is a set of instructions the Agent follows only while it applies to the conversation. Situation-specific flows move out of the main prompt and guidelines, so every reply carries only what its conversation needs.

## Where an instruction belongs

- Main prompt (`short_description`): who the business is and what the Agent is for. Keep it short.
- Guidelines (`ai_commands`): rules for every conversation, such as tone, what never to promise and when to hand off.
- Knowledge base: facts the Agent looks up, such as policies, prices and opening hours.
- Procedure: the steps for one situation, such as a return, a cancellation, a damaged order or a VIP customer. A block in the main prompt or guidelines that starts with "If the customer wants to..." is usually a procedure. Propose moving it, and remove it from the main prompt or guidelines in the same approved change.

## How the Agent uses them

- `ai_decides`: until a procedure is open, the AI sees only its name and when_to_use. When the conversation matches, the AI opens it and follows its instructions for the rest of that conversation, so a flow that runs over several messages stays in force. It can open several in one conversation.
- `when_metadata_matches`: no AI decision. The procedure is active whenever all of its run_conditions match the conversation's metadata. They are checked again before every reply, so it switches on and off as the metadata changes.
- While a procedure is active, its steps win where they conflict with the main prompt or guidelines.
- Only procedures with is_active true are used. Each holds up to 10,000 characters, and the procedures active in one conversation share 40,000 characters, filled in the order the procedures were created: one that does not fit is left out, so keep them focused.
- Edits apply from the next reply, in ongoing conversations too.

## Writing one

- name: the situation, e.g. "Return or exchange". It is unique per Agent and the AI reads it.
- when_to_use: one sentence about what the customer says or wants, e.g. "The customer wants to return or exchange an item they received." A vague line such as "returns" opens it too often or too rarely. Keep procedures from overlapping, because two that match the same message compete.
- instructions: numbered steps that stand on their own: what to ask for, what to check, what to tell the customer, when to hand off and what never to offer. Name the facts the steps rely on, such as the return window, or point to the knowledge base for them. Do not repeat rules the guidelines already set.
- Metadata procedures: take keys from `list_observed_metadata_keys`. Use is_truthy or is_falsy for flags, equals for one exact text value, is_one_of for alternatives, contains for a substring, and greater_than, greater_or_equal, less_than or less_or_equal for numbers, with the number as text such as "100". A missing value never matches equals, contains, is_one_of or a numeric comparison but passes not_equals and is_falsy, so add exists when presence matters. Gate anything permission-sensitive on metadata the website or an Action sets, never on something a visitor claims in the chat.

## Changing them safely

- Read the procedure with `get_procedures` and its procedure_id before an edit: the list carries only a preview, and new instructions replace the stored ones in full.
- Propose each new or changed procedure as a before and after, and save, switch off or delete it with `manage_procedure` only after the user approves; delete also needs confirm=true and the exact expected_name.
- To check an ai_decides procedure, offer a test with `send_message_to_agent` using a message that should open it. A test spends AI credits and can run active AI Actions, so confirm with the user first. A test message cannot set metadata, so a when_metadata_matches procedure switches on in a test only if its conditions match the test conversation's own metadata: conditions that accept a missing key (not_exists, is_falsy, not_equals) match without any, and the others need an Action to save the value during the test.
