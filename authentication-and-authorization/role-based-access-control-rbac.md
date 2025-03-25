# Role-based access control (RBAC)

### **Table of Contents**

1. What is Role-Based Access Control (RBAC)?
2. Why Use RBAC?
3. How RBAC Works in Next.js
4. Setting Up RBAC in Next.js
5. Implementing RBAC in API Routes
6. Protecting Pages Based on User Roles
7. Middleware for Role-Based Authentication
8. Full Example of RBAC in Next.js
9. Conclusion

***

### **1. What is Role-Based Access Control (RBAC)?**

RBAC (Role-Based Access Control) is a security model where users are assigned **roles**, and these roles define what actions they can perform.

📌 **Example Roles:**

* `admin` → Can manage users, view all data, and delete records.
* `editor` → Can edit content but cannot delete users.
* `user` → Can view personal data but cannot edit or delete anything.

🚀 **RBAC ensures users can only access what they are allowed to.**

***

### **2. Why Use RBAC?**

✅ **Better Security** – Prevent unauthorized access.\
✅ **Easy Management** – Assign roles instead of managing individual permissions.\
✅ **Scalability** – Works well as the app grows.

***

### **3. How RBAC Works in Next.js?**

1️⃣ **User logs in and gets a token (JWT) with their role**.\
2️⃣ **Role is stored in cookies or session storage**.\
3️⃣ **Middleware checks role before allowing access**.\
4️⃣ **Only users with the correct role can access certain pages or API routes**.

***

### **4. Setting Up RBAC in Next.js**

📂 **Install dependencies**

```bash
npm install jsonwebtoken
```

📂 **Project Structure**

```
app/
 ├── api/
 │   ├── auth/
 │   │   ├── login/route.js
 │   ├── protected/route.js
 ├── middleware.js
 ├── dashboard/page.js
 ├── admin/page.js
 ├── editor/page.js
```

***

### **5. Implementing RBAC in API Routes**

📌 **Step 1: Create a Login API that Generates a JWT with Roles**

📂 `app/api/auth/login/route.js`

```javascript
import { NextResponse } from "next/server";
import jwt from "jsonwebtoken";

const users = {
  "admin@example.com": { password: "admin123", role: "admin" },
  "editor@example.com": { password: "editor123", role: "editor" },
  "user@example.com": { password: "user123", role: "user" },
};

const SECRET_KEY = "my_secret_key"; // Use env variable in production

export async function POST(req) {
  const { email, password } = await req.json();
  
  if (!users[email] || users[email].password !== password) {
    return NextResponse.json({ error: "Invalid credentials" }, { status: 401 });
  }

  const token = jwt.sign({ email, role: users[email].role }, SECRET_KEY, {
    expiresIn: "1h",
  });

  const response = NextResponse.json({ message: "Login successful" });
  response.cookies.set("token", token, { httpOnly: true });

  return response;
}
```

✅ **Stores user roles inside a JWT token**\
✅ **Sends token as an HTTP-only cookie**

***

📌 **Step 2: Create a Protected API Route that Checks Roles**

📂 `app/api/protected/route.js`

```javascript
import { NextResponse } from "next/server";
import jwt from "jsonwebtoken";

const SECRET_KEY = "my_secret_key";

export async function GET(req) {
  const token = req.cookies.get("token")?.value;

  if (!token) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }

  try {
    const decoded = jwt.verify(token, SECRET_KEY);

    if (decoded.role !== "admin") {
      return NextResponse.json({ error: "Forbidden" }, { status: 403 });
    }

    return NextResponse.json({ message: "Welcome, Admin!" });
  } catch (error) {
    return NextResponse.json({ error: "Invalid token" }, { status: 401 });
  }
}
```

✅ **Allows only `admin` to access the protected route**\
✅ **Returns `403 Forbidden` for other roles**

***

### **6. Protecting Pages Based on User Roles**

📌 **Step 1: Create a Helper Function to Check Roles**

📂 `utils/auth.js`

```javascript
import jwt from "jsonwebtoken";

const SECRET_KEY = "my_secret_key";

export function getUserRole(req) {
  const token = req.cookies?.get("token")?.value;
  if (!token) return null;

  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    return decoded.role;
  } catch {
    return null;
  }
}
```

📌 **Step 2: Protect Admin Page**

📂 `app/admin/page.js`

```javascript
import { getUserRole } from "@/utils/auth";
import { redirect } from "next/navigation";

export default function AdminPage({ req }) {
  const role = getUserRole(req);

  if (role !== "admin") {
    redirect("/login"); // Redirect if not admin
  }

  return <h1>Welcome Admin</h1>;
}
```

✅ **Only `admin` can access `/admin` page**\
✅ **Other users will be redirected to `/login`**

***

### **7. Middleware for Role-Based Authentication**

📌 **Apply Role-Based Restrictions Globally Using Middleware**

📂 `middleware.js`

```javascript
import { NextResponse } from "next/server";
import jwt from "jsonwebtoken";

const SECRET_KEY = "my_secret_key";

export function middleware(req) {
  const token = req.cookies.get("token")?.value;

  if (!token) {
    return NextResponse.redirect(new URL("/login", req.url));
  }

  try {
    const decoded = jwt.verify(token, SECRET_KEY);
    const role = decoded.role;
    const path = req.nextUrl.pathname;

    // Role-based access control
    if (path.startsWith("/admin") && role !== "admin") {
      return NextResponse.redirect(new URL("/unauthorized", req.url));
    }
    if (path.startsWith("/editor") && !["admin", "editor"].includes(role)) {
      return NextResponse.redirect(new URL("/unauthorized", req.url));
    }
  } catch {
    return NextResponse.redirect(new URL("/login", req.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: "/:path*",
};
```

✅ **Automatically blocks users based on role**\
✅ **Redirects unauthorized users to `/unauthorized`**

***

### **8. Full Example of RBAC in Next.js**

1️⃣ User logs in and gets a token with their **role**.\
2️⃣ Middleware **checks the role** before allowing access.\
3️⃣ If unauthorized, the user is **redirected**.\
4️⃣ Protected API routes only **allow specific roles**.

***

### **9. Conclusion**

🚀 **Role-Based Access Control (RBAC) helps secure your Next.js app by controlling user access.**\
✅ **Assigns roles (admin, editor, user)**\
✅ **Protects API routes & pages**\
✅ **Uses middleware for centralized access control**
