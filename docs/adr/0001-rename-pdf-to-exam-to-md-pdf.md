# 0001. 将 `pdf-to-exam` skill 重命名为 `md-pdf`

- **状态**：Accepted
- **日期**：2026-06-23
- **决策者**：项目所有者

## 背景 / Context

`.opencode/skills/pdf-to-exam/` 最初为「PDF 笔记 → 章节练习题」而建，能力边界与名称一致。

但实际承担能力已超出原范围：

1. 新增 `make_lined_paper.py` —— 生成空白横线格信纸模板，与"考试"完全无关
2. 现有 `convert_md_to_pdf.py` —— 核心能力是 MD→PDF 渲染，被 `ai-lecture` 等其他 skill 复用
3. 实际能力矩阵：MD→PDF 渲染（核心） + OCR + 题目合并/残留检查 + 模板生成

「pdf-to-exam」已不能准确反映能力边界，名称与实际用途长期偏离。

## 决策 / Decision

将 skill 重命名为 `md-pdf`。

## 评估的选项

### 选项 A：`md-pdf`（已选）

- 体现核心能力 MD→PDF
- 与用户口述"用 md-pdf 的风格"对齐
- 短小好记

### 选项 B：`md-to-pdf`

- 明确"源 → 目标"过程
- 缺点：名字更长，调用命令时啰嗦

### 选项 C：`pdf-tools`

- "PDF 工具集"语义
- 缺点：偏通用、丢失特色，无法传达"项目主题色风格"的核心价值

### 选项 D：保留 `pdf-to-exam`

- 不动
- 缺点：与实际能力持续偏离，未来再加新工具会进一步脱节

## 后果 / Consequences

### 改动清单

- 目录 `.opencode/skills/pdf-to-exam/` → `.opencode/skills/md-pdf/`
- `.opencode/skills/md-pdf/SKILL.md` 重写：
  - frontmatter `name: md-pdf`
  - description 以 MD→PDF 为主，OCR / 题目 / 模板列为 secondary
  - Workflow 顺序调整，MD→PDF 放到第 1 节
- `.opencode/skills/md-pdf/REFERENCE.md` 标题 + 5 处路径
- `AGENTS.md` 标题 + 6 处路径 + 新增横线格信纸命令条目
- `.opencode/skills/ai-lecture/SKILL.md` 6 处交叉引用

### 迁移成本

- **低**：skill 目录不在 git 跟踪中（`git ls-files` 为空），纯本地重命名
- 脚本内 `Path(__file__).resolve().parent.parent` 是相对路径，目录改名后 `assets/ornament.png` 自动适配（已验证）

### 未变更项

- `plans/` 下 3 个历史归档文件（Java 刷题速成 / 软件工程复习 / 软件工程选择题 / PDF 脚本优化计划）保留旧名引用 —— 历史归档不重写
- 产物 PDF 文件名（如 `横线格信纸_横向A4.pdf`）—— 与 skill 目录名无关
- 字体路径仍硬编码为 macOS 系统路径

### 后续待办

- 用户需重启 OpenCode 让新 SKILL.md 生效
- 未来若启用 OpenSpec 流程（`openspec/changes/`），可把本次 change 转写为 OpenSpec proposal

## 替代方案被否决的原因

`md-to-pdf` 更长、`pdf-tools` 太泛、保留原名持续偏离 —— 见上文选项评估。
