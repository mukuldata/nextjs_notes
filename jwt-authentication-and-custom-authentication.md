# JWT authentication & custom authentication

### **Table of Contents**

1. Introduction to Authentication
2. What is JWT Authentication?
3. How JWT Authentication Works?
4. Installing Dependencies
5. Setting Up JWT Authentication in Next.js
   * Creating a JWT Token
   * Verifying a JWT Token
   * Storing & Using JWT
6. Custom Authentication (Email & Password)
7. Protecting Routes with JWT
8. Example: Full JWT Authentication in Next.js
9. Conclusion

***

### **1. Introduction to Authentication**

Authentication verifies **who a user is** before allowing access to protected resources.\
In Next.js, we can authenticate users using:\
✅ **OAuth Providers** (Google, GitHub, etc.) using NextAuth.js\
✅ **JWT (JSON Web Token) Authentication**\
✅ **Custom Authentication (Email & Password)**

***

### **2. What is JWT Authentication?**

JWT (JSON Web Token) is a **token-based authentication method**.

* It is **stateless** (does not store session on the server).
* The server **creates a token** with user data and sends it to the client.
* The client **stores the token** (local storage, cookies).
* The client **sends the token** with requests to access protected routes.
* The server **verifies the token** to allow or deny access.

***

### **3. How JWT Authentication Works?**

1️⃣ **User logs in with credentials** (email & password).\
2️⃣ **Server generates a JWT token** & sends it to the client.\
3️⃣ **Client stores JWT** (local storage or cookies).\
4️⃣ **Client sends JWT with every request** (Authorization header).\
5️⃣ **Server verifies JWT** & returns protected data.

👉 **JWT Structure:**\
A JWT consists of 3 parts:

```
Header.Payload.Signature
```

Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjEsInJvbGUiOiJ1c2VyIn0.abc123xyz
```

***

### **4. Installing Dependencies**

Install `jsonwebtoken` for creating and verifying JWTs:

```bash
npm install jsonwebtoken bcryptjs
```

***

### **5. Setting Up JWT Authentication in Next.js**

#### **A. Creating a JWT Token**

Create a utility function to **generate a JWT token**:

📂 `utils/jwt.js`

```javascript
import jwt from "jsonwebtoken";

export function generateToken(user) {
  return jwt.sign(
    { id: user.id, email: user.email }, // Payload
    process.env.JWT_SECRET,  // Secret key
    { expiresIn: "1h" }  // Token expiry
  );
}
```

✅ This function **creates a JWT** with a user ID & email.

👉 **Store secret key in `.env.local`**

```
JWT_SECRET=your_secret_key_here
```

***

#### **B. Verifying a JWT Token**

📂 `utils/jwt.js`

```javascript
export function verifyToken(token) {
  try {
    return jwt.verify(token, process.env.JWT_SECRET);
  } catch (error) {
    return null;
  }
}
```

✅ This function **decodes & verifies** the JWT token.

***

#### **C. Storing & Using JWT**

* **Option 1:** Store JWT in **localStorage** (not secure).
* **Option 2:** Store JWT in **HTTP-only cookies** (more secure).

Example: Storing JWT in HTTP-only cookie:\
📂 `app/api/auth/login/route.js`

```javascript
import { NextResponse } from "next/server";
import bcrypt from "bcryptjs";
import { generateToken } from "@/utils/jwt";

// Dummy user database (Replace with DB query)
const users = [{ id: 1, email: "test@example.com", password: "hashedpassword" }];

export async function POST(req) {
  const { email, password } = await req.json();

  const user = users.find((u) => u.email === email);
  if (!user) return NextResponse.json({ error: "User not found" }, { status: 401 });

  // Check password (bcrypt.compare for real applications)
  if (password !== "password123") {
    return NextResponse.json({ error: "Invalid password" }, { status: 401 });
  }

  // Generate JWT token
  const token = generateToken(user);

  // Store token in HTTP-only cookie
  const response = NextResponse.json({ message: "Logged in" });
  response.cookies.set("token", token, { httpOnly: true });

  return response;
}
```

✅ **User logs in → Token is stored in HTTP-only cookie.**

***

### **6. Custom Authentication (Email & Password)**

#### **A. Register User (Signup)**

📂 `app/api/auth/register/route.js`

```javascript
import { NextResponse } from "next/server";
import bcrypt from "bcryptjs";

// Dummy database
const users = [];

export async function POST(req) {
  const { email, password } = await req.json();

  // Hash password
  const hashedPassword = await bcrypt.hash(password, 10);
  
  // Store user (Replace with DB)
  users.push({ id: users.length + 1, email, password: hashedPassword });

  return NextResponse.json({ message: "User registered successfully" });
}
```

✅ **Registers user & stores hashed password.**

***

#### **B. Protecting Routes with JWT**

📂 `app/api/protected/route.js`

```javascript
import { NextResponse } from "next/server";
import { verifyToken } from "@/utils/jwt";

export async function GET(req) {
  const token = req.cookies.get("token")?.value;
  if (!token) return NextResponse.json({ error: "Unauthorized" }, { status: 401 });

  const user = verifyToken(token);
  if (!user) return NextResponse.json({ error: "Invalid token" }, { status: 403 });

  return NextResponse.json({ message: "Protected data", user });
}
```

✅ **Only allows access if the JWT is valid.**

***

#### **C. Logout User**

📂 `app/api/auth/logout/route.js`

```javascript
import { NextResponse } from "next/server";

export async function GET() {
  const response = NextResponse.json({ message: "Logged out" });
  response.cookies.set("token", "", { expires: new Date(0) }); // Remove token
  return response;
}
```

✅ **Clears token from cookies to log out user.**

***

### **7. Example: Full JWT Authentication in Next.js**

* **Register User** (`POST /api/auth/register`)
* **Login User** (`POST /api/auth/login`)
* **Store JWT in Cookie**
* **Access Protected Route** (`GET /api/protected`)
* **Logout User** (`GET /api/auth/logout`)

***

### **8. Conclusion**

🚀 **JWT Authentication is a secure & scalable authentication method.**\
✅ **Stateless** (no sessions needed).\
✅ **Supports API authentication for mobile & web apps.**\
✅ **Secure when used with HTTP-only cookies.**
