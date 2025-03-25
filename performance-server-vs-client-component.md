# Performance: Server Vs Client Component

### **Table of Contents**

1. **Introduction to Server and Client Components**
2. **Key Differences Between Server and Client Components**
3. **Performance Impact: Server vs. Client Components**
4. **When to Use Server Components?**
5. **When to Use Client Components?**
6. **Example: Server vs. Client Components in Action**
7. **Best Practices for Performance Optimization**
8. **Conclusion**

***

### **1. Introduction to Server and Client Components**

In **Next.js 13+ (with App Router)**, React introduced **Server Components** alongside **Client Components**.

🔹 **Server Components**: Rendered on the server, reducing JavaScript sent to the browser.\
🔹 **Client Components**: Rendered on the browser, allowing interactivity (like state and event handling).

🚀 **Why does this matter?**\
Choosing **Server or Client Components** impacts **performance, SEO, and user experience**.

***

### **2. Key Differences Between Server and Client Components**

| Feature                                                | **Server Components**                       | **Client Components**                     |
| ------------------------------------------------------ | ------------------------------------------- | ----------------------------------------- |
| **Where it runs**                                      | On the server                               | In the browser                            |
| **JS bundle size**                                     | 🚀 Smaller                                  | 🐢 Larger                                 |
| **SEO-friendly**                                       | ✅ Yes                                       | ❌ No                                      |
| **Interactivity (useState, useEffect, onClick, etc.)** | ❌ No                                        | ✅ Yes                                     |
| **API calls**                                          | ✅ Can fetch data on the server              | ✅ Can fetch data in the browser           |
| **Performance**                                        | ⚡ Faster for static pages                   | 🔥 Required for interactive pages         |
| **Re-renders**                                         | 🚀 Less frequent (fetch once on the server) | 🔄 More frequent (client-side re-renders) |

***

### **3. Performance Impact: Server vs. Client Components**

#### **🚀 Server Components (Better Performance)**

✅ **Less JavaScript sent to the client** → Faster load times\
✅ **Reduced client-side rendering** → No hydration needed\
✅ **Better SEO** → Content is available before the page loads\
✅ **No unnecessary API calls in the browser**

#### **🐢 Client Components (Slower Performance)**

❌ **More JavaScript sent to the client** → Increases page load time\
❌ **More hydration required** → Slower Time-to-Interactive (TTI)\
❌ **More re-renders in React state updates** → Can cause performance issues

***

### **4. When to Use Server Components?**

Use Server Components when:\
✅ You **don’t need interactivity** (static content, product listings, blog posts).\
✅ You want **better performance and SEO** (e.g., content-heavy pages).\
✅ You **fetch data from a database** (fetching happens before rendering).\
✅ You need to **reduce client-side JavaScript size**.

***

### **5. When to Use Client Components?**

Use Client Components when:\
✅ You need **interactivity** (forms, buttons, modals, animations).\
✅ You need **state management** (useState, useEffect, useContext).\
✅ You have **user-specific UI changes** (e.g., theme switchers, dashboards).\
✅ You handle **real-time updates** (e.g., WebSockets, live chats).

***

### **6. Example: Server vs. Client Components in Action**

#### **🖥️ Server Component Example (Faster Load Time)**

📌 **Fetching data on the server (less JavaScript sent to client)**

```tsx
// app/page.tsx (Server Component)
async function getData() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts");
  return res.json();
}

export default async function Page() {
  const posts = await getData(); // Fetches data before rendering

  return (
    <div>
      <h1>Blog Posts</h1>
      <ul>
        {posts.map((post: any) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```

🔹 **No extra JavaScript** needed on the client → **Faster page load**\
🔹 **SEO-friendly** → Content is available on the server

***

#### **🖱️ Client Component Example (Interactive but Slower)**

📌 **Client-side interactivity with a button click**

```tsx
"use client"; // This must be a Client Component

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

🔹 **JavaScript must be sent to the client** → Increases page load time\
🔹 **Required for interactivity** → Can't use Server Components

***

### **7. Best Practices for Performance Optimization**

✅ **Use Server Components by default** → Reduces JavaScript sent to the browser\
✅ **Only use Client Components when necessary** → Avoid bloating the client bundle\
✅ **Fetch data on the server instead of the client** → Improves page speed\
✅ **Use Suspense and Streaming for better performance** → Helps load parts of the page faster

***

### **8. Conclusion**

🔹 **Server Components** are great for performance and SEO (static pages, blogs, product listings).\
🔹 **Client Components** are needed for interactivity (buttons, forms, modals).\
🔹 **Mixing both wisely** gives the best results in Next.js 13+.
