# Server Actions

### **Table of Contents**

1. **Introduction to React Server Actions**
2. **Why Use Server Actions?**
3. **How Server Actions Work**
4. **Setting Up a Simple Server Action**
5. **Using Server Actions for Form Handling**
6. **Server Actions vs API Routes**
7. **Best Practices and Limitations**
8. **Conclusion**

***

### **1. Introduction to React Server Actions**

1. React Server Actions, introduced in Next.js 14, allow developers to perform **server-side logic** directly within React components.&#x20;
2. This removes the need for separate API routes in many cases, making data mutations simpler and more efficient.

**Key Benefits:**\
✅ Reduce API boilerplate\
✅ Improve performance by avoiding unnecessary client-server communication\
✅ Simplify form handling and data mutations

***

### **2. Why Use Server Actions?**

Traditionally, in Next.js, handling form submissions and data mutations required **API routes** (`app/api/...`) or **client-side fetch requests**. With Server Actions:

* You can **directly call a server function** inside your component.
* No need to create separate API endpoints.
* Works seamlessly with **form submissions** and **data mutations**.

***

### **3. How Server Actions Work**

Server Actions use the **`"use server"` directive** to indicate that a function should be executed on the server.

#### **Example of a Simple Server Action**

```tsx
"use server";

export async function addData(formData: FormData) {
  const name = formData.get("name");
  console.log(`Saving ${name} to the database...`);
  // Simulate database operation
  return { message: "Data saved successfully!" };
}
```

* This function runs **only on the server** and never gets exposed to the client.

***

### **4. Setting Up a Simple Server Action**

#### **Example: Calling a Server Action from a Component**

```tsx
import { addData } from "./actions"; // Import the server function

export default function MyComponent() {
  async function handleSubmit(formData: FormData) {
    const response = await addData(formData);
    alert(response.message);
  }

  return (
    <form action={handleSubmit}>
      <input type="text" name="name" required />
      <button type="submit">Submit</button>
    </form>
  );
}
```

🚀 **No API route needed**—the form submission **directly calls** the server function.

***

### **5. Using Server Actions for Form Handling**

Server Actions make form handling **simpler** and **more efficient**.

#### **Example: Submitting a Form Without API Routes**

```tsx
"use server";

export async function saveUser(formData: FormData) {
  const email = formData.get("email");
  console.log(`Saving email: ${email}`);
  return { message: "User saved successfully!" };
}
```

```tsx
import { saveUser } from "./actions";

export default function SignupForm() {
  async function handleSignup(formData: FormData) {
    const result = await saveUser(formData);
    alert(result.message);
  }

  return (
    <form action={handleSignup}>
      <input type="email" name="email" required />
      <button type="submit">Sign Up</button>
    </form>
  );
}
```

🔥 **No need for `fetch()` calls or API routes**—the action runs directly on the server.

***

### **6. Server Actions vs API Routes**

| Feature          | Server Actions            | API Routes                                   |
| ---------------- | ------------------------- | -------------------------------------------- |
| Setup Complexity | Simple                    | Requires additional files                    |
| Performance      | More efficient            | Slightly slower due to extra network request |
| Security         | Runs only on server       | Needs proper authorization                   |
| Use Case         | Forms, small data updates | Full REST API endpoints                      |

**Best for:**\
✅ Forms\
✅ Small data mutations\
✅ Quick database updates

Use **API routes** when you need:\
❌ RESTful APIs\
❌ External API integrations\
❌ Authentication layers

***

### **7. Best Practices and Limitations**

🔹 **Best Practices:**

* Use **Server Actions** for form submissions and simple data updates.
* Keep **heavy computations and sensitive logic** inside the server function.
* Combine with **Server Components** for optimal performance.

🔹 **Limitations:**

* Cannot be used inside **Client Components** without importing explicitly.
* Not suitable for handling **real-time updates** (use WebSockets instead).

***

### **8. Conclusion**

* Server Actions **eliminate the need for API routes** in many cases.
* They improve **performance** by reducing unnecessary client-server communication.
* **Best for form submissions and small data mutations.**
* Use them wisely alongside **API routes** where needed.
