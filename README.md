# dad

Dan's Agentic Development (DAD) is an agentic SDLC built on top of [OpenSpec](https://github.com/Fission-AI/OpenSpec)

```text
/dad create a sprint pr with implementation of <description of issues or features>
```

Dad creates issues, implements and tests each change, and brings completed work
together on a sprint branch. You get a pull request to review and merge.

## Example

[Flappy Bird with Three.js](https://github.com/greff-ai/benchmark-flappy-bird)
started with one command:

```text
@dad-sprint create a flappy bird game with three.js
```

See the implementation in [sprint PR #18](https://github.com/greff-ai/benchmark-flappy-bird/pull/18).

## Features

- Every change has a GitHub or Linear issue and a GitHub PR for a clear audit trail.
- Choose automatic execution with a final PR for review, or run steps individually
  to review and refine issues and sprint plans before implementation.
- Supports large bugs or features that require multiple PRs
- Specs maintained by your agent using the standard [OpenSpec](https://github.com/Fission-AI/OpenSpec) workflow.
- Pull requests with test summaries and supporting evidence.
- Easily update sprints with follow-up work discovered during review or testing.

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

## Update

Between workflows, run the install command again from your project to refresh
only the Dad skills from this GitHub repository:

```sh
npx skills@latest add https://github.com/greff-ai/dad-skills \
  --skill '*' --agent codex claude-code --copy
```

Add `--global` if you originally installed Dad user-wide. The `skills update`
command filters by skill name or install scope, not by source repository.

## Project setup

```text
/dad setup this repository
```

Requires Node >=20.19.0, Git, an authenticated GitHub CLI with issue/project
permissions, and OpenSpec >=1.13.0. Dad guides project configuration, test
commands, and readiness checks.

Issues can be tracked in GitHub or Linear. Linear also needs an exported
`LINEAR_API_KEY`, the team's key or name, and the Bug, Feature, Task, and
Sprint labels created in that team (Dad reports missing labels but does not
create them). GitHub remains the code host for branches and pull requests.
Linear support can be checked against a throwaway team with the gated live run
in the source repository.

## Switch trackers

A project uses one tracker at a time, chosen by `tracker.provider` in
`dad/settings.json`. Setup never rewrites an existing settings file, so switch
by editing it, between sprints:

1. Finish or close any open sprint. Issues, sprint issues, and checkpoints are
   not copied between trackers.
2. Replace the `tracker` block. The `forge` block stays as it is.

   For Linear:

   ```json
   "tracker": {
     "provider": "linear",
     "linear": { "team": "ENG" },
     "columns": { "ready": "Todo" }
   }
   ```

   Add `"cycle": "current"` inside `linear` to put each sprint issue and its
   planned issues in the team's current cycle.

   For GitHub:

   ```json
   "tracker": {
     "provider": "github",
     "github": { "owner": "acme", "repo": "app", "projectNumber": 3 }
   }
   ```

   Remove a `columns` override that no longer matches the new tracker's
   status names.
3. For Linear, export `LINEAR_API_KEY` where non-interactive shells see it
   (for zsh, `~/.zshenv`), and create the four type labels in the team.
4. Ask Dad to check the setup again:

   ```text
   /dad setup this repository
   ```

   This rediscovers the tracker and rewrites the ignored
   `dad/project-ids.json`. Fix any missing column or label it reports before
   filing issues.

Branch names follow the tracker's keys (`feat/41-…` on GitHub,
`feat/eng-41-…` on Linear), so start new branches only after switching.

Release: 0.4.0
