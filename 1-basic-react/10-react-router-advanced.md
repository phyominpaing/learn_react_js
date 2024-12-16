# Comprehensive Guide to React Router (From Basics to Advanced)

### 1. What is React Router?

React Router is a library for managing navigation in React applications. It allows you to create dynamic, single-page applications (SPAs) with seamless navigation between different views or pages without reloading the entire app.

---

### 2. Setting Up React Router

**Installation**

- Run the following command to install React Router:

```
npm install react-router-dom
```

**Basic Folder Structure**

- For better organization, we’ll separate routes into a dedicated file:

```
src/
  components/
    Header.jsx
    Home.jsx
    About.jsx
    Products.jsx
    ProductDetails.jsx
  routes/
    routes.jsx
  App.jsx
  index.js
```

---

### 3. Basic Example with createBrowserRouter and RouterProvider

###### **routes.jsx**

- This file defines all the routes in the app:

```jsx
import { createBrowserRouter } from "react-router-dom";
import Home from "../components/Home";
import About from "../components/About";
import Products from "../components/Products";
import ProductDetails from "../components/ProductDetails";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />,
  },
  {
    path: "/about",
    element: <About />,
  },
  {
    path: "/products",
    element: <Products />,
  },
  {
    path: "/products/:id",
    element: <ProductDetails />,
  },
]);

export default router;
```

**Result**

- Visiting **/** renders the **Home** component.
- Visiting **/about** renders the **About** component.
- Visiting **/products** renders the **Products** component.
- Visiting **/products/1** renders the **ProductDetails** component for **product 1**.

---

### 4. Link Component

The **Link** component is used for navigation without refreshing the page.

**Example**
```jsx
import { Link } from 'react-router-dom';

const Header = () => {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/products">Products</Link>
    </nav>
  );
};

export default Header;
```
**Key Points**
- **Link** is like an **&lt;a&gt;** tag but prevents full-page reloads.
- Use **to="/path"** to specify the destination route.


---

### 5. Nested Routing
- Nested routing allows you to render child components inside a parent route.
- **Example: Adding Nested Routes**
- **routes.jsx**

```jsx
import { createBrowserRouter } from 'react-router-dom';
import Products from '../components/Products';
import ProductDetails from '../components/ProductDetails';
import Reviews from '../components/Reviews';

const router = createBrowserRouter([
  {
    path: '/products',
    element: <Products />,
    children: [
      {
        path: ':id',
        element: <ProductDetails />,
        children: [
          {
            path: 'reviews',
            element: <Reviews />,
          },
        ],
      },
    ],
  },
]);

export default router;
```
**Products.jsx**

```jsx
import { Outlet } from 'react-router-dom';

const Products = () => {
  return (
    <div>
      <h1>Products</h1>
      <Outlet /> {/* Renders nested routes */}
    </div>
  );
};

export default Products;
```
**ProductDetails.jsx**
```jsx
import { Outlet } from 'react-router-dom';

const ProductDetails = () => {
  return (
    <div>
      <h2>Product Details</h2>
      <Outlet /> {/* Renders nested routes */}
    </div>
  );
};

export default ProductDetails;
```
**Result**
- **/products** renders the **Products** component.
- **/products/1** renders **ProductDetails** inside **Products**.
- **/products/1/reviews** renders **Reviews** inside **ProductDetails**.

---

### 6. Dynamic Routes
Dynamic routes allow you to use parameters in the URL, such as /products/:id.
- Example: **useParams** Hook
```jsx
import { useParams } from 'react-router-dom';

const ProductDetails = () => {
  const { id } = useParams(); // Extract the "id" parameter from the URL

  return <h2>Product ID: {id}</h2>;
};

export default ProductDetails;
```
**Result**
- Visiting **/products/1** will display: **Product ID: 1**.
- Visiting **/products/2** will display: **Product ID: 2**.

----

