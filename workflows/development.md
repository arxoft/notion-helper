# Development Workflow

## Overview

A task moves through development from **Up Next** → **In Progress** → **Pending Review**.
This document covers everything from picking up a task to submitting it for review.

---

## 1. Pick Up a Task

```bash
notion tasks --filter Status "Up Next"
notion tasks <No.>
```

- Read the full task description and acceptance criteria.
- Read all existing comments for context.
- Ask questions in a comment before starting if anything is unclear.

---

## 2. Set Up the Branch

The `Branch Name` property on the task must be populated before you create a branch.
A human confirms the branch name.

Branch naming follows the underscore-separated lineage pattern:

```
<root-branch>_<milestone>_<No.>-short-task-name    ← task with parent milestone
<root-branch>_<No.>-short-task-name                ← standalone task
```

The root branch name varies by project — check the project's README or ask. Common values: `main-master`, `main`, `master`, `develop`.

```bash
git checkout -b main-master_0071-short-task-name
# adjust root branch prefix to match the project
```

Rules:
- Never create or switch branches without being asked.
- Never push directly to the root branch.
- Never use `--force` or `--force-with-lease` without explicit confirmation.

---

## 3. Move to In Progress

Before changing Status, add a comment explaining the move:

```bash
notion comment <No.> "Starting implementation. Branch: <branch-name>."
notion update <No.> Status "In Progress"
```

---

## 4. Implement

- Match existing code patterns exactly — no new abstractions or packages without asking.
- Follow the project's linting/formatting config.
- Read existing code before writing new code.
- If a DB schema change is needed: write a migration file and tag the task `has-migration`.
- If `.env` changes are needed: document what changed and tag the task `has-env-change`.

```bash
notion update <No.> Tags "has-migration"
notion update <No.> Tags "has-env-change"
```

---

## 5. Commits

Never commit without explicit instruction from the user.

When told to commit, the AI agent proposes the commit message in this format:

```
type(scope): short description

No.<No.> — Task title from Notion
One line explaining what changed and why.

https://app.notion.com/p/<task-url>
```

- Stage specific files — avoid `git add .`
- Prefer new commits over `--amend`
- Never skip hooks (`--no-verify`) unless explicitly asked

After the human confirms the commit hash, post a comment on the Notion task:

```
Commit: [a3f92c1](https://github.com/org/repo/commit/a3f92c1) — type(scope): short description

No.<No.> — Task title
What changed and why.

https://app.notion.com/p/<task-url>
```

---

## 6. Push and Open a PR

Push the task branch and open a PR against the parent branch:

```bash
git push -u origin <branch-name>
```

PR rules:
- Title: concise, under 70 characters.
- Body must link the Notion task.
- Never push directly to the root branch.

After creating the PR, add a comment on the Notion task with the PR URL.

---

## 7. Move to Pending Review

Before moving the task status, add a comment with manual testing instructions:
- What to test
- How to trigger it
- Expected outcome

```bash
notion comment <No.> "Testing instructions: ..."
notion update <No.> Status "Pending Review"
```

---

## Tags Reference

| Tag | When to add |
|-----|-------------|
| `has-migration` | Task ships a DB schema change |
| `has-env-change` | Task requires `.env` updates |

---

## Status Flow

`Up Next → In Progress → Pending Review`
