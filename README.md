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
AgentTempo utilizes a decoupled full-stack architecture.

```mermaid
graph TD
    classDef frontend fill:#02569B,stroke:#fff,stroke-width:2px,color:#fff;
    classDef backend fill:#43853D,stroke:#fff,stroke-width:2px,color:#fff;
    classDef ai fill:#232F3E,stroke:#fff,stroke-width:2px,color:#fff;
    classDef db fill:#3ECF8E,stroke:#fff,stroke-width:2px,color:#000;

    subgraph Client Layer [1. Frontend - Flutter Mobile App]
        UI[Chat Interface & Action Cards]
        State[Provider State Management]
        UI <--> State
    end

    subgraph Logic Layer [2. Backend - Node.js + Express MCP]
        API[HTTP REST API Endpoints]
        SSE[Server-Sent Events Streamer]
        Tools[Tool Calling Definitions / Zod]
        API <--> Tools
        API <--> SSE
    end

    subgraph Data & AI Layer [3. Infrastructure]
        LLM[AWS Bedrock - Claude 3.5]
        DB[(Supabase PostgreSQL)]
        RT((Supabase Realtime WebSockets))
    end

    UI -- "1. HTTP POST Request" --> API
    SSE -- "2. Stream Text Responses" --> UI
    Tools <== "3. Context & JSON Payloads" ==> LLM
    API -- "4. Insert pending Action Card" --> DB
    DB -. "Trigger Event" .-> RT
    RT -- "5. Push WebSocket Signal" --> State
    
    class UI,State frontend;
    class API,SSE,Tools backend;
    class LLM ai;
    class DB,RT db;
