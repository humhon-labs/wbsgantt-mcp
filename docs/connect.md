<!-- Language: **English** · [한국어](connect.ko.md) -->

# Connecting from each client

The MCP server URL is the same everywhere (**trailing `/` required**):

```
https://api.wbsgantt.com/mcp/
```

Authenticate by passing your API token (`bwic_pat_...`) as a **Bearer token**. See the [README](../README.md#-connect-in-3-steps-no-install) for how to issue one.

---

## claude.ai / Claude Desktop

1. Settings → **Connectors** → **Add custom connector**.
2. Enter:
   - **URL**: `https://api.wbsgantt.com/mcp/`
   - **Authorization** header: `Bearer <your-token>` (or paste the token into the token field)
3. Save. The tools appear in a new conversation.

## Claude Code

In a terminal:

```bash
claude mcp add --transport http wbsgantt https://api.wbsgantt.com/mcp/ \
  --header "Authorization: Bearer <your-token>"
```

Verify:

```bash
claude mcp list
```

## ChatGPT (custom connector)

1. Add a **custom connector / tool** in settings.
2. **MCP Server URL**: `https://api.wbsgantt.com/mcp/`
3. Auth: `Authorization: Bearer <your-token>`

---

## Verify the connection

In a new conversation, ask:

> "What wbsgantt tools can I use right now?"

If you see `list_projects`, `get_wbs_tree`, `add_wbs_dependency`, `set_wbs_dictionary`, etc., you are connected.
If the list is stale or tools are missing, reconnect the connector — see [troubleshooting](troubleshooting.md).

> Note: you only need to re-register when the token expires (90 days) or is revoked. No routine reconfiguration is required.
