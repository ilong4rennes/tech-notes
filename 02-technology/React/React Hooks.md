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
