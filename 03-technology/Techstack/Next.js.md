**概要**  
Next.js 是一个基于 React 的全栈框架，内置对服务器渲染（SSR）、静态站点生成（SSG）和 API 路由的支持。本文教程分为以下七大部分：

1. 前言：介绍 Next.js 的核心特性
2. 创建项目：使用 `create-next-app` 快速搭建
3. 项目介绍：默认目录结构与路由机制
4. 发布和部署：如何把应用上线到生产环境
5. 大肆改造一：修改界面（样式与组件）
6. 大肆改造二：增加 API（后端路由与数据接口）
7. 大肆改造三：使用数据库（持久化与 ORM）

---

## 1. 前言

Next.js 在 2022 年已成为最受欢迎的服务端框架之一，主要因为它：

- **支持 SSR（Server-Side Rendering）**：在服务器端渲染页面 HTML，提升首屏加载速度和 SEO 效果 ([Juejin](https://juejin.cn/post/7203180600818581563?utm_source=chatgpt.com "Nextjs全栈详细开发教程，完整版 - 稀土掘金"))
- **支持 SSG（Static Site Generation）**：构建时预先生成静态页面，通过 CDN 分发，极大提升性能 ([Juejin](https://juejin.cn/post/7203180600818581563?utm_source=chatgpt.com "Nextjs全栈详细开发教程，完整版 - 稀土掘金"))
- **内置文件系统路由**：无需额外配置，页面文件即路由入口 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))
- **混合渲染模式**：可针对页面灵活选择 SSR、SSG、CSR 等 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs?utm_source=chatgpt.com "Next.js Documentation"))

这些特性让前端和后端在一个项目中无缝协作，简化开发流程。

---

## 2. 创建项目

使用 Next.js 官方脚手架 `create-next-app`，可一键生成包含 TypeScript、ESLint、Tailwind、App Router 等配置的项目骨架。

```bash
npx create-next-app@latest my-next-app
# 依提示选择：TypeScript、ESLint、Tailwind CSS、App Router（推荐）、Turbopack 等
cd my-next-app
npm run dev
```

- **命令行交互**：会询问是否使用 TypeScript、是否在 `src/` 目录下、是否使用 Turbopack 等 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))
- **手动安装**（可选）：
    ```bash
    npm install next@latest react@latest react-dom@latest
    # package.json 添加脚本
    "scripts": {
      "dev": "next dev",
      "build": "next build",
      "start": "next start",
      "lint": "next lint"
    }
    ```

([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))

---

## 3. 项目介绍

Next.js 默认目录结构（App Router 模式）：

```
my-next-app/
├─ app/
│  ├─ layout.tsx      ← 根布局，包含 <html> 和 <body> 标签 :contentReference[oaicite:6]{index=6}  
│  ├─ page.tsx        ← 默认首页内容  
│  └─ api/            ← （可选）App Router 下的 Route Handlers
├─ public/            ← 静态资源，如图片、字体  
├─ styles/            ← 全局或模块化 CSS 文件  
├─ next.config.js     ← Next.js 配置  
└─ package.json
```

- **文件系统路由**：文件和目录名即路由，`app/about/page.tsx` 会映射到 `/about` ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))
- **Layout 嵌套**：可在各级目录下添加 `layout.tsx`，实现共享布局和嵌套路由 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))

---

## 4. 发布和部署

### 4.1 部署到 Vercel （推荐）

Vercel 是 Next.js 的原生平台，一键部署、零配置即可上线：

1. 在 [Vercel 官网] 创建账号，并授权 GitHub 仓库 ([Next.js by Vercel - The React Framework](https://nextjs.org/learn/pages-router/deploying-nextjs-app-deploy?utm_source=chatgpt.com "Pages Router: Deploy to Vercel - Next.js"))
2. 选择项目，使用默认构建命令 `npm run build`、默认输出目录 `.next`
3. 部署完成后，获取唯一的预览和生产环境 URL

### 4.2 自托管方式

Next.js 可通过 Node.js 服务器、Docker 容器或静态导出部署 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/deploying?utm_source=chatgpt.com "Building Your Application: Deploying - Next.js"))

- **Node.js 服务器**
    ```bash
    npm run build
    npm run start   # 启动生产服务器
    ```

- **Docker 容器** 可编写 `Dockerfile`，并部署到 Kubernetes、AWS ECS 等 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/deploying?utm_source=chatgpt.com "Building Your Application: Deploying - Next.js"))
- **静态导出**：`next export`，仅适用于不需要 SSR/动态 API 的情况 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/deploying?utm_source=chatgpt.com "Building Your Application: Deploying - Next.js"))

---

## 5. 大肆改造一：修改界面

### 5.1 CSS Modules

Next.js 原生支持 CSS Modules，将 `.module.css` 文件局部作用域化：

```css
/* components/Button.module.css */
.btn { padding: 8px 16px; background: blue; color: white; }
```

```tsx
import styles from './Button.module.css';

export function Button({ children }) {
  return <button className={styles.btn}>{children}</button>;
}
```

- **优点**：自动生成唯一类名，避免冲突 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/13/app/building-your-application/styling/css-modules?utm_source=chatgpt.com "Styling: CSS Modules - Next.js"))
- **生产环境优化**：按需拆分、压缩，保证最低 CSS 体积 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/13/app/building-your-application/styling/css-modules?utm_source=chatgpt.com "Styling: CSS Modules - Next.js"))

