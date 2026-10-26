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
| Run a sprint, issues, or one issue to a PR | /dad-pr | `references/pr/run.md` |
| Plan or extend sprint scope | /dad-sprint-plan | `references/sprint/plan.md` |
| Address PR feedback | /dad-pr-update | `references/pr/update.md` |

Names identify workflows; use the host's supported skill invocation syntax.
Read only the selected reference. Setup establishes prerequisites; other flows
use the shared bootstrap before operations.
Every code change has an issue; PRs to the main branch stop for human merge.
