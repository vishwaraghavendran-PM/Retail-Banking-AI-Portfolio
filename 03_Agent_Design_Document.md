# Agentic AI Design – Multi-Agent Workflow for CRM

## 1. Agent Roles

### Analysis Agent
Purpose: Examine customer portfolio, compute churn risk, generate personalized product recommendations.

Inputs:
- Customer profile  
- Transaction summary  
- Portfolio balances  
- Product catalog (rules / eligibility)

Outputs:
```json
{
  "risk_score": 0.62,
  "offers": [
    {"id": "cc_upgrade", "reason": "High travel spend", "priority": 1}
  ],
  "cta": "Email outreach recommended"
}
