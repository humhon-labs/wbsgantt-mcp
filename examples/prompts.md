Language: **English** · [한국어](prompts.ko.md)

# Ready-to-use prompts

After connecting, just ask in natural language — the LLM picks the right tool.

## Explore

- "Show me my wbsgantt projects"
- "Summarize the WBS tree for project ○○"
- "Which tasks are on the critical path in project ○○?"

## Create a whole project

- "Build a WBS for a 3-month mobile app launch with Planning, Design, Development, QA, and Launch phases — 3–5 sub-tasks each."
- "Create a new project from these requirements: …" (paste requirements)
- Example input document: [sample-project.json](sample-project.json)

## Edit nodes

- "Add a 'DB design' task under the 'Design' phase"
- "Rename 'Screen design' to 'Screen design v2' and set its duration to 5 days"
- "Move the 'Implementation' node under the 'Development' phase"
- "Turn 'Kickoff' into a milestone"

## Predecessors (dependencies)

- "Make API design start only after screen design finishes" (FS)
- "Start frontend and backend together, but backend 2 days later" (SS +2d)
- "Show the dependencies in project ○○"
- "Remove the 'Design → Development' dependency"

## Work dictionary

- "Fill in the definition, deliverables, and acceptance criteria for 'Requirements definition'"
- "Add 'wireframe' and 'mockup' as deliverables to this dictionary"
- "Set the acceptance criteria for 'API design' to 'PM and lead approval'"

## Progress

- "Update the progress of 'Screen design' to 40%" (enter on leaf tasks; parents roll up automatically)

---

> Tip: if the LLM breaks a rule (zero-duration milestones, no cycles, the 100% Rule, etc.), the server returns an error and the LLM fixes it and retries. For hard-to-undo actions like deletion, confirm the target first.
