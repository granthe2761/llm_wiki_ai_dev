---
name: wiki-ingest
description: |
  Ingest raw files from `raw/` into the structured wiki (`wiki/`). This skill should be used when the user wants to ingest new raw material, process files dropped into `raw/`, or asks to "process this article", "add to wiki", "digest this", or mentions raw files needing integration. ALWAYS use this skill when the user mentions ingesting, digesting, or processing raw material into the wiki — it provides the complete workflow that AGENTS.md only summarizes.
metadata:
  version: "2.0"
---

# Wiki Ingest

**做什么**：将 `raw/` 中的原始材料编译进持久化、交叉引用的 wiki 知识层（`wiki/`），而非一次性摘要。

**何时触发**：用户在 `raw/` 放入新文件，或要求"入库"、"digest"、"加入 wiki"、"处理这篇文章"。

**何时不用**：

| 场景 | 使用本 Skill | 原因 |
|------|-------------|------|
| 新 raw 文件需结构化入库 | ✅ | 核心场景 |
| 已有 wiki 页需因新证据更新 | ✅ | 走 update 分支 |
| 仅临时问答、不需持久化 | ❌ | 直接回答即可 |
| 无 `raw/` 或 `wiki/` 目录 | ❌ | 先初始化项目结构 |
| 维护脚本不可用 | ❌ | 先修复 `scripts/` 再入库 |

## 前置条件

入库前确认以下条件；任一项不满足则先修复，不要开始写页面。

| # | 条件 | 验证命令 |
|---|------|----------|
| 1 | `raw/` 目录存在且目标文件可读 | `test -f raw/<filename>.md && head -5 raw/<filename>.md` |
| 2 | `wiki/` 子目录已初始化 | `test -d wiki/sources && test -d wiki/entities && test -d wiki/concepts && test -d wiki/syntheses` |
| 3 | 维护脚本可用（Python 3） | `python3 --version && test -f scripts/rebuild_index.py && test -f scripts/health_check.py` |
| 4 | 可选：`wiki_size_report.py`、`stale_report.py` | `test -f scripts/wiki_size_report.py && test -f scripts/stale_report.py` |

> ⚠️ 旧版页面可能仍含 `source_count` / `status` 字段——**忽略即可**，不要在新页面中写入，也不要因旧字段报错。

## Overview

```
raw/*.md  →  [Ingest]  →  wiki/sources/  (1 source page)
                        →  wiki/entities/  (N entity pages, created or updated)
                        →  wiki/concepts/  (N concept pages, created or updated)
                        →  wiki/syntheses/ (updated if new evidence changes conclusions)
                        →  wiki/log.md     (1 log entry)
                        →  wiki/index.md   (rebuilt via script)
```

## 分类与路由决策

入库前先决定"新建还是更新"、"实体还是概念"：

```
读取 raw 文件
    │
    ▼
该主题是否已有 source 页？ ──是──► 更新现有 source 页（勿建重复页）
    │否
    ▼
创建新 source 页
    │
    ▼
对每个显著提及项：
    │
    ├─ 具体的人/组织/产品/工具/项目？ ──是──► entity 页（wiki/entities/）
    │
    ├─ 方法/框架/术语/抽象思想？ ──是──► concept 页（wiki/concepts/）
    │
    └─ 跨主题关系值得独立成页？ ──是──► relation 页（wiki/relations/，少见）
    │
    ▼
新证据是否影响跨页结论？ ──是──► 更新 synthesis 页或 [[open-questions]]
    │
    ▼
新证据是否与旧结论矛盾？ ──是──► 使用「观点演进」表格（禁止静默覆盖）
```

| 类型 | 判断依据 | 示例 |
|------|----------|------|
| **entity** | 可指认的具体对象 | Cursor、OpenAI、某银行分行 |
| **concept** | 可复用的方法或抽象 | RAG、Agent loop、观点演进 |
| **synthesis** | 多源综合、对比、阶段性结论 | 工具地图、趋势综述 |

