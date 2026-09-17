# CLI contracts

Use these forms when a flow names a command. Substitute placeholders as argv
values, with proper shell quoting for the resolved executable path and every
argument. Never evaluate tracker text as shell code. Create temporary title,
body, and reply files through file-writing tools; preserve their text exactly.
Use `--title-file` instead of `--title`, and `--body-file` instead of
`--text`. Omit optional filters unless the flow calls for them.

All reads use JSON. Every failure has `error.{code,name,message,details}`;
handling defaults to `references/shared/bootstrap.md`. Keys are opaque strings.
List results may be truncated: report that limit, and require an explicit key
when a complete search is needed to establish identity or uniqueness.

## Settings

```bash
node <dad-skill-dir>/scripts/dad.mjs init --tracker github --project <number> --json
```
Consumes: `ok`, `checks`, `dadDir`, `settingsFile`, `capabilities`;
on failure `error.details` contains the setup report.
Only for explicit setup. Optional `--project-owner <login>` selects the board
owner; `--issues-repo <owner/repo>` selects a separate issue repository.
Reruns preserve existing settings rather than replacing them with flag values.

```bash
node <dad-skill-dir>/scripts/dad.mjs config get git.mainBranch --explain --json
```
Consumes: `value`, `dadDir`, `resolvedBy`, `targetFile`.

```bash
node <dad-skill-dir>/scripts/dad.mjs config get <path> --json
```
Consumes: `value`, `source`.

```bash
node <dad-skill-dir>/scripts/dad.mjs tracker capabilities --json
```
Consumes: `sprintField.present`, `sprintField.kind`, `subIssues`, `columns`.

## Issues

```bash
node <dad-skill-dir>/scripts/dad.mjs issue view <key> --comments --json
```
Consumes: `key`, `url`, `title`, `state`, `type`, `column`, `sprint`,
`parent`, `body`, `comments[].{author,createdAt,body,url}`.
Omit `--comments` when no mirror is needed.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue list --state <state> --json
```
Consumes: `issues[].{key,url,title,state,type,column,sprint,parent}`, `count`,
`truncated`. Optional filters: `--type <type>`, `--status <alias>`,
`--sprint <name>`, `--parent <key>`. State is open, closed, or all.
Use view to establish parentage; an unfiltered list may report parent as null.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue create --type <type> --title-file <title-file> --body-file <body-file> --json
```
Consumes: `issue.{key,url,title,column}`, `column`, `parentLink`;
on failure `error.details`.
Optional `--column <alias>` overrides the configured default.
Optional `--parent <key>` creates a sub-issue. If a partial failure supplies
the created issue key, recover using that issue; never blindly create again.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue move <key> <alias> --json
```
Consumes: `key`, `column`, `previous`.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue append <key> --body-file <body-file> --json
```
Consumes: `key`, `appended`.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue comment <key> --body-file <body-file> --json
```
Consumes: `key`, `url`. Use for durable checkpoint snapshots and test details.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue link <key> --related <target> --json
```
Consumes: `key`, `kind`, `target`, `mode`, `changed`.

```bash
node <dad-skill-dir>/scripts/dad.mjs issue link <key> --parent <parent> --json
```
Consumes: `key`, `kind`, `target`, `mode`, `changed`.
The CLI owns native/fallback relationships; never synthesize Parent lines.

## Branches and sprints

```bash
node <dad-skill-dir>/scripts/dad.mjs branch slug <text> --json
```
Consumes: `slug`.

```bash
node <dad-skill-dir>/scripts/dad.mjs branch name --type <type> --key <key> --slug <slug> --json
```
Consumes: `branch`.

```bash
node <dad-skill-dir>/scripts/dad.mjs branch parse <branch> --json
```
Consumes: `type`, `key`, `slug`.

```bash
node <dad-skill-dir>/scripts/dad.mjs sprint name --short-name <short-name> --date <YYYY-MM-DD> --json
```
Consumes: `name`, `slug`, `branch`, `date`.

```bash
node <dad-skill-dir>/scripts/dad.mjs sprint ensure-branch <name> --json
```
Consumes: `branch`, `created`, `pushed`.
Ensures the branch on origin without moving HEAD; checkout is a separate step.

```bash
node <dad-skill-dir>/scripts/dad.mjs sprint set <key> <name> --json
```
Consumes: `key`, `sprint`.
Only when sprintField.present. A missing option or iteration (NOT_FOUND), or
absent field (UNSUPPORTED), is a warning; never create field values.
Other failures propagate. The sprint issue and ledger own membership.

## Pull requests

Native GitHub media upload is used only after the guarded dad PR operation:

```bash
gh pr edit <pr> --repo <owner/repo> --attach <image-or-video>
```
Requires an attachment-capable CLI and write access. Repeat `--attach` for
multiple supported files. For images, an optional `#<alt text>` suffix belongs
in the same quoted attachment argument; visible captions are added separately.
The command adds uploaded URLs
to the existing body. Read back with the PR view contract below and preserve
those URLs in future body updates. See `references/shared/evidence.md`.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr open --head <head> --base <base> --title-file <title-file> --body-file <body-file> --issue <key> --json
```
Consumes: `number`, `url`, `created`; on failure
`error.details.base`, `error.details.number`.
An existing head with a different base returns FAILURE with those details:
report CONFLICT. Every call supplies the tracking issue.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr view <pr> --json
```
Consumes: `number`, `url`, `state`, `base`, `head`, `body`, `mergeCommit`.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr append <pr> --body-file <body-file> --json
```
Consumes: `number`, `appended`.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr merge <pr> --delete-branch --json
```
Consumes: `number`, `url`, `merged`, `alreadyMerged`, `mergeCommit`,
`branchDeleted`.
Squash only; refuses the configured main branch. Never bypass a forge refusal.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr threads <pr> --json
```
Consumes: `number`, `threads[].{id,path,line,resolved,outdated,comments}`.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr reply <thread-id> --body-file <reply-file> --json
```
Consumes: `threadId`, `comment.url`.

```bash
node <dad-skill-dir>/scripts/dad.mjs pr resolve <thread-id> --json
```
Consumes: `threadId`, `resolved`.
