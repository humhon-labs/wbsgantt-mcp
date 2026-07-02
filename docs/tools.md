<!-- Language: **English** · [한국어](tools.ko.md) -->

# Tool catalog

On connect, the LLM automatically receives this tool list along with each tool's description and input schema. The table below is a human-readable summary. The **scope** is chosen when you issue a token — `read` (read-only) or `write` (read + write).

> ⚠️ This table is maintained by hand. The tool descriptions the client receives are the source of truth for exact behavior and fields.

## Read (`read`)

| Tool | Description | Key parameters |
|---|---|---|
| `list_projects` | Projects you can access | — |
| `get_wbs_tree` | Full WBS tree (per node: id, code, name, type, schedule, progress, etag) | `project_id`, `max_depth` (optional) |
| `list_wbs_dependencies` | All predecessors (dependencies) in a project | `project_id` |
| `get_wbs_dictionary` | A node's work dictionary (returns an empty one if none) | `node_id` |

## Write (`write`)

| Tool | Description | Key parameters |
|---|---|---|
| `import_wbs_project` | Create a new project + WBS from one document | `document` (→ [input spec](../examples/wbs-input.schema.json)) |
| `create_wbs_node` | Create one child node under an existing node | `parent_id`, `name`, `is_milestone`, `duration_days` |
| `update_wbs_node` | Field-level node update | `node_id`, `fields` (name, weight, progress, is_milestone, duration_days, start_planned, schedule_mode), `expected_etag` (optional) |
| `move_wbs_node` | Move a node (with its subtree) to another parent/position | `node_id`, `new_parent_id`, `before_id`/`after_id` (optional), `expected_etag` (optional) |
| `delete_wbs_node` | Delete a node + its subtree (soft delete) | `node_id`, `expected_etag` (optional) |
| `add_wbs_dependency` | Create a predecessor link between two nodes | `predecessor_id`, `successor_id`, `dep_type` (fs/ss/ff/sf), `lag_days` |
| `remove_wbs_dependency` | Delete one dependency | `dependency_id` |
| `set_wbs_dictionary` | Field-level dictionary update | `node_id`, `fields` (definition, deliverables, acceptance_criteria, assumptions, constraints, effort_estimate_hours, cost_estimate_amount, cost_currency, references), `expected_etag` (optional) |

## Domain rules (enforced server-side)

Calls that violate these rules are rejected with an error code; the LLM reads the error and self-corrects.

| Rule | Error code (example) |
|---|---|
| Milestones have zero duration | `WBS_MILESTONE_INVARIANT` |
| No dependency cycles | `DEPENDENCY_CYCLE_DETECTED` |
| No ancestor↔descendant dependency | `DEPENDENCY_ANCESTOR_DESCENDANT` |
| No self-dependency | `DEPENDENCY_SELF_LOOP` |
| No duplicate dependency | `DEPENDENCY_DUPLICATE` |
| Root node cannot be deleted | `WBS_ROOT_NOT_DELETABLE` |
| No access to non-member projects | `PROJECT_FORBIDDEN` |
| Insufficient token scope (write tool with a read token) | `PAT_SCOPE_INSUFFICIENT` |

> Adding or removing a dependency triggers an automatic schedule (CPM critical path) recompute server-side (reflected within seconds).

## ETag (optional conflict guard)

`get_*` responses include an `etag`. If you pass `expected_etag` to a write tool and the item changed in the meantime, the server returns `WBS_ETAG_MISMATCH` with the latest state (omit it to apply against the current state immediately). For most conversational use you can omit it.
