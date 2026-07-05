---
title: "Web 渲染架构选型指南"
kind: synthesis
summary: "跨六种渲染模式与 Portfolio 页型的选型综合：通用模式优先、RSC 主体+RCC 交互、ISR 缓更新、Streaming 解慢 API。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Web 渲染架构选型指南

## 综合论点

**渲染模式选型应先看 HTML 何时生成、爬虫能否无 JS 读到内容，再按页型叠加缓存策略**——SSG/SSR/Streaming 是跨框架通用语言；React 栈上 RSC 承担主体、RCC 仅限交互岛；ISR 解决「 mostly 静态、偶尔更新」；转框架时概念平移、API 改名即可。

## 论证

### 决策轴（按优先级）

1. **SEO 与可索引性** — 主内容必须在服务端生成完整 HTML（[[SSG]]/[[SSR]]/[[React Server Components]]）；[[React Client Components]] 不可承载正文。
2. **数据新鲜度** — 静态 → SSG；实时 → SSR；延迟可接受 → [[ISR]]。
3. **感知性能** — 慢 API 区块 → [[Streaming SSR]] + Suspense，不阻塞框架。
4. **交互边界** — 表单/按钮/轮播 → RCC 或 Astro `client:*`；其余服务端。

### Portfolio 页型推荐（来源整合）

| 页型 | 推荐组合 | 通用思想 |
|------|---------|---------|
| 首页 / About | RSC + [[SSG]] | 固定内容，构建出 HTML |
| 博客列表 | RSC + [[ISR]] | 静态缓存 + 后台刷新 |
| 博客详情 | RSC + [[SSG]] | 每篇独立 HTML，爬虫直达 |
| 联系表单 | RSC 外壳 + [[React Client Components]] 表单 | 框架静态，交互客户端 |
| 实时访客数等 | RSC + [[Streaming SSR]] | 主体先出，数字流式 |

### 与部署链路的衔接

- [[SSG]]/[[ISR]] 产物由 [[Git 原生部署]] 构建后上 [[Vercel]] Edge，与 [[Vercel PaaS 网站上线链路]] 的静态+函数模型一致。
- 纯 [[SSR]] 每请求消耗 Serverless/服务器算力，成本模型不同于 SSG。
- SEO 配置（sitemap、meta）在 [[生产就绪部署链路]] 业务体验层；**渲染模式决定 SEO 上限**。

### 跨框架迁移

[[Next.js]] → [[Nuxt]] / [[SvelteKit]] / [[Astro]]：六种概念**完全平移**，对照见 [[跨框架渲染模式对照]]。ISR 术语可能变为 swr/invalidate/按需渲染，思想均为 SWR。

## 知识缺口

- 各模式在 Core Web Vitals（LCP/INP）上的量化对比数据。
- 国内爬虫（百度/搜狗）对流式 HTML 的支持实测。
- RSC + 边缘 DB（Neon/PlanetScale）延迟对 Streaming 收益的影响。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[跨框架渲染模式对照]]
- [[SSG]]
- [[SSR]]
- [[ISR]]
- [[React Server Components]]
- [[React Client Components]]
- [[Next.js]]
- [[Vercel]]
- [[传统 VPS 与 Vercel 部署对照]]
