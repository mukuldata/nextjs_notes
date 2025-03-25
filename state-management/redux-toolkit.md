# Redux Toolkit

### **Table of Contents**

1. Introduction
2. What is Redux Toolkit?
3. Why Use Redux Toolkit in Next.js?
4. Setting Up Redux Toolkit in Next.js 13+
5. Creating a Redux Store
6. Creating a Redux Slice
7. Using Redux in Components
8. Persisting State Across Pages
9. Summary

***

### **1. Introduction**

Redux Toolkit is a simplified way to use Redux for state management. In **Next.js 13+**, Redux Toolkit is useful for managing **global state** such as authentication, cart data, or theme settings.

***

### **2. What is Redux Toolkit?**

Redux Toolkit (RTK) is the official, recommended way to use Redux in modern React and Next.js applications.

📌 **Key Features:**\
✔ **Simplifies Redux setup**\
✔ **Built-in support for Immer (mutating state easily)**\
✔ **Includes `createSlice` for defining reducers**\
✔ **Works well with Next.js (Client Components only)**

***

### **3. Why Use Redux Toolkit in Next.js?**

| Feature                                 | Redux Toolkit | Context API |
| --------------------------------------- | ------------- | ----------- |
| Large-scale state management            | ✅ Yes         | ❌ No        |
| Best for Authentication, Cart, API data | ✅ Yes         | ⚠ Limited   |
| Boilerplate code                        | ✅ Minimal     | ✅ Minimal   |
| Server Component support                | ❌ No          | ⚠ Limited   |

Redux Toolkit is better for **large applications** where managing state with Context API becomes complex.

***

### **4. Setting Up Redux Toolkit in Next.js 13+**

First, install Redux Toolkit and React Redux:

```bash
npm install @reduxjs/toolkit react-redux
```

Then, create the **Redux store** and a **slice** to manage state.

***

### **5. Creating a Redux Store**

📁 **app/redux/store.js**

```javascript
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./slices/counterSlice";

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

📌 **What Happens Here?**\
✔ Uses `configureStore()` to create a Redux store\
✔ Registers a `counterReducer` (we will create it next)

***

### **6. Creating a Redux Slice**

📁 **app/redux/slices/counterSlice.js**

```javascript
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  value: 0,
};

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    reset: (state) => {
      state.value = 0;
    },
  },
});

export const { increment, decrement, reset } = counterSlice.actions;
export default counterSlice.reducer;
```

📌 **What Happens Here?**\
✔ Defines `counterSlice` with `increment`, `decrement`, and `reset` actions\
✔ Uses **Immer** (so we can modify state directly)

***

### **7. Using Redux in Components**

#### **Wrapping the App with Redux Provider**

To use Redux in Next.js, wrap your app inside a **Provider**.

📁 **app/providers.js**

```javascript
"use client"; // Must be a Client Component

import { Provider } from "react-redux";
import { store } from "./redux/store";

export function ReduxProvider({ children }) {
  return <Provider store={store}>{children}</Provider>;
}
```

Now, modify **layout.js** to include the ReduxProvider:

📁 **app/layout.js**

```javascript
import { ReduxProvider } from "./providers";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <ReduxProvider>{children}</ReduxProvider>
      </body>
    </html>
  );
}
```

#### **Using Redux in a Component**

📁 **app/components/Counter.js**

```javascript
"use client";

import { useSelector, useDispatch } from "react-redux";
import { increment, decrement, reset } from "../redux/slices/counterSlice";

export default function Counter() {
  const count = useSelector((state) => state.counter.value);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Counter: {count}</h2>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
    </div>
  );
}
```

📌 **What Happens Here?**\
✔ Uses `useSelector()` to get the counter value\
✔ Uses `useDispatch()` to update the counter state\
✔ Provides **+**, **-**, and **Reset** buttons

***

### **8. Persisting State Across Pages**

In Next.js, Redux state resets **on page reload** by default. To persist state:

#### **Option 1: Use Local Storage (Simple Method)**

Modify `store.js` to load state from local storage:

```javascript
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./slices/counterSlice";

const loadState = () => {
  try {
    const savedState = localStorage.getItem("reduxState");
    return savedState ? JSON.parse(savedState) : undefined;
  } catch {
    return undefined;
  }
};

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
  preloadedState: loadState(),
});

store.subscribe(() => {
  localStorage.setItem("reduxState", JSON.stringify(store.getState()));
});

export default store;
```

📌 **Now, state will persist even after page reload!**

***

### **9. Summary**

✔ **Redux Toolkit** makes Redux easier to use in Next.js\
✔ **Best for large-scale apps** (auth, cart, API data)\
✔ **Uses `createSlice()` for state management**\
✔ **Requires a Client Component (`use client`)**\
✔ **Can persist state using Local Storage**
