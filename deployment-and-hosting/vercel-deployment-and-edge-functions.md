# Vercel deployment & Edge Functions

## **Vercel Deployment & Edge Functions in Next.js 13+**

### **Table of Contents**

1. What is Vercel?
2. Why Deploy Next.js on Vercel?
3. How to Deploy a Next.js App on Vercel
4. What are Edge Functions?
5. Why Use Edge Functions?
6. Implementing Edge Functions in Next.js
7. Example: Creating an Edge Function in Next.js
8. Comparing Edge Functions vs. Serverless Functions
9. Conclusion

***

### **1. What is Vercel?**

Vercel is a cloud platform designed for **frontend applications**. It is the **best platform to deploy Next.js apps** because Next.js is built by Vercel.

🔹 **Features of Vercel:**\
✅ **Fast Deployments** – One-click deployment via GitHub, GitLab, or Bitbucket\
✅ **Serverless & Edge Functions** – Handles backend logic efficiently\
✅ **Automatic Scaling** – No need to manage servers\
✅ **Free Plan Available** – Ideal for small projects

***

### **2. Why Deploy Next.js on Vercel?**

🚀 **Next.js + Vercel** = Best combination for performance & scalability.

🔹 **Advantages:**\
✅ **Global CDN** – Ensures super-fast page loads\
✅ **Automatic Builds** – Deploy changes with every push\
✅ **Optimized Images & Fonts** – Uses built-in Next.js optimizations\
✅ **Edge Functions** – Run server logic close to users

***

### **3. How to Deploy a Next.js App on Vercel**

📌 **Step 1: Install Vercel CLI (Optional, for manual deployment)**

```bash
npm install -g vercel
```

📌 **Step 2: Push Your Code to GitHub**\
1️⃣ Create a repository on GitHub\
2️⃣ Commit and push your Next.js project

📌 **Step 3: Deploy on Vercel**\
1️⃣ Go to [https://vercel.com](https://vercel.com/)\
2️⃣ Click **"New Project"**\
3️⃣ Connect your GitHub repository\
4️⃣ Click **"Deploy"**

✅ **Vercel will automatically detect Next.js and configure the deployment.**\
✅ **After deployment, you'll get a live URL like `https://your-app.vercel.app`**

***

### **4. What are Edge Functions?**

**Edge Functions** are lightweight, server-side functions that run **closer to the user** at the network edge instead of a central server.

📌 **Why does this matter?**\
🚀 **Faster response times** (because requests don’t travel to a central server)\
🔒 **Better security** (because data processing happens closer to the user)\
💰 **Lower costs** (more efficient than traditional backend servers)

***

### **5. Why Use Edge Functions?**

✅ **Personalization** – Customize content based on user location\
✅ **Security** – Protect routes before the request reaches the main server\
✅ **Faster Data Processing** – Reduce latency for global users

**Example Use Cases:**\
📌 Geolocation-based content\
📌 Rate-limiting API requests\
📌 A/B testing\
📌 Authentication

***

### **6. Implementing Edge Functions in Next.js**

📌 **Step 1: Create an Edge Function API Route**

📂 `app/api/edge-example/route.js`

```javascript
export const config = {
  runtime: "edge", // Enables Edge Function
};

export default async function handler(req) {
  return new Response(
    JSON.stringify({ message: "Hello from Edge Function!" }),
    { status: 200, headers: { "Content-Type": "application/json" } }
  );
}
```

✅ **Runs on the edge network, reducing response time**\
✅ **Faster than traditional serverless functions**

📌 **Step 2: Deploy the Edge Function**\
Once your Next.js app is deployed on Vercel, your Edge Function is **automatically deployed** and accessible at:

```bash
https://your-app.vercel.app/api/edge-example
```

***

### **7. Example: Creating an Edge Function in Next.js**

📌 **Step 1: Create an API Route that Shows User’s Country**

📂 `app/api/geolocation/route.js`

```javascript
export const config = {
  runtime: "edge",
};

export default async function handler(req) {
  const ip = req.headers.get("x-forwarded-for") || "Unknown IP";
  return new Response(
    JSON.stringify({ message: `Your IP is ${ip}` }),
    { status: 200, headers: { "Content-Type": "application/json" } }
  );
}
```

📌 **Step 2: Test Your Edge Function**\
After deployment, visit:

```bash
https://your-app.vercel.app/api/geolocation
```

✅ **It will return your IP address!**

***

### **8. Comparing Edge Functions vs. Serverless Functions**

| Feature         | Edge Functions                       | Serverless Functions                      |
| --------------- | ------------------------------------ | ----------------------------------------- |
| **Location**    | Runs at the network edge             | Runs on a central server                  |
| **Speed**       | Very fast (low latency)              | Slower than Edge Functions                |
| **Use Cases**   | Authentication, geolocation, caching | Full backend processing, database queries |
| **Cold Starts** | No cold starts                       | May have cold starts                      |
| **Limitations** | Cannot use Node.js modules           | Supports full Node.js environment         |

📌 **When to Use Edge Functions?**\
✔️ If you need **low-latency** API responses\
✔️ If you want to **secure** routes before they reach the backend

📌 **When to Use Serverless Functions?**\
✔️ If you need to **query databases**\
✔️ If you need **complex processing**

***

### **9. Conclusion**

🚀 **Vercel makes Next.js deployment super easy.**\
🔹 **Edge Functions** allow you to run server-side logic **closer to the user** for faster and more secure apps.

✅ **Deploy your Next.js app in minutes with Vercel**\
✅ **Use Edge Functions for geolocation, authentication, and security**
