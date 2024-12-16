# React Router: Overview and Key Concepts

- React Router is a popular library in the React ecosystem that facilitates routing and navigation in sReact applications.
- It allows you to navigate between different pages or components without reloading the page. This makes it ideal for single-page applications (SPAs).

---

#### Install React Router

```jsx
npm install react-router-dom
```

---

# Using **createBrowserRouter** and Separating Routes

- The **createBrowserRouter** API allows you to define routes programmatically in a centralized file, making your routing setup more modular and easier to manage.

#### Why Use createBrowserRouter?

- Keeps routes organized in one place.
- Allows integration of route-specific loaders and actions.
- Easier to test and debug.

---

#### Steps to Separate Routes:

**1. Define and Create Routes in a Separate File**

- Create a file (e.g., routes.js) to define your routes:
- Use the **createBrowserRouter** function to define your application's routes in a structured way.
- **Example :**

```jsx
import { createBrowserRouter } from "react-router-dom";
import Layout from "./components/Layout"; // Root component
import Home from "./pages/Home"; // Page components
import About from "./pages/About";
import Contact from "./pages/Contact";

const routes = createBrowserRouter([
  {
    path: "/",
    element: <Layout />, // Root component (acts as a layout)
    children: [
      { path: "", element: <Home /> }, // Default route
      { path: "about", element: <About /> },
      { path: "contact", element: <Contact /> },
    ],
  },
]);

export default routes;
```

**2. Use the Routes and RouterProvider in the Main File**

- In your main.jsx file, integrate the routes using **RouterProvider**.
- **Example :**

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { RouterProvider } from "react-router-dom";
import routes from "./routes";

ReactDOM.render(
  <React.StrictMode>
    <RouterProvider router={routes} />
  </React.StrictMode>,
  document.getElementById("root")
);
```

---

**3. Add Nested Routes with the &lt;Outlet&gt; Component**

- To add nested routes, use the **&lt;Outlet&gt;** component from React Router.
- The **&lt;Outlet&gt;** component will render the child route components based on the current URL.
- If your root component **&lt;Layout&gt;** needs to include child components dynamically, use **&lt;Outlet&gt;**.
- **Example :**

```jsx
import { Outlet, Link } from "react-router-dom";

function Layout() {
  return (
    <div>
      <header>
        <nav>
          <Link to="/">Home</Link> | <Link to="/about">About</Link> |{" "}
          <Link to="/contact">Contact</Link>
        </nav>
      </header>

      <Outlet /> {/* Render child routes here */}

      <footer>
        <Footer />
      </footer>
    </div>
  );
}

export default Layout;
```
**Why Separate Routes?**
1. **Scalability:** Easy to manage as your app grows.
2. **Readability:** Keeps routing logic isolated from components.
3. **Modularity:** Allows easy reuse or modification of routes.

---

## Hooks in React Router

- React Router provides several hooks to manage routing dynamically. Here are the most useful ones:

**1. useParams**
- **Purpose :**
- The **useParams** hook is used to extract dynamic parameters from the URL. For example, in a route like **/user/:id**, the **id** part is dynamic, and **useParams** allows you to access it.
- **useParams** allows you to access dynamic parameters from the current URL.
- Retrieves route parameters from the URL.
- Useful for accessing dynamic parts of the URL (e.g., /users/:id).
- **Example :**
```jsx
// url: http://localhost:3000/users/123 // where 123 is the id

import { useParams } from 'react-router-dom';

function UserProfile() {
  const { id } = useParams(); // Access the "id" parameter
  return <h1>User ID: {id}</h1>; // Output: User ID: 123
}
```

- **Advanced Example: Multiple Parameters**
```jsx
// url : http://localhost:3000/user/456/post/789
import { useParams } from 'react-router-dom';

