# dad

Turn ideas into tested code, ready to review. Tell your agent:

```text
/dad create a sprint pr with implementation of <description of issues or features>
```

Dad creates issues, implements and tests each change, and brings completed work
together on a sprint branch. You get a pull request to review and merge.

## Features

- Bugs, features, and tasks tracked in GitHub issues.
- Sprint planning and execution with resumable progress.
- OpenSpec proposals, implementation, and verification.
- Pull requests with test summaries and supporting evidence.
- Review feedback turned into tracked follow-up work.

## Example

[Flappy Bird with Three.js](https://github.com/greff-ai/benchmark-flappy-bird)
started with one command:

```text
@dad-sprint create a flappy bird game with three.js
```

See the implementation in [sprint PR #18](https://github.com/greff-ai/benchmark-flappy-bird/pull/18).

## Dad Skills

Invoke individual skills for finer control of the workflow, including reviewing
created issues before implementation. Use your agent's supported invocation syntax.

| Skill | Result |
| --- | --- |
| dad | Route your request |
| dad-issue, dad-bug, dad-feature, dad-task | Draft and file issues for review |
| dad-sprint | Plan and run a sprint pr in one request |
| dad-sprint-plan | Plan or extend a sprint before implementation |
| dad-sprint-pr | Run or resume a planned sprint and open its PR |
| dad-sprint-update | Address sprint review feedback through new issues |

## Install

From your repository:

```sh
npx skills@latest add https://github.com/greff-ai/dad-skills \
  --skill '*' --agent codex claude-code --copy
```

Installs all nine skills for Codex and Claude Code, with the CLI included.
Keep the skills together and update between workflows. Add `--global` for a
user-wide install.

## Project setup

```text
/dad setup this repository
```

Requires Node >=20.19.0, Git, an authenticated GitHub CLI with issue/project
permissions, and OpenSpec >=1.13.0. Dad guides project configuration, test
commands, and readiness checks.

Release: 0.1.0+c59204bbca25