### 7. Programmatic Navigation
The useNavigate hook is used to navigate programmatically.
- **Example**
```jsx
import { useNavigate } from 'react-router-dom';

const GoToProducts = () => {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/products'); // Navigate to the Products page
  };

  return <button onClick={handleClick}>Go to Products</button>;
};

export default GoToProducts;
```
**Result**
- Clicking the button navigates to **/products**.

----

### 8. Search and Filtering with **useSearchParams**
- The **useSearchParams** hook helps manage query parameters in the URL.
- The **useSearchParams** hook is used to manage query parameters in the URL. Query parameters are the key-value pairs that come after a **?** in a URL (e.g., **/products?search=phone&category=electronics**).

**Why Use useSearchParams?**
- To handle search and filtering functionality.
- To keep the app state (e.g., search terms or filters) reflected in the URL.
- To make the app shareable with query-specific states.


**Basic Example: Searching**
Scenario:
- We want to create a product search feature where the search term updates the URL.

```jsx
import { useSearchParams } from 'react-router-dom';

const ProductSearch = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const search = searchParams.get('search') || ''; // Get the "search" query parameter

  const handleSearchChange = (e) => {
    setSearchParams({ search: e.target.value }); // Update the "search" query parameter
  };

  return (
    <div>
      <h1>Search Products</h1>
      <input
        type="text"
        value={search}
        placeholder="Search products..."
        onChange={handleSearchChange}
      />
      <p>Searching for: {search}</p>
    </div>
  );
};

export default ProductSearch;
```
**Explanation:**
- **searchParams.get('search')**: Extracts the current value of the **search** query parameter.
- **setSearchParams({ search: e.target.value })**: Updates the query parameter in the URL dynamically.
- Typing in the input box changes the URL (e.g., **/products?search=phone**).

---
**Example: Searching and Filtering**
- **Scenario:**
- Extend the above example to add filtering by category (e.g., Electronics, Books).

```jsx
const ProductSearchFilter = () => {
  const [searchParams, setSearchParams] = useSearchParams();
  const search = searchParams.get('search') || '';
  const category = searchParams.get('category') || 'All';

  const handleSearchChange = (e) => {
    setSearchParams({ search: e.target.value, category }); // Update search and keep category
  };

  const handleCategoryChange = (newCategory) => {
    setSearchParams({ search, category: newCategory }); // Update category and keep search
  };

  return (
    <div>
      <h1>Search and Filter Products</h1>
      <input
        type="text"
        value={search}
        placeholder="Search products..."
        onChange={handleSearchChange}
      />
      <div>
        <button onClick={() => handleCategoryChange('All')}>All</button>
        <button onClick={() => handleCategoryChange('Electronics')}>Electronics</button>
        <button onClick={() => handleCategoryChange('Books')}>Books</button>
      </div>
      <p>Search: {search}</p>
      <p>Category: {category}</p>
    </div>
  );
};

export default ProductSearchFilter;
```
**Explanation:**
- Search Input: Updates the **search** query parameter.
- Category Buttons: Update the **category** query parameter.
- Combined Query: Both **search** and **category** are maintained in the URL.

**Example URLs:**
- **/products?search=phone** → Search for "phone".
- **/products?category=Books** → Filter by "Books".
- **/products?search=phone&category=Electronics** → Search "phone" in "Electronics".

---

### 9. Favorite Hooks Overview
**useParams**
- Extracts dynamic route parameters.
- Example: **/products/:id** → **useParams()** returns **{ id: '1' }**.

**useNavigate**
- Programmatically navigate to a route.
- Example: **navigate('/about')**.

**useSearchParams**
- Manages query parameters.
- Example: **setSearchParams({ search: 'phone' })** updates the URL to **?search=phone**.


---

### 10. Summary
- **Link Component**: For navigation without full-page reloads.
- **Nested Routing**: Render child components inside parent routes.
- **Dynamic Routes**: Use **useParams** for dynamic segments in the URL.
- **Programmatic Navigation**: Use **useNavigate** to navigate via code.
- Search & Filtering: Use **useSearchParams** to handle query parameters.

---

