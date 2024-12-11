# Understanding Zustand

**Zustand** is a simple and lightweight library for managing **global state** in React applications. If you’re new to React and Zustand, this guide will walk you through everything step by step in clear, beginner-friendly language.

---

### What is Zustand?

- **Zustand** is a state management library for React, meaning it helps manage and share **data (state)** across your app's components.
- It’s lightweight, simple, and faster compared to libraries like **Redux**.
- Zustand avoids complex boilerplate and is very beginner-friendly.

---

#### Why Use Zustand?

1. **Global State Sharing** : It makes it easy to share state (like user data, theme settings, etc.) between multiple components.
2. **No Prop Drilling** : You don’t need to pass data through multiple layers of components manually.
3. **Ease of Use** : Simple APIs to read, update, and reset state.
4. **Flexibility** : Works with functional components using hooks.

---

#### How Zustand Works

Zustand operates using the following three concepts:

1. **Store** : This is where you define your state and the functions (actions) to update it.
2. **Hook** : A function provided by Zustand to access your store's state and actions.
3. **React Component** : Your React components can connect to the store using Zustand hooks.

---

## How can I use Zustand?

#### 1. **Install Zustand**

- First, you need to install Zustand in your React project:

```jsx
npm install zustand
```

#### 2. **Set Up the Zustand Store**

- A store in **Zustand** is a place where you define all the state and functions that update that state. You can create a simple store with a few lines of code.

**Example: Create a Counter Store**

1. Import create from Zustand.
2. Define the initial state and methods to manipulate the state.

```jsx
// store.js
import { create } from "zustand";

const useCounterStore = create((set) => ({
  count: 0, // Initial state
  increaseCount: (value) => set((state) => ({ count: state.count + value })), // function to increase count
  decreaseCount: (value) => set((state) => ({ count: state.count - value })), // function to decrease count
  resetCount: () => set({ count: 0 }), // Reset
}));

export default useCounterStore;
```

**Explanation:**

- **create** : This function is used to create the store.
- **count** : The state variable.
- **set** : A function that allows you to update the state.
- **state** : is the object that contains the current values of all your state properties in the store.
- **state.count** accesses the current value of count in the store.
- **increincreaseCountase and decreaseCount**: Functions that update the count state.

1. Initial State :

- **count** is set to **0** initially.

2. Accessing **state** :

- When increaseCount or decreaseCount is called, Zustand passes the current state to the function.
- **state.count** lets you access the current value of count.

---

#### 3. **Use the Store in Your Components**

- Now that we’ve created the store, we can use it in our React components.
- Use the store's state and methods in your React components by calling the it's custom hook.

**Example: Build a Counter Component**

```jsx
import React from "react";
import useCounterStore from "./store"; // Import the store

const Counter = () => {
  // Access state and actions from the store
  const { count, increaseCount, decreaseCount, resetCount } = useCounterStore();

  // Define handlers for the actions
  const handleIncrement = () => increaseCount(1);
  const handleDecrement = () => decreaseCount(1);
  const handleReset = () => resetCount();

  return (
    <div className="flex flex-col items-center">
      <h1 className="text-2xl font-bold">Count: {count}</h1>
      <div className="flex space-x-4 mt-4">
        <button
          className="px-4 py-2 bg-blue-500 text-white"
          onClick={handleIncrement}
        >
          Increment
        </button>
        <button
          className="px-4 py-2 bg-red-500 text-white"
          onClick={handleDecrement}
        >
          Decrement
        </button>
        <button
          className="px-4 py-2 bg-gray-500 text-white"
          onClick={handleReset}
        >
          Reset
        </button>
      </div>
    </div>
  );
};

export default Counter;
```

---

#### 4. Connect Zustand to More Complex Applications

- Now that you understand the basics, let’s build a **to-do list** app to show how to use Zustand for more complex state management.

**Example: Todo App with Zustand**

- **Create the Todo Store (store.js):**

```jsx
import { create } from "zustand";

// Create the store for todos
const useTodoStore = create((set) => ({
  todos: [],
  addTodo: (todo) => set((state) => ({ todos: [...state.todos, todo] })), // Add new todo
  removeTodo: (id) =>
    set((state) => ({
      todos: state.todos.filter((todo) => todo.id !== id), // Remove todo by id
    })),
}));

export default useTodoStore;
```

- **App.js (Component using the store):**

```jsx
import React, { useState } from "react";
import useTodoStore from "./todoStore";

const TodoApp = () => {
  const { todos, addTodo, removeTodo } = useTodoStore();
  const [newTodo, setNewTodo] = useState("");

  const handleAddTodo = () => {
    if (newTodo.trim()) {
      addTodo(newTodo.trim());
      setNewTodo("");
    }
  };

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold">To-Do List</h1>
      <input
        className="border px-2 py-1 my-2"
        type="text"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
        placeholder="Add a new task"
      />
      <button
        className="px-4 py-2 bg-green-500 text-white"
        onClick={handleAddTodo}
      >
        Add
      </button>
      <ul className="mt-4">
        {todos.map((todo, index) => (
          <li key={index} className="flex justify-between border-b py-1">
            {todo}
            <button className="text-red-500" onClick={() => removeTodo(id)}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TodoApp;
```

