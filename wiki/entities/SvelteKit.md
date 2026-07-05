---
title: "SvelteKit"
kind: entity
summary: "Svelte 全栈框架，prerender(SSG)、+page.server.js(SSR)、stream 流式与 invalidate 缓存，组件默认同构。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# SvelteKit

## 概述

**SvelteKit** 是 Svelte 的全栈元框架，实现 [[SSG]]（`prerender`）、[[SSR]]（`+page.server.js`）、[[Streaming SSR]]（`stream` 响应）与配合 CDN 的 [[ISR]] 类失效（`invalidate`）。组件**默认同构**，无 React 式 RSC/RCC 文件级划分。

## 渲染对照

| 模式 | SvelteKit API |
|------|--------------|
| [[SSG]] | `prerender` |
| [[SSR]] | `+page.server.js` |
| [[ISR]] | `invalidate` + CDN |
| [[Streaming SSR]] | stream 响应 |
| RSC/RCC | 无；默认同构 |

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[跨框架渲染模式对照]]
- [[SSG]]
- [[SSR]]
- [[Streaming SSR]]
- [[Vercel]]
