---
title: "React Client Components"
kind: concept
summary: "React 客户端组件：'use client' 标记，浏览器执行，完整交互与浏览器 API，但 SEO 差、增 Bundle，仅用于交互岛屿。"
source_files:
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# React Client Components

## 定义

**React Client Components**（RCC，React 客户端组件）是 [[React Server Components]] 架构下的相对概念。文件顶部 `'use client'` 标记后，组件在**浏览器**下载并执行 JS 才渲染，拥有 `useState`/`useEffect` 与完整浏览器 API；服务端可能仅输出占位符。

## 机制与原理

```
服务端：占位符或空 shell
浏览器：下载 Bundle → 执行 React → 渲染 → 可能再请求 API
```

跨框架：[[Astro]] `client:*` 指令标记交互岛；Vue/Svelte **无 RCC 精确术语**——单文件组件默认同构。

## 为什么重要

**SEO 与首屏的关键反模式**：若 Portfolio **主体内容**放在 RCC，爬虫可能只见空壳，禁 JS 用户不可见内容。原文明确：**RCC 仅用于交互元素**——搜索框、点赞按钮、轮播、模态框、表单；主体内容应留在 RSC。

## 边界与反例

| ✅ 适合 RCC | ❌ 不应 RCC |
|------------|------------|
| 点赞、表单、轮播、模态框 | 文章正文、项目列表、About 页 |
| 依赖 localStorage/window | 爬虫需索引的 SEO 内容 |
| 客户端状态与动画 | 可服务端直出的数据展示 |

## 与其他概念的关系

- [[React Server Components]] — 默认/server vs 显式/client 分工。
- [[SSR]] — RCC 仍可能 SSR 出 shell，但**可索引内容**依赖 hydration 完成。
- [[SSG]] — 静态 HTML 中 RCC 部分仍要客户端激活。

## 相关页面

- [[Web 渲染架构：六种模式与跨框架对照]]
- [[Web 渲染架构选型指南]]
- [[React Server Components]]
- [[Next.js]]
- [[Astro]]
