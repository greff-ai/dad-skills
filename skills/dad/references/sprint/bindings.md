# Sprint bindings

Resolve these once in the coordinating conversation after
`references/shared/bootstrap.md`. Commands: `references/shared/cli.md`.

1. Use an explicit sprint key when supplied. Otherwise read the current
   branch and list open sprint issues, matching their recorded name's slug
   with `sprint.branchPrefix`. Require exactly one match; a truncated search
   cannot establish uniqueness, so request an explicit key.
2. View the issue with comments. Require type sprint and an open issue.
   Read Name and Created from its Sprint section. Require a valid local
   YYYY-MM-DD date, nonempty name, and title equal to name. Do not rerender
   today's template or rename a running sprint.
3. Slugify the recorded name with `dad branch slug`; branch =
   sprint.branchPrefix + slug; SPRINT_DIR = WORK_DIR/sprint/slug,
   stored repo-relative. A disagreeing existing ledger is CONFLICT.

| Binding | Value |
| --- | --- |
| kind | sprint |
| container issue | resolved sprint issue |
| container branch | prefix + recorded name's slug |
| parent branch | MAIN_BRANCH |
| container dir | SPRINT_DIR |
| child link kind | related |
| child record in container issue | complete appended scope block |
| children may be containers | yes |
| dependency default | none |
| autoMerge | false |
| mirror sprint field | yes |
| may ask the user | yes |
| merge instructions | yes |
| container pr line | Sprint PR: |

Required settings: sprint.branchPrefix, issueTypes.sprint.label.
The sprint issue and published ledger checkpoints are authoritative; local
files are working copies and the board sprint field is a convenience mirror.
