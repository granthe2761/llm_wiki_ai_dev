---
title: "Vercel PaaS 网站上线链路"
kind: source
summary: "以 Vercel 为例梳理 PaaS/Frontend Cloud 的 Git→Build→Edge 托管链路，逐项对照传统 VPS 七层能力，明确平台托管边界与 Java 栈不适用性。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# Vercel PaaS 网站上线链路

## 核心摘要

- [[Vercel]] 定位为 **[[PaaS]]** / Frontend Cloud / Serverless Platform，链路为 **Git Push → 构建 → Edge 分发**，而非租用通用服务器。
- 核心路径：`Git Push → Webhook → Build System → Static + [[Serverless Functions]] + [[Edge Functions]] → Vercel Edge Network → 用户最近节点`。
- 平台**全托管**基础设施、运行时、网络加速、构建发布四层；**不托管**数据层（DB/Redis/OSS）、深度长期运维、国内 [[ICP备案]] 合规。
- 与传统环节逐项对照：[[Nginx]] 配置 → `vercel.json`；[[Certbot]] → 全自动 SSL；[[CI-CD]] → Git 原生 Push 即部署；[[灰度发布与回滚]] → Preview URL + Instant Rollback。
- **适用边界**：Next.js/Nuxt/SvelteKit/Astro 等现代前端全栈最优；Java Spring Boot/RuoYi **不适用**（Serverless 冷启动与内存模型不匹配）。

## 关键论点

### Vercel 核心链路

```
Git Push → Webhook 触发
  → Build System（容器化构建 / Framework Preset）
  → 产物：Static Files + Serverless Functions + Edge Functions
  → Vercel Edge Network（全球 Anycast）
  → 用户请求 → 最近 Edge Node
      ├── 静态 → Edge Cache
      ├── API 路由 → Serverless Function（AWS Lambda 封装）
      └── Edge 路由 → Edge Function（V8 Isolate）
```

### 传统 vs Vercel 对照（节选）

| 传统环节 | 传统实现 | Vercel 实现 | 关键差异 |
|---------|---------|------------|---------|
| DNS | 手动 A/CNAME | 自动 `*.vercel.app` 或自定义域名 Anycast | 无需选手动 IP |
| [[ICP备案]] | 国内强制 | **无**（节点海外） | 国内正式业务需国内 CDN 或放弃 Vercel |
| VPS/安全组 | ECS + iptables | **无** | 无 OS/SSH，攻击面趋零 |
| [[Nginx]] | nginx.conf | `vercel.json` / 框架路由 | 声明式，平台处理 SSL/压缩/HTTP2 |
| [[Certbot]] | 手动续期 | **全自动** | 零配置 OCSP Stapling |
| [[进程守护]] | systemd/pm2 | [[Serverless Functions]] | 无状态按需冷启动 |
| [[Redis]] | 自建/云 Redis | 不接，常用 [[Upstash]] | Serverless Redis 一键集成 |
| DB | MySQL/PostgreSQL | Neon/Supabase/PlanetScale 等第三方 | 备份责任在 DB 厂商 |
| [[CI-CD]] | Jenkins/GitHub Actions | **Git 原生** | Push 构建，Preview URL，合并即生产 |
| 回滚 | 蓝绿/滚动 | Instant Rollback + Preview | 点击回滚任意历史部署 |

完整对照见 [[传统 VPS 与 Vercel 部署对照]]。

### 平台托管边界

| 层级 | Vercel 托管？ |
|------|-------------|
| 基础设施（服务器/网络/系统） | ✅ 全托管 |
| 运行时（环境变量/无进程守护） | ✅ 全托管 |
| 网络与加速（DNS/SSL/CDN/DDoS/HTTP2） | ✅ 全托管 |
| 构建与发布 | ✅ 全托管 |
| 数据层（DB/缓存/OSS） | ❌ 需第三方 |
| 长期运维（日志归档/深度监控/备份验证） | ❌ 基础版弱，需 Datadog 等 |
| 合规（备案/数据主权/审计） | ❌ 不解决 |

### 技术栈选型判断

- **现代 JS 全栈框架**（Next.js 等）：Vercel 吃掉 Nginx、SSL、CDN、进程守护，为最优解之一。
- **Java Spring Boot / RuoYi**：Serverless 仅支持 Node/Python/Go 轻量运行时，Java 冷启动与内存占用体验极差 → 更适合阿里云 ECS + Docker 或 Kubernetes。

**一句话**：Vercel 把「服务器、网络、构建、发布」变成水电煤，把「数据库、缓存、长期日志、合规」甩给第三方。

## 涉及实体

- [[Vercel]]
- [[Upstash]]
- [[Nginx]]（对照参照）
- [[Cloudflare]]（Edge 合作方提及）

## 涉及概念

- [[PaaS]]
- [[Serverless Functions]]
- [[Edge Functions]]
- [[Git 原生部署]]
- [[生产就绪部署链路]]（托管边界对照）

## 来源

- `raw/Paas(Verce)网站上线链路.md`

## 相关页面

- [[传统 VPS 与 Vercel 部署对照]]
- [[传统 VPS 部署完整链路]]
- [[Vercel]]
- [[PaaS]]
- [[Serverless Functions]]
- [[Edge Functions]]
