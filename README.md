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
AgentTempo utilizes a decoupled full-stack architecture:

*   📱 **Frontend (Flutter):** Manages the UI, Chat interface, and state (`Provider`). Listens to database events via WebSockets.
*   ⚙️ **Backend (Node.js + Express):** Acts as the MCP Server. Handles AI Tool Calling, communicates with the LLM, and streams text responses.
*   🧠 **AI Engine (AWS Bedrock):** Runs `Claude 3.5 Sonnet` for natural language reasoning and JSON tool payload generation.
*   🗄️ **Database & Realtime (Supabase):** PostgreSQL handles data storage, while Supabase Realtime pushes live updates to the frontend.

## 🔄 Data Flow (Human-in-the-Loop)
Our core automated workflow strictly requires human approval before modifying the calendar:

1.  👤 **User Request:** The user types a command (e.g., "Reschedule my afternoon meeting") in the Flutter app.
2.  🧠 **AI Reasoning:** The Node.js backend sends the context to AWS Bedrock. Claude 3.5 decides to call a tool and returns a JSON payload (`proposeSchedule`).
3.  ⏸️ **Staging Action:** The backend intercepts this JSON and saves it to the Supabase `action_cards` table with a `pending` status.
4.  ⚡ **Realtime Trigger:** Supabase instantly pushes a WebSocket signal to Flutter, popping up an "Action Card" on the screen.
5.  ✅ **Human Approval:** 
    *   If **Approved**, the backend updates the actual calendar and resolves the card.
    *   If **Rejected**, the backend cancels the update and asks the AI for a new solution.

## 🚀 Local Setup

**1. Clone the repository:**
```bash
cd OJT2026
git clone [https://github.com/your-username/agent-tempo.git](https://github.com/your-username/agent-tempo.git)
cd agent-tempo
