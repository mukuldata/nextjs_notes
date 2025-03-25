# Middleware

### **Table of Contents**

1. Introduction
2. What is Middleware?
3. How Middleware Works in Next.js
4. Creating a Simple Middleware
5. Applying Middleware to Specific Routes
6. Middleware Use Cases
7. Summary

***

### **1. Introduction**

1. **Middleware** in Next.js is used to **run code before a request reaches a route**.
2. &#x20;It allows you to modify requests and responses before they are processed by the application.

📌 **Why Use Middleware?**\
✔ Redirect users based on authentication\
✔ Modify request headers or cookies\
✔ Protect API routes\
✔ Log user requests

***

### **2. What is Middleware?**

Middleware in Next.js is a **server-side function** that runs before a request reaches a page or API route.

* Middleware is placed inside the **root directory** (`middleware.js` or `middleware.ts`).
* It runs on **Edge Runtime**, meaning it's very fast.
* It can be used for **authentication, redirects, or custom request handling**.

***

### **3. How Middleware Works in Next.js**

Middleware runs **before** the request reaches the actual page. It can:

1. **Modify the request** (e.g., add headers, check authentication).
2. **Redirect the request** (e.g., send users to another page).
3. **Allow or block access** (e.g., restrict pages based on roles).

📌 **Flow of Middleware Execution:**

```
User Request → Middleware → Page/API Route → Response
```

***

### **4. Creating a Simple Middleware**

To create middleware, add a **`middleware.js`** file in the root of your Next.js project.

📁 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(request) {
  console.log("Middleware executed!");
  return NextResponse.next(); // Continue to the requested page
}
```

🔹 **Where does this run?**\
This runs for **every request** in the application.

***

### **5. Applying Middleware to Specific Routes**

You can run middleware only for **certain pages or API routes** using `config`.

📌 **Example: Apply Middleware Only to `/dashboard` Route**

```javascript
import { NextResponse } from "next/server";

export function middleware(request) {
  console.log("Middleware executed for /dashboard!");
  return NextResponse.next(); // Continue to requested page
}

// Apply only to /dashboard
export const config = {
  matcher: "/dashboard",
};
```

🔹 **Runs only when accessing `/dashboard`**, not other pages.

***

### **6. Middleware Use Cases**

✅ **1. Redirect Users Based on Authentication**\
📁 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(request) {
  const isLoggedIn = request.cookies.get("authToken"); // Check authentication

  if (!isLoggedIn) {
    return NextResponse.redirect(new URL("/login", request.url)); // Redirect to login
  }

  return NextResponse.next(); // Allow access
}

// Apply only to protected pages
export const config = {
  matcher: ["/dashboard", "/profile"],
};
```

🔹 **If user is not logged in, they are redirected to `/login`**.

✅ **2. Block Requests from Certain Countries**\
📁 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(request) {
  const country = request.geo?.country; // Get user's country

  if (country === "CN") {
    return NextResponse.rewrite(new URL("/blocked", request.url)); // Block users from China
  }

  return NextResponse.next();
}
```

🔹 **Users from China are redirected to `/blocked` page**.

✅ **3. Add Security Headers**\
📁 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(request) {
  const response = NextResponse.next();
  response.headers.set("X-Custom-Security", "enabled");
  return response;
}
```

🔹 **Adds a custom security header to all responses**.

***

### **7. Summary**

* **Middleware runs before page requests** and modifies them.
* It can **redirect users, add headers, or restrict access**.
* It is defined in **`middleware.js`** at the root level.
* `matcher` is used to **apply middleware to specific routes**.
* Useful for **authentication, logging, and security**.
