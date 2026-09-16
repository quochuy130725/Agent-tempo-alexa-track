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
1.  **Frontend:** Flutter (Mobile) with Provider for state management.
2.  **Backend:** Node.js + TypeScript (Express) acting as the orchestration layer.
3.  **Database & Realtime:** Supabase (PostgreSQL) handling Auth, storage, and WebSockets.
4.  **AI Engine:** AWS Bedrock serving Claude 3.5 Sonnet.

## 🚀 Local Setup

**1. Clone the repository into your workspace:**
```bash
cd OJT2026
git clone [https://github.com/your-username/agent-tempo.git](https://github.com/your-username/agent-tempo.git)
cd agent-tempo


2. Backend Setup:

Bash
cd backend
npm install
# Add your .env file with Supabase and AWS credentials
npm run dev
3. Frontend Setup:

Bash
cd ../frontend
flutter pub get
# Add your .env file with Supabase Publishable Key
flutter run
