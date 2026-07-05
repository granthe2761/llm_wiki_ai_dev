---
title: "React Server Components"
kind: concept
summary: "React 服务端组件：App Router 默认模式，服务端执行并直访 DB，HTML 直出且不进客户端 Bundle，SEO 优、Bundle 小。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# React Server Components

## 定义

**React Server Components**（RSC，React 服务端组件）由 **React 团队**提出（非 Next.js 独有概念），[[Next.js]] App Router 是第一个生产级实现。组件在**服务端**执行，可直接访问数据库、文件系统、密钥；生成 HTML + RSC Payload 发往客户端，**组件逻辑不打包进客户端 JS Bundle**。

## 机制与原理

```
服务端：执行 RSC → 查 DB/读文件 → 生成 HTML + 序列化组件树
客户端：渲染 HTML → 无需下载 RSC 对应 JS
```

默认无需标记——`app/page.tsx` 即为 RSC。跨框架类似思想：[[Astro]] Server Islands、Fresh（Deno）Islands、Waku（纯 RSC 框架）。

## 为什么重要

Portfolio/内容站 **90% 页面应为 RSC**（来源 [[Web 渲染架构：六种模式与跨框架对照]]）：极致 SEO、更小 Bundle、天然配合 [[Streaming SSR]]。与 [[React Client Components]] 的边界是 App Router 架构核心。

## 边界与反例

- **不能**直接使用 `window`、`localStorage`、浏览器事件监听——需拆到 RCC。
- **不能**使用 `useState`/`useEffect`（客户端 hooks）。
- Vue/Svelte **无 RSC/RCC 严格二分** — 默认同构，概念平移时 API 不同。

## 与其他概念的关系

- [[React Client Components]] — 交互与浏览器 API 的互补面。
- [[SSR]] / [[SSG]] / [[ISR]] — RSC 可与三种缓存策略组合（RSC+SSG 等）。
- [[Vercel]] — Next.js App Router 与 Vercel 构建/Edge 深度集成。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[React Client Components]]
- [[Next.js]]
- [[Streaming SSR]]
