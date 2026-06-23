# 2026-06-23. 重命名 skill：`pdf-to-exam` → `md-pdf`

- **日期**：2026-06-23
- **类型**：Refactor / Skill
- **影响范围**：`.opencode/skills/md-pdf/`、`AGENTS.md`、`.opencode/skills/ai-lecture/SKILL.md`
- **相关 ADR**：[`0001-rename-pdf-to-exam-to-md-pdf.md`](../adr/0001-rename-pdf-to-exam-to-md-pdf.md)

## 改动清单

| 文件 | 改动 |
|---|---|
| `.opencode/skills/pdf-to-exam/` | 目录重命名为 `.opencode/skills/md-pdf/` |
| `.opencode/skills/md-pdf/SKILL.md` | 重写：frontmatter name + 标题 + 描述（MD→PDF 为主）+ 章节顺序调整 |
| `.opencode/skills/md-pdf/REFERENCE.md` | 标题 + 5 处路径更新 |
| `AGENTS.md` | 技能表标题 + 6 处路径 + 新增"横线格信纸"命令条目 |
| `.opencode/skills/ai-lecture/SKILL.md` | 6 处交叉引用（4 处路径 + 2 处文字提及） |

## 未变更

- `plans/` 下 3 个历史归档文件里的旧路径 —— 历史归档不重写
- 脚本内 `Path(__file__).resolve().parent.parent` —— 相对路径自动适配
- 产物 PDF 文件名（`横线格信纸_横向A4.pdf` / `_纵向A4.pdf`）—— 与 skill 目录名无关
- 字体路径（macOS 系统路径硬编码）

## 验证记录

执行了 3 项验证：

1. **`make_lined_paper.py` 跑通**
   ```
   python3 .opencode/skills/md-pdf/scripts/make_lined_paper.py -o P --out /tmp/test_md_pdf.pdf
   → 已生成: 1 页 A4 纵 210×297 mm
   ```

2. **资源路径自动适配**
   ```
   ORNAMENT_PATH = /Users/sunshine/Desktop/期末复习/.opencode/skills/md-pdf/assets/ornament.png
   ```
   目录改名后，相对路径 `parent.parent / "assets" / "ornament.png"` 解析到新位置 ✓

3. **字体路径不变**
   ```
   FONT_HEADING: /System/Library/Fonts/STHeiti Medium.ttc
   ```

## 后续待办

- 用户需重启 OpenCode 让新 SKILL.md 生效（SKILL.md 改完后需重启才会被自动加载）
- 未来若启用 OpenSpec 流程，可把本 change 转写为 OpenSpec proposal
