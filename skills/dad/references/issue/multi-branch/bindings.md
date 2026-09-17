# Run a multi-branch issue

A child of a sprint, with only single-branch sub-issues.
Inputs: facts, issue, row branch/ISSUE_DIR, sprint parent branch, and optional
halt, resume token, and PR. Use `references/shared/cli.md` and
`references/shared/ledger.md`.

## Bindings

| Binding | Value |
| --- | --- |
| kind | issue |
| container issue | supplied large issue |
| container branch | supplied row branch |
| parent branch | supplied sprint branch |
| container dir | ISSUE_DIR |
| child link kind | parent |
| child record in container issue | Sub-issues line |
| children may be containers | no |
| dependency default | previous row |
| autoMerge | true |
| mirror sprint field | no |
| may ask the user | no |
| merge instructions | no |
| container pr line | Issue PR: |

## Flow

1. Require clean tree on the sprint parent, not MAIN_BRANCH. Parse the row
   branch and view the issue: key/type must agree. Require the issue to have
   no parent (this is the only decomposition level); refresh its local mirror.
   Check a known PR for merged/closed state before any branch creation.
2. A merged PR uses shared ledger recovery, even if its branch was deleted.
   Fresh work rejects existing branch/ledger collisions. Otherwise create
   from origin's parent using `git switch --no-track -c <child> origin/<parent>`;
   publish with `git push -u origin <child>`, never the parent's upstream.
   On retry, checkout and fast-forward the existing
   branch. Divergence halts. Resume tokens plan/execute/merge-test require the
   restored nested checkpoint; merge requires confirmed PR evidence and skips branch work.
3. Move the issue to active; append the working branch once and refresh its
   local mirror. Publish checkpoints per the shared ledger. Do not create an
   OpenSpec change for the container itself.
4. Read the current scope tables and Sub-issues records before decomposing.
   Reuse existing children and preserve ledger rows. When decomposition is
   needed, identify independently testable milestones, with user-visible
   increments typed feature, internal work task, and defects bug.
   All-task milestones are a reason to reconsider a feature's decomposition.
   Every child is single-branch and needs a complete typed issue body.
   Use the container's Goal, falling back to Summary.
5. Produce the canonical five-column scope and ordered plan with an explicit
   goal per item. Missing dependency entries default to the previous row;
   explicit none makes a milestone independent. Append the proposal once to
   the container issue before filing children. Unanswerable questions return
   QUESTION; never file an incomplete child or drop a milestone.
6. Run `references/container/plan.md`, then
   `references/container/execute.md`, then
   `references/container/merge.md` with these bindings, in this context.
   Record plan, execute, merge-test, merge, done as each completes.
   Replanning on retry is idempotent; retain all prior child identities.
7. On success return the shared child report on the sprint branch, with PR
   and the all-done checkpoint comment URL as nested evidence. On halt publish
   the checkpoint and preserve local records, then return clean to the sprint.
   Preserve the last token, known PR, and exact halt. A partial issue does not
   auto-merge; its parent sprint may still continue other rows.
