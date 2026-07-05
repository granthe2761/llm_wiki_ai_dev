# 维基日志

仅追加记录。条目前缀格式：`## [YYYY-MM-DD] <操作类型> | <摘要>`

---

## [2026-07-05] init | 项目初始化

- 创建目录结构：`raw/`、`wiki/{concepts,entities,sources,synthesis,comparisons,queries}/`
- 创建 schema：`AGENTS.md`、`README.md`
- 初始化导航：`index.md`、`log.md`、`wiki/overview.md`
- 备注：等待首批 raw 资料导入

## [2026-07-05] ingest | 传统网站上线链路

首批 raw 资料入库，建立 Web 部署运维知识域框架。

**新文件**：
- `wiki/sources/传统网站上线链路.md` — 七层遗漏环节与最小/完整链路对比
- `wiki/concepts/生产就绪部署链路.md` — 七层能力模型
- `wiki/concepts/ICP备案.md` — 国内备案硬门槛
- `wiki/concepts/进程守护.md` — systemd/supervisor/pm2
- `wiki/concepts/CI-CD.md` — 自动化发布流水线
- `wiki/concepts/灰度发布与回滚.md` — 放量与回滚策略
- `wiki/concepts/健康检查与探活.md` — 假死检测与自愈
- `wiki/entities/Nginx.md` — 边缘代理多重角色
- `wiki/entities/Cloudflare.md` — DNS/CDN/WAF
- `wiki/entities/Redis.md` — 数据层缓存
- `wiki/entities/Certbot.md` — HTTPS 自动续期
- `wiki/synthesis/传统 VPS 部署完整链路.md` — 跨层综合论点

**更新页面**：
- `wiki/overview.md` — 定义知识域、综合论点、开放问题
- `index.md` — 重建索引

**关键沉淀**：
1. 「能跑」与「能稳定跑」之间隔着完整七层运维栈，非单点组件可补齐
2. 国内部署 ICP 备案与 DNS/WAF 同属访问层硬门槛
3. 裸 VPS 隐性成本 = 容器/K8s 平台默认托管能力的全部自建

**索引变化**：Sources +1 | Entities +4 | Concepts +6 | Syntheses +1 | 更新 ×2

**相关文件**：[[传统 VPS 部署完整链路]]、[[生产就绪部署链路]]、[[Nginx]]、[[ICP备案]]

**备注**：`scripts/` 维护脚本尚未就绪，Python 未安装；索引手动重建，待脚本可用后补跑 health_check。

## [2026-07-05] ingest | Vercel PaaS 网站上线链路

第二份 raw 入库，与 VPS 链路形成跨源对照。

**新文件**：
- `wiki/sources/Paas(Verce)网站上线链路.md` — Vercel Git→Build→Edge 链路与传统逐项对照
- `wiki/entities/Vercel.md` — Frontend Cloud 平台实体
- `wiki/entities/Upstash.md` — Serverless Redis 集成
- `wiki/concepts/PaaS.md` — 平台托管边界
- `wiki/concepts/Serverless Functions.md` — 无状态函数运行时
- `wiki/concepts/Edge Functions.md` — Edge V8 Isolate
- `wiki/concepts/Git 原生部署.md` — Push 即 CI/CD
- `wiki/comparisons/传统 VPS 与 Vercel 部署对照.md` — 跨两源能力矩阵

**更新页面**：
- `wiki/synthesis/传统 VPS 部署完整链路.md` — 补充 PaaS 分野与观点演进
- `wiki/concepts/生产就绪部署链路.md` — PaaS 托管对照
- `wiki/concepts/CI-CD.md`、`灰度发布与回滚.md`、`ICP备案.md` — Vercel 交叉引用
- `wiki/entities/Nginx.md`、`Redis.md`、`Certbot.md` — PaaS 替代方案
- `wiki/overview.md`、`index.md`

**关键沉淀**：
1. Vercel 托管 infra/运行时/网络/发布四层，数据/合规/深度运维外置第三方
2. Next.js 等 JS 全栈 → Vercel；Java Spring Boot/RuoYi → VPS/Docker/K8s
3. ICP 备案是 Vercel 无法解决的硬分叉，国内正式业务仍需 VPS/国内云

**索引变化**：Sources +1 | Entities +2 | Concepts +4 | Comparisons +1 | 更新 ×9

**相关文件**：[[传统 VPS 与 Vercel 部署对照]]、[[Vercel]]、[[PaaS]]、[[Git 原生部署]]
