---
title: "CI-CD"
kind: concept
summary: "持续集成与持续部署流水线，自动化构建、测试与发布，替代手动 scp 上传的生产发布方式。"
source_files:
  - "raw/传统网站上线链路.md"
  - "raw/Paas(Verce)网站上线链路.md"
updated: 2026-07-05
---

# CI-CD

## 定义

**CI/CD**（Continuous Integration / Continuous Deployment 或 Delivery）是将代码变更从提交到生产环境的自动化流水线：代码 push → 自动构建 → 单元/集成测试 → 制品打包 → 部署到目标环境。在 [[生产就绪部署链路]] 中，它解决「如何把代码可靠地弄到服务器上」这一发布运维层核心问题。

## 为什么重要

手动 `scp` 上传 jar/静态包存在：人为漏步骤、环境不一致、无法审计「谁上了什么版本」、回滚靠找旧包等系统性风险。原文将 CI/CD 与 [[灰度发布与回滚]]、环境隔离、密钥管理并列为发布层必备能力。

## 典型工具链

| 平台 | 定位 |
|------|------|
| GitHub Actions | 与 GitHub 仓库深度集成，适合中小团队 |
| GitLab CI | 自建或 SaaS，流水线与仓库一体 |
| Jenkins | 可高度定制，插件生态成熟，运维成本较高 |

流水线常见阶段：lint → test → build artifact → deploy to staging → smoke test → deploy to production（可接 [[灰度发布与回滚]]）。

## 与传统 VPS 部署的衔接

- 制品推送到服务器：SSH + rsync、Ansible、或 agent 拉取 OSS 上的构建产物。
- 部署后触发 [[进程守护]] reload（systemd restart / pm2 reload）。
- 配置与密钥通过环境变量或配置中心注入，禁止写死在代码库。

## 边界与反例

- **CI/CD ≠ 自动上生产**：Continuous Delivery 可能停在「可一键发布」；Continuous Deployment 才默认 merge 即上线，需强测试与监控兜底。
- **小项目过度工程**：单人 side project 可暂用手动部署，但应保留可脚本化路径以便规模增长。

## 与其他概念的关系

- [[灰度发布与回滚]] — CI/CD 产出制品后，发布策略决定如何切流量与回滚。
- [[进程守护]] — 部署动作的最终执行层。
- [[生产就绪部署链路]] — CI/CD 是发布运维层核心组件。

- **[[Git 原生部署]]** — [[Vercel]] 等 [[PaaS]] 将 CI/CD 压缩为 Git webhook：Push 即构建，无需独立 Jenkins（见 [[Vercel PaaS 网站上线链路]]）。

## 相关页面

- [[传统网站上线链路：从能跑到能稳定跑]]
- [[Vercel PaaS 网站上线链路]]
- [[Git 原生部署]]
- [[Vercel]]
- [[生产就绪部署链路]]
- [[灰度发布与回滚]]
- [[进程守护]]
