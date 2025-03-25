# Nested Routes

### **Table of Contents**

1. Introduction
2. What are Nested Routes?
3. Creating a Basic Nested Route
4. Example of Nested Routes in Next.js 13+
5. Layouts for Nested Routes
6. Handling Dynamic Nested Routes
7. Summary

***

### **1. Introduction**

1. In **Next.js 13+**, routing is based on the **app directory** and follows a **file-based routing system**.\

2. **Nested routes** allow you to structure your application into multiple levels, making it easy to organize related pages within subfolders.

#### **Example URL Structure for Nested Routes:**

* `/dashboard`
* `/dashboard/settings`
* `/dashboard/profile`

***

### **2. What are Nested Routes?**

A **nested route** is a route that exists inside another route. It means one page is placed **inside another page**.\
This structure helps in creating sections like **admin panels, dashboards, and user profiles**.

✅ **Example File Structure for Nested Routes:**

```
app/  
│── dashboard/  
│   ├── page.js       → "/dashboard"  
│   ├── settings/  
│   │   ├── page.js   → "/dashboard/settings"  
│   ├── profile/  
│   │   ├── page.js   → "/dashboard/profile"  
```

Here, `dashboard/settings` and `dashboard/profile` are **nested inside the `dashboard` route**.

***

### **3. Creating a Basic Nested Route**

#### **Step 1: Create the Parent Route (`/dashboard`)**

Create a file:\
📁 `app/dashboard/page.js`

```jsx
export default function Dashboard() {
  return <h1>Dashboard Home</h1>;
}
```

🔹 **Available at:** `http://localhost:3000/dashboard`

#### **Step 2: Create the Child Route (`/dashboard/settings`)**

Create a folder `settings/` inside `dashboard/`.\
📁 `app/dashboard/settings/page.js`

```jsx
export default function Settings() {
  return <h1>Dashboard Settings</h1>;
}
```

🔹 **Available at:** `http://localhost:3000/dashboard/settings`

✅ **Now we have a nested route:**

* `dashboard/` → Shows **Dashboard Home**
* `dashboard/settings/` → Shows **Dashboard Settings**

***

### **4. Example of Nested Routes in Next.js 13+**

You can create **multiple levels of nested routes** by organizing files inside folders.

#### **Example: Nested User Profile Pages**

```
app/  
│── users/  
│   ├── page.js        → "/users"  
│   ├── [id]/  
│       ├── page.js    → "/users/:id"  
│       ├── posts/  
│           ├── page.js  → "/users/:id/posts"  
```

#### **Code for `/users/:id`**

📁 `app/users/[id]/page.js`

```jsx
export default function UserProfile({ params }) {
  return <h1>User ID: {params.id}</h1>;
}
```

🔹 **Available at:** `http://localhost:3000/users/123` → Shows **User ID: 123**

#### **Code for `/users/:id/posts`**

📁 `app/users/[id]/posts/page.js`

```jsx
export default function UserPosts({ params }) {
  return <h1>Posts by User ID: {params.id}</h1>;
}
```

🔹 **Available at:** `http://localhost:3000/users/123/posts` → Shows **Posts by User ID: 123**

***

### **5. Layouts for Nested Routes**

In **Next.js 13+**, you can use **`layout.js`** files to create a **shared layout** for all nested pages inside a route.

#### **Example Layout for `/dashboard`**

📁 `app/dashboard/layout.js`

```jsx
export default function DashboardLayout({ children }) {
  return (
    <div>
      <h1>Dashboard Navigation</h1>
      {children} {/* This will render the nested pages */}
    </div>
  );
}
```

Now, all pages under `dashboard/` will **inherit this layout**.

***

### **6. Handling Dynamic Nested Routes**

#### **Example File Structure for Dynamic Nested Routes:**

```
app/  
│── categories/  
│   ├── [category]/  
│       ├── page.js   → "/categories/:category"  
│       ├── products/  
│           ├── [productId]/  
│               ├── page.js  → "/categories/:category/products/:productId"  
```

#### **Code for `/categories/:category/products/:productId`**

📁 `app/categories/[category]/products/[productId]/page.js`

```jsx
export default function ProductPage({ params }) {
  return (
    <h1>
      Category: {params.category}, Product ID: {params.productId}
    </h1>
  );
}
```

🔹 **Available at:**

* `http://localhost:3000/categories/electronics/products/1001`\
  → Shows **Category: electronics, Product ID: 1001**

***

### **7. Summary**

* **Nested routes** help organize pages inside folders for better structure.
* **Layouts (`layout.js`)** allow shared UI components for nested pages.
* **Dynamic nested routes** allow flexible URLs for products, users, and more.
