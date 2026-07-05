---
title: "SSG"
kind: concept
summary: "Static Site Generation：构建时生成完整 HTML，CDN 直出，TTFB 最低，SEO 与首屏最优，内容更新需重新构建。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# SSG

## 定义

**SSG**（Static Site Generation，静态站点生成）在**构建阶段**读取数据源（Markdown、DB、API），为每个路由生成完整 HTML 文件；用户请求时 CDN/Web 服务器**直接返回预生成文件**，不执行数据查询。这是 Web 的原始形态，也是 GitHub Pages 的基石。

## 机制与原理

```
构建时：数据源 → 渲染 → 输出 .html
访问时：CDN/服务器 → 直接返回 HTML（零计算）
```

[[Next.js]]：`generateStaticParams` + 默认 App Router 静态行为。跨框架：Jekyll、Hugo、Gatsby、Eleventy、[[Astro]]（默认）、[[Nuxt]] `nuxt generate`。

## 为什么重要

在 [[Web 渲染架构选型指南]] 中，SSG 是 **SEO 与性能的上界**：爬虫抓取完整 HTML，无需等 JS；TTFB 极低。与 [[ISR]] 组合可缓解「更新需全量构建」的缺点。

## 边界与反例

| ✅ 适合 | ❌ 不适合 |
|--------|----------|
| 博客详情、About、Projects 列表 | 实时仪表盘、个性化登录页 |
| 内容变更频率低 | 秒级更新需求 |
| 页面量大但可增量构建 | 无限动态路由且无构建策略 |

## 与其他概念的关系

- [[SSR]] — 请求时生成 vs 构建时生成；实时性 vs 速度。
- [[ISR]] — SSG + 后台 revalidate，静态速度 + 延迟更新。
- [[React Server Components]] — App Router 下 RSC 可在构建时被静态化（RSC + SSG）。
- [[Git 原生部署]] — Push 触发构建，SSG 产物部署到 [[Vercel]] Edge。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[SSR]]
- [[ISR]]
- [[Next.js]]
- [[Astro]]
