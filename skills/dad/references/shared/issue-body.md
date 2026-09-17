# Issue bodies

Keep summaries short; detailed implementation records belong in change
artifacts. Write titles and bodies through files using `references/shared/cli.md`.

## Ordinary issue

Use these sections in order:

1. `## Summary`: what and why, 2-5 sentences. If
   `issueTypes.<type>.releaseNotes` is false, mark it internal.
2. Type-specific sections: bug = `Reproduction`, `Expected`, `Actual`;
   feature = `User story`, `Acceptance criteria`;
   task = `Scope`, `Definition of done`.
3. `## Implementation sketch`: a short triage-level approach.
4. `## Size`: exactly one checked choice, then a brief reason.
5. `## Links`: relevant references, one per line.

```markdown
## Size
- [x] single-branch
- [ ] multi-branch
```

A multi-branch recommendation needs independently testable milestones.
Planning may override it. Do not invent reproduction steps or acceptance facts.

## Append-only history

Use `dad issue append`, checking for the same addition before retrying.
Use `dad issue link` for relationships, never hand-author Parent/Related
metadata. Keep the ignored local issue.md mirror current; publish audit details
to the issue/PR rather than leaving the only copy in local files.

Leaves append working branch, deviations (including none), archived artifact
links, and `Merged in PR <url>`. Multi-branch issues append working branch,
`## Sub-issues` entries (`- <ref> — <title>`), and `Issue PR: <url>`.
Sprints append working branch and `Sprint PR: <url>`.
Do not force-add ignored sprint files or silently untrack legacy directories.

## Sprint body

Title = the recorded sprint name. Sections:
`Goal`, `Scope`, `Plan`, `Out of scope`, `Sprint`.

```markdown
## Scope
| Issue | Title | Type | Size | Status |
| --- | --- | --- | --- | --- |
| <ref or needs issue> | <title> | <type> | <size> | <column alias> |

## Plan
1. <ref or planned title>: <goal>; none
2. <ref or planned title>: <goal>; after <ref>

## Sprint
Name: <rendered name>
Created: <local YYYY-MM-DD>
```

The first table cell is always the issue reference. Escape pipes/newlines in
cells. Keep the name/date unchanged on re-planning. Append additions under
`## Added after planning — <local date>`, with a complete five-column table
and the new items' plan entries. Never append a bare table row or rewrite the
original scope. Read plan entries in body order; the last entry for an item
wins. Explicit `none` means no dependency; a missing entry uses the binding
default.
