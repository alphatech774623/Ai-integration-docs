# Steps to Initialise AI Assistance in Your Project

## Step 1 — Create a Separate Branch for the AI Feature

Never start a new AI feature directly on the `main` branch.

The existing `main` branch should remain stable.

Create a separate branch from the existing project and develop the AI Assistant there.

```text
main
  │
  └── ai-assistant
```

### Create the branch

First make sure you are on the latest `main`:

```bash
git checkout main
git pull origin main
```

Now create a new branch:

```bash
git checkout -b feature/ai-assistant
```

Push the branch to GitHub:

```bash
git push -u origin feature/ai-assistant
```

From this point, all AI Assistant development will happen inside:

```text
feature/ai-assistant
```

The `main` branch will not be changed while the feature is being developed.

---

## Step 2 — Create a Separate AI Module

AI-related code should be kept separate from the existing application code.

Instead of mixing AI logic with existing controllers, routes, models, and services, create a dedicated AI structure.

For example:

```text
backend/
│
├── controllers/
│   └── ai.controller.js
│
├── models/
│   └── ai.model.js
│
├── routes/
│   └── ai.routes.js
│
├── services/
│   └── ai.service.js
│
└── config/
    └── ai.config.js
```

### Purpose of each file

| File | Purpose |
|---|---|
| `ai.controller.js` | Receives requests and sends responses |
| `ai.model.js` | Stores AI-related database data when required |
| `ai.routes.js` | Defines AI API endpoints |
| `ai.service.js` | Contains the main AI integration/business logic |
| `ai.config.js` | Keeps AI-related configuration separate |

The exact structure can be adjusted according to the existing project architecture.

---

## Step 3 — Development Flow

The initial development flow will be:

```text
Existing main branch
        ↓
feature/ai-assistant
        ↓
Create AI module
        ↓
Develop AI Assistant
        ↓
Test everything
        ↓
Fix issues
        ↓
Final testing
        ↓
Merge into main
```

The important rule is:

> **AI integration stays isolated until development and testing are complete.**

Only after the feature works correctly will we merge it into `main`.

---

## Important

At this stage, we are **only preparing the project**.

We are not implementing the AI API, chatbot, RAG, memory, tools, or AI Agent yet.

Those will be implemented in the next steps.