1. **newTodo (local state)** is used to manage the input value.
2. **addTodo** : Adds a new todo to the list.
3. **removeTodo** : Removes a todo from the list by its id.

---

### Advanced Features of Zustand

###### **Persist State**

- Zustand supports **persistent state** using the **persist** middleware.
- Save state to localStorage or sessionStorage to make it persist across page reloads.

###### Understanding Persist State in Zustand

- When you use Zustand to manage state in a React application, the state resets when you reload the page. Persisting state ensures that your app remembers its state across page reloads by saving it to **localStorage** or **sessionStorage**.

---

###### Why Persist State?

- Persisting state can be useful in scenarios where you want to keep data across page reloads, such as user authentication, shopping cart items, or form data.
- When your app reloads, its state (like user input or count values) resets. Persisting state means saving this data to the browser’s **localStorage** or **sessionStorage** so that it doesn't get lost on page reloads.

1. **Remembers User Data** : For example, keeping a user's cart items, preferences, or form inputs after refreshing.
2. **Improves User Experience** : The user doesn't lose their progress or settings on a reload.
3. **Easy Setup with Zustand Middleware** : Zustand makes persisting state very simple using the persist middleware.

---

###### How to Use Persist State in Zustand:

**Step 1: Create a Store with Persistence**

- Use the **persist** function to define a store that automatically saves and loads its state.

**Example: Counter with Persisted State**

```jsx
// File: useCounterStore.js
import { create } from "zustand";
import { persist } from "zustand/middleware";

const useCounterStore = create(
  persist(
    (set) => ({
      count: 0, // Initial state value
      increment: () => set((state) => ({ count: state.count + 1 })), // Function to increase count
      decrement: () => set((state) => ({ count: state.count - 1 })), // Function to decrease count
      reset: () => set({ count: 0 }), // Function to reset count to 0
    }),
    {
      name: "counter-storage", // Key used to save in localStorage
    }
  )
);

export default useCounterStore;
```

**What’s Happening Here?**

1. **persist:**
   - Wraps the store logic.
   - Saves state to **localStorage** automatically.
2. **name: "counter-storage":**
   - Specifies the key under which the state is stored in **localStorage**.

---

**Step 2: Access the Store in a Component**

- Use the store in your components just like any other normal Zustand store.
  **File: Counter.js**

```jsx
import React from "react";
import useCounterStore from "../Store/useCounterStore"; // Import the store

const Counter = () => {
  const { count, increment, decrement, reset } = useCounterStore(); // Access state and actions

  return (
    <div className="app">
      <h1>Counter: {count}</h1> {/* Display the count */}
      <button onClick={increment}>Increase</button> {/* Increase count */}
      <button onClick={decrement}>Decrease</button> {/* Decrease count */}
      <button onClick={reset}>Reset</button> {/* Reset count */}
    </div>
  );
};

export default Counter;

```

---

**Step 3: Run Your App**
1. Start your app using npm start.
2. Open the counter page.
3. Use the buttons to **increase, decrease, or reset the counter**.
4. Refresh the page. Notice that the counter value remains the same, even after reloading.

---

**Step 4: Check localStorage**
1. Open your browser’s **Developer Tools** (press F12 or right-click → Inspect).
2. Go to the **Application tab**.
3. Under **Local Storage, look for your app's URL**.
4. You’ll see a key named **counter-storage**. Its value contains the saved state.

---

###### How Does This Work?
1. The **persist** Middleware:
- Saves the state to localStorage automatically whenever it changes.
- Loads the saved state when the app starts.
2. The **name** Option:
- Specifies the key used in **localStorage** (in this case, **counter-storage**).
3. **Automatic State Loading** :
- When the page reloads, Zustand fetches the saved data from **localStorage** and applies it to the store.

----

### Advanced Example: Persisting State in sessionStorage
- If you want the state to only persist while the browser tab is open (and clear it after closing), use **sessionStorage** instead of **localStorage**.
- Here’s how to do it: File: **useCounterStore.jsx**
```jsx
import { create } from "zustand";
import { persist } from "zustand/middleware";

const useCounterStore = create(
  persist(
    (set) => ({
      count: 0,
      increment: () => set((state) => ({ count: state.count + 1 })),
      decrement: () => set((state) => ({ count: state.count - 1 })),
      reset: () => set({ count: 0 }),
    }),
    {
      name: "counter-session-storage", // Key for sessionStorage
      storage: sessionStorage, // Use sessionStorage instead of localStorage
    }
  )
);

export default useCounterStore;
```
- Everything else stays the same in your component. This will now save the state in **sessionStorage**, which resets after closing the browser tab.

---

**Key Takeaways**
1. Persist State is a feature to save Zustand state in the browser’s storage.
2. **persist** Middleware makes it easy to save and load state automatically.
3. Use **localStorage** for permanent persistence or **sessionStorage** for temporary persistence.
4. Customize which parts of the state are saved or run code on state load.

----

