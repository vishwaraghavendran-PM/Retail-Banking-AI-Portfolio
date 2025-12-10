# Agentic AI Design – Multi-Agent Workflow for CRM

## TOC
- 1 Overview
- 2 Agent Roles  
  - 2.1 Analysis Agent  
  - 2.2 Compliance Agent  
  - 2.3 Execution Agent  
- 3 Orchestration Sequence  
  - 3.1 Primary reasoning  
  - 3.2 Compliance guardrails  
  - 3.3 Automated execution  
  - 3.4 Final packaging  
- 4 Prompt Engineering Strategy  
  - 4.1 Bank persona rules  
  - 4.2 JSON schema enforcement  
  - 4.3 Temperature constraints  
  - 4.4 Compliance constraints  
- 5 Evaluation Strategy  
  - 5.1 Consistency checks  
  - 5.2 Completeness  
  - 5.3 JSON validity  
  - 5.4 Compliance accuracy  
  - 5.5 Latency benchmarks
- 6 Future Enhancements
 
## 1. Overview

The system uses a structured, multi-agent pattern to ensure that portfolio analysis is:

- analytically sound  
- compliant with retail-banking regulations  
- safely executed through automated actions  
- deterministic and auditable  

Agents operate in a sequenced pipeline, with each layer adding value and guardrails.

---

## 2. Agent Roles

### 2.1 Analysis Agent
Purpose: Examine customer portfolio, compute churn risk, generate personalized product recommendations.

Inputs:
- Customer profile  
- Transaction summary  
- Portfolio balances  
- Product catalog (rules / eligibility)

```json
{
  "risk_score": 0.62,
  "offers": [
    {"id": "cc_upgrade", "reason": "High travel spend", "priority": 1}
  ],
  "cta": "Email outreach recommended"
}
```
### 2.2 Compliance Agent
Purpose: Validate outputs from the Analysis Agent for:
- regulatory alignment
- forbidden financial claims
- sensitive-word violations
- suitability/appropriateness checks

```json
  {
  "approved": true,
  "flags": []
}
```
If "approved": false, the system returns flagged issues and halts execution.

### 2.3 Execution Agent
Purpose: Convert approved recommendations into operational CRM actions, including:

- generating CRM task payloads
- creating communication drafts (email/SMS)
- writing updates to Cosmos DB

Example Outputs:

```json
{
  "crm_task": {
    "taskType": "PortfolioReview",
    "priority": "High",
    "customerId": "12345"
  },
  "message": "Draft outreach message...",
  "dbUpdate": true
}
```

## 3. Orchestration Sequence

The orchestration logic is intentionally linear to maintain compliance, safety, and traceability. It follows a strict three-stage pipeline.

### 3.1 Primary Agent Reasoning
The Analysis Agent generates the core insight and recommended actions using deterministic prompting and schema constraints.

### 3.2 Compliance Guardrail Check
The Compliance Agent evaluates:

- regulatory correctness  
- tone and suitability  
- absence of sensitive or misleading claims  

If compliance fails, the pipeline terminates with a structured error.

### 3.3 Automated Action Execution
The Execution Agent finalizes operational steps:

- storing outputs  
- preparing customer-facing communication  
- generating tasks for RM or CRM systems  

### 3.4 Final Response Packaging
The backend merges the outputs into a single standardized JSON response for the UI.

---

## 4. Prompt Engineering Strategy

Prompts serve as the behavioral contract for each agent. The following principles guide the design.

### 4.1 Bank Persona + Required Disclosures
Each agent uses a system prompt defining:

- bank-compliant tone  
- mandatory disclaimers  
- sensitivity rules  
- restrictions around claims, predictions, or guarantees  

### 4.2 JSON Schema Enforcement
All agents return responses strictly adhering to a declared schema.

Schema constraints ensure:

- predictable parsing  
- safety checks  
- clean handoff to downstream agents  

### 4.3 Low Temperature for Determinism
Temperature is kept between **0.1 and 0.3** to:

- reduce hallucinations  
- enforce consistency  
- improve testability  

### 4.4 Explicit Compliance Constraints
Prompts include:

- forbidden word lists  
- rules against speculative claims  
- suitability/appropriateness filters  
- guidance for risk-aligned advice  

This ensures safety even before Compliance Agent validation.

---

## 5. Evaluation Strategy

To treat this as a production-grade agentic system, evaluation focuses on the following areas.

### 5.1 Consistency Across Runs
Identical inputs should produce near-identical outputs  
(variability tolerance depends on temperature settings).

### 5.2 Completeness
Outputs must include:

- risk indicators  
- product opportunities  
- action plan  
- call-to-action (CTA)  

### 5.3 Accuracy of JSON
The system validates:

- structure  
- property types  
- required fields  
- nested completeness  

### 5.4 Compliance Correctness
The Compliance Agent must reliably:

- catch inappropriate phrasing  
- detect prohibited claims  
- reject incomplete analysis  

### 5.5 Latency
Pipeline should complete under **2.5 seconds** end-to-end:

- <1.0s analysis  
- <0.8s compliance  
- <0.7s execution  

Performance is logged for monitoring.

---

## 6. Future Enhancements

- Add Retrieval Agent for document grounding  
- Add Risk Scoring Agent for more advanced analytics  
- Introduce feedback-loop fine-tuning  
- Extend compliance rules via Azure Prompt Shields and Content Safety  
