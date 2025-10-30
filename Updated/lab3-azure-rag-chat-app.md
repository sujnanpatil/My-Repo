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
- Access to **Azure portal** and **Azure AI Studio**  
- Access to **Azure AI Search** and **Azure OpenAI**  
- Local dev environment: **.NET** or **Node.js** (choose one)

## Accounts & Credentials
- **Email/Username:** <inject key="AzureAdUserEmail"></inject>  
- **Password:** <inject key="AzureAdUserPassword"></inject>

## Task 1: Set up Azure OpenAI & deploy embeddings

**Goal:** Provision Azure OpenAI and create two deployments (chat + embeddings).

1. Open **Azure AI Studio**: <https://ai.azure.com/> → **Sign in**.  
2. **Create project** (if prompted):  
   - **Project name:** `rag-lab`  
   - **Hub/Workspace:** create or select defaults  
3. In the left nav, open **Deployments** → **+ Deploy model**.  
   - **Model type:** Chat/completions (select an available chat model)  
   - **Deployment name:** `chat-lab` → **Deploy**  
4. Deploy an **Embeddings** model:  
   - **Model type:** Embeddings (select an available embeddings model)  
   - **Deployment name:** `embeddings-lab` → **Deploy**
5. In **Project settings**, note **Endpoint** and **Key** for use in your app.

> Screenshot: ![](../media/lab3-t1-deployments.png)

## Task 2: Build a vector index in Azure AI Search

**Goal:** Create a search service and a vector-enabled index, then ingest documents.

1. Open **Azure portal**: <https://portal.azure.com/> → **Sign in**.  
2. **Create a resource** → **Azure AI Search** →  
   - **Resource group:** `rg-rag-lab`  
   - **Service name:** `contoso-search-lab`  
   - **Region:** same as OpenAI if possible → **Create**
3. Prepare content in **Azure Blob Storage**:  
   - Create storage `straglab<uniqueid>` → container `docs` → upload:  
     - `Contoso-Guide.pdf`, `Handbook.md`, `FAQ.html`
4. In your **Search** service, choose **Import data (new)** → select **Azure Blob Storage** → connect to `straglab<uniqueid>/docs`.  
5. Configure **Index**:  
   - **Index name:** `idx-contoso-knowledge`  
   - **Fields:** `id` (key), `content` (searchable), `title`, `url`, `category`  
   - If vector flow appears, add a **vector** field and associate the **embeddings** deployment  
6. Configure **Indexer**:  
   - **Name:** `ixr-contoso-knowledge`  
   - **Schedule:** Once → **Submit**  
7. Verify content in **Search explorer** with queries like `"return policy"`.

> Screenshot: ![](../media/lab3-t2-index.png)

## Task 3: Implement the RAG pipeline

**Goal:** Build a minimal app that embeds queries, retrieves documents, and generates grounded answers.

### Option A — .NET Minimal API

1. Open **VS Code** or terminal.  
2. Create a folder:
   ```bash
   mkdir rag-app-dotnet && cd rag-app-dotnet
   dotnet new web -n RagApp
   cd RagApp
   ```
3. Add packages:
   ```bash
   dotnet add package Azure.Search.Documents
   dotnet add package Azure.AI.OpenAI
   ```
4. Create **appsettings.json**:
   ```json
   {
     "OpenAI": {
       "Endpoint": "https://<your-openai-endpoint>.openai.azure.com/",
       "Key": "<your-openai-key>",
       "ChatDeployment": "chat-lab",
       "EmbeddingDeployment": "embeddings-lab"
     },
     "Search": {
       "Endpoint": "https://contoso-search-lab.search.windows.net",
       "Key": "<your-search-admin-key>",
       "IndexName": "idx-contoso-knowledge"
     }
   }
   ```
5. Replace `Program.cs` with the RAG flow (pseudocode outline):
   ```csharp
   // 1) Receive q
   // 2) Create embedding using OpenAI -> vector
   // 3) Search Azure AI Search (vector or hybrid) topK docs
   // 4) Build grounded prompt with snippets + titles/urls
   // 5) Call chat model and return { answer, citations[] }
   ```
6. Run:
   ```bash
   dotnet run
   ```
7. Test: `GET http://localhost:5000/ask?q=What is our return policy?`

### Option B — Node.js Express

1. Open terminal:
   ```bash
   mkdir rag-app-node && cd rag-app-node
   npm init -y
   npm i express @azure/search-documents @azure/openai dotenv
   ```
2. Create `.env`:
   ```
   OPENAI_ENDPOINT=https://<your-openai-endpoint>.openai.azure.com/
   OPENAI_KEY=<your-openai-key>
   CHAT_DEPLOYMENT=chat-lab
   EMBEDDING_DEPLOYMENT=embeddings-lab

   SEARCH_ENDPOINT=https://contoso-search-lab.search.windows.net
   SEARCH_KEY=<your-search-admin-key>
   SEARCH_INDEX=idx-contoso-knowledge
   ```
3. Create `server.js` with this flow (pseudocode):
   ```js
   // 1) Create embedding for user query
   // 2) Vector (or hybrid) search topK docs
   // 3) Build grounded prompt (include snippets + doc titles/urls)
   // 4) Call chat completion
   // 5) Return answer + citations
   ```
4. Start:
   ```bash
   node server.js
   ```
5. Test: `GET http://localhost:3000/ask?q=Where can I find the Contoso handbook?`

> Screenshot: ![](../media/lab3-t3-app.png)

## Task 4: Add citations & tune retrieval

**Goal:** Return source titles/links and tune ranking.

1. Include `title` and `url` from search hits as **citations** in the response.  
2. Tune:
   - **topK:** start with 5–8  
   - **Hybrid search:** combine vector + keyword queries  
   - **Semantic reranking:** enable if available on your tier  
3. Re-run the same prompts and ensure answers are grounded.

> Screenshot: ![](../media/lab3-t4-citations.png)

## Task 5: Expose a simple client

**Goal:** Provide a browser surface.

- **.NET**: Add a minimal Razor page or serve `index.html` with a textbox → calls `/ask?q=...`  
- **Node.js**: Serve a static `index.html` that fetches `/ask?q=...` and renders **citations**.

> Screenshot: ![](../media/lab3-t5-client.png)

## Task 6: Evaluate & monitor

**Goal:** Measure groundedness, relevance, latency, and safety.

1. Open **Azure AI Studio**: <https://ai.azure.com/> → your project → **Evaluation**.  
2. Create an evaluation with **RAG evaluators** (groundedness, relevance, citation).  
3. Log latency, tokens, and search metrics in your app; iterate **topK**, prompts, and filters.  
4. Apply **content filters** and review **Responsible AI** guidance.

> Screenshot: ![](../media/lab3-t6-eval.png)

## Notes
- Start with classic RAG; move to **agentic retrieval** if you need multi-step tool use.  
- Use **Managed Identity** or **Key Vault** for secrets when deploying.

<validation step="lab3-validate-rag" />

## Summary
You built a production-ready RAG baseline with Azure OpenAI + Azure AI Search, exposed it via a simple app surface, added citations, and set up an evaluation loop for quality and safety.
