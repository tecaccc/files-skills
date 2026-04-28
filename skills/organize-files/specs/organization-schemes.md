# Organization Schemes

## Core Principle

**Two-level hierarchy: Type → Project.** Date information is embedded into file names rather than folder structure.

```
<root>/
├── 文档/
│   ├── <项目A>/
│   │   ├── 2024-03-15_需求文档.pdf
│   │   └── 2024-06-01_会议纪要.docx
│   └── <项目B>/
│       └── 2025-01-10_设计方案.pdf
├── 图片/
│   ├── <项目A>/
│   │   ├── 2024-03-20_架构图.png
│   │   └── 2024-05-12_截图.jpg
│   └── 未分类/
│       └── 2025-02-28_壁纸.jpg
├── 视频/
│   ├── <项目A>/
│   └── 未分类/
├── 音频/
│   ├── <项目A>/
│   └── 未分类/
├── 压缩包/
│   ├── <项目A>/
│   └── 未分类/
├── 代码/
│   ├── <项目A>/
│   └── 未分类/
├── 安装包/
│   └── 未分类/           ← 安装包通常不属于具体项目
├── 字体/
│   └── 未分类/
└── 其他/
    └── 未分类/
```

## File Naming Convention

When organizing, rename files to embed the modification date as a prefix:

**Format:** `YYYY-MM-DD_<cleaned-original-name>.<ext>`

**Rules:**
- If the file name already starts with a date (`YYYY-MM-DD_` or `YYYYMMDD`), keep/normalize it.
- If no date, prepend the file's modification date.
- The rest of the name follows the `clean` naming pattern (see rename-files skill).
- Extension is always preserved.

**Examples:**
```
"项目报告.pdf"           → "2024-03-15_项目报告.pdf"
"IMG_20240115_photo.jpg" → "2024-01-15_photo.jpg"   (normalize existing date)
"最终方案 v2.docx"        → "2024-06-20_最终方案-v2.docx"
"readme.md"              → "2025-01-10_readme.md"
```

## Project Inference

The skill determines project assignment by priority:

1. **User hint** — if the user specifies `--project <name>`, all files go to that project.
2. **Existing subfolder** — if a file is already inside a folder named after a project, keep it.
3. **File name prefix** — common prefixes like `[项目A]_`, `项目A-`, `项目A_` are extracted.
4. **Directory context** — the parent directory name may serve as the project name.
5. **Uncategorized** — files that cannot be assigned go to `未分类/` under their type folder.

## Customization

Users can customize by:
- Editing the type categories in `file-categories.md`.
- Providing `--project <name>` to force all files into one project.
- Using `--no-rename` to skip date-prefix renaming (only move files).
