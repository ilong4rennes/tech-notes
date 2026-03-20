# 第 0 步：先理解 React 函数组件在干嘛

在 React 里，一个组件本质上就是一个函数：

```jsx
export default function App() {
  return <h1>Hello</h1>;
}
```

它返回的是一段 UI。

但问题来了：

如果你想让页面上的内容**随着用户操作变化**，就不能只写死 HTML，得让组件“记住数据”。  
这时候就需要 **hooks**。

---

# 第 1 步：先写最小骨架

先在 `App.jsx` 里写这个：

```jsx
export default function App() {
  return (
    <div>
      <h1>Todo App</h1>
    </div>
  );
}
```

### 解释

这一步只是确认：

- `App` 是根组件
    
- 它返回页面内容
    
- 现在还没有任何交互
    

---

# 第 2 步：加入 `useState`，让输入框能输入

先引入：

```jsx
import { useState } from "react";
```

然后改成这样：

```jsx
import { useState } from "react";

export default function App() {
  const [text, setText] = useState("");

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />

      <p>你正在输入：{text}</p>
    </div>
  );
}
```

---

## 这里 `useState` 是什么

```jsx
const [text, setText] = useState("");
```

你可以把它读成：

- `text`：当前状态值
    
- `setText`：修改这个状态的方法
    
- `""`：初始值
    

也就是：

一开始 `text = ""`  
当你在输入框输入内容时，调用 `setText(...)`，React 就会更新状态，然后重新渲染页面。

---

## 为什么不能写普通变量？

比如这样：

```jsx
let text = "";
```

你改它，React 不会自动刷新页面。  
因为 React 只会跟踪 **state**，不会跟踪普通变量。

所以第一条核心原则是：

**会影响页面显示的数据，要放进 `useState`。**

---

# 第 3 步：加入任务列表，再加一个添加按钮

现在我们需要两个状态：

- 输入框里的内容 `text`
    
- 所有任务组成的数组 `todos`
    

代码改成这样：

