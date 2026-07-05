---
title: "Upstash"
kind: entity
summary: "Serverless Redis 服务，Vercel 生态常用第三方缓存集成，替代自建 Redis 的数据层方案。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Upstash

## 概述

**Upstash** 是 Serverless Redis 服务，在 [[Vercel]] 部署模型中充当 [[Redis]] 的第三方替代——因 Vercel **不直接提供**缓存层，官方生态推荐 Upstash 一键集成（见 [[Vercel PaaS 网站上线链路]]）。

## 在 Vercel 栈中的角色

| 传统 VPS | Vercel + Upstash |
|---------|-----------------|
| 自建 Redis 或购买云 Redis 实例 | Serverless Redis，按请求/存储计费 |
| 与 VPS 混部或独立实例，自行运维端口/备份 | 平台化 API，无进程守护需求 |
| [[进程守护]]、内存上限、持久化策略自建 | 备份与 SLA 由 Upstash 负责 |

典型用途不变：Session 共享、热点缓存、限流计数——但执行模型从「长期运行进程旁的 Redis 实例」变为「HTTP/API 调用的 Serverless Redis」。

## 边界

- 延迟特性与常驻 Redis 不同，需评估 Serverless 函数 + Upstash 的组合延迟。
- 数据主权、合规与 [[ICP备案]] 仍取决于 Upstash 区域与业务 jurisdiction，Vercel 不代管。

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[Redis]]
- [[传统 VPS 与 Vercel 部署对照]]
- [[PaaS]]
