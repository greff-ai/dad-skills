# Address a sprint change

Input: user instructions, or unresolved review threads, plus an optional
explicit sprint key. Run in the coordinating conversation.

1. Bootstrap via `references/shared/bootstrap.md` and resolve
   `references/sprint/bindings.md`. Restore missing local records using
   `references/shared/ledger.md`. Require a clean tree, valid ledger, and an
   open PR whose head/base match the sprint.
   Read its number from the ledger; never manufacture a PR to discover it.
   Use command forms in `references/shared/cli.md`.
2. Prefer supplied instructions. Otherwise use `dad pr threads`, preserving
   thread IDs and mapping comments to requested changes. No instructions and
   no actionable threads: report that there is no change to process.
3. Split independent requests. Use only the classification step of
   `references/issue/create.md`, then draft through
   `references/issue/create-common.md` with mode draft and size single-branch.
   Do not file yet. Scope needing a multi-branch decomposition returns
   OUT_OF_SCOPE pointing to sprint planning for this sprint.
4. Show the drafts, original related issues, and thread-to-issue mapping;
   wait for approval. Record any threads the user wants left open.
   On retry, first reuse existing update issues and ledger entries identified
   by the request/thread URLs; ambiguous matches require clarification.
5. Checkout/fast-forward the sprint branch. File approved issues using title
   and body files. Save each returned key before any further write; partial
   failures resume that issue. Relate each to the sprint and any original
   issue it changes. Append a canonical scope block and plan entries to the
   sprint issue, including source thread URLs for future recovery.
6. Mirror the sprint field when supported and refresh issue.md. Run
   `references/container/plan.md` to append ledger rows, preserving existing
   ones. Do not write code on the sprint branch.
7. Run `references/container/execute.md` for all runnable rows, then
   `references/container/merge.md` to test and refresh the sprint PR.
   The new issues use the same leaf flow as ordinary sprint work.
8. For each addressed review thread, require every mapped issue's row done
   and confirmed merged. Reply with issue and leaf PR links using a reply
   file; skip an identical existing reply. Resolve only those threads, except
   the ones the user asked to leave open. Halted or partially addressed
   requests leave their threads open.
9. Report new/reused issues, leaf PRs, sprint PR, suite result, and unresolved
   work. The sprint PR remains for human merge.
