---
title: "llm-wiki"
source: "https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f"
author:
published:
created: 2026-04-26
description: "llm-wiki. GitHub Gist: instantly share code, notes, and snippets."
tags:
  - "clippings"
---
## LLM Wiki大型语言模型维基

A pattern for building personal knowledge bases using LLMs.

利用大型语言模型构建个人知识库的模式。

This is an idea file, it is designed to be copy pasted to your own LLM Agent (e.g. OpenAI Codex, Claude Code, OpenCode / Pi, or etc.). Its goal is to communicate the high level idea, but your agent will build out the specifics in collaboration with you.

这是一个想法文件，设计成可以复制粘贴到你自己的大型语言模型代理（例如 OpenAI Codex、Claude Code、OpenCode / Pi 等）。它的目标是传达高层次的想法，但具体细节会由你的代理与你协作构建。

## The core idea核心理念

Most people's experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. There's no accumulation. Ask a subtle question that requires synthesizing five documents, and the LLM has to find and piece together the relevant fragments every time. Nothing is built up. NotebookLM, ChatGPT file uploads, and most RAG systems work this way.

大多数人使用LLM和文档的体验就像RAG：你上传一组文件，LLM在查询时获取相关片段，生成答案。这方法有效，但LLM是在每个问题上从零重新发现知识。没有积累。问一个需要综合五份文档的微妙问题，LLM每次都得找到并拼凑相关片段。没有任何构建。NotebookLM、ChatGPT文件上传和大多数RAG系统都是这样工作的。

The idea here is different. Instead of just retrieving from raw documents at query time, the LLM **incrementally builds and maintains a persistent wiki** — a structured, interlinked collection of markdown files that sits between you and the raw sources. When you add a new source, the LLM doesn't just index it for later retrieval. It reads it, extracts the key information, and integrates it into the existing wiki — updating entity pages, revising topic summaries, noting where new data contradicts old claims, strengthening or challenging the evolving synthesis. The knowledge is compiled once and then *kept current*, not re-derived on every query.

这里的思路不同。它不仅仅是在查询时从原始文档中检索，而是逐步 **构建并维护一个持久维基** ——一个结构化、相互关联的markdown文件集合，位于你和原始源之间。当你添加新源代码时，LLM不仅仅是索引以供后续检索。它读取该信息，提取关键信息，并将其集成到现有维基中——更新实体页面，修订主题摘要，指出新数据与旧主张相矛盾的地方，强化或挑战不断演变的综合。知识被编译一次后 *保持最新* ，而非每次查询都重新推导。

This is the key difference: **the wiki is a persistent, compounding artifact.** The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything you've read. The wiki keeps getting richer with every source you add and every question you ask.

关键区别在于： **维基是一个持续存在、不断叠加的伪造品。** 交叉引用已经存在。矛盾已经被标记出来。综合内容已经反映了你所读到的一切。你添加的每一个来源和每一个问题，维基都会变得更丰富。

You never (or rarely) write the wiki yourself — the LLM writes and maintains all of it. You're in charge of sourcing, exploration, and asking the right questions. The LLM does all the grunt work — the summarizing, cross-referencing, filing, and bookkeeping that makes a knowledge base actually useful over time. In practice, I have the LLM agent open on one side and Obsidian open on the other. The LLM makes edits based on our conversation, and I browse the results in real time — following links, checking the graph view, reading the updated pages. Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.

你从不（或很少）自己编写维基——大语言模型负责编写和维护所有内容。你负责寻找资料、探索和提出正确的问题。LLM负责所有繁重的工作——总结、交叉引用、归档和簿记，这些都是让知识库随着时间真正有用的。实际上，我一边打开LLM代理，另一边开着Obsidian。LLM根据我们的对话进行编辑，我实时浏览结果——点击链接、查看图表视图、阅读更新页面。Obsidian是IDE;LLM是程序员;维基是代码库。

This can apply to a lot of different contexts. A few examples:

