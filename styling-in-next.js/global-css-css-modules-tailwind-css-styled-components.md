# Global CSS, CSS Modules, Tailwind CSS, Styled Components

### **Table of Contents**

1. Introduction
2. Global CSS
   * Explanation
   * Example
3. CSS Modules
   * Explanation
   * Example
4. Tailwind CSS
   * Explanation
   * Example
5. Styled Components
   * Explanation
   * Example
6. Comparison of Different CSS Methods
7. Conclusion

***

### **1. Introduction**

Next.js supports multiple ways to style applications. The four most commonly used methods are:

✅ **Global CSS** → Styles are applied across the entire app.\
✅ **CSS Modules** → Styles are scoped to a specific component.\
✅ **Tailwind CSS** → A utility-first CSS framework for fast styling.\
✅ **Styled Components** → A CSS-in-JS library for dynamic styling.

Let's explore each method with examples.

***

### **2. Global CSS**

#### **📌 What is Global CSS?**

* CSS that is applied **to the entire application**.
* Defined in a single `.css` file and imported into `_app.js` or `layout.js`.
* **Use case**: General styles like fonts, background colors, and themes.

#### **📌 Example of Global CSS in Next.js**

**1️⃣ Create a global CSS file (`app/globals.css`)**

```css
body {
  background-color: #f4f4f4;
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}
```

**2️⃣ Import it in `layout.js` (or `app/layout.js` for App Router)**

```javascript
import './globals.css';

export default function Layout({ children }) {
  return <div>{children}</div>;
}
```

✅ **Advantage**: Easy to set up, applies styles globally.\
🛑 **Disadvantage**: Can cause conflicts if styles overwrite each other.

***

### **3. CSS Modules**

#### **📌 What is CSS Modules?**

* CSS files that **scope styles to a specific component**.
* Prevents **class name conflicts** by generating unique names.
* **Use case**: Component-specific styles.

#### **📌 Example of CSS Modules in Next.js**

**1️⃣ Create a CSS module file (`app/Button.module.css`)**

```css
.button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}
```

**2️⃣ Use it in a component (`app/Button.js`)**

```javascript
import styles from './Button.module.css';

export default function Button() {
  return <button className={styles.button}>Click Me</button>;
}
```

✅ **Advantage**: Avoids global style conflicts.\
🛑 **Disadvantage**: More effort required for every component.

***

### **4. Tailwind CSS**

#### **📌 What is Tailwind CSS?**

* A **utility-first CSS framework** for building custom designs quickly.
* Uses **class-based styling** (no need for separate CSS files).
* **Use case**: Rapid development with pre-designed utility classes.

#### **📌 Steps to Install Tailwind CSS in Next.js**

1️⃣ Install Tailwind CSS

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

2️⃣ Configure `tailwind.config.js`

```javascript
module.exports = {
  content: ['./app/**/*.{js,ts,jsx,tsx}'], // Scan these files for Tailwind classes
  theme: {
    extend: {},
  },
  plugins: [],
};
```

3️⃣ Import Tailwind in `globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

#### **📌 Example of Tailwind CSS in a Component**

```javascript
export default function Card() {
  return (
    <div className="bg-white shadow-md p-4 rounded-lg">
      <h2 className="text-xl font-bold">Tailwind Card</h2>
      <p className="text-gray-600">This is a styled component using Tailwind CSS.</p>
    </div>
  );
}
```

✅ **Advantage**: Fast development with minimal CSS files.\
🛑 **Disadvantage**: Requires learning Tailwind class names.

***

### **5. Styled Components**

#### **📌 What is Styled Components?**

* A **CSS-in-JS library** that allows writing CSS directly inside JavaScript.
* Styles are applied dynamically to components.
* **Use case**: When you need dynamic styles based on component props.

#### **📌 Steps to Install Styled Components in Next.js**

1️⃣ Install Styled Components

```bash
npm install styled-components
```

2️⃣ **Enable Server-Side Rendering (Optional)**

* Create a `pages/_document.js` file for styled-components support.

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

#### **📌 Example of Styled Components in Next.js**

```javascript
import styled from 'styled-components';

const Button = styled.button`
  background-color: ${(props) => (props.primary ? 'blue' : 'gray')};
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
`;

export default function StyledButton() {
  return <Button primary>Styled Button</Button>;
}
```

✅ **Advantage**: Dynamic styles without needing external CSS.\
🛑 **Disadvantage**: Slightly increases bundle size.

***

### **6. Comparison of Different CSS Methods**

| Method                | Scoped? | Performance         | Ease of Use | Best For                  |
| --------------------- | ------- | ------------------- | ----------- | ------------------------- |
| **Global CSS**        | ❌ No    | 🚀 Fast             | 😊 Easy     | Global styles, themes     |
| **CSS Modules**       | ✅ Yes   | 🚀 Fast             | 😐 Medium   | Component-specific styles |
| **Tailwind CSS**      | ✅ Yes   | 🚀 Fast             | 🤔 Medium   | Utility-based styling     |
| **Styled Components** | ✅ Yes   | 🐢 Slower (Runtime) | 😐 Medium   | Dynamic styling in JS     |

***

### **7. Conclusion**

✅ **Global CSS** → Best for general styles (but can cause conflicts).\
✅ **CSS Modules** → Best for scoped, reusable component styles.\
✅ **Tailwind CSS** → Best for rapid UI development.\
✅ **Styled Components** → Best for dynamic styles and component-based styling.
