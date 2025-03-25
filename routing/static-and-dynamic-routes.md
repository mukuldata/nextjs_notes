# Static and Dynamic Routes

### **Table of Contents**

1. Introduction
2. What are Static Routes?
   * Example of a Static Route
3. What are Dynamic Routes?
   * Example of a Dynamic Route
4. Difference Between Static and Dynamic Routes
5. Catch-All Routes (`...slug`)
6. Optional Catch-All Routes (`[[...slug]]`)
7. Conclusion

***

### **1. Introduction**

In **Next.js 13 and above**, routing is handled using **file-based routing**, where the file and folder structure determines the URLs.

There are **two main types of routes**:

1. **Static Routes** – Pages with fixed URLs (e.g., `/about`, `/contact`).
2. **Dynamic Routes** – Pages where part of the URL can change dynamically (e.g., `/blog/:id`).

Next.js allows us to create **both static and dynamic routes easily** inside the `app/` directory.

***

### **2. What are Static Routes?**

A **static route** is a page with a **fixed URL** that does not change.

#### **Example File Structure for Static Routes**

```
app/  
│── page.js          → "/" (Home Page)  
│── about/  
│   ├── page.js      → "/about"  
│── contact/  
│   ├── page.js      → "/contact"  
```

#### **Example Code (`app/about/page.js`)**

```jsx
export default function About() {
  return <h1>About Page</h1>;
}
```

🔹 **Available at:** `http://localhost:3000/about`

#### **Characteristics of Static Routes:**

✅ Fixed URLs.\
✅ Easier to manage.\
✅ Best for pages like Home, About, Contact.

***

### **3. What are Dynamic Routes?**

A **dynamic route** is a page where **part of the URL changes** dynamically, based on a variable (like an `id`).

#### **Example File Structure for Dynamic Routes**

```
app/  
│── blog/  
│   ├── [id]/  
│       ├── page.js  → "/blog/:id"  
```

Here, `[id]` is a **dynamic segment** that changes based on the URL.

#### **Example Code (`app/blog/[id]/page.js`)**

```jsx
export default function BlogPost({ params }) {
  return <h1>Blog Post ID: {params.id}</h1>;
}
```

🔹 **Available at:**

* `http://localhost:3000/blog/1` → Shows **"Blog Post ID: 1"**
* `http://localhost:3000/blog/2` → Shows **"Blog Post ID: 2"**

#### **Characteristics of Dynamic Routes:**

✅ The URL is flexible and changes dynamically.\
✅ Used for pages that depend on **dynamic content** (e.g., blog posts, user profiles).\
✅ Uses **URL parameters (`params`)** to fetch the correct data.

***

### **4. Difference Between Static and Dynamic Routes**

| Feature                      | Static Route (`/about`)                | Dynamic Route (`/blog/:id`)                             |
| ---------------------------- | -------------------------------------- | ------------------------------------------------------- |
| **URL Type**                 | Fixed                                  | Variable (changes based on parameter)                   |
| **Example URL**              | `/about`                               | `/blog/1`, `/blog/2`, etc.                              |
| **File Naming**              | `page.js` inside a folder              | `[id]/page.js` inside a folder                          |
| **Use Case**                 | Pages with fixed content (Home, About) | Pages with changing content (Blog posts, User profiles) |
| **Requires URL Parameters?** | No                                     | Yes (`params.id`)                                       |

***

### **5. Catch-All Routes (`...slug`)**

A **catch-all route** is a special type of dynamic route that can **match multiple segments** in a URL.

#### **Example File Structure**

```
app/  
│── docs/  
│   ├── [...slug]/  
│       ├── page.js  → "/docs/anything/here"  
```

#### **Example Code (`app/docs/[...slug]/page.js`)**

```jsx
export default function DocsPage({ params }) {
  return <h1>Docs Path: {params.slug.join(" / ")}</h1>;
}
```

🔹 **Available at:**

* `http://localhost:3000/docs/intro` → **Docs Path: intro**
* `http://localhost:3000/docs/setup/install` → **Docs Path: setup / install**

#### **When to Use Catch-All Routes?**

✅ When the number of path segments is **unknown**.\
✅ Useful for **documentation pages**, **categories**, etc.

***

### **6. Optional Catch-All Routes (`[[...slug]]`)**

An **optional catch-all route** is like a catch-all route, but it also works **when no parameter is provided**.

#### **Example File Structure**

```
app/  
│── docs/  
│   ├── [[...slug]]/  
│       ├── page.js  → "/docs" or "/docs/setup"  
```

#### **Example Code (`app/docs/[[...slug]]/page.js`)**

```jsx
export default function DocsPage({ params }) {
  if (!params.slug) {
    return <h1>Welcome to the Docs</h1>;
  }
  return <h1>Docs Path: {params.slug.join(" / ")}</h1>;
}
```

🔹 **Available at:**

* `http://localhost:3000/docs` → **"Welcome to the Docs"**
* `http://localhost:3000/docs/setup` → **"Docs Path: setup"**

✅ **Benefit:** Handles both **"/docs"** and **"/docs/some/path"**

***

### **7. Conclusion**

* **Static routes** are simple, with fixed URLs (e.g., `/about`).
* **Dynamic routes** allow URLs to change (e.g., `/blog/:id`).
* **Catch-all routes (`...slug`)** handle multiple segments.
* **Optional catch-all routes (`[[...slug]]`)** allow flexible routing.
