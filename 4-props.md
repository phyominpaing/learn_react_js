# What are Props?

- **Props** (short for **properties**) are a mechanism in React for passing data from a **parent component** to a **child component**.
- **Props** are a way to pass data from a **parent component** to a **child component**, enabling communication between them.
- They allow you to make components reusable by customizing their behavior or appearance based on the passed data.

In essence, props are the inputs to a React component. They are immutable, meaning a component cannot change the props it receives.

---

## How Props Work:

1. **Passing Props:**

- A **parent component** passes data to a **child component** using props.
- Example:

```jsx
import React from "react";

function ParentComponent() {
  return (
    <div>
      <ChildComponent message="Hello, World!" />
    </div>
  );
}

function ChildComponent(props) {
  return (
    <div>
      <h2>{props.message}</h2>
    </div>
  );
}
```

---

## Key Features of Props:

### 1. Read-Only :

    - Props are immutable, meaning a component cannot change the props it receives.
    - Once passed to a component, they cannot be modified within that component.

### 2. Reusability :

    - Props enable you to create reusable components by customizing their behavior or appearance based on the passed data.
    - Props make components reusable by allowing different data to be passed in.

### 3. Unidirectional Data Flow:

    - Props follow a top-down data flow, meaning data flows from the parent component to its child components.

### 4. Data Binding:

    - Props are a way to pass data from a parent component to a child component and bind it to the component's state or props.
    - This allows you to create dynamic and interactive components.
    - That's type of data binding is called **one way data binding**.

---

# How Props are Used

## 1. Passing Props:

- Props are passed to a component as attributes in JSX.
- Example :

```jsx
function App() {
  return <Greeting name="John" />;
}

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}
```

- Explanation :
- The Parent **App component** passes a name prop with the value "John" to the **child Greeting component**.
- Inside Greeting, props.name is used to access the value.

---

## 2. Destructuring Props:

- Instead of accessing props.name, you can destructure props for cleaner code.
- Example :

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

---

## 3. Default Props:

- You can specify default values for props if none are provided.
- Example :

```jsx
function Button({ label = "Click Me" }) {
  return <button>{label}</button>;
}

function App() {
  return <Button />;
}
```

- Output :
- The button will display Click Me because no label prop was passed.

---

## Simple Example: Passing Static Data

#### **Scenario: A Parent Sends a Name to a Child Component**

**- Parent Component (App):**

```jsx
function App() {
  return <Greeting name="Alice" />;
}

export default App;
```

**- Child Component (Greeting):**

```jsx
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```

**How It Works**

1. The **App component (parent)** passes a name prop with the value "Alice" to the **Greeting component (child)**.
2. The child accesses the value using props.name.

---

## Improving Readability: Destructuring Props

**You can destructure props directly in the function signature for cleaner code.**

**- Child Component (Greeting):**

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

---

## Default Props: Specifying Default Values

**You can specify default values for props if none are provided.**

**- Child Component (Greeting):**

```jsx
function Greeting({ name = "World" }) {
  return <h1>Hello, {name}!</h1>;
}
```

---

## Adding More Props

#### **Scenario: Passing Multiple Props**

- Now we include an age prop in addition to name.

**- Parent Component (App):**

```jsx
function App() {
  return <UserInfo name="Alice" age={25} />;
}

export default App;
```

**- Child Component (UserInfo):**

```jsx
function UserInfo({ name, age }) {
  return (
    <div>
      <h1>Name: {name}</h1>
      <p>Age: {age}</p>
    </div>
  );
}

export default UserInfo;
```

---

## Passing Dynamic Data

- Props can also pass **dynamic data** such as variables or state.

**- Parent Component (App):**

```jsx
import React, { useState } from "react";

function App() {
  const [user, setUser] = useState({ name: "Bob", age: 30 });

  return <UserInfo name={user.name} age={user.age} />;
}

export default App;
```

**How It Works:**

- The **App** component stores user data in its state.
- It dynamically passes **name** and **age** to the UserInfo component as props.

---

## Passing Functions as Props

#### **Scenario: Child Component Communicates with Parent**

- Props can pass functions, enabling a child component to trigger actions in its parent.

