# Environment variables

### **Table of Contents**

1. What are Environment Variables?
2. Why Use Environment Variables in Next.js?
3. Setting Up Environment Variables (.env.local)
4. Using Environment Variables in Next.js
5. Public vs. Private Environment Variables
6. Best Practices for Handling Environment Variables
7. Loading Environment Variables Correctly
8. Conclusion

***

### **1. What are Environment Variables?**

Environment variables are **key-value pairs** used to store **sensitive or configurable data** outside the codebase. They help keep credentials, API keys, database URLs, and other important settings secure.

Example:

```
DATABASE_URL=mongodb://username:password@localhost:27017/mydb
NEXT_PUBLIC_API_URL=https://api.example.com
```

***

### **2. Why Use Environment Variables in Next.js?**

✅ **Security** – Keep sensitive information like API keys hidden.\
✅ **Configuration Flexibility** – Easily switch between development, staging, and production setups.\
✅ **Code Cleanliness** – Avoid hardcoding values in source files.

***

### **3. Setting Up Environment Variables (.env.local)**

Next.js supports environment variables through `.env` files.

#### **Creating an `.env.local` File**

Inside your Next.js project, create a new file:

```
.env.local
```

Add variables inside:

```ini
NEXT_PUBLIC_API_URL=https://api.example.com
DATABASE_URL=mongodb://username:password@localhost:27017/mydb
SECRET_KEY=my-secret-key
```

📌 **`.env.local` is automatically ignored in Git**, so secrets remain safe.

***

### **4. Using Environment Variables in Next.js**

#### **4.1 Accessing Environment Variables in Next.js**

Inside **server-side code** (e.g., API routes, `getServerSideProps`, `getStaticProps`), use:

```javascript
console.log(process.env.DATABASE_URL);
```

Inside **client-side code (React components)**, use variables prefixed with `NEXT_PUBLIC_`:

```javascript
console.log(process.env.NEXT_PUBLIC_API_URL);
```

🔹 **Only `NEXT_PUBLIC_*` variables are accessible in the browser!**

***

### **5. Public vs. Private Environment Variables**

| Type        | Example                      | Available in Client (Browser)? | Available in Server (API, SSR)? |
| ----------- | ---------------------------- | ------------------------------ | ------------------------------- |
| **Public**  | `NEXT_PUBLIC_API_URL`        | ✅ Yes                          | ✅ Yes                           |
| **Private** | `DATABASE_URL`, `SECRET_KEY` | ❌ No                           | ✅ Yes                           |

📌 **Never expose private variables (like database credentials) to the frontend!**

***

### **6. Best Practices for Handling Environment Variables**

✅ **Use `.env.local` for local development** and `.env.production` for production.\
✅ **Avoid committing secrets** to Git by adding `.env*` to `.gitignore`.\
✅ **Use `NEXT_PUBLIC_` for variables that must be accessible in the browser.**\
✅ **Restart the server** after modifying environment variables.

***

### **7. Loading Environment Variables Correctly**

*   **In API Routes (`/pages/api/`)**

    ```javascript
    export default function handler(req, res) {
      res.json({ message: `Database URL is ${process.env.DATABASE_URL}` });
    }
    ```
*   **In `getServerSideProps` (SSR)**

    ```javascript
    export async function getServerSideProps() {
      return { props: { secretKey: process.env.SECRET_KEY } };
    }
    ```
*   **In `getStaticProps` (SSG)**

    ```javascript
    export async function getStaticProps() {
      return { props: { apiUrl: process.env.NEXT_PUBLIC_API_URL } };
    }
    ```

📌 **Remember:** Variables are only available **during build time** in `getStaticProps`.

***

### **8. Conclusion**

✅ **Environment variables** allow secure and configurable settings.\
✅ **Use `NEXT_PUBLIC_` prefix** for frontend-accessible values.\
✅ **Restart the server** after updating `.env.local`.\
✅ **Never expose private variables** to the client-side.
