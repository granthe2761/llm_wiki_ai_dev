以下是两篇内容的**系统性整合**，既保留每个模式的完整技术细节，也标注了它们的**行业通用性**和**跨框架对应关系**。

---

## 总览：六种渲染模式一览

| 缩写 | 全称 | 行业通用性 | HTML 生成时机 | 是否需要 JS 执行才能看到内容 | SEO 友好度 |
|------|------|-----------|-------------|---------------------------|-----------|
| **SSG** | **Static Site Generation**<br>静态站点生成 | ⭐⭐⭐ 极高 | 构建时 | ❌ 不需要 | ⭐⭐⭐ 极佳 |
| **SSR** | **Server-Side Rendering**<br>服务端渲染 | ⭐⭐⭐ 极高 | 请求时 | ❌ 不需要 | ⭐⭐⭐ 极佳 |
| **Streaming SSR** | **Streaming Server-Side Rendering**<br>流式服务端渲染 | ⭐⭐⭐ 高 | 请求时（分块流式） | 部分需要 | ⭐⭐⭐ 极佳 |
| **ISR** | **Incremental Static Regeneration**<br>增量静态再生 | ⭐⭐ 术语特有，思想通用 | 构建时 + 后台定时 | ❌ 不需要 | ⭐⭐⭐ 极佳 |
| **RSC** | **React Server Components**<br>React 服务端组件 | ⭐⭐ React 特有，思想被借鉴 | 请求时（服务端） | ❌ 不需要 | ⭐⭐⭐ 极佳 |
| **RCC** | **React Client Components**<br>React 客户端组件 | ⭐ React 术语 | 浏览器运行时 | ✅ 必须执行 JS | ⭐ 较差 |

---

## 一、全行业通用的基础模式

这些模式**早于 Next.js 甚至 React 存在**，是所有 Web 框架的共通语言。

### 1. SSG — Static Site Generation（静态站点生成）

**定义**：在**构建阶段**（Build Time）将所有页面的 HTML 完全生成好，部署到 CDN 或任意 Web 服务器。用户请求时直接返回预先生成的文件，**不执行任何数据查询**。

**工作原理**：
```
构建时：
  读取数据源（Markdown/数据库/API）
  → 生成完整 HTML（含数据）
  → 输出到 .html 文件
用户访问：
  → CDN/服务器直接返回 HTML
```

**Next.js 实现**：
```tsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map((post) => ({ slug: post.slug }));
}

export default async function Page({ params }) {
  const post = await getPost(params.slug);
  return <article>{post.content}</article>;
}
```

**跨框架对应**：
- **Jekyll**（Ruby）、**Hugo**（Go）、**Gatsby**（React）、**Eleventy**、**Astro**、**Nuxt**（`nuxt generate`）
- 这是**GitHub Pages 的基石**，也是 Web 的原始形态

**对搜索效率的影响**：爬虫直接抓取完整 HTML，无需等待 JS，TTFB 极低，**索引效率最高**。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 首屏速度最快 | 内容更新后需重新构建部署 |
| 服务器压力为零 | 无法展示实时/个性化数据 |
| SEO 最完美 | 页面数量巨大时构建时间变长 |

**适用场景**：Portfolio 的 About 页、Projects 列表、博客文章详情页。

---

### 2. SSR — Server-Side Rendering（服务端渲染）

**定义**：在**用户请求到达时**，服务器实时执行数据查询和组件渲染，生成完整 HTML 后返回。每次请求都是"新鲜"的 HTML。

**工作原理**：
```
用户请求 /dashboard
  → 服务器收到请求
  → 查询数据库/API（实时数据）
  → 在服务端渲染为 HTML
  → 返回 HTML + 客户端 JS（用于 hydration 交互）
```

**Next.js 实现**：
```tsx
export const dynamic = 'force-dynamic'; // 强制 SSR

export default async function DashboardPage() {
  const stats = await fetch('https://api.example.com/stats', { cache: 'no-store' });
  return <Dashboard data={await stats.json()} />;
}
```

**跨框架对应**：
- **PHP**（1995 年诞生时就是 SSR）、**Ruby on Rails**、**Django**、**ASP.NET**、**Spring MVC**
- **Vue** 的 `renderToString`、**Angular Universal**、**SvelteKit**（`+page.server.js`）

**对搜索效率的影响**：爬虫收到完整 HTML，SEO 效果好；但 TTFB 比 SSG 慢（需实时查询），高并发时可能影响爬虫抓取速度。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 数据永远最新 | TTFB 比 SSG 慢 |
| 支持个性化内容 | 服务器持续运行，有计算成本 |
| SEO 友好 | 高并发时服务器压力大 |

**适用场景**：需要实时数据的页面（如"最新动态"、用户登录后的仪表盘）。

---