function UserPost() {
  const { id, postId } = useParams(); // Access "id" and "postId" parameters

  return (
    <div>
      <h1>User ID: {id}</h1>  // Output: User ID: 456
      <h2>Post ID: {postId}</h2> // Output: Post ID: 789
    </div>
  );
}
```
--- 

**2. useNavigate**
- **Purpose :**
- The **useNavigate** hook is used for programmatic navigation. It allows you to redirect users or navigate between routes without relying on **&lt;Link&gt;** or **&lt;Link&gt;** components.
- **useNavigate** is used to navigate between different routes.
- Programmatically navigates to a different route.
- Useful for redirecting users after an action (e.g., form submission).
- **Example :**
```jsx
import { useNavigate } from 'react-router-dom';

function HomePage() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/about'); // Navigate to the "About" page
  };

  return <button onClick={handleClick}>Go to About</button>;
}
```
- Initial URL: **/** 
- After Button Click: **/about**

---
### Combining **useNavigate** and **useParams**
- Example: Redirect After Form Submission
```jsx
import { useParams, useNavigate } from 'react-router-dom';

function UpdateUser() {
  const { id } = useParams(); // Get user ID from URL
  const navigate = useNavigate();

  const handleUpdate = () => {
    // Perform some update logic...

    // Redirect to the user's profile
    navigate(`/user/${id}/profile`);
  };

  return <button onClick={handleUpdate}>Update User</button>;
}
```


---

**4. useSearchParams**
- The **useSearchParams** hook is a React Router feature that allows you to interact with the query string of a URL (the part after the **?**). 
- This hook is useful for working with **query parameters** in a declarative and React-friendly way.

**Key Features**
1. **Read Query Parameters:**
- Extract specific parameters from the URL.
- For example, in **http://localhost:3000?query=react&page=1** , you can read **query** and **page**.

2. **Update Query Parameters:**
- Modify the query string programmatically while keeping the UI state in sync.

3. **Reactivity :**
- The component re-renders automatically when the query string changes.

**Syntax**
```jsx
const [searchParams, setSearchParams] = useSearchParams();
```
- **searchParams:** An object-like API to get query parameter values.
- **setSearchParams:** A function to update the query string.

#### **Reading Query Parameters**
- **Example :**
```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams] = useSearchParams();

  const query = searchParams.get('query'); // Get the value of "query"
  const page = searchParams.get('page');   // Get the value of "page"

  return (
    <div>
      <h1>Search Results</h1>
      <p>Query: {query}</p>
      <p>Page: {page}</p>
    </div>
  );
}
```
- URL: **http://example.com/search?query=react&page=1**
- Output:
```jsx
Query: react
Page: 1
```
---
#### Updating Query Parameters
- **Example :**
```jsx
import { useSearchParams } from 'react-router-dom';

function SearchPage() {
  const [searchParams, setSearchParams] = useSearchParams();

  const updateQuery = () => {
    setSearchParams({ query: 'javascript', page: '2' }); // Updates the URL
  };

  return (
    <div>
      <button onClick={updateQuery}>Search for JavaScript</button>
    </div>
  );
}
```
- Initial URL: **http://example.com/search**
- After Click: **http://example.com/search?query=javascript&page=2**
---

### Common Use Cases
**1. Search and Filters:**
- Store search terms or filter criteria in the query string.
- Example: **/products?category=shoes&size=10.**

**2. Pagination:**
- Track the current page of a list or table.
- Example: **/articles?page=3.**

**3.Sorting and Preferences:**
- Sort items based on user preferences.
- Example: **/items?sort=price&order=asc.**

---
**Advanced Features**
**- Combining with State**
- You can use **useSearchParams** in combination with **useState** to manage query parameters and local state.
- **Example :**
```jsx
import { useSearchParams } from 'react-router-dom';
import { useState } from 'react';

function SearchWithState() {
  const [searchParams, setSearchParams] = useSearchParams();
  const [searchTerm, setSearchTerm] = useState(searchParams.get('query') || '');

  const handleSearch = () => {
    setSearchParams({ query: searchTerm });
  };

  return (
    <div>
      <input
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      <button onClick={handleSearch}>Search</button>
    </div>
  );
}
```
---




