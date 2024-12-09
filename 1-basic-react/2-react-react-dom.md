# React JS and Functional Programming

## What is React JS?
React JS is a JavaScript library for building user interfaces (UIs). Developed by Facebook (now Meta), it focuses on creating reusable UI components and efficiently updating the DOM (Document Object Model).

React embraces concepts from functional programming to create predictable, maintainable code. Let’s break down this relationship and how functional programming principles apply to React.

--- 

### React and Functional Programming: Key Connections

1. #### Components as Pure Functions:
    - In React, functional components behave like pure functions.
    - A pure function takes inputs (props) and returns an output (JSX). It does not modify external state or have side effects.
- Example :

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}
```
- This component always returns the same output for the same input (props.name), making it predictable.


---

2. #### Immutability:
    - React promotes immutability by encouraging you to avoid directly modifying state. Instead, you create new copies of data.
    - This immutability is critical for React’s ability to efficiently re-render components.
- Example :

```jsx
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4]; // New array, original unchanged
```

---

3. #### State Management with Hooks (Pure Logic):
    - Hooks like useState and useReducer are inspired by functional programming, allowing you to manage state within functional components.

- Example with **useState** :

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```
- The setCount function creates a new state rather than modifying the existing one, maintaining immutability.


---

4. #### Higher-Order Components (HOCs):
    - Higher Order Components are functions that take a component and return a new component, similar to how higher-order functions work in functional programming.
- Example :

```jsx
function withLogger(WrappedComponent) {
  return function LoggerComponent(props) {
    console.log('Props:', props);
    return <WrappedComponent {...props} />;
  };
}
```

---

5. #### Declarative Code:
    - React’s declarative nature means you describe what the UI should look like, and React takes care of how to update the DOM.
- Example :

```jsx
function App() {
  const isLoggedIn = true;
  return <div>{isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign in.</h1>}</div>;
}
```

---

## React DOM: How It Works with React

### What is React DOM?
- **React DOM** is a package that allows React to interact with the DOM (the tree structure of HTML elements in a webpage). It acts as a bridge between your React components and the browser’s actual DOM.

### How React DOM Works:
1. **Rendering React Elements to the DOM:**
    - React DOM provides a method called **ReactDOM.createRoot()** or **ReactDOM.render()** to render your React components into an actual HTML element in the browser.
- Example :

```jsx 
import React from 'react';
import ReactDOM from 'react-dom/client';

function App() {
  return <h1>Hello, World!</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
- Here, the **App** component is rendered inside the HTML element with the **id="root"**.


---

2. **Virtual DOM:**
    - React uses a ***Virtual DOM*** to optimize updates. Instead of updating the real DOM directly (which is slow), React creates a virtual representation of the DOM in memory.
    - When a component’s state or props change, React:
        - Creates a new virtual DOM.
        - Compares it with the previous one (using a process called diffing).
        - Updates only the parts of the real DOM that have changed.
- Example :

- **initial Render**     
```jsx 
<div>
  <h1>Hello</h1>
</div>
```

- **After State Change:**
```jsx
<div>
  <h1>Hello, World!</h1> {/* Only this text is updated in the real DOM */}
</div>
```

---

3. **Reconciliation Process:**
    - React’s **reconciliation** is the process of updating the DOM efficiently by comparing the old and new virtual DOMs.
    - It finds the minimal set of changes and applies them, making updates fast.

---

4. **Hydration (for Server-Side Rendering):**
    - React DOM also supports **hydration**, which allows React to attach event listeners and update the UI for components rendered on the server.
- Example :

```jsx
const root = ReactDOM.hydrateRoot(document.getElementById('root'), <App />); // Not necessary
```

---

## Key Methods in React DOM: 

1. ### **ReactDOM.createRoot():**
    - Used to render a component tree into the DOM in React 18 and above.
- Example :

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```
---

2. ### **ReactDOM.render():**
    - Used in older versions of React (pre-React 18). (Deprecated)
- Example :

```jsx
ReactDOM.render(<App />, document.getElementById('root'));
```

---

3. ### **ReactDOM.hydrate():**
    - Used for hydrating a server-rendered React app.
- Example :

```jsx
ReactDOM.hydrate(<App />, document.getElementById('root'));
```

---

### How React and React DOM Work Together:

- **React** is responsible for creating and managing components and the Virtual DOM.
- **React DOM** takes the Virtual DOM and updates the real DOM efficiently.
- This separation allows React to be used in environments other than the browser (e.g., React Native for mobile apps).

---

### Summary:

- **React JS** embraces functional programming by using pure functions, immutability, and higher-order components.
- **React DOM** bridges the gap between React components and the browser’s actual DOM.
- The **Virtual DOM** and reconciliation make React’s updates fast and efficient by minimizing real DOM manipulations.
- You use methods like **ReactDOM.createRoot()** to render components into the DOM.




