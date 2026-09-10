# Notion Workspace Rules

These are workspace best practices for managing tasks in Notion via this CLI.

---

## Titles

- **Keep titles concise** — 9–12 words maximum.
- Titles should describe the work clearly without being a full sentence.
- Avoid filler words like "we need to", "please", "I want to".

**Good:**
```
Fix login redirect loop on expired sessions
Add CSV export to the reports dashboard
```

**Bad:**
```
We need to fix the issue where the login page redirects users in a loop when their session has expired
```

---

## Descriptions

- Every task must have a **detailed description** saved in the `Notes` or `Description` property — whichever is available in the database.
- The description should answer: *what*, *why*, and *acceptance criteria*.
- If relevant project documents exist (specs, RFCs, design files, API docs), **link or reference them** in the description.
- Use the CLI to set it:
  ```
  ./notion update <No.> Notes "Full description here..."
  ```

---

## Comments

- Comments have a **character limit** — keep each comment focused.
- If a comment is getting long, **break it into multiple shorter comments**.
- One comment per concern: status update, blocker, question, decision — not all at once.

**Good (multiple short comments):**
```
./notion comment 42 "Blocked: waiting on design approval for the modal layout."
./notion comment 42 "Design approved. Moving to implementation."
```

**Bad (one long dump):**
```
./notion comment 42 "So I looked into the issue and it seems like the problem is related to the modal layout which design hasn't approved yet and also there's a question about whether we should use a drawer instead but I also noticed the API returns a 500 in some edge cases so we need to look at that too..."
```

---

## Referencing Docs in Descriptions

- If the project has relevant documents — specs, design files, RFCs, ADRs, changelogs — mention them in the task description.
- Use direct links where possible, or reference them by name if they're in a shared workspace.

**Example:**
```
Implement the export feature as defined in the reporting-spec.md.
See also: Figma design #123 and the API contract in openapi.yaml.
```

---

## Status Changes

- **Always add a comment before changing the `Status`** of a task.
- The comment should briefly explain *why* the status is changing.
- This creates a clear audit trail and keeps collaborators in the loop.

**Good:**
```
./notion comment 42 "Implementation done and tested locally. Moving to Review."
./notion update 42 Status "In Review"
```

**Bad:**
```
./notion update 42 Status "In Review"
```
*(No context left for the team about what changed or why.)*

---

## Effort Points

Every task should have an **Effort** property set using Fibonacci points:

| Points | Meaning |
|--------|---------|
| 1      | Trivial — a quick config change, typo fix, or one-liner |
| 2      | Small — straightforward, well-understood work |
| 3      | Medium-small — a bit of thought required, low risk |
| 5      | Medium — meaningful work, some unknowns |
| 8      | Large — complex or touches multiple areas |
| 13     | Very large — high uncertainty; consider breaking it down |

- If a task feels like it's between two numbers, **pick the higher one**.
- Tasks at **13 points are a signal** — they're likely too big and should be split before being picked up.
- Don't use effort points as a time estimate. They measure complexity and uncertainty, not hours.

**Example:**
```
./notion update 42 Effort 5
```

---

## Tags

Use tags to signal special requirements that affect how a task is picked up and deployed.

- If the task involves **running a migration** (database schema changes, data migrations, seed scripts), add the tag:
  ```
  has-migration
  ```
- If the task requires **changes to environment variables** (new keys, updated values, removed entries in `.env`), add the tag:
  ```
  has-env-change
  ```

These tags help the team prepare the environment before or during deployment and avoid surprises.

**Example:**
```
./notion update 42 Tags "has-migration"
./notion update 42 Tags "has-env-change"
```

---

## Summary

| Rule          | Guideline                                                                 |
|---------------|---------------------------------------------------------------------------|
| Title length  | 9–12 words max                                                            |
| Description   | Always fill in Notes/Description — include what, why, acceptance criteria |
| Relevant docs | Reference any related specs, designs, or docs in the description          |
| Comments      | Keep short; split long thoughts into multiple comments                    |
| Status change | Always comment before updating the Status property                        |
| Tags          | Add `has-migration` or `has-env-change` when applicable                   |
| Effort        | Set Fibonacci points (1–13); 13 = too big, consider splitting             |
