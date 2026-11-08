# Build and verify an Action workflow

Aim for a useful, correct result. Carry out the user's authorized creation or editing request, preserving any limits they gave. A missing optional detail is a reason to state a reasonable assumption or leave an owner setup placeholder, not to abandon a documented useful integration. Ask for a critical missing contract or decision only when it prevents the next step; continue independent steps while waiting. Never ask for an API key, token or password in this chat. The owner configures credentials in the Quickchat dashboard.

For a clear standalone utility such as public web search, weather or geocoding, choosing a documented provider and explaining that choice can produce a useful draft without another clarification. This does not authorize guessing the user's CRM, payment processor or private business API. Prefer a suitable existing Action before choosing another service, and leave any account setup to the owner.

## Inspect before creating

1. Resolve the Agent with `list_scenarios`. Call `list_ai_actions` with include_configurations=true to inspect the full existing configurations, including disabled Actions. Reuse a suitable lookup or write instead of creating a duplicate; preserve unrelated fields when editing. Read individual details with `get_ai_action` and action_id when only one Action matters. Never use a displayed mask, including an HTTP secret header mask, as a new literal value. When editing a complete Remote MCP headers list, echo each unchanged mask under its same unique credential header name to preserve that stored credential. Omit untouched masked fields or collections otherwise; make credential changes in the dashboard.
2. Call `list_observed_metadata_keys` with include_producers=true. Check each needed key's producer Action, input or response path, enabled state and execution mode. Configured memory with zero observations can be a valid future handoff, but an inactive producer, disabled MCP tool or missing prerequisite must be addressed. An observed key does not mean every visitor has it.
3. Identify the actual source of an authorization gate. Channel/backend-authored role or eligibility data can enforce a gate when that integration guarantees its source. A visitor field, AI argument, or input capture does not prove authorization. An API response capture proves only which configured result supplies the value; verify the endpoint's meaning before treating it as permission.
4. Map the goal into HTTP requests or available MCP tools, their inputs, required outputs and success evidence. Inspect a remote server's available tools before using it via `list_remote_mcp_server_tools`. Prefer supplied contracts and official documentation; do not invent endpoints, response fields or metadata producers. Distinguish illustrative payload values from fixed requirements: when the user asks for actual values, sample IDs or counts must not become a permanent restriction or a locked default.

## Configure every useful capability

Consider each option for the goal; leave it empty or at its default when it adds no value.

| Capability | Use it when it improves the workflow |
| --- | --- |
| Description and parameters | Explain when to call, what to collect, any requested confirmation, prerequisites and how to use the response. Keep known constants fixed and lock defaults that the AI must not change. |
| Execution mode | Use ai_decides for conversational requests, before_first_reply for requested initial enrichment, or when_metadata_matches for a once-per-conversation silent side effect after a turn. Automatic modes cannot ask for inputs. |
| Run conditions | Enforce specified eligibility and prerequisites using known metadata. Conditions are ANDed; is_one_of supplies alternatives for one key. Use is_truthy for boolean flags. Do not invent a permission flag or replace an unsupported cross-key OR with AND. |
| Response filters | Expose the documented branches needed to answer, retaining business-error indicators. Empty filters expose the full response. Filtering cannot make an unknown response contract reliable. |
| Response memory | Save a useful scalar ID/status from the original successful response for a later step, even if filters hide it from the AI. Only the first JSONPath match counts; objects, lists and null are not saved. |
| Input memory | Retain collected scalar inputs, including supported Inbox contact fields, when useful. A declared input can be used only in a memory path such as $.name without being sent to the API. Paths use normalized parameter names. Capture happens before HTTP and is not evidence that the remote operation succeeded. |
| Progress text | Give a short helpful message for visible work, without claiming success early. Use empty text for silent work. |
| Remote MCP and knowledge scope | Enable the tools needed for the goal, configure their thinking text and memory, and use a Knowledge Base tag when the search should stay within a topic. |

Use `create_http_request_action` for new HTTP steps and `update_http_request_action` with action_id for edits. Use `manage_ai_action` operation=save for Remote MCP, Knowledge Base or Shopify configuration; this save does not need confirm, while operation=delete requires confirm=true. Read back before editing replacement lists and retain every item still needed. Valid GET Actions can activate automatically; other newly created Actions stay inactive until activated.

## Complete the supported steps

One HTTP Action sends one request. Create every missing supported step needed by the user's goal and reuse the suitable ones already present. For example, a customer lookup can capture its returned scalar ID as crm_customer_id; a ticket Action can use {{metadata_crm_customer_id}}, require that key to exist, and describe when to call the lookup first. The downstream gate prevents execution without the prerequisite; descriptions alone do not enforce ordering.

Check timing as well as key names. Before-first-reply Actions run oldest first and cannot depend on a later conversational call. Metadata-triggered Actions run after the reply and their output cannot answer the current question. A capture can be absent after a failed request or a non-scalar result. Do not call a previously observed key a guaranteed prerequisite, or claim that an input capture proves a paid call succeeded.

If a required step cannot yet be configured, build the reliable useful part when requested and identify the remaining Action, its required contract/input, and what the workflow still cannot do. Do not describe a pricing-only Action as checking availability too, or a membership lookup as enforcing access to every reply. If an existing Action already completes the task, report that and configure only the missing connection or behavior.

## Test, inspect and iterate

1. Read back saved configuration and validation results. Resolve setup placeholders and credentials through the dashboard. Before a live test, identify the expected business result, response evidence, permitted inputs and side effects. Existing user authorization applies; obtain it for any test or effect outside that scope. Never execute customer Actions merely to inspect their configuration.
2. Run `test_http_request_action` when authorized. Check outcome/test_passed and then inspect the response against the expected business result, such as a returned ticket ID or the requested order's actual status. HTTP 200 alone does not establish business success; a response can contain an application error or an empty wrong-account result. An isolated test uses simulated metadata and does not save conversation memory.
3. For a multi-step workflow, use an authorized test conversation with `send_message_to_agent` and inspect `get_ai_action_calls` plus conversation diagnostics. Verify that the intended Action ran, the producer supplied the correct value, the consumer used it, the gate rejects the disallowed case, filters retain the answer, and the reply reflects the real result. Agent tests use credits and can execute active Actions. A mock value proves neither a live metadata source nor the complete workflow.
4. Fix the specific mismatch and rerun the relevant check. Check the prior outcome before retrying a write that may already have succeeded. If live verification is blocked, distinguish configuration verified from behavior not yet tested and name the exact prerequisite.

Finish with two or three friendly sentences: what now works, how it meets the goal, and any concrete setup or remaining step. Keep schema details in the configuration and do not claim an untested draft is a working complete workflow.
