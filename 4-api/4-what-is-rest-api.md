# What is a REST API?
**REST** stands for **Representational State Transfer**. A **REST API** is a type of API that follows the principles of REST. 

It uses standard HTTP methods (like **GET, POST, PUT, DELETE**) to interact with resources, typically over the web.

- REST APIs are stateless, meaning each request from a client to the server must contain all the information needed to understand and process the request.
- Data formats like JSON or XML are commonly used to exchange data between the client and server.

---

##### Example usage of a REST API
Let’s consider a REST API for a blog platform where users can create posts.

**Base URL:**

```url
https://example.com/api
```

**Endpoints:**
| HTTP Method	    | Endpoint	 | Description |
| -------- | ------- | ------- |
|GET	  | **/posts**    | Fetch all posts. |
|GET	  | **/posts/1**    | Fetch a specific post by ID. |
|POST	  | **/posts**    | Create a new post. |
|PUT	  | **/posts/1**    | Update an entire post. |
|PATCH	  | **/posts**    | Update part of a post. |
|DELETE	  | **/posts/1**    | 	Delete a specific post by ID. |

---

## Using REST API in React

Fetching Data from a REST API:
```jsx
import React, { useState, useEffect } from "react";

function BlogPosts() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    // Fetch data from REST API
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((response) => response.json())
      .then((data) => setPosts(data)); // Set fetched data to state
  }, []); // Runs only once after the component is mounted

  return (
    <div>
      <h1>Blog Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default BlogPosts;
```

**Explanation:**
1. **Endpoint:**
- The URL **https://jsonplaceholder.typicode.com/posts** represents the **GET /posts endpoint.**


2. **fetch:**
- Sends a request to the endpoint and retrieves the data.

3. **Response Parsing:**
- **response.json()** converts the JSON response into a JavaScript object.

4. **State Update:**
- The retrieved data is stored in the posts **state** using **setPosts.**

---

### Sending Data to a REST API:
Here’s how you can create a new post using the POST method:

```jsx
function CreatePost() {
  const createPost = () => {
    const newPost = { title: "New Post", body: "This is a new post." };

    fetch("https://jsonplaceholder.typicode.com/posts", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(newPost),
    })
      .then((response) => response.json())
      .then((data) => {
        console.log("Post created:", data);
      });
  };

  return <button onClick={createPost}>Create Post</button>;
}

export default CreatePost;

```

**Explanation:**
1. **Endpoint:**
- The URL **https://jsonplaceholder.typicode.com/posts** is the **POST /posts endpoint.**
2. **Request Body:**
- **newPost** contains the data to be sent to the server.
3. **JSON.stringify:**
- Converts the **JavaScript object** into a **JSON string**.
4. **Response Parsing:**
- **response.json()** converts the JSON response into a JavaScript object.
5. **Headers:**
- **Content-Type: application/json** informs the server that the data is in **JSON format.**
6. **Response Handling:**
- The response from the server (e.g., the created post) is logged to the console.

---

##### Advantages of REST APIs:

1. **Scalability:** REST APIs are designed to be **scalable** and can handle large amounts of data efficiently.
2. **Flexibility:** REST APIs are **flexible** and can be easily adapted to different use cases.
3. **Standardization:** REST APIs are **standardized** and can be easily consumed by other systems or services.
4. **Security:** REST APIs are **secure** and can provide **authentication** and **authorization** features.
5. **Performance:** Efficient use of HTTP methods and caching mechanisms.

----

#### Summary:
- A REST API provides a structured way to perform CRUD operations on resources over the web.
- It uses standard HTTP methods and URLs to interact with resources.
- REST APIs are lightweight, flexible, and widely used in modern applications.
- In React, you can fetch or send data using tools like fetch or Axios or SWR to interact with REST APIs.

---





