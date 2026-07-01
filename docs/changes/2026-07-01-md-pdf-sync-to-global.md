# 2026-07-01. md-pdf 同步到全局 + 主题参数化 + 字体扩展

- **日期**：2026-07-01
- **类型**：Feature / Skill
- **影响范围**：`~/.claude/skills/md-pdf/`（全局 skill 完全重写）
- **相关计划**：[`skill-zazzy-cocoa.md`](../../.claude/plans/skill-zazzy-cocoa.md)

## 背景

`md-pdf` skill 原本只在本项目 `.opencode/skills/md-pdf/`，且最近做的关键改动（数学符号真实显示、Arial Unicode 字体切换）未推送到全局 `~/.claude/skills/md-pdf/`。本次同步把项目特化版本改造为**通用化、可主题切换、可字体切换**的全局 skill。

## 改动清单

| 文件 | 改动 |
|---|---|
| `~/.claude/skills/md-pdf/scripts/convert_md_to_pdf.py` | 完全重写：THEMES 字典（4 套主题）、JetBrains Mono 拉丁字符路由（代码块专用）、CLI 参数 (`--theme`, `--chapter-name`)、颜色查表（`self._t("body")`）、Arial Unicode 默认正文字体 |
| `~/.claude/skills/md-pdf/SKILL.md` | 重写：去项目化，主题/字体/CLI 参数文档 |
| `~/.claude/skills/md-pdf/REFERENCE.md` | 重写：主题系统 + 字符路由原理 + 字体配置 |
| `~/.claude/skills/md-pdf/examples/example_qa.md` | 重写：中性示例（混合中英文 + 数学符号 + 代码块 + 表格） |

## 4 套主题

| 主题 | 背景 | 主调 | 适合 |
|---|---|---|---|
| `claude-brown`（默认） | 信纸米 `#FFFBF5` | 深棕/琥珀橙 | 默认/笔记/复习 |
| `academic` | 纯白 `#FFFFFF` | 黑白灰 | 论文/正式文档 |
| `bilibili-pink` | 纯白 `#FFFFFF` | B站粉 `#FB7299` | 娱乐/分享 |
| `lol` | 深蓝 `#0A0E27` | LoL 金 `#C8AA6E` | 游戏/视觉冲击 |

## 字体策略

| 字符类型 | 字体 | 文件 |
|---|---|---|
| 标题 | STHeiti Medium | `/System/Library/Fonts/STHeiti Medium.ttc` |
| 正文 | Arial Unicode（38K 字形含 CJK + Latin + math） | `/System/Library/Fonts/Supplemental/Arial Unicode.ttf` |
| 代码块 | JetBrains Mono Regular | `/Users/sunshine/Library/Fonts/JetBrainsMono-Regular.ttf` |

**字符路由**：`_write_rich` / `add_heading` / `_render_code_block` 三个方法在遇到 math 字符（U+2070-U+209F / U+2200-U+22FF / U+2700-U+27BF）时切到 Songti（Arial Unicode）以保证真实显示。代码块内 CJK 字符也切到 Songti。

## CLI 参数

```bash
python3 ~/.claude/skills/md-pdf/scripts/convert_md_to_pdf.py input.md [output.pdf] [选项]

选项:
  --theme NAME          # 主题：claude-brown / academic / bilibili-pink / lol
  --chapter-name TEXT   # 页脚章节名
```

## 验证结果

**验证 1（本项目回归）**：
- 转换 `大题专项_练习题.md` 和 `大题专项_练习题解析.md`
- 唯一警告：`'️' (️)` 变体选择符（零宽无害字符）
- 视觉确认无位置偏移，数学符号真实显示，章节结构清晰

**验证 2（临时项目 4 主题）**：
- `/tmp/test-md-pdf/` 创建临时项目，4 主题各生成一份 PDF
- 字符统计：每份 PDF 都有 ⋈:1, ∃:1, ∀:1, ₂:4
- 视觉确认：4 套主题颜色明显不同，字体可读，符号真实显示

## 修复的 bug

### 位置偏移问题

**症状**：第一次实现"per-character 字体路由"时，PDF 视觉上出现严重错位（标题内容与下一行内容叠加）。

**根因**：Arial Unicode、STHeiti、JetBrains Mono 三种字体的 ascent/descent 不同，混排时 y 推进量不一致。

**解决**：取消正文和标题的 per-character 路由，统一使用 Arial Unicode 作为唯一正文字体。JetBrains Mono 仅用于代码块（明确分隔区域，行高不影响相邻内容）。仅在 `add_heading` 标题和 `_write_rich` bold 片段中保留 math 字符的轻量级路由（math 字符切到 Songti，其他字符保持原字体——只有 2 个字体参与，行高差异小）。

### JetBrains Mono 警告

**症状**：完整转换时 fpdf2 警告"MPDFAA+JetBrainsMono is missing the following glyphs: '调', '度', '姓'..."

**根因**：代码块和 bold 标题中含 CJK 字符，JetBrains Mono 不含这些字符。

**解决**：代码块和 bold 标题做 per-character 切字体——CJK 字符切到 Songti（Arial Unicode）。

## 未变更

- `~/.claude/skills/md-pdf/scripts/ocr_pdf.swift`（OCR 工具，与本次任务无关）
- `~/.claude/skills/md-pdf/scripts/merge_qa.py`（题目合并，与本次任务无关）
- `~/.claude/skills/md-pdf/scripts/make_lined_paper.py`（信纸模板，与本次任务无关）
- `~/.claude/skills/md-pdf/scripts/check_residue.py`（残留检查，与本次任务无关）
- `~/.claude/skills/md-pdf/assets/ornament.png`（装饰图标）
- 本地 `.opencode/skills/md-pdf/`（保留项目特化版本）
- `ai-lecture` skill（未涉及）

## 后续可考虑

- 表格内 Latin 字符 fallback 到 CJK 字体的 Latin 部分（已知限制，`cell()` 不支持混字体）
- `--font-latin` / `--font-cjk` CLI 参数（让非 macOS 用户可覆盖字体路径）
- 装饰图标在 dark theme（lol）下的反色处理
