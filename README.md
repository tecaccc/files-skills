# files-skills

A collection of Claude Code skills for personal file management — organize, rename, and clean up your files with consistent conventions.

## Skills

### `/organize-files` — 文件归档整理

按照 **类型 → 项目** 两级目录结构归档文件，日期信息融入文件名。

```
文档/
├── 项目A/
│   ├── 2024-03-15_需求文档.pdf
│   └── 2024-06-01_会议纪要.docx
└── 未分类/
    └── 2025-01-10_设计方案.pdf
```

```bash
# 整理下载文件夹（自动推断项目归属）
/organize-files ~/Downloads

# 指定项目名，所有文件归入该项目
/organize-files ~/Downloads --project 安恒项目

# 仅预览，不实际执行
/organize-files ~/Desktop --dry-run

# 只移动文件，不改名
/organize-files ~/Work --no-rename
```

**目录结构：** 第一层按类型（文档/图片/视频/...），第二层按项目名
**文件命名：** 自动添加 `YYYY-MM-DD_` 日期前缀

### `/rename-files` — 批量重命名

按照统一的命名规范批量重命名文件。

```bash
# 清理文件名（去除特殊字符）
/rename-files ~/Downloads --pattern clean

# 添加日期前缀
/rename-files ~/Documents --pattern date-prefix

# 顺序编号
/rename-files ~/Screenshots --pattern sequential

# 中文文件名规范化
/rename-files ~/文件 --pattern chinese --preview
```

**支持的命名模式：**
- `clean` — 去除特殊字符，标准化空格
- `date-prefix` — 添加日期前缀（YYYY-MM-DD_）
- `sequential` — 顺序编号（001, 002, ...）
- `project-prefix` — 添加项目前缀
- `lowercase` — 全部小写
- `chinese` — 中文文件名规范化

### `/clean-files` — 清理去重

扫描目录中的重复文件、临时文件和空目录。

```bash
# 全面扫描
/clean-files ~/Downloads

# 仅查找重复文件
/clean-files ~/Documents --mode duplicates

# 仅查找临时文件
/clean-files ~/Desktop --mode temp

# 查找并删除空目录
/clean-files ~/Work --mode empty --delete
```

**支持的清理模式：**
- `duplicates` — 通过内容哈希检测重复文件
- `temp` — 查找临时文件和垃圾文件
- `empty` — 查找空目录和零字节文件
- `all` — 全部检查（默认）

## Installation

通过 Claude Code 安装：

```
/plugin install /path/to/files-skills
```

## Safety

所有技能都遵循安全原则：

- **永不自动删除文件** — 所有删除操作都需要用户确认
- **支持预览模式** — 使用 `--dry-run` 或 `--preview` 查看计划而不执行
- **跳过系统目录** — 自动排除 `.git`、`node_modules`、系统文件夹等
- **幂等操作** — 重复运行不会产生副作用

## License

MIT
