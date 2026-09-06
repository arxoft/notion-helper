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

## Summary

| Rule          | Guideline                                                                 |
|---------------|---------------------------------------------------------------------------|
| Title length  | 9–12 words max                                                            |
| Description   | Always fill in Notes/Description — include what, why, acceptance criteria |
| Relevant docs | Reference any related specs, designs, or docs in the description          |
| Comments      | Keep short; split long thoughts into multiple comments                    |