### 3. Streaming SSR — Streaming Server-Side Rendering（流式服务端渲染）

**定义**：服务端不需要等所有数据查询完成再一次性返回 HTML，而是**先返回页面框架，再逐步流式传输剩余内容**。这是 HTTP 协议**分块传输编码**（Chunked Transfer Encoding）的应用。

**工作原理**：
```
传统 SSR：
  等数据A → 等数据B → 等数据C → 一次性返回完整 HTML（慢）

Streaming SSR：
  立即返回 <html><head>...</head><body><header>...</header>
  → 流式传输 <main><Suspense fallback={<Skeleton />} />...</main>
  → 最后传输 <footer>...</footer></body></html>
```

**Next.js 实现**：
```tsx
import { Suspense } from 'react';
import { ProjectList, ProjectListSkeleton } from '@/components/ProjectList';

export default function Page() {
  return (
    <main>
      <h1>我的作品</h1>
      <Suspense fallback={<ProjectListSkeleton />}>
        <ProjectList /> {/* 异步数据流式到达 */}
      </Suspense>
    </main>
  );
}

async function ProjectList() {
  const projects = await fetch('https://api.example.com/projects');
  return <div>{/* 渲染 */}</div>;
}
```

**跨框架对应**：
- **Django** 的 `StreamingHttpResponse`
- **Node.js** 原生 `res.write()` 流式写入
- **Vue 3** 的 `pipeable` 流式渲染
- **SvelteKit** 的 `stream` 响应

**对搜索效率的影响**：爬虫可立即开始解析页面结构；配合骨架屏，改善了**关键内容的到达时间**。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 首字节时间（TTFB）极快 | 实现复杂度稍高 |
| 用户感知加载更快 | 需合理设计 Suspense 边界 |
| 慢查询不阻塞整体页面 | 部分旧爬虫可能不等待流式内容（Google 已支持） |

**适用场景**：页面部分内容依赖慢 API（如第三方 GitHub 统计），但骨架框架可立即展示。

---

## 二、React / Next.js 生态的演进模式

这些术语由 React 或 Vercel 提出，但核心思想正在被其他框架借鉴。

### 4. ISR — Incremental Static Regeneration（增量静态再生）

**通用性说明**："ISR"这个词是 **Vercel 为 Next.js 创造的营销术语**，但背后的 **Stale-While-Revalidate** 思想是 CDN 行业的通用策略（RFC 5861）。

**定义**：**先按 SSG 方式生成静态页面并缓存，用户访问时直接返回缓存版本；后台按设定时间自动重新生成**，实现"静态速度 + 动态更新"。

**工作原理**：
```
构建时：生成 HTML 并缓存（同 SSG）
用户访问：返回缓存 HTML（极快）
后台：到达 revalidate 时间后，下次访问触发重新生成
```

**Next.js 实现**：
```tsx
export const revalidate = 3600; // 1小时后后台自动重新生成

export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map((p) => ({ slug: p.slug }));
}
```

**跨框架对应**：
- **Nuxt**：`swr` / `isr` 模式
- **SvelteKit**：`invalidate` + CDN 重新验证
- **Astro**：按需渲染（On-demand Rendering）
- **CDN 层**：Cloudflare / Fastly 原生支持 `stale-while-revalidate`

**对搜索效率的影响**：爬虫抓取缓存的静态 HTML，速度同 SSG；内容更新后最多延迟 `revalidate` 时间。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 首屏速度 = SSG | 更新有延迟（非实时） |
| 无需手动重新部署 | 首次更新请求可能拿到旧数据 |
| 自动后台更新 | 配置不当可能导致缓存不一致 |

**适用场景**：博客列表页、作品展示页——内容会更新，但无需秒级实时。

---

### 5. RSC — React Server Components（React 服务端组件）

**通用性说明**：这是 **React 团队**（非 Next.js）提出的架构，Next.js App Router 是第一个生产级实现。其他框架没有"Server Component"这个精确术语，但有**类似思想**。

**定义**：App Router 的默认组件模式。组件在服务端执行，可直接访问数据库/文件系统，生成 HTML 后发送到客户端，但**不会打包到客户端 JS Bundle 中**。

**工作原理**：
```
服务端：
  → 执行 RSC（查询数据库、读取文件）
  → 生成 HTML + RSC Payload（序列化组件树）
  → 发送到浏览器
客户端：
  → 接收 HTML 直接渲染
  → 无需下载 RSC 的 JS 代码
```

**Next.js 实现**：
```tsx
// app/page.tsx —— 默认就是 RSC，无需标记
import { db } from '@/lib/db';

export default async function HomePage() {
  const projects = await db.project.findMany(); // 服务端执行，浏览器不可见
  return <main>{projects.map(p => <ProjectCard key={p.id} project={p} />)}</main>;
}
```

