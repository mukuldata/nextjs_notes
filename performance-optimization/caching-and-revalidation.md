# Caching & Revalidation

### **Table of Contents**

1. Introduction
2. What is Caching?
3. What is Revalidation?
4. Different Types of Caching in Next.js
5. How Revalidation Works in Next.js
6. Example: Fetching Data with Automatic Caching
7. Example: Revalidating Data on Demand
8. Example: Revalidating at Regular Intervals
9. Example: Revalidating After a User Action
10. Conclusion

***

### **1. Introduction**

Caching and revalidation in Next.js help improve performance by **storing data temporarily** and updating it **only when necessary**.

✅ **Caching** → Speeds up page loads by storing responses.\
✅ **Revalidation** → Refreshes cached data **only when needed** (without full reload).

Next.js **automatically caches API responses and page data**, but we can control **when and how data updates**.

***

### **2. What is Caching?**

Caching **stores data temporarily** to avoid repeated API calls.

📌 **Example: Without Caching**\
1️⃣ Every user request **fetches new data** from an API.\
2️⃣ This **slows down** the page and increases server load.

📌 **Example: With Caching**\
1️⃣ The first request **fetches data and stores it**.\
2️⃣ Subsequent requests **use the stored data** (until revalidation).\
3️⃣ This **makes the app faster** and reduces server load.

🔹 **Next.js 13+ caches data automatically** for API requests and static pages.

***

### **3. What is Revalidation?**

Revalidation **updates cached data** only when needed.

📌 **Example: Without Revalidation**

* Data is cached **forever** (until the app is restarted).
* Users **never see updated content** unless they refresh manually.

📌 **Example: With Revalidation**

* The cache **updates automatically** at intervals or after an action.
* Users **always see fresh data** without reloading the page.

✅ **Next.js supports different revalidation strategies:**

* **Time-based revalidation** (revalidate data every X seconds).
* **On-demand revalidation** (revalidate when needed, e.g., after a user action).

***

### **4. Different Types of Caching in Next.js**

| Caching Type              | Description                                                                        |
| ------------------------- | ---------------------------------------------------------------------------------- |
| **Static Caching (SSG)**  | Preloads pages at build time and caches them.                                      |
| **Dynamic Caching (ISR)** | Uses Incremental Static Regeneration (ISR) to revalidate cached data at intervals. |
| **Full Page Caching**     | Caches entire HTML pages for fast rendering.                                       |
| **Data Fetching Caching** | Caches API responses from `fetch()` and revalidates when needed.                   |

✅ **Next.js automatically caches all fetch requests in `getStaticProps()` and `getServerSideProps()`.**

***

### **5. How Revalidation Works in Next.js?**

🔹 Next.js **supports different ways to refresh cached data**:\
1️⃣ **Time-based Revalidation** → Data updates automatically after X seconds.\
2️⃣ **On-demand Revalidation** → Data updates manually via an API call.\
3️⃣ **Revalidation After User Action** → Data updates after a user interaction.

***

### **6. Example: Fetching Data with Automatic Caching**

By default, `fetch()` in Next.js **caches API responses**.

📌 **Code Example (Data Caching with fetch())**

```javascript
export default async function Page() {
  const response = await fetch('https://api.example.com/data'); // Cached by default
  const data = await response.json();

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

✅ **Data is cached** and served quickly.\
🛑 **Problem:** The data never updates unless we revalidate.

***

### **7. Example: Revalidating Data on Demand**

We can **force revalidation** when needed using `fetch()` with `{ cache: 'no-store' }`.

📌 **Code Example (Disable Caching for Every Request)**

```javascript
export default async function Page() {
  const response = await fetch('https://api.example.com/data', { cache: 'no-store' });
  const data = await response.json();

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

✅ **Always fetches fresh data** (no caching).\
🛑 **Problem:** Slower performance (API call on every request).

***

### **8. Example: Revalidating at Regular Intervals**

We can **set an interval** to refresh cached data using `{ next: { revalidate: X } }`.

📌 **Code Example (Revalidate Every 60 Seconds)**

```javascript
export default async function Page() {
  const response = await fetch('https://api.example.com/data', { next: { revalidate: 60 } });
  const data = await response.json();

  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

✅ **Data refreshes every 60 seconds** (better performance).\
✅ **Users see fresh data automatically** without a full page reload.

***

### **9. Example: Revalidating After a User Action**

We can **manually trigger revalidation** using `revalidatePath()`.

📌 **Steps:**\
1️⃣ Fetch data with caching.\
2️⃣ Add a **button to refresh data**.\
3️⃣ Use `revalidatePath('/page')` to refresh cached data.

📌 **Code Example (Trigger Revalidation Manually)**\
🔹 **API Route (`app/api/revalidate/route.js`)**

```javascript
import { revalidatePath } from 'next/cache';

export async function POST(request) {
  revalidatePath('/page'); // Refreshes cache for this page
  return Response.json({ revalidated: true });
}
```

🔹 **Page Component (`app/page.js`)**

```javascript
export default function Page() {
  async function refreshData() {
    await fetch('/api/revalidate', { method: 'POST' }); // Calls revalidation API
  }

  return (
    <div>
      <h1>Revalidate Data Example</h1>
      <button onClick={refreshData}>Refresh Data</button>
    </div>
  );
}
```

✅ **Users can manually refresh data** without a full reload.\
✅ **Cache updates only when needed** (better performance).

***

### **10. Conclusion**

✅ **Caching makes Next.js apps faster** by storing responses.\
✅ **Revalidation ensures data stays up-to-date** without full reloads.\
✅ **Use `fetch()` caching to optimize API requests**.\
✅ **Choose the right revalidation strategy:**

* Use `{ cache: 'no-store' }` for **always fresh data**.
* Use `{ next: { revalidate: X } }` for **automatic updates**.
* Use `revalidatePath()` for **manual cache updates**.
