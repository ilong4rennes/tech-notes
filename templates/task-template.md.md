---
who: 
problem: 
plan: 
need: 
tags: 
aliases:
---
<%*
const taskName = await tp.system.prompt("任务名称（英文/拼音均可）")
const today = tp.date.now("YYYYMMDD")
const title = `${today}-${taskName}`
await tp.file.rename(title)
%>

# <%* tR = title %>

## 谁 assign 给你这个 task？
上级 / 老师 / 自发：

## 需要解决什么问题
（写清楚要解决的问题是什么）

## 怎么做？
（写你现在目前的 plan / 获取的信息，这一块可以非常零散）

## 需要什么？
（是要一个 high level plan 还是要实现？需要讨论还是直接执行？）

