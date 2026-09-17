# Execute a container

Inputs: bootstrap facts and resolved bindings.
Read `references/shared/ledger.md`, `references/shared/cli.md`, and
`references/shared/subagents.md`. Run rows sequentially in ledger order.

1. Require a clean tree on the container branch; fetch origin and reconcile
   it by fast-forward. A verified local descendant may be pushed to recover a
   prior failed product/OpenSpec push. Divergence halts CONFLICT. Restore and
   validate ledger identity and required keys before dispatch.
2. Recover running rows and recorded merged PRs using the shared ledger's
   evidence checks. Convert an interrupted, unmerged row to halted while
   preserving its checkpoint. Do not infer a merge from an archive alone.
3. Walk each unchecked row once this run. Validate state and size; malformed
   values halt that row without dispatch, then continue. Read dependencies
   from the ledger's current state, including rows merged earlier this pass.
   If any are not done, mark blocked by those keys, retaining the old halt
   and checkpoint, publish changed state, and continue.
4. View the issue with comments. A missing/unreadable issue halts this row.
   Make the child directory, refresh issue.md preserving local audit additions,
   and record running. Publish the container checkpoint before branching.
5. Dispatch a single-branch row to
   `references/issue/single-branch/pr.md`; dispatch a multi-branch row to
   `references/issue/multi-branch/bindings.md` only when permitted.
   Supply facts, issue key, row branch and dir, return/container branch, and
   the container's parent branch. For a multi-branch child, its own container
   branch is the row branch and its parent is this container.
   A retry also receives the exact halt, last completed token (none = empty),
   and known PR. Delegate when supported; otherwise run in this context.
6. Check clean tree and expected return branch before recording anything.
   Failure of either stops orchestration, because the next row shares this
   tree. An ordinary child halt only halts its row.
7. Copy sub-step, PR, archive, warnings, and halt from the shared report.
   Confirm a reported success with merged PR evidence before marking done.
   When the PR is already merged, finish missing bookkeeping through the
   shared recovery path even if the child halted afterward.
8. Publish changed ledger state to the container issue and refresh local
   mirrors; then continue to the next independent row. Do not immediately
   retry a halted row in the same pass.
9. Report processed, merged, halted, and blocked counts with reasons.
   Return to the caller for `references/container/merge.md`; unfinished rows
   remain available for the next run.
