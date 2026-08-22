# Steps to Initialise AI Assistance in Your MERN Project

## Step 6 — Build the Fixkar AI Knowledge Base

At this stage, the basic Gemini integration is already working.

The next goal is to make the AI understand the Fixkar platform instead of behaving like a generic chatbot.

The important idea is:

```text
Generic Gemini AI
        ↓
Fixkar Knowledge
        ↓
Fixkar AI Assistant
```

The knowledge layer is responsible for explaining **how Fixkar works**.

Live user data and platform actions will be connected later through backend tools.

---

# 6.1 Why We Need a Knowledge Base

A normal Gemini model has general-world knowledge, but it does not automatically know the internal behaviour of the Fixkar platform.

For example, Gemini cannot automatically know:

- How a professional registers on Fixkar
- How professional onboarding works
- What a visitor can do
- What a customer can do
- How Fixkar bookings work
- How late cancellation works
- How professional charges work
- How professional payouts work
- How Fixkar's dynamic services work

Therefore, we created a dedicated knowledge base for Fixkar.

---

# 6.2 Knowledge Base Structure

The Fixkar AI knowledge base is kept separately from the normal backend business logic.

Current structure:

```text
Ai_Assistant/
│
└── AiKnowledge/
    ├── platform.md
    ├── user-roles.md
    ├── services.md
    ├── booking.md
    ├── payments.md
    ├── professional-profile.md
    ├── ai-response-rules.md
    ├── additional-platform-context.md
    └── system-instructions.md
```

Each file has a specific responsibility.

---

# 6.3 `platform.md`

This file explains the overall Fixkar platform.

It gives the AI the general context of:

- What Fixkar is
- Who uses Fixkar
- General customer workflow
- General professional workflow
- Platform-level behaviour
- Knowledge vs live application data

An important rule in this document is that Fixkar services are dynamic.

The AI must not create a permanent list of services from its own knowledge.

Current service availability must eventually come from live Fixkar data.

---

# 6.4 `user-roles.md`

This file explains the three main user types:

```text
Visitor
Customer
Professional
```

It documents the permissions and capabilities of each role.

It also documents the professional registration and onboarding flow:

```text
Register
   ↓
Google or normal registration
   ↓
Mobile OTP verification
   ↓
Professional onboarding
   ↓
Identity/document submission
   ↓
Pending approval
   ↓
Admin approval
   ↓
Professional dashboard
   ↓
Profile completion
```

This allows the AI to answer general questions about becoming a Fixkar professional.

---

# 6.5 `services.md`

The service system is documented separately because Fixkar services are dynamic.

This file explains:

- Services are database-driven
- Services can change over time
- Service skills/tasks are also dynamic
- Service types
- Skill types
- Pricing sources
- Fixed vs inspection booking concepts
- Service matching rules

The most important rule is:

```text
Static Knowledge
      ↓
Explain how services work

Live Backend Data
      ↓
Tell which services are currently available
```

The AI must not invent a service simply because it exists in the real world.

---

# 6.6 `booking.md`

This file documents the actual Fixkar booking architecture.

It covers:

- Direct hire
- Pickup-based booking
- Booking creation
- Service/task selection
- Booking validation
- Professional acceptance/rejection
- Reaching the customer
- Reached OTP
- In-progress state
- Quote flow
- Payment connection
- Cancellation
- Late cancellation
- Booking completion
- Assignment status
- Booking status

The AI can use this document to answer general booking questions.

Actual booking status for a particular user will later come from a backend tool.

---

# 6.7 `payments.md`

This file explains the Fixkar payment system and payment-related intents.

It covers:

- Final service payment
- Payment verification
- Razorpay flow
- Fixed-price payment
- Quote-based payment
- Late cancellation payment
- Normal cancellation
- Professional earning
- Professional wallet
- Professional withdrawal
- Admin-managed payout
- Commission and settlement concepts

The AI must distinguish between different payment intents instead of responding to every question containing the word `payment` in the same way.

For example:

```text
"Final payment kab karna hai?"
        ↓
Final Service Payment
```

while:

```text
"Professional ko payment kaise milega?"
        ↓
Professional Earnings / Payout
```

Actual user payment status will later come from live backend data.

---

# 6.8 `professional-profile.md`

This file contains detailed professional-side knowledge.

It documents:

- Professional profile
- Profile completion
- Basic information updates
- Profile picture update
- Skills
- Dynamic skills
- Service-specific pricing
- Specialized-service pricing
- Visiting charges
- Gallery
- Gallery upload
- Gallery deletion
- Bank details
- Bank verification
- Profile health
- Reviews
- Profile visibility
- Professional-side error handling

It also defines when the AI should answer directly and when live professional data is required.

---

# 6.9 `ai-response-rules.md`

This file defines how the AI should behave when information is missing or uncertain.

The most important rule is:

> Never guess when the answer is unknown, unavailable, or unverified.

The AI must distinguish between:

```text
Known information
      ↓
Answer
```

```text
Current information required
      ↓
Use live data when available
```

