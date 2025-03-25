# Incremental Static Regeneration (ISR)

### **Table of Contents**

1. Introduction
2. What is Incremental Static Regeneration (ISR)?
3. How ISR Works in Next.js 13+
4. Example: Implementing ISR in Next.js
5. Benefits of ISR
6. When to Use ISR?
7. Summary

***

### **1. Introduction**

Next.js provides different rendering methods, and **Incremental Static Regeneration (ISR)** allows you to **update static pages without rebuilding the entire site**.

ISR is useful when you need **fast page loading** (like Static Site Generation - SSG) but also need **updated content** (like Server-Side Rendering - SSR).

***

### **2. What is Incremental Static Regeneration (ISR)?**

#### 🔹 **Definition:**

* **ISR allows static pages to be updated automatically** after a certain time interval.
* Pages are pre-rendered **at build time**, but **they can be refreshed periodically** without requiring a full site rebuild.
* This makes it ideal for sites where content updates **every few minutes/hours/days** but **doesn’t need real-time updates**.

#### 🔹 **Key Features of ISR:**

✔ **Fast Performance** – Serves pre-rendered pages from CDN.\
✔ **Auto Update** – Rebuilds pages **only when needed**.\
✔ **Reduced Build Time** – No need to rebuild all pages for small changes.\
✔ **Good SEO** – Fully generated HTML for better search rankings.

***

### **3. How ISR Works in Next.js 13+?**

#### **🔹 Process Flow:**

1. **At Build Time** → Next.js generates a static page.
2. **User Visits the Page** → The static page is served instantly.
3. **After a Set Time (`revalidate` value)** → The page is **regenerated in the background** when a new request comes.
4. **New Visitors See the Updated Page** → Once the new version is generated, all new visitors get the latest page.

#### **🔹 When to Use ISR?**

* Blog posts that **update frequently** (but not instantly).
* E-commerce product pages that change **every few hours**.
* News websites that update articles **every few minutes**.
* Any site that needs **fast performance + periodic updates**.

***

### **4. Example: Implementing ISR in Next.js 13+**

In Next.js 13+ (App Router), ISR is used by setting a **revalidation time** in a **Server Component**.

#### **📁 app/products/\[id]/page.js** (ISR Example for Product Page)

```javascript
export default async function ProductPage({ params }) {
  const response = await fetch(`https://api.example.com/products/${params.id}`, {
    next: { revalidate: 60 }, // Regenerate the page every 60 seconds
  });

  const product = await response.json();

  return (
    <div>
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      <p>Price: ${product.price}</p>
    </div>
  );
}
```

#### **How This Works:**

✅ **Fetches product data from API at build time**.\
✅ **Caches the page and serves it instantly**.\
✅ **Every 60 seconds**, Next.js **fetches new data** in the background.\
✅ **Users always see the latest version after the update is complete**.

***

### **5. Benefits of ISR**

✅ **Super Fast** – Pre-rendered pages load instantly.\
✅ **Content Stays Fresh** – Updates automatically without a full rebuild.\
✅ **Better Performance** – Reduces server load compared to SSR.\
✅ **SEO Optimized** – Serves fully built HTML pages.

***

### **6. When to Use ISR?**

| Scenario                                 | Use ISR?                    |
| ---------------------------------------- | --------------------------- |
| Blog posts (updated every few hours)     | ✅ Yes                       |
| E-commerce product pages (price updates) | ✅ Yes                       |
| News articles (refresh every minute)     | ✅ Yes                       |
| Real-time stock market data              | ❌ No (Use SSR or Streaming) |
| User profile pages                       | ❌ No (Use SSR or CSR)       |

***

### **7. Summary**

* **ISR (Incremental Static Regeneration) updates static pages automatically** after a set time.
* **It combines the speed of Static Site Generation (SSG) with automatic updates** like Server-Side Rendering (SSR).
* **Perfect for blogs, e-commerce, and news sites** that need **fast load times + fresh content**.
* **Next.js 13+ uses `next: { revalidate: X }` to set the update interval**.
