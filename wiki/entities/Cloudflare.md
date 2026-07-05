---
title: "Cloudflare"
kind: entity
summary: "DNS、CDN、DDoS 防护与 WAF 服务商，传统裸 VPS 部署中常用的免费代理与安全层。"
source_files:
  - "raw/传统网站上线链路.md"
updated: 2026-07-05
---

# Cloudflare

## 概述

**Cloudflare** 在 [[生产就绪部署链路]] 的网络与访问层中扮演 DNS 解析、CDN 加速、DDoS 防护与基础 WAF 角色。原文指出：裸 VPS 公网 IP 易被扫描与攻击，**至少**应使用 Cloudflare 免费版做一层代理，或购买云厂商 DDoS 高防。

## 关键能力

| 能力 | 与传统 VPS 部署的关系 |
|------|---------------------|
| **DNS** | A/AAAA/CNAME 管理，TTL 控制，解析稳定性 |
| **橙云代理** | 隐藏源站 IP，吸收 L3/L4/L7 攻击 |
| **免费 WAF** | 基础规则集，商业版规则更完整 |
| **CDN** | 静态资源边缘缓存，减轻源站 [[Nginx]] 压力 |

## 部署考量

- **源站连接**：橙云开启后，源站看到的是 Cloudflare IP，需在 [[Nginx]]/防火墙配置真实 IP 转发（`CF-Connecting-IP`）。
- **SSL 模式**：Flexible / Full / Full (Strict) 影响源站是否必须 HTTPS；生产推荐 Full (Strict)。
- **与 [[ICP备案]] 的关系**：境内用户 + 境内源站时，Cloudflare 橙云 + 未备案组合可能导致合规或连通性问题，需按业务 jurisdiction 设计。

## 相关页面

- [[传统网站上线链路：从能跑到能稳定跑]]
- [[ICP备案]]
- [[生产就绪部署链路]]
- [[Nginx]]
- [[传统 VPS 部署完整链路]]
