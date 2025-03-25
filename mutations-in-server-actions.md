# Mutations in Server Actions

### **Table of Contents**

1. **Introduction to Mutations in Server Actions**
2. **Why Use Server Actions for Mutations?**
3. **Understanding Mutations vs Queries**
4. **Basic Example: Updating Data with Server Actions**
5. **Handling Form Submissions with Mutations**
6. **Using Mutations with a Database (Prisma Example)**
7. **Optimistic Updates with Server Actions**
8. **Error Handling in Mutations**
9. **Best Practices for Server Actions Mutations**
10. **Conclusion**

***

### **1. Introduction to Mutations in Server Actions**

1. In **Next.js 13+**, **Server Actions** allow developers to run server-side logic directly in React components without needing API routes.
2. &#x20;**Mutations** in this context refer to **modifying data** (e.g., creating, updating, or deleting records in a database).

✅ **No need for separate API endpoints**\
✅ **Improves performance by reducing client-server round trips**\
✅ **Direct interaction with databases, without `fetch()`**

***

### **2. Why Use Server Actions for Mutations?**

🔹 **Before Server Actions:** Developers used **API routes** (`app/api/...`) to handle mutations, requiring an extra request to modify data.\
🔹 **With Server Actions:** You can **mutate data directly** from a component, reducing complexity.

✅ **Better performance**\
✅ **Less boilerplate code**\
✅ **Simplified form handling**

***

### **3. Understanding Mutations vs Queries**

| Feature            | Queries                              | Mutations                           |
| ------------------ | ------------------------------------ | ----------------------------------- |
| **Purpose**        | Fetch/read data                      | Modify/write data                   |
| **Examples**       | `fetch users`, `get product details` | `update user info`, `delete a post` |
| **Implementation** | Can be cached and optimized          | Affects database or backend state   |

***

### **4. Basic Example: Updating Data with Server Actions**

Let’s say we have a **user profile** and we want to update the name.

#### **Step 1: Create a Server Action to Update User Data**

```tsx
"use server";

export async function updateUser(prevState: any, formData: FormData) {
  const name = formData.get("name")?.toString();
  
  if (!name) {
    return { error: "Name cannot be empty!" };
  }

  // Simulating database update
  return { success: `Updated name to ${name}` };
}
```

#### **Step 2: Create a Form with `useFormState`**

```tsx
"use client";

import { useFormState } from "react";
import { updateUser } from "./actions";

export default function ProfileForm() {
  const [state, formAction] = useFormState(updateUser, null);

  return (
    <form action={formAction}>
      <input type="text" name="name" placeholder="Enter new name" required />
      <button type="submit">Update Name</button>
      {state?.success && <p>{state.success}</p>}
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
    </form>
  );
}
```

✅ **Updates data without an API route**\
✅ **Handles form submission and validation seamlessly**

***

### **5. Handling Form Submissions with Mutations**

Let’s extend the example to **handle multiple fields**.

#### **Server Action: Update Name & Email**

```tsx
"use server";

export async function updateUser(prevState: any, formData: FormData) {
  const name = formData.get("name")?.toString();
  const email = formData.get("email")?.toString();

  if (!name || !email) {
    return { error: "Both fields are required!" };
  }

  return { success: `Updated user: ${name}, ${email}` };
}
```

#### **Client Component: Form for Name & Email**

```tsx
"use client";

import { useFormState } from "react";
import { updateUser } from "./actions";

export default function ProfileForm() {
  const [state, formAction] = useFormState(updateUser, null);

  return (
    <form action={formAction}>
      <input type="text" name="name" placeholder="Enter new name" required />
      <input type="email" name="email" placeholder="Enter new email" required />
      <button type="submit">Update</button>
      
      {state?.success && <p>{state.success}</p>}
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
    </form>
  );
}
```

🚀 **Now updates both name and email!**

***

### **6. Using Mutations with a Database (Prisma Example)**

Now, let’s connect it to **a real database** using Prisma.

#### **Install Prisma**

```sh
npm install @prisma/client
```

#### **Server Action: Update User in Database**

```tsx
"use server";
import { prisma } from "@/lib/prisma";

export async function updateUser(prevState: any, formData: FormData) {
  const id = formData.get("id")?.toString();
  const name = formData.get("name")?.toString();

  if (!id || !name) {
    return { error: "Invalid input!" };
  }

  await prisma.user.update({
    where: { id },
    data: { name },
  });

  return { success: `User updated successfully!` };
}
```

🔹 **Why is this better?**\
✅ **Directly updates the database**\
✅ **No need for a separate API route**

***

### **7. Optimistic Updates with Server Actions**

🔹 **Optimistic updates**: Update the UI **before the server response** to improve UX.

#### **Client Component: Optimistic Update**

```tsx
"use client";
import { useOptimistic } from "react";
import { updateUser } from "./actions";

export default function ProfileForm() {
  const [optimisticName, setOptimisticName] = useOptimistic("");

  const handleChange = (e) => {
    setOptimisticName(e.target.value);
  };

  return (
    <form action={updateUser}>
      <input
        type="text"
        name="name"
        placeholder="Enter new name"
        value={optimisticName}
        onChange={handleChange}
        required
      />
      <button type="submit">Update</button>
      <p>Optimistic name: {optimisticName}</p>
    </form>
  );
}
```

✅ **Smooth user experience**\
✅ **Reduces waiting time**

***

### **8. Error Handling in Mutations**

**Handle errors gracefully with `try/catch`.**

#### **Example: Catching Errors in a Server Action**

```tsx
"use server";

export async function updateUser(prevState: any, formData: FormData) {
  try {
    const name = formData.get("name")?.toString();
    if (!name) throw new Error("Name is required!");

    // Simulating database update
    return { success: `Updated name to ${name}` };
  } catch (error) {
    return { error: error.message };
  }
}
```

✔️ **Prevents crashes**\
✔️ **Displays user-friendly messages**

***

### **9. Best Practices for Server Actions Mutations**

✅ **Use Server Actions for forms that modify data**\
✅ **Keep the mutation logic inside the server action**\
✅ **Handle errors properly using `try/catch`**\
✅ **Use optimistic updates for a better user experience**\
✅ **Avoid using Server Actions for frequent updates (use WebSockets instead)**

***

### **10. Conclusion**

✅ **Mutations in Server Actions simplify data updates**\
✅ **No need for API routes**\
✅ **Works great with databases like Prisma**\
✅ **Optimistic updates improve UX**
