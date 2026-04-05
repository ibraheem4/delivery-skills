# Lucitra Skills

Executable agent workflows for tasks that go beyond code patterns — deployment, QA testing, dependency triage, and release management. Builds on the public [agent-skills](https://github.com/lucitra/agent-skills) repo for core engineering patterns.

## Skills

| Skill | Description |
|-------|-------------|
| **create-plan** | Design implementation plans (PRD → Architecture → Stories) |
| **qa** | Systematic QA testing with screenshots and interaction |
| **retro** | Generate engineering retrospective from git history |
| **npm-publish** | Version bump, build, publish, tag, and push |
| **triage-dependabot** | Triage Dependabot PRs: close superseded, merge safe, rebase stale |
| **reset** | Full workspace reset: sync submodules, prune worktrees, sync skills, verify clean state |
| **deploy** | Deploy a service to dev or prod with pre-flight validation |
| **cloud-build** | Monitor cloud build status, view logs, diagnose failures |
| **sunset** | Sunset a service: remove submodule, disable infra, archive repo |

## Setup

```bash
for skill in skills/*/; do
  name=$(basename "$skill")
  ln -sf "$(pwd)/$skill" ~/.claude/skills/"$name"
done
```
