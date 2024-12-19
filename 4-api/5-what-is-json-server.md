# What is JSON Server?

**JSON Server** is a simple tool that allows you to create a mock REST API quickly and easily using a JSON file. It is widely used for prototyping, testing, or developing front-end applications without needing a fully functional backend.

---

#### Key Features of JSON Server:

1. **Mock API with Zero Configuration:**

- You only need a JSON file (e.g., **db.json, tasks.json, products.json**) to simulate a RESTful API.

2. **Supports REST API Features:**

- Perform CRUD operations using HTTP methods like **GET, POST, PUT, PATCH, and DELETE.**

3. **Extremely Lightweight:**

- JSON Server is installed as a Node.js package and can be run locally on your computer.

4. **Custom Routes and Middlewares:**

- You can extend its functionality using custom routes and middleware.

5. **Supports Query Parameters:**

- JSON Server can filter, paginate, and sort data using query parameters.

---

### How JSON Server Works

1. **Install JSON Server**: Install it using npm or yarn.
2. **Create a JSON file**: Define your database structure in a simple JSON file (e.g., db.json).
3. **Run JSON Server**: Start the server to expose RESTful endpoints automatically.

---

## Step-by-Step Usage

**1. Install JSON Server**

```bash
npm install -g json-server
```

This installs **JSON Server** globally so you can use it from anywhere on your system.

---

**2. Create a db.json File: Create a JSON file named db.json with sample data. For example:**

```json
{
  "users": [
    { "id": 1, "name": "Alice", "email": "alice@example.com" },
    { "id": 2, "name": "Bob", "email": "bob@example.com" }
  ],
  "posts": [
    { "id": 1, "title": "Hello World", "content": "This is the first post." },
    { "id": 2, "title": "JSON Server", "content": "Mocking APIs is easy!" }
  ],
  "comments": [
    { "id": 1, "postId": 1, "text": "Great post!" },
    { "id": 2, "postId": 2, "text": "Very helpful tool." }
  ]
}
```

- **posts** and **comments** represent resources.
- Each object in the array is treated as a resource instance.

---

**3. Run JSON Server**

- Start the server with the following command:

```bash
json-server --watch db.json --port 3000
```

By default, JSON Server runs on port 3000. It will create the following endpoints automatically:

| HTTP Method | Endpoint     | Description                   |
| ----------- | ------------ | ----------------------------- |
| GET         | **/posts**   | Fetch all posts.              |
| GET         | **/posts/1** | Fetch a specific post by ID.  |
| POST        | **/posts**   | Create a new post.            |
| PUT         | **/posts/1** | Update an entire post.        |
| PATCH       | **/posts**   | Update part of a post.        |
| DELETE      | **/posts/1** | Delete a specific post by ID. |

---

**4. Test the API**

- Open your browser and navigate to **http://localhost:3000/posts**. You should see the list of posts or use a tool like **Postman** to interact with the API.

---

### Using JSON Server with React
Here’s how to integrate JSON Server with a React app.

**Step 1: Fetching Data**
```jsx
import React, { useState, useEffect } from "react";

const UserList = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3000/users") // Fetch data from JSON Server
      .then((response) => response.json()) // Convert response to JSON
      .then((data) => setUsers(data)); // Set the fetched users in state
  }, []); // Run only once when the component mounts

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} ({user.email})
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UserList;
```


---

**Step 2: Adding a New User**
```jsx
import React, { useState } from "react";

const AddUser = () => {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  const handleAddUser = () => {
    const newUser = { name, email };
    fetch("http://localhost:3000/users", {
      method: "POST", // Use POST method
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newUser), // Convert user object to JSON
    })
      .then((response) => response.json())
      .then((data) => console.log("User added:", data));
  };

  return (
    <div>
      <h1>Add New User</h1>
      <input
        type="text"
        placeholder="Name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button onClick={handleAddUser}>Add User</button>
    </div>
  );
};

export default AddUser;
```

---

