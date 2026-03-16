---
tags:
  - frontend
---
## 1. fetch

浏览器自带的原生功能，用来向后端发请求、获取数据。不需要安装任何东西。

**一句话：** 前端跟后端说话的基础工具。

**使用案例：** 用户点击按钮，获取一条天气数据

```js
const res = await fetch('https://api.weather.com/today');
const data = await res.json();
console.log(data); // { city: 'Pittsburgh', temp: 15 }
```

⚠️ **缺点：** 每次都要手动处理 token、headers、报错，代码重复。

## 2. axios

第三方库，干的事和 fetch 一样，但更方便。自动帮你处理 JSON、报错更清晰、可以设置全局配置。

**一句话：** 更好用的 fetch，加了很多实用功能。

**使用案例：** 统一配置 baseURL 和 token，所有请求自动带上

```js
const api = axios.create({
  baseURL: 'https://myapp.com/api',
  headers: { Authorization: 'Bearer ' + token }
});

const { data } = await api.get('/users'); // 自动解析 JSON
```

✅ **优点：** 配置一次，全局生效。比 fetch 少写很多重复代码。

## 3. React Query

不只是发请求，还帮你管理从后端拿来的数据的状态。比如：这个数据加载中吗？出错了吗？多久刷新一次？

**一句话：** 帮你管理服务器数据的管家，解决 loading / error / 缓存问题。

**使用案例：** 获取用户列表，自动处理加载状态和缓存

```js
const { data, isLoading, isError } = useQuery({
  queryKey: ['users'],
  queryFn: () => apiFetch('/api/users')
});

if (isLoading) return <p>加载中...</p>;
if (isError) return <p>出错了！</p>;
```

✅ **优点：** 自动缓存、自动重试、自动刷新。适合中大型项目。

## 4. SWR

Vercel（Next.js 的公司）出的轻量版 React Query。功能类似，但更简单，包体积更小。名字来自缓存策略 stale-while-revalidate（先用旧数据，后台悄悄更新）。

**一句话：** 轻量版 React Query，Next.js 项目常用。

**使用案例：** 先展示旧数据，后台悄悄刷新

```js
const { data, error } = useSWR('/api/users', apiFetch);
// 页面先显示上次缓存的数据，同时后台自动更新
```

✅ **优点：** 极简 API，上手快。  
⚠️ **缺点：** 功能没 React Query 全。

## 总结对比

| 技术          | 类型      | 核心作用            | 需要安装                              |
| ----------- | ------- | --------------- | --------------------------------- |
| fetch       | 浏览器原生   | 发 HTTP 请求       | 不需要                               |
| axios       | 第三方库    | 更好用的 fetch      | npm install axios                 |
| React Query | 状态+请求管理 | 管理服务器数据         | npm install @tanstack/react-query |
| SWR         | 状态+请求管理 | 轻量版 React Query | npm install swr                   |

## 典型组合方案

- **小项目：** fetch 或 axios 够了
- **中大型项目：** axios + React Query（最主流）
- **Next.js 项目：** fetch + SWR 或 React Query