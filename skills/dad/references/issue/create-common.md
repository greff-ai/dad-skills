# Draft and file an issue

Inputs: type, original context, mode `file` (default) or `draft`, and an
optional required size. Draft mode returns a complete draft without filing.
It is reused by sprint updates. No branch changes or implementation.

1. Run `references/shared/bootstrap.md`. Read
   `issueTypes.<type>.label`, `defaults.newIssueColumn`, `paths.openspec`
   (required), and `issueTypes.<type>.releaseNotes` (optional, default true).
   An explicitly requested column must resolve to a configured alias.
2. Explore the supplied context with the installed OpenSpec explore workflow
   using `references/shared/openspec-flow.md` and
   `references/shared/subagents.md`. Supply the type's required sections.
   Investigation is read-only, including on a dirty tree; verify the baseline
   afterward. Return a proposed title, summary, evidence, missing information,
   and implementation sketch.
3. Refuse a thin draft. Require a real title and summary plus the type's
   required sections from `references/shared/issue-body.md`. Ask targeted
   questions, incorporate the answers, and investigate again as needed.
   In a child context, return QUESTION to the coordinator. Do not file
   assumptions as facts.
4. Recommend single-branch unless there are concrete milestones whose suites
   can each pass. A forced single-branch request that needs decomposition
   returns OUT_OF_SCOPE pointing to sprint planning.
5. Format the body with `references/shared/issue-body.md`. Draft mode returns
   title, body, type, size, column, and relevant links here.
6. In file mode show title, body, type, size, and target column. Wait for
   approval; edits repeat the draft, a type change repeats validation, cancel
   stops. Existing parent requests are shown explicitly; use a parent link
   only when the user authorized a sub-issue.
7. Write the approved title and body to separate temporary files with a
   file-writing tool. Use `dad issue create` in `references/shared/cli.md`,
   with title-file/body-file, the chosen type, and a column override only when
   explicitly requested. Never auto-advance to ready.
8. If creation failed after returning an issue key, retain that key and report
   the unfinished operation. For a move failure, retry the move once on that
   issue. For a parent-link failure, recover the link on that issue. Other
   failures stop; never create a duplicate as recovery.
9. Report the issue URL and column, size, internal status, and any unfinished
   bookkeeping. The user may file issues from any branch.
