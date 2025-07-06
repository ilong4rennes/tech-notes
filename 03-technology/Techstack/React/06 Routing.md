# React Router

### What React gives you (and what it doesn’t)

- **React** lets you build pieces of UI called **components** and stitch them together.
- Out of the box, React knows nothing about **pages** or **URLs**—to React, the whole browser window is just one big component tree.

### Single‑Page Apps in one sentence

#### Traditional Multi-Page Applications

Traditionally, web applications worked by loading a completely new HTML document every time you clicked a link or submitted a form:

1. You click a link to go from your inbox to settings
2. The browser makes a request to the server
3. The server generates a new HTML page
4. Your browser discards the current page completely
5. The new HTML is downloaded and rendered
6. All JavaScript and CSS must be reloaded
#### Single-Page Application Approach

In a SPA architecture:

1. The initial page load fetches one HTML file (often very minimal)
2. JavaScript frameworks like React then take control
3. When you navigate (like clicking a link to settings):
    - JavaScript intercepts the click
    - Instead of requesting a new HTML page, it requests just the data needed (often as JSON)
    - React updates only the parts of the page that need to change
    - The URL changes (using browser history API) but no full page reload occurs

### Enter a **router**

A router is a tiny library that watches the browser’s address bar and decides **which component to show**.

```jsx
// React‑Router example (not Next.js yet) 
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';  

function App() {   
	return (     
		<BrowserRouter>       
			<nav>         
				<Link to="/">Home</Link> | <Link to="/about">About</Link>
			</nav>        
			
			<Routes>         
				<Route path="/" element={<HomePage />} />         
				<Route path="/about" element={<AboutPage />} />       
			</Routes>     
		</BrowserRouter>   
	); 
}
```

- `BrowserRouter` listens to `window.history`.
- `Route` pairs a URL **pattern** (`/about`) with a React **component** (`<AboutPage/>`).

### The **router hook**

Instead of sprinkling `<Link>` everywhere, you sometimes need to change the URL in code (e.g., after saving a form). React‑Router gives you:

```Javascript
import { useNavigate } from 'react-router-dom'; 
const navigate = useNavigate(); 
navigate('/dashboard');      // changes the URL and swaps the view
```

Next.js (see §3) exposes a similar idea called `useRouter()`.


# React Router Basics

### Setup

```javascript
npm install react-router-dom
```

### Core Components

1. **BrowserRouter**: Wraps your app to enable routing
2. **Routes**: Container for all route definitions
3. **Route**: Defines a single route with path and component
4. **Link**: Creates navigation links without page refreshes

### Simple Example

```javascript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Key Concepts

- **Dynamic Routes**: Use `:paramName` in paths
- **useParams**: Hook to access URL parameters
- **useNavigate**: Hook for programmatic navigation
- **Nested Routes**: Create routes inside other routes

# Next.js Router Basics

### Pages Router (Traditional)

- No installation needed - built into Next.js
- Create files in the `pages` directory to define routes
- Filename directly corresponds to the URL path

### Example

```
pages/
  index.js         → /
  about.js         → /about
  users/
    index.js       → /users
    [id].js        → /users/123 (dynamic)
```

### Navigation

```javascript
import Link from 'next/link';
import { useRouter } from 'next/router';

function NavBar() {
  const router = useRouter();
  
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      
      {/* Programmatic navigation */}
      <button onClick={() => router.push('/users')}>
        Users
      </button>
    </nav>
  );
}
```

### App Router (Next.js 13+)

- Routes defined in the `app` directory
- Uses folder structure with special files:
    - `page.js` → renders the UI for a route
    - `layout.js` → shared UI wrapper

### Example Structure

```
app/
  page.js           → /
  about/
    page.js         → /about
  users/
    page.js         → /users
    [id]/
      page.js       → /users/123 (dynamic)
```

### Navigation in App Router

```javascript
import Link from 'next/link';
import { useRouter } from 'next/navigation'; // Note: different import

function NavBar() {
  const router = useRouter();
  
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      
      <button onClick={() => router.push('/users')}>
        Users
      </button>
    </nav>
  );
}
```

## Router.query in Next.js

Router.query transforms URL elements into a JavaScript object:

1. **Query parameters** become key-value pairs: `/search?keyword=javascript` → `{ keyword: 'javascript' }`
2. **Dynamic route segments** become properties: `/products/12345` → `{ id: '12345' }`
3. **Combined elements** merge together: `/categories/electronics/products/12345?color=black` → `{ category: 'electronics', id: '12345', color: 'black' }`
4. **Repeated parameters** become arrays: `/blog?tag=react&tag=nextjs` → `{ tag: ['react', 'nextjs'] }`

`const { course: courseQuery } = router.query;`
- This creates a variable named `courseQuery` with the value from `router.query.course`.