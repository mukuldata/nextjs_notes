# Mutations

### **Table of Contents**

1. Introduction
2. What Are Mutations?
3. Why Use React Query for Mutations?
4. Setting Up React Query in Next.js 13+
5. Example: **POST (Create) Data**
6. Example: **PUT (Update) Data**
7. Example: **DELETE (Remove) Data**
8. Summary

***

### **1. Introduction**

In Next.js, API requests are made to interact with a backend. While **GET requests** fetch data, **mutations (POST, PUT, DELETE)** modify data. React Query makes these operations easier by **handling API calls, caching, and automatic updates**.

***

### **2. What Are Mutations?**

Mutations are operations that **modify data on the server**, such as:\
✔ **POST** → Adding new data (Create)\
✔ **PUT/PATCH** → Updating existing data (Update)\
✔ **DELETE** → Removing data (Delete)

***

### **3. Why Use React Query for Mutations?**

| Feature                        | React Query | Traditional `fetch` |
| ------------------------------ | ----------- | ------------------- |
| Handles Loading & Error States | ✅ Yes       | ❌ No                |
| Auto Updates UI                | ✅ Yes       | ❌ No                |
| Caches Responses               | ✅ Yes       | ❌ No                |
| Optimistic Updates             | ✅ Yes       | ❌ No                |

React Query **automatically updates the UI after mutations**, preventing unnecessary page reloads.

***

### **4. Setting Up React Query in Next.js 13+**

To use React Query, install it first:

```sh
npm install @tanstack/react-query
```

Next, create a **React Query Provider** to wrap your app.

📁 **app/providers.js**

```javascript
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { useState } from "react";

export default function Providers({ children }) {
  const [queryClient] = useState(() => new QueryClient());

  return <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>;
}
```

Wrap your application with the provider:

📁 **app/layout.js**

```javascript
import Providers from "./providers";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

Now we can use **React Query mutations** in our components.

***

### **5. Example: POST (Create) Data**

Let's add a new user using a **POST request**.

📁 **app/users/page.js**

```javascript
"use client";

import { useMutation, useQueryClient } from "@tanstack/react-query";
import { useState } from "react";

export default function UsersPage() {
  const queryClient = useQueryClient();
  const [name, setName] = useState("");

  const addUserMutation = useMutation({
    mutationFn: async (newUser) => {
      const response = await fetch("/api/users", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(newUser),
      });
      return response.json();
    },
    onSuccess: () => {
      queryClient.invalidateQueries(["users"]); // Refresh users list
    },
  });

  const handleAddUser = () => {
    addUserMutation.mutate({ name });
    setName("");
  };

  return (
    <div>
      <h1>Add User</h1>
      <input value={name} onChange={(e) => setName(e.target.value)} placeholder="Enter name" />
      <button onClick={handleAddUser} disabled={addUserMutation.isLoading}>
        {addUserMutation.isLoading ? "Adding..." : "Add User"}
      </button>
      {addUserMutation.isError && <p>Error: {addUserMutation.error.message}</p>}
    </div>
  );
}
```

#### **How This Works:**

✔ **`useMutation` handles the POST request**\
✔ **`onSuccess` refreshes user data automatically**\
✔ **Prevents manual API calls after adding a user**

***

### **6. Example: PUT (Update) Data**

Let's update a user's name using a **PUT request**.

📁 **app/users/page.js**

```javascript
const updateUserMutation = useMutation({
  mutationFn: async ({ id, name }) => {
    const response = await fetch(`/api/users/${id}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ name }),
    });
    return response.json();
  },
  onSuccess: () => {
    queryClient.invalidateQueries(["users"]); // Refresh users list
  },
});

const handleUpdateUser = (id, newName) => {
  updateUserMutation.mutate({ id, name: newName });
};
```

📌 **Explanation:**\
✔ **Uses `useMutation` for PUT request**\
✔ **`onSuccess` refreshes user list**\
✔ **Auto-updates UI without reloading**

***

### **7. Example: DELETE (Remove) Data**

Now, let's delete a user using a **DELETE request**.

📁 **app/users/page.js**

```javascript
const deleteUserMutation = useMutation({
  mutationFn: async (id) => {
    await fetch(`/api/users/${id}`, { method: "DELETE" });
  },
  onSuccess: () => {
    queryClient.invalidateQueries(["users"]); // Refresh users list
  },
});

const handleDeleteUser = (id) => {
  deleteUserMutation.mutate(id);
};
```

📌 **Explanation:**\
✔ **Handles DELETE request with `useMutation`**\
✔ **Removes user from the list after success**\
✔ **No need to manually refresh the page**

***

### **8. Summary**

* **Mutations modify data** (POST, PUT, DELETE).
* **React Query makes API calls easier** and **updates UI automatically**.
* **No need to manually refresh pages after a mutation**.
* **Best for interactive apps** like dashboards and user management.
