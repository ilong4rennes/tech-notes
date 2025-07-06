# State

- State is simply data that can change over time.

State is:
- **Variables that change** and affect what the user sees
- **Memory for your components** to track information between renders
- **The current snapshot of your app's data** at any given moment

### Types of State

1. **Component state**: Belongs to one component (useState)
2. **Global state**: Shared across many components (Redux)

Redux is just a way to manage global state when you have lots of components that need to share and update the same data.

# Redux

- a single place to store app‑wide state
### Why can’t I just use `useState` everywhere?

You can! But when **many** components need to read/write the same piece of data—think user login info, a shopping cart, or the current page in a paginator—lifting state up becomes messy. Redux offers:

- **One store** that lives outside React.
- **Actions** (plain objects) that describe _what happened_.
- **Reducers** (pure functions) that say _how state changes_.

### The mental model

```php
Component ---> dispatch({ type: "ADD_TODO", text: "Learn Redux" })                                                       |                                                                                v                                                                             Reducer(s)                                                                          |                                                                                v                                                                          New Store State
```

Components **dispatch** actions; reducers **calculate** new state; React‑Redux makes components **re‑render** when the slice they care about changes.

### The two hooks you’ll see everywhere

```javascript
import { useSelector, useDispatch } from 'react-redux';  

const todos = useSelector(state => state.todos); // read from the store
const dispatch = useDispatch();                  // write to the store

dispatch({ type: "ADD_TODO", text: "Buy milk" });
```

- `useSelector` subscribes to the store and returns the bit you ask for.
- `useDispatch` gives you the `dispatch` function so you can fire actions.

Teams often wrap them in **typed helpers** (`useAppSelector`, `useAppDispatch`) so TypeScript knows exactly what’s inside `state` and which actions exist.


### 1. Store

The single source of truth that holds your application's state.

```javascript
import { createStore } from 'redux';
const store = createStore(reducer);
```

### 2. Actions

Plain JavaScript objects that describe what happened. They must have a `type` property.

```javascript
// Action
{
  type: 'ADD_TODO',
  text: 'Learn Redux'
}

// Action creator
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  };
}
```

### 3. Reducers

Pure functions that take the current state and an action, then return a new state.

```javascript
function todoReducer(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, { text: action.text, completed: false }];
    case 'TOGGLE_TODO':
      return state.map((todo, index) => 
        index === action.index 
          ? { ...todo, completed: !todo.completed } 
          : todo
      );
    default:
      return state;
  }
}
```

### 4. Dispatch

The only way to update the state is to dispatch an action.

```javascript
store.dispatch(addTodo('Learn Redux'));
```

## Basic Redux Flow

1. User interacts with the UI (clicks a button)
2. The component dispatches an action
3. The reducer processes the action and returns a new state
4. The store updates the state
5. UI components connected to the store update

## Setting Up Redux with React

### 1. Install necessary packages

```bash
npm install redux react-redux @reduxjs/toolkit
```

### 2. Create a slice using Redux Toolkit

```javascript
// src/features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

export const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    }
  }
});

export const { increment, decrement, incrementByAmount } = counterSlice.actions;
export default counterSlice.reducer;
```

### 3. Set up the store

```javascript
// src/app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export default configureStore({
  reducer: {
    counter: counterReducer
  }
});
```

### 4. Connect Redux to React

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import App from './App';
import store from './app/store';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

### 5. Use Redux in a component

```javascript
// src/features/counter/Counter.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement } from './counterSlice';

export function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <button onClick={() => dispatch(decrement())}>-</button>
      <span>{count}</span>
      <button onClick={() => dispatch(increment())}>+</button>
    </div>
  );
}
```

## When to Use Redux

- Your app has a large amount of application state needed in many places
- The state is updated frequently
- The logic to update state is complex
- The app has a medium or large-sized codebase with many people working on it
- You need to track how state is updated over time

## Redux DevTools

Install the Redux DevTools Extension for Chrome or Firefox to debug your state changes.

```javascript
// Add this when creating your store
const store = createStore(
  reducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);

// Or with Redux Toolkit
export default configureStore({
  reducer: {
    counter: counterReducer
  },
  devTools: process.env.NODE_ENV !== 'production'
});
```

## Best Practices

1. Use Redux Toolkit to avoid boilerplate code
2. Keep your state normalized (avoid nesting)
3. Use selectors to access state
4. Write meaningful action types
5. Use middleware for side effects (Redux Thunk or Redux Saga)

With these fundamentals, you now have a solid foundation for using Redux in your React applications!

---

Let's analyze the Redux usage in the SyllabiSearchList.tsx file:

1. Redux imports and hooks:
```typescript
import { useAppDispatch, useAppSelector } from "~/app/hooks";
import { filtersSlice } from "~/app/filters";
```

