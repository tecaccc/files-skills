# File Categories

Simplified file type to category mappings.

| Category | Extensions |
|---|---|
| 文档 | `.pdf` `.doc` `.docx` `.xls` `.xlsx` `.ppt` `.pptx` `.txt` `.rtf` `.md` `.csv` |
| 图片 | `.jpg` `.jpeg` `.png` `.gif` `.svg` `.webp` `.bmp` `.tiff` `.ico` `.psd` `.ai` |
| 视频 | `.mp4` `.avi` `.mkv` `.mov` `.wmv` `.flv` |
| 音频 | `.mp3` `.wav` `.flac` `.aac` `.ogg` `.wma` |
| 代码 | `.js` `.ts` `.jsx` `.tsx` `.py` `.java` `.kt` `.go` `.rs` `.c` `.cpp` `.h` `.html` `.css` `.scss` `.json` `.yaml` `.yml` `.xml` `.toml` |
| 其他 | 不匹配以上任何扩展名的文件（含压缩包 `.zip` `.rar` `.7z` 等） |

## Rules

- First-level folders use these exact names: `文档/` `图片/` `视频/` `音频/` `代码/` `其他/`.
- No sub-categories within the first level. Keep it flat and simple.
- Compressed archives fall into `其他/` since they are transient files — unpack and organize the contents instead.
