# Static Site Generation (SSG)

### **Table of Contents**

1. Introduction
2. What is Static Site Generation (SSG)?
3. How SSG Works in Next.js 13+
4. Example: Implementing SSG in Next.js
5. Benefits of SSG
6. When to Use SSG?
7. Summary

***

### **1. Introduction**

Next.js provides multiple ways to render pages, and **Static Site Generation (SSG)** is one of the most efficient methods for building **fast and SEO-friendly websites**.

In **Next.js 13+**, SSG has been improved using the **App Router** and **React Server Components**, making it even more powerful.

***

### **2. What is Static Site Generation (SSG)?**

#### 🔹 **Definition:**

* **SSG means generating the HTML pages at build time** (before users request them).
* The generated HTML is **stored and served directly from a CDN**, making it **extremely fast**.
* **No database or API calls happen at request time**, improving **performance and SEO**.

#### 🔹 **Key Benefits:**

✔ **Fast Performance** – Pages load instantly since they are pre-generated.\
✔ **SEO-Friendly** – Fully generated HTML improves search engine ranking.\
✔ **Reduced Server Load** – No need to process requests dynamically.\
✔ **Better Caching** – Can be served via a Content Delivery Network (CDN) for global speed.

***

### **3. How SSG Works in Next.js 13+?**

#### **🔹 Process Flow:**

1. **Build Time**: Next.js **fetches data** and generates static HTML pages.
2. **Deploy to CDN**: The pages are **stored and cached** for fast access.
3. **User Request**: When a user visits the site, **Next.js serves the pre-built page instantly**.

#### **🔹 When to Use SSG?**

* Blog posts
* Documentation sites
* E-commerce product pages (without frequent updates)
* Marketing websites

***

### **4. Example: Implementing SSG in Next.js 13+**

In **Next.js 13+ (App Router)**, we **fetch data at build time** inside a **Server Component**.

#### **📁 app/blog/\[id]/page.js** (SSG for a Blog Post)

```javascript
export async function generateStaticParams() {
  // Fetch the list of blog posts at build time
  const response = await fetch("https://api.example.com/posts");
  const posts = await response.json();

  return posts.map((post) => ({
    id: post.id.toString(),
  }));
}

export default async function BlogPost({ params }) {
  // Fetch a single blog post at build time
  const response = await fetch(`https://api.example.com/posts/${params.id}`);
  const post = await response.json();

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
  );
}
```

#### **How This Works:**

✅ **`generateStaticParams`** → Pre-builds pages for each blog post at build time.\
✅ **The `fetch` call** runs **at build time**, not on every request.\
✅ The generated pages are **stored and served instantly** without extra API calls.

***

### **5. Benefits of SSG**

✅ **Super Fast** – Pages are pre-built, making them load almost instantly.\
✅ **Better SEO** – Search engines can index the fully generated HTML pages.\
✅ **No Database Load** – Since pages are static, no backend is needed for each request.\
✅ **Lower Hosting Cost** – Can be hosted for free on platforms like **Vercel or Netlify**.

***

### **6. When to Use SSG?**

| Scenario                                 | Use SSG?              |
| ---------------------------------------- | --------------------- |
| Blog pages                               | ✅ Yes                 |
| Marketing landing pages                  | ✅ Yes                 |
| Product pages (with rare updates)        | ✅ Yes                 |
| Real-time dashboards                     | ❌ No (Use SSR or CSR) |
| User-specific pages (e.g., Profile page) | ❌ No (Use SSR)        |

***

### **7. Summary**

* **Static Site Generation (SSG) pre-renders pages at build time** for better speed and performance.
* **Next.js 13+ uses `generateStaticParams` to fetch data and generate pages.**
* **SSG is ideal for blogs, documentation, and marketing websites.**
* **Pre-generated pages are served instantly, reducing server load and improving SEO.**?
