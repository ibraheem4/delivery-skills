# Delivery Skills

Generalized agent skills for workflow automation, infrastructure management, and deployment. Usable by any team with GitHub + a project tracker.

For core engineering skills (testing, code review, security, etc.), see the public [agent-skills](https://github.com/ibraheem4/agent-skills) repo. These skills build on top of those patterns with executable workflows.

## Skills

### Workflow
| Skill | Description |
|-------|-------------|
| **implement** | Pick up a ticket end-to-end: context → branch → code → validate → review → PR |
| **ship** | Validate, commit, push, and create PR — fully automated |
| **create-plan** | Design implementation plans (PRD → Architecture → Stories) |
| **validate** | Run validation checks locally before pushing |
| **qa** | Systematic QA testing with screenshots and interaction |
| **retro** | Generate engineering retrospective from git history |
| **npm-publish** | Version bump, build, publish, tag, and push |
| **triage-dependabot** | Triage Dependabot PRs: close superseded, merge safe, rebase stale |

### Infrastructure
| Skill | Description |
|-------|-------------|
| **deploy** | Deploy a service to dev or prod with pre-flight validation |
| **cloud-build** | Monitor cloud build status, view logs, diagnose failures |
| **prime** | Front-load context files and recent activity for a service |
| **sunset** | Sunset a service: remove submodule, disable infra, archive repo |
| **apple-signing** | Set up Apple code signing for desktop app releases |
| **security-audit** | Audit branch protections, Dependabot, code scanning across repos |

## Relationship to agent-skills

These skills reference patterns from the public [agent-skills](https://github.com/ibraheem4/agent-skills) repo:

| This Skill | Uses Pattern From |
|-----------|-------------------|
| implement | scope-discipline, incremental-implementation, pr-lifecycle |
| ship | shipping-and-launch, review-response, pr-lifecycle |
| qa | frontend-ui-engineering |
| validate | engineering-fundamentals-checklist |

## Setup

Symlink into `~/.claude/skills/`:

```bash
for skill in skills/*/; do
  name=$(basename "$skill")
  ln -sf "$(pwd)/$skill" ~/.claude/skills/"$name"
done
```
