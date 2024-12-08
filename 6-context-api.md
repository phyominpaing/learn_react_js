# React Context API: Deep Dive

The **Context API** in React is a feature designed to share state (data) **_globally_** across components without having to pass props manually through every level of the component tree.
It is an effective way to avoid **prop drilling**.

---

## Core Concepts of the Context API

**1. Creating Context :**

- Context is created using the React.createContext() method.
- This method returns an object containing two components:
  1. **Provider** : Used to provide the data.
  2. **Consumer** : Used to access the data.

---

**2. Provider :**

- Wraps the components across the tree that need access to the context.
- The Provider component accepts a **value prop** that provides the data to be shared.
- The **value prop** can be any data type, including objects, arrays, or functions.

---

**3. Consumer :**

- Retrieves the value from the nearest Provider in the tree.

---

**4. useContext Hook :**

- A hook provided by React that allows components to access the **context value**.
- Simplifies consuming the context by avoiding the need for **Consumer**.

---

## Advantages of Context API

- Eliminates prop drilling, allowing data to be shared at any depth in the component tree.
- Lightweight and built into React, with no need for external libraries.
- Works seamlessly with functional components via hooks.
- Easy to use and understand.

---

## Setting Up a Context API Example

**1. Create a Separate Context File**

- **File: UserContext.js**
  - This file defines the context and its provider.

```jsx
import React, { createContext, useState } from "react";

// Create Context
export const UserContext = createContext();

// Create Provider Component
export const UserProvider = ({ children }) => {
  const [user, setUser] = useState({ name: "Alice", age: 25 });

  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
};
```

---

**2. Wrap the Application in the Provider**

- **File: App.jsx**

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { UserProvider } from "./UserContext";

ReactDOM.render(
  <React.StrictMode>
    <UserProvider>
      <App />
    </UserProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

---

**3. Consume the Context in a Component**

- **File: Profile.jsx (Component)**

```jsx
import React, { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {
  const { user, setUser } = useContext(UserContext);

  const updateName = () => setUser((prev) => ({ ...prev, name: "Bob" }));

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold">Welcome, {user.name}!</h1>
      <p>Age: {user.age}</p>
      <button
        onClick={updateName}
        className="mt-4 bg-blue-500 text-white px-4 py-2 rounded"
      >
        Update Name
      </button>
    </div>
  );
}

export default Profile;
```

---

### Disadvantages of Context API

**1. Context Hell (Overuse of Contexts) :**

- Context can become unwieldy when used in complex component trees.
- When multiple contexts are used in a single component tree, nesting providers becomes complex and hard to manage.
- Example of Context Hell:

```jsx
<AuthProvider>
  <ThemeProvider>
    <LanguageProvider>
      <MyComponent />
    </LanguageProvider>
  </ThemeProvider>
</AuthProvider>
```

- **Solution** : Use a custom provider wrapper or switch to libraries like Redux , Zustand for large-scale state management.

---

**2. Performance Issues :**

- When a context value changes, all consuming components re-render, even if only a part of the tree needs the update.
- This can lead to unnecessary re-renders and performance issues.
- **Solution** : Use React.useMemo() to memoize the context value and avoid unnecessary re-renders.

---

**3. Limited Debugging Tools :**

- Context API doesn't provide built-in debugging tools like React DevTools.
- Context debugging can become challenging without explicit tools compared to Redux.

---

# Final Advanced Exercise: Todo App with Context API

- **Goal** : Build a fully functional Todo App where the Context API handles the state for todos.

---

**Step 1: Create a Context File**

- File: TodoContext.js

```jsx
import React, { createContext, useState } from 'react';

export const TodoContext = createContext();

export const TodoProvider = ({ children }) => {
  const [todos, setTodos] = useState([]);

  const addTodo = (todo) => {
    setTodos([...todos, todo]);
  };

  const deleteTodo = (index) => {
    setTodos(todos.filter((_, i) => i !== index));
  };

  return (
    <TodoContext.Provider value={{ todos, addTodo, deleteTodo }}>
      {children}
    </TodoContext.Provider>
  );
};
```

---

**Step 2: Wrap the Application in the Provider**

- File: App.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import { TodoProvider } from "./TodoContext";

ReactDOM.render(
  <React.StrictMode>
    <TodoProvider>
      <App />
    </TodoProvider>
  </React.StrictMode>,
  document.getElementById("root")
);
```

---

**Step 3: Create the Todo Component and Consume the Context in a Component**

- File: Todo.jsx

```jsx
import React, { useContext, useState } from 'react';
import { TodoContext } from '../Context/TodoContext';

function Todo() {
  const { todos, addTodo, deleteTodo } = useContext(TodoContext);
  const [newTodo, setNewTodo] = useState('');

  const handleAddTodo = () => {
    if (newTodo.trim() !== '') {
      addTodo(newTodo);
      setNewTodo('');
    }
  };

  const handleAddBtn = (event) => {
    setNewTodo(event.target.value);
  };

  return (
    <div className="p-4 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">Todo App</h1>
      <div className="flex gap-2 mb-4">
        <input
          type="text"
          value={newTodo}
          onChange={handleAddBtn}
          placeholder="Add a new todo"
          className="flex-grow p-2 border rounded"
        />
        <button
          onClick={handleAddTodo}
          className="bg-green-500 text-white px-4 py-2 rounded"
        >
          Add
        </button>
      </div>
      <ul>
        {todos.map((todo, index) => (
          <li
            key={index}
            className="flex justify-between items-center mb-2 border-b pb-1"
          >
            {todo}
            <button
              onClick={() => deleteTodo(index)}
              className="text-red-500 hover:underline"
            >
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default Todo;

```

---

### Key Features of the Todo App
**1. Global State Management with Context API:**
- **addTodo** and **deleteTodo** are shared across all components.

**2. Dynamic Rendering :**
- The todo list is rendered based on the **todos** state.
- The list of todos updates in real-time as items are added or removed.

**3. Separation of Concerns :**
- The **TodoContext.jsx** file handles all state logic, making the main **App component** clean and focused.


---