**- Parent Component :**

```jsx
function App() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <ActionButton onClick={handleClick} />;
}

export default App;
```

**- Child Component (ActionButton) :**

```jsx
function ActionButton({ onClick }) {
  return (
    <button
      onClick={onClick}
      className="bg-blue-500 text-white px-4 py-2 rounded"
    >
      Click Me
    </button>
  );
}

export default ActionButton;
```

**How It Works:**

1. The **App** component passes the **handleClick** function as a prop **(onClick)** to **ActionButton**.
2. The **ActionButton** calls the function when the button is clicked.

---

## Rendering Lists Using Props

#### **Scenario: Passing an Array as Props**

**- Parent Component :**

```jsx
function App() {
  const users = ["Alice", "Bob", "Charlie"];

  return <UserList users={users} />;
}

export default App;
```

**- Child Component (ActionButton) :**

```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map((user, index) => (
        <li key={index}>{user}</li>
      ))}
    </ul>
  );
}

export default UserList;
```

**How It Works:**

1. The **App** component passes an array of users as a prop **(users)** to **UserList**.
2. The parent passes an array of users to the **UserList** component.
3. The child maps through the array to display each user as a list item.

---

## Advanced: Nested Components with Props

#### **Scenario: Passing Nested Data**

**- Parent Component :**

```jsx
function App() {
  const user = {
    name: "Alice",
    address: {
      street: "123 Main St",
      city: "Wonderland",
    },
  };

  return <UserProfile user={user} />;
}

export default App;
```

**- Child Component (UserProfile) :**

```jsx
function UserProfile({ user }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <Address address={user.address} />
    </div>
  );
}

export default UserProfile;
```

**- Grandchild Component (Address) :**

```jsx
function Address({ address }) {
  return (
    <p>
      {address.street}, {address.city}
    </p>
  );
}

export default Address;
```

---

## Prop Drilling Hell

#### **Scenario: Passing Data Across Many Levels**

- When props are passed through multiple intermediate components, prop drilling occurs.

- Example :

```jsx
function App() {
  const user = { name: "Alice" };

  return <Parent user={user} />;
}


function Parent({ user }) {
  return <Child user={user} />;
}


function Child({ user }) {
  return <GrandChild user={user} />;
}


function GrandChild({ user }) {
  return <h1>Welcome, {user.name}!</h1>;
}

export default App;
```

**Problems**
- Each intermediate component (Parent, Child) must pass the user prop even if it doesn’t use it.
- This makes the code harder to maintain.
    
---

### Solutions to Prop Drilling Hell

**1. Context API:**
    - React’s built-in Context API allows you to share data across components without prop drilling.
- Example :

```jsx
import React, { createContext, useContext } from 'react';

const UserContext = createContext();

function Child() {
  const user = useContext(UserContext);
  return <p>User: {user}</p>;
}

function App() {
  return (
    <UserContext.Provider value="John Doe">
      <Child />
    </UserContext.Provider>
  );
}
```

**2. State Management Libraries:**
    - Tools like Redux, Recoil, or Zustand allow you to manage global state efficiently and avoid prop drilling.


**3. Component Composition:**
    - Use higher-order components (HOCs) or render props to encapsulate logic and avoid passing props explicitly. **(rare)**


---

## Avoiding Prop Drilling with Context API

#### **Solution: Context API** 

**Code Example:**

```jsx
import React, { createContext, useContext } from 'react';

const UserContext = createContext();

function GrandChild() {
  const user = useContext(UserContext);
  return <h1>Welcome, {user.name}!</h1>;
}

function App() {
  const user = { name: "Alice" };

  return (
    <UserContext.Provider value={user}>
      <GrandChild />
    </UserContext.Provider>
  );
}

export default App;
```

**How It Works:**
1. **UserContext.Provider** shares the **user** data globally.
2. **GrandChild** accesses **user** using the **useContext** hook, eliminating prop drilling.

---

### Summary of Key Points
- Props are immutable and are used to pass data from parent to child components.
- You can pass any data type as props, including strings, numbers, objects, arrays, and functions.
- Prop drilling can become problematic in deeply nested components but can be mitigated using the Context API or state management libraries.

---