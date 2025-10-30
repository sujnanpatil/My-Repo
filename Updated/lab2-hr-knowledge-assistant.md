# Lab 2 - Create a Knowledge Assistant agent for HR in Copilot Studio that leverages Azure AI Search

### Estimated Duration: 60 Minutes

## Overview
Provision **Azure AI Search**, index HR policies/FAQs, connect it to a **Copilot Studio** agent using **Generative answers**, add HR tools, apply safety/evaluation, and publish to a pilot channel.

## Objectives
- Task 1: Provision **Azure AI Search**  
- Task 2: Import and index HR content (keyword or vector)  
- Task 3: Create HR agent & connect **Generative answers** to the index  
- Task 4: Add HR tools/flows (Leave Balance, Benefits Summary)  
- Task 5: Apply safety & evaluation practices  
- Task 6: Publish & share

## Prerequisites
- Azure subscription access to **Azure portal** and **Azure AI Studio**  
- **Copilot Studio** and **Power Automate** access  
- HR policy documents (PDF/DOCX/HTML) or SharePoint site

## Accounts & Credentials
- **Email/Username:** <inject key="AzureAdUserEmail"></inject>  
- **Password:** <inject key="AzureAdUserPassword"></inject>

---

## Task 1: Create an Azure AI Search service

**Goal:** Create a search service for HR content.

1. Open **Azure portal**: <https://portal.azure.com/> → **Sign in**.  
2. Click **Create a resource** → search **Azure AI Search** → **Create**.  
3. Fill deployment details:  
   - **Subscription/Resource group:** `rg-hr-search-lab` *(create new if needed)*  
   - **Service name:** `hr-search-<uniqueid>` (e.g., `hr-search-2391`)  
   - **Region:** closest to you  
   - **Pricing tier:** `Basic` (or higher for semantic)  
4. **Review + create** → **Create** → wait for deployment.

> Screenshot placeholder: ![](../media/lab2-t1-create-search.png)

---

## Task 2: Import & index HR content

**Goal:** Build a vector-ready index and ingest documents.

1. In the **Azure portal**, open your **hr-search-<uniqueid>** service.  
2. Select **Import data (new)**.  
3. **Data source options:**  
   - **Azure Blob Storage** (preferred for lab):  
     1. Open a new tab → **Create a storage account** `sthrlab<uniqueid>`  
     2. **Containers** → `hr` → **Upload** files:  
        - `HR-Leave-Policy.pdf`  
        - `HR-Benefits-FAQ.pdf`  
        - `Holiday-Calendar-2025.pdf`  
     3. Return to Search → choose **Azure Blob Storage** → connect to `sthrlab<uniqueid>/hr`.  
   - **SharePoint**: choose SharePoint connection and authenticate.
4. **Index settings:**  
   - **Index name:** `idx-hr-policies`  
   - **Fields:** ensure `id` (key), `content` (searchable), `category` (filterable).  
   - If vector flow available, add a **vector** field (associate embeddings in wizard if prompted).  
5. **Indexer:**  
   - **Name:** `ixr-hr-policies`  
   - **Schedule:** `Once` → **Submit**  
6. Validate with **Search explorer** (left nav) → query `"leave balance"` → confirm results.

> Screenshot placeholder: ![](../media/lab2-t2-index.png)

---

## Task 3: Create the HR agent & connect Generative answers

**Goal:** Connect the search index to Copilot Studio for grounded answers.

1. Open **Copilot Studio**: <https://copilotstudio.microsoft.com/> → **Sign in**.  
2. Click **Create → Agent**.  
   - **Agent name:** `HR Assistant`  
   - **Description:** `Answers HR policy questions using authoritative HR content and can call HR tools.`  
   - **Create**.
3. Go to **Knowledge** (or add a **Generative answers** node in a topic) → **+ Add data** → choose:  
   - **Azure OpenAI on your data** (if available) and connect to your Search index, or  
   - **Website/Files/SharePoint** with the same HR documents
4. Test prompts in **Test** panel:  
   - `What is the maternity leave policy?`  
   - `Where is the benefits handbook?`

> Screenshot placeholder: ![](../media/lab2-t3-agent-knowledge.png)

---

## Task 4: Add HR tools/flows

**Goal:** Allow the agent to fetch live HR data.

### A. Flows in Power Automate
1. Open **Power Automate**: <https://make.powerautomate.com/> → **Sign in**.  
2. **Create → Instant cloud flow**  
   - **Name:** `GetLeaveBalance`  
   - **Trigger:** `Manually trigger a flow` with text input **UserId** (default `EMP4488`)  
   - Add **Compose** with:
     ```json
     {
       "UserId": "@{triggerBody()['text']}",
       "AnnualLeaveBalance": "12",
       "SickLeaveBalance": "5",
       "LastUpdated": "2025-10-30"
     }
     ```
   - Add **Response** (200) → **Body:** outputs of compose → **Save** → **Test**.
3. Create **GetBenefitSummary** similarly with:
   ```json
   {
     "UserId": "@{triggerBody()['text']}",
     "Plan": "Contoso Health Plus",
     "Coverage": "Employee + Family",
     "Contact": "benefits@contoso.com"
   }
   ```
   **Save** & **Test**.

### B. Register tools in Copilot Studio
1. In **Copilot Studio** → **HR Assistant** → **Tools → Add a tool → Flow**.  
2. Select `GetLeaveBalance` → **Add**.  
   - **Description:** `Returns current leave balances for a user.`
3. Add `GetBenefitSummary` → **Description:** `Summarizes the user’s benefit plan.`  
4. **Save**.

> Screenshot placeholder: ![](../media/lab2-t4-tools.png)

---

## Task 5: Safety & evaluation

**Goal:** Apply content filters and validate response quality.

1. Open **Azure AI Studio**: <https://ai.azure.com/> → **Sign in**.  
2. Open your Azure OpenAI project/resource → **Safety** (content filters).  
3. Set default filters (Hate/Violence/Sexual etc.) to **recommended** thresholds.  
4. Back in **Copilot Studio**, refine **Agent → Instructions** to enforce safe, HR-appropriate tone.  
5. Validate with test prompts (policy lookups, edge cases) and ensure responses cite the correct HR docs.

> Screenshot placeholder: ![](../media/lab2-t5-safety.png)

---

## Task 6: Publish & share

1. In **Copilot Studio**, click **Publish** (top-right).  
2. Choose **Web app** or **Microsoft Teams**: <https://teams.microsoft.com/>  
3. Share with HR stakeholders and capture feedback.

> Screenshot placeholder: ![](../media/lab2-t6-publish.png)

---

## Notes
- Prefer **vector** indexing for RAG-style grounding.  
- Add metadata (e.g., `PolicyCategory`) for better filtering.

<validation step="lab2-validate-hr-assistant" />

## Summary
Your HR assistant now answers grounded questions from your indexed HR corpus and can call HR tools for live data.
