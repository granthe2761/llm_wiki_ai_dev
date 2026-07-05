---
title: "Web 渲染架构：六种模式与跨框架对照"
kind: source
summary: "系统梳理 SSG/SSR/Streaming SSR/ISR/RSC/RCC 六种渲染模式的技术机制、SEO 影响、跨框架 API 对照及 Portfolio 选型建议。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Web 渲染架构：六种模式与跨框架对照

## 核心摘要

- Web 渲染按 **HTML 生成时机** 与 **是否依赖客户端 JS** 分为六种模式：[[SSG]]、[[SSR]]、[[Streaming SSR]]、[[ISR]]、[[React Server Components]]、[[React Client Components]]。
- **SSG/SSR/Streaming SSR** 是早于 React/Next.js 的行业通用架构；PHP、Rails、Django 本质即 SSR。
- **ISR** 是 Vercel/Next.js 营销术语，底层思想为 CDN 通用的 **Stale-While-Revalidate**（RFC 5861）。
- **RSC/RCC** 是 React App Router 特有划分；Astro Server Islands、Fresh Islands 借鉴类似思想。
- **SEO 核心规律**：爬虫收到完整 HTML（SSG/SSR/RSC）索引效率最高；主内容放 RCC 则 SEO 差、禁 JS 不可见。
- Portfolio 选型：主体内容用 RSC+SSG/ISR；交互元素才用 RCC；慢 API 区块用 Streaming。

## 关键论点

### 六种模式总览

| 模式 | HTML 生成时机 | 需 JS 才见内容 | SEO | 行业通用性 |
|------|-------------|--------------|-----|-----------|
| [[SSG]] | 构建时 | ❌ | ⭐⭐⭐ | 极高 |
| [[SSR]] | 请求时 | ❌ | ⭐⭐⭐ | 极高 |
| [[Streaming SSR]] | 请求时分块流式 | 部分 | ⭐⭐⭐ | 高 |
| [[ISR]] | 构建+后台定时 | ❌ | ⭐⭐⭐ | 术语特有，思想通用 |
| [[React Server Components]] | 请求时（服务端） | ❌ | ⭐⭐⭐ | React 特有 |
| [[React Client Components]] | 浏览器运行时 | ✅ | ⭐ | React 术语 |

### 全行业通用模式（早于 Next.js）

**[[SSG]]**：构建阶段生成完整 HTML，CDN 直出；TTFB 最低，内容更新需重新构建。跨框架：Jekyll、Hugo、Gatsby、Astro、Nuxt `nuxt generate`。

**[[SSR]]**：请求时实时查库渲染；数据最新，TTFB 慢于 SSG。跨框架：PHP/Rails/Django/Spring MVC、Vue `renderToString`、SvelteKit `+page.server.js`。

**[[Streaming SSR]]**：Chunked Transfer，先返框架再流式填充；Suspense 边界设计关键。跨框架：Django `StreamingHttpResponse`、Vue pipeable、SvelteKit stream。

### React/Next.js 演进模式

**[[ISR]]**：先返 SSG 缓存，后台按 `revalidate` 刷新；非实时。跨框架：Nuxt swr/isr、SvelteKit invalidate、[[Cloudflare]] stale-while-revalidate。

**[[React Server Components]]**：App Router 默认；服务端直访 DB，不进客户端 Bundle；不能直接用 `window`/localStorage。

**[[React Client Components]]**：`'use client'` 标记；完整浏览器 API 与状态，但 SEO 差、增 Bundle。Portfolio **主体内容不应放 RCC**。

### 跨框架 API 对照（节选）

| 模式 | [[Next.js]] | [[Nuxt]] | [[SvelteKit]] | [[Astro]] |
|------|------------|---------|--------------|----------|
| SSG | `generateStaticParams` | `nuxt generate` | `prerender` | 默认 SSG |
| SSR | `dynamic = 'force-dynamic'` | `ssr: true` | `+page.server.js` | server islands |
| ISR | `revalidate` | swr/isr | invalidate+CDN | 按需渲染 |
| Streaming | Suspense + `loading.tsx` | Suspense 流式 | stream | 渐进式渲染 |
| RSC | App Router 默认 | 无 | 无 | Server Islands |
| RCC | `'use client'` | 默认同构 | 默认同构 | `client:*` |

完整对照见 [[跨框架渲染模式对照]]。

### Portfolio 选型（整合建议）

| 页面 | 推荐 | 理由 |
|------|------|------|
| 首页/About | RSC + SSG | 数据固定，爬虫直达 |
| 博客列表 | RSC + ISR | 有更新自动刷新，无需手动部署 |
| 博客详情 | RSC + SSG | 每篇独立 HTML |
| 联系表单 | RSC 外壳 + RCC 表单 | 框架静态，交互客户端 |
| 实时数据 | RSC + Streaming | 主体不阻塞，慢块流式 |

### 一句话总结

SSG/SSR/Streaming 是 Web 通用语言；ISR 包装 SWR 思想；RSC/RCC 是 React 实现但「服务端组件」正被 Astro/Fresh 借鉴。转 Nuxt/SvelteKit/Astro 时概念平移，仅 API 名不同。

## 涉及实体

- [[Next.js]]
- [[Nuxt]]
- [[SvelteKit]]
- [[Astro]]
- [[Vercel]]

## 涉及概念

- [[SSG]]
- [[SSR]]
- [[Streaming SSR]]
- [[ISR]]
- [[React Server Components]]
- [[React Client Components]]

## 来源

- `raw/Web渲染架构.md`

## 相关页面

- [[Web 渲染架构选型指南]]
- [[跨框架渲染模式对照]]
- [[Vercel]]
- [[Git 原生部署]]
