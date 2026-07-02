<!-- Language: **English** · [한국어](README.ko.md) -->

# wbsgantt MCP

Talk to [wbsgantt](https://wbsgantt.com) from **LLM clients like Claude and ChatGPT** through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io). Build a WBS and edit nodes, predecessors (dependencies), and work dictionaries — all in a conversation.

> This repository contains **connection guides, a tool catalog, and examples only**. The server is a **Remote MCP** hosted by wbsgantt — there is nothing for you to install or run.

## ✨ What you can do

- "Create a new project WBS from these requirements" — generate a whole project
- "Show me the WBS for project ○○" — read the tree
- "Add a 'Design' phase under ○○ with three sub-tasks" — add / edit / move / delete nodes
- "Make API design start only after screen design finishes" — predecessors (FS/SS/FF/SF, Lead/Lag)
- "Fill in the definition, deliverables, and acceptance criteria for this task" — work dictionary

## 🚀 Connect in 3 steps (no install)

1. **Issue a token** — sign in to wbsgantt → profile menu (top right) → **Account settings → API tokens**. Choose a scope (*read-only* or *read + write*). The token is shown **only once at creation** — copy it.
2. **Register the connector** — add the MCP server URL and token to your LLM client.

   ```
   https://api.wbsgantt.com/mcp/
   ```
   > ⚠️ Include the **trailing `/`**.

3. **Start chatting** — ask in natural language, e.g. "show me my wbsgantt projects".

See [docs/connect.md](docs/connect.md) for per-client setup.

## 📚 Docs

| Doc | Contents |
|---|---|
| [docs/connect.md](docs/connect.md) | claude.ai · Claude Desktop · Claude Code · ChatGPT setup |
| [docs/tools.md](docs/tools.md) | Catalog of available tools (scopes, parameters) |
| [docs/troubleshooting.md](docs/troubleshooting.md) | Auth errors · tools not showing · token expiry |
| [examples/prompts.md](examples/prompts.md) | Ready-to-use prompts |
| [examples/sample-project.json](examples/sample-project.json) | Example document for project creation |
| [examples/wbs-input.schema.json](examples/wbs-input.schema.json) | Project input JSON spec (JSON Schema) |

## 🔒 Security & permissions

- The LLM acts **as the user who issued the token** — it can only see and edit the projects you can access on the web.
- Tokens **expire 90 days** after issuance; up to 10 per user. **Revoke** immediately from the account screen if a token may be leaked.
- wbsgantt rules (milestones have zero duration, dependency cycles are blocked, the 100% Rule, etc.) are enforced server-side — invalid edits are rejected with an error code and the LLM self-corrects.

## 💬 Feedback

Please open an [issue](../../issues) for bugs or feature requests.

---
Docs and examples are provided under the [MIT License](LICENSE). Use of the wbsgantt service itself is governed by the [wbsgantt.com](https://wbsgantt.com) terms.