```jsx
import { useState } from "react";

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos([...todos, newTodo]);
    setText("");
  }

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <button onClick={handleAdd}>添加</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            {todo.text} - {todo.done ? "已完成" : "未完成"}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 这里你要理解的重点

### 1. `todos` 是数组

因为任务有很多个，所以不能用一个字符串，要用数组。

```jsx
const [todos, setTodos] = useState([]);
```

初始值是空数组。

---

### 2. 添加任务时不要直接改原数组

这句很关键：

```jsx
setTodos([...todos, newTodo]);
```

意思是：

- 把原来的 `todos` 展开
    
- 再把 `newTodo` 放进去
    
- 得到一个新数组
    

为什么不能写：

```jsx
todos.push(newTodo);
```

因为 React 里状态更新最好是**不可变更新**。  
你要给 React 一个**新数组**，这样 React 才更容易判断数据变了。

所以这里你可以记住：

- 数组更新常用 `map`、`filter`、展开运算符 `...`
    
- 不要直接 `push`、`splice`
    

---

# 第 4 步：点击任务，切换完成状态

现在让任务变成可以点击切换。

改 `li` 这一部分：

```jsx
import { useState } from "react";

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos([...todos, newTodo]);
    setText("");
  }

  function handleToggle(id) {
    const newTodos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, done: !todo.done };
      }
      return todo;
    });

    setTodos(newTodos);
  }

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <button onClick={handleAdd}>添加</button>

      <ul>
        {todos.map((todo) => (
          <li
            key={todo.id}
            onClick={() => handleToggle(todo.id)}
            style={{
              cursor: "pointer",
              textDecoration: todo.done ? "line-through" : "none",
            }}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 这里为什么用 `map`

因为我们想要：

- 找到那个被点击的任务
    
- 只改它的 `done`
    
- 其他任务保持不变
    

`map` 很适合做这个事：

```jsx
const newTodos = todos.map((todo) => {
  if (todo.id === id) {
    return { ...todo, done: !todo.done };
  }
  return todo;
});
```

这里：

```jsx
{ ...todo, done: !todo.done }
```

意思是：

- 先复制原来 todo 的所有属性
    
- 再把 `done` 改成相反值
    

比如原来：

```js
{ id: 1, text: "吃饭", done: false }
```

变成：

```js
{ id: 1, text: "吃饭", done: true }
```

---

# 第 5 步：加删除功能

加一个按钮：

```jsx
import { useState } from "react";

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos([...todos, newTodo]);
    setText("");
  }

  function handleToggle(id) {
    const newTodos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, done: !todo.done };
      }
      return todo;
    });

    setTodos(newTodos);
  }

  function handleDelete(id) {
    const newTodos = todos.filter((todo) => todo.id !== id);
    setTodos(newTodos);
  }

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <button onClick={handleAdd}>添加</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            <span
              onClick={() => handleToggle(todo.id)}
              style={{
                cursor: "pointer",
                textDecoration: todo.done ? "line-through" : "none",
                marginRight: "10px",
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => handleDelete(todo.id)}>删除</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 这里为什么用 `filter`

删除的本质就是：

**保留所有不是这个 id 的项**

```jsx
const newTodos = todos.filter((todo) => todo.id !== id);
```

如果数组里有 5 个任务，这句会返回一个新的、更短的数组。

所以你要建立这个直觉：

- `map`：一一转换
    
- `filter`：筛掉不要的
    
- `find`：找一个
    
- `reduce`：累加/汇总
    

---

# 第 6 步：提炼一下，现在你已经真正理解了 `useState`

到这里你已经实际用了 `useState` 三次思维：

1. 输入框内容是状态
    
2. 任务列表是状态
    
3. 点击按钮时通过 `setState` 更新 UI
    

所以 `useState` 的原理你可以先这样理解：

**React 会帮你把组件里的状态记住。每次你调用 `setXxx`，React 就会重新执行组件函数，用新的状态重新生成界面。**

---

# 第 7 步：加入 `useEffect`，把任务保存到本地

现在如果你刷新页面，任务就没了。  
所以我们加 `localStorage`。

先引入：

```jsx
import { useState, useEffect } from "react";
```

然后加两个 effect：

```jsx
import { useState, useEffect } from "react";

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos([...todos, newTodo]);
    setText("");
  }

  function handleToggle(id) {
    const newTodos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, done: !todo.done };
      }
      return todo;
    });

    setTodos(newTodos);
  }

  function handleDelete(id) {
    const newTodos = todos.filter((todo) => todo.id !== id);
    setTodos(newTodos);
  }

  useEffect(() => {
    const savedTodos = localStorage.getItem("todos");
    if (savedTodos) {
      setTodos(JSON.parse(savedTodos));
    }
  }, []);

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <button onClick={handleAdd}>添加</button>

      <ul>
        {todos.map((todo) => (
          <li key={todo.id}>
            <span
              onClick={() => handleToggle(todo.id)}
              style={{
                cursor: "pointer",
                textDecoration: todo.done ? "line-through" : "none",
                marginRight: "10px",
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => handleDelete(todo.id)}>删除</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## `useEffect` 是什么

`useEffect` 的作用是：

**在组件渲染完成后，去做一些额外的事情。**

这些额外的事情就叫 **副作用**。

比如：

- 读本地存储
    
- 写本地存储
    
- 发请求
    
- 开定时器
    
- 监听事件
    

---

## 第一个 effect

```jsx
useEffect(() => {
  const savedTodos = localStorage.getItem("todos");
  if (savedTodos) {
    setTodos(JSON.parse(savedTodos));
  }
}, []);
```

这里的 `[]` 表示：

**只在组件第一次渲染后执行一次**

所以它非常适合“初始化读取”。

---

## 第二个 effect

```jsx
useEffect(() => {
  localStorage.setItem("todos", JSON.stringify(todos));
}, [todos]);
```

这里的 `[todos]` 表示：

**每次 `todos` 变化后，就执行这个 effect**

所以它非常适合“同步保存”。

---

## 依赖数组怎么理解

你先死记这版：

- `useEffect(fn, [])`：只执行一次
    
- `useEffect(fn, [a])`：`a` 变了就执行
    
- `useEffect(fn)`：每次渲染都执行
    

---

# 第 8 步：加入搜索功能，然后引出 `useMemo`

先加一个搜索状态：

```jsx
const [search, setSearch] = useState("");
```

然后加输入框：

```jsx
<input
  value={search}
  onChange={(e) => setSearch(e.target.value)}
  placeholder="搜索任务"
/>
```

再过滤列表。

先不用 `useMemo`，先写最普通版：

```jsx
const filteredTodos = todos.filter((todo) =>
  todo.text.toLowerCase().includes(search.toLowerCase())
);
```

完整代码：

```jsx
import { useState, useEffect } from "react";

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);
  const [search, setSearch] = useState("");

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos([...todos, newTodo]);
    setText("");
  }

  function handleToggle(id) {
    const newTodos = todos.map((todo) => {
      if (todo.id === id) {
        return { ...todo, done: !todo.done };
      }
      return todo;
    });

    setTodos(newTodos);
  }

  function handleDelete(id) {
    const newTodos = todos.filter((todo) => todo.id !== id);
    setTodos(newTodos);
  }

  useEffect(() => {
    const savedTodos = localStorage.getItem("todos");
    if (savedTodos) {
      setTodos(JSON.parse(savedTodos));
    }
  }, []);

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  const filteredTodos = todos.filter((todo) =>
    todo.text.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>

      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <button onClick={handleAdd}>添加</button>

      <br />
      <br />

      <input
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        placeholder="搜索任务"
      />

      <ul>
        {filteredTodos.map((todo) => (
          <li key={todo.id}>
            <span
              onClick={() => handleToggle(todo.id)}
              style={{
                cursor: "pointer",
                textDecoration: todo.done ? "line-through" : "none",
                marginRight: "10px",
              }}
            >
              {todo.text}
            </span>
            <button onClick={() => handleDelete(todo.id)}>删除</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

# 第 9 步：把过滤结果换成 `useMemo`

现在引入：

```jsx
import { useState, useEffect, useMemo } from "react";
```

然后改：

```jsx
const filteredTodos = useMemo(() => {
  console.log("重新计算 filteredTodos");
  return todos.filter((todo) =>
    todo.text.toLowerCase().includes(search.toLowerCase())
  );
}, [todos, search]);
```

---

## `useMemo` 是什么

它的作用是：

**缓存一个计算结果。**

意思是：

- 如果依赖没变，就直接用上次算好的结果
    
- 如果依赖变了，才重新计算
    

---

## 为什么要有它

因为函数组件每次重新渲染时，里面的代码会重新执行。

也就是说，这种代码：

```jsx
const filteredTodos = todos.filter(...)
```

理论上每次渲染都会重新算。

如果这个计算很重，列表很大，就浪费性能。

所以 `useMemo` 的核心是：

**避免重复做没必要的计算**

---

## 你现在先怎么理解它

你先别把它想得太神秘。  
就把它当成：

```jsx
const 某个值 = useMemo(() => {
  return 复杂计算结果;
}, [依赖]);
```

当依赖不变时，这个值直接复用缓存。

---

# 第 10 步：加入主题切换，开始讲 `useContext`

现在我们要做：

- 浅色主题
    
- 深色主题
    
- 子组件里也能拿到 theme
    

这时如果所有东西都写在 `App` 里，看不出 `useContext` 的必要性。  
所以我们要先**拆组件**。

---

# 第 11 步：先拆出 `Header`

先写：

```jsx
function Header({ theme, toggleTheme }) {
  return (
    <div>
      <h1>Todo App</h1>
      <button onClick={toggleTheme}>
        当前主题：{theme}
      </button>
    </div>
  );
}
```

然后在 `App` 里加状态：

```jsx
const [theme, setTheme] = useState("light");

function toggleTheme() {
  setTheme(theme === "light" ? "dark" : "light");
}
```

然后渲染：

```jsx
<Header theme={theme} toggleTheme={toggleTheme} />
```

这时候完整结构大概是：

```jsx
import { useState, useEffect, useMemo } from "react";

function Header({ theme, toggleTheme }) {
  return (
    <div>
      <h1>Todo App</h1>
      <button onClick={toggleTheme}>当前主题：{theme}</button>
    </div>
  );
}

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);
  const [search, setSearch] = useState("");
  const [theme, setTheme] = useState("light");

  function toggleTheme() {
    setTheme(theme === "light" ? "dark" : "light");
  }

  const filteredTodos = useMemo(() => {
    return todos.filter((todo) =>
      todo.text.toLowerCase().includes(search.toLowerCase())
    );
  }, [todos, search]);

  return (
    <div
      style={{
        padding: "20px",
        background: theme === "light" ? "white" : "#222",
        color: theme === "light" ? "black" : "white",
        minHeight: "100vh",
      }}
    >
      <Header theme={theme} toggleTheme={toggleTheme} />
    </div>
  );
}
```

---

## 这里问题在哪

现在 `Header` 要用 `theme`，所以得传 props。  
以后如果 `TodoList`、`Stats`、`Footer` 都要用 `theme`，你就得一直传：

- `App -> Layout -> Main -> TodoList`
    

这就很烦。  
这叫 **props drilling**。

于是就需要 `context`。

---

# 第 12 步：创建 `ThemeContext`

先引入：

```jsx
import {
  useState,
  useEffect,
  useMemo,
  useContext,
  createContext,
} from "react";
```

然后在组件外面写：

```jsx
const ThemeContext = createContext();
```

这相当于创建了一个“全局数据通道”。

---

# 第 13 步：用 Provider 包起来

在 `App` 里把返回值改成：

```jsx
<ThemeContext.Provider value={{ theme, toggleTheme }}>
  <div
    style={{
      padding: "20px",
      background: theme === "light" ? "white" : "#222",
      color: theme === "light" ? "black" : "white",
      minHeight: "100vh",
    }}
  >
    <Header />
  </div>
</ThemeContext.Provider>
```

注意这里已经不传 props 了。

---

# 第 14 步：在 `Header` 里用 `useContext`

改 Header：

```jsx
function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div>
      <h1>Todo App</h1>
      <button onClick={toggleTheme}>当前主题：{theme}</button>
    </div>
  );
}
```

---

## `useContext` 是什么

它的作用是：

**从最近的 Provider 里取共享数据。**

这里：

```jsx
const { theme, toggleTheme } = useContext(ThemeContext);
```

就相当于在说：

“我要从 `ThemeContext` 这条通道里拿数据。”

---

## 你要怎么理解 `context`

你可以把它理解成一个公共仓库。

- `Provider`：往仓库里放值
    
- `useContext`：从仓库里拿值
    

适合放：

- theme
    
- 当前用户
    
- 语言
    
- 登录状态
    
- 全局设置
    

---

# 第 15 步：加入 `useRef`，让添加后自动聚焦输入框

先引入：

```jsx
import {
  useState,
  useEffect,
  useMemo,
  useContext,
  createContext,
  useRef,
} from "react";
```

然后在 `App` 里：

```jsx
const inputRef = useRef(null);
```

给输入框：

```jsx
<input
  ref={inputRef}
  value={text}
  onChange={(e) => setText(e.target.value)}
  placeholder="输入任务"
