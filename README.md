# Table of Content

1. **Fundamentals**
   * What is Next.js and why use it?
   * Next.js vs React&#x20;
   * File-based routing (`pages/` vs `app/`)
2. **Routing**
   * Static and Dynamic Routes&#x20;
   * Nested Routes
   * API Routes&#x20;
   * Middleware
   * Parallel & Intercepting Routes
3.  **Rendering & Data Fetching**

    * Server-Side Rendering (SSR)&#x20;

    &#x20;         1\.  SSR with authentication example

    * Static Site Generation (SSG)&#x20;
    * Incremental Static Regeneration (ISR)&#x20;
    * Client-Side Fetching
    * &#x20;React Query in Next JS\
      1\.  Mutations
    * How layouts work&#x20;
4. **State Management**
   * Context API in Next.js
   * Redux Toolkit in Next.js
   * Zustand/Jotai/Recoil
5. **Performance Optimization**
   * Image Optimization
   * Code Splitting&#x20;
   * Font Optimization
   * Caching & Revalidation&#x20;
   * Performance comparison: Server vs. Client Components
6. **Styling in Next.js**
   * Global CSS, CSS Modules, Tailwind CSS, Styled Components
   * Server-side styled-components setup
7. **Authentication & Authorization**
   * NextAuth.js integration
   * JWT authentication & custom authentication
   * Middleware-based authentication
   * Role-based access control (RBAC)
8. **Deployment & Hosting**
   * Vercel deployment & Edge Functions
   * Dockerizing Next.js Apps
   * Environment variables handling (`.env.local`)
9. **SEO & Accessibility**
   * Metadata handling
   * Structured data (`JSON-LD`, `next-seo`) and Optimizing Lighthouse scores

***

### Advance Concepts

#### **3. Server Actions & Form Handling**

* React Server Actions (New in Next.js 14)
* Handling form actions without API routes `useFormState` for form submissions
* Mutations in Server Actions

#### **4. Performance & Build Enhancements**

* Turbopack (New in Next.js 14) vs. Webpack/Vite and How Turbopack improves build performance
* Partial Prerendering (Hybrid static + dynamic rendering)

#### **5. Middleware & Streaming**

* Streaming with Middleware and Edge Middleware improvements
* Streaming SSR with Suspense
* Progressive rendering with Suspense

#### **6. Next.js & AI Integration**

* Using Vercel AI SDK with Next.js
* Building AI-powered applications

#### **9. Monorepo & Turborepo Integration**

* Setting up Next.js in a monorepo
* Benefits of Turborepo for large-scale apps
