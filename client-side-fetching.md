# Client-Side Fetching

### **Table of Contents**

1. Introduction
2. What is Client-Side Fetching?
3. How Client-Side Fetching Works in Next.js 13+
4. Example: Fetching Data on the Client Side
5. When to Use Client-Side Fetching?
6. Pros and Cons of Client-Side Fetching
7. Summary

***

### **1. Introduction**

1. In Next.js, there are multiple ways to fetch data, including **Server-Side Rendering (SSR)**, **Static Site Generation (SSG)**, and **Client-Side Fetching**.
2. Client-Side Fetching is used when data needs to be fetched **after the page is loaded** in the user’s browser.

***

### **2. What is Client-Side Fetching?**

#### 🔹 **Definition:**

* Client-Side Fetching means **fetching data inside a React component after the page has loaded**.
* The request is made from the user's browser instead of the server.
* It’s useful for **dynamic data that updates frequently** and doesn’t need to be pre-rendered.

#### 🔹 **Key Features:**

✔ **Data is fetched after the page loads**\
✔ **Useful for user-specific or real-time data**\
✔ **Doesn't affect build time**\
✔ **Data is not SEO-friendly since it loads after rendering**

***

### **3. How Client-Side Fetching Works in Next.js 13+?**

In Next.js 13+, Client-Side Fetching is usually done inside a **Client Component** using the **useEffect** or **React Query**.

***

### **4. Example: Fetching Data on the Client Side**

#### **Example: Fetching a List of Users**

📁 **app/users/page.js**

```javascript
"use client"; // This is a client component

import { useState, useEffect } from "react";

export default function UsersPage() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/users");
        const data = await response.json();
        setUsers(data);
      } catch (error) {
        console.error("Error fetching users:", error);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();
  }, []);

  if (loading) return <p>Loading users...</p>;

  return (
    <div>
      <h1>Users List</h1>
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

#### **How This Works:**

✅ **Page loads first**, then fetches data from the API.\
✅ **Data is updated dynamically in the component state**.\
✅ **If API takes time, a loading message is shown**.\
✅ **Perfect for fetching data that changes often**.

***

### **5. When to Use Client-Side Fetching?**

| Scenario                              | Use Client-Side Fetching? |
| ------------------------------------- | ------------------------- |
| Fetching user profile details         | ✅ Yes                     |
| Fetching live sports scores           | ✅ Yes                     |
| Displaying notifications in real-time | ✅ Yes                     |
| Fetching product details (for SEO)    | ❌ No (Use SSR or SSG)     |
| Fetching blog posts                   | ❌ No (Use SSG or ISR)     |

***

### **6. Pros and Cons of Client-Side Fetching**

| Pros                                    | Cons                                               |
| --------------------------------------- | -------------------------------------------------- |
| No impact on server build time          | Not SEO-friendly (data loads after page rendering) |
| Works well for frequently changing data | Increases initial page load time                   |
| Ideal for personalized user data        | More API calls from client devices                 |

***

### **7. Summary**

* **Client-Side Fetching loads data in the user's browser after the page is rendered**.
* **It's useful for dynamic or real-time data**, like user profiles, notifications, or live updates.
* **Implemented using `useEffect` in Client Components**.
* **Not good for SEO, as content is not available in the initial HTML**.
* **Best for interactive, frequently changing data that doesn’t need pre-rendering**.
