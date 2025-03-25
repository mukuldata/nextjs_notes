# Parallel & Intercepting Routes

### **Table of Contents**

1. Introduction
2. What Are Parallel Routes?
   * Example of Parallel Routes
3. What Are Intercepting Routes?
   * Example of Intercepting Routes
4. Key Differences
5. Use Cases
6. Summary

***

### **1. Introduction**

1. In Next.js 13+, **Parallel Routes** and **Intercepting Routes** are advanced routing features introduced in the App Router.&#x20;
2. They help in **building dynamic layouts**, **modals**, and **conditional rendering of components** without affecting the main navigation.

#### 📌 **Why Use These Features?**

✔ **Parallel Routes** → Render multiple pages simultaneously in different sections.\
✔ **Intercepting Routes** → Load new pages within existing layouts, useful for modals or in-place navigation.

***

### **2. What Are Parallel Routes?**

Parallel Routes allow you to render **multiple pages at the same time in different sections** of your layout.

#### 🔹 **How It Works?**

* You define multiple **slots** in a layout (`@slotName` folders).
* Each slot loads a different page **in parallel**.
* Great for **dashboards, split views, and side-by-side components**.

***

#### **Example of Parallel Routes**

**📌 Folder Structure**

```
app/
  layout.js
  @dashboard/
    page.js
  @settings/
    page.js
```

**📁 layout.js (Defines Parallel Slots)**

```javascript
export default function Layout({ dashboard, settings }) {
  return (
    <div style={{ display: "flex", gap: "20px" }}>
      <div style={{ flex: 1, border: "1px solid black" }}>{dashboard}</div>
      <div style={{ flex: 1, border: "1px solid red" }}>{settings}</div>
    </div>
  );
}
```

**📁 @dashboard/page.js**

```javascript
export default function Dashboard() {
  return <h2>Dashboard Section</h2>;
}
```

**📁 @settings/page.js**

```javascript
export default function Settings() {
  return <h2>Settings Section</h2>;
}
```

✅ **Result:**

* `@dashboard` and `@settings` pages render **side by side** inside `layout.js`.
* Both sections **load independently** without blocking each other.

***

### **3. What Are Intercepting Routes?**

Intercepting Routes **replace navigation behavior** by loading a page **within an existing layout** instead of a full-page reload.

#### 🔹 **How It Works?**

* Uses **(..) and (.)** in route folders to control how a route behaves.
* **(..) → Renders the new route inside the current layout.**
* **(.) → Renders the new route but keeps the existing page visible.**
* Great for **modals, popups, or in-place updates.**

***

#### **Example of Intercepting Routes**

**📌 Folder Structure**

```
app/
  layout.js
  products/
    page.js
    [id]/
      page.js
  (..)modal/
    products/
      [id]/
        page.js
```

**📁 layout.js**

```javascript
export default function Layout({ children }) {
  return (
    <div>
      <h1>My Store</h1>
      {children}
    </div>
  );
}
```

**📁 products/page.js (Product List)**

```javascript
import Link from "next/link";

export default function Products() {
  return (
    <div>
      <h2>Products</h2>
      <Link href="/products/1">View Product 1</Link>
    </div>
  );
}
```

**📁 products/\[id]/page.js (Regular Product Page)**

```javascript
export default function ProductPage({ params }) {
  return <h2>Product Details for ID: {params.id}</h2>;
}
```

**📁 (..)modal/products/\[id]/page.js (Intercepted Modal)**

```javascript
export default function ProductModal({ params }) {
  return (
    <div style={{ position: "fixed", top: 0, left: 0, background: "white" }}>
      <h2>Quick View Product ID: {params.id}</h2>
    </div>
  );
}
```

✅ **Result:**

* Clicking `/products/1` normally opens a **full page**.
* If navigated through `(..)modal/products/1`, it opens as a **popup (modal)** instead.

***

### **4. Key Differences**

| Feature       | Parallel Routes                   | Intercepting Routes                     |
| ------------- | --------------------------------- | --------------------------------------- |
| Purpose       | Render multiple sections together | Load new page inside an existing layout |
| Use Case      | Dashboards, split screens         | Modals, popups, in-place updates        |
| Folder Syntax | `@slotName`                       | `(..) or (.)`                           |
| Navigation    | Regular                           | Replaces full-page reload               |

***

### **5. Use Cases**

✅ **Parallel Routes:**\
✔ Dashboards with multiple sections (like Analytics & Reports).\
✔ Admin panels with independent tabs.\
✔ E-commerce layouts with a sidebar and content area.

✅ **Intercepting Routes:**\
✔ Modals for quick view of products.\
✔ Opening chat without leaving the main page.\
✔ In-place updates without breaking navigation history.

***

### **6. Summary**

* **Parallel Routes** → Allow **multiple independent sections** to load side-by-side.
* **Intercepting Routes** → Let a new route open **inside an existing page** (great for modals).
* Both **improve user experience** and **optimize navigation** in Next.js 13+.
