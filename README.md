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


 🔄 Data Flow (Human-in-the-Loop)
Our core automated workflow strictly requires human approval before modifying the calendar:

[User] ──(1) Chat Command──> [Flutter UI] ──(2) HTTP POST──> [Node.js Server]
[Node.js Server] ──(3) Context & Tools──> [AWS Bedrock (Claude 3.5)]
[AWS Bedrock] ──(4) JSON Tool Payload──> [Node.js Server]
[Node.js Server] ──(5) INSERT pending record──> [Supabase Database]
[Supabase Database] ──(6) WebSocket Trigger──> [Flutter UI]
[Flutter UI] ──(7) Popup Action Card──> [User Approval]

If Approved: Backend updates the actual calendar and resolves the card.

If Rejected: Backend cancels the update and asks the AI for a new solution.


🚀 Local Setup
1. Clone the repository:

Bash
git clone [https://github.com/your-username/agent-tempo.git](https://github.com/your-username/agent-tempo.git)
cd agent-tempo
2. Backend Setup:

Bash
cd backend
npm install
npm run dev
3. Frontend Setup:

Bash
cd frontend
flutter pub get
flutter run
