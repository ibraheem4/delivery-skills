---
name: prime
description: "Front-load all relevant context files, dependencies, and recent git activity for a specific Acme service. Use when starting work on a service, switching context, or when user says 'prime studio', 'load context for validate', or 'get ready to work on marketing'."
context: fork
argument-hint: "<service-name>"
---

# Prime Context for a Service

Front-load all relevant context for working on a specific service.

## Arguments

Parse `$ARGUMENTS` as a service name (e.g., `validate`, `studio`, `marketing`).

## Service Shortcuts

| Shortcut | Service Directory |
|----------|------------------|
| `validate` | `acme-validate/` |
| `studio` | `acme-studio/` |
| `marketing` | `acme-marketing/` |
| `platform` | `acme-platform-api/` |
| `mcp` | `acme-mcp-server/` |
| `infra` | `acme-infrastructure/` |
| `dashboard` | `acme-dashboard/` |
| `resume` | `acme-resume/` |
| `appratings` | `acme-appratings/` |
| `umami` | `acme-umami/` |
| `cli` | `acme-cli/` |

## Steps

1. **Load core context files**:
   - `.claude/context/product.md` — What Acme builds
   - `.claude/context/tech-stack.md` — Full technology stack
   - `.claude/context/dev-standards.md` — Development standards

2. **Load service-specific context**:
   - Read `{service}/CLAUDE.md` if it exists
   - Read `{service}/package.json` or `{service}/pyproject.toml` for dependencies
   - Read `{service}/README.md` for service-specific docs

3. **Load infrastructure context if relevant**:
   - `.claude/context/infrastructure.md` — Cloud SQL, Terraform, CLI auth
   - `.claude/context/deployment.md` — CI/CD, branching strategy
   - `.claude/context/auth-oauth.md` — If service handles auth

4. **Check recent activity**:
   - `git log --oneline -20` in the service directory for recent changes
   - Search Linear for open issues related to this service

5. **Summarize** what was loaded and any notable recent changes or open issues.
