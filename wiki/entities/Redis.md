---
title: "Redis"
kind: entity
summary: "内存键值存储，生产环境常用于 Session、热点数据缓存、限流等，是传统 Web 栈数据层的标配组件。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Redis

## 概述

**Redis** 是 [[传统 VPS 部署完整链路]] 数据层中「几乎必配」的缓存与会话存储组件。原文将其与关系型数据库（MySQL/PostgreSQL/Oracle）、对象存储（OSS/S3/MinIO）并列为数据层三要素，解决 Session 共享、热点读、限流计数等场景。

## 典型用途

| 场景 | 说明 |
|------|------|
| **Session 存储** | 多后端实例间共享登录态，避免 sticky session 单点 |
| **热点数据缓存** | 减轻 DB 读压力，需 TTL 与缓存穿透/击穿策略 |
| **限流** | 滑动窗口计数、令牌桶，配合 API 网关或应用层 |
| **消息/队列** | 轻量 pub/sub 或 list 队列（非专业 MQ 场景的妥协） |

## 部署拓扑

- **与 VPS 混部**：成本低，但争抢 CPU/内存，且端口暴露增加攻击面。
- **独立实例/托管 Redis**：资源隔离，便于备份与版本升级；云厂商提供主从/集群。

## 运维注意点

- 持久化策略（RDB/AOF）与 [[生产就绪部署链路]] 中的备份恢复演练。
- 内存上限与 eviction 策略，避免 OOM 拖垮同机 [[Nginx]]/应用进程。
- 密码与 bind 地址限制，禁止公网无鉴权暴露 6379。

## PaaS 对照

[[Vercel]] 不直接提供 Redis；常用 [[Upstash]] Serverless Redis 一键集成（见 [[Vercel PaaS 网站上线链路]]）。数据层责任仍在第三方，与传统 VPS 自建 Redis 的运维模型不同。

## 相关页面

- [[传统网站上线链路：从能跑到能稳定跑]]
- [[Vercel PaaS 网站上线链路]]
- [[Upstash]]
- [[Vercel]]
- [[传统 VPS 部署完整链路]]
- [[生产就绪部署链路]]
- [[Nginx]]
