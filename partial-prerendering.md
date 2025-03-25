# Partial Prerendering

### **Table of Contents**

1. **Introduction to Partial Prerendering**
2. **Why is Partial Prerendering Needed?**
3. **How Partial Prerendering Works**
4. **Static vs. Dynamic Rendering in Next.js**
5. **Example: Implementing Partial Prerendering**
6. **Performance Benefits of Partial Prerendering**
7. **Best Practices for Using Partial Prerendering**
8. **Conclusion**

***

### **1. Introduction to Partial Prerendering**

**Partial Prerendering (PPR)** is a new feature in **Next.js 14+** that allows a page to have both **static (pre-rendered) and dynamic (real-time) content**.

🔹 **Static Content** → Rendered at build time (fast, cached, and SEO-friendly)\
🔹 **Dynamic Content** → Rendered at request time (updates instantly)

✅ **Best of both worlds** → Faster load times with up-to-date data!

***

### **2. Why is Partial Prerendering Needed?**

🔴 **Problem with Static Rendering (SSG)**

* Generates pages **at build time** (good for speed but doesn't update frequently).
* Not ideal for pages needing **frequent updates** (e.g., news feeds, stock prices).

🔴 **Problem with Dynamic Rendering (SSR)**

* Generates pages **at request time** (ensures fresh data but can be slow).
* Not good for **high-traffic pages** (increases server load).

✅ **Solution: Partial Prerendering (Hybrid Rendering)**

* **Static parts load instantly** (pre-rendered at build time).
* **Dynamic parts update in real-time** (without needing full page reload).

***

### **3. How Partial Prerendering Works?**

With **Partial Prerendering**, Next.js pre-renders the page **statically** but allows specific parts to be **dynamically updated on the client-side or with streaming**.

🛠 **How it's implemented?**

* Use **static rendering** for content that doesn’t change often.
* Use **dynamic fetching (React Suspense, Streaming, or Server Components)** for content that needs real-time updates.

***

### **4. Static vs. Dynamic Rendering in Next.js**

| Feature            | **Static Rendering (SSG)** | **Dynamic Rendering (SSR)**     | **Partial Prerendering (PPR)**   |
| ------------------ | -------------------------- | ------------------------------- | -------------------------------- |
| **Rendering time** | Build time                 | Request time                    | Hybrid (both)                    |
| **Performance**    | 🚀 Fastest                 | 🐢 Slower                       | ⚡ Optimized                      |
| **Fresh Data**     | ❌ No                       | ✅ Yes                           | ✅ Yes (for dynamic parts)        |
| **Ideal for**      | Blog posts, landing pages  | User dashboards, real-time apps | Mixed content (static + dynamic) |

***

### **5. Example: Implementing Partial Prerendering**

#### **🚀 Basic Page Without Partial Prerendering (Static Only)**

📌 **A blog page that loads data at build time (not updated dynamically)**

```tsx
// app/page.tsx (Static Rendering - No Partial Prerendering)
async function getData() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts");
  return res.json();
}

export default async function Page() {
  const posts = await getData(); // Data is fetched once at build time

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

🔴 **Problem:** Data won’t update unless you rebuild the app.

***

#### **🟢 Using Partial Prerendering (Static + Dynamic Content)**

📌 **Use React Suspense to fetch data dynamically for certain parts**

```tsx
// app/page.tsx (Partial Prerendering Example)
import { Suspense } from "react";

async function getData() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts");
  return res.json();
}

function DynamicData() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/posts")
      .then((res) => res.json())
      .then((data) => setPosts(data));
  }, []);

  return (
    <ul>
      {posts.map((post: any) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}

export default function Page() {
  return (
    <div>
      <h1>Blog Posts</h1>
      
      {/* Static content (prerendered at build time) */}
      <p>Welcome to our blog!</p>

      {/* Dynamic content (fetching in real-time) */}
      <Suspense fallback={<p>Loading latest posts...</p>}>
        <DynamicData />
      </Suspense>
    </div>
  );
}
```

✅ **Static Content Loads Instantly**\
✅ **Latest Data is Fetched Dynamically**\
✅ **Improves Performance & SEO**

***

### **6. Performance Benefits of Partial Prerendering**

✅ **Reduces Time-to-First-Byte (TTFB)** → Page loads instantly.\
✅ **Lowers JavaScript Bundle Size** → Static content reduces unnecessary client-side JS.\
✅ **Improves SEO** → Static parts are indexed, while dynamic parts update in real-time.\
✅ **Efficient Server Load** → Reduces need for expensive SSR requests.

***

### **7. Best Practices for Using Partial Prerendering**

✅ **Use Static Rendering for Most Content** → Static content is always faster.\
✅ **Use Partial Prerendering for Frequently Updated Sections** → Dynamic parts should update smoothly.\
✅ **Leverage React Suspense for Streaming Data** → Helps load dynamic content progressively.\
✅ **Combine Server & Client Components** → Use **Server Components** for fetching and **Client Components** for interactivity.

***

### **8. Conclusion**

🚀 **Partial Prerendering in Next.js 14+** allows developers to:

* Combine **Static and Dynamic Rendering** for **faster performance**.
* Load **static content instantly** while keeping **dynamic parts fresh**.
* **Improve SEO and User Experience** with hybrid rendering.
