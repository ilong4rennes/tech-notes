## Core Concepts

Memoization in React is an optimization technique that caches expensive function calls or component renders to avoid unnecessary recalculations or re-renders.

## useMemo Hook

`useMemo` caches the result of a calculation between renders.

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

The function runs only when dependencies change.

### Example: Expensive Calculation

```javascript
function ProfilePage({ userId }) {
  // Without memoization - recalculates on every render
  // const userStats = calculateStats(userData);
  
  // With memoization - recalculates only when userId changes
  const userStats = useMemo(() => {
    console.log("Calculating stats...");
    return calculateStats(userData);
  }, [userData]);
  
  return <div>{userStats.totalScore}</div>;
}
```

### Example: Preventing Object Recreation

```javascript
function SearchComponent() {
  const [query, setQuery] = useState("");
  
  // Without memoization - creates new object every render
  // const searchOptions = { caseSensitive: false, term: query };
  
  // With memoization - only creates new object when query changes
  const searchOptions = useMemo(() => {
    return { caseSensitive: false, term: query };
  }, [query]);
  
  return <ResultsList options={searchOptions} />;
}
```

## React.memo HOC

`React.memo` is a higher-order component that memoizes an entire component.

```javascript
const MemoizedComponent = React.memo(MyComponent);
```

The component re-renders only when props change (with shallow comparison).

### Example: List Item Component

```javascript
// Without memoization
// function TodoItem({ text, completed }) {
//   console.log(`Rendering: ${text}`);
//   return <li style={{ textDecoration: completed ? 'line-through' : 'none' }}>{text}</li>;
// }

// With memoization
const TodoItem = React.memo(function TodoItem({ text, completed }) {
  console.log(`Rendering: ${text}`);
  return <li style={{ textDecoration: completed ? 'line-through' : 'none' }}>{text}</li>;
});
```

## useCallback Hook

`useCallback` memoizes functions themselves to maintain referential equality.

```javascript
const memoizedCallback = useCallback(() => doSomething(a, b), [a, b]);
```

### Example: Event Handler in Child Component

```javascript
function ParentComponent() {
  const [count, setCount] = useState(0);
  
  // Without memoization - creates new function on every render
  // const handleClick = () => setCount(count + 1);
  
  // With memoization - maintains same function reference unless count changes
  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);
  
  return <ChildButton onClick={handleClick} />;
}

// Memoized child only re-renders when handleClick reference changes
const ChildButton = React.memo(function ChildButton({ onClick }) {
  console.log("Child button rendering");
  return <button onClick={onClick}>Increment</button>;
});
```

## When to Use Memoization

- For expensive calculations
- For preventing object/array recreation in props
- For event handlers passed to memoized child components
- For dependencies in useEffect

## When to Avoid Memoization

- Simple calculations
- Primitive values (strings, numbers, booleans)
- When performance isn't a concern

## Performance Measurement

Always measure before and after memoization with React DevTools Profiler to ensure it's actually improving performance.