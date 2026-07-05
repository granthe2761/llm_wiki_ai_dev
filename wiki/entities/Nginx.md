---
title: "Nginx"
kind: entity
summary: "开源 Web 服务器与反向代理，在传统 VPS 部署中承担 SSL 终结、负载均衡、压缩、静态缓存与错误页等多重角色。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Nginx

## 概述

**Nginx** 是 [[传统 VPS 部署完整链路]] 中的核心边缘组件：在「最小可跑链路」里它仅是反向代理，在「生产就绪链路」里同时承担 SSL 终结、负载均衡（Upstream）、Gzip/Brotli 压缩、HTTP/2、静态资源缓存策略、自定义 404/50x 错误页等职责。

## 在部署链路中的角色

| 能力层 | Nginx 职责 |
|--------|-----------|
| 网络/访问 | HTTPS 终结（配合 [[Certbot]] 证书）、可选 HTTP/2/3 |
| 性能 | Gzip/Brotli、Cache-Control/ETag 配置、upstream 负载均衡 |
| 发布 | [[灰度发布与回滚]] 的流量切换执行面 |
| 可靠性 | upstream 健康检查、`max_fails` 摘流 |
| 体验 | 静态前端托管、自定义错误页 |

## 典型拓扑

```
Client → Nginx (443 SSL) → upstream backend1/backend2
                        → static files / CDN 回源
                        → 前端 SPA 静态资源
```

单台 VPS 上 Nginx 常同时代理 API 与静态资源；流量上升后扩展为多后端 + SLB/HAProxy/Nginx Upstream 消除单点。

## 运维注意点

- 访问日志与 error log 须 logrotate，否则磁盘爆满（见 [[传统网站上线链路：从能跑到能稳定跑]]）。
- SSL 证书需与 [[Certbot]] 自动续期联动，reload Nginx 加载新 cert。
- 配置变更应版本化，与 [[CI-CD]] 集成测试 staging 配置后再上生产。

## PaaS 对照

[[Vercel]] 以 `vercel.json` 与框架路由约定**声明式替代** Nginx 的 location/SSL/压缩配置，平台自动处理 SSL 终结与 HTTP/2（见 [[Vercel PaaS 网站上线链路]]、[[传统 VPS 与 Vercel 部署对照]]）。Java/Spring 栈仍依赖 Nginx + VPS/Docker，不适用 Vercel Serverless。

## 相关页面

- [[传统网站上线链路：从能跑到能稳定跑]]
- [[Vercel PaaS 网站上线链路]]
- [[Vercel]]
- [[传统 VPS 与 Vercel 部署对照]]
- [[传统 VPS 部署完整链路]]
- [[生产就绪部署链路]]
- [[Certbot]]
- [[灰度发布与回滚]]
- [[健康检查与探活]]
- [[Cloudflare]]
