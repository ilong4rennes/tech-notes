---
created: 2025-07-07
tags:
  - frontend
  - nextjs
---
## 我啥时候遇到的

在2025暑假实习的时候，写[[20250706-Frontend|Add Model Button]] 这个功能的时候碰到了相关的bug。

我遇到 Next.js Server Actions 的问题，是在做前端上传 `.pt` 模型文件的时候。原本是想用 Server Actions 来上传，但因为 YOLO 的模型文件（比如 `yolov8n.pt` 大概 6.2MB）太大，超过了 Next.js 默认 1MB 的请求体限制，所以上传失败。

```js
const response = await fetch("/api/models", {
  method: "POST",
  body: formData,
});
```

以上代码不行的原因是：
- 因为 /api/models 是 Next.js 自己的 API Route，走的是 Next.js 的服务器（Node.js）。  
- Next.js 对 API Routes 的请求体有默认限制（比如 1MB）。

后来我就不再用 Server Actions，而是改成直接用 `fetch` 把文件上传到 Flask 后端，所以才出现这段修改后的代码：

```js
const response = await fetch("http://localhost:5000/api/models", {   
	method: "POST",   
	body: formData, 
});
```
## 它解决了什么问题

以上讲了为什么Server Actions导致了问题，那当我们使用它的时候解决了什么问题呢？
- Server Actions let you run code on the server directly from your React components, without needing to write a separate API route.

## 我啥时候用得上

- Great for things like saving to a database or handling secrets

## 一句话用法

上传大文件时，别用 Server Actions，直接 fetch 后端 API 比较稳。
但是，Server Actions are a way to handle server logic directly from your UI code.
