# Server-Side Rendering (SSR)

### **Table of Contents**

1. Introduction
2. What is Server-Side Rendering (SSR)?
3. How SSR Works in Next.js
4. Example of SSR in Next.js
5. Benefits of SSR
6. When to Use SSR?
7. Comparison: SSR vs. SSG vs. CSR
8. Summary

***

### **1. Introduction**

1. In web development, fetching and displaying data efficiently is crucial. **Server-Side Rendering (SSR)** is a rendering technique where a page is generated **on the server at the time of the request** and sent to the browser.

***

### **2. What is Server-Side Rendering (SSR)?**

#### 🔹 **Definition:**

* SSR means **generating the HTML for a page on the server at runtime** before sending it to the user’s browser.
* Each request generates a **fresh** HTML page, including dynamic data.

#### 🔹 **Key Characteristics:**

✔ **Fresh Data:** The page always contains up-to-date information.\
✔ **Better SEO:** Since HTML is generated before reaching the browser, search engines can easily index the content.\
✔ **Slower Initial Load:** The server needs time to generate the page before sending it.

***

### **3. How SSR Works in Next.js?**

Next.js uses **server components** and `getServerSideProps()` (for pages using the `pages/` directory) to enable SSR.

#### **🔹 Process Flow:**

1. User requests a page (e.g., `/news`).
2. Next.js **executes the page’s code on the server** and fetches required data.
3. The server **generates the HTML page with data** and sends it to the browser.
4. The browser **displays the fully-rendered page** to the user.

***

### **4. Example of SSR in Next.js 13+**

Next.js 13+ **does not use `getServerSideProps` anymore** in the App Router (`app/` directory). Instead, **React Server Components** and the `fetch()` function are used directly inside components.

#### 📁 **app/news/page.js** (Fetching news articles on the server)

```javascript
export default async function NewsPage() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    cache: "no-store", // Ensures fresh data on every request (SSR)
  });
  const articles = await response.json();

  return (
    <div>
      <h1>Latest News</h1>
      <ul>
        {articles.slice(0, 5).map((article) => (
          <li key={article.id}>
            <h3>{article.title}</h3>
            <p>{article.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

#### **Explanation:**

✔ `fetch()` is called **inside the server component**, so it runs on the server.\
✔ **`cache: "no-store"`** ensures fresh data on every request.\
✔ The server fetches the latest news **each time the user loads the page**.

***

### **5. Benefits of SSR**

✅ **SEO-Friendly:** Since the HTML is ready before reaching the browser, search engines can easily index the content.\
✅ **Up-to-Date Data:** Always fetches the latest data from APIs/databases.\
✅ **Better Performance for Large Pages:** Useful for pages with **real-time** or **frequently updated data**.\
✅ **Faster First Paint:** The browser receives a **fully rendered page**, improving perceived speed.

***

### **6. When to Use SSR?**

| Scenario                                        | Use SSR?           |
| ----------------------------------------------- | ------------------ |
| Frequently updated data (news, stock prices)    | ✅ Yes              |
| E-commerce product pages with real-time pricing | ✅ Yes              |
| Personalized user dashboards                    | ✅ Yes              |
| Static blogs or documentation                   | ❌ No (Use **SSG**) |
| Public content that rarely changes              | ❌ No (Use **SSG**) |

***

### **7. Comparison: SSR vs. SSG vs. CSR**

| Feature       | SSR (Server-Side Rendering)          | SSG (Static Site Generation)              | CSR (Client-Side Rendering)               |
| ------------- | ------------------------------------ | ----------------------------------------- | ----------------------------------------- |
| Data Fetching | On every request                     | At build time                             | In the browser                            |
| Performance   | Slower than SSG, but fresher data    | Fast, but may show outdated data          | Fast initial load, but slow data fetch    |
| SEO           | ✅ Good                               | ✅ Best                                    | ❌ Less SEO-friendly                       |
| Use Case      | **Real-time content** (news, prices) | **Rarely changing content** (blogs, docs) | **Highly interactive pages** (dashboards) |

***

### **8. Summary**

* **SSR generates pages on the server at request time.**
* It ensures **fresh data** but can be **slower than static generation (SSG)**.
* **Next.js 13+ uses Server Components for SSR**, replacing `getServerSideProps()`.
* **Ideal for SEO-heavy and real-time content pages.**
