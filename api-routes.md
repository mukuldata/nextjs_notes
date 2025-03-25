# API Routes

### **Table of Contents**

1. Introduction
2. What are API Routes?
3. Creating a Simple API Route
4. Handling GET and POST Requests
5. Dynamic API Routes
6. Using API Routes with a Database
7. Summary

***

### **1. Introduction**

1. In **Next.js 13+**, API routes allow you to create **server-side endpoints** within your application.
2. &#x20;These routes work like a **backend** inside your Next.js project, handling requests and returning responses without needing an external server.

📌 **Key Benefits of API Routes:**\
✔ No need for a separate backend server\
✔ Works like a serverless function\
✔ Can connect with databases, APIs, and authentication

***

### **2. What are API Routes?**

API routes in Next.js **handle requests** (GET, POST, PUT, DELETE) inside the `app/api/` folder.

* They return **JSON responses** that can be used in the frontend.
* API routes are useful for **fetching data, handling forms, and authentication.**

✅ **Example API Endpoints:**

* `/api/hello` → Returns "Hello, API!"
* `/api/users` → Returns a list of users
* `/api/products/1` → Returns details of product 1

***

### **3. Creating a Simple API Route**

📁 **File Structure:**

```
app/  
│── api/  
│   ├── hello/  
│       ├── route.js  → "/api/hello"
```

📌 **Example: A Basic API Route**\
📁 `app/api/hello/route.js`

```javascript
export async function GET() {
  return Response.json({ message: "Hello, API!" });
}
```

🔹 **Available at:** `http://localhost:3000/api/hello`\
🔹 **Output (JSON response):**

```json
{
  "message": "Hello, API!"
}
```

***

### **4. Handling GET and POST Requests**

API routes can handle different request methods like **GET and POST**.

📁 `app/api/user/route.js`

```javascript
export async function GET() {
  return Response.json({ name: "John Doe", age: 30 });
}

export async function POST(request) {
  const data = await request.json(); // Get data from the request body
  return Response.json({ message: "User created", data });
}
```

#### **How to Test the API?**

1. **GET Request** (`http://localhost:3000/api/user`)
   *   Response:

       ```json
       {
         "name": "John Doe",
         "age": 30
       }
       ```
2. **POST Request with Body `{ "name": "Alice", "age": 25 }`**
   *   Response:

       ```json
       {
         "message": "User created",
         "data": { "name": "Alice", "age": 25 }
       }
       ```

***

### **5. Dynamic API Routes**

Dynamic API routes allow you to handle **parameters** in URLs, such as fetching user details by ID.

📁 **File Structure:**

```
app/  
│── api/  
│   ├── users/  
│       ├── [id]/  
│           ├── route.js  → "/api/users/:id"
```

📌 **Example: API Route for User ID**\
📁 `app/api/users/[id]/route.js`

```javascript
export async function GET(request, { params }) {
  const { id } = params; // Extract ID from URL
  return Response.json({ userId: id, name: "User " + id });
}
```

🔹 **Available at:** `http://localhost:3000/api/users/5`\
🔹 **Response:**

```json
{
  "userId": "5",
  "name": "User 5"
}
```

***

### **6. Using API Routes with a Database**

Next.js API routes can **fetch data from a database** like MongoDB, PostgreSQL, or Firebase.

📌 **Example: Fetching Users from a Database**\
📁 `app/api/users/route.js`

```javascript
import { connectToDatabase } from "@/lib/db"; // Import a database connection function

export async function GET() {
  const db = await connectToDatabase();
  const users = await db.collection("users").find().toArray();
  
  return Response.json(users);
}
```

🔹 **Available at:** `http://localhost:3000/api/users`\
🔹 **Response Example:**

```json
[
  { "id": 1, "name": "Alice" },
  { "id": 2, "name": "Bob" }
]
```

***

### **7. Summary**

* **API Routes** allow you to create backend functionality inside Next.js.
* **Route.js in `app/api/`** handles server-side requests and responses.
* Supports **GET, POST, PUT, DELETE** methods.
* Can be **dynamic** (`/api/users/:id`).
* Can connect to **databases** like MongoDB.
