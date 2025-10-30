# Lab 2 - Create a Knowledge Assistant agent for HR in Copilot Studio that leverages Azure AI Search

### Estimated Duration: 60 Minutes

## Overview
Provision **Azure AI Search**, index HR policies/FAQs (keyword or vector), connect the index to a **Copilot Studio** HR agent using **Generative answers**, add basic HR tools (leave/benefits), apply safety/evaluation, and publish.

## Objectives
- Task 1: Provision **Azure AI Search**  
- Task 2: Import and index HR content (keyword or vector)  
- Task 3: Create an HR agent & connect to the index  
- Task 4: Add HR tools/flows (Leave Balance, Benefits Summary)  
- Task 5: Apply safety & evaluation practices  
- Task 6: Publish & share

## Prerequisites
- Azure subscription (create **Azure AI Search** in **Azure portal**: [portal.azure.com](https://portal.azure.com/))  
- HR policy documents or SharePoint site  
- **Copilot Studio** access ([copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/)) and **Power Automate** permissions ([make.powerautomate.com](https://make.powerautomate.com/))

---

## Task 1: Create an Azure AI Search service
1. Go to **Azure portal** ([portal.azure.com](https://portal.azure.com/)) → **Create a resource** → **Azure AI Search**.  
2. Pick **Resource group**, region, tier (Basic+ for semantic).  
3. **Review + create** → **Create**.

**MS Learn reference:** Create a search service  
https://learn.microsoft.com/azure/search/search-create-service-portal

---

## Task 2: Import & index HR content
1. In the **Azure portal** ([portal.azure.com](https://portal.azure.com/)), open your **Search** service → **Import data (new)**.  
2. Select data source: **Blob**, **ADLS Gen2**, or **SharePoint**.  
3. Choose **Keyword** or **Vector** indexing; map fields (**content**, **metadata**, **vector**).  
4. Run import; validate with **Search explorer** in your Search resource.

**MS Learn references:**  
• Import data wizard – https://learn.microsoft.com/azure/search/search-import-data-portal  
• Vector search overview – https://learn.microsoft.com/azure/search/vector-search-overview  
• Search explorer – https://learn.microsoft.com/azure/search/search-explorer

---

## Task 3: Create the HR agent & connect Generative answers
1. In **Copilot Studio** ([copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/)), create **HR Assistant**.  
2. Add a **Generative answers** node (or **Knowledge**).  
3. Connect **Azure OpenAI – use your data** to your Search index (endpoint/index/query settings).  
4. Test sample queries (leave, benefits, holidays).

**MS Learn references:**  
• Use your data with Azure OpenAI – https://learn.microsoft.com/azure/ai-services/openai/use-your-data  
• Copilot Studio knowledge/grounding – https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 4: Add HR tools/flows
1. **Power Automate** ([make.powerautomate.com](https://make.powerautomate.com/)): build **GetLeaveBalance(UserId)** → **Save & Publish**.  
2. Build **GetBenefitSummary(UserId)** → **Publish**.  
3. In **Copilot Studio** ([copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/)) → **Tools → Add a tool → Flow** → attach flows; write helpful **descriptions**.

**MS Learn references:**  
• Power Automate overview – https://learn.microsoft.com/power-automate/overview  
• Add tools/flows to copilots – https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 5: Safety & evaluation
1. Configure **content filtering** in **Azure AI Studio** ([ai.azure.com](https://ai.azure.com/)) or Azure OpenAI settings.  
2. Review **Responsible AI** guidance; refine **Instructions** to enforce policy tone/boundaries.  
3. Validate groundedness and citations with representative prompts.

**MS Learn references:**  
• Content filtering – https://learn.microsoft.com/azure/ai-services/openai/concepts/content-filtering  
• Responsible use – https://learn.microsoft.com/azure/ai-services/openai/concepts/responsible-use

---

## Task 6: Publish & share
1. In **Copilot Studio** ([copilotstudio.microsoft.com](https://copilotstudio.microsoft.com/)), click **Publish**.  
2. Choose **Web app** or **Microsoft Teams** ([teams.microsoft.com](https://teams.microsoft.com/)) and share with HR stakeholders.

**MS Learn references:**  
• Publish/deploy – https://learn.microsoft.com/power-platform/copilot-studio/  
• Deploy to Teams – https://learn.microsoft.com/power-platform/copilot-studio/

## Notes
- Prefer **vector** indexing for RAG-style grounding.  
- Add metadata (e.g., **PolicyCategory**) for better filtering.

<validation step="lab2-validate-hr-assistant" />

## Summary
You built an HR knowledge assistant grounded on your own corpus, extended with HR tools, and readied it for pilot deployment.
