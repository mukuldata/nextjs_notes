# Server-side styled-components

### **Table of Contents**

1. Introduction to Styled Components
2. Why Use Server-Side Rendering (SSR) for Styled Components?
3. Setting Up Styled Components in Next.js
4. Implementing Styled Components
5. Enabling SSR for Styled Components
6. Example: Styled Button Component
7. Conclusion

***

### **1. Introduction to Styled Components**

**Styled Components** is a popular **CSS-in-JS** library that allows you to style components in JavaScript.

✅ **Key Features:**

* Write CSS directly inside JavaScript.
* Supports **dynamic styling** using props.
* **Scoped styles**, avoiding conflicts.
* Supports **server-side rendering (SSR)** in Next.js.

***

### **2. Why Use Server-Side Rendering (SSR) for Styled Components?**

By default, styled-components render styles **on the client side**, which may cause **flickering (FOUC - Flash of Unstyled Content)** when the page loads.

✅ **Benefits of SSR with Styled Components in Next.js:**

* **Improves performance** by preloading styles before rendering.
* **Fixes flickering issue** (FOUC) in Next.js applications.
* **Better SEO** as styles are already applied when the page loads.

***

### **3. Setting Up Styled Components in Next.js 13+**

#### **Step 1: Install Styled Components**

Run the following command in your Next.js project:

```bash
npm install styled-components
```

For **TypeScript users**, install the types as well:

```bash
npm install --save-dev @types/styled-components
```

***

### **4. Implementing Styled Components**

#### **📌 Example: Create a Styled Button Component**

Create a file `components/Button.js`:

```javascript
import styled from 'styled-components';

const Button = styled.button`
  background-color: ${(props) => (props.primary ? 'blue' : 'gray')};
  color: white;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
  cursor: pointer;
  font-size: 16px;

  &:hover {
    background-color: ${(props) => (props.primary ? 'darkblue' : 'darkgray')};
  }
`;

export default function StyledButton() {
  return <Button primary>Click Me</Button>;
}
```

✅ **This works, but without SSR, it may cause flickering**.

***

### **5. Enabling SSR for Styled Components in Next.js**

To fix **flickering (FOUC)**, we need to configure **server-side rendering (SSR) for styled-components**.

#### **📌 Step 1: Create `_document.js` in `pages/` (for Pages Router)**

In Next.js 13+, if you're using the **Pages Router**, add `_document.js`:

📂 `pages/_document.js`

```javascript
import Document from 'next/document';
import { ServerStyleSheet } from 'styled-components';

export default class MyDocument extends Document {
  static async getInitialProps(ctx) {
    const sheet = new ServerStyleSheet();
    const originalRenderPage = ctx.renderPage;

    try {
      ctx.renderPage = () =>
        originalRenderPage({
          enhanceApp: (App) => (props) => sheet.collectStyles(<App {...props} />),
        });

      const initialProps = await Document.getInitialProps(ctx);
      return {
        ...initialProps,
        styles: [...initialProps.styles, sheet.getStyleElement()],
      };
    } finally {
      sheet.seal();
    }
  }
}
```

✅ **What does this do?**

* Uses **ServerStyleSheet** from `styled-components` to **collect styles on the server**.
* Ensures styles are loaded before rendering the page.
* Fixes the **flickering issue (FOUC)** when using styled-components in Next.js.

***

#### **📌 Step 2: Create `layout.js` in `app/` (for App Router)**

If you're using the **App Router (app directory)** in Next.js 13+, add the following in `app/layout.js`:

📂 `app/layout.js`

```javascript
'use client';
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
  }
`;

export default function RootLayout({ children }) {
  return (
    <>
      <GlobalStyle />
      {children}
    </>
  );
}
```

✅ **What does this do?**

* Ensures **global styles** are loaded using `createGlobalStyle`.
* Works inside **App Router (`app/` directory)** without requiring `_document.js`.

***

### **6. Example: Styled Button Component with SSR**

Now, let’s use our **Styled Button** inside a Next.js page.

📂 `app/page.js`

```javascript
import StyledButton from '../components/Button';

export default function Home() {
  return (
    <div>
      <h1>Styled Components with SSR</h1>
      <StyledButton />
    </div>
  );
}
```

✅ **Now your Styled Components are fully optimized with SSR!**

***

### **7. Conclusion**

🎯 **Why Enable SSR for Styled Components in Next.js?**

* Prevents **Flickering (FOUC)** issues.
* Improves **Performance** & **SEO**.
* Ensures styles are **preloaded before rendering**.

📌 **Which setup should you use?**

| Router Type                 | File to Use                          |
| --------------------------- | ------------------------------------ |
| **Pages Router (`pages/`)** | `_document.js`                       |
| **App Router (`app/`)**     | `layout.js` with `createGlobalStyle` |
