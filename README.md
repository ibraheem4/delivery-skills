# Delivery Skills

Private, project-specific agent skills for workflow automation, infrastructure management, and deployment.

For generic engineering skills (testing, code review, security, etc.), see the public [agent-skills](https://github.com/ibraheem4/agent-skills) repo.

## Skills

### Workflow
| Skill | Description |
|-------|-------------|
| **implement** | Pick up a Linear story end-to-end: context → branch → code → validate → review → PR |
| **ship** | Validate, commit, push, and create PR to dev |
| **babysit-pr** | Shepherd a PR through review + CI until merge-ready |
| **resolve-review** | Fix PR review comments, reply, resolve threads |
| **review** | Pre-landing code review with two-pass checklist |
| **create-plan** | Design implementation plans (PRD → Architecture → Stories) |
| **validate** | Run validation checks locally before pushing |
| **qa** | Systematic QA testing with screenshots and interaction |
| **retro** | Generate engineering retrospective from git history |
| **npm-publish** | Version bump, build, publish, tag, and push |

### Infrastructure
| Skill | Description |
|-------|-------------|
| **deploy** | Deploy a service to dev or prod with pre-flight validation |
| **cloud-build** | Monitor Google Cloud Build status, view logs, diagnose failures |
| **prime** | Front-load context files and recent activity for a service |
| **sunset** | Sunset a service: remove submodule, disable infra, archive repo |
| **apple-signing** | Set up Apple code signing for Tauri desktop releases |
| **security-audit** | Audit branch protections, Dependabot, code scanning across repos |
| **triage-dependabot** | Triage Dependabot PRs: close superseded, merge safe, rebase stale |

## Setup

Symlink into `~/.claude/skills/`:

```bash
for skill in skills/*/; do
  name=$(basename "$skill")
  ln -sf "$(pwd)/$skill" ~/.claude/skills/"$name"
done
```
