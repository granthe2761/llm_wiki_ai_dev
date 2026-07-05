---
title: "ISR"
kind: concept
summary: "Incremental Static Regeneration：先返 SSG 缓存，后台按 revalidate 间隔刷新，Stale-While-Revalidate 的 Next.js/Vercel 术语实现。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# ISR

## 定义

**ISR**（Incremental Static Regeneration，增量静态再生）是 **Vercel 为 [[Next.js]] 创造的术语**，实现策略为：**构建时生成静态 HTML 并缓存 → 用户访问直接返缓存 → 到达 revalidate 时间后，下次访问触发后台重新生成**。底层思想是 CDN 行业通用的 **Stale-While-Revalidate**（RFC 5861），非 Next.js 独有。

## 机制与原理

```
构建：generateStaticParams → 静态 HTML 缓存
访问：返回缓存（同 [[SSG]] 速度）
后台：revalidate 到期 → 异步再生 → 后续请求获新版本
```

Next.js：`export const revalidate = 3600`。跨框架：[[Nuxt]] swr/isr、[[SvelteKit]] invalidate + CDN、[[Astro]] 按需渲染、[[Cloudflare]] CDN 原生 SWR。

## 为什么重要

博客列表、作品展示等**会更新但无需秒级实时**的页面：ISR 提供 SSG 级首屏 + 自动后台刷新，无需每次内容变更都全量 [[Git 原生部署]] 构建。更新延迟上限 = `revalidate` 窗口。

## 边界与反例

| ✅ 适合 | ❌ 不适合 |
|--------|----------|
| 博客列表、CMS 驱动页 | 实时库存、聊天、秒级数据 |
| 可接受分钟~小时级延迟 | 强一致读场景 |
| 配合 CDN 边缘缓存 | 配置错误导致缓存不一致 |

首次触发再生的请求可能仍拿到旧数据（stale），属 SWR 语义而非 bug。

## 与其他概念的关系

- [[SSG]] — ISR 的基础是静态缓存；ISR = SSG + 定时/按需失效。
- [[SSR]] — ISR 非每请求渲染；实时性弱于 SSR。
- [[Vercel]] — ISR 与 Edge 缓存深度集成，营销与产品绑定强。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[SSG]]
- [[Next.js]]
- [[Vercel]]
- [[Cloudflare]]
