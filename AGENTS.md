# LLM Wiki Agent 规范

本文件是维基维护者的**唯一权威配置**。所有 ingest、query、lint 操作必须遵循本文档。

## 项目定位

这是一个由 LLM 增量构建与维护的个人知识维基。人类负责**策展原始资料、提出好问题、指导分析方向**；LLM 负责**阅读、提炼、交叉引用、更新、一致性维护**。

核心原则：**维基是持久、可叠加的产物**——知识编译一次后保持最新，而非每次查询从零检索。

## 目录结构

```
llm_wiki_ai_dev/
├── AGENTS.md          # 本文件：schema 与工作流
├── README.md          # 人类可读的项目说明
├── index.md           # 内容目录（按类别）
├── log.md             # 时间线日志（仅追加）
├── raw/               # 原始资料（只读，永不修改）
│   └── assets/        # 图片与附件
└── wiki/              # LLM 生成与维护的维基内容
    ├── overview.md    # 全局概览与当前综合论点
    ├── concepts/      # 概念页：抽象思想、机制、方法论
    ├── entities/      # 实体页：人物、组织、产品、项目
    ├── sources/       # 来源摘要：每份 raw 资料对应一页
    ├── synthesis/     # 综合页：跨来源的主题综合
    ├── comparisons/   # 对比分析
    └── queries/       # 查询产出的分析，归档入库
```

### 三层架构

| 层 | 路径 | 规则 |
|---|---|---|
| 原始资料 | `raw/` | **不可变**。只读，不编辑、不删除内容。新资料以新文件加入。 |
| 维基 | `wiki/` | LLM 全权维护。创建、更新、交叉引用、修订矛盾。 |
| Schema | `AGENTS.md` | 定义结构、格式、工作流。随实践演进。 |

## 写作标准（强制）

所有维基页面必须满足以下标准。泛泛而谈的摘要视为不合格，需重写。

### 1. 结构化

每页必须包含 YAML frontmatter 和固定章节骨架（见下方模板）。不允许只有几段散文的"空壳页"。

### 2. 客观独立

- **陈述事实与论点时标明出处**：`[[sources/xxx]]` 或 `raw/xxx.md`
- **区分三层信息**：原始作者观点 / 维基综合判断 / 待验证假说
- **矛盾必须显式记录**：新资料与旧主张冲突时，在相关页面添加 `## 矛盾与修订` 节，说明双方依据与当前采信立场
- **避免空洞套话**：禁止"具有重要意义""值得关注"等无信息增益的表述；每句话应传递可验证或可推演的知识

### 3. 足够深度

- **概念页**：定义 + 机制/原理 + 边界与反例 + 与相关概念的关系 + 已知局限
- **实体页**：是什么 + 关键事实时间线 + 在知识域中的角色 + 与相关实体的关系
- **来源页**：核心论点（带页码/章节引用）+ 关键证据 + 方法论/偏见评估 + 对维基的贡献（新建/更新了哪些页面）
- **综合页**：跨 ≥2 个来源的归纳，必须有**可证伪的综合论点**，而非罗列摘要
- **最低篇幅**：来源摘要 ≥800 字；概念/实体页 ≥500 字；综合页 ≥1000 字（内容稀薄时合并到相关页，不建空页）

### 4. 交叉引用

- 首次提及的实体、概念必须使用 `[[wiki/...]]` 链接
- 每页末尾必须有 `## 相关页面` 节，列出 3–8 个相关链接
- 新建页面后，**必须**回写所有提及该主题的已有页面，添加链入链接

## 页面模板

### 通用 Frontmatter

```yaml
---
title: "页面标题"
type: concept | entity | source | synthesis | comparison | query
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
sources: []          # 来源页路径列表，如 [sources/article-x]
status: draft | stable | disputed
---
```

### 来源摘要 `wiki/sources/<slug>.md`

```markdown
---
title: "资料标题"
type: source
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
sources: []
raw_path: raw/<filename>
status: stable
---

# 资料标题

## 元信息
- **原始文件**：`raw/<filename>`
- **作者/出处**：
- **日期**：
- **资料类型**：论文 / 文章 / 报告 / 书籍章节 / 其他

## 核心论点
（逐条列出作者的核心主张，每条附章节/段落定位）

## 关键证据与数据
（具体数据、实验、案例，保留可追溯细节）

## 方法论与偏见评估
（作者如何论证？有何局限、利益冲突、样本偏差？）

## 可提取的概念与实体
（列出应从本文档新建或更新的 wiki 页面）

## 对现有维基的影响
（与哪些已有页面一致/矛盾？需更新什么？）

## 相关页面
```

### 概念页 `wiki/concepts/<slug>.md`

```markdown
---
title: "概念名"
type: concept
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
sources: [sources/...]
status: stable
---

# 概念名

## 定义
（一句话精确定义 + 展开说明）

## 机制与原理
（这个概念如何运作？因果链是什么？）

## 边界与反例
（何时不适用？已知反例？）

## 与相关概念的关系
（区分易混淆概念，说明层级关系）

## 证据基础
（哪些来源支撑上述内容？）

## 矛盾与修订
（若有来源冲突，记录于此）

## 开放问题
（目前知识缺口）

## 相关页面
```

