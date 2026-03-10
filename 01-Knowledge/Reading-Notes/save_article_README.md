# 📚 Save Article — 文章保存到 Obsidian

自动抓取网页文章 → AI 总结 → 保存到 Obsidian vault。

---

## 前置要求

- **Anthropic API Key**：[console.anthropic.com/settings/keys](https://console.anthropic.com/settings/keys)
- 设置环境变量（只需做一次）：

```bash
echo 'export ANTHROPIC_API_KEY="sk-ant-你的key"' >> ~/.zshrc
source ~/.zshrc
```

- 设置快捷命令（可选，只需做一次）：

```bash
echo 'alias save="python3 ~/save_article.py"' >> ~/.zshrc
source ~/.zshrc
```

---

## 基本用法

```bash
# 直接抓取（适用于大多数网站）
save <URL>

# 从剪贴板读取（适用于豆瓣、微博等需要登录的网站）
save <URL> --clipboard

# 保存到指定子文件夹
save <URL> --folder Computer-Science
```

---

## 各网站操作方法

### ✅ 自动抓取（直接用）

```bash
save 'https://mp.weixin.qq.com/s/xxxxx'         # 微信公众号
save 'https://zhuanlan.zhihu.com/p/xxxxx'        # 知乎专栏
save 'https://sspai.com/post/xxxxx'              # 少数派
save 'https://medium.com/@xxx/xxxxx'             # Medium
```

### ⚠️ 需要手动复制（--clipboard 模式）

适用于：**豆瓣、微博、付费内容、需要登录的网站**

1. 在浏览器中打开文章
2. `Cmd+A` 全选 → `Cmd+C` 复制
3. 运行命令：

```bash
save 'https://www.douban.com/group/topic/xxxxx' --clipboard
```

---

## 保存位置

所有文章保存在：

```
~/Documents/notes/tech-notes/01-Knowledge/Reading-Notes/
```

文件名格式：`2026-03-09-文章标题.md`

### 指定其他子文件夹

```bash
save <URL> --folder Computer-Science
save <URL> --folder Philosophy-and-Psychology
save <URL> --folder Information-Systems
```

---

## 生成的笔记格式

```markdown
---
title: "文章标题"
source: "https://..."
date: 2026-03-09
tags: [reading-note]
---

# 文章标题

> 原文：https://...

## AI 总结

**一句话概括**：...

**主要内容**：
- 要点一
- 要点二

**关键概念**：...

**我的思考**：...

---

## 原文内容

（完整原文）
```

---

## 常见问题

| 错误 | 原因 | 解决 |
|------|------|------|
| `403 Forbidden` | 网站屏蔽爬虫 | 改用 `--clipboard` 模式 |
| `credit balance is too low` | API 余额不足 | 去 console.anthropic.com 充值 |
| `ANTHROPIC_API_KEY` 未设置 | 没配置环境变量 | 见上方「前置要求」 |
| 剪贴板为空 | 忘记复制文章 | 先复制再运行 |