### 5.2 全局样式 & Tailwind

- 全局 CSS：在 `app/globals.css` 中定义，并在根布局中导入
- Tailwind：CLI 选择 Tailwind 后自动完成配置，或手动安装 `tailwindcss` 并在 `tailwind.config.js` 中启用

---

## 6. 大肆改造二：增加 API

Next.js 内置 API 路由（Pages Router）或 Route Handlers（App Router），无需独立后端服务。

### 6.1 Pages Router 下的 API Routes

在 `pages/api/hello.ts` 中：

```ts
import type { NextApiRequest, NextApiResponse } from 'next';

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  res.status(200).json({ message: 'Hello from Next.js!' });
}
```

- 文件即端点：`pages/api/hello.ts` → `/api/hello` ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/routing/api-routes?utm_source=chatgpt.com "API Routes - Next.js"))
- 支持动态路由、捕获所有路由、请求体解析等 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/routing/api-routes?utm_source=chatgpt.com "API Routes - Next.js"))

### 6.2 App Router 下的 Route Handlers

在 `app/api/hello/route.ts` 中：

```ts
export async function GET() {
  return new Response(JSON.stringify({ message: 'Hello from App Router!' }), {
    headers: { 'Content-Type': 'application/json' },
  });
}
```

- 更贴近 Web Fetch API，支持 Edge Runtime ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/building-your-application/routing/route-handlers?utm_source=chatgpt.com "Route Handlers - Next.js"))

---

## 7. 大肆改造三：使用数据库

结合 ORM（如 Prisma）即可轻松接入数据库。以下以 PostgreSQL + Prisma 为例：

### 7.1 安装与初始化

```bash
npm install prisma @prisma/client
npx prisma init   # 生成 prisma/schema.prisma
```

- 自动生成 `.env` 和基础配置 ([Prisma](https://www.prisma.io/docs/guides/nextjs?utm_source=chatgpt.com "How to use Prisma ORM with Next.js"))

### 7.2 定义数据模型 & 迁移

在 `prisma/schema.prisma` 中：

```prisma
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String?
  published Boolean  @default(false)
  createdAt DateTime @default(now())
}
```

```bash
npx prisma migrate dev --name init
```

- 生成数据库表并更新 Prisma Client ([Prisma](https://www.prisma.io/docs/guides/nextjs?utm_source=chatgpt.com "How to use Prisma ORM with Next.js"))

### 7.3 在 API 中使用 Prisma

```ts
// pages/api/posts.ts
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

export default async function handler(req, res) {
  if (req.method === 'GET') {
    const posts = await prisma.post.findMany();
    res.status(200).json(posts);
  } else if (req.method === 'POST') {
    const { title, content } = req.body;
    const post = await prisma.post.create({ data: { title, content } });
    res.status(201).json(post);
  }
}
```

- 推荐开发时使用全局单例实例，避免热重载时重复实例化 ([Prisma](https://www.prisma.io/docs/orm/more/help-and-troubleshooting/nextjs-help?utm_source=chatgpt.com "Comprehensive Guide to Using Prisma ORM with Next.js"))
- 可在 App Router 的 Route Handlers 中同样使用 ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/building-your-application/routing/route-handlers?utm_source=chatgpt.com "Route Handlers - Next.js"))

---

通过以上七个部分的详细讲解，你已经掌握了从初始化项目、理解目录结构，到部署上线、页面样式、API 路由和数据库持久化的 Next.js 全栈开发要点。希望本指南能帮助你快速上手并深入使用 Next.js 来构建高性能的 Web 应用。

---

**参考文档**

- Create Next App CLI · Next.js ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/api-reference/cli/create-next-app?utm_source=chatgpt.com "CLI: create-next-app - Next.js"))
    
- Getting Started: Installation · Next.js ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/getting-started/installation?utm_source=chatgpt.com "Getting Started: Installation | Next.js"))
    
- Deploy to Vercel · Next.js Learn ([Next.js by Vercel - The React Framework](https://nextjs.org/learn/pages-router/deploying-nextjs-app-deploy?utm_source=chatgpt.com "Pages Router: Deploy to Vercel - Next.js"))
    
- How to deploy your Next.js application · Next.js Docs ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/deploying?utm_source=chatgpt.com "Building Your Application: Deploying - Next.js"))
    
- CSS Modules · Next.js ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/13/app/building-your-application/styling/css-modules?utm_source=chatgpt.com "Styling: CSS Modules - Next.js"))
    
- API Routes · Next.js ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/pages/building-your-application/routing/api-routes?utm_source=chatgpt.com "API Routes - Next.js"))
    
- Route Handlers (App Router) · Next.js ([Next.js by Vercel - The React Framework](https://nextjs.org/docs/app/building-your-application/routing/route-handlers?utm_source=chatgpt.com "Route Handlers - Next.js"))
    
- How to use Prisma ORM with Next.js · Prisma Docs ([Prisma](https://www.prisma.io/docs/guides/nextjs?utm_source=chatgpt.com "How to use Prisma ORM with Next.js"))
    
- How to Build a Fullstack App with Next.js, Prisma, and Postgres · Vercel Guides ([vercel.com](https://vercel.com/guides/nextjs-prisma-postgres?utm_source=chatgpt.com "How to Build a Fullstack App with Next.js, Prisma, and Postgres"))