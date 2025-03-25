# Optimizing Lighthouse Scores

### **Table of Contents**

1. **Structured Data and Its Importance**
2. **Using JSON-LD in Next.js**
   * **Manually Adding JSON-LD**
   * **Using `next-seo` for JSON-LD**
3. **Optimizing Lighthouse Scores**
   * **Core Lighthouse Metrics**
   * **Techniques to Improve Lighthouse Scores**
4. **Best Practices for Structured Data and Optimization**
5. **Conclusion**

***

## **1. Structured Data and Its Importance**

Structured data is a standardized format that helps **search engines** understand the content of a webpage. It improves **SEO** and enables features like:\
✅ Rich snippets (e.g., star ratings, FAQs, product prices)\
✅ Better indexing on Google\
✅ Improved social media previews

Example of structured data for a blog post:

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Understanding Structured Data",
  "author": {
    "@type": "Person",
    "name": "John Doe"
  },
  "datePublished": "2024-03-25",
  "description": "Learn about structured data and its importance for SEO.",
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://example.com/blog/structured-data"
  }
}
```

***

## **2. Using JSON-LD in Next.js**

### **2.1 Manually Adding JSON-LD**

In Next.js, structured data can be added manually using `<script>` inside the `Head` component.

#### **Example: Adding JSON-LD for a Blog Post**

```tsx
import Head from "next/head";

export default function BlogPost() {
  const jsonLdData = {
    "@context": "https://schema.org",
    "@type": "BlogPosting",
    "headline": "Next.js and Structured Data",
    "author": { "@type": "Person", "name": "John Doe" },
    "datePublished": "2024-03-25",
    "description": "Learn about Next.js structured data integration.",
    "mainEntityOfPage": {
      "@type": "WebPage",
      "@id": "https://example.com/blog/nextjs-structured-data"
    }
  };

  return (
    <>
      <Head>
        <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(jsonLdData) }} />
      </Head>
      <h1>Understanding Structured Data in Next.js</h1>
    </>
  );
}
```

📌 **Use `dangerouslySetInnerHTML` to inject JSON-LD properly.**

***

### **2.2 Using `next-seo` for JSON-LD**

A better way to manage structured data in Next.js is using the `next-seo` package.

#### **Install `next-seo`**

```bash
npm install next-seo
```

#### **Example: Adding JSON-LD with `next-seo`**

```tsx
import { NextSeo, BlogJsonLd } from "next-seo";

export default function BlogPost() {
  return (
    <>
      <NextSeo title="Next.js SEO Guide" description="Learn about structured data in Next.js." />
      <BlogJsonLd
        url="https://example.com/blog/nextjs-seo"
        title="Next.js SEO Guide"
        images={["https://example.com/images/blog-cover.jpg"]}
        datePublished="2024-03-25"
        authorName="John Doe"
        description="A guide on using structured data in Next.js."
      />
      <h1>Next.js SEO Guide</h1>
    </>
  );
}
```

📌 **`next-seo` makes structured data easier to implement and maintain.**

***

## **3. Optimizing Lighthouse Scores**

Lighthouse is a tool that measures a web page’s performance, accessibility, SEO, and best practices. The key metrics are:\
✅ **Performance** – Measures speed and efficiency.\
✅ **SEO** – Ensures best practices for search engines.\
✅ **Accessibility** – Checks for usability improvements.\
✅ **Best Practices** – Analyzes security and coding standards.

***

### **3.1 Core Lighthouse Metrics**

* **Largest Contentful Paint (LCP)** – Measures how fast the largest element loads.
* **First Input Delay (FID)** – Measures interactivity speed.
* **Cumulative Layout Shift (CLS)** – Checks layout stability.

***

### **3.2 Techniques to Improve Lighthouse Scores**

#### **1. Optimize Images**

✅ Use Next.js `<Image>` component:

```tsx
import Image from "next/image";

export default function Home() {
  return <Image src="/image.jpg" width={600} height={400} alt="Optimized Image" />;
}
```

✅ Enable automatic **lazy loading**.\
✅ Use **WebP format** for smaller file sizes.

***

#### **2. Enable Automatic Code Splitting**

Next.js **automatically** splits code, but make sure to:\
✅ Use **dynamic imports** for components:

```tsx
import dynamic from "next/dynamic";
const HeavyComponent = dynamic(() => import("../components/HeavyComponent"), { ssr: false });
```

✅ **Remove unused CSS & JavaScript** with `next/script`:

```tsx
import Script from "next/script";

export default function Home() {
  return (
    <>
      <Script src="https://example.com/heavy-script.js" strategy="lazyOnload" />
      <h1>Optimized Page</h1>
    </>
  );
}
```

***

#### **3. Optimize Fonts**

✅ Use the **Next.js Font Optimization** feature:

```tsx
import { Inter } from "next/font/google";
const inter = Inter({ subsets: ["latin"] });

export default function Home() {
  return <h1 className={inter.className}>Optimized Fonts</h1>;
}
```

📌 **Avoid blocking render with font files.**

***

#### **4. Reduce CLS (Cumulative Layout Shift)**

✅ Set explicit width and height for images.\
✅ Reserve space for dynamic content (e.g., ads, embeds).

***

#### **5. Enable Server-side Rendering (SSR) & Static Generation (SSG)**

✅ Use **getServerSideProps** for dynamic data:

```tsx
export async function getServerSideProps() {
  const data = await fetch("https://api.example.com").then((res) => res.json());
  return { props: { data } };
}
```

✅ Use **getStaticProps** for static content:

```tsx
export async function getStaticProps() {
  const data = await fetch("https://api.example.com").then((res) => res.json());
  return { props: { data } };
}
```

***

## **4. Best Practices for Structured Data & Optimization**

✅ **Use JSON-LD for SEO** (manual or `next-seo`).\
✅ **Optimize images with Next.js `<Image>` component**.\
✅ **Use dynamic imports for large components**.\
✅ **Enable font optimization to reduce render-blocking**.\
✅ **Use SSR or SSG for faster page loads**.

***

## **5. Conclusion**

* **Structured data (JSON-LD)** improves SEO and helps search engines understand your content.
* Use **Next.js `<Image>` and font optimization** to boost Lighthouse scores.
* Use **SSR, SSG, and dynamic imports** to optimize performance.
