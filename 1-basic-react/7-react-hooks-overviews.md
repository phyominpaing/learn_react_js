# React Hooks

- React hooks are **special functions** that let you use React features like state and lifecycle methods in functional components. Introduced in React 16.8, hooks eliminate the need to write class components for **managing state or side effects**.

- React hooks are **special functions** that enable functional components to access features like **state management, side effects, and context**. They make React components more readable, reusable, and easier to maintain.

---

## Why React Hooks?

- Before hooks, React required class components for advanced features like state or lifecycle methods.

- Hooks eliminate the need for class components and make React components more **readable, reusable, and easier to maintain**.

**- Hooks :**

1. **Simplify Functional Components :**

   - Hooks provide a way to use state and lifecycle methods in functional components.
   - Removes complexities like managing **this**.

2. **Encourage Reusable Logic :**
   - Logic can be extracted into **custom hooks**, promoting reusability.

---

## Key Points About Hooks

**- What are Hooks?**

- Hooks are functions starting with **use** (e.g., **useState**, **useEffect**). They let you use React features in functional components.

**- Rules for Using Hooks :**

1. Only call hooks **at the top level** (not inside loops, conditions, or nested functions).
2. Only call hooks in **React functions** (functional components or custom hooks).

---

## Popular and Useful Hooks: Syntax, Usage, and Examples

#### 1. useState (State Management)

**What it does :**

- The **useState** hook lets you create and manage **state** in a functional component.
- Think of "state" as data or variables that React **tracks for changes**.
- When the state changes, React re-renders the component to show the updated values.

**When to use :**

- Use useState when you need to store and **update values like user inputs, counters, toggles, or any dynamic data**.

**Syntax :**

```jsx
const [state, setState] = useState(initialValue);
```

- **state:** The current value of the state.
- **setState:** A function to update the state.
- **initialValue:** The starting value of the state.

**Example 1: Counter**

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increaseCount = () => setCount(count + 1);
  const decreaseCount = () => setCount(count - 1);

  return (
    <div>
      <p>Current Count: {count}</p>
      <button onClick={increaseCount}>Increase</button>
      <button onClick={decreaseCount}>Decrease</button>
    </div>
  );
}
```

**Advanced Example: State with Objects**

```jsx
import React, { useState } from "react";

function UserProfile() {
  const [user, setUser] = useState({ name: "", age: "" });

  const handleName = (event) => {
    setUser({ ...user, name: event.target.value });
  };
  const handleAge = (event) => {
    setUser({ ...user, age: event.target.value });
  };
  return (
    <div>
      <input placeholder="Name" value={user.name} onChange={handleName} />
      <input placeholder="Age" value={user.age} onChange={handleAge} />
      <p>
        User: {user.name} (Age: {user.age})
      </p>
    </div>
  );
}

export default UserProfile;
```

---

### 2. useEffect (Side Effects)

**What it does :**

- The **useEffect** hook lets you perform side effects in your component.
- - Side effects include things like **fetching data, updating the DOM directly, or setting up subscriptions** (e.g., WebSocket connections).

**When to use :**

- Use useEffect whenever you need to:
  1. Perform a task when the component renders or updates.
  2. Perform cleanup tasks when the component unmounts.

**Syntax :**

```jsx
useEffect(() => {
  // Your effect logic here

  return () => {
    // Cleanup logic here (optional)
  };
}, [dependencies]);
```

- **dependencies:** An array of variables that the effect depends on. The effect runs again if these change.

**Example 1: Basic Usage (Run on Initial Render)**

```jsx
import React, { useEffect } from "react";

function Welcome() {
  useEffect(() => {
    console.log("Component mounted");
  }, []); // Empty array means this runs only once.

  return <h1>Welcome to React!</h1>;
}
```

**Example 2: Fetching Data**

```jsx
import React, { useState, useEffect } from "react";

