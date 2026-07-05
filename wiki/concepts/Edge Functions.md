---
title: "Edge Functions"
kind: concept
summary: "在全球 Edge 节点以 V8 Isolate 运行的轻量函数，用于低延迟路由与边缘逻辑，与中心 Serverless Functions 分工。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Edge Functions

## 定义

**Edge Functions** 在 CDN/Edge 节点（非中心区域）以 **V8 Isolate** 沙箱执行，面向低延迟、短逻辑场景（A/B 路由、鉴权头改写、geo 分流）。在 [[Vercel]] 核心链路中，用户请求到达最近 Edge Network 节点后：静态资源直出 Edge Cache，API 走 [[Serverless Functions]]，Edge 路由走 Edge Functions。

## 为什么重要

传统 VPS 上类似能力需 [[Nginx]] + 多地部署或 [[Cloudflare]] Workers 自建；Vercel 将 Edge 执行与构建产物一体化分发，是「Frontend Cloud」区别于纯 Serverless 托管的关键组件。

## 与 Serverless Functions 的分工

| | Edge Functions | Serverless Functions |
|--|----------------|---------------------|
| 运行位置 | 全球 Edge 节点 | 区域 Lambda 类中心 |
| 隔离 | V8 Isolate | 容器/Lambda |
| 典型用途 | 路由、轻量变换 | CRUD API、重逻辑 |
| 限制 | CPU/时长更严 | 相对宽松 |

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[Serverless Functions]]
- [[PaaS]]
- [[Cloudflare]]
