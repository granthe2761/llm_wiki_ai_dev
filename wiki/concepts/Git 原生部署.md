---
title: "Git 原生部署"
kind: concept
summary: "以 Git Push 为发布触发器的 CI/CD 模型，Push 即构建、分支即 Preview、合并即生产，替代独立 Jenkins 流水线。"
source_files:
  - "raw/Paas(Verce)网站上线链路.md"
  - "raw/Web渲染架构.md"
updated: 2026-07-05
---

# Git 原生部署

## 定义

**Git 原生部署**（Git-native Deployment）将 [[CI-CD]] 深度绑定版本控制：代码 push 触发 Webhook → 自动构建 → 部署到 Preview 或 Production 环境，无需单独维护 Jenkins/GitLab Runner 与 scp 制品上传。是 [[Vercel]] 等 [[PaaS]] 的默认发布模型。

## 为什么重要

传统 [[生产就绪部署链路]] 发布层需自建 [[CI-CD]]、环境隔离与 [[灰度发布与回滚]]；Git 原生部署将三者压缩为平台能力：

- **Push 即构建**：每次 commit 触发 Build System（Framework Preset）。
- **Preview Deployment**：每个分支/PR 自动生成 Preview URL，天然环境隔离。
- **合并即生产**：main 分支 merge 触发 Production 部署。
- **Instant Rollback**：Dashboard 一键回滚至任意历史部署，语义上等价于简化版 [[灰度发布与回滚]]。

## 与传统 CI/CD 对照

| 维度 | Jenkins/GitHub Actions + VPS | Git 原生（Vercel） |
|------|------------------------------|-------------------|
| 触发 | 手写 pipeline yaml | Git webhook 内置 |
| 构建机 | 自维护 runner/agent | 平台容器化构建 |
| 环境 | 手动 staging/prod 配置 | Production/Preview/Development 变量隔离 |
| 制品 | jar/dist 上传服务器 | 平台托管构建产物 |

## 边界

- 构建逻辑仍受平台限制（框架 preset、构建时长、monorepo 策略）。
- 数据库 migration、有状态回滚需额外设计，非 Git push 自动覆盖。

- **构建产物**：Push 触发 [[SSG]]/[[ISR]] 静态 HTML 或 [[SSR]] 函数部署（见 [[Web 渲染架构选型指南]]、[[Vercel]]）。

## 相关页面

- [[Vercel PaaS 网站上线链路]]
- [[Web 渲染架构选型指南]]
- [[SSG]]
- [[Vercel]]
- [[CI-CD]]
- [[灰度发布与回滚]]
- [[PaaS]]
- [[传统 VPS 与 Vercel 部署对照]]
