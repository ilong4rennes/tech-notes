### **1. `useState`: Add State to a Functional Component**

- **Purpose**: `useState` is used to store and update data in a component. The component re-renders whenever the state changes.

**Syntax**:

```javascript
const [state, setState] = useState(initialValue);
```

- `state`: The current value of the state.
- `setState`: A function to update the state.
- `initialValue`: The default value of the state.

**Example**: Counter

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // Start with count = 0.

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**React useState Summary**

- `const [state, setState] = useState(initial)`
- `state` holds the current value; `setState` updates it
- You pass `setState` either:
    1. **A new value** (same type as `state`)
    2. **An updater fn**: `(prev) => next` based on the previous state

---

### **2. `useEffect`: Perform Side Effects**

- **Purpose**: `useEffect` runs code after the component renders. Use it for:
    - Fetching data.
    - Subscribing to services (like WebSockets).
    - Updating the DOM.

**Syntax**:

```javascript
useEffect(() => {
  // Code to run after render.
  return () => {
    // Cleanup code (optional).
  };
}, [dependencies]);
```

- `dependencies`: An array of values. The effect runs only when one of these changes.

**Example**: Update the document title

```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Count: ${count}`; // Update the title after render.
  }, [count]); // Only runs when `count` changes.

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### **3. `useContext`: Access Context Values in a Component**

- **Purpose**: `useContext` allows you to access data from the **Context API** directly, without passing props.

**Syntax**:

```javascript
const value = useContext(Context);
```

- `Context`: The context object created with `React.createContext`.

**Example**: Theme Context

```jsx
import React, { createContext, useContext } from 'react';

const ThemeContext = createContext('light'); // Default value: 'light'.

function ThemedButton() {
  const theme = useContext(ThemeContext); // Access context value.
  return <button className={theme}>Click Me</button>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedButton />
    </ThemeContext.Provider>
  );
}
```

---

### **4. `useReducer`: Manage Complex State Logic**

- **Purpose**: `useReducer` is an alternative to `useState` for handling complex state updates (e.g., multiple values or actions).

**Syntax**:

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

- `reducer`: A function that takes the current state and an action, and returns a new state.
- `dispatch`: A function to trigger actions.
- `initialState`: The starting value of the state.

**Example**: Counter with Reducer

```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
}
```

---

### **5. `useRef`: Access DOM Nodes or Persistent Values**

- **Purpose**: `useRef` stores a reference to a DOM element or a value that doesn’t reinitialize on re-renders.

**Syntax**:

```javascript
const ref = useRef(initialValue);
```

- `ref.current`: Holds the value (or DOM element).

**Example**: Focusing an Input

```jsx
import React, { useRef } from 'react';

function InputFocus() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus(); // Access the DOM element.
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

---

### **6. `useMemo`: Optimize Expensive Computations**

- **Purpose**: `useMemo` memoizes the result of an expensive calculation to prevent re-computation on every render.

**Syntax**:

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- `computeExpensiveValue`: The function to compute the value.
- `[a, b]`: Dependencies. The function re-runs only if these change.

**Example**: Expensive Calculation

```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveComponent() {
  const [count, setCount] = useState(0);

  const expensiveCalculation = useMemo(() => {
    console.log('Calculating...');
    return count * 2;
  }, [count]);

  return (
    <div>
      <p>Expensive Value: {expensiveCalculation}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

---

### **7. `useCallback`: Optimize Functions**

- **Purpose**: `useCallback` memoizes a function to prevent it from being re-created unnecessarily during renders.

**Syntax**:

```javascript
const memoizedCallback = useCallback(() => doSomething(a, b), [a, b]);
```

- `doSomething`: The function to memoize.
- `[a, b]`: Dependencies. The function re-creates only if these change.

**Example**: Prevent Button Re-render

```jsx
import React, { useState, useCallback } from 'react';

function Button({ onClick }) {
  console.log('Button rendered');
  return <button onClick={onClick}>Click Me</button>;
}

function App() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []); // Function re-created only if dependencies change.

  return (
    <div>
      <p>Count: {count}</p>
      <Button onClick={increment} />
    </div>
  );
}
```

---

### **Summary Table**

| **Hook**      | **Purpose**                                        | **Example Use Case**                      |
| ------------- | -------------------------------------------------- | ----------------------------------------- |
| `useState`    | Add state to functional components.                | Counter, toggles, input fields.           |
| `useEffect`   | Perform side effects after render.                 | Fetch data, update the DOM.               |
| `useContext`  | Access context values in a component.              | Theme, user authentication.               |
| `useReducer`  | Manage complex state logic.                        | Multi-step forms, complex counters.       |
| `useRef`      | Access DOM nodes or persistent values.             | Focusing inputs, storing timers.          |
| `useMemo`     | Optimize expensive computations.                   | Prevent recalculations (e.g., sorting).   |
| `useCallback` | Optimize functions to prevent unnecessary renders. | Prevent unnecessary function re-creation. |
[Callback](https://zhida.zhihu.com/search?content_id=438443385&content_type=Answer&match_order=1&q=Callback&zhida_source=entity) 是一种异步调用的实现，打个比方，你希望让你的异地的同事帮助你完成某项工作，为了保证你们业务衔接流畅，你留了你的电话让他有进展和你联系（即Callback函数），那么随着事情的推进，你的同事会不断的打电话跟进让你知道那边的情况，这就是一种异步回调；C**allback 本意就是你传递一个函数给对方，当对方的工作有进展的时候就调用这个函数通知你**。

[Hook](https://zhida.zhihu.com/search?content_id=438443385&content_type=Answer&match_order=1&q=Hook&zhida_source=entity) 则是一种 API 拦截手段，就比如一个公司有两个部门，他们平时会各指派一个业务对接人（[API接口](https://zhida.zhihu.com/search?content_id=438443385&content_type=Answer&match_order=1&q=API%E6%8E%A5%E5%8F%A3&zhida_source=entity)）去沟通彼此工作；但是某天因为业务调整，他们之间有一些事务需要外包出去，外包的事项是由你临时代为沟通，两边的对接人都不知道具体详情；为了避免混乱，此时你被作为一个中间人介入，切断原来两个部门之间的联系，他们的联络人都先和你沟通，由你决定哪些事情需要外包，哪些事情需要转达到另一个部门，这时候你就是一个Hook函数，拦截了原有的调用，并重新决定下一步的做什么，如果要继续原有流程也需要你代为转达。**Hook的特点就是不改变原有双边的逻辑的情况下，在API接口上插入一个拦截调用的Hook函数，从而截取调用数据、甚至可以改变程序行为。**
