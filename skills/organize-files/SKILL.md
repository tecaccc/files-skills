---
name: organize-files
description: Organize files into a two-level hierarchy (type → project) with date-prefixed file names.
argument-hint: <directory> [--project <name>] [--dry-run] [--depth <n>] [--no-rename]
allowed-tools: Read, Edit, Write, Glob, Bash, Grep
version: 1.1.0
---

# Organize Files

Organize files into a **类型 → 项目** two-level folder structure, with modification dates embedded into file names.

## Input

- `<directory>` (required): The target directory to organize.
- `--project` (optional): Force all files into this project subfolder.
- `--dry-run` (optional): Only print the planned moves without executing.
- `--depth` (optional): How deep to scan. Defaults to `1` (only immediate files).
- `--no-rename` (optional): Move files only, skip date-prefix renaming.

## Phase 1 — Analyze

1. Scan all files in `<directory>` up to the specified depth.
2. For each file, gather:
   - File name and extension
   - File size
   - Modification date
   - MIME type or inferred category
3. Read the organization specs from `${CLAUDE_SKILL_DIR}/specs/`:
   - `file-categories.md` — defines file type → category mappings
   - `organization-schemes.md` — defines the two-level structure and naming rules
4. For each file, determine:
   - **Type folder**: based on extension → category mapping.
   - **Project folder**: inferred from `--project`, file name prefix, or existing directory context. Falls back to `未分类`.
   - **New file name**: `YYYY-MM-DD_<cleaned-name>.<ext>` (unless `--no-rename`).

## Phase 2 — Plan

1. Generate the full `类型/项目/` folder structure that will be created.
2. Generate a move+rename plan: each entry shows `source → target`.
3. Detect conflicts:
   - Target path already exists → append numeric suffix.
   - Two files would land at the same path → differentiate by name.
4. If `--dry-run`: print the plan and stop.
5. Otherwise, present the plan to the user for confirmation.

## Phase 3 — Execute

1. Create all target directories.
2. Move and rename files to their target locations.
3. Print a summary:
   - Total files processed
   - Files moved per type/project
   - Empty directories left behind (if any)

## Rules

- NEVER delete files. Only move or rename.
- NEVER process hidden files (starting with `.`) unless explicitly asked.
- If a file is already in the correct location with correct naming, skip it.
- Preserve file extensions during moves.
- Always confirm with the user before executing moves (unless `--dry-run`).
- Use the category definitions in `${CLAUDE_SKILL_DIR}/specs/file-categories.md` for type mapping.
- Use the structure and naming rules in `${CLAUDE_SKILL_DIR}/specs/organization-schemes.md`.