**跨框架对应**：
- **Astro**："Server Islands"（服务端渲染的岛屿）
- **Fresh**（Deno）：Islands Architecture
- **Waku**：完全基于 RSC 的 React 轻量框架

**对搜索效率的影响**：极致 SEO，HTML 在服务端完全生成，且客户端 JS Bundle 更小，页面交互更快。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 零客户端 Bundle 体积增加 | 不能直接使用浏览器 API（window/localStorage） |
| 直接访问后端资源（数据库、密钥） | 需要理解服务端/客户端边界 |
| 天然支持 Streaming | 学习曲线稍陡 |

**适用场景**：App Router 下**所有数据展示型组件**的默认选择。Portfolio 中 90% 的页面都应该是 RSC。

---

### 6. RCC — React Client Components（React 客户端组件）

**通用性说明**：这是 RSC 架构下的**相对概念**。其他框架（Vue、Svelte）的组件默认"同构"（既能在服务端也能在客户端运行），不严格区分 Server/Client，因此没有 RCC 这个精确术语。

**定义**：在浏览器中执行的组件，需在文件顶部标记 `'use client'`。拥有完整浏览器 API 访问能力，但**必须下载并执行 JS 后才能渲染内容**。

**工作原理**：
```
服务端：仅渲染占位符（或空 div）
浏览器：下载 JS Bundle → 执行 React → 渲染组件 → 可能再发起 API 请求
```

**Next.js 实现**：
```tsx
'use client';

import { useState } from 'react';

export default function LikeButton() {
  const [likes, setLikes] = useState(0);
  return <button onClick={() => setLikes(l => l + 1)}>❤️ {likes}</button>;
}
```

**跨框架对应**：
- **Vue**：无此严格区分，`.vue` 文件默认同构
- **Svelte**：组件默认同构
- **Astro**：通过 `client:*` 指令标记客户端交互岛屿

**对搜索效率的影响**：如果主内容放在 RCC 里，爬虫可能只能看到空壳；首屏依赖 JS，禁用 JS 则内容不可见。

| ✅ 优点 | ❌ 缺点 |
|--------|---------|
| 完整浏览器 API 支持 | SEO 差 |
| 支持交互状态（useState/useEffect） | 增加客户端 JS 体积 |
| 适合用户交互 | 需要 hydration，首屏有延迟 |

**适用场景**：仅用于**交互元素**：搜索框、按钮、轮播图、模态框、评论表单。Portfolio 的**主体内容绝不应放在 RCC 中**。

---

## 三、跨框架实现对照表

| 模式 | Next.js | Nuxt（Vue） | SvelteKit | Astro | Rails/Django |
|------|---------|-----------|-----------|-------|-------------|
| **SSG** | `generateStaticParams` | `nuxt generate` | `prerender` | 默认就是 SSG | 不适用 |
| **SSR** | `dynamic = 'force-dynamic'` | `ssr: true` | `+page.server.js` | `server islands` | 原生默认 |
| **ISR** | `revalidate` | `swr` / `isr` | `invalidate` + CDN | 按需渲染 | 需配合 CDN |
| **Streaming** | `loading.tsx` + Suspense | `Suspense` + 流式 | `stream` 响应 | 渐进式渲染 | ActionCable/Channels |
| **RSC** | App Router 默认 | 无（Vue 不区分） | 无 | Server Islands | 不适用 |
| **RCC** | `'use client'` | 默认同构 | 默认同构 | `client:*` 指令 | 不适用 |

---

## 四、Portfolio 选型建议（整合版）

| 页面类型 | 推荐模式 | 通用思想 | 理由 |
|---------|---------|---------|------|
| 首页 / About | **RSC + SSG** | 服务端直出 + 静态生成 | 数据固定，直接输出 HTML，任何框架都适用 |
| 博客列表 | **RSC + ISR** | 静态缓存 + 后台刷新 | 有新文章时自动更新，无需手动部署 |
| 博客详情 | **RSC + SSG** | 纯静态生成 | 每篇文章构建为独立 HTML，爬虫直达 |
| 联系表单 | **RSC 外壳 + RCC 表单** | 服务端框架 + 客户端交互 | 页面框架静态，表单交互用客户端组件 |
| 实时访客数 | **RSC + Streaming** | 流式分块渲染 | 主体静态，数字部分流式加载，不阻塞首屏 |

---

## 五、一句话总结

> **SSG、SSR、Streaming 是 Web 的通用架构，比 Next.js 早存在几十年；ISR 是 Vercel 包装术语，但 Stale-While-Revalidate 是 CDN 通用策略；RSC/RCC 是 React 的特定实现，但"服务端渲染组件"的思想正被 Astro、Fresh 等框架借鉴。**

如果你从 Next.js 转向 **Nuxt**、**SvelteKit** 或 **Astro**，这些概念**完全平移**，只是 API 名字不同。