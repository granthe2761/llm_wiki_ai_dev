---
title: "Nuxt"
kind: entity
summary: "Vue 全栈框架，支持 nuxt generate(SSG)、ssr、swr/isr 与 Suspense 流式，无 RSC/RCC 严格二分，组件默认同构。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Nuxt

## 概述

**Nuxt** 是 Vue 生态的全栈框架，覆盖 [[SSG]]（`nuxt generate`）、[[SSR]]（`ssr: true`）、[[ISR]] 类能力（`swr`/`isr` 模式）与 Suspense 流式渲染。与 React/Next 不同，**Vue 单文件组件默认同构**，不区分 [[React Server Components]] / [[React Client Components]] 精确术语。

## 渲染对照

| 模式 | Nuxt API |
|------|---------|
| [[SSG]] | `nuxt generate` |
| [[SSR]] | `ssr: true` |
| [[ISR]] | `swr` / `isr` |
| [[Streaming SSR]] | Suspense + 流式 |
| RSC/RCC | 无对应；`.vue` 同构 |

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[跨框架渲染模式对照]]
- [[SSG]]
- [[SSR]]
- [[ISR]]
- [[Vercel]]
