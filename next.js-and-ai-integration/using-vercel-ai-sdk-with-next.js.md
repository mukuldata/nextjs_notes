# Using Vercel AI SDK with Next.js

### **Table of Contents**

1. **Introduction to Vercel AI SDK**
2. **Why Use Vercel AI SDK with Next.js?**
3. **Installing Vercel AI SDK in Next.js**
4. **Using Vercel AI SDK for AI Models (Example with OpenAI API)**
5. **Building an AI Chatbot in Next.js with Vercel AI SDK**
6. **Streaming AI Responses with Vercel AI SDK**
7. **Best Practices for Using Vercel AI SDK**
8. **Conclusion**

***

### **1. Introduction to Vercel AI SDK**

🔹 **Vercel AI SDK** is a tool that helps developers integrate **AI models** like OpenAI’s GPT, other LLMs into their **Next.js applications** easily.\
🔹 It **simplifies API calls**, enables **real-time streaming**, and works well with **Server Actions & Edge Functions** in Next.js.

📌 **What Can You Do with It?**\
✔ Create AI-powered **chatbots**\
✔ Generate **text, images, or summaries** using AI\
✔ Stream AI responses progressively

***

### **2. Why Use Vercel AI SDK with Next.js?**

🚀 **Benefits of using Vercel AI SDK:**\
✔ **Easy API integration** – No need for complex HTTP requests.\
✔ **Supports streaming** – AI responses appear **word by word** like ChatGPT.\
✔ **Works with Server Actions** – Optimized for Next.js **Server Components**.\
✔ **Edge deployment ready** – Fast execution with **Vercel Edge Functions**.

✅ **Use Cases:**

* AI-powered **chatbots**
* Text **summarization**
* AI-based **code generation**
* **Q\&A systems** with real-time responses

***

### **3. Installing Vercel AI SDK in Next.js**

📌 **First, install the SDK in your Next.js project:**

```sh
npm install ai
```

or using Yarn:

```sh
yarn add ai
```

***

### **4. Using Vercel AI SDK for AI Models (Example with OpenAI API)**

📌 **Let's connect OpenAI’s GPT model to Next.js using Vercel AI SDK.**

#### **Step 1: Set Up API Route for AI Requests**

🔹 We create an API route that **sends user input to OpenAI and gets AI-generated responses**.

📌 **Create `app/api/chat/route.ts` in your Next.js project:**

```ts
import { OpenAIStream, StreamingTextResponse } from 'ai';
import OpenAI from 'openai';

// Initialize OpenAI API
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY, // Set API Key in .env file
});

export async function POST(req: Request) {
  const { prompt } = await req.json(); // Get user input

  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: [{ role: "user", content: prompt }],
    stream: true, // Enable streaming for real-time responses
  });

  // Convert OpenAI response into a readable stream
  const stream = OpenAIStream(response);
  return new StreamingTextResponse(stream);
}
```

✅ **What Happens Here?**\
✔ The API **receives user input (`prompt`)**.\
✔ Sends input to **OpenAI’s GPT-3.5-turbo** model.\
✔ Uses **streaming** to send responses back word by word.\
✔ Returns AI-generated text **as a live stream**.

***

### **5. Building an AI Chatbot in Next.js with Vercel AI SDK**

📌 **Now, create a simple chatbot UI that connects with our AI API.**

#### **Step 1: Create Chat Component**

🔹 This component will **send messages to the AI API** and **display responses**.

📌 **Create `app/chat/page.tsx`**

```tsx
"use client";

import { useChat } from "ai/react";

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat({
    api: "/api/chat",
  });

  return (
    <div className="chat-container">
      <h1>AI Chatbot</h1>
      <div className="messages">
        {messages.map((m, index) => (
          <p key={index} className={m.role === "user" ? "user" : "ai"}>
            <strong>{m.role}:</strong> {m.content}
          </p>
        ))}
      </div>
      <form onSubmit={handleSubmit}>
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Ask me anything..."
        />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

✅ **How It Works:**\
✔ **useChat hook** simplifies AI interactions.\
✔ Messages **update in real-time** as AI responds.\
✔ **User input** is sent to our API (`/api/chat`).\
✔ AI-generated responses **appear progressively**.

***

### **6. Streaming AI Responses with Vercel AI SDK**

📌 **Streaming improves user experience by displaying AI responses as they are generated (like ChatGPT).**

#### **Modify API Route to Enable Streaming**

🔹 If not already done, make sure **streaming is enabled** in `app/api/chat/route.ts`:

```ts
const response = await openai.chat.completions.create({
  model: "gpt-3.5-turbo",
  messages: [{ role: "user", content: prompt }],
  stream: true, // Enables live streaming
});
```

✅ **Now, AI responses will be shown word by word instead of waiting for the full answer.**

***

### **7. Best Practices for Using Vercel AI SDK**

🚀 **Follow these tips for the best performance:**\
✔ **Use Edge Functions** for faster AI responses.\
✔ **Enable Streaming** to avoid long wait times.\
✔ **Store API keys securely** in `.env.local` file.\
✔ **Use `useChat()` for easier state management** in UI.\
✔ **Optimize prompts** to get better AI responses.

📌 **Example `.env.local` (DO NOT share this file)**

```sh
OPENAI_API_KEY=your-secret-api-key
```

***

### **8. Conclusion**

🔥 **Vercel AI SDK + Next.js** makes AI integration super easy!\
🔹 It **simplifies API calls**, **enables streaming**, and **optimizes performance**.\
🔹 With **useChat() hook**, AI-powered apps become **faster and more interactive**.\
🔹 **Perfect for chatbots, text generation, and AI-powered applications.**