2. Usage of Redux hooks in the components:

In SyllabiPage component:
```typescript
const dispatch = useAppDispatch();
const page = useAppSelector((state) => state.filters.page);
```

In SyllabiSearchList component:
```typescript
const dispatch = useAppDispatch();
const curPage = useAppSelector((state) => state.filters.page);
```

3. Redux state usage:

The component is using the `page` state from the Redux store's `filters` slice to handle pagination.

4. Redux state updates:

The component dispatches actions to update the page number:
```typescript
dispatch(filtersSlice.actions.setPage(correctPage));
```

```typescript
const handlePageClick = (page: number) => {
  void dispatch(filtersSlice.actions.setPage(page + 1));
};
```

5. From the codebase context, I can see the filters slice looks like:
```typescript
export interface FiltersState {
  search: string;
  departments: {
    active: boolean;
    names: string[];
    query: string;
  };
  units: {
    active: boolean;
    min: number;
    max: number;
  };
  semesters: {
    active: boolean;
    sessions: Session[];
  };
  levels: {
    active: boolean;
    selected: boolean[]; // selected[i] <=> show i00 level
  };
  page: number;
  exactResultsCourses: string[];
}
```

So the component is specifically working with the `page` property of this state.

The component seems to be using Redux for:
1. Storing the current page number in a global state
2. Retrieving that page number across different components
3. Updating the page number when pagination is clicked or when a course filter requires changing pages

# Redux Usage in SyllabiSearchList.tsx

## Redux Setup and Imports

```typescript
import { useAppDispatch, useAppSelector } from "~/app/hooks";
import { filtersSlice } from "~/app/filters";
```

The component uses custom Redux hooks (`useAppDispatch` and `useAppSelector`) which are likely typed wrappers around the standard Redux hooks. It also imports the `filtersSlice` which contains the actions and reducers for filter-related state.

## Redux State Structure

From the code and the `filters.ts` file, we can see that the Redux state contains a `filters` slice with this structure:

```typescript
export interface FiltersState {
  search: string;
  departments: { active: boolean; names: string[]; query: string; };
  units: { active: boolean; min: number; max: number; };
  semesters: { active: boolean; sessions: Session[]; };
  levels: { active: boolean; selected: boolean[]; };
  page: number;  // <-- This is what SyllabiSearchList uses
  exactResultsCourses: string[];
}
```

## Redux in SyllabiPage Component

```typescript
const dispatch = useAppDispatch();
const page = useAppSelector((state) => state.filters.page);
```

The `SyllabiPage` component retrieves:
- The Redux `dispatch` function
- The current pagination page from the global Redux state

Later in the component, Redux is used to update the page during the URL-based navigation:

```typescript
useEffect(() => {
  if (courseFilter && courseNumbers.length > 0) {
    const courseIndex = courseNumbers.indexOf(courseFilter);
    if (courseIndex >= 0) {
      const correctPage = Math.floor(courseIndex / ITEMS_PER_PAGE) + 1;
      if (page !== correctPage) {
        dispatch(filtersSlice.actions.setPage(correctPage));
      }
    }
  }
}, [courseFilter, courseNumbers, page, dispatch]);
```

This effect ensures that when a user filters to a specific course, the pagination automatically jumps to the correct page where that course would be found. It dispatches a `setPage` action from the `filtersSlice`.

## Redux in SyllabiSearchList Component

```typescript
const dispatch = useAppDispatch();
const curPage = useAppSelector((state) => state.filters.page);
```

The main container component also retrieves the current page and dispatch function.

```typescript
const handlePageClick = (page: number) => {
  void dispatch(filtersSlice.actions.setPage(page + 1));
};
```

This function handles pagination clicks by dispatching the `setPage` action. Note that it adds 1 to the page parameter because the Pagination component uses zero-indexed page numbers, but the Redux store uses one-indexed values.

The pagination UI conditionally renders based on the Redux state:

```typescript
{totalPages > 1 && !courseFilter && (
  <div className="px-6 my-6">
    <Pagination
      currentPage={curPage - 1}
      setCurrentPage={handlePageClick}
      totalPages={totalPages}
    />
  </div>
)}
```

## Redux Data Flow

1. Initial state: The `page` field in the `filters` slice is initialized (presumably to 1)
2. User interaction:
   - User clicks pagination -> `handlePageClick` dispatches `setPage` action
   - User applies course filter -> useEffect may dispatch `setPage` to jump to correct page
3. State update: The Redux reducer in `filtersSlice` updates the `page` value
4. Re-render: Components that select the `page` state re-render with the new value
5. UI updates: Pagination controls display the current page, and the `pagedCourseNumbers` memoized value recalculates to show the appropriate courses

This Redux implementation centralizes the pagination state, allowing both the course listing and pagination components to stay in sync, even when the user navigates directly to a filtered URL.
