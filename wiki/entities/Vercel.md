---
title: "Vercel"
kind: entity
summary: "Frontend Cloud / Serverless Platform，以 Git 为源、构建为桥、Edge 为终点的 PaaS 托管平台，托管 infra/运行时/网络/发布四层。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Vercel

## 概述

**Vercel** 是 [[PaaS]] 范畴内的 Frontend Cloud / Serverless Platform（见 [[Vercel PaaS 网站上线链路]]）。它不是通用 VPS 租用，而是 **Git Push → 构建 → Edge Network 分发** 的托管链路，产物包括静态文件、[[Serverless Functions]]（AWS Lambda 封装）与 [[Edge Functions]]（V8 Isolate）。

## 核心链路

```
Git Push → Webhook → Build System（Framework Preset）
  → Static + Serverless Functions + Edge Functions
  → Edge Network（Anycast）→ 最近节点响应
```

## 平台能力

| 能力 | 实现 |
|------|------|
| 域名/DNS | `*.vercel.app` 或外部域名 CNAME，Anycast 自动路由 |
| SSL | 全自动签发、续期、OCSP Stapling |
| CDN/压缩 | Edge Network 原生 CDN；构建时预压缩 `.br`/`.gz` |
| 路由/反代 | `vercel.json` 或框架约定，替代 [[Nginx]] 手工配置 |
| 发布 | [[Git 原生部署]]：Push 即构建，Preview URL，合并即生产 |
| 回滚 | Instant Rollback 至任意历史部署 |
| 对象存储 | Vercel Blob（原生）或 S3/R2 |
| 监控 | Vercel Analytics / Speed Insights（Web Vitals）；深度 APM 需第三方 |

## 不托管的能力

- **数据层**：数据库（Neon、Supabase、PlanetScale 等）、[[Redis]]（常用 [[Upstash]]）、备份——责任在第三方。
- **合规**：无 [[ICP备案]]，节点在海外；国内正式业务需额外方案。
- **长期运维**：Dashboard 日志保留 1–3 天，归档需 Datadog 等。

## 适用边界

| 场景 | 判断 |
|------|------|
| Next.js / Nuxt / SvelteKit / Astro | ✅ 最优解之一 |
| Java Spring Boot / RuoYi | ❌ Serverless 冷启动与内存模型不适用；选 ECS/K8s |

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[传统 VPS 与 Vercel 部署对照]]
- [[PaaS]]
- [[Serverless Functions]]
- [[Edge Functions]]
- [[Git 原生部署]]
- [[Upstash]]
- [[Nginx]]
- [[ICP备案]]
