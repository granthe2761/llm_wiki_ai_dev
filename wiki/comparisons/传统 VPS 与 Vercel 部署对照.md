---
title: "传统 VPS 与 Vercel 部署对照"
kind: comparison
summary: "跨两份来源逐项对照传统 VPS 七层生产就绪能力与 Vercel PaaS 托管边界，含技术栈选型结论。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# 传统 VPS 与 Vercel 部署对照

## 综合论点

**Vercel 将 [[生产就绪部署链路]] 中 infra/运行时/网络/发布四层变为平台默认能力，但刻意不托管数据层、深度运维与国内合规**——因此「Vercel 省运维」与「VPS 全自控」不是简单替代关系，而是 **Java 重后端 vs JS 全栈** 与 **国内合规 vs 海外 Edge** 两条分叉上的选型问题。

## 链路结构对照

| | 传统 VPS（最小→完整） | Vercel PaaS |
|--|---------------------|-------------|
| **触发** | 手动 scp / [[CI-CD]] | [[Git 原生部署]]：Push → Webhook |
| **计算** | VPS + [[进程守护]] | [[Serverless Functions]] + [[Edge Functions]] |
| **边缘** | [[Nginx]] + [[Certbot]] | `vercel.json` + 全自动 SSL |
| **加速** | [[Cloudflare]] / 云 CDN | Edge Network 原生 CDN |
| **数据** | DB + [[Redis]] + OSS 自建 | 第三方：Neon/Supabase、[[Upstash]]、Blob/S3 |
| **合规** | [[ICP备案]] 可完成 | ❌ 节点海外，国内正式业务需额外方案 |

## 逐项能力矩阵

来源：[[传统网站上线链路：从能跑到能稳定跑]] × [[Vercel PaaS 网站上线链路]]

| 能力层 | 传统 VPS | Vercel | 维基判断 |
|--------|---------|--------|---------|
| DNS | 手动 A/CNAME | Anycast 自动 | Vercel 省配置 |
| 备案 | 国内必须 | 不提供 | **国内业务 VPS/国内云仍必要** |
| DDoS/WAF | Cloudflare/高防 | Edge 默认集成 | Vercel 免费版已有基础防护 |
| 安全组/SSH | 自建加固 | 无 OS 暴露 | Vercel 攻击面更小 |
| HTTPS 续期 | [[Certbot]] + cron | 全自动 | Vercel 零运维 |
| HTTP/2·3、压缩 | Nginx 手工 | 默认开启 + 构建预压缩 | Vercel 默认优 |
| 进程守护 | systemd/pm2 | 平台调度 | 语义不同：Serverless 冷启动 |
| 健康检查 | 自建 `/health` | 平台内置 + 504 超时 | VPS 可更深定制 |
| 日志 | logrotate | Dashboard 1–3 天 | VPS 需自建；Vercel 需 Datadog |
| DB/Redis/OSS | 自建或云产品 | 全第三方 | **两者均需规划数据层** |
| CI/CD | Jenkins/Actions | Git 内置 | Vercel 对前端团队 friction 更低 |
| 灰度/回滚 | 蓝绿/滚动 | Preview + Instant Rollback | Vercel 回滚 UX 更简单 |
| 邮件/SEO | 自建/SMTP | Resend/SendGrid + 框架 SSR | 均依赖第三方或框架 |

## 技术栈选型（跨源共识）

| 项目类型 | 推荐路径 | 依据 |
|---------|---------|------|
| Next.js / Nuxt / SvelteKit / Astro | [[Vercel]] | 平台与框架同源优化，托管 Nginx/SSL/CDN/守护 |
| Java Spring Boot / RuoYi | VPS + Docker / K8s | Serverless 不支持 JVM 生产体验 |
| 国内正式 ToC 业务 | VPS 或国内云 + [[ICP备案]] | Vercel 无法解决备案与数据主权 |

## 观点演进

| 时间 | 结论 | 来源 | 状态 |
|------|------|------|------|
| 2026-07 | Docker/K8s/Serverless「可托管多数环节」，细节待补充 | [[传统 VPS 部署完整链路]] | ❌ 已被细化 |
| 2026-07 | Vercel 明确托管四层、不托管数据/合规/深度运维；Java 不适用 | [[Vercel PaaS 网站上线链路]] | ✅ 当前 |

## 知识缺口

- Vercel + 阿里云 DCDN 混合架构的备案合规实操路径。
- Upstash/Neon 与自建 Redis/MySQL 的延迟与成本量化对比。
- 同等 QPS 下 Vercel 账单 vs 单台 VPS TCO。

## 相关页面

- [[传统 VPS 部署完整链路]]
- [[Vercel PaaS 网站上线链路]]
- [[传统网站上线链路：从能跑到能稳定跑]]
- [[生产就绪部署链路]]
- [[Vercel]]
- [[PaaS]]
- [[ICP备案]]
- [[Nginx]]
- [[CI-CD]]
- [[Git 原生部署]]
