# React Query in Next JS

### **Table of Contents**

1. Introduction
2. What is React Query?
3. Why Use React Query?
4. Setting Up React Query in Next.js 13+
5. Example: Fetching Data with React Query
6. Benefits of React Query
7. Summary

***

### **1. Introduction**

Fetching data in Next.js can be done in multiple ways, such as **Server-Side Rendering (SSR), Static Site Generation (SSG), and Client-Side Fetching**.\
For efficient **client-side fetching**, React Query is a powerful library that **handles caching, background refetching, and state management** automatically.

***

### **2. What is React Query?**

**React Query** is a data-fetching library that makes API calls **easier and more efficient** by:\
✔ Handling **caching** automatically\
✔ **Refetching** data in the background\
✔ Managing **loading and error states**\
✔ Reducing **API calls** by reusing cached data

***

### **3. Why Use React Query?**

| Feature              | React Query | Traditional useEffect |
| -------------------- | ----------- | --------------------- |
| Caching              | ✅ Yes       | ❌ No                  |
| Automatic Refetching | ✅ Yes       | ❌ No                  |
| Background Updates   | ✅ Yes       | ❌ No                  |
| Manual API Calls     | ❌ No        | ✅ Yes                 |
| Easy to Use          | ✅ Yes       | ❌ No                  |

**React Query is better than `useEffect` for API calls because it reduces redundant API requests and improves performance.**

***

### **4. Setting Up React Query in Next.js 13+**

To use React Query in a Next.js 13+ app, install it first:

```sh
npm install @tanstack/react-query
```

Next, create a **React Query Provider** to wrap your application.

📁 **app/providers.js**

```javascript
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { useState } from "react";

export default function Providers({ children }) {
  const [queryClient] = useState(() => new QueryClient());

  return <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>;
}
```

Then, wrap your application with the provider:

📁 **app/layout.js**

```javascript
import Providers from "./providers";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

Now React Query is ready to use in your app! 🚀

***

### **5. Example: Fetching Data with React Query**

Let's fetch a list of users using React Query instead of `useEffect`.

📁 **app/users/page.js**

```javascript
"use client"; // This is a client component

import { useQuery } from "@tanstack/react-query";

export default function UsersPage() {
  const { data: users, error, isLoading } = useQuery({
    queryKey: ["users"],
    queryFn: async () => {
      const response = await fetch("https://jsonplaceholder.typicode.com/users");
      if (!response.ok) throw new Error("Failed to fetch users");
      return response.json();
    },
  });

  if (isLoading) return <p>Loading users...</p>;
  if (error) return <p>Error: {error.message}</p>;

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

✔ **`useQuery` automatically fetches and caches data**\
✔ **Handles loading and error states automatically**\
✔ **Prevents duplicate API calls by reusing cached data**\
✔ **Supports background refetching for fresh data**

***

### **6. Benefits of React Query**

| Feature               | Benefit                                 |
| --------------------- | --------------------------------------- |
| Automatic Caching     | No need to store API responses manually |
| Background Refetching | Keeps data updated without page reload  |
| Error Handling        | Handles API failures easily             |
| Data Synchronization  | Works well with real-time applications  |
| Optimized Performance | Reduces unnecessary API calls           |

***

### **7. Summary**

* **React Query is a better alternative to `useEffect` for API fetching**
* **It improves performance with caching and background updates**
* **Easy to integrate with Next.js 13+ using a provider**
* **Handles loading and error states automatically**
* **Best for client-side fetching in dynamic applications**
