# Add Standards

Explicitly add new standards to your standards library.

![[!_guidelines]]

## Usage

```
/add-standards "All API responses must use cursor-based pagination"
/add-standards ./docs/api-style.md
/add-standards --split "## " ./mixed-notes.txt
/add-standards
```

## Process

### Step 1: Parse Input

Check `$ARGUMENTS` to determine the input mode:

**Mode detection:**

| Input | Mode | Action |
|-------|------|--------|
| (empty) | Interactive | Go to Step 2 |
| File path (ends in `.md`, `.txt`, etc.) | Import | Read file contents, go to Step 3 |
| Quoted text | Direct | Use as-is, go to Step 3 |
| `--split "DELIM"` + text/file | Batch explicit | Use delimiter, go to Step 3 |

**If Import mode**, read the file. If file not found, tell the user and stop.

**If Direct mode**, the arguments after `/add-standards` are the standard content.

### Step 2: Interactive Mode

Use {{ask_user_questions_tool}} to gather the standard:

```
What standard do you want to add?

You can:
- Paste the standard text directly
- Provide a file path to import
- Describe the practice and I'll draft it for you
```

Wait for user response. Then:
- If they paste text → use as content, go to Step 3
- If they provide a file path → read it, go to Step 3
- If they describe a practice → draft a standard, show it for confirmation, then go to Step 3 with the confirmed draft

### Step 3: Detect Multiple Standards

Analyze the content for separators. Auto-detect in this priority order:

1. **Horizontal rules** — `---`, `***`, or `___` on their own line
2. **Numbered headers** — `## 1.`, `## 2.`, `**1.**`, `**2.**` etc.
3. **Markdown headers** — `# ` at the start of lines (split on each `# ` header)
4. **Double blank lines** — Two or more consecutive empty lines

**If a separator pattern is found:**

Use {{ask_user_questions_tool}} to confirm:

```
I detected {count} standards separated by {marker}:

1. [first line or title of standard 1]
2. [first line or title of standard 2]
3. [first line or title of standard 3]

Is this split correct? (yes / adjust / it's just one standard)
```

- **Yes** → process each standard through Step 4
- **Adjust** → ask user to clarify the split, re-detect
- **It's just one standard** → treat entire content as single standard

**If no separator found** → treat as single standard.

**If `--split` flag was provided** → use that delimiter directly, still confirm with user.

### Step 4: Process Each Standard

For each standard (process one at a time):

#### Step 4a: Determine Folder

1. Scan `better-agents/standards/` for existing folders (if the directory exists)
2. Based on the standard's content, suggest the most relevant existing folder
3. If no existing folder fits, suggest `global`

Use {{ask_user_questions_tool}}:

```
Standard: [first line or title]

Suggested folder: {suggested_folder}

Available folders: {list existing folders, or "none yet (will create)"}

Which folder? (accept / choose: {other_folder})
```

Wait for user response.

#### Step 4b: Generate Slug and Description

- **Slug**: Derive from the standard's title or first line. Lowercase, hyphens, no extension. Example: `api-response-format`
- **Description**: Auto-generate from the first sentence or summary of the content. Keep to one short sentence.

#### Step 4c: Show Draft and Confirm

Use {{ask_user_questions_tool}}:

```
Here's the draft:

  File: {folder}/{slug}.md
  Description: "{auto-generated description}"

---
[full formatted standard content]
---

Create this file? (yes / edit: [your changes] / skip)
```

- **Yes** → create the file
- **Edit** → apply user's changes, show updated draft, confirm again
- **Skip** → skip this standard, move to next

#### Step 4d: Create File

Create `better-agents/standards/{folder}/{slug}.md` with the confirmed content.

### Step 5: Update Index

After all standards are created (or after each one if batch):

1. Read `better-agents/standards/index.toml` if it exists
2. Add new entries with their auto-generated descriptions
3. Write the updated index following these rules:
   - Alphabetize by folder, then by filename
   - File names without `.md` extension
   - One-line descriptions only

**Index format:**
```yaml
folder-name:
  file-name:
    description: Brief description here
```

If `root` folder standards were created (`.md` files directly in `standards/`), they go under the `root:` key.

### Step 6: Report and Offer Continuation

Use {{ask_user_questions_tool}}:

```
Standards added:
  ✓ {folder}/{slug}.md
  ✓ {folder}/{slug}.md

Total: {count} standards added

Add more standards, or done?
```

If "add more", loop back to Step 2 (Interactive mode).

![[!_standards-writing-guide]]

## Tips

- **Bulk import** — Paste multiple standards from docs/wiki, separated by `---`
- **Be specific** — The more context you provide, the better the auto-generated description
- **Review the draft** — Use the edit option to refine before creating
- **Run `/index-standards` after** if you manually edit files later

## Output

All standards: `better-agents/standards/{folder}/{slug}.md`
Index file: `better-agents/standards/index.toml`