完整页面模板见 [references/templates.md](references/templates.md)。

## Workflow

### Step 1: Read and Understand

Read the target file in `raw/`. Identify:
- **Subject**: what is this about?
- **Entities**: people, organizations, products, tools, projects mentioned
- **Concepts**: methods, frameworks, terms, ideas introduced or used
- **Claims**: key arguments, evidence, data points
- **Relations**: how these things connect to each other
- **What's new**: does this confirm, extend, or contradict existing wiki pages?

**Checkpoint**：列出将创建/更新的页面清单（source + entities + concepts + syntheses），确认无重复 source 页。

### Step 2: Create or Update the Source Page

Every raw file gets exactly one source page in `wiki/sources/`. The filename is the raw file's basename (without the `.md` extension). Use the [source template](references/templates.md#source-page).

Rules for source pages:
- The `summary` field is used by `rebuild_index.py` to generate the index — keep it informative.
- Always link entities and concepts with wikilinks (`[[...]]`) in the body, not just list them at the bottom.
- Include the raw file path in `source_files` and again at the bottom under `## 来源`.
- If a raw file updates a topic that already has a source page, update that existing page rather than creating a duplicate. Note the update in the summary.

**Checkpoint**：确认 frontmatter 含 `title`, `kind`, `summary`, `source_files`, `updated` 五个必填字段：

```bash
grep -E '^(title|kind|summary|source_files|updated):' wiki/sources/<Page>.md
```

### Step 3: Extract and Create/Update Entity Pages

