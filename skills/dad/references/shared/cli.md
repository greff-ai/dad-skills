# CLI contracts

Use these forms when a flow names a command. Substitute placeholders as argv
values, shell-quoting the resolved executable path and every argument. Never
evaluate tracker text as shell code. Create temporary title, body, and reply
files with file-writing tools; preserve their text exactly. Use
`--title-file`, not `--title`, and `--body-file`, not `--text`. Omit optional
filters unless the flow calls for them.

All reads use JSON. Every failure has `error.{code,name,message,details}`;
handling defaults to `references/shared/bootstrap.md`. Keys are opaque strings.
List results may be truncated: report that limit, and require an explicit key
when identity or uniqueness needs a complete search.

## Settings

```
node <dad-skill-dir>/scripts/dad.mjs init --tracker github --project <number> --json
```
Consumes: `ok`, `checks`, `dadDir`, `settingsFile`, `capabilities`;
on failure `error.details` holds the setup report.
Only for explicit setup. Optional `--project-owner <login>` selects the board
owner; `--issues-repo <owner/repo>` selects a separate issue repository.
For Linear, pass `--tracker linear --team <team>` in place of the GitHub
flags; optional `--cycle current` adds sprint work to the team's current
cycle. Reruns keep existing settings; a disagreeing flag is refused.

```
node <dad-skill-dir>/scripts/dad.mjs config get git.mainBranch --explain --json
```
Consumes: `value`, `dadDir`, `resolvedBy`, `targetFile`.

```
node <dad-skill-dir>/scripts/dad.mjs config get <path> --json
```
Consumes: `value`, `source`.

```
node <dad-skill-dir>/scripts/dad.mjs tracker capabilities --json
```
Consumes: `sprintField.present`, `sprintField.kind`, `subIssues`, `columns`.

## Issues

```
node <dad-skill-dir>/scripts/dad.mjs issue view <key> --comments --json
```
Consumes: `key`, `url`, `title`, `state`, `type`, `column`, `sprint`,
`parent`, `body`, `comments[].{author,createdAt,body,url}`.
Omit `--comments` when no mirror is needed.

```
node <dad-skill-dir>/scripts/dad.mjs issue list --state <state> --json
```
Consumes: `issues[].{key,url,title,state,type,column,sprint,parent}`, `count`,
`truncated`. Optional filters: `--type <type>`, `--status <alias>`,
`--sprint <name>`, `--parent <key>`. State is open, closed, or all.
Use view to establish parentage; an unfiltered list may report parent as null.

```
node <dad-skill-dir>/scripts/dad.mjs issue create --type <type> --title-file <title-file> --body-file <body-file> --json
```
Consumes: `issue.{key,url,title,column}`, `column`, `parentLink`;
on failure `error.details`.
Optional `--column <alias>` overrides the configured default; `--parent <key>`
creates a sub-issue. If a partial failure supplies the created issue key,
recover using that issue; never blindly create again.

```
node <dad-skill-dir>/scripts/dad.mjs issue move <key> <alias> --json
```
Consumes: `key`, `column`, `previous`.

```
node <dad-skill-dir>/scripts/dad.mjs issue append <key> --body-file <body-file> --json
```
Consumes: `key`, `appended`.

```
node <dad-skill-dir>/scripts/dad.mjs issue comment <key> --body-file <body-file> --json
```
Consumes: `key`, `url`. Use for durable checkpoint snapshots and test details.

```
node <dad-skill-dir>/scripts/dad.mjs issue link <key> --related <target> --json
```
Consumes: `key`, `kind`, `target`, `mode`, `changed`.

```
node <dad-skill-dir>/scripts/dad.mjs issue link <key> --parent <parent> --json
```
Consumes: `key`, `kind`, `target`, `mode`, `changed`.
The CLI owns native/fallback relationships; never synthesize Parent lines.

## Branches and sprints

```
node <dad-skill-dir>/scripts/dad.mjs branch slug <text> --json
```
Consumes: `slug`.

```
node <dad-skill-dir>/scripts/dad.mjs branch name --type <type> --key <key> --slug <slug> --json
```
Consumes: `branch`.

```
node <dad-skill-dir>/scripts/dad.mjs branch parse <branch> --json
```
Consumes: `type`, `key`, `slug`.

```
node <dad-skill-dir>/scripts/dad.mjs sprint name --short-name <short-name> --date <YYYY-MM-DD> --json
```
Consumes: `name`, `slug`, `branch`, `date`.

```
node <dad-skill-dir>/scripts/dad.mjs sprint ensure-branch <name> --json
```
Consumes: `branch`, `created`, `pushed`.
Ensures the branch on origin without moving HEAD; check out separately.

```
node <dad-skill-dir>/scripts/dad.mjs sprint set <key> <name> --json
```
Consumes: `key`, `sprint`.
Only when sprintField.present. A missing option or iteration (NOT_FOUND), or
absent field (UNSUPPORTED), is a warning; never create field values.
Other failures propagate. The sprint issue and ledger own membership.

## Pull requests

Use native GitHub media upload only after the guarded dad PR operation:

```
gh pr edit <pr> --repo <owner/repo> --attach <image-or-video>
```
Requires an attachment-capable CLI and write access. Repeat `--attach` per
supported file. For images, an optional `#<alt text>` suffix goes in the same
quoted argument; visible captions are added separately. Uploaded URLs are
added to the existing body: read back with the PR view contract below and
preserve them in later body updates. See `references/shared/evidence.md`.

```
node <dad-skill-dir>/scripts/dad.mjs pr open --head <head> --base <base> --title-file <title-file> --body-file <body-file> --issue <key> --json
```
Consumes: `number`, `url`, `created`; on failure
`error.details.base`, `error.details.number`.
An existing head with a different base returns FAILURE with those details:
report CONFLICT. Every call supplies the tracking issue.

```
node <dad-skill-dir>/scripts/dad.mjs pr view <pr> --json
```
Consumes: `number`, `url`, `state`, `base`, `head`, `body`, `mergeCommit`.

```
node <dad-skill-dir>/scripts/dad.mjs pr append <pr> --body-file <body-file> --json
```
Consumes: `number`, `appended`.

```
node <dad-skill-dir>/scripts/dad.mjs pr merge <pr> --delete-branch --json
```
Consumes: `number`, `url`, `merged`, `alreadyMerged`, `mergeCommit`,
`branchDeleted`.
Squash only; refuses the configured main branch. Never bypass a forge refusal.

```
node <dad-skill-dir>/scripts/dad.mjs pr threads <pr> --json
```
Consumes: `number`, `threads[].{id,path,line,resolved,outdated,comments}`.

```
node <dad-skill-dir>/scripts/dad.mjs pr reply <thread-id> --body-file <reply-file> --json
```
Consumes: `threadId`, `comment.url`.

```
node <dad-skill-dir>/scripts/dad.mjs pr resolve <thread-id> --json
```
Consumes: `threadId`, `resolved`.
