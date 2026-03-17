---
tags: [index]
---

## Contents

- [Frontend](#frontend)
	- [JavaScript](#javascript)
	- [React](#react)
	- [Next.js & Other](#nextjs-other)
- [Backend](#backend)
	- [Django](#django)
	- [Other Backend](#other-backend)
- [Python & Data](#python-data)
- [DevOps & Cloud](#devops-cloud)
- [Interview Prep](#interview-prep)
	- [DSA](#dsa)
	- [General](#general)
- [Tools](#tools)

---

## Frontend

### JavaScript

| Name                     | Link                                              | Summary                                                          |
| ------------------------ | ------------------------------------------------- | ---------------------------------------------------------------- |
| 事件循环机制 Event Loop        | [[01 事件循环机制 Event Loop]]                          | JS runtime model; call stack, task queue, microtask queue        |
| 宏任务与微任务                  | [[02 宏任务与微任务]]                                    | Macro vs micro tasks; execution order                            |
| Promise                  | [[03 Promise]]                                    | Promise API; chaining; error handling                            |
| async / await            | [[04 async await]]                                | Syntax sugar over Promise; async control flow                    |
| 几种异步方式的比较                | [[05 几种异步方式的比较]]                                  | Comparison of callback, Promise, async/await                     |
| 闭包 Closure               | [[06 闭包 closure]]                                 | Lexical scoping; closure use cases and gotchas                   |
| JS Object                | [[07 JS 数据类型]]                                  | Object model; prototype chain; property descriptors              |

---

### React

| Name              | Link                         | Summary                                                                  |
| ----------------- | ---------------------------- | ------------------------------------------------------------------------ |
| React Guide (All) | [[00 React Guide (All)]]     | ES6+, JSX, Virtual DOM, component architecture, lifecycle, hooks         |
| React Basics      | [[01 React Basics]]          | JSX syntax and rules; component fundamentals                             |
| React Hooks       | [[02 React Hooks]]           | useState, useEffect, and state management patterns                       |
| Memoization       | [[03 Memoization]]           | useMemo, useCallback; performance optimization                           |
| Refs              | [[04 Refs]]                  | useRef; accessing DOM elements; persisting values across renders         |
| Error Handling    | [[05 Error Handling]]        | Error boundaries; try/catch in async handlers                            |
| Routing           | [[06 Routing]]               | React Router; SPA vs multi-page routing                                  |
| State Management  | [[07 State Management]]      | Component state, global state, Redux patterns                            |
| Data Fetching     | [[08 Data Fetching]]         | React Query; useQuery for fetching, caching, syncing server data         |

---

### Next.js & Other

| Name                 | Link                                              | Summary                                                                       |
| -------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Next.js              | [[Next.js]]                                       | SSR, SSG, file-system routing, full-stack setup, DB integration               |
| Next.js Server Actions | [[Next.js Server Actions]]                      | File upload size limits; request config for large files                       |
| 前后端通信                | [[前后端通信 fetch｜axios｜React Query｜SWR]]             | fetch, axios, React Query, SWR — comparison and usage patterns                |
| React Global State   | [[React Global State]]                            | Global state patterns across Context, Zustand, Redux                          |

---

## Backend

### Django

| Name               | Link                        | Summary                                                              |
| ------------------ | --------------------------- | -------------------------------------------------------------------- |
| Setup              | [[00 Set up]]               | Prerequisites (Python, Homebrew, PostgreSQL), project init           |
| Django Introduction | [[01 Django Introduction]] | Venv, project creation, app installation                             |
| Connecting to a Database | [[02 Connecting to a Database]] | MySQL setup, client config, connection procedures           |

---

### Other Backend

| Name          | Link                    | Summary                                                                    |
| ------------- | ----------------------- | -------------------------------------------------------------------------- |
| Node.js       | [[Node.js]]             | Globals, modules, file system, client/server, deployment basics            |
| Flask Blueprint | [[Blueprint（Flask）]]  | Modularizing Flask routes with Blueprints                                  |
| Prisma ORM    | [[Prisma ORM]]          | Type-safe database client; schema definition; queries and migrations        |
| Redis         | [[Redis]]               | In-memory key-value store; caching, queues (lpush/rpop FIFO)               |
| Bull Message Queue | [[Bull Message Queue]] | Job queue for Node.js backed by Redis; producer/consumer pattern      |
| View Engine   | [[View Engine]]         | EJS, Pug, Handlebars, Benchpress in Express.js                             |

---

## Python & Data

| Name   | Link       | Summary                                                                |
| ------ | ---------- | ---------------------------------------------------------------------- |
| NumPy  | [[Numpy]]  | Array creation, I/O, data types, math operations                       |
| PyTest | [[PyTest]] | Unit testing for Flask; test client, request simulation, edge cases    |
| boto3  | [[boto3]]  | AWS SDK for Python; S3 connections, endpoint config, credentials       |

---

## DevOps & Cloud

| Name       | Link           | Summary                                                               |
| ---------- | -------------- | --------------------------------------------------------------------- |
| Docker     | [[Docker]]     | Containerization fundamentals; images, containers, compose            |
| LocalStack | [[LocalStack]] | Local AWS emulator for testing S3, SQS without prod access            |

---

## Interview Prep

### DSA

| Name              | Link                 | Summary                                                                          |
| ----------------- | -------------------- | -------------------------------------------------------------------------------- |
| 题型总结              | [[题型总结]]             | Tree structures; Binary Tree, Tree, Graph relationships; traversal               |
| Backtracking      | [[Backtracking]]     | Recursive exploration with pruning; template patterns                            |
| Graph             | [[Graph]]            | Adjacency matrix/list representations; Python implementation                     |
| Linked List       | [[Linked List]]      | Linked list ops; reverse linked list algorithm                                   |
| Search Algorithms | [[Search Algorithms]] | Linear search and binary search with implementations                            |
| Sort Algorithms   | [[Sort Algorithms]]  | Bubble sort and others; time complexity analysis                                  |

---

### General

| Name       | Link                  | Summary                                                               |
| ---------- | --------------------- | --------------------------------------------------------------------- |
| SWE面试      | [[SWE]]               | Frontend, backend, API design, DB, auth interview topics              |
| System Design | [[System Design]]  | System design interview prep                                          |
| 前端八股       | [[前端八股]]              | Frontend fundamentals: browser, CSS, JS, React interview questions    |
| 后端八股       | [[后端八股]]              | Backend fundamentals: DB, caching, concurrency interview questions    |
| 计算机网络八股    | [[计算机网络八股]]           | Networking fundamentals; TCP vs UDP; HTTP; DNS                        |
| 项目与经历介绍    | [[项目与经历介绍]]           | How to present projects and experience in interviews                  |
| Behavioral Questions | [[Behavioral Questions]] | STAR method; common behavioral question bank                   |
| HR面         | [[HR面]]               | HR interview questions; salary negotiation; offer evaluation          |

---

## Tools

| Name              | Link            | Summary                                                         |
| ----------------- | --------------- | --------------------------------------------------------------- |
| Git               | [[Git]]         | Staging, committing, checkout, branching workflow reference     |
| Command Line      | [[Command Line]] | CLI argument handling in Python with sys module                |
| Logger            | [[Logger]]      | Structured logging for production; alternative to print()       |
| Tech Card Template | [[技术卡片模版]]    | Standard template for new tech notes                            |
