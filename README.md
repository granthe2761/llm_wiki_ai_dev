# LLM Wiki — AI 时代的现代软件/网站开发常识库

基于 [Karpathy 的 llm-wiki 模式](llm-wiki.md) 构建的、由 LLM 增量维护的**持久化知识维基**。

> **定位**：为 AI 辅助编程时代沉淀「现代软件与网站开发」的结构化常识——部署链路、基础设施、工具选型、架构权衡——让 Agent 和人类共享同一份**可叠加、可交叉引用、会随新资料演进**的知识层，而不是每次对话从零检索。

## 为什么需要这个

AI 编程工具（Cursor、Codex、Claude Code 等）擅长写代码，但对**上线、运维、合规、选型**等「常识型知识」往往碎片化：问一次答一次，结论留在聊天里，下次重来。

传统 RAG 也是同样问题——每次查询从 raw 文档里重新拼片段，没有积累。

本项目的做法不同：

| 方式 | 行为 | 问题 |
|------|------|------|
| 聊天 / RAG | 每次从零检索、一次性摘要 | 无积累，矛盾不显式，跨文档综合靠运气 |
| **LLM Wiki** | 导入时编译进 `wiki/`，持续维护交叉引用 | 知识**编译一次、保持最新**，Agent 读 index 即可定位 |

**人类**负责策展 `raw/` 资料、提出好问题、指导分析方向；**LLM** 负责阅读、提炼、建页、回写链接、标记矛盾、更新综合论点。

## 知识域：现代软件/网站开发常识

本维基聚焦 AI 时代开发者**应该懂但容易漏**的全栈常识，而非某单一项目的业务文档：

| 主题 | 示例（已入库或计划覆盖） |
|------|-------------------------|
| **部署与上线** | VPS 七层生产就绪链路、Vercel/PaaS 托管边界、ICP 备案 |
| **基础设施** | Nginx、DNS、HTTPS、CDN、DDoS/WAF、进程守护 |
| **发布与运维** | CI/CD、灰度发布、回滚、健康检查、日志与监控 |
| **架构选型** | 传统 VPS vs PaaS vs 容器化；Java 后端 vs JS 全栈的路径分叉 |
| **工具与平台** | Vercel、Cloudflare、Redis/Upstash、Certbot 等实体图谱 |
| **AI 协作本身** | Agent 工作流、维基维护规范、query 归档 |

当前已覆盖：**Web 部署运维**（2 份 raw → 20+ wiki 页）。详见 [index.md](index.md) 与 [wiki/overview.md](wiki/overview.md)。

### 当前核心论点（跨来源综合）

- **「能跑」≠「能稳定跑」**：最小链路（域名 → HTTPS → VPS → Nginx → 前后端）与生产就绪七层栈之间存在系统性差距。
- **PaaS 不是万能替代**：Vercel 等平台托管 infra/运行时/网络/发布四层，但数据层、深度运维、国内合规仍须外置或回退 VPS。
- **技术栈决定部署路径**：Next.js 等 JS 全栈 → PaaS；Java Spring Boot/RuoYi → VPS/Docker/K8s。

→ 完整对照：[传统 VPS 与 Vercel 部署对照](wiki/comparisons/传统%20VPS%20与%20Vercel%20部署对照.md)

## 快速开始

### 1. 导入资料（Ingest）

```text
将文章保存到 raw/，然后对 Agent 说：

「请 ingest raw/xxx.md」
```

Agent 将：创建来源摘要 → 提取概念/实体 → 更新综合/对比页 → 维护交叉引用 → 更新 index 与 log。

项目内置 Cursor Skill：`.cursor/skills/wiki-ingest/`（可 `@wiki-ingest` 触发）。

### 2. 提问（Query）

```text
「根据维基，传统 VPS 和 Vercel 在数据层分别要自己做哪些事？」
```

有价值的分析会归档到 `wiki/queries/`。

### 3. 健康检查（Lint）

```text
「请对维基做一次 lint」
```

检查矛盾、孤儿页、薄页、index 同步等。

## 目录结构

| 路径 | 用途 |
|------|------|
| `AGENTS.md` | Agent 工作规范（结构、模板、流程）——**权威配置** |
| `index.md` | 维基内容目录（Agent 回答前先读） |
| `log.md` | 操作时间线（仅追加） |
| `raw/` | 原始资料（**只读**，LLM 不修改） |
| `raw/assets/` | 图片与附件 |
| `wiki/overview.md` | 全局概览与当前综合论点 |
| `wiki/sources/` | 来源摘要（每份 raw 一页） |
| `wiki/concepts/` | 概念页（机制、方法论、术语） |
| `wiki/entities/` | 实体页（工具、平台、产品） |
| `wiki/synthesis/` | 跨来源综合 |
| `wiki/comparisons/` | 结构化对比 |
| `wiki/queries/` | 查询归档 |
| `scripts/` | 维护脚本（index 重建、健康检查等，待补充） |

## 三层架构

```
raw/          原始资料（不可变，真相来源）
  ↓ ingest
wiki/         LLM 生成与维护（摘要、概念、实体、综合、对比）
  ↑ 规范
AGENTS.md     Schema：结构、模板、工作流
```

## 写作标准

维基页面由 LLM 撰写，须满足 `AGENTS.md` 中的标准：

- **结构化**：YAML frontmatter + 固定章节骨架
- **客观独立**：区分来源观点 / 维基综合 / 待验证假说；矛盾写入「观点演进」
- **足够深度**：禁止空壳摘要；来源 ≥800 字，概念/实体 ≥500 字，综合 ≥1000 字
- **交叉引用**：`[[wikilink]]` 双向链接；每页有「相关页面」节

## 推荐工具链

| 工具 | 用途 |
|------|------|
| **Cursor / Claude Code** | 执行 ingest、query、lint 的 Agent |
| **Obsidian** | 浏览维基、图谱视图、双向链接 |
| **Obsidian Web Clipper** | 网页剪藏为 markdown 放入 `raw/` |
| **Git** | 版本管理；维基即 markdown 仓库 |

## 参考

- 模式原理：[llm-wiki.md](llm-wiki.md)（Karpathy Gist 中文版）
- Agent 规范：[AGENTS.md](AGENTS.md)
- 内容索引：[index.md](index.md)
