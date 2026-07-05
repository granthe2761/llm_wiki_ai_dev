---
title: "Serverless Functions"
kind: concept
summary: "无状态、按需冷启动的函数运行时，平台自动调度，替代传统 VPS 上的进程守护与常驻后端进程。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Serverless Functions

## 定义

**Serverless Functions** 是 PaaS 上的无状态计算单元：请求到达时平台启动隔离运行时执行 handler，空闲时不占常驻进程。在 [[Vercel]] 链路中，API 路由映射到 Serverless Functions（底层 AWS Lambda 封装），替代传统 VPS 上的 `java -jar` / `node app.js` + [[进程守护]]。

## 为什么重要

传统链路中 [[进程守护]]、[[健康检查与探活]]、logrotate 是为**长期运行进程**设计的；Serverless 模型下这些语义由平台承担：超时返回 504、失败可配置重试、无 SSH/OS 攻击面。但引入**冷启动延迟**与**运行时限制**（内存、时长、语言支持）。

## Vercel 上的行为

- 支持 Node.js、Python、Go 等轻量运行时；**不支持** Java Spring Boot 等重型 JVM 栈（冷启动与内存占用体验极差，见 [[Vercel PaaS 网站上线链路]]）。
- 与 [[Edge Functions]] 分工：API 路由 → Serverless；低延迟 Edge 逻辑 → V8 Isolate  at edge。

## 与传统 VPS 对照

| 维度 | VPS 常驻进程 | Serverless Functions |
|------|-------------|---------------------|
| 守护 | systemd/pm2 自建 | 平台调度 |
| 计费 | 机器闲置也付费 | 按请求/执行时间 |
| 状态 | 可内存 Session | 无状态，需 [[Redis]]/DB |
| 冷启动 | 无（已 warm） | 首请求可能慢 |

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[Edge Functions]]
- [[进程守护]]
- [[PaaS]]
- [[传统 VPS 与 Vercel 部署对照]]
