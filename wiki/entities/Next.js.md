---
title: "Next.js"
kind: entity
summary: "React 全栈框架，App Router 生产级实现 RSC/SSR/SSG/ISR/Streaming，与 Vercel 同源，现代前端部署首选之一。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Next.js

## 概述

**Next.js** 是 Vercel 主导的 React 全栈框架，[[Vercel]] 平台与其构建/路由/渲染 preset 深度集成。在 [[Web 渲染架构：六种模式与跨框架对照]] 中，Next.js 是六种渲染模式（[[SSG]]、[[SSR]]、[[Streaming SSR]]、[[ISR]]、[[React Server Components]]、[[React Client Components]]）的**参考实现**与 API 命名来源。

## 渲染能力映射

| 模式 | Next.js API |
|------|-------------|
| [[SSG]] | `generateStaticParams`、默认静态行为 |
| [[SSR]] | `dynamic = 'force-dynamic'`、`cache: 'no-store'` |
| [[Streaming SSR]] | `Suspense`、`loading.tsx` |
| [[ISR]] | `export const revalidate = N` |
| [[React Server Components]] | App Router 默认（`app/`） |
| [[React Client Components]] | `'use client'` |

## 在知识域中的角色

- **部署路径**：与 [[Vercel]]、[[Git 原生部署]] 组合为 JS 全栈最优解之一（对照 [[传统 VPS 与 Vercel 部署对照]]）。
- **架构选型**：Portfolio 推荐 RSC 为主体 + RCC 仅交互 + SSG/ISR/Streaming 按页型组合（见 [[Web 渲染架构选型指南]]）。
- **不适用**：Java Spring Boot 后端主导项目——应 VPS/Docker，非 Next Serverless。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[跨框架渲染模式对照]]
- [[Vercel]]
- [[React Server Components]]
- [[ISR]]
