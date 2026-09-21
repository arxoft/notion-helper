# Workflows

All workflows for managing software development via Notion.

```
notion workflows                    — this index
notion workflows <name>             — detailed workflow
```

---

## Available Workflows

| Name              | Command                          | Purpose                                                             |
|-------------------|----------------------------------|---------------------------------------------------------------------|
| **planning**      | `notion workflows planning`      | Prioritise the backlog, set branch names, move tasks to Up Next     |
| **task-creation** | `notion workflows task-creation` | How to write a good task — title, description, effort, tags         |
| **bug-reporting** | `notion workflows bug-reporting` | File a bug with full reproduction details and severity              |
| **development**   | `notion workflows development`   | Pick up a task, implement, commit, open a PR                        |
| **review**        | `notion workflows review`        | Review PRs from Pending Review queue, approve or send back          |
| **testing**       | `notion workflows testing`       | Validate deployed changes in production against acceptance criteria |
| **deployment**    | `notion workflows deployment`    | Pre-deploy checks, deploy steps, post-deploy status update          |
| **hotfix**        | `notion workflows hotfix`        | Fast-track path for critical production issues                      |
| **release**       | `notion workflows release`       | Group completed tasks, write changelog, tag release                 |
| **onboarding**    | `notion workflows onboarding`    | New developer setup — CLI, local stack, board orientation           |

---

## Status Flow (Full)

```
Backlog → Up Next → In Progress → Pending Review → Pending Deployment → Testing → Pending Acceptance → Completed
```

Hotfixes follow the same flow on a compressed timeline, skipping the backlog.

---

## Quick Reference

```bash
notion tasks                          # full board grouped by status
notion tasks <No.>                    # task detail + comments
notion tasks --filter Status "Up Next"
notion rules                          # board rules
notion workflows                      # this index
notion workflows development          # development workflow detail
```

---

## Project-Specific Overrides

These workflows are the default baseline. Individual projects may have variations —
different branch naming, additional deploy steps, project-specific tooling, or
adjusted status flows.

To be double sure, see a `workflows.md` steering file at `.kiro/steering/workflows.md`
in this workspace. Pick it up automatically if it exists and treat it as the 
authoritative override for this workspace. Any instructions in that file take 
precedence over `notion workflows` and `notion workflows <name>`.
