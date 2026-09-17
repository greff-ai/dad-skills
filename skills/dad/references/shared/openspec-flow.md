# OpenSpec

Read `paths.openspec` through the CLI and resolve it from the repository root.
Use the host's installed OpenSpec skill or command for the named workflow;
do not assume a command prefix or installation directory.

| Workflow | Contract |
| --- | --- |
| explore | Read-only investigation; return evidence and a draft. |
| propose | Seed from issue.md; create proposal, design, specs, and tasks; stop. |
| apply | Separate invocation implementing the change's tasks. |
| verify | Read-only comparison of implementation against current artifacts. |
| archive | Run on the leaf branch after the merge suite passes; sync, then archive. |

The leaf branch's parsed slug is its change name. Refuse collisions with an
existing active or archived change for a fresh row. Missing root/workflow:
`MISSING` with setup instructions; do not install it mid-flow.

Use `references/shared/subagents.md` for worker boundaries. Supply the issue
and committed files, not only the proposal. Unanswered planning questions
return `QUESTION`; do not implement an invented answer.

After fixes, reconcile proposal, design, specs, and tasks with the actual
result, then verify. Allow at most three fix-and-retry rounds after the initial
verification. Record warnings; unresolved critical findings produce
`VERIFY_FAILED`.

Archive in the leaf's current context. Select sync before archive, or archive
when already synced. Incomplete tasks/artifacts or a sync failure halt the
merge. Confirm the active directory is gone and exactly one matching archive
contains proposal.md, design.md, and tasks.md. Containers archive no change of
their own.
