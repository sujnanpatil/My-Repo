# Lab 1 - Build and enhance a Safe Travels agent and implement Multi agent orchestration

### Estimated Duration: 60 Minutes

## Overview
Create a **Copilot Studio** agent for employee travel (“Safe Travels”), ground it with policy/FAQ knowledge, plug in **Power Automate** flows for itinerary lookup and approvals, enable **generative orchestration**, test, and publish to a channel.

## Objectives
- Task 1: Create a Copilot Studio agent and set purpose/instructions  
- Task 2: Add **Generative answers** and connect knowledge sources  
- Task 3: Add tools via **agent flows** (Trip Lookup / Approvals)  
- Task 4: Enable **generative orchestration** and (optionally) multi-agent  
- Task 5: Test & refine with the simulator  
- Task 6: Publish to Web or Teams

## Prerequisites
- Microsoft 365 tenant with access to **Microsoft Copilot Studio**  
- Permissions to create **Power Automate** flows  
- Sample policy/FAQ files (PDF/DOCX/HTML) or public URLs

## Accounts & Credentials
- **Email/Username:** <inject key="AzureAdUserEmail"></inject>  
- **Password:** <inject key="AzureAdUserPassword"></inject>

## Task 1: Create your Copilot Studio agent

**Goal:** Create an agent named **Safe Travels** and define its instructions.

1. Open **Copilot Studio**: <https://copilotstudio.microsoft.com/>  
2. Click **Sign in** (top-right).  
   - **Email/Username:** `<inject key="AzureAdUserEmail"></inject>` → **Next**  
   - **Password:** `<inject key="AzureAdUserPassword"></inject>` → **Sign in**  
   - If prompted **Stay signed in?** choose **No, this app only**.
3. On the landing page, select **Create** → **Agent**.  
4. Fill the form and click **Create**.  
   - **Agent name:** `Safe Travels`  
   - **Description:** `Helps employees with travel policy questions, itinerary lookups, approvals, and safety guidance.`  
   - **Primary language:** English (United States)
5. Go to **Agent → Instructions** and paste:
   ```
   You are Safe Travels, a travel help agent for Contoso.
   • Answer travel policy questions using connected knowledge only.
   • When a user asks about itineraries or approvals, call the appropriate tool.
   • Be concise, friendly, and reference authoritative sources when available.
   • If unsure, ask a clarifying question before acting.
   ```
6. Click **Save**.

> Screenshot: ![](../media/lab1-t1-create-agent.png)

## Task 2: Add Generative answers & connect knowledge

**Goal:** Ground the agent on real policy/FAQ content.

1. In the left nav, select **Knowledge** (or add a **Generative answers** node inside a topic).  
2. Click **+ Add data** and choose one of:  
   - **Website** → URL: `https://www.contoso.com/travel-policy` *(dummy)*  
   - **Files** → Upload `TravelPolicy.pdf`, `FAQ-Travel.pdf`  
   - **SharePoint** → Select your HR/Travel site  
3. For each source, set a **Description** (e.g., `Official Contoso travel policy and FAQ`).  
4. Click **Save**.  
5. Test a prompt in the **Test** panel (right):  
   - `What is the daily hotel limit in India for Level-2 employees?`

> Screenshot: ![](../media/lab1-t2-knowledge.png)

## Task 3: Add tools via agent flows (Trip Lookup / Approvals)

**Goal:** Add two flows and register them as tools for the agent.

### A. Build flows in Power Automate
1. Open **Power Automate**: <https://make.powerautomate.com/> → **Sign in**.  
2. Confirm **Environment** (top-right) → choose your default or `Contoso (default)`.
3. Click **Create** → **Instant cloud flow**.  
   - **Flow name:** `GetTripByEmployee`  
   - **Trigger:** `Manually trigger a flow` → **Create**
4. Add **Inputs** to the trigger:  
   - Text **EmployeeId** (default `EMP12345`)
5. Add **Compose** action labeled **FakeTripData** (lab dummy output) with:
   ```json
   {
     "EmployeeId": "@{triggerBody()['text']}",
     "Itinerary": "TRIP-001: BLR → DEL → BLR, 2 nights",
     "Status": "Booked",
     "Hotel": "Contoso Residency"
   }
   ```
6. Add **Response** (HTTP) action:  
   - **Status code:** `200`  
   - **Body:** outputs of **FakeTripData**
7. Click **Save** → **Test** → **Manually** → **Run flow** (EmployeeId `EMP12345`) → verify output.

8. Create a second flow **RequestTravelApproval**:  
   - **Flow name:** `RequestTravelApproval`  
   - **Trigger:** `Manually trigger a flow` with text **TripId** (default `TRIP-001`) and number **Amount** (default `1200`)  
   - **Compose** →  
     ```json
     {
       "TripId": "@{triggerBody()['text']}",
       "Amount": 1200,
       "ApprovalResult": "Approved",
       "Approver": "manager@contoso.com"
     }
     ```  
   - **Response** → return the compose output → **Save** & **Test**.

> Screenshot: ![](../media/lab1-t3-flows.png)

### B. Register flows as tools in Copilot Studio
1. Return to **Copilot Studio** (<https://copilotstudio.microsoft.com/>) → open **Safe Travels**.  
2. Go to **Tools** → **Add a tool** → **Flow**.  
3. Select `GetTripByEmployee` → **Add**.  
   - **Tool description:** `Looks up itinerary details by EmployeeId.`
4. Add `RequestTravelApproval` with description: `Submits a travel for approval with amount.`  
5. Click **Save**.

> Screenshot: ![](../media/lab1-t3-tools.png)

## Task 4: Enable generative orchestration (and multi-agent)

1. In **Copilot Studio** → **Agent → Settings → Generative AI**.  
2. Toggle **Use generative orchestration** → **On**.  
3. Ensure each **Tool** and **Knowledge** item has a clear **Description**.  
4. *(Optional)* Add a helper agent (e.g., `Expense Helper`) and reference it in **Instructions**:
   ```
   For expense queries, ask the Expense Helper agent to assist.
   ```

> Screenshot: ![](../media/lab1-t4-orchestration.png)

## Task 5: Test & refine

1. Click **Test** (top-right) to open the simulator.  
2. Try: `Show my itinerary for EMP12345 and submit for approval of 1200 USD.`  
3. Open the **trace/activity** view and verify:  
   - Knowledge used for policy Q&A  
   - Tool `GetTripByEmployee` called → tool `RequestTravelApproval` called  
4. Adjust **Instructions** or tool **Descriptions** if routing isn’t ideal.

> Screenshot: ![](../media/lab1-t5-test.png)

## Task 6: Publish

1. Click **Publish** (top-right).  
2. Choose **Web app** or **Microsoft Teams**:  
   - **Teams:** <https://teams.microsoft.com/> → publish to a team/channel  
   - **Web app:** copy the web link and share it with pilot users
3. Validate with a colleague account.

> Screenshot: ![](../media/lab1-t6-publish.png)

## Notes
- Tight, goal-driven **descriptions** improve orchestration results.  
- Keep **Instructions** compliant and specific to travel.

<validation step="lab1-validate-safe-travels" />

## Summary
You built and published a grounded travel agent that can answer policy questions and perform actions via flows—automatically choosing between knowledge and tools with generative orchestration.
