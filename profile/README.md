# Welcome to the Agentic AI Project 🤖

This organization hosts the **Agentic AI Platform**, a cutting-edge ecosystem for building, managing, and interacting with autonomous AI agents.

## 🌟 Mission
To democratize access to advanced agentic workflows, enabling developers to build complex, stateful, and tool-using AI applications with ease.

## 🏗️ Core Architecture
Our platform represents a modern microservices approach to AI:
- **Frontend**: React & HeroUI based interactive dashboard.
- **Backend**: A suite of specialized Node.js and Python microservices.
- **Intelligence**: LangGraph-orchestrated agents capable of planning and self-correction.
- **Integration**: Model Context Protocol (MCP) support for extensible tools.

## 🧠 Agent Architecture
Our agents are powered by **LangGraph**, enabling them to plan, execute, research, and self-correct.

```mermaid
graph TD
    START((START)) --> Router{needsPlanning?<br/>Model: gemini-2.0-flash-lite}

    %% Planning Phase & Research Loop
    Router -- "No Req / Empty" --> LLM[LLM Call <br/> Decision Node <br/> Model: gemini-2.5-flash-lite]
    Router -- COMPLEX --> Planning[Planning Node <br/> Model: gemini-2.5-flash-lite]
    Router -- SIMPLE --> LLM
    
    Planning -- "Ready to Plan" --> LLM
    Planning -- "Need Info (Research)" --> Tool[Tool Node]

    %% Main Execution Loop
    LLM --> Check{shouldContinue?}
    
    %% Branches from Decision
    Check -- "Msg > 12 OR <br/> Size > 200k & Msg > 5" --> Summarize[Summarize Node <br/> Model: gemini-2.0-flash-lite]
    Check -- "tool_call" --> Tool
    Check -- "ready_to_reply" --> Final[Final Answer Node <br/> Model: gemini-2.5-flash-lite]
    Check -- "Steps >= 30 <br/> or No Msg" --> END((END))
    
    %% Tool Execution & Replanning & Research Return
    Tool --> ReplanCheck{shouldReplan?}
    ReplanCheck -- "Researching (No Plan)" --> Planning
    ReplanCheck -- "Execution Success" --> LLM
    ReplanCheck -- "Failure >= 2 <br/> Loop Detected <br/> Steps >= 20" --> Replanner[Replanner Node <br/> Model: gemini-2.5-flash-lite]
    Replanner -- "New Plan" --> LLM
    
    %% Summarization Loop
    Summarize -- "Context Compressed" --> LLM
    
    %% Terminal States
    Final --> END
    
    %% Styling
    classDef plain fill:#000,stroke:#333,stroke-width:1px;
    classDef special fill:#000,stroke:#01579b,stroke-width:2px;
    classDef term fill:#000,stroke:#333,stroke-width:2px;
    
    class Planning,LLM,Tool,Replanner,Summarize,Final plain;
    class Router,Check,ReplanCheck special;
    class START,END term;
```

## 🚀 Service Ecosystem

| Service | Description | Path |
| :--- | :--- | :--- |
| **Frontend** | React-based interactive dashboard and chat interface. | `ai-agents-frontend` |
| **API Gateway** | Unified entry point handling routing, auth, and proxying. | `backend/api-gateway` |
| **Agent Service** | Core intelligence engine using LangGraph for orchestration. | `backend/agent-service` |
| **Auth Service** | Manages user authentication (OAuth, JWT) and roles. | `backend/auth-service` |
| **User Service** | User profile management and secure file uploads. | `backend/user-service` |
| **LLM Chat Service** | Python service for direct LLM interactions and streaming. | `backend/llm-chat-service` |
| **Chatbot Cards** | Manages conversation history and UI "cards". | `backend/chatbot-cards-service` |
| **Feature Service** | Dynamic feature flag configuration system. | `backend/feature-service` |
| **MCP Client** | Aggregates tools from connected MCP servers. | `backend/mcp-client` |
| **My MCP Server** | Custom MCP server providing tools (Weather, FireTV). | `mcp-server/my-mcp-server` |

---
*Built with ❤️ by the Agentic AI Team.*
