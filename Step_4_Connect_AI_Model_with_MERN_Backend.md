# Steps to Initialise AI Assistance in Your MERN Project

## Step 4 — Connect a Real AI Model with the MERN Backend

At this stage, the Fixkar AI Assistant has a working frontend and backend structure. We first experimented with a mock AI response so that the complete application flow could be developed and tested without depending on a paid AI API.

After that initial testing, we connected the backend with the **Gemini API** so the assistant can generate real AI responses.

> **Current implementation uses Gemini. OpenAI is not the active AI provider in the current implementation.**

---

## 4.1 What We Are Building

The goal is to move from predefined responses to a real AI-powered assistant.

### Earlier development flow

```text
User Message
      ↓
Fixkar Backend
      ↓
Mock AI / Predefined Responses
      ↓
Response
      ↓
Frontend
```

This was useful for developing and testing the chatbot UI and API flow, but it could not genuinely understand different user questions.

### Current flow

```text
User Message
      ↓
Fixkar Frontend
      ↓
Axios
      ↓
POST /api/ai/chat
      ↓
AI Controller
      ↓
AI Service
      ↓
Gemini API
      ↓
Gemini Model
      ↓
AI Response
      ↓
Frontend Chat
```

The assistant can now generate responses instead of selecting only from predefined mock responses.

---

## 4.2 Why We Started With Mock AI

During development, using a mock response system first allowed us to build the complete application flow without immediately depending on an external AI provider.

The mock system helped us test:

- Chat UI
- Backend AI route
- Controller
- AI service structure
- Axios communication
- Loading state
- Error handling
- Displaying AI responses

Once this flow was working correctly, we replaced the mock response generation with a real AI model.

The mock AI is therefore a **development/testing stage**, not the final AI implementation.

---

## 4.3 Select the Gemini Model

For the current implementation, we use a Gemini Flash model suitable for fast application responses and future AI-assistant development.

The selected model is configured through an environment variable instead of being hard-coded inside the service.

Example:

```env
GEMINI_MODEL=gemini-3.7-flash
```

Keeping the model name in `.env` makes it easier to change the model later without changing the service code.

> Model availability, free-tier limits, and pricing can change. Always verify the currently available model and limits in the official Gemini documentation before deploying to production.

---

## 4.4 Create the Gemini API Key

The Gemini API key is created through Google AI Studio.

General process:

```text
Google AI Studio
      ↓
API Keys
      ↓
Create API Key
      ↓
Copy API Key
      ↓
Fixkar Backend .env
```

The API key must remain on the backend.

Never expose it inside React code or commit it to GitHub.

---

## 4.5 Store Gemini Credentials in `.env`

Add the Gemini credentials to the backend `.env` file:

```env
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-3.7-flash
```

The real API key replaces `your_gemini_api_key`.

Make sure `.env` is included in `.gitignore`:

```gitignore
.env
```

The important security rule is:

```text
React Frontend
      ↓
Fixkar Backend
      ↓
Gemini API
```

Not:

```text
React Frontend
      ↓
Gemini API directly
```

---

## 4.6 Install the Gemini Node.js SDK

Inside the Fixkar backend, install Google's current JavaScript/Node.js GenAI SDK:

```bash
npm install @google/genai
```

This SDK is used by the backend to communicate with the Gemini API.

---

## 4.7 Configure the Gemini Client

Keep the Gemini client configuration separate from the main AI service.

Example structure:

```text
Ai_Assistant/
│
├── AiControllers/
│   └── ai.controller.js
│
├── AiRoutes/
│   └── ai.routes.js
│
└── AiServices/
    ├── ai.config.js
    └── ai.service.js
```

The configuration file initializes the Gemini client using the API key from `.env`.

Example:

```js
import { GoogleGenAI } from "@google/genai";

const ai = new GoogleGenAI({
  apiKey: process.env.GEMINI_API_KEY,
});

export default ai;
```

The API key is therefore never hard-coded into the source code.

---

## 4.8 Connect Gemini in `ai.service.js`

The AI service is responsible for sending the user's message to Gemini and returning the generated text.

Basic flow:

```text
User Message
      ↓
generateAIResponse(message)
      ↓
Gemini Client
      ↓
Selected Gemini Model
      ↓
Generated Text
```

A basic implementation is:

```js
import ai from "./ai.config.js";

export const generateAIResponse = async (message) => {
  const response = await ai.models.generateContent({
    model: process.env.GEMINI_MODEL,
    contents: message,
  });

  return response.text;
};
```

At this stage, this is intentionally simple. We are first verifying that real AI communication works before adding advanced features.

---

## 4.9 Keep the Controller Simple

The controller receives the message from the frontend and passes it to the AI service.

The basic flow is:

```text
HTTP Request
      ↓
AI Controller
      ↓
generateAIResponse(message)
      ↓
Gemini
      ↓
Reply
```

