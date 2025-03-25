# Metadata handling

### **Table of Contents**

1. What is Metadata?
2. Why is Metadata Important?
3. Setting Metadata in Next.js
   * **Using the `metadata` Object (Recommended)**
   * **Using `head` in Client Components**
   * **Using `next-seo` for Advanced SEO**
4. Dynamic Metadata
5. Best Practices for Metadata Handling
6. Conclusion

***

### **1. What is Metadata?**

Metadata is **information about a web page** that helps search engines and social media platforms understand its content. It includes:\
✅ **Title** (appears in the browser tab and search results)\
✅ **Description** (summarizes the page for SEO)\
✅ **Open Graph (OG) Tags** (used for sharing on social media)\
✅ **Twitter Meta Tags** (used for Twitter previews)\
✅ **Robots Directives** (tell search engines how to index a page)

Example:

```html
<title>My Page</title>
<meta name="description" content="This is a description of my page.">
<meta property="og:title" content="My Page">
<meta property="og:image" content="/image.png">
<meta name="twitter:card" content="summary_large_image">
```

***

### **2. Why is Metadata Important?**

✅ **Improves SEO** – Helps search engines rank pages better.\
✅ **Enhances Social Media Sharing** – Displays proper previews when shared.\
✅ **Boosts User Engagement** – Makes the page more attractive in search results.

***

### **3. Setting Metadata in Next.js**

#### **3.1 Using the `metadata` Object (Recommended in Next.js 13+)**

In Next.js 13+, metadata is handled in **server components** using the `metadata` object.

**Example: Adding Metadata in a Page (`app/page.tsx`)**

```tsx
export const metadata = {
  title: "Home Page",
  description: "This is the home page of my Next.js app.",
  openGraph: {
    title: "Home Page",
    description: "Check out our website!",
    url: "https://example.com",
    images: [
      {
        url: "/og-image.jpg",
        width: 800,
        height: 600,
      },
    ],
  },
};
export default function HomePage() {
  return <h1>Welcome to My Website</h1>;
}
```

📌 **This approach is recommended in Next.js 13+ because it's easy to use and automatically optimizes SEO.**

***

#### **3.2 Using `head` in Client Components**

For client components, you can use the `<Head>` component from `next/head`:

```tsx
import Head from "next/head";

export default function AboutPage() {
  return (
    <>
      <Head>
        <title>About Us</title>
        <meta name="description" content="Learn more about us." />
      </Head>
      <h1>About Us</h1>
    </>
  );
}
```

📌 **Use this only when working with Client Components.**

***

#### **3.3 Using `next-seo` for Advanced SEO**

For advanced metadata management, use `next-seo`:

```bash
npm install next-seo
```

Then, in your page:

```tsx
import { NextSeo } from "next-seo";

export default function BlogPost() {
  return (
    <>
      <NextSeo
        title="Blog Post"
        description="This is a blog post."
        openGraph={{
          title: "Blog Post",
          description: "Read our latest blog post!",
          images: [{ url: "/blog-image.jpg" }],
        }}
      />
      <h1>Blog Post</h1>
    </>
  );
}
```

📌 **`next-seo` makes it easy to set SEO metadata dynamically.**

***

### **4. Dynamic Metadata**

If metadata needs to be generated dynamically (e.g., fetching from an API), use `generateMetadata()`.

**Example: Fetching Metadata for a Blog Post**

```tsx
export async function generateMetadata({ params }) {
  const post = await fetch(`https://api.example.com/posts/${params.id}`).then((res) => res.json());

  return {
    title: post.title,
    description: post.description,
    openGraph: {
      title: post.title,
      description: post.description,
      url: `https://example.com/posts/${params.id}`,
      images: [{ url: post.image }],
    },
  };
}

export default function BlogPost({ params }) {
  return <h1>Blog Post {params.id}</h1>;
}
```

📌 **This method is great for blog posts, product pages, and dynamic content.**

***

### **5. Best Practices for Metadata Handling**

✅ **Use the `metadata` object** in Server Components (recommended in Next.js 13+).\
✅ **Set `generateMetadata()`** for dynamic content.\
✅ **Use Open Graph & Twitter tags** for social sharing.\
✅ **Keep titles and descriptions unique** for each page.\
✅ **Avoid duplicate metadata** across pages.

***

### **6. Conclusion**

Metadata helps search engines and social media platforms understand your page. In Next.js 13+, use:\
✅ **`metadata` object** for static metadata\
✅ **`generateMetadata()`** for dynamic metadata\
✅ **`next-seo` package** for advanced SEO
