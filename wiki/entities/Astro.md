---
title: "Astro"
kind: entity
summary: "内容导向 Web 框架，默认 SSG，Server Islands 与 client:* 指令实现类 RSC/RCC 分离，SEO 与零 JS 默认优先。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Astro

## 概述

**Astro** 是内容站/营销页导向的 Web 框架，**默认 [[SSG]]**，以「送更少 JS 到浏览器」为设计目标。在跨框架渲染对照中，Astro 用 **Server Islands** 对应 [[React Server Components]] 思想，用 **`client:*` 指令**对应 [[React Client Components]] 的交互岛模式。

## 渲染对照

| 通用模式 | Astro 实现 |
|---------|-----------|
| [[SSG]] | 默认构建行为 |
| [[SSR]] / 服务端块 | Server Islands |
| [[ISR]] | 按需渲染（On-demand Rendering） |
| 客户端交互 | `client:load` / `client:visible` 等 |
| Streaming | 渐进式渲染 |

## 选型位置

与 [[Next.js]]、[[Nuxt]]、[[SvelteKit]] 并列：从 Next 转向 Astro 时，[[SSG]]/[[SSR]]/Streaming 概念**平移**，仅 API 名不同（见 [[跨框架渲染模式对照]]）。适合博客、文档、Portfolio 等 SEO 敏感、交互较少的站点。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[跨框架渲染模式对照]]
- [[SSG]]
- [[React Server Components]]
- [[React Client Components]]
- [[Vercel]]