Example controller response:

```js
return res.status(200).json({
  success: true,
  reply,
});
```

The controller does not contain Gemini-specific logic. That logic stays inside `ai.service.js`.

---

## 4.10 Connect the Frontend with Axios

The Fixkar AI frontend sends the user's message to the backend using Axios.

Example request:

```js
await axios.post(
  `${import.meta.env.VITE_API_URL}/api/ai/chat`,
  {
    message: trimmedMessage,
  }
);
```

The frontend does not communicate directly with Gemini.

The request flow is:

```text
Fixkar AI Chat UI
      ↓
Axios
      ↓
POST /api/ai/chat
      ↓
Fixkar Backend
      ↓
Gemini
```

---

## 4.11 Display the Gemini Response

The backend returns the generated response:

```json
{
  "success": true,
  "reply": "Generated response from Gemini"
}
```

The frontend reads the reply and adds it to the chat messages.

```text
Gemini Response
      ↓
Axios Response
      ↓
setMessages()
      ↓
Chat Bubble
      ↓
User
```

This completes the basic real-AI chat loop.

---

## 4.12 Handle Temporary Gemini Errors

While testing the Gemini API, temporary `503` availability errors can occur when the selected model is experiencing high demand.

For temporary availability or rate-limit errors, the service can retry the request with a short exponential backoff.

Conceptually:

```text
Request
   ↓
Gemini
   ↓
Temporary 503 / 429
   ↓
Wait
   ↓
Retry
   ↓
Success
```

Retries should be limited. The application should never retry indefinitely.

---

## 4.13 What We Have Completed So Far

The current implementation has reached this stage:

```text
[✓] Separate AI feature branch created
[✓] Separate AI backend structure created
[✓] AI controller created
[✓] AI route created
[✓] AI service created
[✓] Mock AI created for initial testing
[✓] Frontend AI Assistant UI created
[✓] Frontend connected to backend with Axios
[✓] Mock response displayed in chat
[✓] Gemini API key configured in backend
[✓] Gemini Node.js SDK installed
[✓] Gemini client configured
[✓] Gemini model configured through .env
[✓] Real Gemini response connected to the AI service
[✓] Gemini response displayed in the Fixkar AI chat
[✓] Temporary API availability errors handled with limited retries
```

---

## 4.14 What the AI Can Do Right Now

At the current stage, Gemini can understand natural-language messages and generate dynamic responses.

For example, users can ask different questions instead of receiving only predefined mock responses.

However, the AI **does not yet have access to Fixkar's MongoDB data or application actions**.

For example, if a user asks:

```text
What is the status of my booking?
```

Gemini currently cannot look up the user's actual booking from MongoDB.

Similarly, if a user asks:

```text
Find me an electrician near Varanasi.
```

Gemini cannot yet query Fixkar professionals and return real matching professionals.

These capabilities are intentionally being implemented next.

---

## 4.15 Current Limitation

The current architecture is:

```text
User
 ↓
Fixkar AI
 ↓
Gemini
 ↓
Text Response
 ↓
User
```

The target architecture is:

```text
User
 ↓
Gemini
 ↓
Understand Intent
 ↓
Choose an Allowed Tool
 ↓
Fixkar Backend
 ↓
MongoDB / Application Data
 ↓
Tool Result
 ↓
Gemini
 ↓
Natural Language Response
 ↓
User
```

The second architecture is what will make Fixkar AI genuinely useful as a platform assistant.

---

## 4.16 What We Will Implement Next

The next stage is **not another AI provider integration**.

We will improve the existing Gemini integration by giving the AI knowledge about Fixkar and controlled access to useful platform operations.

The planned sequence is:

```text
Current
  ↓
Gemini generates responses
  ↓
Fixkar-specific system instructions
  ↓
Intent understanding
  ↓
Tool / Function Calling
  ↓
Backend AI Tools
  ↓
MongoDB data access
  ↓
Real Fixkar information
  ↓
Conversation memory
  ↓
RAG for Fixkar documentation
```

### Examples of future tools

```text
searchProfessionals()
getProfessionalDetails()
getServices()
getUserBookings()
getBookingStatus()
getPaymentStatus()
```

The AI will **not receive direct unrestricted access to MongoDB**. Instead, it will call controlled backend functions that validate requests and return only the required data.

---

## Important Architecture Rule

The AI model should not directly access the database.

Use:

```text
Gemini
   ↓
Approved Backend Tool
   ↓
Validation
   ↓
MongoDB
   ↓
Safe Result
   ↓
Gemini
```

This keeps the AI integration secure, maintainable, and easier to control.

---

## Final Status

At this point, Fixkar has moved from a **mock chatbot** to a **real Gemini-powered AI Assistant**.

The current goal is no longer to create another basic chatbot. The next goal is to make the assistant understand what the user wants and safely interact with Fixkar's real application data and features.
