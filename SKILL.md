---
name: obsidian-noter
description: Move documents or folders into the Obsidian vault. Operates only on Markdown and plain text files. Text files (.txt, .text) are renamed and reformatted to .md. Non-text files are skipped. Folder structure is preserved.
disable-model-invocation: true
allowed-tools: Bash, Read, Write, Glob, Grep
argument-hint: [source-path]
---

# Obsidian Noter

Move Markdown and plain text files into the Obsidian vault at `~/Obsidian-vault-01`, preserving folder structure.

## Rules

1. **Only operate on Markdown and plain text files.** Accepted extensions: `.md`, `.markdown`, `.txt`, `.text`
2. **Skip all other file types** (images, PDFs, Office documents, HTML, etc.). Report skipped files to the user.
3. **Rename and reformat text files to Markdown.** Any `.txt` or `.text` file must be renamed to `.md`. Review the content and ensure it uses valid Markdown formatting (headings, lists, paragraphs). Do not invent content, only adjust formatting where needed.
4. **Preserve folder structure.** When moving a folder, recreate the same directory hierarchy inside the vault.
5. **Do not overwrite existing files.** If a file with the same name already exists in the vault, ask the user how to proceed (skip, rename, or overwrite).
6. **Report results.** After processing, print a summary: files moved, files renamed (.txt to .md), files skipped.

## Workflow

Given `$ARGUMENTS` as the source path:

### Step 1: Validate source

- Verify the source path exists.
- Determine if it is a file or a directory.

### Step 2: Discover files

- If the source is a single file, process that file.
- If the source is a directory, recursively find all files within it.
- Classify each file as accepted (`.md`, `.markdown`, `.txt`, `.text`) or skipped (everything else).

### Step 3: Process each accepted file

For each accepted file:

1. Read the file content.
2. If the extension is `.txt`, `.text`, or `.markdown`:
   - Change the target extension to `.md`.
   - Review the content for basic Markdown formatting. Fix obvious issues (e.g., add blank lines before headings, ensure list markers are consistent). Do not alter the meaning or add new content.
3. Determine the target path inside the vault (`~/Obsidian-vault-01`), preserving relative folder structure from the source.
4. Check if the target path already exists. If so, ask the user before proceeding.
5. Create any necessary parent directories in the vault.
6. Write the file to the target location.
7. Remove the original file from the source location after successful write.

### Step 4: Summary

Print a summary table:

```
Moved:    X files
Renamed:  Y files (.txt/.text/.markdown -> .md)
Skipped:  Z files (list filenames)
```
