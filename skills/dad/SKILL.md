---
name: dad
description: Initialize projects and run tracked development through issues, sprint branches, tests, and pull requests.
---

Route the user's requested work using the table. Ask only for missing intent,
issue type, or sprint identity; sufficient context can go straight to a flow.
Issue creation and sprint updates include draft approval. Once approved, run
the selected workflow through completion or a recorded halt.

| Request | Entry point | Reference |
| --- | --- | --- |
| Initialize a project for dad | dad | `references/shared/setup.md` |
| File an issue; choose its type | /dad-issue | `references/issue/create.md` |
| Report a defect | /dad-bug | `references/issue/create-bug.md` |
| Request user-visible behavior | /dad-feature | `references/issue/create-feature.md` |
| Track internal work | /dad-task | `references/issue/create-task.md` |
| Plan and run a sprint | /dad-sprint | `references/sprint/run.md` |
| Plan or extend sprint scope | /dad-sprint-plan | `references/sprint/plan.md` |
| Run or resume a planned sprint | /dad-sprint-pr | `references/sprint/pr.md` |
| Address sprint feedback | /dad-sprint-update | `references/sprint/update.md` |

Names identify workflows; use the host's supported skill invocation syntax.
Read only the selected reference. Setup establishes prerequisites; other flows
use the shared bootstrap before operations.
Every code change has an issue; sprint PRs stop for human merge.
