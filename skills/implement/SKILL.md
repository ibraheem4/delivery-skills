---
name: implement
description: "Pick up a ticket and implement it end-to-end: load context, create branch, code the solution, validate, review, and open a PR. Use when user says 'implement XXX', 'pick up this story', 'work on this ticket', or 'build this feature'."
user_invocable: true
argument-hint: "<ticket-id>"
---

# Implement a Ticket

Pick up a ticket from the project tracker, load relevant context, implement it end-to-end, then review and ship a PR.

## Arguments

Parse `$ARGUMENTS` as a ticket ID (e.g., `PROJ-301`) or description to search for.

## Steps

### Phase 1: Understand

1. **Find the ticket**: Look up `$ARGUMENTS` in the project tracker. If no ID given, search for matching issues.

2. **Load context**: Read relevant files based on what the ticket touches:
   - Always: project instructions (`CLAUDE.md`, `AGENTS.md`, or equivalent)
   - Service-specific docs if they exist
   - Related source files and tests

3. **Understand the ticket**: Read the full description, acceptance criteria, and linked design docs.

4. **Update status**: Set ticket status to `In Progress`.

### Phase 2: Implement (in isolation)

5. **Create an isolated workspace**: Use a worktree or branch so this work is isolated from other in-progress changes.

6. **Set up branch**: Create a feature branch from the base branch:
   ```bash
   git fetch origin {base} && git checkout -b {prefix}/{ticket-id}-{short-description} origin/{base}
   ```

7. **Implement**: Follow the acceptance criteria. Work incrementally — build, test, commit in small steps.

8. **Validate**: Run the project's validation/CI checks locally before committing.

### Phase 3: Review & Ship

9. **Pre-landing review**: Review your own diff before shipping:
   - **CRITICAL**: Secrets, type safety issues, security flaws
   - **INFORMATIONAL**: Console.log, dead code, TODOs without tickets
   - Fix critical issues before proceeding

10. **Stage and commit**: Use conventional commit format with ticket reference:
    ```
    {type}: {description}

    closes {TICKET-ID}

    Co-Authored-By: Claude <noreply@anthropic.com>
    ```

11. **Push and create PR**:
    ```bash
    git push -u origin $(git branch --show-current)
    gh pr create --base {base-branch} --title "{type}: {summary}" --body "..."
    ```
    PR body must include: summary, ticket reference, validation results, test plan.

12. **Output the PR URL.**

### Phase 4: Post-PR

13. **Monitor the PR** through automated review and CI. Fix issues as they arise. See the `pr-lifecycle` pattern.

## Important Rules

- **Never push directly to the base branch.** Always create a PR.
- **Never commit without running the review checklist.** It catches what validation misses.
- **Never skip validation.** If local checks fail, fix the issues first.
- **Always include ticket reference.** No commits without a tracker link.
- **Never merge the PR.** Leave the final merge to the human.
- **Ask for user confirmation** before creating the commit and PR.
