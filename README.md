# LLM Wiki — AI 知识维基

基于 [Karpathy 的 llm-wiki 模式](llm-wiki.md) 构建的、由 LLM 增量维护的个人知识库。

## 核心理念

不同于传统 RAG「每次查询从零检索」，本项目让 LLM **持续编译并维护一份结构化维基**：

- 原始资料放入 `raw/`，作为不可变的真相来源
- LLM 阅读资料后，在 `wiki/` 中创建/更新摘要、概念页、实体页、综合页
- 知识随每次导入而**叠加**，交叉引用与矛盾标记已预先建立
- 人类策展资料、提出问题；LLM 负责繁重的维护工作

## 快速开始

### 1. 导入一篇资料

```text
将文章保存到 raw/ 目录，然后对 Agent 说：

「请 ingest raw/xxx.md」
```

Agent 将：创建来源摘要 → 提取概念/实体 → 更新综合页 → 维护交叉引用 → 更新 index 与 log。

### 2. 提问

```text
「根据维基，XXX 和 YYY 有什么本质区别？」
```

有价值的分析会自动归档到 `wiki/queries/`。

### 3. 健康检查

```text
「请对维基做一次 lint」
```

## 目录结构

| 路径 | 用途 |
|------|------|
| `AGENTS.md` | Agent 工作规范（结构、模板、流程） |
| `index.md` | 维基内容目录 |
| `log.md` | 操作时间线 |
| `raw/` | 原始资料（只读） |
| `raw/assets/` | 图片与附件 |
| `wiki/overview.md` | 全局概览 |
| `wiki/concepts/` | 概念页 |
| `wiki/entities/` | 实体页 |
| `wiki/sources/` | 来源摘要 |
| `wiki/synthesis/` | 跨来源综合 |
| `wiki/comparisons/` | 对比分析 |
| `wiki/queries/` | 查询归档 |

## 写作要求

维基文章由 LLM 撰写，须满足 `AGENTS.md` 中的标准：

- **结构化**：固定 frontmatter + 章节骨架
- **客观独立**：区分来源观点与综合判断，显式记录矛盾
- **足够深度**：禁止泛泛而谈；概念/来源/综合页有最低篇幅与论证要求
- **交叉引用**：实体概念首次提及须链接，每页有「相关页面」节

## 推荐工具

- **Obsidian**：浏览维基、图谱视图、双向链接
- **Obsidian Web Clipper**：将网页文章剪藏为 markdown 放入 `raw/`
- **Git**：版本管理与协作

## 参考

- 模式说明：[llm-wiki.md](llm-wiki.md)（Karpathy Gist 中文版）
- Agent 规范：[AGENTS.md](AGENTS.md)
