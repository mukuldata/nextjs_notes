# NextAuth.js integration

### **Table of Contents**

1. Introduction to NextAuth.js
2. Why Use NextAuth.js?
3. Installing NextAuth.js in Next.js 13+
4. Setting Up Authentication Providers
5. Creating API Route for Authentication
6. Adding Authentication to a Page
7. Protecting Pages (Middleware & Session Handling)
8. Customizing Authentication (Callbacks, Credentials)
9. Example: Google Authentication
10. Conclusion

***

### **1. Introduction to NextAuth.js**

NextAuth.js  is a **popular authentication library** for Next.js that supports:

* **OAuth Providers** (Google, GitHub, Facebook, etc.)
* **Email Authentication**
* **Credentials-Based Authentication**
* **JWT (JSON Web Token) & Database Sessions**

It makes authentication **easy** with minimal setup.

***

### **2. Why Use NextAuth.js?**

✅ **Easy to integrate** with OAuth providers like Google, GitHub.\
✅ **Supports Server & Client Authentication**.\
✅ **Built-in Security** with CSRF Protection.\
✅ **Works with Next.js App Router (`app/`) & Pages Router (`pages/`)**.\
✅ **Session Management & JWT Support**.

***

### **3. Installing NextAuth.js in Next.js 13+**

First, install NextAuth.js:

```bash
npm install next-auth
```

If you're using TypeScript, install the types:

```bash
npm install --save-dev @types/next-auth
```

***

### **4. Setting Up Authentication Providers**

NextAuth.js works with multiple authentication **providers** (Google, GitHub, etc.).

👉 **For this example, we’ll use Google Authentication.**\
Go to [Google Developer Console](https://console.cloud.google.com/):

1. Create a new project.
2. Enable **OAuth consent screen** & create credentials.
3. Copy **Client ID** and **Client Secret**.

Now, create a **`.env.local`** file in your project and add:

```
NEXTAUTH_SECRET=your_secret_key
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
NEXTAUTH_URL=http://localhost:3000
```

👉 **Generate a secret key** using:

```bash
openssl rand -base64 32
```

***

### **5. Creating API Route for Authentication**

NextAuth.js requires an **API route** to handle authentication.

👉 **For Pages Router (`pages/`)**\
📂 `pages/api/auth/[...nextauth].js`

```javascript
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export default NextAuth({
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
  secret: process.env.NEXTAUTH_SECRET,
});
```

👉 **For App Router (`app/`)**\
📂 `app/api/auth/[...nextauth]/route.js`

```javascript
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export const authOptions = {
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
  secret: process.env.NEXTAUTH_SECRET,
};

const handler = NextAuth(authOptions);
export { handler as GET, handler as POST };
```

✅ **This sets up Google authentication** in NextAuth.js.

***

### **6. Adding Authentication to a Page**

Now, we’ll create a simple **login/logout** page.

📂 `app/login/page.js`

```javascript
'use client';
import { signIn, signOut, useSession } from "next-auth/react";

export default function LoginPage() {
  const { data: session } = useSession();

  return (
    <div>
      {session ? (
        <div>
          <p>Welcome, {session.user.name}</p>
          <button onClick={() => signOut()}>Sign Out</button>
        </div>
      ) : (
        <button onClick={() => signIn("google")}>Sign In with Google</button>
      )}
    </div>
  );
}
```

✅ **What does this do?**

* If **user is logged in**, show their name & **Sign Out** button.
* If **user is not logged in**, show **Sign In with Google** button.

***

### **7. Protecting Pages (Middleware & Session Handling)**

To **protect pages** (only allow logged-in users), use the `useSession` hook.

📂 `app/protected/page.js`

```javascript
'use client';
import { useSession } from "next-auth/react";
import { useRouter } from "next/navigation";

export default function ProtectedPage() {
  const { data: session, status } = useSession();
  const router = useRouter();

  if (status === "loading") return <p>Loading...</p>;

  if (!session) {
    router.push("/login");
    return null;
  }

  return <h1>Welcome to the Protected Page, {session.user.name}!</h1>;
}
```

✅ **What does this do?**

* If **user is not logged in**, redirect to **login page**.
* If **user is logged in**, show **protected content**.

***

### **8. Customizing Authentication (Callbacks, Credentials)**

NextAuth allows customization using **callbacks**.

#### **Example: Adding a Callback to Modify Session Data**

📂 `app/api/auth/[...nextauth]/route.js`

```javascript
export const authOptions = {
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
  callbacks: {
    async session({ session, user }) {
      session.user.role = "admin"; // Add custom data to session
      return session;
    },
  },
};
```

✅ **Now every session will include `role: "admin"`**.

***

### **9. Example: Google Authentication**

Now, let’s create a full authentication system.

#### **📌 Steps:**

1. Install `next-auth` ✅
2. Create **Google OAuth credentials** ✅
3. Configure **NextAuth API route** ✅
4. Create **Login Page with Sign In/Sign Out** ✅
5. Protect a page using **useSession** ✅

#### **📌 Final Code Summary**

📂 `app/api/auth/[...nextauth]/route.js`

```javascript
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export const authOptions = {
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
  ],
  secret: process.env.NEXTAUTH_SECRET,
};

const handler = NextAuth(authOptions);
export { handler as GET, handler as POST };
```

📂 `app/login/page.js`

```javascript
'use client';
import { signIn, signOut, useSession } from "next-auth/react";

export default function LoginPage() {
  const { data: session } = useSession();

  return (
    <div>
      {session ? (
        <div>
          <p>Welcome, {session.user.name}</p>
          <button onClick={() => signOut()}>Sign Out</button>
        </div>
      ) : (
        <button onClick={() => signIn("google")}>Sign In with Google</button>
      )}
    </div>
  );
}
```

✅ **Now your Next.js app supports authentication!**

***

### **10. Conclusion**

🚀 **NextAuth.js makes authentication easy in Next.js!**\
✅ Supports **Google, GitHub, Credentials-based Auth**.\
✅ Works with **App Router & Pages Router**.\
✅ Provides **session management & protected routes**.
