---
name: clean-files
description: Scan a directory for duplicate files, temporary files, and empty directories. Report findings and optionally clean them up to reclaim disk space.
argument-hint: <directory> [--mode duplicates|temp|empty|all] [--delete] [--min-size <bytes>]
allowed-tools: Read, Edit, Write, Glob, Bash, Grep
version: 1.0.0
---

# Clean Files

Scan directories for junk files, duplicates, and empty folders to keep your file system clean.

## Input

- `<directory>` (required): The target directory to scan.
- `--mode` (optional): What to scan for. Defaults to `all`.
  - `duplicates`: Find duplicate files (by content hash).
  - `temp`: Find temporary and junk files.
  - `empty`: Find empty directories and zero-byte files.
  - `all`: Run all three scans.
- `--delete` (optional): Actually delete found items (default is report-only).
- `--min-size` (optional): Minimum file size to consider for duplicate scanning. Defaults to `1024` (1KB, skip tiny files).

## Phase 1 — Load Rules

1. Read the cleanup rules from `${CLAUDE_SKILL_DIR}/specs/cleanup-rules.md`.
2. Determine which scan modes to run.

## Phase 2 — Scan

Run each applicable scan mode:

### Duplicate Scan
1. Recursively list all files with size > `--min-size`.
2. Group files by size — same-size files are duplicate candidates.
3. For each same-size group, compute file hashes (MD5 or SHA-256).
4. Group by hash to find true duplicates.
5. For each duplicate group, mark the oldest/original file as "keep" and the rest as "duplicate".

### Temp File Scan
1. Match file names against the temp file patterns in `cleanup-rules.md`.
2. Match common temp directory names (`tmp/`, `temp/`, `~$*`).
3. Report each match with its size and modification date.

### Empty Scan
1. Find all zero-byte files.
2. Find all directories that contain no files (recursively).

## Phase 3 — Report & Clean

1. Print a summary report:
   - Duplicate groups: X groups, Y files, Z bytes reclaimable
   - Temp files: X files, Y bytes
   - Empty directories: X directories
   - Zero-byte files: X files
2. If `--delete` is set and user confirms:
   - Delete duplicate files (keep the original).
   - Delete temp files.
   - Delete empty directories.
   - Delete zero-byte files.
   - Print a final summary of what was deleted.

## Rules

- NEVER delete files without explicit user confirmation.
- NEVER scan system directories (`/System`, `C:\Windows`, `/usr`, etc.).
- NEVER process hidden directories (`.git`, `.svn`, `node_modules`, `__pycache__`, etc.) unless explicitly asked.
- Always compute full file hashes for duplicate detection — never rely on name or size alone.
- For duplicates, always keep the file with the earliest modification date (likely the original).
- Report file sizes in human-readable format (KB, MB, GB).