function FetchData() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => response.json())
      .then((data) => setData(data));

    // Cleanup (optional) if needed
    return () => console.log("Cleanup on unmount");
  }, []); // Empty array ensures this runs once.

  return (
    <ul>
      {data.map((item) => (
        <li key={item.id}>{item.title}</li>
      ))}
    </ul>
  );
}
```

**Advanced Example: Subscriptions**

```jsx
import React, { useEffect, useState } from "react";

function Timer() {
  const [time, setTime] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setTime((prevTime) => prevTime + 1);
    }, 1000);

    // Cleanup the interval on unmount
    return () => clearInterval(interval);
  }, []); // Run once

  return <p>Timer: {time} seconds</p>;
}
```

---

### 3. useReducer (Advanced State Management)

**What it does :**

- The **useReducer** hook is an alternative to **useState** for managing complex state logic. It is similar to **Redux** but works locally in a component.

**When to use :**

- Use useReducer when:

1. Your state is an object with multiple values.
2. State transitions depend on specific actions.

**Syntax :**

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

- **reducer :** A function that defines how the state changes based on actions.
- **initialState :** The starting state value.

**Example: Counter with Actions**

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
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
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </div>
  );
}
```

---

### 4. useMemo (Memoization)

**What it does :**

- **useMemo** optimizes performance by memoizing (remembering) the result of a calculation. It prevents unnecessary recalculations of expensive operations when dependencies haven’t changed.
- **When to use :**
- Use useMemo when:

1. You want to memoize the result of a calculation.
2. You have a computationally expensive function that runs often.
3. The function depends on values that don't frequently change.

**Syntax :**

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

- **computeExpensiveValue :** The function to compute the value.
- **[a, b] :** Dependencies. The function runs only if these change.

**Example: Expensive Calculation:**

```jsx
import React, { useState, useMemo } from "react";

function ExpensiveComponent({ num }) {
  const compute = useMemo(() => {
    console.log("Calculating...");
    return num * 2;
  }, [num]);

  return <p>Result: {compute}</p>;
}

export default ExpensiveComponent;
```

**Why it’s useful :**

- Without **useMemo**, **expensiveCalculation** would run every time the component re-renders, even if **count** hasn't changed.

---

### 6. useCallback (Memoized Functions)

**What it does :**

- **useCallback** memoizes a function, so it doesn’t get recreated on every render unless its dependencies change. This is helpful when passing callbacks to child components to avoid unnecessary re-renders.

**When to use :**

- Use useCallback when:

1. You pass a function as a prop to a child component.
2. The child should not re-render unless the function's dependencies change.

**Syntax :**

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

**Example: Optimized Button Click Handler**

```jsx
import React, { useState, useCallback } from "react";

function Button({ onClick, label }) {
  console.log(`Rendering button: ${label}`);
  return <button onClick={onClick}>{label}</button>;
}

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Button clicked!");
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <Button onClick={handleClick} label="Click Me" />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
    </div>
  );
}
```

**Why it’s useful :**

- Without **useCallback**, the **handleClick** function would be recreated on every render, causing unnecessary re-renders of the **Button** component.

---

### 7. useRef (References)

**What it does :**

- **useRef** provides a way to access and modify a DOM element directly or store mutable values without causing re-renders.

**When to use:**

- Use useRef for:

1. Accessing DOM elements (like focusing an input).
2. Storing mutable values that don’t trigger re-renders when changed.

**Syntax :**

```jsx
const ref = useRef(initialValue);
```

- **ref.current :** The current value.

**Example 1: Accessing a DOM Element**

```jsx
import React, { useRef } from "react";

function InputFocus() {
  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} placeholder="Type here..." />
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

**Example 2: Storing Mutable State**
```jsx
import React, { useRef, useState } from 'react';

function Timer() {
  const [time, setTime] = useState(0);
  const intervalRef = useRef(null);

  const startTimer = () => {
    if (!intervalRef.current) {
      intervalRef.current = setInterval(() => setTime((prev) => prev + 1), 1000);
    }
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
    intervalRef.current = null;
  };

  return (
    <div>
      <p>Time: {time} seconds</p>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </div>
  );
}
```
**Why it’s useful:**
- **useRef** helps manage intervals or DOM manipulations without causing unnecessary re-renders.

---

