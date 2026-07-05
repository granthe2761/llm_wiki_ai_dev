---
title: "SSR"
kind: concept
summary: "Server-Side Rendering：每次请求在服务端实时查库渲染 HTML，数据最新，SEO 友好，TTFB 与服务器成本高于 SSG。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# SSR

## 定义

**SSR**（Server-Side Rendering，服务端渲染）在**用户请求到达时**执行数据查询与组件/模板渲染，生成完整 HTML 返回，并附带客户端 JS 用于 hydration 交互。PHP（1995）、Rails、Django、Spring MVC 的默认模式本质上都是 SSR。

## 机制与原理

```
请求 → 服务器查 DB/API → 服务端渲染 HTML → 返回 HTML + JS bundle
```

[[Next.js]]：`dynamic = 'force-dynamic'` 或 `fetch(..., { cache: 'no-store' })`。跨框架：Vue `renderToString`、Angular Universal、[[SvelteKit]] `+page.server.js`。

## 为什么重要

SSR 在**数据 freshness** 与 **SEO** 之间取得平衡：爬虫收到完整 HTML；但 TTFB 高于 [[SSG]]，高并发下服务器压力与爬虫抓取速率可能受影响。Dashboard、登录后个性化页的典型选择。

## 边界与反例

- **纯静态营销页用 SSR 是过度设计** — 应优先 SSG/ISR。
- **SSR ≠ 无 JS** — 通常仍下发 hydration 脚本；禁 JS 时静态内容可见，交互不可用。
- **[[Streaming SSR]]** — SSR 的流式优化变体，不必等所有数据就绪。

## 与其他概念的关系

- [[SSG]] — 构建时 vs 请求时；速度 vs 实时。
- [[Streaming SSR]] — 分块返回，改善 TTFB 与感知性能。
- [[React Server Components]] — React 生态的 SSR 演进，减少客户端 Bundle。
- [[生产就绪部署链路]] — SSR 需常驻服务端或 Serverless 函数，关联进程/函数运行时。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Streaming SSR]]
- [[SSG]]
- [[React Server Components]]
- [[Next.js]]
