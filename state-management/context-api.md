# Context API

### **Table of Contents**

1. Introduction
2. What is Context API?
3. Why Use Context API in Next.js?
4. Setting Up Context API in Next.js 13+
5. Example: Creating a Theme Context
6. Example: Using Context in a Component
7. Using Context in the Server & Client Components
8. Summary

***

### **1. Introduction**

In Next.js, managing state across multiple components can be challenging. The **Context API** provides a way to share data between components **without** passing props manually at every level.

***

### **2. What is Context API?**

Context API is a built-in **state management** solution in React that allows you to **share data** (like themes, authentication, or global settings) across components **without prop drilling**.

📌 **Key Features:**\
✔ Centralized **state management**\
✔ Avoids **prop drilling**\
✔ Works with both **Client** & **Server components** in Next.js 13+

***

### **3. Why Use Context API in Next.js?**

| Feature                                 | Context API              | Prop Drilling |
| --------------------------------------- | ------------------------ | ------------- |
| Global state access                     | ✅ Yes                    | ❌ No          |
| Avoids passing props at every level     | ✅ Yes                    | ❌ No          |
| Works in Next.js Server Components      | ✅ Yes (with limitations) | ❌ No          |
| Good for Auth, Theme, Language settings | ✅ Yes                    | ❌ No          |

Context API is best used for **themes, authentication, user preferences, and language settings**.

***

### **4. Setting Up Context API in Next.js 13+**

To use the Context API, we follow three steps:\
✔ **Create Context** → Define the data to be shared\
✔ **Provide Context** → Wrap the app with the provider\
✔ **Consume Context** → Use the context inside components

***

### **5. Example: Creating a Theme Context**

📁 **app/context/ThemeContext.js**

```javascript
"use client"; // Context must run in the client

import { createContext, useContext, useState } from "react";

// Create a Context
const ThemeContext = createContext();

// Create a Provider Component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  // Toggle function
  const toggleTheme = () => {
    setTheme(theme === "light" ? "dark" : "light");
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook for consuming context
export function useTheme() {
  return useContext(ThemeContext);
}
```

📌 **What Happens Here?**\
✔ **Creates a `ThemeContext`** to store theme data\
✔ **Defines `ThemeProvider`** to wrap around the app\
✔ **Provides `toggleTheme` function** to switch themes\
✔ **Exports `useTheme` hook** to access theme data

***

### **6. Example: Using Context in a Component**

Now, let's use the **ThemeContext** in a component.

📁 **app/components/ThemeToggle.js**

```javascript
"use client";

import { useTheme } from "@/context/ThemeContext";

export default function ThemeToggle() {
  const { theme, toggleTheme } = useTheme();

  return (
    <div>
      <p>Current Theme: {theme}</p>
      <button onClick={toggleTheme}>
        Switch to {theme === "light" ? "Dark" : "Light"} Mode
      </button>
    </div>
  );
}
```

📌 **What Happens Here?**\
✔ **Uses `useTheme()` to access context**\
✔ **Displays the current theme**\
✔ **Toggles between Light/Dark mode on button click**

***

### **7. Using Context in the Server & Client Components**

📌 **In Next.js 13+, Context API works only in Client Components**.\
If you need **global state** in Server Components, you must **use cookies, sessions, or databases instead**.

To wrap the entire app with our **ThemeProvider**, modify the **layout.js** file.

📁 **app/layout.js**

```javascript
import { ThemeProvider } from "@/context/ThemeContext";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  );
}
```

Now, any component inside **app/** can access the theme context.

***

### **8. Summary**

* **Context API** helps in **global state management**.
* **Best for theme, authentication, and user settings**.
* **Works in Client Components only** in Next.js 13+.
* **Simple to use compared to external state management libraries like Redux**.