For each significant entity, create or update a page in `wiki/entities/`. Use the [entity template](references/templates.md#entity-page).

When updating an existing entity:
- Append new `raw/` files to `source_files` (avoid duplicates).
- Update `updated` date.
- If the new information contradicts prior claims, mark it explicitly: "**2026-06 更新**：..." rather than silently overwriting.
- Update `summary` if the entity's significance has changed.

**Checkpoint**：`source_files` 中的路径均存在于 `raw/`：

```bash
grep 'raw/' wiki/entities/<Page>.md
```

### Step 4: Extract and Create/Update Concept Pages

For each concept, method, or framework, do the same in `wiki/concepts/`. Use the [concept template](references/templates.md#concept-page).

**Checkpoint**：同 Step 3，确认 frontmatter 完整且 `source_files` 路径有效。

### Step 5: Cross-linking

Ensure every new or updated page has meaningful bidirectional links:
- Source page links to the entities and concepts it introduces.
- Each entity/concept page links back to its source pages (under `## 相关页面` or in body text).
- If the new material changes or extends an existing synthesis page, update that synthesis page too.
- If there's a conflict with an existing page's conclusions, note it in that page and in [[open-questions]].

**Checkpoint**：抽查双向链接——source 页引用的 `[[Entity]]` 在对应 entity 页的 `## 相关页面` 或正文中应回链到该 source：

```bash
grep -o '\[\[[^]]*\]\]' wiki/sources/<Page>.md | sort -u
```

### Step 6: Update Synthesis Pages

Check whether the new evidence:
- **Confirms** an existing conclusion → note the additional support
- **Contradicts** an existing conclusion → mark the conflict explicitly, update [[open-questions]]
- **Introduces a new cross-cutting pattern** → consider creating or updating a synthesis page in `wiki/syntheses/`

A "持续更新" synthesis page like [[0. AI coding 工具地图]] should absorb new tools, trends, or comparisons directly:
1. Add new sections or update existing tables
2. Update frontmatter `summary` (mark version and new additions) and `updated`
3. Silently overwriting old conclusions is forbidden — use the standard "观点演进" table format (see Handling Conflicts below)

**Checkpoint**：若更新了 synthesis 页，确认 `updated` 日期已刷新且旧结论未被静默删除。

### Step 7: Log the Ingest

Append an entry to `wiki/log.md` using the [log template](references/templates.md#log-entry).

**Checkpoint**：log 条目包含 **新文件**、**更新页面**、**关键沉淀**、**索引变化** 四段。

### Step 8: Rebuild Index, Health Check & Token Report

After all pages are written, run the maintenance scripts:

1. **Rebuild the index** (generates `wiki/index.md` from all page summaries):
   ```bash
   python3 scripts/rebuild_index.py
   ```

2. **Health check** (finds orphan pages and dangling links):
   ```bash
   python3 scripts/health_check.py
   ```

3. **Token budget report** (decides whether wiki still fits in-context):
   ```bash
   python3 scripts/wiki_size_report.py
   ```
   - If total < **80k tokens** (~60% of a 128k context window, 留余量给对话与工具输出): continue direct full-context reads, no RAG needed
   - If total >= 80k tokens: flag for sharding or RAG consideration

If the health check reveals issues (orphan pages, dangling links), fix them or note the known orphans in [[open-questions]].

## Handling Conflicts

When new material contradicts existing wiki pages:

1. **Do not silently overwrite.** Keep the old conclusion visible.
2. In the relevant page, add a "观点演进" section using the [standard table format](references/templates.md#观点演进-table-conflict-resolution).
3. Add an entry to [[open-questions]] under a dated heading.
4. If the contradiction is resolved by additional evidence later, update the page and move the open question to a note about resolution.

## Stale Detection

Run periodically (e.g., weekly or before major queries) to detect content that may need recompilation:

```bash
python3 scripts/stale_report.py --days 30
```

Default **30 days** balances freshness vs. noise — adjust with `--days` for your wiki's update cadence.

This script scans all wiki pages and reports:
- Pages whose `updated` date is older than `--days` threshold
- Pages whose `source_files` reference raw files that have been modified since the page's `updated` date
- Synthesis pages that haven't been updated despite new source pages in their domain

Review the stale report and decide which pages need re-ingestion or manual update.

## Page Writing Rules

- **Obsidian wikilinks** are preferred: `[[Page Name]]`
- Page filenames should match the page title for easy linking.
- Pages other than `index.md`, `log.md`, `overview.md`, `open-questions.md` must have YAML frontmatter.
- Required frontmatter fields: `title`, `kind`, `summary`, `source_files`, `updated`.
- Distinguish three kinds of information in the body:
  1. Known facts (from sources)
  2. Inferences drawn from multiple sources
  3. Open questions pending verification
- Write for long-term value — someone reading this 6 months later should find it useful.
- Do not write in chat style. Avoid "I think", "you should", etc.

## Types of Pages

| kind | directory | when to create |
|------|-----------|----------------|
| `source` | `wiki/sources/` | Every raw file gets one |
| `entity` | `wiki/entities/` | People, orgs, products, tools, projects |
| `concept` | `wiki/concepts/` | Methods, frameworks, terms, ideas |
| `relation` | `wiki/relations/` | Cross-cutting relationships worth a standalone page |
| `synthesis` | `wiki/syntheses/` | Comparisons, reviews, answered questions, phased conclusions |

## 入库完成验证清单

全部步骤完成后，逐项确认：

| # | 检查项 | 命令 / 动作 |
|---|--------|-------------|
| 1 | 每个 raw 文件对应唯一 source 页 | 核对 `wiki/sources/` 无重复 basename |
| 2 | 新/改页面 frontmatter 五字段齐全 | `grep -E '^(title\|kind\|summary\|source_files\|updated):' wiki/**/*.md` |
| 3 | `source_files` 路径真实存在 | 逐条 `test -f <path>` |
| 4 | 双向 wikilink 已建立 | health_check 无新增 dangling links |
| 5 | `wiki/log.md` 已追加本次 ingest 条目 | `tail -30 wiki/log.md` |
| 6 | `wiki/index.md` 已重建 | `python3 scripts/rebuild_index.py` 成功退出 |
| 7 | 健康检查通过或已知问题已记录 | `python3 scripts/health_check.py` |
| 8 | Token 预算已评估 | `python3 scripts/wiki_size_report.py` |
