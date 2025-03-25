# Middleware-based authentication

### **Table of Contents**

1. Introduction to Middleware
2. Why Use Middleware for Authentication?
3. How Middleware-Based Authentication Works
4. Setting Up Middleware in Next.js
5. Implementing Authentication Middleware
6. Protecting Routes with Middleware
7. Redirecting Unauthorized Users
8. Example: Full Authentication Middleware in Next.js
9. Conclusion

***

### **1. Introduction to Middleware**

Middleware in Next.js is a function that runs **before a request is processed**.

* It is executed **at the Edge** (before hitting the server).
* It can be used for **authentication, logging, redirects, and security checks**.

👉 **Middleware runs before API routes and page requests.**

***

### **2. Why Use Middleware for Authentication?**

✅ **Protects multiple routes** without adding logic to each page.\
✅ **Improves performance** by blocking requests **before they reach the server**.\
✅ **Handles authentication in a centralized way**.\
✅ **Works for API routes & page requests**.

***

### **3. How Middleware-Based Authentication Works**

1️⃣ User makes a request to a **protected page or API**.\
2️⃣ Middleware **checks if the user has a valid token** (JWT stored in cookies).\
3️⃣ If the token is valid, the request proceeds ✅.\
4️⃣ If the token is **missing or invalid**, the user is redirected to the **login page** 🚫.

***

### **4. Setting Up Middleware in Next.js**

📂 **Create a `middleware.js` file** in the root of the project:

```bash
touch middleware.js
```

***

### **5. Implementing Authentication Middleware**

📂 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(req) {
  // Get token from cookies
  const token = req.cookies.get("token")?.value;

  // List of protected routes
  const protectedRoutes = ["/dashboard", "/profile", "/settings"];

  // Check if the request is for a protected route
  if (protectedRoutes.includes(req.nextUrl.pathname)) {
    if (!token) {
      // Redirect to login if not authenticated
      return NextResponse.redirect(new URL("/login", req.url));
    }
  }

  return NextResponse.next();
}

// Apply middleware to all routes
export const config = {
  matcher: "/:path*",
};
```

✅ **How it works?**

* The middleware **checks the token** stored in cookies.
* If the user tries to access `/dashboard`, `/profile`, or `/settings` **without a token**, they are **redirected to the login page**.
* If the user is authenticated, the request **continues** to the protected page.

***

### **6. Protecting Routes with Middleware**

#### **A. Securing API Routes**

For API routes, the middleware runs before requests hit the API.

Example: Secure API route `/api/protected`\
📂 `app/api/protected/route.js`

```javascript
import { NextResponse } from "next/server";

export async function GET(req) {
  const token = req.cookies.get("token")?.value;

  if (!token) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  return NextResponse.json({ message: "Protected API Data" });
}
```

✅ **Ensures that only authenticated users can access this API.**

***

### **7. Redirecting Unauthorized Users**

#### **A. Redirecting to Login Page**

📂 `middleware.js`

```javascript
export function middleware(req) {
  const token = req.cookies.get("token")?.value;
  const protectedRoutes = ["/dashboard", "/profile"];

  if (protectedRoutes.includes(req.nextUrl.pathname)) {
    if (!token) {
      return NextResponse.redirect(new URL("/login", req.url));
    }
  }

  return NextResponse.next();
}
```

✅ **If the user is not logged in, they are redirected to `/login`.**

#### **B. Redirecting Logged-in Users Away from Login/Register Pages**

If the user **already has a token**, they shouldn't see the login/register pages.

📂 `middleware.js`

```javascript
export function middleware(req) {
  const token = req.cookies.get("token")?.value;

  if (token && ["/login", "/register"].includes(req.nextUrl.pathname)) {
    return NextResponse.redirect(new URL("/dashboard", req.url));
  }

  return NextResponse.next();
}
```

✅ **Logged-in users will be redirected to `/dashboard` if they try to access `/login`.**

***

### **8. Example: Full Authentication Middleware in Next.js**

1️⃣ **Login API (`POST /api/auth/login`)**\
📂 `app/api/auth/login/route.js`

```javascript
import { NextResponse } from "next/server";
import { generateToken } from "@/utils/jwt";

export async function POST(req) {
  const { email, password } = await req.json();

  if (email !== "test@example.com" || password !== "password123") {
    return NextResponse.json({ error: "Invalid credentials" }, { status: 401 });
  }

  const token = generateToken({ email });

  // Store token in HTTP-only cookie
  const response = NextResponse.json({ message: "Logged in" });
  response.cookies.set("token", token, { httpOnly: true });

  return response;
}
```

2️⃣ **Logout API (`GET /api/auth/logout`)**\
📂 `app/api/auth/logout/route.js`

```javascript
export async function GET() {
  const response = NextResponse.json({ message: "Logged out" });
  response.cookies.set("token", "", { expires: new Date(0) }); // Clear token
  return response;
}
```

3️⃣ **Middleware to Protect Routes**\
📂 `middleware.js`

```javascript
import { NextResponse } from "next/server";

export function middleware(req) {
  const token = req.cookies.get("token")?.value;
  const protectedRoutes = ["/dashboard", "/profile"];

  if (protectedRoutes.includes(req.nextUrl.pathname)) {
    if (!token) {
      return NextResponse.redirect(new URL("/login", req.url));
    }
  }

  return NextResponse.next();
}

export const config = {
  matcher: "/:path*",
};
```

***

### **9. Conclusion**

🚀 **Middleware-Based Authentication is a powerful way to secure routes in Next.js.**\
✅ **Protects routes globally without modifying individual pages**\
✅ **Enhances performance by blocking unauthorized requests at the Edge**\
✅ **Handles redirects & access control efficiently**
