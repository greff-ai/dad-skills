# Run work to a PR

Input: a sprint key, issue keys, a status selector, a description, or none.
Start with `references/shared/bootstrap.md` and view supplied keys.

- None: a branch matching one open sprint per `references/sprint/bindings.md`
  runs `references/sprint/pr.md`. A branch whose parsed key has a latest
  checkpoint with parent MAIN_BRANCH runs `references/pr/issue.md`.
  Otherwise ask what to work on and route the answer.
- One sprint key runs `references/sprint/pr.md`; with added scope, first
  extend it via `references/sprint/plan.md`. Several sprint keys: QUESTION.
- One other key runs `references/pr/issue.md`.
- Several keys or a selector: `references/sprint/plan.md`, then
  `references/sprint/pr.md` with the resulting sprint key.
- A description: decide whether it is one work item or several, asking once
  if unclear, and show that choice in the draft or plan. One item:
  `references/issue/create.md`, then `references/pr/issue.md` with the filed
  key. Several: plan and run as above.

Planning questions and approvals still apply; continue from an approved plan
or filed issue without another confirmation.
