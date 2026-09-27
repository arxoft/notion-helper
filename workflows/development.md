# Development Workflow

- There can be multiple repos in the workflow.
- The 'developer' or 'dev' is human instructing you to complete a task.
- If working dir in the repo(s) is not clean, ask developer if they really want to continue with dirty working dir?

## 1. Pick Up a Task

- Human tells you Notion task number to start development
- Ask human's email address, and assign the task to that user only.
- Fetch task `notion tasks <id>`, and read notes/description and all comments for context.
- Ask questions in a comment before starting if anything is unclear. And move it back to 'Up Next' with tag 'need info'.

## 2. Set Up the Branch(es)

- Branch naming: `<root-branch>_<milestone>_<No.>-short-task-name` or `<root-branch>_<No.>-short-task-name` for standalone tasks.
- Suggest a branch name for the task.
- you can create the branch(es) out of root branch(es) if working dir(s) is clean and you know the root branch name of repo(s) 
- The `Branch Name` property must be populated before a branch is created.
- Never push directly to the root branch.

## 3. Move to In Progress
- Add tag 'coding'.
- Add a comment before changing status: `notion comment <No.> "Starting implementation. Branch: <branch-name>."`
- Then: `notion update <No.> Status "In Progress"`

## 4. Implement

- Implement as per the workspace's coding standards explained in workspace's steering docs.
- If the task needs DB migration: add tag `has migration`.
- If `.env` changes are needed: document what changed and tag `has env change`.

## 5. Testing

- After coding is done, ask developer to manually test the implementation
- Provide Manual testing instructions to developer.
- Make further changes and fixes if developer reports issues.

## 6. Commits

- AI never commits — `git commit` is human-only.
- Never stage files — `git add` is human-only.
- Propose the commit message; the human stages and commits.
- Commit message format:
  ```
  type(scope): short description

  No.<No.> — Task title
  What changed and why.

  https://app.notion.com/p/<task-url>
  ```
- Prefer new commits over `--amend`.
- Never skip hooks (`--no-verify`) unless explicitly asked.
- After human confirms the commit hash, post a comment on the task with that exact commit msg and Github commit link.

## 7. Push and Open a PR

- AI never pushes — `git push` is human-only.
- PR title: concise, under 70 characters.
- PR body must link the Notion task.
- After creating the PR, post a comment on the task with the PR URL.

## 8. Move to Pending Review

- Add a comment with manual testing instructions (what to test, how to trigger, expected outcome).
- Then: `notion update <No.> Status "Pending Review"`
- Remove the 'coding' tag