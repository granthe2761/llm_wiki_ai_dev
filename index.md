# 维基索引

> 由 LLM 在每次 ingest / query / lint 后自动维护。回答查询前先读此文件定位相关页面。

最后更新：2026-07-05

---

## 概览

- [全局概览](wiki/overview.md) — 部署运维 + 前端渲染架构双域框架 · 更新于 2026-07-05 · 来源数 3

## 综合

- [传统 VPS 部署完整链路](wiki/synthesis/传统 VPS 部署完整链路.md) — 七层链路与 PaaS 托管边界 · 更新于 2026-07-05 · 来源数 2
- [Web 渲染架构选型指南](wiki/synthesis/Web 渲染架构选型指南.md) — 六种模式与 Portfolio 页型选型 · 更新于 2026-07-05 · 来源数 1

## 概念

- [生产就绪部署链路](wiki/concepts/生产就绪部署链路.md) — 从能跑到稳定运行的七层能力差集 · 更新于 2026-07-05 · 来源数 2
- [SSG](wiki/concepts/SSG.md) — 构建时静态 HTML，CDN 直出，SEO 最优 · 更新于 2026-07-05 · 来源数 1
- [SSR](wiki/concepts/SSR.md) — 请求时服务端渲染，数据实时 · 更新于 2026-07-05 · 来源数 1
- [Streaming SSR](wiki/concepts/Streaming SSR.md) — 分块流式 SSR，降低 TTFB · 更新于 2026-07-05 · 来源数 1
- [ISR](wiki/concepts/ISR.md) — 静态缓存 + 后台 revalidate · 更新于 2026-07-05 · 来源数 1
- [React Server Components](wiki/concepts/React Server Components.md) — 服务端组件，零客户端 Bundle · 更新于 2026-07-05 · 来源数 1
- [React Client Components](wiki/concepts/React Client Components.md) — 客户端交互岛，SEO 差 · 更新于 2026-07-05 · 来源数 1
- [PaaS](wiki/concepts/PaaS.md) — 平台托管 infra/运行时，外置数据与合规 · 更新于 2026-07-05 · 来源数 1
- [Serverless Functions](wiki/concepts/Serverless Functions.md) — 无状态按需函数 · 更新于 2026-07-05 · 来源数 1
- [Edge Functions](wiki/concepts/Edge Functions.md) — Edge V8 Isolate 轻量函数 · 更新于 2026-07-05 · 来源数 1
- [Git 原生部署](wiki/concepts/Git 原生部署.md) — Push 即构建部署 · 更新于 2026-07-05 · 来源数 2
- [ICP备案](wiki/concepts/ICP备案.md) — 国内公网 Web 备案硬门槛 · 更新于 2026-07-05 · 来源数 2
- [进程守护](wiki/concepts/进程守护.md) — systemd/supervisor/pm2 · 更新于 2026-07-05 · 来源数 2
- [CI-CD](wiki/concepts/CI-CD.md) — 自动化构建测试部署 · 更新于 2026-07-05 · 来源数 2
- [灰度发布与回滚](wiki/concepts/灰度发布与回滚.md) — 渐进放量与快速回滚 · 更新于 2026-07-05 · 来源数 2
- [健康检查与探活](wiki/concepts/健康检查与探活.md) — 进程存活 vs 服务可用 · 更新于 2026-07-05 · 来源数 1

## 实体

- [Next.js](wiki/entities/Next.js.md) — React 全栈，六种渲染模式参考实现 · 更新于 2026-07-05 · 来源数 1
- [Astro](wiki/entities/Astro.md) — 内容站框架，默认 SSG + Islands · 更新于 2026-07-05 · 来源数 1
- [Nuxt](wiki/entities/Nuxt.md) — Vue 全栈，同构无 RSC 二分 · 更新于 2026-07-05 · 来源数 1
- [SvelteKit](wiki/entities/SvelteKit.md) — Svelte 全栈，prerender/stream · 更新于 2026-07-05 · 来源数 1
- [Vercel](wiki/entities/Vercel.md) — Frontend Cloud，Next.js 同源 PaaS · 更新于 2026-07-05 · 来源数 2
- [Upstash](wiki/entities/Upstash.md) — Serverless Redis · 更新于 2026-07-05 · 来源数 1
- [Nginx](wiki/entities/Nginx.md) — 反向代理、SSL、LB · 更新于 2026-07-05 · 来源数 2
- [Cloudflare](wiki/entities/Cloudflare.md) — DNS、CDN、DDoS、WAF · 更新于 2026-07-05 · 来源数 2
- [Redis](wiki/entities/Redis.md) — Session、缓存、限流 · 更新于 2026-07-05 · 来源数 2
- [Certbot](wiki/entities/Certbot.md) — Let's Encrypt 自动续期 · 更新于 2026-07-05 · 来源数 2

## 来源

- [传统网站上线链路：从能跑到能稳定跑](wiki/sources/传统网站上线链路.md) — VPS 七层生产就绪清单 · 更新于 2026-07-05 · 来源数 1
- [Vercel PaaS 网站上线链路](wiki/sources/Paas(Verce)网站上线链路.md) — Git→Edge 与 VPS 对照 · 更新于 2026-07-05 · 来源数 1
- [Web 渲染架构：六种模式与跨框架对照](wiki/sources/Web渲染架构.md) — SSG/SSR/RSC 等六种模式 · 更新于 2026-07-05 · 来源数 1

## 对比

- [传统 VPS 与 Vercel 部署对照](wiki/comparisons/传统 VPS 与 Vercel 部署对照.md) — 部署七层能力矩阵 · 更新于 2026-07-05 · 来源数 2
- [跨框架渲染模式对照](wiki/comparisons/跨框架渲染模式对照.md) — Next/Nuxt/SvelteKit/Astro API 对照 · 更新于 2026-07-05 · 来源数 1

## 查询归档

_（尚无查询归档。）_
