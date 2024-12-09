# Props in React: Advanced Explanation

## 1. Advanced Usage of Props

**Example: Passing Functions as Props**
- You can pass functions as props to allow child components to communicate with parent components.

**Scenario :**
- We have a **TodoList** app where the parent manages the state of the todos, and a child component **(TodoItem)** can trigger a delete action.



**- Code Example :**
```jsx
import React, { useState } from 'react';

function TodoItem({ todo, onDelete }) {
  return (
    <li className="flex justify-between items-center">
      <span>{todo}</span>
      <button
        onClick={() => onDelete(todo)}
        className="bg-red-500 text-white px-4 py-1 rounded hover:bg-red-700"
      >
        Delete
      </button>
    </li>
  );
}

function TodoList() {
  const [todos, setTodos] = useState(['Learn React', 'Build a project', 'Master JavaScript']);

  const deleteTodo = (todo) => {
    setTodos(todos.filter((t) => t !== todo));
  };

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold mb-4">Todo List</h1>
      <ul>
        {todos.map((todo, index) => (
          <TodoItem key={index} todo={todo} onDelete={deleteTodo} />
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

---

#### **Explanation**
- **Parent (TodoList)** manages the state of todos.
- **Child (TodoItem)** receives two props:
    - todo (the string to display).
    - **onDelete** (a function to delete the specific todo).
- The child calls the **onDelete** function when the "Delete" button is clicked, passing the todo back to the parent.

This showcases how props enable interaction and dynamic behavior between components.

---

## 2. Complex Prop Structures
- You can pass objects or arrays as props to manage complex data structures.

**Scenario :**
- We are building a **UserProfile** component that displays nested data for a user.

**- Code Example :**
```jsx
function Address({ address }) {
  return (
    <div>
      <p>
        <span className="font-bold">Street:</span> {address.street}
      </p>
      <p>
        <span className="font-bold">City:</span> {address.city}
      </p>
      <p>
        <span className="font-bold">ZIP:</span> {address.zip}
      </p>
    </div>
  );
}

function UserProfile({ user }) {
  return (
    <div className="p-4 border rounded shadow-md max-w-sm">
      <h1 className="text-xl font-bold">{user.name}</h1>
      <p>
        <span className="font-bold">Email:</span> {user.email}
      </p>
      <h2 className="text-lg font-semibold mt-2">Address:</h2>
      <Address address={user.address} />
    </div>
  );
}

function App() {
  const user = {
    name: 'Jane Doe',
    email: 'jane.doe@example.com',
    address: {
      street: '123 Main St',
      city: 'Springfield',
      zip: '12345',
    },
  };

  return (
    <div className="flex justify-center items-center h-screen">
      <UserProfile user={user} />
    </div>
  );
}

export default App;
```

#### **Explanation**
- Nested Prop Passing :
    - The UserProfile component passes the address prop further down to the Address component.
- This structure handles deeply nested data and keeps components modular and focused on specific tasks.


--- 

## 3. Prop Drilling Hell

**The Problem :**
- Imagine an application where a parent component manages user authentication and needs to pass the ***user*** object to a deeply nested component. Without global state management, **prop drilling** is hell.

**Scenario :**
```jsx
function UserProfile({ user }) {
  return <h1>Welcome, {user.name}!</h1>;
}

function Sidebar({ user }) {
  return (
    <div className="w-64 h-full bg-gray-100">
      <UserProfile user={user} />
    </div>
  );
}

function Dashboard({ user }) {
  return (
    <div className="flex">
      <Sidebar user={user} />
      <div className="flex-grow p-4">Main Content</div>
    </div>
  );
}

function App() {
  const user = { name: 'John Doe', email: 'john.doe@example.com' };

  return <Dashboard user={user} />;
}

export default App;
```
##### Explanation of the Problem:
- The **user** prop is passed from the **App** component to **Dashboard**, then to **Sidebar**, and finally to **UserProfile**.
- Intermediate Components **(Sidebar, Dashboard)** don’t need the **user** prop but must pass it down, creating prop drilling.
    
---

## 4. Solving Prop Drilling Hell

**Solution 1: Using Context API**
- The **Context API** eliminates prop drilling by allowing you to share data directly between components, regardless of their nesting level.


**- Code Example :**
```jsx
import React, { createContext, useContext } from 'react';

const UserContext = createContext();

function UserProfile() {
  const user = useContext(UserContext);
  return <h1>Welcome, {user.name}!</h1>;
}

function Sidebar() {
  return (
    <div className="w-64 h-full bg-gray-100">
      <UserProfile />
    </div>
  );
}

function Dashboard() {
  return (
    <div className="flex">
      <Sidebar />
      <div className="flex-grow p-4">Main Content</div>
    </div>
  );
}

function App() {
  const user = { name: 'John Doe', email: 'john.doe@example.com' };

  return (
    <UserContext.Provider value={user}>
      <Dashboard />
    </UserContext.Provider>
  );
}

export default App;
```

**Explanation :**
1. Create a Context **(UserContext)**:
    - This is the global storage for the **user** data.

2. Wrap the Application in a Provider:
    - The **UserContext.Provider** supplies the **user** data to all components within its tree.

3. Consume the Data with **useContext** Hook:
    - Components like **UserProfile** can access the **user** data directly without needing intermediate props.


---

**Solution 2: Using State Management Libraries**
- For larger applications, libraries like **Redux, Zustand or Recoil** can manage the state globally, avoiding prop drilling and improving scalability.


