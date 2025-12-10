# Solution Architecture – Retail Banking Agentic AI

## 1. High-Level Architecture
The system is designed as a modular, multi-agent pipeline using Azure AI Foundry orchestration principles.

Key components:
- **Azure AI Foundry Hub**  
  Hosts model endpoints, tools, and agent configurations.
- **Analysis Agent (Primary LLM Agent)**  
  Performs portfolio reasoning, churn scoring, and offer selection.
- **Compliance Agent**  
  Validates outputs for regulatory or policy violations.
- **Execution Agent**  
  Automates CRM tasks or generates follow-up actions.
- **Cosmos DB**  
  Stores customer profiles, transactions, and agent outputs.
- **Node.js Backend**  
  Orchestrates calls between agents and stores results.
- **React Frontend (Static Web App)**  
  Provides a demo UI for portfolio review workflows.
- **Azure Monitoring / App Insights**  
  Tracks agent steps and performance metrics.

## 2. Diagram (Text-Based)
flowchart LR
    U[User] --> R[React Frontend]
    R --> B[Backend API]
    B --> A[Analysis Agent]
    A --> C[Cosmos DB]
    CA[Compliance Agent] --> C
    C --> E[Execution Agent]
    E --> T[CRM Task]
    E --> M[Email]

## 3. Data Flow
1. Backend retrieves customer info from Cosmos DB.  
2. Analysis agent processes portfolio and returns structured insights.  
3. Compliance agent checks recommendations.  
4. Backend logs actions + returns result to UI.  
5. Execution agent optionally triggers CRM workflow.  
6. Entire flow logged for analytics.  

## 4. Tech Stack
- Azure AI Foundry (Hub + Agents)
- Azure OpenAI / Foundry-hosted models
- Azure Cosmos DB (Serverless)
- Node.js (Express)
- React + Static Web Apps
- Azure App Service
- Azure Monitor / App Insights

## 5. Security + Governance
- Managed Identity for DB access  
- All secrets stored in Key Vault  
- PII is synthetic  
- All agent outputs logged for governance  
