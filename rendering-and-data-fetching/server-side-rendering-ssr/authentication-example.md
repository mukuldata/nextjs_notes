# Authentication example

### **Table of Contents**

1. Introduction
2. What is SSR with Authentication?
3. How SSR Works with Authentication in Next.js 13+
4. Example: Implementing SSR with Authentication
5. Benefits of Using SSR for Authentication
6. When to Use SSR for Authentication?
7. Summary

***

### **1. Introduction**

1. In web applications, authentication is a crucial part of managing **user access and security**. Next.js 13+ allows us to **fetch user authentication details on the server** before rendering the page, ensuring **secure and fast data delivery**.

***

### **2. What is SSR with Authentication?**

#### 🔹 **Definition:**

* **Server-Side Rendering (SSR) with authentication** means fetching the **user’s authentication status** on the server **before** sending the page to the client.
* This ensures that **only authenticated users can access protected pages**.

#### 🔹 **Key Benefits:**

✔ **Better Security** – Authentication logic runs on the server, hiding sensitive data from the client.\
✔ **SEO-Friendly** – The fully rendered page is sent to the browser, improving SEO.\
✔ **Faster Initial Load** – Since authentication happens on the server, no extra client-side requests are needed.

***

### **3. How SSR Works with Authentication in Next.js 13+?**

#### **🔹 Process Flow:**

1. **User Requests a Protected Page** – Example: `/dashboard`.
2. **Server Checks Authentication** – The server verifies the user’s session or token.
3. **Page is Rendered Accordingly**
   * If authenticated, the user **sees the protected content**.
   * If **not authenticated**, they are redirected to the **login page**.

***

### **4. Example: Implementing SSR with Authentication in Next.js 13+**

Next.js 13+ (App Router) **does not use `getServerSideProps`**. Instead, we use **middleware** or fetch session data inside a **server component**.

#### **📁 app/dashboard/page.js** (Protected Dashboard Page)

```javascript
import { cookies } from "next/headers";
import { redirect } from "next/navigation";

export default async function Dashboard() {
  // Get authentication token from cookies
  const cookieStore = cookies();
  const token = cookieStore.get("authToken");

  // If no token, redirect to login page
  if (!token) {
    redirect("/login");
  }

  // Fetch user details using the token
  const response = await fetch("https://api.example.com/user", {
    headers: {
      Authorization: `Bearer ${token.value}`,
    },
    cache: "no-store", // Ensure fresh data
  });

  const user = await response.json();

  return (
    <div>
      <h1>Welcome, {user.name}!</h1>
      <p>Email: {user.email}</p>
    </div>
  );
}
```

#### **📁 app/login/page.js** (Login Page)

```javascript
"use client";

import { useState } from "react";
import { useRouter } from "next/navigation";

export default function Login() {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const router = useRouter();

  async function handleLogin(event) {
    event.preventDefault();

    const response = await fetch("/api/auth", {
      method: "POST",
      body: JSON.stringify({ email, password }),
      headers: { "Content-Type": "application/json" },
    });

    if (response.ok) {
      router.push("/dashboard"); // Redirect to dashboard after login
    } else {
      alert("Invalid credentials");
    }
  }

  return (
    <form onSubmit={handleLogin}>
      <input type="email" placeholder="Email" onChange={(e) => setEmail(e.target.value)} />
      <input type="password" placeholder="Password" onChange={(e) => setPassword(e.target.value)} />
      <button type="submit">Login</button>
    </form>
  );
}
```

#### **📁 app/api/auth/route.js** (API Route for Authentication)

```javascript
import { NextResponse } from "next/server";

export async function POST(req) {
  const { email, password } = await req.json();

  // Simulated authentication logic
  if (email === "user@example.com" && password === "password123") {
    const response = NextResponse.json({ message: "Login successful" });

    // Set authentication cookie
    response.cookies.set("authToken", "secureRandomToken", {
      httpOnly: true,
      maxAge: 60 * 60 * 24, // 1 day
    });

    return response;
  }

  return NextResponse.json({ message: "Invalid credentials" }, { status: 401 });
}
```

***

### **5. Benefits of Using SSR for Authentication**

✅ **Improved Security:** Sensitive authentication logic is handled on the server.\
✅ **Faster Initial Load:** The user receives a fully rendered page without additional API calls.\
✅ **SEO-Friendly:** The protected page can still be indexed if needed.\
✅ **Better UX:** Users don’t see a loading state while authentication is checked.

***

### **6. When to Use SSR for Authentication?**

| Scenario                                  | Use SSR?       |
| ----------------------------------------- | -------------- |
| Dashboard with user-specific content      | ✅ Yes          |
| Admin panel with restricted access        | ✅ Yes          |
| Blog pages with public access             | ❌ No (Use SSG) |
| Highly interactive pages (e.g., chat app) | ❌ No (Use CSR) |

***

### **7. Summary**

* **SSR with authentication ensures user data is validated before rendering a page.**
* **Next.js 13+ handles authentication inside server components, using cookies and middleware.**
* **Protected pages redirect unauthenticated users to the login page.**
* **This method improves security, SEO, and performance.**
