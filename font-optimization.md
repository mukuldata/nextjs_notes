# Font Optimization

### **Table of Contents**

1. Introduction
2. Why Font Optimization is Important
3. Next.js Built-in Font Optimization
4. Using Google Fonts in Next.js
5. Self-Hosting Fonts in Next.js
6. Example: Optimizing Google Fonts
7. Example: Self-Hosting Custom Fonts
8. Conclusion

***

### **1. Introduction**

Font optimization in Next.js helps **improve performance and user experience** by reducing font loading times.\
✅ **Faster Page Load** – Fonts load efficiently, reducing render delays.\
✅ **Better User Experience** – Text appears quickly without layout shifts.\
✅ **Improved SEO & Core Web Vitals** – Optimized fonts improve page speed scores.

***

### **2. Why Font Optimization is Important?**

When using external fonts (e.g., Google Fonts), browsers:\
🔹 First load the page **without** the correct font (Fallback font)\
🔹 Then **download** the external font\
🔹 Finally, apply the correct font, causing **layout shifts (FOUT/FOIT)**

🛑 **Problems without optimization:**

* Fonts take **longer** to load.
* Page content appears **unstyled** until fonts load.
* It affects **SEO and performance** (Largest Contentful Paint - LCP).

✅ **Next.js solves this by:**

* Preloading fonts to **avoid layout shifts**.
* Automatically **optimizing** Google Fonts.
* Supporting **self-hosted fonts** for better control.

***

### **3. Next.js Built-in Font Optimization**

Next.js 13+ provides **@next/font** (now renamed to `next/font`) for **automatic font optimization**.

🔹 **Features of next/font:**\
✅ Loads fonts **directly from Google** (no extra HTTP requests).\
✅ Supports **self-hosting** for performance.\
✅ Eliminates **Flash of Unstyled Text (FOUT)**.\
✅ Reduces **unused font variants**.

***

### **4. Using Google Fonts in Next.js**

Next.js makes it easy to use **Google Fonts** with built-in optimization.

📌 **Example: Using Inter Font (Google Font)**

```javascript
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });

export default function Home() {
  return (
    <div className={inter.className}>
      <h1>Optimized Google Font in Next.js</h1>
    </div>
  );
}
```

✅ **What happens here?**

* **Next.js automatically optimizes the font**.
* The font loads **without render delay**.
* No need to include a `<link>` tag manually.

🛑 **Without optimization (traditional way):**

```html
<link href="https://fonts.googleapis.com/css2?family=Inter&display=swap" rel="stylesheet">
```

🔴 **Problem**: Adds an extra network request, slowing down the page.

✅ **Next.js Solution:** `next/font/google` **downloads and optimizes** the font **at build time**.

***

### **5. Self-Hosting Fonts in Next.js**

Instead of using Google Fonts, you can **self-host** fonts to improve performance.

📌 **Steps to Self-Host a Font:**

1. **Download** the font file (`.woff`, `.woff2`, etc.).
2. **Store** it inside the `public/fonts` folder.
3. **Use `@next/font/local`** to load the font.

📌 **Example: Self-Hosting a Font**\
🔹 **Place font file in:** `/public/fonts/CustomFont.woff2`

📌 **Import the font in Next.js**

```javascript
import localFont from 'next/font/local';

const myFont = localFont({
  src: '../public/fonts/CustomFont.woff2',
  weight: '400',
  style: 'normal',
});

export default function Home() {
  return (
    <div className={myFont.className}>
      <h1>Self-Hosted Font Example</h1>
    </div>
  );
}
```

✅ **Benefits of Self-Hosting:**

* No **external requests** (faster loading).
* Works **offline** (better reliability).
* More **control** over font behavior.

***

### **6. Example: Optimizing Google Fonts**

If using **multiple Google Font styles**, you can optimize by selecting only the required **weights and subsets**.

📌 **Better Approach:**

```javascript
import { Roboto } from 'next/font/google';

const roboto = Roboto({ weight: '400', subsets: ['latin'] });

export default function Home() {
  return (
    <div className={roboto.className}>
      <h1>Optimized Google Font in Next.js</h1>
    </div>
  );
}
```

✅ **Optimized Loading:**

* Loads only **one weight (400)** instead of all.
* Reduces **unused font variations**.
* Improves **performance and page speed**.

***

### **7. Example: Self-Hosting Custom Fonts**

📌 **Steps to use self-hosted fonts:**\
1️⃣ **Download the font** (e.g., Open Sans `.woff2` file).\
2️⃣ **Store it in** `public/fonts/`.\
3️⃣ **Use `@next/font/local`** to import it.

📌 **Code Example**

```javascript
import localFont from 'next/font/local';

const openSans = localFont({
  src: '../public/fonts/OpenSans-Regular.woff2',
  weight: '400',
  style: 'normal',
});

export default function Home() {
  return (
    <div className={openSans.className}>
      <h1>Self-Hosted Font Example</h1>
    </div>
  );
}
```

✅ **No external requests → Faster page load!**

***

### **8. Conclusion**

✅ Next.js **automatically optimizes fonts** for better performance.\
✅ Using `next/font/google` helps in **loading Google Fonts efficiently**.\
✅ **Self-hosting fonts** is the best option for **full control and performance**.\
✅ **Select only required weights and subsets** to reduce unused fonts.