/>
```

在添加后：

```jsx
function handleAdd() {
  if (!text.trim()) return;

  const newTodo = {
    id: Date.now(),
    text: text,
    done: false,
  };

  setTodos([...todos, newTodo]);
  setText("");
  inputRef.current.focus();
}
```

---

## `useRef` 是什么

它返回一个对象：

```jsx
{ current: ... }
```

比如：

```jsx
const inputRef = useRef(null);
```

开始时：

```js
inputRef.current === null
```

等 React 把 input 渲染出来以后，`current` 就会指向这个 DOM 元素。

所以你就能：

```jsx
inputRef.current.focus();
```

---

## 为什么它不叫 `useState`

因为 `ref` 的特点是：

**它可以存值，但改它不会触发重新渲染。**

所以它适合：

- 拿 DOM
    
- 存定时器 id
    
- 存某些中间值
    

而 `state` 是：

**改了以后页面要跟着更新。**

---

# 第 16 步：把完整版本整理出来

现在我给你一版整合后的代码，你应该已经能看懂大部分了。

```jsx
import {
  useState,
  useEffect,
  useMemo,
  useContext,
  createContext,
  useRef,
} from "react";

const ThemeContext = createContext();

function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div style={{ marginBottom: "20px" }}>
      <h1>Todo App</h1>
      <button onClick={toggleTheme}>当前主题：{theme}</button>
    </div>
  );
}

