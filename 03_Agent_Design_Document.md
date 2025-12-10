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
