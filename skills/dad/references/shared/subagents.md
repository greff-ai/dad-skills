# Delegation

Use sequential delegates for context isolation when the host supports them.
When delegation or further nesting is unavailable, run the same steps in the
current context, with the same inputs, checks, and return report. Do not require
a particular model, product, agent name, or tool.

| Logical level | Work |
| --- | --- |
| Coordinator | Sprint ledger |
| Child | Leaf issue or multi-branch issue's ledger |
| Grandchild | Sub-issue leaf |
| Worker | Explore, propose, apply, test, or verify |

The deepest delegated path needs three levels below the coordinator. Flatten
work into the current context when the host's limit is smaller. The
test -> fix -> test-fix chain always stays in one context.

A worker prompt includes bootstrap facts, exact workflow/reference and
arguments, owned paths, seed issue, expected return branch, permitted edits,
and this report:

```text
files written: <paths or none>
result: pass | fail
question: <only if unanswered by the supplied issue>
halt: <CODE>: <reason, only on failure; last line>
```

Workers read actual code and current artifacts; report disagreements.
Return captured evidence paths with captions and test summaries; only the
coordinator publishes them. Keep sprint records and evidence ignored. Commit
product/OpenSpec changes, never local ledgers or generated review artifacts.
Planning and verification are separate from implementation. Read-only workers
make no commits. Editing workers commit their own scoped changes on the given
branch before returning; a halted child preserves work on its own branch and
returns to its container branch. No concurrent agents share the working tree.

The caller checks cleanliness and expected branch before recording a report
or starting another worker. A dirty or wrong-branch return stops orchestration;
never sweep leftovers into a container commit. For read-only exploration on a
dirty tree, compare HEAD, index/worktree diff, and untracked file contents
against a pre-run snapshot; preserve all pre-existing edits.

User questions and draft approval stay in the coordinating conversation.
Container full-suite workers use `fix: no`: code repairs need an issue branch.