```text
Information unavailable / uncertain
      ↓
Do not guess
      ↓
Guide the user toward Fixkar Support
```

This prevents hallucinated Fixkar policies, prices, statuses, and other information.

---

# 6.10 `additional-platform-context.md`

This file contains additional general context that does not need a separate large knowledge document.

It reinforces:

- Service marketplace concept
- Location-based discovery
- Dynamic platform data
- Privacy
- General vs current platform information
- Accuracy over guessing
- Unsupported information handling

It acts as a supporting knowledge layer for the other documents.

---

# 6.11 `system-instructions.md`

This is the most important control document for the Gemini model.

It does not describe every Fixkar feature itself.

Instead, it tells the AI **how to use the Fixkar knowledge base**.

It defines that:

- The AI is the Fixkar AI Assistant
- The Markdown files are its Fixkar knowledge base
- General Fixkar questions should follow the documented knowledge
- Fixkar functionality must not be invented
- User intent must be understood before answering
- Current/user-specific data is different from general knowledge
- Tool results must never be fabricated
- Sensitive information must be protected
- System errors must not be diagnosed by guessing
- The AI should answer in the user's language/style
- Accuracy is more important than appearing confident

The overall model instruction is:

```text
Knowledge explains how Fixkar works.
Tools will later provide current data and actions.
```

---

# 6.12 Knowledge vs Live Data

This is a fundamental architectural separation.

### Knowledge

Knowledge is used for general platform questions.

Examples:

```text
What is Fixkar?
How does booking work?
How does a professional register?
How does late cancellation work?
How does professional pricing work?
```

### Live Data

Live data is needed for questions about the current state of the platform or a user's account.

Examples:

```text
What is my booking status?
What is my wallet balance?
Did my payment succeed?
Which professionals are available now?
Which services are currently active?
Is my bank verification complete?
```

Currently, the AI knowledge layer does not provide these live answers by itself.

Those capabilities will be implemented later through controlled backend tools.

---

# 6.13 Why We Are Not Implementing Tools Yet

We intentionally separated the work into two phases.

### Phase 1 — Knowledge

```text
Gemini
  ↓
Understand Fixkar
  ↓
Answer general questions accurately
```

### Phase 2 — Tools

```text
Gemini
  ↓
Understand user intent
  ↓
Call authorized backend tool
  ↓
Get actual Fixkar data
  ↓
Generate response
```

This separation makes the system easier to test and debug.

We first want Gemini to understand Fixkar correctly before giving it access to live application operations.

---

# 6.14 What Has Been Completed

At this stage the AI development has reached the following state:

```text
[✓] Separate AI feature branch created
[✓] Separate AI module created
[✓] AI controller created
[✓] AI route created
[✓] AI service created
[✓] Mock AI used for initial development/testing
[✓] Fixkar AI frontend created
[✓] Frontend connected to backend using Axios
[✓] Gemini API integrated
[✓] Gemini model configured
[✓] Gemini responses connected to the AI service
[✓] Temporary Gemini availability errors handled
[✓] Fixkar knowledge base designed
[✓] Fixkar platform knowledge documented
[✓] User roles documented
[✓] Dynamic services documented
[✓] Booking system documented
[✓] Payment system documented
[✓] Professional profile functionality documented
[✓] AI uncertainty/anti-hallucination rules documented
[✓] AI system instructions created
```

---

# 6.15 Current Architecture

The current conceptual architecture is:

```text
                    User
                      ↓
                Fixkar AI Chat
                      ↓
                 Axios Request
                      ↓
                Fixkar Backend
                      ↓
                 AI Service
                      ↓
                  Gemini
                      ↑
                      │
             Fixkar Knowledge Base
                 /           \
                /             \
        System Instructions   Knowledge
                              Markdown Files
```

At this stage, the AI knows the documented Fixkar platform context, but it does not yet have unrestricted access to Fixkar's MongoDB data.

---

# 6.16 Next Step

The next implementation step is to connect the knowledge base to the AI service.

We will create a dedicated knowledge loader/service that reads the Markdown files and prepares them as AI context.

Target architecture:

```text
AiKnowledge/*.md
       ↓
Knowledge Loader
       ↓
Prepared Fixkar Context
       ↓
System Instructions
       ↓
Gemini
       ↓
Fixkar-specific Response
```

For the first implementation, we will keep this simple and verify that Gemini can correctly answer Fixkar-related questions using the knowledge base.

After that works reliably, we can introduce retrieval/RAG so that only the most relevant knowledge is sent to Gemini.

Tools for live database access and platform actions will be implemented after the knowledge layer is stable.

---

# Final Status

The Fixkar AI Assistant has now moved from a generic Gemini chatbot to a structured AI system with a dedicated Fixkar knowledge layer.

The next goal is to make Gemini actually consume this knowledge at runtime.

```text
Knowledge Base
      ↓
Knowledge Loader
      ↓
Gemini
      ↓
Fixkar-aware Answers
      ↓
Later:
Tools → Live Data → Actions
```
