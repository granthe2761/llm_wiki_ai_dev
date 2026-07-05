# Wiki Page Templates

Complete frontmatter + body templates for wiki-ingest. Copy and adapt when creating or updating pages.

## Source Page

Filename: `wiki/sources/<raw-basename>.md` (basename of the raw file, without `.md`).

```markdown
---
title: "Exact Title of the Article"
kind: source
summary: "One-sentence description covering what this source is, who created it, and its main contribution."
source_files:
  - "raw/filename-in-raw.md"
updated: YYYY-MM-DD
---

# Exact Title of the Article

## 核心摘要

- Bullet points of key takeaways
- Each should be self-contained and link to relevant concepts/entities

## 关键论点

### Sub-section per major argument

The article's main claims, with supporting evidence.

## 涉及实体

- [[Entity One]]
- [[Entity Two]]

## 来源

- `raw/filename-in-raw.md`
```

## Entity Page

```markdown
---
title: "Entity Name"
kind: entity
summary: "One sentence about who/what this is and why it matters in the wiki."
source_files:
  - "raw/filename.md"
updated: YYYY-MM-DD
---

# Entity Name

Brief description of the entity.

## 主要贡献 / 特征

- What this entity does or contributes
- Link to relevant concepts

## 相关页面

- [[Related Concept]]
- [[Related Entity]]
```

## Concept Page

```markdown
---
title: "Concept Name"
kind: concept
summary: "One-sentence definition."
source_files:
  - "raw/filename.md"
updated: YYYY-MM-DD
---

# Concept Name

## 定义

Clear, concise definition.

## 为什么重要

Why this concept matters, what problem it solves.

## 与其他概念的关系

- [[Related Concept A]] — how they connect
- [[Related Concept B]] — how they connect

## 相关页面

- [[Related Entity]]
```

## Log Entry

Append to `wiki/log.md`:

```markdown
## [YYYY-MM-DD] ingest | Brief Title of What Was Ingested

Brief description (1-2 sentences).

**新文件**：
- `wiki/sources/Source Page Name.md` — 摘要
- `wiki/entities/Entity.md` — 摘要
- `wiki/concepts/Concept.md` — 摘要

**更新页面**：
- `wiki/concepts/Existing Page.md` — what was changed

**关键沉淀**：
1. Key insight or conclusion
2. ...

**索引变化**：Sources +N | Entities +N | Concepts +N | Syntheses ±N | 更新 ×N

**相关文件**：[[Page A]]、[[Page B]]、[[Page C]]
```

## 观点演进 Table (Conflict Resolution)

Add to any page where new evidence contradicts prior conclusions:

```markdown
## 观点演进

| 时间 | 结论 | 来源 | 状态 |
|------|------|------|------|
| 2026-05 | 旧结论描述 | [[Old Source]] | ❌ 已被更新 |
| 2026-07 | 新结论描述 | [[New Source]] | ✅ 当前 |
| 2026-07 | 待验证假设 | [[New Source]] | ❓ 待验证 |
```