export default function App() {
  const [text, setText] = useState("");
  const [todos, setTodos] = useState([]);
  const [search, setSearch] = useState("");
  const [theme, setTheme] = useState("light");

  const inputRef = useRef(null);

  function handleAdd() {
    if (!text.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: text,
      done: false,
    };

    setTodos((prev) => [...prev, newTodo]);
    setText("");
    inputRef.current.focus();
  }

  function handleToggle(id) {
    setTodos((prev) =>
      prev.map((todo) =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      )
    );
  }

  function handleDelete(id) {
    setTodos((prev) => prev.filter((todo) => todo.id !== id));
  }

  function toggleTheme() {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  }

  useEffect(() => {
    const savedTodos = localStorage.getItem("todos");
    const savedTheme = localStorage.getItem("theme");

    if (savedTodos) {
      setTodos(JSON.parse(savedTodos));
    }

    if (savedTheme) {
      setTheme(savedTheme);
    }
  }, []);

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);

  useEffect(() => {
    localStorage.setItem("theme", theme);
  }, [theme]);

  const filteredTodos = useMemo(() => {
    console.log("重新计算 filteredTodos");
    return todos.filter((todo) =>
      todo.text.toLowerCase().includes(search.toLowerCase())
    );
  }, [todos, search]);

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <div
        style={{
          minHeight: "100vh",
          padding: "20px",
          background: theme === "light" ? "white" : "#222",
          color: theme === "light" ? "black" : "white",
        }}
      >
        <Header />

        <input
          ref={inputRef}
          value={text}
          onChange={(e) => setText(e.target.value)}
          placeholder="输入任务"
        />
        <button onClick={handleAdd}>添加</button>

        <br />
        <br />

        <input
          value={search}
          onChange={(e) => setSearch(e.target.value)}
          placeholder="搜索任务"
        />

        <ul>
          {filteredTodos.map((todo) => (
            <li key={todo.id}>
              <span
                onClick={() => handleToggle(todo.id)}
                style={{
                  cursor: "pointer",
                  textDecoration: todo.done ? "line-through" : "none",
                  marginRight: "10px",
                }}
              >
                {todo.text}
              </span>
              <button onClick={() => handleDelete(todo.id)}>删除</button>
            </li>
          ))}
        </ul>
      </div>
    </ThemeContext.Provider>
  );
}
```

---

# 第 17 步：把每个 hook 的原理用最白话的话讲清楚

## `useState`

组件重新执行时，普通变量会重置。  
`useState` 会让 React 帮你把数据存起来。

所以它解决的是：

**函数每次重跑，数据怎么不丢。**

---

## `useEffect`

组件渲染完以后，有些事不能直接写在渲染里做。  
比如访问浏览器存储、发请求、定时器。

所以它解决的是：

**渲染之后，怎么安全地做副作用。**

---

## `useMemo`

组件每次重渲染，里面的计算默认都会重跑。  
`useMemo` 可以缓存计算结果。

所以它解决的是：

**重复渲染时，怎么避免没必要的重复计算。**

---

## `useContext`

有些数据很多组件都要用，一级一级传 props 太麻烦。  
`useContext` 允许你直接共享。

所以它解决的是：

**跨层级传数据太麻烦的问题。**

---

## `useRef`

有时你想拿到真实 DOM，或者存一个不会触发渲染的值。  
这时候用 `useRef`。

所以它解决的是：

**既想跨渲染保留一个值，又不想引发重渲染。**

---

# 第 18 步：你现在最该做的练习

你不要一下子背所有 hook。  
你应该按下面顺序自己重新敲一遍：

1. 先只写输入框 + `useState`
    
2. 再写 todo 数组 + 添加
    
3. 再写 toggle + delete
    
4. 再加 `useEffect` 保存本地
    
5. 再加搜索
    
6. 再把搜索换成 `useMemo`
    
7. 再拆 Header，加 `useContext`
    
8. 最后加 `useRef`
    

这样你会真正知道每一步为什么出现。

---

# 第 19 步：你很容易混淆的几个点

## 1. `useEffect` 不是“页面一加载就自动运行的魔法”

它本质是：

**渲染后执行**

只不过 `[]` 让它看起来像“只执行一次”。

---

## 2. `useMemo` 不是必须

很多时候不用也能跑。  
它主要是性能优化。

所以不要见到计算就乱套 `useMemo`。

---

## 3. `useContext` 不是状态管理库

它只是共享数据的通道。  
状态本身通常还是 `useState` 或 `useReducer` 在管。

---

## 4. `useRef.current` 变了不会刷新页面

这一点和 state 最大不同。

---

# 第 20 步：下一步怎么学最有效

下一步最适合你的方式是：

**我带你从零手写这个项目，你每一步只补 5 到 15 行代码，我检查并继续往下带。**

你先做第一步，只写这个最小版本到 `App.jsx`：

```jsx
import { useState } from "react";

export default function App() {
  const [text, setText] = useState("");

  return (
    <div style={{ padding: "20px" }}>
      <h1>Todo App</h1>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="输入任务"
      />
      <p>{text}</p>
    </div>
  );
}
```

你写完后，我直接继续带你写第二步。