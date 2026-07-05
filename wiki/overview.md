---
title: "全局概览"
kind: synthesis
summary: "Web 部署运维知识域；跨传统 VPS 与 Vercel PaaS 两源建立生产就绪链路与托管边界对照框架。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# 全局概览

## 知识域

本维基覆盖 **Web 应用部署与基础设施运维** 主题，关注从「能跑起来」到「生产就绪」之间的能力差距、**传统 VPS 自建**与 **PaaS（以 Vercel 为代表）托管**的路径对照，以及技术栈（Java vs JS 全栈）与合规（ICP）分叉。

## 当前综合论点

**生产就绪的真实工作量要么是 VPS 上补齐七层运维栈，要么是 PaaS 上接受「四层平台化 + 数据/合规/深度运维外置」的边界**——跨源对照见 [[传统 VPS 与 Vercel 部署对照]]。Java Spring Boot/RuoYi 走 VPS/Docker；Next.js 等现代全栈走 Vercel 更优。

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

- 原始资料数：2
- 来源摘要数：2
- 概念页数：10
- 实体页数：6
- 综合页数：1
- 对比页数：1

## 开放问题

- Vercel + 阿里云 DCDN 混合架构的备案合规实操？
- Upstash/Neon vs 自建 Redis/MySQL 的延迟与 TCO 量化？
- 单人/小团队生产就绪的最小能力子集（VPS 路径 vs PaaS 路径）？

## 相关页面

- [维基索引](../index.md)
- [操作日志](../log.md)
- [[传统 VPS 与 Vercel 部署对照]]
- [[传统 VPS 部署完整链路]]
- [[Vercel PaaS 网站上线链路]]
- [[传统网站上线链路：从能跑到能稳定跑]]
- [[Vercel]]
- [[PaaS]]
