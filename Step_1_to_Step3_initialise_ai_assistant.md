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

        ### Development Flow

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


      # Steps to Initialise AI Assistance in Your Project

        ## Step 3 — AI Provider Credentials Setup

        Is step mein hum Fixkar project ke liye OpenAI API provider ki credentials configuration complete karenge.

        Is step mein sirf ye kaam honge:

        1. OpenAI mein new project create karna
        2. API key create karna
        3. API secret ko backend `.env` file mein securely rakhna
        4. AI configuration file mein environment variables ko configure karna

        ---

        ### 3.1 OpenAI mein New Project Create Karein

        OpenAI API Platform par login karein:

        https://platform.openai.com/

        Login karne ke baad project creation option open karein.

        Select:

        ```text
        Create project
        ```

        Project name rakhein:

        ```text
        Fixkar AI Assistant
        ```

        Agar description ka option aaye to:

        ```text
        AI Assistant integration for Fixkar MERN application
        ```

        Phir:

        ```text
        Create
        ```

        par click karein.

        Ab Fixkar ke AI integration ke liye dedicated OpenAI project create ho jayega.

        ---

        ### 3.2 API Key Create Karein

        Created `Fixkar AI Assistant` project ke andar API Keys section open karein.

        Navigation:

        ```text
        Fixkar AI Assistant
                ↓
        Project Settings
                ↓
        API Keys
        ```

        Phir:

        ```text
        Create new secret key
        ```

        par click karein.

        Agar key ka name/label poocha jaye to:

        ```text
        Fixkar AI Development
        ```

        rakh sakte hain.

        API key create hone ke baad jo secret key milegi usse immediately copy karein.

        Example:

        ```text
        sk-xxxxxxxxxxxxxxxxxxxxxxxx
        ```

        Important:

        - API key kisi ke saath share nahi karni hai.
        - API key GitHub par commit nahi karni hai.
        - API key frontend/React code mein nahi rakhni hai.
        - API key public code mein nahi rakhni hai.
        - API key backend `.env` file mein securely rakhni hai.

        ---

        ### 3.3 API Secret ko Backend `.env` File Mein Rakhein

        Fixkar ke backend project mein `.env` file open karein.

        Example:

        ```text
        Fixkar_Backend/
        │
        ├── controllers/
        ├── models/
        ├── routes/
        ├── services/
        ├── config/
        ├── .env
        └── server.js
        ```

        `.env` file mein API key add karein:

        ```env
        AI_API_KEY=PASTE_YOUR_SECRET_API_KEY_HERE
        ```

        Example:

        ```env
        AI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxx
        ```

        Yahan apni actual generated API key paste karein.

        `.env` file ko GitHub par push nahi karna hai.

        Ensure karein ki `.gitignore` mein:

        ```gitignore
        .env
        .env.*
        ```

        present ho.

        ---

        ### 3.4 AI Configuration File Mein API Credentials Configure Karein

        Ab backend ke `config` folder mein AI configuration file create karein:

        ```text
        backend/
        └── config/
        └── ai.config.js
        ```

        `ai.config.js` mein `.env` se API credentials read karein:

        ```js
        const aiConfig = {
        apiKey: process.env.AI_API_KEY,
        model: process.env.AI_MODEL,
        };

        export default aiConfig;
        ```

        Iska purpose AI-related configuration ko ek dedicated place par rakhna hai.

        Abhi `AI_MODEL` configure nahi karna hai. Model next step mein select kiya jayega.

        Current flow:

        ```text
        OpenAI Project
        ↓
        API Key
        ↓
        .env
        ↓
        ai.config.js
        ↓
        AI Service
        ```

        Is step ke baad AI provider credentials securely configure ho jayengi.
