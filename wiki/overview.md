---
title: "全局概览"
kind: synthesis
summary: "Web 部署运维 + 前端渲染架构双域；VPS/PaaS 托管边界与 SSG/SSR/RSC 选型框架。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# 全局概览

## 知识域

本维基覆盖 **Web 应用部署与基础设施运维** 及 **前端渲染架构** 两大主题：

1. **部署运维**：从「能跑」到「生产就绪」的 VPS 七层栈 vs [[Vercel]] 等 [[PaaS]] 托管边界（[[传统 VPS 与 Vercel 部署对照]]）。
2. **渲染架构**：SSG/SSR/Streaming/ISR/RSC/RCC 六种模式、跨框架 API 对照、Portfolio 选型（[[Web 渲染架构选型指南]]）。

技术栈分叉：**Java 后端 → VPS/Docker**；**Next.js 等 JS 全栈 → PaaS + RSC/SSG**。

## 当前综合论点

1. **部署**：生产就绪 = VPS 补齐七层运维栈，或 PaaS 接受「四层平台化 + 数据/合规外置」。
2. **渲染**：主内容必须服务端出 HTML（RSC/SSG/SSR）；RCC 仅限交互；ISR 缓更新；Streaming 解慢 API。
3. **联动**：渲染模式决定 SEO 上限与构建/函数成本，部署平台（Vercel vs VPS）决定运维负担。

## 知识结构

| 类型 | 目录 | 说明 |
|------|------|------|
| 来源摘要 | `wiki/sources/` | 每份 raw 资料一篇 |
| 概念 | `wiki/concepts/` | 抽象思想、机制、方法论 |
| 实体 | `wiki/entities/` | 工具、产品、平台 |
| 综合 | `wiki/synthesis/` | 跨来源主题归纳 |
| 对比 | `wiki/comparisons/` | 结构化对比分析 |
| 查询 | `wiki/queries/` | 探索性问答的归档 |

## 来源统计

- 原始资料数：3
- 来源摘要数：3
- 概念页数：16
- 实体页数：10
- 综合页数：2
- 对比页数：2

## 开放问题

- Vercel + 国内 CDN 备案合规实操？
- 国内搜索引擎对流式 HTML 的支持？
- 各渲染模式 Core Web Vitals 量化对比？

## 相关页面

- [维基索引](../index.md)
- [操作日志](../log.md)
- [[传统 VPS 与 Vercel 部署对照]]
- [[Web 渲染架构选型指南]]
- [[跨框架渲染模式对照]]
- [[Vercel]]
- [[Next.js]]