这适用于许多不同的情境。举几个例子：

- **Personal**: tracking your own goals, health, psychology, self-improvement — filing journal entries, articles, podcast notes, and building up a structured picture of yourself over time.
	**个人：** 追踪自己的目标、健康、心理、自我提升——整理日记、文章、播客笔记，并逐步构建一个结构化的自我形象。
- **Research**: going deep on a topic over weeks or months — reading papers, articles, reports, and incrementally building a comprehensive wiki with an evolving thesis.
	**研究** ：在数周或数月内深入探讨一个主题——阅读论文、文章、报告，逐步构建一个带有不断发展论点的综合维基。
- **Reading a book**: filing each chapter as you go, building out pages for characters, themes, plot threads, and how they connect. By the end you have a rich companion wiki. Think of fan wikis like [Tolkien Gateway](https://tolkiengateway.net/wiki/Main_Page) — thousands of interlinked pages covering characters, places, events, languages, built by a community of volunteers over years. You could build something like that personally as you read, with the LLM doing all the cross-referencing and maintenance.
	**读书** ：边读边整理每一章，构建角色、主题、情节线索及其关联页面。最终你拥有了一个丰富的配套维基。可以想象成托 [尔金之门（Tolkien Gateway](https://tolkiengateway.net/wiki/Main_Page) ）这样的粉丝维基——数千个相互关联的页面，涵盖角色、地点、事件、语言，由志愿者社区多年构建。你也可以边读边亲自构建类似的东西，由大型语言模型负责所有交叉引用和维护。
- **Business/team**: an internal wiki maintained by LLMs, fed by Slack threads, meeting transcripts, project documents, customer calls. Possibly with humans in the loop reviewing updates. The wiki stays current because the LLM does the maintenance that no one on the team wants to do.
	**业务/团队** ：由大型语言模型维护的内部维基，通过Slack线程、会议记录、项目文档、客户电话提供信息。可能还有人工参与审核更新。维基保持最新，因为大型语言模型负责团队中没人愿意做的维护。
- **Competitive analysis, due diligence, trip planning, course notes, hobby deep-dives** — anything where you're accumulating knowledge over time and want it organized rather than scattered.
	**竞争分析、尽职调查、行程规划、课程笔记、兴趣深入探讨** ——任何你在积累知识、希望它有条理而非零散的项目。

## Architecture建筑

There are three layers:

分为三层：

**Raw sources** — your curated collection of source documents. Articles, papers, images, data files. These are immutable — the LLM reads from them but never modifies them. This is your source of truth.

**原始资料** ——你精心整理的原始文档集合。文章、论文、图片、数据文件。这些是不可变的——大型语言模型会读取它们，但从不修改它们。这就是你的真相来源。

**The wiki** — a directory of LLM-generated markdown files. Summaries, entity pages, concept pages, comparisons, an overview, a synthesis. The LLM owns this layer entirely. It creates pages, updates them when new sources arrive, maintains cross-references, and keeps everything consistent. You read it; the LLM writes it.

**维基** ——一个由LLM生成的markdown文件目录。摘要、实体页、概念页、比较、概述、综合。LLM完全拥有这一层。它创建页面，在新源到来时更新页面，维护交叉引用，并保持一切一致。你阅读;LLM编写。

**The schema** — a document (e.g. CLAUDE.md for Claude Code or AGENTS.md for Codex) that tells the LLM how the wiki is structured, what the conventions are, and what workflows to follow when ingesting sources, answering questions, or maintaining the wiki. This is the key configuration file — it's what makes the LLM a disciplined wiki maintainer rather than a generic chatbot. You and the LLM co-evolve this over time as you figure out what works for your domain.

**模式** ——一份文档（例如Claude Code的 CLAUDE.md 或Codex的 AGENTS.md），告诉LLM维基的结构、惯例是什么，以及在获取源代码、回答问题或维护维基时应遵循哪些工作流程。这是关键的配置文件——它使LLM成为一个有纪律的维基维护者，而非泛泛的聊天机器人。你和LLM会随着时间共同进化，找出适合你领域的方法。

## Operations运营

**Ingest.** You drop a new source into the raw collection and tell the LLM to process it. An example flow: the LLM reads the source, discusses key takeaways with you, writes a summary page in the wiki, updates the index, updates relevant entity and concept pages across the wiki, and appends an entry to the log. A single source might touch 10-15 wiki pages. Personally I prefer to ingest sources one at a time and stay involved — I read the summaries, check the updates, and guide the LLM on what to emphasize. But you could also batch-ingest many sources at once with less supervision. It's up to you to develop the workflow that fits your style and document it in the schema for future sessions.

**导入。** 你把一个新源放入原始集合，并让大型语言模型处理它。举个流程示例：大型语言模型会读取源代码，和你讨论关键要点，在维基中写摘要页，更新索引，更新相关的实体和概念页面，并在日志中添加条目。一个源代码可能涉及10到15个维基页面。我个人更喜欢一次只吸收一个源代码并保持参与——我会阅读摘要，检查更新情况，指导大型语言模型强调什么。但你也可以一次性批量采集多个源代码，监督更少。你需要制定适合自己风格的工作流程，并在模式中记录以备后续会话使用。

**Query.** You ask questions against the wiki. The LLM searches for relevant pages, reads them, and synthesizes an answer with citations. Answers can take different forms depending on the question — a markdown page, a comparison table, a slide deck (Marp), a chart (matplotlib), a canvas. The important insight: **good answers can be filed back into the wiki as new pages.** A comparison you asked for, an analysis, a connection you discovered — these are valuable and shouldn't disappear into chat history. This way your explorations compound in the knowledge base just like ingested sources do.

**查询。** 你会对着维基提问。LLM会搜索相关页面，阅读它们，并综合答案并附上引用。答案可以根据问题的不同形式出现——降价页面、比较表、幻灯片（Marp）、图表（matplotlib）、画布。重要的洞见是： **好的答案可以作为新页面重新归档到维基。** 你请求的比较、分析、你发现的联系——这些都很有价值，不应消失在聊天历史中。这样你的探索会像被吸收的资源一样，在知识库中积累。

**Lint.** Periodically, ask the LLM to health-check the wiki. Look for: contradictions between pages, stale claims that newer sources have superseded, orphan pages with no inbound links, important concepts mentioned but lacking their own page, missing cross-references, data gaps that could be filled with a web search. The LLM is good at suggesting new questions to investigate and new sources to look for. This keeps the wiki healthy as it grows.

**线索。** 定期要求LLM对维基进行健康检查。注意：页面间的矛盾、陈旧的说法认为新来源取代了新资源、没有入站链接的孤立页面、提及重要概念但没有独立页面、缺少交叉引用、数据空白（可通过网络搜索填补）。LLM擅长提出新的问题和新的来源。这能让维基在成长过程中保持健康。

## Indexing and logging索引与记录

Two special files help the LLM (and you) navigate the wiki as it grows. They serve different purposes:

两个特殊文件帮助LLM（以及你）在维基中导航。它们有不同的用途：

**index.md** is content-oriented. It's a catalog of everything in the wiki — each page listed with a link, a one-line summary, and optionally metadata like date or source count. Organized by category (entities, concepts, sources, etc.). The LLM updates it on every ingest. When answering a query, the LLM reads the index first to find relevant pages, then drills into them. This works surprisingly well at moderate scale (~100 sources, ~hundreds of pages) and avoids the need for embedding-based RAG infrastructure.

**index.md** 以内容为导向。它是维基中所有内容的目录——每个页面都附有链接、一行摘要，以及可选的元数据，如日期或来源数量。按类别（实体、概念、来源等）组织。LLM 在每次导入时更新索引。回答查询时，LLM 先读取索引找到相关页面，然后深入分析。这在中等规模（~100 个源代码，~数百页）下效果出奇地好，避免了嵌入式的 RAG 基础设施需求。

**log.md** is chronological. It's an append-only record of what happened and when — ingests, queries, lint passes. A useful tip: if each entry starts with a consistent prefix (e.g. `## [2026-04-02] ingest | Article Title`), the log becomes parseable with simple unix tools — `grep "^## \[" log.md | tail -5` gives you the last 5 entries. The log gives you a timeline of the wiki's evolution and helps the LLM understand what's been done recently.

**log.md** 是按时间顺序排列的。它只是一个仅附加的记录，记录发生了什么、何时发生了什么——包括吞入、查询、lint 传递。一个有用的提示：如果每个条目都以一致的前缀开头（例如 `## [2026-04-02] ingest | Article Title` ），日志就会用简单的 Unix 工具解析—— `grep "^## \[" log.md | tail -5` 会显示最近 5 条记录。日志会给你维基的发展时间线，帮助大型语言模型理解最近的进展。

## Optional: CLI tools可选：CLI 工具

At some point you may want to build small tools that help the LLM operate on the wiki more efficiently. A search engine over the wiki pages is the most obvious one — at small scale the index file is enough, but as the wiki grows you want proper search. [qmd](https://github.com/tobi/qmd) is a good option: it's a local search engine for markdown files with hybrid BM25/vector search and LLM re-ranking, all on-device. It has both a CLI (so the LLM can shell out to it) and an MCP server (so the LLM can use it as a native tool). You could also build something simpler yourself — the LLM can help you vibe-code a naive search script as the need arises.

某个阶段你可能想构建一些小型工具，帮助LLM在wiki上更高效地运行。在wiki页面上使用搜索引擎是最明显的选择——在小规模上，索引文件就足够了，但随着wiki的扩展，你需要真正的搜索。 [QMD](https://github.com/tobi/qmd) 是个不错的选择：它是一个本地搜索引擎，支持Markdown文件，结合了BM25/矢量搜索和LLM重新排序，全部在设备上运行。它既有CLI（这样LLM可以付费使用），也有MCP服务器（方便LLM作为原生工具使用）。你也可以自己做更简单的——LLM可以根据需要帮你vibecode一个简单的搜索脚本。

## Tips and tricks技巧与窍门

- **Obsidian Web Clipper** is a browser extension that converts web articles to markdown. Very useful for quickly getting sources into your raw collection.
	**Obsidian Web Clipper** 是一个浏览器扩展，可以将网页文章转换为可行的折扣。非常实用，可以快速将原始资料导入你的原始收藏。
- **Download images locally.** In Obsidian Settings → Files and links, set "Attachment folder path" to a fixed directory (e.g. `raw/assets/`). Then in Settings → Hotkeys, search for "Download" to find "Download attachments for current file" and bind it to a hotkey (e.g. Ctrl+Shift+D). After clipping an article, hit the hotkey and all images get downloaded to local disk. This is optional but useful — it lets the LLM view and reference images directly instead of relying on URLs that may break. Note that LLMs can't natively read markdown with inline images in one pass — the workaround is to have the LLM read the text first, then view some or all of the referenced images separately to gain additional context. It's a bit clunky but works well enough.
	本地 **下载图片。** 在Obsidian设置→文件和链接中，将“附件文件夹路径”设置为固定目录（例如 `raw/assets/` ）。然后在设置→快捷键中搜索“下载”，找到“下载当前文件附件”，并将其绑定到快捷键（例如Ctrl+Shift+D）。裁剪文章后，按下热键，所有图片都会下载到本地磁盘。这是可选但有用的——它让LLM可以直接查看和引用图片，而不必依赖可能损坏的URL。注意LLM不能一次性读取内联图片的markdown——解决方法是让LLM先读取文本，然后单独查看部分或全部引用图片以获得更多上下文。虽然有点笨重，但效果还算不错。
- **Obsidian's graph view** is the best way to see the shape of your wiki — what's connected to what, which pages are hubs, which are orphans.
	**Obsidian 的图表视图** 是了解维基结构的最佳方式——哪些连接着什么，哪些页面是枢纽，哪些是孤儿。
- **Marp** is a markdown-based slide deck format. Obsidian has a plugin for it. Useful for generating presentations directly from wiki content.
	**Marp** 是一种基于 markdown 的幻灯片格式。Obsidian 有一个插件。它适合直接从维基内容生成演示文稿。
- **Dataview** is an Obsidian plugin that runs queries over page frontmatter. If your LLM adds YAML frontmatter to wiki pages (tags, dates, source counts), Dataview can generate dynamic tables and lists.
	**Dataview** 是一个 Obsidian 插件，可以对页面前页运行查询。如果你的大型语言模型（LLM）在维基页面上添加了 YAML 前页（标签、日期、源代码计数），Dataview 可以生成动态表和列表。
- The wiki is just a git repo of markdown files. You get version history, branching, and collaboration for free.
	维基只是一个 git 仓库，里面装着 markdown 文件。你可以免费获得版本历史、分支和协作功能。

## Why this works为什么这样做有效

The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining consistency across dozens of pages. Humans abandon wikis because the maintenance burden grows faster than the value. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass. The wiki stays maintained because the cost of maintenance is near zero.

维护知识库的繁琐部分不在于阅读或思考——而是记账。更新交叉引用，保持摘要最新，指出新数据与旧数据相矛盾，保持数十页的一致性。人类放弃维基是因为维护负担增长快于价值。大型语言模型不会感到厌倦，别忘了更新交叉引用，一次就能处理15个文件。维基之所以能保持维护，是因为维护成本几乎为零。

The human's job is to curate sources, direct the analysis, ask good questions, and think about what it all means. The LLM's job is everything else.

人类的工作是整理资料，指导分析，提出好问题，思考这一切的意义。LLM的工作是其他所有事情。

The idea is related in spirit to Vannevar Bush's Memex (1945) — a personal, curated knowledge store with associative trails between documents. Bush's vision was closer to this than to what the web became: private, actively curated, with the connections between documents as valuable as the documents themselves. The part he couldn't solve was who does the maintenance. The LLM handles that.

这个想法在精神上与范尼瓦·布什的Memex（1945年）相关——一个个人化、策划的知识库，文档之间有关联路径。布什的愿景比网络更接近这一点：私密、主动策划，文档之间的联系价值与文档本身一样宝贵。他无法解决的是谁来维护。这由大型语言模型负责。

## Note注释

This document is intentionally abstract. It describes the idea, not a specific implementation. The exact directory structure, the schema conventions, the page formats, the tooling — all of that will depend on your domain, your preferences, and your LLM of choice. Everything mentioned above is optional and modular — pick what's useful, ignore what isn't. For example: your sources might be text-only, so you don't need image handling at all. Your wiki might be small enough that the index file is all you need, no search engine required. You might not care about slide decks and just want markdown pages. You might want a completely different set of output formats. The right way to use this is to share it with your LLM agent and work together to instantiate a version that fits your needs. The document's only job is to communicate the pattern. Your LLM can figure out the rest.

本文档有意抽象。它描述的是想法，而非具体实现。具体目录结构、模式惯例、页面格式、工具——所有这些都取决于你的领域、偏好和选择的大型语言模型。以上提到的都是可选且模块化的——选择有用的，忽略无用的。例如：你的源代码可能仅为文本，因此完全不需要图像处理。你的维基可能足够小，只需索引文件，不需要搜索引擎。你可能不在意幻灯片，只想要标记页。你可能想要完全不同的输出格式。正确的使用方式是与你的大型语言模型代理共享，共同实例化一个符合你需求的版本。文档的唯一职责是传达模式。你的大型语言模型可以自行处理剩下的。