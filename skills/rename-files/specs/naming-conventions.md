# Naming Conventions

File naming rules and patterns for consistent, clean file names.

## General Rules

These rules apply to ALL naming patterns as a baseline cleanup:

1. **Remove special characters**: Strip `!@#$%^&*+=[]{}|;:'",<>?` from file names.
2. **Normalize spaces**: Replace multiple spaces with a single space. Replace spaces with `-` or `_` based on pattern.
3. **Remove leading/trailing spaces and dots**: File names must not start or end with spaces or dots.
4. **Preserve extensions**: Never modify the file extension.
5. **Max length**: Truncate file names to 200 characters (excluding extension) if they exceed this limit.
6. **No duplicate separators**: Collapse consecutive `-` or `_` into a single one.

## Pattern: clean

Standardize file names without changing their core identity.

**Rules:**
- Replace spaces with `-`.
- Remove special characters (keep: `-`, `_`, `.`, Chinese characters, alphanumeric).
- Remove leading/trailing `-` or `_`.
- Keep original casing (do not force lower/upper case).

**Examples:**
```
"项目 报告 (最终版).pdf"    → "项目-报告-最终版.pdf"
"IMG 2024 01 15.jpg"       → "IMG-2024-01-15.jpg"
"meeting~~notes!!.docx"    → "meeting-notes.docx"
"  my file.txt  "          → "my-file.txt"
"___temp__file___.csv"     → "temp-file.csv"
```

## Pattern: date-prefix

Prepend the file's modification date for chronological sorting.

**Rules:**
- Prefix format: `YYYY-MM-DD_`
- If the file name already starts with a date (YYYY-MM-DD or YYYYMMDD), replace it.
- The rest of the name follows `clean` pattern rules.

**Examples:**
```
"项目报告.pdf"            → "2025-03-15_项目报告.pdf"
"2024-01-01_photo.jpg"   → "2024-01-01_photo.jpg"  (already has date, no change)
"20240115_notes.txt"     → "2024-01-15_notes.txt"  (normalize date format)
```

## Pattern: sequential

Rename files to a sequential numbered pattern while preserving context.

**Rules:**
- Format: `{prefix}-{NNN}.{ext}` where NNN is zero-padded (001, 002, ...).
- Prefix is derived from the original file name or directory name.
- Sort order: alphabetical by original file name, or by modification date.

**Examples:**
```
"screenshot (1).png"  → "screenshot-001.png"
"screenshot (2).png"  → "screenshot-002.png"
"screenshot (3).png"  → "screenshot-003.png"

// Or with directory-based prefix in a folder called "旅行照片":
"DSC_0001.JPG"  → "旅行照片-001.JPG"
"DSC_0002.JPG"  → "旅行照片-002.JPG"
```

## Pattern: project-prefix

Prepend a project identifier to all files in a directory.

**Rules:**
- Format: `{PROJECT}_{cleaned-name}.{ext}`
- The project identifier is inferred from the parent directory name or provided by the user.
- Underscore separates project prefix from file name.

**Examples:**
```
// In a directory called "安恒项目":
"需求文档v2.pdf"  → "安恒项目_需求文档v2.pdf"
"接口设计.docx"   → "安恒项目_接口设计.docx"
```

## Pattern: lowercase

Convert file names to lowercase.

**Rules:**
- Convert all ASCII letters to lowercase.
- Chinese characters remain unchanged.
- Apply `clean` pattern rules as well.

**Examples:**
```
"README.MD"        → "readme.md"
"Final Report.PDF" → "final-report.pdf"
"项目文件 V2.DOCX"  → "项目文件-v2.docx"
```

## Pattern: chinese

Normalize Chinese file names specifically.

**Rules:**
- Convert full-width characters to half-width: `Ａ→A`, `１→1`, `（→(`, `）→)`.
- Unify Chinese punctuation to standard forms: `。`, `，`, `！`, `？` kept; `．→。`, `､→，`.
- Remove redundant Chinese/English mixed spaces.
- Apply `clean` pattern rules.

**Examples:**
```
"项目报告（最终版）．pdf"  → "项目报告(最终版).pdf"
"文件　名称   测试.docx"   → "文件名称-测试.docx"
"第１章　概述.pdf"         → "第1章-概述.pdf"
```

## Conflict Resolution

When a renamed file would collide with an existing file:

1. Append a numeric suffix before the extension: `file-2.pdf`, `file-3.pdf`.
2. If the collision is with the original file itself (no change needed), skip.
3. For case-only conflicts on case-insensitive systems (e.g., `File.txt` → `file.txt`), use a temporary name first.
