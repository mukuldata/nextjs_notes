# File-based routing

### **Table of Contents**

1. Introduction
2. What is File-Based Routing?
3. Understanding `pages/` Directory (Next.js < 13)
4. Understanding `app/` Directory (Next.js 13+)
5. Key Differences Between `pages/` and `app/`
6. Example Comparisons
7. When to Use `pages/` vs `app/`
8. Conclusion

***

### **1. Introduction**

1. In traditional React applications, routing is handled using libraries like **React Router**, where you manually define routes.&#x20;
2. **Next.js simplifies this** by using **file-based routing**, meaning the file and folder structure determines the URLs automatically.

From **Next.js 13 onwards**, Next.js introduced a new **"App Router" (`app/` directory)**, replacing the old **"Pages Router" (`pages/` directory)**. Both systems work, but **the app router is the recommended approach moving forward**.

***

### **2. What is File-Based Routing?**

Instead of defining routes manually, Next.js automatically creates routes based on files inside the `pages/` or `app/` directory.

For example, if you create a file:

```
pages/about.js  
```

This will automatically be available at:

```
http://localhost:3000/about  
```

This makes routing **easier** and removes the need for external routing libraries like React Router.

***

### **3. Understanding `pages/` Directory (Next.js < 13)**

In older versions of Next.js (before Next.js 13), all routes were created inside the `pages/` directory.

#### **How It Works**

* Each file inside `pages/` represents a route.
* The folder structure determines the URL.
* API routes are created inside `pages/api/`.

#### **Example File Structure (`pages/` approach)**

```
pages/  
│── index.js        → "/" (Home Page)  
│── about.js        → "/about"  
│── contact.js      → "/contact"  
│── blog/  
│   ├── index.js    → "/blog"  
│   ├── [id].js     → "/blog/:id" (Dynamic Route)  
│── api/  
│   ├── hello.js    → "/api/hello" (API Route)  
```

#### **Example Code (`pages/about.js`)**

```jsx
export default function About() {
  return <h1>About Page</h1>;
}
```

🔹 **Automatically available at**: `http://localhost:3000/about`

#### **Drawbacks of `pages/` Approach**

* Limited flexibility.
* Does not support React Server Components.
* Requires separate **getServerSideProps** or **getStaticProps** for data fetching.

***

### **4. Understanding `app/` Directory (Next.js 13+)**

From **Next.js 13 onwards**, a new **App Router (`app/` directory)** was introduced, allowing better flexibility with **React Server Components, streaming, and layouts**.

#### **How It Works**

* Each folder inside `app/` represents a route.
* Each route contains a `page.js` file.
* `layout.js` can be used to create shared layouts.
* API routes are created inside `app/api/`.

#### **Example File Structure (`app/` approach)**

```
app/  
│── layout.js       → Shared layout for all pages  
│── page.js         → "/" (Home Page)  
│── about/  
│   ├── page.js     → "/about"  
│── contact/  
│   ├── page.js     → "/contact"  
│── blog/  
│   ├── page.js     → "/blog"  
│   ├── [id]/  
│       ├── page.js → "/blog/:id" (Dynamic Route)  
│── api/  
│   ├── hello/  
│       ├── route.js → "/api/hello" (API Route)  
```

#### **Example Code (`app/about/page.js`)**

```jsx
export default function About() {
  return <h1>About Page</h1>;
}
```

🔹 **Automatically available at**: `http://localhost:3000/about`

***

### **5. Key Differences Between `pages/` and `app/`**

| Feature                  | `pages/` (Old)                              | `app/` (New)                                |
| ------------------------ | ------------------------------------------- | ------------------------------------------- |
| **Introduced In**        | Before Next.js 13                           | Next.js 13+                                 |
| **Routing Type**         | File-based routing                          | Folder-based routing                        |
| **React Component Type** | Uses **Client Components** by default       | Uses **Server Components** by default       |
| **Data Fetching**        | Uses `getServerSideProps`, `getStaticProps` | Uses `fetch()` inside Server Components     |
| **Layout Handling**      | Requires `_app.js` for layouts              | Uses `layout.js` per folder                 |
| **API Routes**           | Defined in `pages/api/`                     | Defined in `app/api/`                       |
| **Performance**          | Less optimized                              | More optimized with React Server Components |

***

### **6. Example Comparisons**

#### **1️⃣ Creating a Route (`/about`)**

**📌 Using `pages/` (Old Way)**

```jsx
// pages/about.js
export default function About() {
  return <h1>About Page</h1>;
}
```

**📌 Using `app/` (New Way)**

```jsx
// app/about/page.js
export default function About() {
  return <h1>About Page</h1>;
}
```

***

#### **2️⃣ Dynamic Routes (`/blog/:id`)**

**📌 Using `pages/` (Old Way)**

```jsx
// pages/blog/[id].js
import { useRouter } from "next/router";

export default function BlogPost() {
  const router = useRouter();
  return <h1>Blog Post ID: {router.query.id}</h1>;
}
```

**📌 Using `app/` (New Way)**

```jsx
// app/blog/[id]/page.js
export default function BlogPost({ params }) {
  return <h1>Blog Post ID: {params.id}</h1>;
}
```

✅ **Benefit of `app/`**: `params.id` is automatically available without using `useRouter()`.

***

### **7. When to Use `pages/` vs `app/`**

| Use Case                                                                | Best Choice |
| ----------------------------------------------------------------------- | ----------- |
| **Using Next.js < 13**                                                  | `pages/`    |
| **SEO & performance optimizations**                                     | `app/`      |
| **Need layouts per route**                                              | `app/`      |
| **Need only client-side rendering**                                     | `pages/`    |
| **Want React Server Components**                                        | `app/`      |
| **Using old projects or third-party libraries that depend on `pages/`** | `pages/`    |

🔹 **For new projects, use `app/` since it's the recommended approach going forward.**

***

### **8. Conclusion**

* **`pages/` was the old way** of handling routes but is still supported for backward compatibility.
* **`app/` is the new and improved way**, offering better performance, layouts, and React Server Components.
* **If you’re starting a new project, use `app/`.**
