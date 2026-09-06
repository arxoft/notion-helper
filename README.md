# notion

A minimal Notion CLI for task management. List, view, create, update tasks and add comments — all from the terminal.

**Requires:** `bash`, `curl`, `jq`

---

## Setup

### 1. Create a Notion integration

Go to [notion.so/profile/integrations](https://www.notion.so/profile/integrations) → **New integration** → give it a name → select your workspace → Save.

Enable these capabilities:
- Read content
- Update content
- Insert content
- Read comments
- Create comments

Copy the `secret_xxx` API key.

### 2. Connect integration to your database

Open your task board in Notion → `...` (top right) → **Connections** → find your integration → connect.

### 3. Get your database ID

From the board URL:
```
https://app.notion.com/p/<DATABASE_ID>?v=...
```

### 4. Create `.env.notion` in your project root

```
NOTION_API_KEY=secret_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
NOTION_DATABASE_ID=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

| Variable | Required | Default | Description |
|---|---|---|---|
| `NOTION_API_KEY` | ✓ | — | Integration secret from Notion |
| `NOTION_DATABASE_ID` | ✓ | — | ID from the board URL |

The script reads `.env.notion` from whichever directory you call it from, so each project can have its own file pointing to a different database.

---

## Globalize

To call `notion` from anywhere:

```bash
# copy to a directory on your PATH
cp /path/to/notion ~/.local/bin/notion
chmod +x ~/.local/bin/notion

# verify
which notion
notion help
```

Then from any project directory that has a `.env.notion`:
```bash
cd ~/my-project
notion tasks
```

---

## Usage

```
notion <command> [arguments]
```

| Command | Description |
|---|---|
| `notion tasks` | List all tasks grouped by `Status` |
| `notion tasks <No.>` | Show all properties + comments for a task |
| `notion tasks --filter <prop> <value>` | Filter tasks by any property |
| `notion tasks --sort <prop> [asc\|desc]` | Sort task list |
| `notion comment <No.> "text"` | Add a comment to a task |
| `notion update <No.> <property> <value>` | Update a task property |
| `notion create --title "..." [--prop val]` | Create a new task |
| `notion open <No.>` | Print the Notion URL for a task |
| `notion help` | Show usage |

Tasks are addressed by their `No.` — the built-in auto-incremented ID per database. Property names are case-insensitive.

---

## Examples

```bash
# list all tasks
notion tasks

# view task detail with comments
notion tasks 42

# filter and sort
notion tasks --filter stage "In Progress"
notion tasks --sort "No." desc

# add a comment
notion comment 42 "Blocked on design review"

# update properties
notion update 42 stage "Done"
notion update 42 priority "High"
notion update 42 "due date" 2026-09-15

# create a task
notion create --title "Fix login bug" --stage "To Do" --priority "High"

# get the Notion URL
notion open 42
```

---

## Notes

- `No.` resolution queries the database for `unique_id.equals = <No.>` to get the internal page UUID before any operation.
- `update` auto-detects the property type from the page schema and builds the correct API payload. Supported types: `title`, `rich_text`, `select`, `status`, `multi_select`, `number`, `checkbox`, `url`, `email`, `phone_number`, `date`.
- `create` fetches the database schema to infer types for any extra `--prop` flags.
- If `.env.notion` is not found in the current directory, the script exits with an error and lists the required variables.
