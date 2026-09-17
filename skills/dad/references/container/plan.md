# Plan a container

Inputs: bootstrap facts, resolved bindings, and optional proposed scope.
Use `references/shared/cli.md`, `references/shared/issue-body.md`, and
`references/shared/ledger.md`. Stay in the caller's context.

1. Require a clean tree on the container branch. Load the container issue
   with comments. Read defaults.newIssueColumn, paths.openspec, and each
   planned type's label and releaseNotes setting.
2. Gather scope from tables with the exact five-column header
   `Issue | Title | Type | Size | Status`, including appended planning blocks.
   A task's ordinary Scope heading is not a scope table. Read plan entries in
   body order, with later entries winning. When no scope table exists, use the
   supplied decomposition; if neither exists, return QUESTION.
3. Restore the ledger from issue checkpoints, or create its header for fresh
   work per the shared ledger. Validate identity
   and existing rows. Reconcile by issue key, preserving all existing branches,
   directories, dependencies, and completion history. Report removed scope
   without deleting rows. Include newly related issues or Sub-issues entries
   from the container body; do not use board membership as the authority.
4. Before creating anything, validate all new items' types, sizes, goals, and
   dependency references. Use the container Goal, or Summary when no Goal
   exists, to seed child descriptions. A child draft must satisfy its type's
   required sections. Ask the user when allowed; otherwise return QUESTION.
   Do not quietly omit an unfileable milestone and merge the remaining scope.
5. Existing issue: view it, check type and parentage, and reuse it.
   Missing issue: create it using separate title/body files and its type.
   For parent bindings pass the parent key at creation. Retain a returned
   issue key on any partial failure and recover that issue; never blindly
   repeat creation.
6. Link each issue using the binding's related/parent form. Do not reparent an
   existing child. Immediately append its scope/sub-issue record to the
   container issue, including a resolution for a matching needs-issue item.
   On retries, resolve an unkeyed planned item against those records first;
   ambiguous matches halt CONFLICT instead of filing again.
7. Mirror the sprint field only when the binding and capabilities permit;
   missing values warn per the CLI contract. Refresh issue.md from the tracker
   while retaining local-only audit additions.
8. For each new key, choose a three-word kebab summary of its intent, then
   use `dad branch name`. Refuse branch/change-slug collisions and choose a
   distinct meaningful slug before recording it. Derive dir by replacing the
   branch's slashes with hyphens below container-dir/issues.
9. Resolve dependencies: explicit none = none; after refs = those earlier
   keys; absent entry = the binding default (none or previous row). Reject
   unknown/forward references and cycles. Never treat explicit none as omitted.
10. Size comes from the scope decision, then the issue recommendation.
    If absent, use single-branch and record a warning. Unknown values halt
    CONFLICT. A binding with children-may-be-containers no rejects multi-branch
    children; it never silently selects a leaf for them.
11. Append complete keyed rows to the local ledger. Mirror the container issue
    and publish its checkpoint before execution; do not commit sprint files.
    Report added/retained rows, links, and warnings. A planning
    failure prevents execution/merge of an incomplete decomposition.
