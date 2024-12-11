# What is a Global State?
**Definition**
- When building applications, you often need to share data **(state)** between multiple components. This is where global state management comes in.
- A **global state** is a shared state that multiple components in a React application can access and update. It is **"global"** because it is not tied to a single component—it is accessible across the **entire application**.
- Instead of passing **props** down through several layers of components **(prop drilling)**, **global state** lets you store data in one central location and access it from anywhere in your app.

---

### Why Use Global State?
In small apps, you can use React’s local state (e.g., **useState**) within individual components. However, as apps grow, managing shared data between components becomes challenging. Here are scenarios where global state helps:
1. **Shared Data**: When multiple components need access to the same data (e.g., user authentication status, theme, cart items).
2. **Complex State Logic**: When state management logic becomes too complex to maintain in individual components.
3. **Avoiding Prop Drilling**: Instead of passing props down multiple component layers, you can store data in a global state.

---

### Global State Management Libraries
React doesn’t have built-in support for global state management. Libraries like **Redux**, **Recoil**, and **Zustand** help manage global states efficiently. Among these, **Zustand** is lightweight, easy to use, and perfect for beginners.

---



