---
title: "Streaming SSR"
kind: concept
summary: "流式服务端渲染：利用 HTTP 分块传输先返回页面框架，再逐步流式填充慢数据区块，降低 TTFB 与感知延迟。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Streaming SSR

## 定义

**Streaming SSR**（流式服务端渲染）基于 HTTP **Chunked Transfer Encoding**：服务端不必等所有数据查询完成，而是**立即发送 HTML 框架**，再逐步流式传输 Suspense 边界内的异步内容。传统 SSR「等 A→等 B→等 C→一次返回」变为「先 header/main 骨架→流式填充慢块→footer」。

## 机制与原理

```
传统 SSR：  等全部数据 → 一次性完整 HTML
Streaming： 立即 <html><header>… → 流式 <Suspense> 内容 → </html>
```

[[Next.js]]：`Suspense` + `loading.tsx` + 异步 Server Component。[[React Server Components]] 天然支持 Streaming。跨框架：Django `StreamingHttpResponse`、Node `res.write()`、Vue 3 pipeable、[[SvelteKit]] stream。

## 为什么重要

页面**部分依赖慢第三方 API**（如 GitHub 统计）时，Streaming 让主体框架与骨架屏立即展示，慢查询不阻塞整页 TTFB。Google 已支持解析流式 HTML；极旧爬虫可能不等待后续 chunk。

## 边界与反例

- 需合理设计 **Suspense 边界** — 边界过粗则流式收益有限。
- SEO 关键内容不应只放在流式末尾且无 fallback 语义。
- 实现复杂度高于普通 [[SSR]]。

## 与其他概念的关系

- [[SSR]] — Streaming 是 SSR 的性能优化变体。
- [[React Server Components]] — App Router 默认配合 Suspense 流式。
- [[SSG]] — 静态页无 Streaming 需求（构建时已全量）。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[SSR]]
- [[React Server Components]]
- [[Next.js]]
