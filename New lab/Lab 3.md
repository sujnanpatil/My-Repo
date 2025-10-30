# Lab 3 - Develop Intelligent chat applications with Azure RAG

### Estimated Duration: 60 Minutes

## Overview
Provision **Azure OpenAI** and an **embeddings** model, create a **vector** index in **Azure AI Search**, implement a **RAG** pipeline (retrieve → generate), add citations, surface a simple client/API, and evaluate quality/safety.

## Objectives
- Task 1: Set up Azure OpenAI & deploy **embeddings**  
- Task 2: Build a **vector** index in Azure AI Search  
- Task 3: Implement the **RAG** pipeline  
- Task 4: Add **citations** and tune retrieval  
- Task 5: Expose an API or minimal web client  
- Task 6: Evaluate & monitor

## Prerequisites
- Azure access to **Azure OpenAI** and **Azure AI Search** in **Azure portal** ([portal.azure.com](https://portal.azure.com/))  
- **Azure AI Studio** access ([ai.azure.com](https://ai.azure.com/))  
- Sample corpus (PDF/Markdown/HTML)  
- Local dev environment (**.NET**, **Node.js**, or **Python**)

---

## Task 1: Set up Azure OpenAI & deploy embeddings
1. In **Azure AI Studio** ([ai.azure.com](https://ai.azure.com/)) or **Azure portal** ([portal.azure.com](https://portal.azure.com/)), create an **Azure OpenAI** resource.  
2. Deploy a **chat** model (generation) and an **embeddings** model (vectorization).  
3. Note **endpoint**, **key**, and deployment names.

**MS Learn references:**  
• Embeddings concepts – https://learn.microsoft.com/azure/ai-services/openai/concepts/understand-embeddings  
• Azure OpenAI quickstart – https://learn.microsoft.com/azure/ai-services/openai/quickstart

---

## Task 2: Build a vector index in Azure AI Search
1. In **Azure portal** ([portal.azure.com](https://portal.azure.com/)), open your **Azure AI Search** service → **Import data (new)**.  
2. Choose a data source; enable **Vector**; select embedding setup if prompted.  
3. Map fields (**content**, **vector**, **id**, **metadata**).  
4. Run import; test with **Search explorer** in your Search resource.

**MS Learn references:**  
• Vector search overview – https://learn.microsoft.com/azure/search/vector-search-overview  
• Create/load an index – https://learn.microsoft.com/azure/search/search-import-data-portal  
• Search explorer – https://learn.microsoft.com/azure/search/search-explorer

---

## Task 3: Implement the RAG pipeline
1. **Embed** the user query with your embeddings deployment (via **Azure OpenAI**).  
2. Query **Azure AI Search** using **vector** (optionally hybrid + semantic).  
3. Build a grounded prompt with top passages; call the **chat** model from **Azure OpenAI**.  
4. Return the answer with source IDs/links.

**MS Learn references:**  
• RAG with Azure AI Search (concepts/quickstarts) – https://learn.microsoft.com/azure/search/vector-search-overview  
• Chat completions quickstart – https://learn.microsoft.com/azure/ai-services/openai/quickstart

---

## Task 4: Add citations & tune retrieval
1. Include titles/links from search hits as **citations** in responses.  
2. Tune **topK**, **hybrid (vector+keyword)**, and **semantic reranking**.  
3. Validate groundedness using a test prompt set.

**MS Learn references:**  
• Semantic/hybrid search – https://learn.microsoft.com/azure/search/search-linguistic-features  
• Indexing/analyzers – https://learn.microsoft.com/azure/search/search-howto-indexing-portal

---

## Task 5: Expose a simple app
**Option A – .NET minimal app**  
1. Start from the **Azure OpenAI .NET quickstart** (code scaffolding).  
2. Replace direct chat call with your RAG steps (embed → search → generate).

**Option B – Node.js web client**  
1. Create an **Express** API that runs the same RAG steps.  
2. Add a minimal HTML/JS client that displays **citations**.

**MS Learn references:**  
• .NET quickstart – https://learn.microsoft.com/azure/ai-services/openai/quickstart?tabs=dotnet  
• Node.js quickstart – https://learn.microsoft.com/azure/ai-services/openai/quickstart?tabs=nodejs

---

## Task 6: Evaluate & monitor
1. Use **Azure AI Studio** ([ai.azure.com](https://ai.azure.com/)) evaluation with RAG evaluators (groundedness, relevance, citation).  
2. Log latency, tokens, and retrieval metrics; iterate prompts and parameters.  
3. Apply **content filters** and **Responsible AI** guidance.

**MS Learn references:**  
• Evaluate in Azure AI Studio – https://learn.microsoft.com/azure/ai-studio/how-to/evaluate  
• Content filtering – https://learn.microsoft.com/azure/ai-services/openai/concepts/content-filtering  
• Responsible use – https://learn.microsoft.com/azure/ai-services/openai/concepts/responsible-use

## Notes
- Start with classic RAG; adopt **agentic retrieval** later if multi-step tool use is required.  
- Use **managed identities**/Key Vault for secrets in hosted deployments.

<validation step="lab3-validate-rag" />

## Summary
You built a production-ready RAG baseline with vector search, grounded generation, citations, a simple surface, and an evaluation loop to keep quality high.
