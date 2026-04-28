# Cleanup Rules

Definitions for what constitutes temporary/junk files and cleanup policies.

## Temporary File Patterns

Files matching these name patterns are considered temporary:

| Pattern | Description | Example |
|---|---|---|
| `~$*` | Office lock files | `~$报告.docx` |
| `~*` | Editor temp files | `~file.txt` |
| `*.tmp` | Generic temp files | `data.tmp` |
| `*.temp` | Generic temp files | `config.temp` |
| `*.bak` | Backup files | `file.bak` |
| `*.swp` | Vim swap files | `.file.swp` |
| `*.swo` | Vim swap files | `.file.swo` |
| `*~` | Backup files (Unix convention) | `file~` |
| `.DS_Store` | macOS metadata | `.DS_Store` |
| `Thumbs.db` | Windows thumbnails | `Thumbs.db` |
| `desktop.ini` | Windows folder config | `desktop.ini` |
| `*.log` | Log files (optional) | `debug.log` |
| `*.cache` | Cache files | `data.cache` |
| `*.part` | Partial downloads | `video.part` |
| `*.crdownload` | Chrome partial downloads | `file.crdownload` |
| `*.download` | Safari partial downloads | `file.download` |

## Temp Directory Names

Directories with these names are treated as temp directories:

| Name | Description |
|---|---|
| `tmp` | Generic temp |
| `temp` | Generic temp |
| `Temp` | Windows temp |
| `__pycache__` | Python cache |
| `.cache` | Application cache |
| `.tmp` | Hidden temp |

## Duplicate Detection

### Hash Algorithm

Use SHA-256 for file content hashing. It is fast and collision-resistant.

### Size-Based Pre-Filter

Before hashing, group files by exact byte size. Only files with identical sizes need to be hashed against each other. This dramatically reduces the number of hash computations.

### File Size Threshold

Files smaller than `--min-size` (default 1KB) are excluded from duplicate scanning. These tiny files are typically config files, thumbnails, or metadata and are rarely worth deduplicating.

### Keep Policy

When duplicates are found, keep the file that:
1. Has the earliest modification date (likely the original), OR
2. Is in the most "canonical" location (deeper in the file tree, not in a temp/download folder).

## Empty Directory Policy

### Detection

A directory is considered empty if it contains no regular files, recursively. Directories that only contain other empty directories are also considered empty.

### Protection

Never delete directories that are:
- The root directory being scanned.
- Hidden directories (`.git`, `.svn`, etc.).
- System directories.

## Zero-Byte Files

Files with 0 bytes size. These are usually artifacts from interrupted downloads, crashed programs, or empty placeholders.

Always report zero-byte files separately from duplicates, since they may be intentional (e.g., `.gitkeep`).

## Exclusions

These files/directories should NEVER be included in any cleanup scan:

- `.git/` and `.svn/` — version control internals
- `node_modules/` — package dependencies
- `.venv/`, `venv/` — Python virtual environments
- `.env` — environment configuration
- Any file the user has open (check for lock indicators)
