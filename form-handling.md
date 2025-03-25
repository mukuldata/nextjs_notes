# Form Handling

### **Table of Contents**

1. **Introduction to Form Handling Without API Routes**
2. **Why Use `useFormState`?**
3. **How `useFormState` Works**
4. **Setting Up a Simple Form with `useFormState`**
5. **Form Validation with `useFormState`**
6. **Updating UI Based on Form Submission State**
7. **Best Practices and Limitations**
8. **Conclusion**

***

### **1. Introduction to Form Handling Without API Routes**

1. In **traditional form handling**, developers often create **API routes** (`app/api/...`) to process form submissions.&#x20;
2. However, **Next.js 13+** introduced **React Server Actions** and **`useFormState`**, allowing developers to handle form submissions **without needing API routes**.

✅ **No API routes required**\
✅ **No need for client-side `fetch()` calls**\
✅ **Improves performance and security**

***

### **2. Why Use `useFormState`?**

**`useFormState`** is a React hook that helps manage form state efficiently.

🔹 **Benefits:**

* Keeps form **stateful** and **reactive**.
* Provides **immediate feedback** after submission.
* Works seamlessly with **React Server Actions**.

***

### **3. How `useFormState` Works**

`useFormState` manages the form's state by **updating the UI based on submission results**. It works in combination with **server actions**.

#### **Basic Structure**

1. Create a **server action** to handle form submission.
2. Use `useFormState` to manage form state.
3. Attach it to a form in a **Server Component**.

***

### **4. Setting Up a Simple Form with `useFormState`**

Let's create a form where users **submit their name**, and the server **returns a success message**.

#### **Step 1: Create a Server Action**

```tsx
"use server";

export async function submitForm(prevState: any, formData: FormData) {
  const name = formData.get("name")?.toString() || "";
  if (!name) {
    return { error: "Name is required!" };
  }
  return { message: `Hello, ${name}!` };
}
```

* The function runs **only on the server**.
* It **validates the input** and **returns a message**.

#### **Step 2: Create the Form Component with `useFormState`**

```tsx
"use client";

import { useFormState } from "react";
import { submitForm } from "./actions";

export default function NameForm() {
  const [state, formAction] = useFormState(submitForm, null);

  return (
    <form action={formAction}>
      <input type="text" name="name" placeholder="Enter your name" required />
      <button type="submit">Submit</button>
      {state?.message && <p>{state.message}</p>}
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
    </form>
  );
}
```

✅ **No need for API calls (`fetch()`) or extra client-side logic**\
✅ **Handles submission, validation, and state in one place**

***

### **5. Form Validation with `useFormState`**

Let’s extend our example to **handle multiple fields and errors**.

#### **Updated Server Action**

```tsx
"use server";

export async function registerUser(prevState: any, formData: FormData) {
  const email = formData.get("email")?.toString();
  const password = formData.get("password")?.toString();

  if (!email || !password) {
    return { error: "All fields are required!" };
  }
  if (password.length < 6) {
    return { error: "Password must be at least 6 characters." };
  }

  return { message: "User registered successfully!" };
}
```

#### **Updated Client Form**

```tsx
"use client";

import { useFormState } from "react";
import { registerUser } from "./actions";

export default function RegisterForm() {
  const [state, formAction] = useFormState(registerUser, null);

  return (
    <form action={formAction}>
      <input type="email" name="email" placeholder="Email" required />
      <input type="password" name="password" placeholder="Password" required />
      <button type="submit">Register</button>

      {state?.message && <p>{state.message}</p>}
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
    </form>
  );
}
```

🚀 **Now we have:**\
✅ **Email and password validation**\
✅ **Error handling inside the UI**

***

### **6. Updating UI Based on Form Submission State**

`useFormState` makes UI updates **seamless**.

🔹 **Example: Disabling the Submit Button During Submission**

```tsx
"use client";

import { useFormState } from "react";
import { registerUser } from "./actions";

export default function RegisterForm() {
  const [state, formAction] = useFormState(registerUser, null);
  const isSubmitting = state === null;

  return (
    <form action={formAction}>
      <input type="email" name="email" placeholder="Email" required />
      <input type="password" name="password" placeholder="Password" required />
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Submitting..." : "Register"}
      </button>

      {state?.message && <p>{state.message}</p>}
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
    </form>
  );
}
```

✔️ **Disables the submit button while processing**\
✔️ **Prevents duplicate submissions**

***

### **7. Best Practices and Limitations**

✅ **Best Practices:**

* Use `useFormState` for **forms that need server validation**.
* Keep form logic **inside the server action** for better security.
* **Always return an object** from the server action (`{ message: "", error: "" }`).
* Use a **Server Component** for better performance.

⚠️ **Limitations:**

* **Does not work inside Client Components directly** (must use `use client`).
* **Cannot be used for file uploads** (use API routes instead).
* **No support for real-time updates** (use WebSockets for that).

***

### **8. Conclusion**

✅ **`useFormState` eliminates the need for API routes**.\
✅ **Simplifies form handling and state management**.\
✅ **Great for form validation and UI updates**.\
✅ **Use it for improved performance and better DX (Developer Experience).**
