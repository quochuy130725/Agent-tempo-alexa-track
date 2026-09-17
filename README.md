# 🤖 AgentTempo - Autonomous Life-Admin Assistant

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)
![AWS Bedrock](https://img.shields.io/badge/AWS_Bedrock-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)

**AgentTempo** is an AI-driven autonomous life-admin assistant built for the **Amazon Build, Ship, Shape Hackathon (Alexa+ Track)**. It handles messy daily schedules and resolves calendar conflicts using a "Human-in-the-Loop" approval interface.

## ✨ Core Features
*   **Conversational AI:** Powered by Claude 3.5 Sonnet (via AWS Bedrock) to naturally understand complex scheduling requests.
*   **Agentic Tool Calling:** AI autonomously determines when to query the calendar or propose new schedules.
*   **Human-in-the-Loop UI:** AI actions are not executed silently. They generate UI "Action Cards" pushed in real-time via WebSockets for user approval.
*   **Live Text Streaming:** Server-Sent Events (SSE) provide a natural, typing-like response experience.

## 🏗️ System Architecture

📱 **Frontend (Flutter)** ──(HTTP/SSE/WebSocket)──> ⚙️ **Backend (Node.js/Express)** ──(API)──> 🗄️ **Supabase & AWS Bedrock**

*   **Client Layer:** Manages the UI, Chat interface, and state (`Provider`). Listens to database events via WebSockets.
*   **Logic Layer (MCP Server):** Handles AI Tool Calling, communicates with the LLM, and streams text responses.
*   **Data & AI Layer:** PostgreSQL handles data storage, Supabase Realtime pushes live updates, and AWS Bedrock runs `Claude 3.5 Sonnet` for reasoning.

## 📂 Enterprise Codebase Structure (DDD Pattern)
```text
AgentTempo/
├── backend/                  # Node.js + TypeScript
│   ├── src/
│   │   ├── config/           # Environment, Supabase, AWS clients
│   │   ├── controllers/      # Route logic & HTTP response handling
│   │   ├── services/         # Core business logic (AI, Calendar)
│   │   ├── routes/           # Express API endpoints
│   │   ├── tools/            # MCP Agentic Tools & Zod schemas
│   │   └── utils/            # Shared utilities (logger, helpers)
│   └── package.json
│
└── frontend/                 # Flutter Mobile App
    ├── lib/
    │   ├── providers/        # State management (WebSocket listeners)
    │   ├── services/         # API & SSE clients
    │   ├── screens/          # Main UI views
    │   └── widgets/          # Reusable components (Action Cards)
    └── pubspec.yaml
