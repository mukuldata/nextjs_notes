# Progressive rendering with Suspense

### **Table of Contents**

1. **Introduction to Progressive Rendering**
2. **What is React Suspense?**
3. **How Progressive Rendering Works with Suspense**
4. **Why Use Progressive Rendering?**
5. **Example: Progressive Rendering with Suspense in Next.js**
6. **Benefits of Progressive Rendering with Suspense**
7. **Best Practices for Using Progressive Rendering**
8. **Conclusion**

***

### **1. Introduction to Progressive Rendering**

🔹 **Progressive Rendering** is a technique where a web page loads **in parts**, instead of waiting for everything to be ready before displaying content.\
🔹 It improves **user experience** by showing **important content first** while slower parts load later.\
🔹 Next.js **13+ and 14** support **Progressive Rendering with Suspense**, allowing developers to load **slow components separately**.

✅ **Example:**\
Imagine an e-commerce page:\
✔ **Product title & price load instantly** (important content).\
✔ **Reviews & recommendations load later** (less critical).

This ensures users **see important information first** while the rest loads in the background!

***

### **2. What is React Suspense?**

🔹 **React Suspense** is a feature that allows **delaying the rendering of specific parts of a page** while showing a **fallback UI** (like a loading spinner).\
🔹 It works great with **Progressive Rendering** because it lets pages load in steps.

✅ **How Suspense Works:**

* **Without Suspense:** The entire page **waits** for all data before rendering.
* **With Suspense:** The page **renders instantly**, and slow parts appear later.

***

### **3. How Progressive Rendering Works with Suspense**

🔹 Next.js **streams content progressively** using **Suspense**.\
🔹 **Components wrapped in `<Suspense>`** load separately from the rest of the page.\
🔹 **Fallback UI** (like "Loading...") is shown until the content is ready.

✅ **Step-by-Step Process:**\
1️⃣ **Server renders the page** and sends **static content first**.\
2️⃣ **Suspense waits for slow content** while showing a **loading placeholder**.\
3️⃣ **Slow content loads progressively**, replacing the placeholder.

📌 **This improves perceived performance because users see content faster!**

***

### **4. Why Use Progressive Rendering?**

🚀 **Benefits of Progressive Rendering:**\
✔ **Faster Page Load** – Important content appears first.\
✔ **Better User Experience** – Users don’t have to wait for everything.\
✔ **Reduced TTFB (Time to First Byte)** – Page starts loading immediately.\
✔ **Improved Core Web Vitals** – Better performance scores (LCP, FCP).

📌 **Example Use Cases:**

* **E-commerce**: Show product details first, load reviews later.
* **News websites**: Show headlines first, images & comments later.
* **Social media**: Show user info first, load posts later.

***

### **5. Example: Progressive Rendering with Suspense in Next.js**

#### **🚀 Example 1: Basic Progressive Rendering**

📌 **We load static content first and delay fetching slow data using Suspense.**

```tsx
// app/page.tsx
import { Suspense } from "react";

async function fetchData() {
  await new Promise((resolve) => setTimeout(resolve, 3000)); // Simulating slow API
  return "🔥 Dynamic Data Loaded!";
}

async function DynamicSection() {
  const data = await fetchData();
  return <p>{data}</p>;
}

export default function Page() {
  return (
    <div>
      <h1>Welcome to My Page</h1>

      {/* Static content loads instantly */}
      <p>This is a static part of the page.</p>

      {/* Dynamic content loads progressively */}
      <Suspense fallback={<p>Loading dynamic content...</p>}>
        <DynamicSection />
      </Suspense>
    </div>
  );
}
```

✅ **What Happens?**\
✔ **Static content loads immediately** (title & paragraph).\
✔ **Suspense delays slow content** while showing "Loading dynamic content...".\
✔ **Dynamic content appears later** after 3 seconds.

***

#### **🚀 Example 2: Progressive Rendering for API Data**

📌 **We fetch API data progressively using Suspense.**

```tsx
// app/page.tsx
import { Suspense } from "react";

async function fetchPosts() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts?_limit=5");
  const posts = await res.json();
  return posts;
}

async function PostsList() {
  const posts = await fetchPosts();

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default function Page() {
  return (
    <div>
      <h1>Latest Blog Posts</h1>

      {/* Suspense wraps slow content */}
      <Suspense fallback={<p>Loading posts...</p>}>
        <PostsList />
      </Suspense>
    </div>
  );
}
```

✅ **What Happens?**\
✔ Users **see the page instantly** (title loads first).\
✔ **"Loading posts..." appears while fetching data**.\
✔ **Posts stream in when ready** (faster experience).

***

### **6. Benefits of Progressive Rendering with Suspense**

🚀 **With Progressive Rendering + Suspense:**\
✔ **Important content loads first** (good UX).\
✔ **Slow components load later without blocking the page**.\
✔ **Reduces Time to First Byte (TTFB)** → Faster initial page load.\
✔ **Better SEO and Core Web Vitals**.

📌 **Without Progressive Rendering:** The whole page **waits for everything** to load.\
📌 **With Progressive Rendering:** The page **renders in steps**, improving speed!

***

### **7. Best Practices for Using Progressive Rendering**

✅ **Use Suspense for slow-loading parts** (API data, database queries).\
✅ **Keep important content outside Suspense** → Loads instantly.\
✅ **Use multiple Suspense components** to load sections separately.\
✅ **Avoid overusing Suspense** to prevent unnecessary delays.

***

### **8. Conclusion**

🔥 **Progressive Rendering with Suspense in Next.js** makes pages **faster** and **more user-friendly**.\
🔹 **Progressive Rendering** → Loads content **in steps** instead of waiting.\
🔹 **Suspense** → Delays slow content while showing a **fallback UI**.\
🔹 **Next.js 13+ & 14** make this **automatic** for better performance!