**Step 3: Deleting a User**
```jsx
import React, { useState, useEffect } from "react";

const DeleteUser = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("http://localhost:3000/users")
      .then((response) => response.json())
      .then((data) => setUsers(data));
  }, []);

  const handleDelete = (id) => {
    fetch(`http://localhost:3000/users/${id}`, {
      method: "DELETE", // Use DELETE method
    }).then(() => {
      setUsers(users.filter((user) => user.id !== id)); // Update state after deletion
    });
  };

  return (
    <div>
      <h1>User List</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} ({user.email})
            <button onClick={() => handleDelete(user.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default DeleteUser;
```

---
**Step 4: Updating a User Using PUT**
```jsx
import React, { useState, useEffect } from "react";

const UpdateUser = () => {
  const [users, setUsers] = useState([]);
  const [selectedUser, setSelectedUser] = useState(null);
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  // Fetch users
  useEffect(() => {
    fetch("http://localhost:3000/users")
      .then((response) => response.json())
      .then((data) => setUsers(data));
  }, []);

  // Handle selecting a user for editing
  const handleSelectUser = (user) => {
    setSelectedUser(user);
    setName(user.name);
    setEmail(user.email);
  };

  // Handle updating a user with PUT
  const handleUpdateUser = () => {
    const updatedUser = { id: selectedUser.id, name, email };

    fetch(`http://localhost:3000/users/${selectedUser.id}`, {
      method: "PUT", // Full update
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(updatedUser), // Replace the entire resource
    })
      .then((response) => response.json())
      .then((data) => {
        setUsers(
          users.map((user) => (user.id === data.id ? data : user)) // Update the user in the list
        );
        setSelectedUser(null);
        setName("");
        setEmail("");
      });
  };

  return (
    <div>
      <h1>Update User</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} ({user.email})
            <button onClick={() => handleSelectUser(user)}>Edit</button>
          </li>
        ))}
      </ul>

      {selectedUser && (
        <div>
          <h2>Editing {selectedUser.name}</h2>
          <input
            type="text"
            placeholder="Name"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <input
            type="email"
            placeholder="Email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
          <button onClick={handleUpdateUser}>Update User</button>
        </div>
      )}
    </div>
  );
};

export default UpdateUser;
```

**Step 5: Updating a User Using PATCH**
```jsx
import React, { useState, useEffect } from "react";

const UpdateUserPartial = () => {
  const [users, setUsers] = useState([]);
  const [selectedUser, setSelectedUser] = useState(null);
  const [email, setEmail] = useState("");

  // Fetch users
  useEffect(() => {
    fetch("http://localhost:3000/users")
      .then((response) => response.json())
      .then((data) => setUsers(data));
  }, []);

  // Handle selecting a user for partial editing
  const handleSelectUser = (user) => {
    setSelectedUser(user);
    setEmail(user.email);
  };

  // Handle updating a user's email with PATCH
  const handleUpdateEmail = () => {
    fetch(`http://localhost:3000/users/${selectedUser.id}`, {
      method: "PATCH", // Partial update
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ email }), // Only update the email field
    })
      .then((response) => response.json())
      .then((data) => {
        setUsers(
          users.map((user) => (user.id === data.id ? data : user)) // Update the user in the list
        );
        setSelectedUser(null);
        setEmail("");
      });
  };

  return (
    <div>
      <h1>Update User Email</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>
            {user.name} ({user.email})
            <button onClick={() => handleSelectUser(user)}>Edit Email</button>
          </li>
        ))}
      </ul>

      {selectedUser && (
        <div>
          <h2>Editing {selectedUser.name}</h2>
          <input
            type="email"
            placeholder="Email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
          />
          <button onClick={handleUpdateEmail}>Update Email</button>
        </div>
      )}
    </div>
  );
};

export default UpdateUserPartial;
```

## Summary
- **REST API:** A set of rules for building APIs.
- **JSON Server:** A tool to create mock APIs quickly with a JSON file.
- **Key Operations:** GET, POST, PUT, DELETE to interact with resources.
- **React Integration:** Use fetch() to perform API calls and manage state accordingly.
---