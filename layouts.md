# Layouts

### **Table of Contents**

1. **Introduction to Layouts in Next.js**
2. **How Layouts Work in the `app/` Directory**
3. **Creating a Basic Layout Component**
4. **Using Nested Layouts**
5. **Shared Layouts with Persistent UI**
6. **Example: Building a Multi-Page Layout**
7. **Conclusion**

***

### **1. Introduction to Layouts in Next.js**

1. In Next.js 13 and later, layouts help structure pages by wrapping them in a reusable UI.&#x20;
2. Unlike traditional React components, layouts in the `app/` directory persist across page navigation, improving performance and user experience.

### **2. How Layouts Work in the `app/` Directory**

* In the `app/` directory, layouts are defined using `layout.tsx` or `layout.jsx`.
* Layouts **persist** across page navigations, meaning components inside a layout **do not re-render** when navigating between child pages.
* This is useful for **navigation bars, sidebars, and footers** that remain the same across multiple pages.

### **3. Creating a Basic Layout Component**

Here’s how you can define a layout in Next.js 13+:

#### **Example: Creating a Global Layout** (`app/layout.tsx`)

```tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        <header>My Website Header</header>
        <main>{children}</main>
        <footer>Footer Content</footer>
      </body>
    </html>
  );
}
```

* The **`children`** prop ensures that all pages inside this layout inherit the structure.

### **4. Using Nested Layouts**

You can create **multiple layouts** for different sections of your site.

#### **Example: Creating a Dashboard Layout** (`app/dashboard/layout.tsx`)

```tsx
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div>
      <nav>Dashboard Sidebar</nav>
      <div>{children}</div>
    </div>
  );
}
```

* Now, all pages inside `app/dashboard/` will have the sidebar.

### **5. Shared Layouts with Persistent UI**

A major advantage of Next.js 13 layouts is **persistent UI**, meaning parts of the layout do not reload when navigating between pages.

* Example: **A music player stays active** while users browse different pages.

### **6. Example: Building a Multi-Page Layout**

#### **Project Structure**

```
app/
 ├── layout.tsx        // Root Layout (applies to all pages)
 ├── page.tsx          // Homepage
 ├── dashboard/
 │    ├── layout.tsx   // Dashboard-specific Layout
 │    ├── page.tsx     // Dashboard Homepage
```

With this setup:

* `layout.tsx` applies to all pages.
* `dashboard/layout.tsx` applies **only to dashboard pages**.

### **7. Conclusion**

* Layouts **persist UI** across navigation.
* They help in **creating reusable structures** like headers, sidebars, and footers.
* Nested layouts allow **scoped layouts for specific sections** of the app.
