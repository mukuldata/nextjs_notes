# Next JS

### **Table of Contents**

1. What is Next.js?
2. Why Use Next.js?
3. Key Features of Next.js
4. How Does Next.js Work?
5. Basic Example of Next.js
6. Conclusion

***

### **1. What is Next.js?**

1. **Next.js** is a React framework that helps developers build modern web applications efficiently.&#x20;
2. It provides built-in features like **server-side rendering (SSR), static site generation (SSG), API routes, and automatic code splitting** to improve performance and SEO.

Think of **Next.js** as an upgraded version of React that makes web applications faster, SEO-friendly, and easier to develop.

***

### **2. Why Use Next.js?**

Here are some key reasons why developers choose Next.js:

| Feature                          | Benefits                                                                |
| -------------------------------- | ----------------------------------------------------------------------- |
| **SEO-Friendly**                 | Improves search engine ranking by pre-rendering pages                   |
| **Fast Performance**             | Uses automatic code splitting and optimized loading                     |
| **Server-Side Rendering (SSR)**  | Fetches and renders data on the server before sending it to the browser |
| **Static Site Generation (SSG)** | Pre-builds HTML files for fast loading                                  |
| **API Routes**                   | Allows backend functionality within the same project                    |
| **Automatic Routing**            | Pages are automatically linked without manual setup                     |
| **Image Optimization**           | Uses built-in Next.js Image component for faster image loading          |

***

### **3. Key Features of Next.js**

1. **File-based Routing** – Each file in the `pages` directory automatically becomes a route.
2. **Pre-rendering (SSR & SSG)** – Pages load faster by rendering content before reaching the browser.
3. **API Routes** – Easily create backend endpoints within Next.js.
4. **Image Optimization** – Handles image resizing and lazy loading automatically.
5. **Incremental Static Regeneration (ISR)** – Updates static pages without rebuilding the entire site.

***

### **4. How Does Next.js Work?**

Next.js handles different types of page rendering:

* **Static Site Generation (SSG)** – Generates HTML at build time. (Fastest method)
* **Server-Side Rendering (SSR)** – Generates HTML on each request. (Good for dynamic content)
* **Client-Side Rendering (CSR)** – Fetches data after the page loads. (Like React)
* **Incremental Static Regeneration (ISR)** – Updates static pages in the background without a full rebuild.

***

### **5. Basic Example of Next.js**

Here’s a simple example of a **Next.js project**:

#### **Step 1: Install Next.js**

Run the following command in your terminal:

```bash
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```

#### **Step 2: Create a New Page**

Inside the `pages` directory, create a new file called `about.js`:

```jsx
// pages/about.js
export default function About() {
  return (
    <div>
      <h1>About Page</h1>
      <p>This is a simple page in Next.js.</p>
    </div>
  );
}
```

Now, you can visit `http://localhost:3000/about` in your browser.

#### **Step 3: Fetch Data with Server-Side Rendering (SSR)**

```jsx
// pages/ssr-example.js
export async function getServerSideProps() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts/1');
  const post = await res.json();

  return {
    props: { post }, // Will be passed to the page component as props
  };
}

export default function SSRExample({ post }) {
  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.body}</p>
    </div>
  );
}
```

This fetches data on **every request**, ensuring fresh content.

***

### **6. Conclusion**

Next.js is a powerful framework for building **fast, SEO-friendly, and scalable** web applications. It combines the best of **React, server-side rendering, and static site generation**, making it a great choice for modern web development.
