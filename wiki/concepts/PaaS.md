---
title: "PaaS"
kind: concept
summary: "Platform as a Service：平台托管基础设施与运行时，开发者以代码和配置交付应用，按用量计费而非租服务器。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# PaaS

## 定义

**PaaS**（Platform as a Service）将操作系统以下层（服务器、网络、运行时调度）托管给平台，开发者聚焦应用代码与声明式配置。在 Web 部署语境中，[[Vercel]] 被归类为 PaaS，更精确标签还包括 **Serverless Platform** 或 **Frontend Cloud**——以 Git 为源、构建为桥、Edge 为终点，而非「买一台 VPS 自己装环境」。

## 为什么重要

[[生产就绪部署链路]] 七层能力在传统 VPS 上需全部或大部分自建；PaaS 将其中**基础设施、运行时、网络加速、构建发布**四层变为平台默认能力（见 [[传统 VPS 与 Vercel 部署对照]]）。选型本质是：**用 vendor 托管换运维负担，用平台边界换自主可控**。

## PaaS 托管 vs 仍须自建（以 Vercel 为例）

| 层级 | PaaS（Vercel） | 仍须第三方/自建 |
|------|---------------|----------------|
| Infra + OS | ✅ | — |
| 运行时/守护 | ✅ Serverless | — |
| DNS/SSL/CDN/WAF | ✅ Edge Network | — |
| CI/CD/回滚 | ✅ Git 原生 | — |
| DB/缓存/OSS | — | Neon、[[Upstash]]、Blob/S3 |
| 深度监控/日志归档 | — | Datadog 等 |
| 国内合规 | — | [[ICP备案]]、数据主权 |

## 与其他概念的关系

- **IaaS（VPS/ECS）** — 租虚拟机，OS 及以上自建；见 [[传统 VPS 部署完整链路]]。
- **Serverless** — PaaS 子集，函数按需冷启动；见 [[Serverless Functions]]。
- **SaaS** — 直接消费成品应用，不涉及部署链路。

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[传统 VPS 与 Vercel 部署对照]]
- [[生产就绪部署链路]]
- [[Git 原生部署]]
