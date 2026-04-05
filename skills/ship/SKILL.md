---
name: ship
description: "Fully automated ship workflow: validate, commit, push, and open a PR. Use when user says 'ship it', 'create a PR', 'open a pull request', 'send for review', or is done with a feature branch and wants to merge."
argument-hint: "[TICKET-ID] [--skip-validate]"
---

# Ship: Automated PR Workflow

Non-interactive, fully automated workflow. Run straight through and output the PR URL at the end.

**Only stop for:**
- On the base branch (abort — ship from a feature branch)
- No ticket reference found (ask for one)
- Merge conflicts that can't be auto-resolved
- Validation failures
- Pre-landing review finds CRITICAL issues

**Never stop for:**
- Uncommitted changes (always include them)
- Commit message approval (auto-compose from diff)
- PR description content (auto-generate)

## Arguments

Parse `$ARGUMENTS`:
- **TICKET-ID**: Project tracker reference (required — if not provided, check branch name)
- **--skip-validate**: Skip validation step (for non-code changes like docs)

---

## Step 1: Pre-flight

1. Check current branch. If on the base branch:
   - If there are uncommitted changes, create a feature branch first
   - If no changes, abort

2. Extract ticket reference from args or branch name.

3. Run `git status` (never use `-uall`). Note uncommitted changes — they'll be included.

4. **Lockfile sync check:** Verify the lockfile is in sync with the manifest. If not, update and include it.

5. Check what's being shipped: `git log {base}..HEAD --oneline` and `git diff {base}...HEAD --stat`.

---

## Step 2: Merge base branch

Fetch and merge the base branch so validation runs against the merged state:

```bash
git fetch origin {base} && git merge origin/{base} --no-edit
```

If merge conflicts, try auto-resolve for simple cases. For complex conflicts, stop.

---

## Step 3: Validate (unless --skip-validate)

Run the project's local validation checks (linting, type checking, tests, build):

```bash
# Detect and run appropriate validation command
# e.g., make validate, npm test, pnpm check, etc.
```

If validation fails, show failures and stop.

---

## Step 4: Pre-Landing Review

Review the diff for issues that tests don't catch:

1. Get the full diff: `git diff origin/{base}`

2. **CRITICAL** (stop-ship):
   - Committed secrets (.env values, API keys, tokens)
   - Type safety violations (`any`, `@ts-ignore` without justification)
   - Security flaws (injection, XSS, missing auth)
   - Missing ticket reference

3. **INFORMATIONAL** (note but don't block):
   - `console.log` / `debugger` statements
   - TODO/FIXME without ticket references
   - Dead imports or unused variables

4. If critical issues found, present each with recommended fix and let user decide.

---

## Step 5: Stage and Commit

1. Stage all changes: `git add -A`

2. Compose commit message using conventional format:
   ```
   {type}: {concise summary}

   {optional body — what and why}

   closes {TICKET-ID}

   Co-Authored-By: Claude <noreply@anthropic.com>
   ```

3. For large changesets (>8 files, >300 lines), split into bisectable commits.

---

## Step 6: Push and Create PR

```bash
git push -u origin $(git branch --show-current)
gh pr create --base {base-branch} --title "{type}: {summary}" --body "..."
```

PR body includes: summary, ticket reference, review findings, validation results, test plan.

**Output the PR URL.**

---

## Step 7: Post-PR Monitoring

After PR creation, monitor through automated review and CI. Fix issues as they arise (max 3 cycles). See the `pr-lifecycle` pattern.

**Never merge automatically. The final merge is the human's decision.**

---

## Important Rules

- **Never push to the base branch directly.** Always create a PR.
- **Never skip the pre-landing review.** It catches what validation misses.
- **Never force push.** Regular `git push` only.
- **Always include ticket reference.** No commits without one.
- **Never merge the PR.** Leave the final merge to the human.
