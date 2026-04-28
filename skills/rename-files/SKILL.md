---
name: rename-files
description: Batch rename files in a directory following consistent naming conventions. Supports date-prefix, sequential numbering, project-prefix, and custom patterns.
argument-hint: <directory> [--pattern <pattern>] [--preview] [--recursive]
allowed-tools: Read, Edit, Write, Glob, Bash, Grep
version: 1.0.0
---

# Rename Files

Batch rename files in a directory to follow consistent, clean naming conventions.

## Input

- `<directory>` (required): The target directory containing files to rename.
- `--pattern` (optional): Naming pattern to apply. Defaults to `clean`.
  - `clean`: Remove special characters, normalize spaces, standardize case.
  - `date-prefix`: Prepend the file's modification date (YYYY-MM-DD_).
  - `sequential`: Rename to a sequential numbered pattern (001, 002, ...).
  - `project-prefix`: Prepend a project/context identifier.
  - `lowercase`: Convert all file names to lowercase.
  - `chinese`: Normalize Chinese file names (full-width → half-width, unify punctuation).
- `--preview` (optional): Show the rename plan without executing.
- `--recursive` (optional): Process files in subdirectories as well.

## Phase 1 — Load Rules

1. Read the naming conventions from `${CLAUDE_SKILL_DIR}/specs/naming-conventions.md`.
2. Determine which pattern to apply based on the `--pattern` argument.
3. Establish the rules for the chosen pattern.

## Phase 2 — Analyze & Plan

1. Scan all files in `<directory>` (and subdirectories if `--recursive`).
2. Skip hidden files (starting with `.`).
3. For each file, compute the new name based on the selected pattern.
4. Build a rename plan showing `old_name → new_name` for each file.
5. Detect conflicts:
   - If the new name collides with an existing file, append a numeric suffix.
   - If the file is already correctly named, mark as `skipped`.
6. If `--preview`: print the plan and stop.
7. Otherwise, present the plan to the user for confirmation.

## Phase 3 — Execute

1. Rename files according to the plan.
2. Handle edge cases:
   - Cross-device renames (copy + delete fallback).
   - Case-only renames on case-insensitive file systems (use temp name).
3. Print a summary:
   - Total files renamed
   - Files skipped (already correct)
   - Conflicts resolved

## Rules

- NEVER change file extensions unless the user explicitly asks.
- NEVER rename files in system directories or hidden directories.
- Always preserve file extensions.
- Always confirm with the user before executing renames (unless `--preview`).
- Idempotent: running the skill twice should produce the same result on the second run (no double-renaming).
- Follow the conventions in `${CLAUDE_SKILL_DIR}/specs/naming-conventions.md`.
