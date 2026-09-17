# Plan a sprint

Inputs: issue keys, a status selector, problems/document, or an existing
sprint key plus additional scope. Run in the coordinating conversation.
Start with `references/shared/bootstrap.md`; commands and bodies are in
`references/shared/cli.md` and `references/shared/issue-body.md`.

1. Gather the requested scope. View explicit issues; resolve status selectors
   to configured aliases before listing. Report truncated selection results.
   For problems/documents, draft discrete work items without creating their
   issues yet. Ask for scope when none was supplied.
2. Order work, state each item's goal and dependencies, and choose a type
   and single-/multi-branch size. Existing parents stay parents; do not adopt
   a sub-issue as an independent sprint item without resolving its parent.
   Reject duplicate keys and contradictory selections.
3. New sprint: ask for a short name (or confirm a proposed one), capture the
   current local date once, and use `dad sprint name` with that date.
   Show the resulting name and plan before creating the sprint issue.
   Existing sprint: resolve its bindings through
   `references/sprint/bindings.md`; retain its name and creation date.
4. New sprint: create its canonical sprint body and title through files,
   type sprint. Record Name/Created and mark unfiled items needs issue.
   Existing sprint: append a complete scope/plan block for new items,
   preserving prior scope and omitting duplicate additions.
5. Relate existing member issues to the sprint with `dad issue link`.
   Sprint membership is not parentage. Mirror the sprint field when supported;
   missing values warn, while other failures propagate. Do not create children
   for needs-issue rows yet; container planning does that after scope approval.
6. Report sprint issue URL, recorded name/date, planned items, and warnings.
   Stop unless called by `references/sprint/run.md`.

Replanning an open sprint PR can add larger scope here. Focused changes or
review replies use `references/sprint/update.md`. No code or branch edits
are needed just to plan.
