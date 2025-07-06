### JSX (JavaScript XML)

- JSX allows you to write HTML-like code in JavaScript files. It makes React code more readable and intuitive.
- Translated to JavaScript using tools like Babel.
#### Example: JSX

```jsx
const element = <h1>Hello, world!</h1>;
ReactDOM.createRoot(document.getElementById('root')).render(element);
```

#### JSX Rules

1. **Return a single parent element**:
    ```jsx
    return (
      <div>
        <h1>Title</h1>
        <p>Subtitle</p>
      </div>
    );
    ```
    
2. **JavaScript expressions inside `{}`**:
    ```jsx
    const name = "Alice";
    return <h1>Hello, {name}!</h1>;
    ```
    
3. **Use camelCase for attributes**:
    ```jsx
    <div className="container"></div> // Use "className" instead of "class".
    ```

### React Components

1. **Functional Components:**
    - A simple JavaScript function that returns JSX.
    - Can use React Hooks to manage state and lifecycle.
    ```jsx
    function Greeting() {
      return <h1>Hello, world!</h1>;
    }
    ```
    
2. **Class Components:**
    - A JavaScript class that extends `React.Component` and has a `render()` method.
    ```jsx
    class Greeting extends React.Component {
      render() {
        return <h1>Hello, world!</h1>;
      }
    }
    ```

### Props (Properties)

Props are used to pass data from a **parent component** to a **child component**. They are immutable (cannot be changed by the child).

#### Example: Using Props

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Usage:
<Welcome name="Alice" />
```

### State

State is a way to manage dynamic data within a component. When the state changes, the component re-renders.

### Handling Events

React events are similar to DOM events, but they use camelCase syntax and a function reference.

#### Example: Event Handling

```jsx
function ButtonClick() {
  const handleClick = () => alert('Button clicked!');

  return <button onClick={handleClick}>Click Me</button>;
}
```
