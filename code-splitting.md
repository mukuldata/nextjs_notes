# Code Splitting

### **Table of Contents**

1. Introduction
2. Why Code Splitting is Important
3. Types of Code Splitting in Next.js
   * Automatic Code Splitting
   * Dynamic Imports (Lazy Loading)
   * Component-Level Code Splitting
4. Using Dynamic Imports in Next.js
5. Example: Code Splitting with Dynamic Import
6. When to Use Code Splitting
7. Conclusion

***

### **1. Introduction**

Code splitting is a technique used to **split JavaScript code into smaller bundles** that are loaded only when needed. This helps in:\
✅ **Faster initial page load**\
✅ **Reduced JavaScript size**\
✅ **Improved performance, especially on mobile devices**

Next.js **automatically** handles code splitting, but we can also manually split code using **dynamic imports**.

***

### **2. Why Code Splitting is Important**

If a Next.js app has **many components and libraries**, loading everything at once can slow down the page.\
🔹 Code splitting **loads only the necessary code** for each page.\
🔹 This improves performance by reducing the amount of JavaScript loaded initially.

**Without Code Splitting:**

* The entire JavaScript bundle loads at once.
* Large pages take longer to load.

**With Code Splitting:**

* Only required code loads for the current page.
* Additional code loads dynamically when needed.

***

### **3. Types of Code Splitting in Next.js**

Next.js supports different types of code splitting:

#### **A. Automatic Code Splitting**

Next.js **automatically** splits JavaScript by page.

* When a user visits `/about`, only the code for `about.js` loads.
* When they visit `/contact`, the contact page code loads separately.

✅ **No extra configuration is needed!**

***

#### **B. Dynamic Imports (Lazy Loading)**

* Load components **only when needed** instead of at the start.
* Useful for **large components** like charts, maps, and modals.

***

#### **C. Component-Level Code Splitting**

* Load parts of a component only when required.
* Example: A modal that only loads when the user clicks a button.

***

### **4. Using Dynamic Imports in Next.js**

To **manually** split code, we use `next/dynamic`.

```javascript
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("../components/HeavyComponent"));

export default function Home() {
  return (
    <div>
      <h1>Home Page</h1>
      <HeavyComponent /> {/* Loads only when needed */}
    </div>
  );
}
```

✅ The `HeavyComponent` will **only load when needed**, reducing initial page size.

***

### **5. Example: Code Splitting with Dynamic Import**

📁 **components/HeavyComponent.js**

```javascript
export default function HeavyComponent() {
  return <h2>This is a heavy component</h2>;
}
```

📁 **app/page.js**

```javascript
import dynamic from "next/dynamic";

const HeavyComponent = dynamic(() => import("../components/HeavyComponent"), {
  loading: () => <p>Loading...</p>, // Show this while loading
});

export default function Home() {
  return (
    <div>
      <h1>Welcome to Next.js</h1>
      <HeavyComponent /> {/* Loaded dynamically */}
    </div>
  );
}
```

✅ **What happens here?**

* `HeavyComponent` **is not loaded initially**.
* It loads **only when needed**.
* A **loading indicator** (`Loading...`) is shown while fetching the component.

***

### **6. When to Use Code Splitting?**

🔹 **For large libraries** (e.g., Chart.js, moment.js)\
🔹 **For components that aren’t always visible** (e.g., modals, dropdowns)\
🔹 **For third-party widgets** (e.g., maps, chat widgets)\
🔹 **For admin dashboards** (only load admin components when needed)

***

### **7. Conclusion**

✅ **Code splitting improves performance** by reducing initial load time.\
✅ **Next.js automatically splits code per page**, so only required code loads.\
✅ We can manually **split code using dynamic imports**.\
✅ Best used for **heavy components, libraries, and rarely used UI elements**.
