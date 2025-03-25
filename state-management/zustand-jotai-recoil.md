# Zustand/Jotai/Recoil

### **Table of Contents**

1. Introduction
2. Why Use Zustand, Jotai, or Recoil Instead of Redux?
3. **Zustand**
   * What is Zustand?
   * How to Use Zustand (Example)
4. **Jotai**
   * What is Jotai?
   * How to Use Jotai (Example)
5. **Recoil**
   * What is Recoil?
   * How to Use Recoil (Example)
6. Comparison Table
7. Conclusion

***

### **1. Introduction**

State management is important in **Next.js 13+**, especially when dealing with **client-side state** (e.g., theme selection, authentication, or shopping cart).\
👉 While **Redux** is commonly used, alternatives like **Zustand, Jotai, and Recoil** offer simpler solutions for managing state.

***

### **2. Why Use Zustand, Jotai, or Recoil Instead of Redux?**

Redux can be **complex** with **boilerplate code**. Zustand, Jotai, and Recoil provide **lightweight** and **simpler** ways to manage global state without the need for **reducers or actions**.

| Feature                      | Zustand    | Jotai      | Recoil | Redux    |
| ---------------------------- | ---------- | ---------- | ------ | -------- |
| **Boilerplate Code**         | ❌ Very Low | ❌ Very Low | ❌ Low  | ✅ High   |
| **Performance**              | ✅ Fast     | ✅ Fast     | ✅ Fast | ⚠ Slower |
| **Global State**             | ✅ Yes      | ✅ Yes      | ✅ Yes  | ✅ Yes    |
| **Complex App Support**      | ⚠ Moderate | ⚠ Moderate | ✅ Good | ✅ Best   |
| **Server Component Support** | ❌ No       | ✅ Yes      | ✅ Yes  | ❌ No     |

***

## **3. Zustand**

#### **What is Zustand?**

Zustand is a **fast** and **simple** state management library for React and Next.js.\
📌 **Features:**\
✔ **No boilerplate** (no reducers, no actions)\
✔ **Easy to use with minimal setup**\
✔ **Supports middlewares like persistence and devtools**

#### **How to Use Zustand in Next.js 13+**

**Step 1: Install Zustand**

```bash
npm install zustand
```

**Step 2: Create a Store**

📁 **app/store/useCounterStore.js**

```javascript
import { create } from "zustand";

export const useCounterStore = create((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
  decrease: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}));
```

**Step 3: Use Zustand in a Component**

📁 **app/components/Counter.js**

```javascript
"use client";

import { useCounterStore } from "../store/useCounterStore";

export default function Counter() {
  const { count, increase, decrease, reset } = useCounterStore();

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={increase}>+</button>
      <button onClick={decrease}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

📌 **Why Zustand?**\
✔ No need for `Provider`\
✔ Simpler than Redux\
✔ Good for small to medium apps

***

## **4. Jotai**

#### **What is Jotai?**

Jotai is a **minimal and scalable** state management library based on **atoms** (small independent state pieces).\
📌 **Features:**\
✔ Uses **atoms** (smallest pieces of state)\
✔ **Works with Server Components** in Next.js 13+\
✔ **Very simple API**

#### **How to Use Jotai in Next.js 13+**

**Step 1: Install Jotai**

```bash
npm install jotai
```

**Step 2: Create an Atom (State)**

📁 **app/store/counterAtom.js**

```javascript
import { atom } from "jotai";

export const counterAtom = atom(0);
```

**Step 3: Use Jotai in a Component**

📁 **app/components/Counter.js**

```javascript
"use client";

import { useAtom } from "jotai";
import { counterAtom } from "../store/counterAtom";

export default function Counter() {
  const [count, setCount] = useAtom(counterAtom);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

📌 **Why Jotai?**\
✔ **Better than Zustand for large apps**\
✔ **Works well in Server Components**\
✔ **No need for reducers or actions**

***

## **5. Recoil**

#### **What is Recoil?**

Recoil is a **powerful state management library from Facebook** that is useful for complex applications.\
📌 **Features:**\
✔ Uses **atoms** (small state units) like Jotai\
✔ Supports **async state**\
✔ Better than Zustand for **large-scale** apps

#### **How to Use Recoil in Next.js 13+**

**Step 1: Install Recoil**

```bash
npm install recoil
```

**Step 2: Create a Recoil Atom**

📁 **app/store/counterAtom.js**

```javascript
import { atom } from "recoil";

export const counterState = atom({
  key: "counterState",
  default: 0,
});
```

**Step 3: Wrap Next.js App with RecoilRoot**

📁 **app/providers.js**

```javascript
"use client";

import { RecoilRoot } from "recoil";

export function RecoilProvider({ children }) {
  return <RecoilRoot>{children}</RecoilRoot>;
}
```

Modify `layout.js` to include Recoil:\
📁 **app/layout.js**

```javascript
import { RecoilProvider } from "./providers";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <RecoilProvider>{children}</RecoilProvider>
      </body>
    </html>
  );
}
```

**Step 4: Use Recoil in a Component**

📁 **app/components/Counter.js**

```javascript
"use client";

import { useRecoilState } from "recoil";
import { counterState } from "../store/counterAtom";

export default function Counter() {
  const [count, setCount] = useRecoilState(counterState);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

📌 **Why Recoil?**\
✔ **Best for large apps**\
✔ **Supports async state (e.g., API calls)**\
✔ **More powerful than Zustand and Jotai**

***

## **6. Comparison Table**

| Feature                      | Zustand    | Jotai       | Recoil     |
| ---------------------------- | ---------- | ----------- | ---------- |
| **Boilerplate Code**         | ✅ Low      | ✅ Very Low  | ⚠ Medium   |
| **Server Component Support** | ❌ No       | ✅ Yes       | ✅ Yes      |
| **Best For**                 | Small Apps | Medium Apps | Large Apps |
| **Supports Async State**     | ⚠ Limited  | ✅ Yes       | ✅ Yes      |

***

## **7. Conclusion**

✔ **Use Zustand** for small projects (fast, simple)\
✔ **Use Jotai** for scalable apps (better than Zustand)\
✔ **Use Recoil** for large projects (Facebook-backed, powerful)
