# dad

Development workflows for an agent that supports Markdown skills: create
issues, plan sprints, implement changes with OpenSpec, test, and open PRs.

Release: 0.1.0+6be375ddbfa1

## Install

From your target repository, use npm's `npx` to install directly from GitHub
with the [skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills@latest add https://github.com/greff-ai/dad-skills \
  --skill '*' --agent codex claude-code --copy
```

This installs all nine dad skills for Codex and Claude in the current project.
Remove either agent name to install for just one; add `--global` for a user-wide
installation. `--copy` creates independent files, not symlinks. Keep `dad` and
its wrappers installed together, and update only between active workflows.
The GitHub distribution includes the built CLI; no manual clone or dad build
is needed. Use `greff-ai/dad-skills`, not the `greff-ai/dad` source repository.

The skills locate the payload through the host's skill metadata. Set
`DAD_SKILL_DIR` to the installed `dad` directory for a custom layout.
For globally installed OpenSpec workflows, set `DAD_SKILLS_DIR` to their
containing skills directory when running setup.

## Project setup

Ask your agent:

> Setup dad in this repository using the installed `dad` skill. Follow
> `references/shared/setup.md`, resolved relative to the directory containing
> that skill's `SKILL.md`, not the repository root.

Requires Node >=20.19.0, Git, the GitHub CLI with issue/project permissions,
and OpenSpec >=1.13.0. The workflow covers OpenSpec initialization, GitHub
project selection, labels/statuses, test commands, and readiness checks.

For manual setup, complete the workflow's prerequisites, then run from the
project root (replace the payload path and project number):

```sh
node <installed-dad>/scripts/dad.mjs init --tracker github --project <number>
```

Configure the repository's actual test commands in `dad/settings.json` and
resolve reported setup failures before starting work. Follow the workflow's
remaining checks; the init command alone does not complete setup.

The work directory can be renamed. For a nested location, set `DAD_DIR`.
GitHub is the implemented tracker; other provider names are reserved stubs.

Init ignores the project ID cache and `sprint/` inside the work directory.
Sprint ledgers, mirrors, and evidence are local working records; issues and
PRs retain checkpoint state, test summaries, and published evidence. Existing
tracked sprint files are not silently removed; migrate only after preserving
their remote records. Product code and OpenSpec artifacts remain versioned.

## Workflows

Invoke these names using your agent's supported syntax:

| Skill | Result |
| --- | --- |
| dad | Route a request |
| dad-issue, dad-bug, dad-feature, dad-task | Draft and file a tracking issue |
| dad-sprint-plan | Plan or extend sprint scope |
| dad-sprint | Plan and run a sprint |
| dad-sprint-pr | Run/resume issues and open the sprint PR |
| dad-sprint-update | Address sprint feedback through new issues |

The skills use sequential delegates when supported and otherwise run in the
current context. No particular agent executable or model is required.

Every code change has an issue. Completed leaf changes are archived before
their PR merges into the container branch. Sprint PRs remain open for a human
to merge, preserving issue commits with a merge commit.
