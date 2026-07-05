---
title: "Certbot"
kind: entity
summary: "Let's Encrypt 证书的自动化申请与续期工具，须配合 cron/systemd timer 避免 90 天过期导致 HTTPS 宕机。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Certbot

## 概述

**Certbot**（原 Let's Encrypt 官方客户端）用于从 Let's Encrypt 自动申请与续期免费 TLS 证书。在 [[生产就绪部署链路]] 的安全合规层，HTTPS 不是「配一次就行」——Let's Encrypt 证书 **90 天过期**，必须配置 certbot 定时任务（cron 或 systemd timer）并在续期后 reload [[Nginx]]，否则到期即全站 HTTPS 不可用。

## 典型工作流

1. 安装 certbot 与 web 服务器插件（如 `python3-certbot-nginx`）。
2. 首次 `certbot --nginx` 或 standalone 模式验证域名所有权。
3. 配置自动续期：`certbot renew` 由 cron/timer 每日执行（仅临近过期时实际续期）。
4. 续期钩子：`--deploy-hook "systemctl reload nginx"` 加载新证书。

## 边界与反例

- **仅 HTTP-01 验证**：需 80 端口可达；[[Cloudflare]] 橙云 + 错误 SSL 模式可能导致验证失败。
- **Wildcard 证书**：需 DNS-01 验证，配置复杂度高于单域名。
- **K8s/cert-manager**：容器环境常用 cert-manager 替代裸机 certbot，语义等价。

## PaaS 对照

[[Vercel]] 等平台全自动 SSL 签发、续期与 OCSP Stapling，**无需** Certbot（见 [[传统 VPS 与 Vercel 部署对照]]）。

## 相关页面

- [[传统网站上线链路：从能跑到能稳定跑]]
- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[Nginx]]
- [[生产就绪部署链路]]
- [[Cloudflare]]
