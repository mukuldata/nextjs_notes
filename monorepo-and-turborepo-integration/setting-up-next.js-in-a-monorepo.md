# Setting up Next.js in a monorepo

#### **Table of Contents**

1. **Introduction to Monorepos**
   * What is a monorepo?
   * Why use a monorepo?
2. **Tools for Managing a Monorepo**
   * Turborepo
   * Nx
   * Yarn Workspaces
3. **Setting Up Next.js in a Monorepo**
   * Folder structure
   * Installing dependencies
   * Configuring `package.json`
4. **Sharing Code Between Apps**
   * Creating a shared package
   * Importing shared code
5. **Running & Building the Monorepo**
   * Running individual apps
   * Optimizing builds
6. **Advantages & Use Cases**

***

#### **1. Introduction to Monorepos**

A **monorepo** allows multiple projects to exist within a single repository. Instead of maintaining separate repos for different apps, you can keep all related projects in **one place**.

**Why Use a Monorepo?**

✅ **Code Reusability** – Share components, utilities, and libraries between multiple apps.\
✅ **Easier Dependency Management** – Install dependencies centrally.\
✅ **Improved Collaboration** – Work on multiple projects without switching repos.\
✅ **Better Build Performance** – Optimize builds using caching and parallel execution.

***

#### **2. Tools for Managing a Monorepo**

Monorepos require **workspace management tools** to handle dependencies and tasks efficiently. Some popular tools include:

* **Turborepo** – Optimized for JavaScript/TypeScript projects, caches builds.
* **Nx** – Provides plugins and optimizations for various frameworks.
* **Yarn Workspaces** – Manages dependencies efficiently across projects.

***

#### **3. Setting Up Next.js in a Monorepo**

**Folder Structure Example**

```
my-monorepo/
│── apps/
│   ├── next-app-1/
│   ├── next-app-2/
│── packages/
│   ├── ui-components/
│   ├── utils/
│── package.json
│── turbo.json (if using Turborepo)
│── tsconfig.json (for TypeScript projects)
```

* `apps/` → Contains multiple **Next.js applications**.
* `packages/` → Shared components, utilities, or hooks.
* `package.json` → Manages dependencies for the entire monorepo.

**Configuring `package.json` (Root)**

```json
{
  "private": true,
  "workspaces": ["apps/*", "packages/*"],
  "devDependencies": {
    "turbo": "latest"
  }
}
```

* **Workspaces** allow managing dependencies across apps and packages.

**Installing Dependencies**

```sh
npm install turbo -D
```

OR (if using Yarn)

```sh
yarn add turbo -D
```

***

#### **4. Sharing Code Between Apps**

To create **shared utilities or UI components**, place them in the `packages/` folder.

**Example: Creating a Shared Component**

* Inside `packages/ui-components/`, create a `Button.tsx` component:

```tsx
export const Button = () => {
  return <button>Click Me</button>;
};
```

* Add a `package.json` inside `ui-components/`:

```json
{
  "name": "@my-monorepo/ui-components",
  "version": "1.0.0",
  "main": "index.ts"
}
```

* In `next-app-1`, install and use it:

```tsx
import { Button } from "@my-monorepo/ui-components";

export default function Home() {
  return <Button />;
}
```

This **avoids code duplication** and improves **maintainability**.

***

#### **5. Running & Building the Monorepo**

**Running Individual Apps**

Use Turborepo or npm scripts:

```sh
turbo run dev --filter=next-app-1
```

OR run inside the specific app:

```sh
cd apps/next-app-1
npm run dev
```

**Optimizing Builds with Turborepo**

Add a `turbo.json` in the root folder:

```json
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**"]
    }
  }
}
```

This **caches builds**, reducing compilation time.

***

#### **6. Advantages & Use Cases**

✅ **Great for Large-Scale Projects** – Manage multiple Next.js apps efficiently.\
✅ **Faster CI/CD Pipelines** – Cached builds reduce build time.\
✅ **Centralized Dependency Management** – Prevents version conflicts.\
✅ **Scalability** – Easily add more apps or packages.

***

#### **Conclusion**

Setting up **Next.js in a monorepo** helps scale projects efficiently, share code, and speed up development. Tools like **Turborepo** make builds faster, while **workspaces** help manage dependencies.
