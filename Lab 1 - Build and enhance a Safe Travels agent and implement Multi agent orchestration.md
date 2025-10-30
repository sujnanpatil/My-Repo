<!-- File: lab1-safe-travels-agent.md -->

# Lab 1 - Build and enhance a Safe Travels agent and implement Multi agent orchestration

### Estimated Duration: 60 Minutes

## Overview
Create a Copilot Studio agent for employee travel (“Safe Travels”), ground it with travel policies/FAQs, plug in flows for itinerary lookup and approvals, enable **generative orchestration**, test, and publish.

## Objectives
- Task 1: Create a Copilot Studio agent and set purpose/instructions  
- Task 2: Add **Generative answers** and connect knowledge sources  
- Task 3: Add tools via **agent flows** (Trip Lookup / Approvals)  
- Task 4: Enable **generative orchestration** and (optionally) multi-agent  
- Task 5: Test & refine with the simulator  
- Task 6: Publish to Web or Teams

## Prerequisites
- Access to **Microsoft Copilot Studio**  
- Sample travel policy/FAQ content (PDF/URL/SharePoint)  
- Permissions to build **Power Automate** flows

---

## Task 1: Create your Copilot Studio agent
1. Open **Microsoft Copilot Studio** → **Create → Agent** → name it **Safe Travels**.  
2. In **Instructions**, define scope, tone, boundaries (e.g., *trip queries, approvals, safety guidance*).  
3. Save.

**MS Learn reference:** Copilot Studio – Create & manage agents  
https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 2: Add Generative answers & connect knowledge
1. In the agent, open **Knowledge** (or add a **Generative answers** node).  
2. Add sources: **Website**, **Files**, **SharePoint**, or **Azure OpenAI on your data** (with Azure AI Search).  
3. Provide clear **descriptions** for each source; Save and ask test questions.

**MS Learn references:**  
• Knowledge/Generative answers basics – https://learn.microsoft.com/power-platform/copilot-studio/  
• Use your data with Azure OpenAI – https://learn.microsoft.com/azure/ai-services/openai/use-your-data

---

## Task 3: Add tools via agent flows (Trip Lookup / Approvals)
1. In **Power Automate**, create **GetTripByEmployee** (input: EmployeeId; output: Itinerary/Status) → **Save & Publish**.  
2. Create **RequestTravelApproval** (inputs: TripId, Amount; output: ApprovalResult) → publish.  
3. Back in Copilot Studio → **Tools → Add a tool → Flow** → attach both flows.  
4. Write concise **tool descriptions** (improves orchestration).

**MS Learn references:**  
• Power Automate overview – https://learn.microsoft.com/power-automate/overview  
• Add tools/flows to copilots – https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 4: Enable generative orchestration (and multi-agent)
1. **Agent → Settings → Generative AI** → turn **Use generative orchestration** **On**.  
2. Ensure **Instructions** and **descriptions** are crisp and goal-oriented.  
3. *(Optional)* Register a helper agent (e.g., “Expense Helper”) and reference it.

**MS Learn reference:** Generative orchestration – concepts & setup  
https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 5: Test & refine
1. Open **Test** to launch the simulator.  
2. Try compound prompts (e.g., “*Show my Delhi trip and submit for approval*”).  
3. Review the **trace/activity** to see tool calls & knowledge grounding.  
4. Iterate **Instructions**, **tool descriptions**, or **knowledge scope**.

**MS Learn reference:** Create/test agents  
https://learn.microsoft.com/power-platform/copilot-studio/

---

## Task 6: Publish
1. Click **Publish** → choose **Web app** or **Microsoft Teams**.  
2. Share the link with pilot users.

**MS Learn references:**  
• Publish/deploy – https://learn.microsoft.com/power-platform/copilot-studio/  
• Deploy to Teams – https://learn.microsoft.com/power-platform/copilot-studio/

## Notes
- Clear tool/knowledge **descriptions** dramatically improve orchestration outcomes.  
- Keep Instructions short, compliance-aligned, and action-oriented.

<validation step="lab1-validate-safe-travels" />

## Summary
You built and published a grounded travel assistant that can also perform actions via flows, with orchestration automatically choosing knowledge vs. tools.
