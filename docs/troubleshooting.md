<!-- Language: **English** · [한국어](troubleshooting.ko.md) -->

# Troubleshooting

## I connected but get an auth error (401)

1. **Trailing `/`** — the URL must end with a slash: `https://api.wbsgantt.com/mcp/`.
2. **Token copy** — paste the full string starting with `bwic_pat_`.
3. **Header format** — `Authorization: Bearer <token>` (one space after `Bearer`).
4. **Expired / revoked** — tokens expire 90 days after issuance. Check the status under Account settings → API tokens; if expired or revoked, issue a new one and re-register. (Error codes: `PAT_EXPIRED` / `PAT_INVALID`.)

## Newly added tools don't show up

LLM clients usually re-fetch the tool list **when a new conversation starts**. If new tools (e.g. dependency/dictionary editing) don't appear after a server update:

- **Start a new conversation.** (Simplest.)
- If they still don't appear, **toggle the connector off/on (reconnect)** or restart the client.
- **No token reissue is needed.**

Check by asking in a new conversation: *"What wbsgantt tools can I use right now?"*

## The LLM can read but not edit

Your token was issued with **read-only** scope (error code: `PAT_SCOPE_INSUFFICIENT`). Issue a new token with **read + write** scope and register it.

## My dependency link is rejected

- `DEPENDENCY_CYCLE_DETECTED` — it forms a cycle. Reverse the direction or link different tasks.
- `DEPENDENCY_ANCESTOR_DESCENDANT` — parent/child (ancestor–descendant) nodes cannot be linked.
- `DEPENDENCY_DUPLICATE` — the same dependency already exists.

## Dates don't update immediately

After changing dependencies or durations, the critical-path (CPM) recompute runs **asynchronously**. It usually reflects within seconds — re-check with `get_wbs_tree` shortly after.

## Anything else

If it's still not resolved, open an [issue](../../issues) with your client type and the error message (never include the token value).
