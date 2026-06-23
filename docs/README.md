# 项目文档

本目录是工作区级文档根目录，记录项目内的架构决策、改动历史、策划计划、复盘反思与写作规范。

## 子目录索引

### `docs/adr/` — 架构决策记录

记录"**为什么这么改**"——背景、评估的选项、最终决定与后果。

- 命名规范：`NNNN-简短标题.md`（4 位编号，自增，标题用连字符分隔）
- 状态：Proposed / Accepted / Deprecated / Superseded
- 模板：Michael Nygard 6 段式（背景 / 决策 / 选项 / 后果）

### `docs/changes/` — 改动记录

记录"**改了什么、何时改**"——具体改动清单、验证记录、相关 ADR。

- 命名规范：`YYYY-MM-DD-简短标题.md`
- 类型标签：Feature / Refactor / Fix / Docs / Skill / …
- 影响范围：列出受影响的文件路径

### `docs/plans/` — 策划与计划

前向策划型文档（"**要做什么、按什么步骤**"），与 `changes/` 的后向记录互补。

- 内容范围：复习资料生成计划、深度优化计划、后续待办
- 命名规范：`{主题}-计划.md` / `{主题}-优化计划.md` / `{主题}-{优先级}-后续计划.md`

### `docs/retrospectives/` — 复盘与错误教训

事后反思型文档（"**踩过什么坑、下次怎么避免**"），是 post-mortem / retrospective 的归档。

- 内容范围：阶段生成回顾、错误教训、流程改进
- 命名规范：`{主题}-{阶段/类别}-错误教训.md`

### `docs/guides/` — 写作与生成规范

写作与生成任务的标准说明（"**做这件事要遵守的规则**"），生成特定类型产物前必读。

- 内容范围：题目深度规格、解析深度规格、风格模板
- 命名规范：`{产物类型}-深度规格说明.md`

### `docs/templates/` — 可打印 PDF 模板

用于手写/打印的 PDF 模板文件（信纸、答题卡、通用稿纸等），按学科或场景分类。

- 内容范围：横线格信纸、方格作文本、答卷模板
- 文件命名：`{模板名}_{规格}.pdf`

## 当前统计

- ADR 数：1
- 最新改动：2026-06-23（[rename-skill-to-md-pdf](changes/2026-06-23-rename-skill-to-md-pdf.md)）
- 策划计划：5 个文件（详见 [plans/](plans/)）
- 复盘文档：3 个（详见 [retrospectives/](retrospectives/)）
- 规范文档：2 个（详见 [guides/](guides/)）
- 模板文件：2 个（详见 [templates/](templates/)）

## 阅读入口

- 最新决策：[`docs/adr/0001-rename-pdf-to-exam-to-md-pdf.md`](adr/0001-rename-pdf-to-exam-to-md-pdf.md)
- 最新改动：[`docs/changes/2026-06-23-rename-skill-to-md-pdf.md`](changes/2026-06-23-rename-skill-to-md-pdf.md)
- 策划计划索引：[`docs/plans/`](plans/)
- 复盘索引：[`docs/retrospectives/`](retrospectives/)
- 规范索引：[`docs/guides/`](guides/)
- 模板索引：[`docs/templates/`](templates/)
