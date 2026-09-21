# Review Workflow

## Overview

Reviews are picked up from tasks in **Pending Review**, ordered by PR number ascending (lowest first) — unless explicitly told otherwise.

A review ends in one of two outcomes: **Approved** (merge + move to Pending Deployment) or **Issues Found** (send back to developer).

---

## 1. Prepare

Ensure git working directories are clean in all relevant repos:

```bash
git status
```

If the workspace has multiple repos (e.g. frontend + backend), run `git status` in each and ensure all are clean before starting.

Make sure you are on the correct target branch (the one that will receive the incoming merge).

---

## 2. Pick Up the Next Review

```bash
notion tasks --filter Status "Pending Review"
```

Order by **PR number ascending** (lowest PR number first) — unless the reviewer explicitly specifies otherwise. Lower PR numbers were submitted earlier and are less likely to have unresolved dependencies.

Read the full task:

```bash
notion tasks <No.>
```

- Review the task description (what the bug/feature is).
- Read all comments — especially the manual testing instructions left by the developer.
- Present the testing instructions before inspecting the diff.

---

## 3. Inspect the Diff

Run `./review <branch-name>` in each relevant repo. This script fetches the branch, merges it into the current branch in `--no-commit` mode, and stages the diff for review.

If the workspace has multiple repos, run `./review` in each separately.

Things to check:
- Does the implementation match the acceptance criteria?
- Are there any regressions or unintended side effects?
- Does the code follow existing patterns (no new abstractions, no new packages without approval)?
- If tagged `has-migration` — is the migration file present and correct?
- If tagged `has-env-change` — are the env changes documented?

Minor issues (typos, formatting, small logic fixes):
- Unstage the file, fix it, stage again — no new PR needed.

---

## 4. Conflict Resolution

If `./review` reports a merge conflict:

- Open the conflicted file(s) and resolve manually.
- Stage the resolved file: `git add <file>`
- Conflicts from duplicate additions (two PRs adding the same thing) — take the incoming version unless HEAD has a known improvement.
- Do not abort unless the conflict is unresolvable without the developer's input — in that case, treat as Issues Found.

---

## 5. Abort a Review

To abort a review in progress (e.g. wrong order, missing dependency):

```bash
git reset --hard origin/<base-branch>   # backend: origin/master, frontend: origin/main
```

This discards the staged merge. The working directory returns to the clean base state.

---

## 6. Outcome: Issues Found (Major)

If a significant issue is found:

```bash
notion comment <No.> "Review: [describe the issue clearly]"
notion update <No.> Tags "issue-persists"
notion update <No.> "Assigned To" "<developer-email>"
notion update <No.> Status "Up Next"
```

Also add a review comment on the GitHub PR describing the issue.

---

## 7. Outcome: Approved

If the review passes:

1. Merge the staged commit and push the branch (PRs auto-merge on push).
2. Tag the task and move status:

```bash
notion comment <No.> "Review passed. Merging and moving to Pending Deployment."
notion update <No.> Tags "reviewed"
notion update <No.> Status "Pending Deployment"
```

Move on to the next **Pending Review** task.

---

## Tags Reference

| Tag | Set by | Meaning |
|-----|--------|---------|
| `reviewed` | Reviewer | Code review passed; clear to deploy |
| `issue-persists` | Reviewer | Major issue found; sent back to developer |
| `has-migration` | Developer | Task ships a DB schema change; run migration on server before/after deploy |
| `has-env-change` | Developer | Task requires `.env` updates; update server `.env` before deploying |

---

## Status Flow

`Pending Review → Pending Deployment`  (approved)
`Pending Review → Up Next`             (issues found)
