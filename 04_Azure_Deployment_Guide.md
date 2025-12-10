# Azure Deployment Guide – Retail Banking Agentic AI

This guide walks through deploying the Retail Banking Agentic CRM demo on Azure using **Azure AI Foundry**, **Cosmos DB**, **App Service**, and a **Static Web App** for the UI.

---

## 1. Prerequisites

Before deployment, ensure you have:
- An active Azure subscription  
- Azure CLI installed  
- Node.js 18+  
- Git installed  
- Access to Azure AI Foundry (AI Hub)  
- Your GitHub repo cloned locally  

---

## 2. Resource Group
Create a dedicated resource group:
```bash
az group create \
  --name pm-portfolio-rg \
  --location eastus
```

## 3. Cosmos DB (Serverless)
```bash
Create a serverless Cosmos DB account:
az cosmosdb create \
  --name pmportfoliocosmos \
  --resource-group pm-portfolio-rg \
  --capabilities EnableServerless
```

Create a database & container (optional — can also be created in code):
```bash
az cosmosdb sql database create \
  -a pmportfoliocosmos \
  -g pm-portfolio-rg \
  -n crm

az cosmosdb sql container create \
  -a pmportfoliocosmos \
  -g pm-portfolio-rg \
  -d crm \
  -n customers \
  --partition-key-path "/id"
```


## 4. Backend Deployment (Node.js App Service)
Step 1 — Create App Service Plan
```bash
az appservice plan create \
  --name pm-plan \
  --resource-group pm-portfolio-rg \
  --sku B1
```

Step 2 — Create Web App
```bash
az webapp create \
  --resource-group pm-portfolio-rg \
  --plan pm-plan \
  --name pm-backend-demo \
  --runtime "NODE|18-lts"
```

Step 3 — Deploy Backend Code
```bash
From your backend directory:
az webapp up \
  --name pm-backend-demo \
  --resource-group pm-portfolio-rg \
  --runtime "NODE|18-lts"
```

## 5. Configuration Settings
Retrieve Cosmos connection string:
```bash
az cosmosdb keys list \
  -n pmportfoliocosmos \
  -g pm-portfolio-rg \
  --type connection-strings
```

Update App Service config:
```bash
az webapp config appsettings set \
  --resource-group pm-portfolio-rg \
  --name pm-backend-demo \
  --settings \
    COSMOS_DB_CONN="<cosmos-connection-string>" \
    FOUNDRY_ENDPOINT="<your-agent-endpoint>" \
    FOUNDRY_KEY="<your-foundry-key>"
```


## 6. Azure AI Foundry Setup (Agent Endpoint)
- Open Azure AI Foundry (formerly Azure AI Studio).
- Create an AI Hub (workspace).
- Deploy a model or configure a custom agent using:
- Azure OpenAI
- OSS models hosted in Foundry
- Expose it via an endpoint.
- Copy the endpoint + key into App Service settings.

## 7. Frontend Deployment (Static Web App)
Deploy the React UI:
```bash
az staticwebapp create \
  --name pm-crm-ui \
  --resource-group pm-portfolio-rg \
  --source . \
  --location eastus \
  --app-location "frontend"
```

If using GitHub Actions, Azure will generate a workflow file automatically.
Update frontend API environment:

VITE_API_URL=https://pm-backend-demo.azurewebsites.net

Add this in Static Web App → Configuration.
Rebuild & redeploy if needed.

## 8. Validate End-to-End Flow
- Open the Static Web App URL.
- Enter synthetic customer ID (e.g., cust-001).
- Trigger Portfolio Review.
Backend calls:
- Cosmos DB
- Foundry analysis agent
- Compliance agent
Results appear in UI.

## 9. Optional Enhancements
- Add Azure AD authentication
- Add Key Vault + managed identity
- Add Application Insights logging
- Convert backend to Azure Functions Premium
- Deploy full IaC using Terraform/Bicep (files provided in repo)

## 10. Troubleshooting
CORS issues
- Add frontend origin to App Service → CORS settings.
500 errors from agent
- Check Foundry endpoint URL
- Verify key validity
- Validate JSON formatting in prompts
Cosmos DB item not found
- Ensure your synthetic dataset is loaded into the container.
