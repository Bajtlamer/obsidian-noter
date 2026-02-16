# obsidian-noter

A Claude Code skill that moves Markdown and plain text files into an Obsidian vault. It preserves the original folder structure, converts plain text files to properly formatted Markdown, and reports a summary of everything it processed.

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [What the skill does](#what-the-skill-does)
- [Supported file types](#supported-file-types)
- [Behavior details](#behavior-details)
- [Output summary](#output-summary)

---

## Requirements

- [Claude Code](https://code.claude.com) installed and configured
- An existing Obsidian vault on your machine

---

## Installation

1. Locate or create the skills directory for Claude Code:

   ```
   ~/.claude/skills/
   ```

2. Copy the `obsidian-noter` folder (the one containing `SKILL.md`) into that directory:

   ```bash
   cp -r obsidian-noter ~/.claude/skills/
   ```

   The resulting path should be:

   ```
   ~/.claude/skills/obsidian-noter/SKILL.md
   ```

3. Restart Claude Code or reload skills if required.

---

## Configuration

Before using the skill, open `~/.claude/skills/obsidian-noter/SKILL.md` and update the vault path on this line:

```
Move Markdown and plain text files into the Obsidian vault at `~/Obsidian-vault-01`
```

Replace `~/Obsidian-vault-01` with the actual path to your vault. For example:

```
~/Documents/MyVault
```

The same path also appears in the workflow section (Step 3). Update both occurrences so they match.

---

## Usage

Invoke the skill from Claude Code with a source path as the argument:

```
/obsidian-noter [source-path]
```

**Move a single file:**

```
/obsidian-noter ~/Downloads/meeting-notes.txt
```

**Move an entire folder:**

```
/obsidian-noter ~/Downloads/project-docs
```

The `source-path` can be an absolute path or a path relative to your current working directory.

---

## What the skill does

1. **Validates the source path.** Confirms the file or folder exists before doing anything.

2. **Discovers files.** For a directory, it walks the tree recursively and classifies every file as accepted or skipped.

3. **Processes each accepted file.**
   - Reads the file content.
   - For `.txt`, `.text`, and `.markdown` files: renames them to `.md` and reviews the content for basic Markdown formatting issues (blank lines before headings, consistent list markers, etc.). It does not add new content or change meaning.
   - Determines the target path inside the vault, preserving the relative folder structure from the source.
   - Creates any missing parent directories in the vault.
   - Writes the file to the vault.
   - Removes the original file from the source location after a successful write.

4. **Handles conflicts.** If a file with the same name already exists in the vault, the skill pauses and asks whether to skip, rename, or overwrite before continuing.

5. **Reports a summary.** After all files are processed, it prints a summary of what was moved, renamed, and skipped.

---

## Supported file types

| Extension | Action |
|-----------|--------|
| `.md` | Moved as-is |
| `.markdown` | Renamed to `.md`, formatting reviewed |
| `.txt` | Renamed to `.md`, formatting reviewed |
| `.text` | Renamed to `.md`, formatting reviewed |
| All others | Skipped, reported to the user |

Files that are skipped (images, PDFs, Office documents, HTML, and any other non-text format) are never touched and are listed in the final summary.

---

## Behavior details

- **Folder structure is preserved.** If you move `~/Downloads/project/notes/draft.txt`, the file lands at `<vault>/project/notes/draft.md`, not at the vault root.
- **Original files are deleted after a successful move.** This is a move, not a copy. Verify your vault backup strategy before processing important files.
- **No content is invented.** When reformatting `.txt` or `.markdown` files, the skill only adjusts formatting, never alters meaning or adds new text.
- **Conflicts require explicit confirmation.** The skill never silently overwrites an existing vault file.

---

## Output summary

After processing, the skill prints a table in this format:

```
Moved:    X files
Renamed:  Y files (.txt/.text/.markdown -> .md)
Skipped:  Z files (list filenames)
```

---

## Links

- [Claude Code skills documentation](https://code.claude.com/docs/en/skills)
- [Obsidian](https://obsidian.md)