### 实体页 `wiki/entities/<slug>.md`

```markdown
---
title: "实体名"
type: entity
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
sources: [sources/...]
status: stable
---

# 实体名

## 概述
（是什么、在知识域中的角色）

## 关键事实
（时间线或结构化事实表）

## 核心观点与立场
（若适用：该实体的主张、产品、贡献）

## 与其他实体的关系

## 证据基础

## 矛盾与修订

## 相关页面
```

### 综合页 `wiki/synthesis/<slug>.md`

```markdown
---
title: "主题综合"
type: synthesis
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: []
sources: [sources/..., sources/...]
status: draft | stable | disputed
---

# 主题综合

## 综合论点
（一句可证伪的跨来源归纳——这是整页的核心）

## 论证
（分论点 + 每个分论点的来源支撑）

## 来源间的共识

## 来源间的分歧
（分歧点、各方依据、当前采信立场及理由）

## 知识缺口
（还需要什么资料才能加强/削弱综合论点？）

## 相关页面
```

## 工作流

### Ingest（导入原始资料）

触发：用户在 `raw/` 放入新文件并说"处理/导入/ingest"。

**步骤（单次 ingest 必须全部完成）：**

1. **读取** `raw/<file>`，必要时读取 `raw/assets/` 中的图片
2. **与用户确认**（可选但推荐）：简述 3–5 条关键 takeaway，询问强调方向
3. **创建来源摘要** `wiki/sources/<slug>.md`，slug 用英文小写连字符
4. **识别新概念/实体**：为每个值得独立成页的概念/实体创建或更新页面
5. **更新综合页**：若影响跨来源论点，修订 `wiki/synthesis/` 或 `wiki/overview.md`
6. **回写交叉引用**：扫描并更新所有相关已有页面
7. **更新** `index.md`：添加新页面条目
8. **追加** `log.md` 条目

**单来源预期触达页面数**：10–15 页（来源页 + 新建/更新的概念/实体/综合页 + 交叉引用修订）

**log 条目格式：**
```markdown
## [YYYY-MM-DD] ingest | 资料标题
- raw: `raw/<filename>`
- 新建: [sources/xxx](wiki/sources/xxx.md), [concepts/yyy](wiki/concepts/yyy.md)
- 更新: [overview](wiki/overview.md), ...
- 备注: （矛盾、待跟进问题）
```

### Query（查询）

触发：用户针对维基提问。

**步骤：**

1. 先读 `index.md` 定位相关页面
2. 阅读相关 wiki 页面（必要时回溯 `raw/`）
3. 综合回答，**附引用**（`[[page]]` 或 `raw/path`）
4. 若分析有持久价值（对比、深度分析、新连接），归档到 `wiki/queries/<slug>.md`
5. 更新 `index.md`，追加 `log.md`：
   ```markdown
   ## [YYYY-MM-DD] query | 问题摘要
   - 回答归档: [queries/xxx](wiki/queries/xxx.md)（若有）
   - 涉及页面: ...
   ```

### Lint（健康检查）

触发：用户说"lint / 健康检查 / 检查维基"。

**检查项：**

- [ ] 页面间矛盾（`status: disputed` 是否都有 `## 矛盾与修订`？）
- [ ] 过时主张（新来源已取代旧说法的页面）
- [ ] 孤儿页（`index.md` 中无任何入链的页面）
- [ ] 提及但未建页的重要概念/实体
- [ ] 缺少交叉引用的页面
- [ ] 薄页（低于最低篇幅或缺乏深度结构）
- [ ] `index.md` 与 `wiki/` 实际文件是否同步

**输出：** 问题清单 + 建议修复顺序 + 建议调查的新问题/新来源。修复后追加 `log.md`。

## index.md 维护规则

`index.md` 按类别组织，每个条目格式：

```markdown
- [标题](wiki/path/to/page.md) — 一行摘要（≤80字） · 更新于 YYYY-MM-DD · 来源数 N
```

类别顺序：概览 → 综合 → 概念 → 实体 → 来源 → 对比 → 查询

## 命名约定

- 文件名：英文小写，连字符分隔，如 `attention-mechanism.md`
- 中文标题写在 frontmatter `title` 和正文 `#` 标题
- 同一实体只保留一个 canonical 页面，别名在正文中说明

## 禁止事项

- **禁止修改** `raw/` 下任何文件内容
- **禁止** 创建无来源支撑的概念/实体页（至少链接一个 `sources/` 或标注为 `status: draft` 并说明待补充来源）
- **禁止** 将聊天中的分析留在对话中而不归档——有价值的 query 产出必须写入 `wiki/queries/`
- **禁止** 跳过 `index.md` 和 `log.md` 更新

## 工具建议

- 用 Obsidian 打开本目录，利用图谱视图检查连接
- 图片引用使用相对路径 `raw/assets/...`
- 本仓库应用 git 管理，每次 ingest 后可提交变更
