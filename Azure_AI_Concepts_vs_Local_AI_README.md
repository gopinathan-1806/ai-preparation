# Azure AI Concepts — Compared with a Local AI / LLM Setup

> **Purpose:** A practical learning reference to connect the local GenAI concepts I already learned — LLMs, RAG, FAISS, BM25, LangChain, guardrails, APIs, monitoring — with the equivalent services and architecture in Azure.

---

## 1. The Big Picture

The easiest way to understand Azure AI is **not to learn every Azure service independently**.

Instead, map the components you already know from a local GenAI application to their managed Azure equivalents.

### Local AI application

```text
User
  ↓
Streamlit / FastAPI
  ↓
Input Guardrails
  ↓
Application Logic
  ↓
FAISS / BM25
  ↓
RAG Context
  ↓
Local LLM / OpenAI API
  ↓
Output Guardrails
  ↓
Response
```

### Azure enterprise AI application

```text
User
  ↓
Microsoft Entra ID
  ↓
Azure Front Door / WAF
  ↓
Azure API Management
  ↓
AI Application / Agent
  ↓
Authorization + Rate Limits + Guardrails
  ↓
Azure AI Search
  ↓
Azure OpenAI / Foundry Model
  ↓
Output Guardrails
  ↓
Azure Monitor / Application Insights / Foundry Observability
  ↓
Response
```

The important difference is that Azure provides **managed enterprise capabilities** around the LLM and RAG pipeline: identity, networking, authorization, quotas, security, observability, governance and managed data services.

---

# 2. Local AI vs Azure AI — Quick Mapping

| What I learned locally | Azure equivalent | What it does |
|---|---|---|
| Ollama | Azure OpenAI / Foundry Models | Runs/serves foundation models |
| Local LLM | Azure-hosted foundation model | Generates responses |
| OpenAI API | Azure OpenAI / Foundry model endpoint | Model inference |
| FAISS | Azure AI Search | Vector retrieval |
| BM25 | Azure AI Search full-text search | Keyword retrieval |
| Hybrid BM25 + Vector | Azure AI Search Hybrid Search | Combines keyword + vector retrieval |
| Reranker / FlashRank | Azure AI Search semantic ranking | Improves relevance |
| LangChain | Application/orchestration layer | Connects models, retrieval and tools |
| FastAPI | Azure App Service / Container Apps / AKS | Hosts application/API |
| Streamlit | App Service / Container Apps / Frontend service | Hosts UI |
| `.env` secrets | Azure Key Vault | Secure secret storage |
| Local user login | Microsoft Entra ID | Enterprise authentication |
| Python authorization logic | Entra ID + application authorization + Search security trimming | Controls data access |
| Custom rate-limit code | Azure API Management | API throttling, quotas and policies |
| Input/output safety code | Azure AI Content Safety / Prompt Shields | AI safety and guardrails |
| Local PDF/text parsing | Azure AI Document Intelligence | Extracts structured content from documents |
| Local files | Azure Blob Storage / ADLS | Enterprise document storage |
| Neo4j | Azure Cosmos DB for Apache Gremlin | Managed graph database |
| Local logs | Azure Monitor / Log Analytics | Centralized monitoring |
| Python logging | Application Insights | Application telemetry |
| LangSmith | Foundry observability / tracing + Azure Monitor ecosystem | AI/application tracing |
| pytest / custom evaluation | Foundry evaluation capabilities | Model/agent evaluation |
| Docker | Azure Container Apps / AKS / App Service | Containerized deployment |
| Local network | Azure VNet / Private Endpoint | Private enterprise networking |

> **Important:** These are conceptual mappings, not always 1:1 replacements. For example, Azure AI Search is broader than FAISS because it provides vector, full-text, semantic and hybrid search plus enterprise capabilities.

---

# 3. Foundation Models

## Local setup

I can run a model locally using Ollama:

```text
Application
    ↓
Ollama
    ↓
Local LLM
```

Advantages:

- Runs locally
- Data can remain on the machine
- Easy experimentation
- No per-request cloud inference cost
- Useful for learning

Challenges:

- GPU/CPU/RAM management
- Model downloads and upgrades
- Scaling
- Availability
- Security
- Monitoring
- Production operations

## Azure

Azure provides managed access to foundation models through Microsoft Foundry and Azure OpenAI/model deployments.

```text
Application
    ↓
Azure OpenAI / Foundry Model
    ↓
Managed model inference
```

Azure handles much of the infrastructure around model serving.

### Mental model

```text
Ollama + Local LLM
        ↓
Managed cloud model endpoint
        ↓
Azure OpenAI / Foundry
```

---

# 4. RAG

The RAG concept itself does **not change**.

## Local RAG

My local architecture:

```text
Documents
    ↓
Document Loader
    ↓
Text Splitter
    ↓
Embeddings
    ↓
FAISS
    +
BM25
    ↓
Retriever
    ↓
Reranker
    ↓
Relevant Context
    ↓
LLM
```

## Azure RAG

```text
Documents
    ↓
Blob / ADLS / SharePoint / Other Sources
    ↓
Document Processing
    ↓
Chunking + Enrichment
    ↓
Embeddings
    ↓
Azure AI Search
    ↓
Vector + Keyword + Semantic/Hybrid Retrieval
    ↓
Relevant Context
    ↓
Azure OpenAI / Foundry Model
```

The **RAG principle remains the same**.

Azure mainly replaces the infrastructure I previously built manually.

---

# 5. Azure AI Search vs FAISS + BM25

This is one of the most important mappings to remember.

### Local

```text
                    ┌── FAISS
Documents → Chunks ─┤
                    └── BM25
                         ↓
                      Reranker
```

### Azure

```text
                    ┌── Vector Search
Documents → Index ──┼── Full Text Search
                    ├── Semantic Search
                    └── Hybrid Search
                         ↓
                   Semantic Ranking
```

Azure AI Search can therefore act as the **retrieval layer** for an enterprise RAG system.

It is more than a simple vector database.

---

# 6. Knowledge Graph

I previously learned:

```text
Documents
    ↓
Entity Extraction
    ↓
Relationship Extraction
    ↓
Neo4j
    ↓
Cypher Query
```

Azure can provide managed graph capabilities using **Azure Cosmos DB for Apache Gremlin**.

Conceptually:

```text
Documents
    ↓
Entity Extraction
    ↓
Relationship Extraction
    ↓
Cosmos DB / Gremlin
    ↓
Graph Traversal
```

This can be combined with Azure AI Search for GraphRAG-style applications.

```text
User Question
      ↓
 ┌────┴─────┐
 ↓          ↓
AI Search  Knowledge Graph
 ↓          ↓
 └────┬─────┘
      ↓
  LLM / Agent
```

---

# 7. API Layer

## Local

I can build:

```text
User
 ↓
FastAPI
 ↓
Python application
```

I manually implement:

- Authentication
- Validation
- Rate limiting
- Logging
- Error handling
- API security

## Azure

A common enterprise pattern is:

```text
User
 ↓
Front Door / WAF
 ↓
API Management
 ↓
AI Application
```

### API Management can provide

- API authentication/validation
- Rate limiting
- Quotas
- Policies
- API versioning
- Usage controls
- Secure API exposure

This is especially important when many users are calling an AI application.

---

# 8. Authentication vs Authorization

These are different concepts.

### Authentication

> Who are you?

Azure service:

**Microsoft Entra ID**

```text
User
 ↓
Entra ID
 ↓
Identity token
 ↓
Application
```

### Authorization

> What are you allowed to access?

For example:

```text
User A
 ↓
Can access HR documents

User B
 ↓
Can access Engineering documents
```

The application and data/retrieval layer must enforce this.

**Do not rely on the LLM to decide whether a user is allowed to see a document.**

---

# 9. Guardrails

I previously built guardrails in application code.

Example:

```text
User Input
 ↓
Check input
 ↓
RAG
 ↓
LLM
 ↓
Check output
 ↓
Response
```

Azure provides managed capabilities around AI safety.

Important concepts:

- Azure AI Content Safety
- Prompt Shields
- Content filtering
- Groundedness checks/evaluation
- Application-level validation

A stronger enterprise pattern is:

```text
User
 ↓
Input Validation
 ↓
Prompt Injection Protection
 ↓
Authorization
 ↓
RAG
 ↓
LLM
 ↓
Output Safety
 ↓
Response
```

---

# 10. Rate Limiting and Token Control

This is an important enterprise concept.

Locally:

```text
User
 ↓
Python
 ↓
if requests > limit:
      reject
```

Azure:

```text
User
 ↓
API Management
 ↓
Rate Limit / Quota
 ↓
AI Application
 ↓
Model
```

There are two different things to understand:

### API/user quota

How many requests a user/application can make.

### Model capacity

How much model traffic a deployment can handle.

Model deployments also have service quotas such as tokens-per-minute/request limits depending on the model and Azure configuration.

---

# 11. Secrets Management

## Local

I may use:

```text
.env

OPENAI_API_KEY=xxxx
GITHUB_TOKEN=xxxx
```

This is acceptable for local development if handled properly, but secrets should not be committed to Git.

## Azure

Use:

**Azure Key Vault**

```text
Application
    ↓
Managed Identity
    ↓
Key Vault
    ↓
Secret
```

This removes the need to hard-code production credentials.

---

# 12. Document Intelligence

For simple text files, my local pipeline can use Python libraries.

But enterprise documents can be complex:

```text
PDF
Scanned PDF
Tables
Forms
Invoices
Contracts
Images
```

Azure Document Intelligence can extract structured information.

Example:

```text
PDF
 ↓
Document Intelligence
 ↓
Text + Tables + Fields + Structure
 ↓
Chunking
 ↓
Azure AI Search
 ↓
RAG
```

This is particularly useful for enterprise document-heavy RAG.

---

# 13. Storage

## Local

```text
data/
├── documents/
├── embeddings/
└── vector_db/
```

## Azure

Typical architecture:

```text
Blob Storage / ADLS
        ↓
Document Processing
        ↓
Azure AI Search
```

Blob Storage is commonly used as an enterprise landing/storage layer.

---

# 14. Monitoring

This is where enterprise Azure becomes much more powerful than a local prototype.

## Local

I might use:

```text
Python logging
Prometheus
Grafana
LangSmith
```

## Azure

Common components include:

```text
Application
    ↓
Application Insights
    ↓
Azure Monitor
    ↓
Log Analytics
```

For AI workloads, monitor:

- Request count
- Latency
- Model latency
- Token usage
- Errors
- Search latency
- Dependency failures
- Agent traces
- Evaluation metrics
- Groundedness/relevance
- Cost/usage

---

# 15. Microsoft Foundry

Think of Foundry as the **AI development and application platform**, rather than just another LLM API.

Important areas to learn:

```text
Microsoft Foundry
│
├── Models
├── Agents
├── Tools
├── RAG / Knowledge
├── Evaluation
├── Safety
├── Tracing / Observability
└── Application lifecycle
```

For your learning path, focus particularly on:

1. Models
2. Agents
3. Tool calling
4. RAG
5. Evaluation
6. Guardrails
7. Tracing
8. MCP
9. Agent workflows

---

# 16. Agents

Local:

```text
LangChain
    ↓
Agent
    ↓
Tools
```

You can also use CrewAI or other agent frameworks.

Azure/Foundry:

```text
Foundry Agent
      ↓
 ┌────┼───────────┐
 ↓    ↓           ↓
Search API   Database   Custom Tool
      ↓
    Model
```

The important concept is:

> An agent is not simply an LLM. It is an LLM connected to instructions, tools, data, memory/state and an execution loop.

---

# 17. Evaluation

A common mistake is:

> "The answer looks good, so the application works."

Production AI systems need evaluation.

Evaluate:

- Answer relevance
- Groundedness
- Retrieval quality
- Correctness
- Safety
- Hallucination
- Tool usage
- Agent behavior

Local:

```text
pytest
+ custom evaluation dataset
+ LangSmith
```

Azure:

```text
Foundry Evaluation
+ tracing
+ Azure Monitor
```

---

# 18. Enterprise Security Layers

For an enterprise application, think in layers.

```text
Internet
   ↓
Front Door / WAF
   ↓
API Management
   ↓
Entra ID
   ↓
Application
   ↓
Authorization
   ↓
Guardrails
   ↓
AI Search
   ↓
Model
```

Behind the application, use:

```text
Key Vault
Blob Storage
Cosmos DB
Azure AI Search
Private Endpoints
VNet
Azure Monitor
```

The important concept is **defense in depth**.

---

# 19. The Complete Azure Enterprise RAG Flow

This is the flow I would remember for interviews.

```text
                         ┌──────────────────────┐
                         │       User           │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Entra ID             │
                         │ Authentication       │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Front Door / WAF     │
                         │ Network Protection   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ API Management       │
                         │ Rate Limit / Quota   │
                         │ API Policies         │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ AI Application       │
                         │ / Agent              │
                         └──────────┬───────────┘
                                    │
                     ┌──────────────┴──────────────┐
                     ▼                             ▼
          ┌────────────────────┐        ┌──────────────────┐
          │ Input Guardrails   │        │ Authorization    │
          │ Prompt Shields     │        │ User permissions │
          └──────────┬─────────┘        └────────┬─────────┘
                     │                           │
                     └──────────────┬────────────┘
                                    ▼
                         ┌──────────────────────┐
                         │ Azure AI Search      │
                         │ Vector + Keyword +   │
                         │ Semantic / Hybrid    │
                         └──────────┬───────────┘
                                    │
                              Relevant Context
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Azure OpenAI /       │
                         │ Foundry Model        │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Output Guardrails    │
                         │ Safety / Grounding   │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ User Response        │
                         └──────────────────────┘


       ┌───────────────────────────────────────────────────┐
       │              Cross-cutting Services               │
       │                                                   │
       │  Key Vault     Azure Monitor     App Insights    │
       │  Log Analytics  Foundry Tracing  Evaluation      │
       │  Private Link   VNet             Purview          │
       └───────────────────────────────────────────────────┘
```

---

# 20. Where the Data Comes From

A production RAG application may look like this:

```text
SharePoint
Git
Blob Storage
SQL
PDF
Word
Web
        │
        ▼
┌───────────────────────┐
│ Ingestion Pipeline    │
│                       │
│ Parsing               │
│ Document Intelligence │
│ Chunking              │
│ Metadata              │
│ Embeddings            │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Azure AI Search       │
│                       │
│ Vector Index          │
│ Full Text Index       │
│ Semantic Search       │
│ Security Filters      │
└──────────┬────────────┘
           │
           ▼
      RAG / Agent
           │
           ▼
     Azure Model
```

---

# 21. Local → Azure Mental Model

This is the cheat sheet I want to remember.

```text
LOCAL                          AZURE
────────────────────────────────────────────────

Ollama                  →      Azure OpenAI / Foundry Models

FAISS                   →      Azure AI Search
BM25                    →      Azure AI Search
FlashRank               →      Semantic Ranking

LangChain               →      Application / Agent Layer
CrewAI                  →      Foundry Agents / Agent Framework

FastAPI                 →      App Service / Container Apps / AKS

.env                    →      Key Vault

Custom Login            →      Entra ID

Custom Rate Limiter     →      API Management

Python Guardrails       →      Content Safety / Prompt Shields
                             + application guardrails

Local Files             →      Blob Storage / ADLS

PDF Parser              →      Document Intelligence

Neo4j                   →      Cosmos DB / Gremlin

Prometheus/Grafana      →      Azure Monitor

Python Logs             →      Application Insights / Log Analytics

LangSmith               →      Foundry tracing + evaluation ecosystem

Docker                  →      Container Apps / AKS / App Service

Local Network           →      VNet / Private Endpoints

pytest / eval scripts   →      Foundry Evaluation
```

---

# 22. What I Should Learn First

I do **not** need to learn all Azure services deeply.

### Phase 1 — Core AI

```text
Azure OpenAI
      ↓
Microsoft Foundry
      ↓
Azure AI Search
      ↓
RAG
      ↓
Agents
```

### Phase 2 — Enterprise Security

```text
Entra ID
      ↓
API Management
      ↓
Content Safety
      ↓
Key Vault
      ↓
Private Endpoints
```

### Phase 3 — Production

```text
Azure Monitor
      ↓
Application Insights
      ↓
Foundry Tracing
      ↓
Evaluation
      ↓
Cost / Token Monitoring
```

### Phase 4 — Advanced AI

```text
Document Intelligence
      ↓
Cosmos DB / Knowledge Graph
      ↓
GraphRAG
      ↓
MCP
      ↓
Multi-Agent Systems
```

### Phase 5 — Governance

```text
Purview
Data Governance
Compliance
Responsible AI
Enterprise Policies
```

---

# 23. The Most Important Architecture to Memorize

If I am asked in an interview:

> "How would you build a secure enterprise GenAI application on Azure?"

I should be able to explain:

```text
User
 ↓
Entra ID
 ↓
Front Door / WAF
 ↓
API Management
 ↓
AI Application / Agent
 ↓
Authentication + Authorization
 ↓
Input Guardrails
 ↓
Azure AI Search
 ↓
Authorized RAG Context
 ↓
Azure OpenAI / Foundry Model
 ↓
Output Guardrails
 ↓
Response
```

And across the whole system:

```text
Key Vault
Private Networking
Azure Monitor
Application Insights
Foundry Tracing
Evaluation
Governance
```

That is the core **Azure Enterprise AI architecture** I should understand before going deep into individual services.

---

## 24. Final Mental Model

The simplest way to connect everything I've learned:

```text
                LOCAL GENAI
                     │
                     │  Same AI concepts
                     ▼
              AZURE ENTERPRISE AI
                     │
        ┌────────────┼────────────┐
        │            │            │
      Model         RAG        Agents
        │            │            │
 Azure OpenAI   AI Search     Foundry
        │            │            │
        └────────────┼────────────┘
                     │
              Enterprise Layer
                     │
       ┌─────────────┼─────────────┐
       │             │             │
    Entra ID    API Management   Guardrails
       │             │             │
       └─────────────┼─────────────┘
                     │
                Production
                     │
       ┌─────────────┼─────────────┐
       │             │             │
    Monitor       Security      Governance
       │             │             │
       └─────────────┼─────────────┘
                     │
              Enterprise AI
```

> **Key takeaway:** Azure does not change the fundamental GenAI concepts you already learned. It gives you managed, secure, scalable enterprise building blocks around them.

---

## Recommended Learning Order

**Ollama / Local LLM → RAG → FAISS/BM25 → LangChain → Azure OpenAI → Azure AI Search → Foundry → Agents → Entra ID → API Management → Guardrails → Key Vault → Monitoring → Document Intelligence → GraphRAG → Governance**

This sequence lets me continuously map each new Azure concept to something I already understand instead of learning Azure as a completely separate ecosystem.

## 25. Visual Architecture Diagram

The diagram below gives a visual comparison of the local AI/LLM setup and the Azure enterprise architecture, including the end-to-end request flow and the cross-cutting monitoring layer.

<img width="1223" height="1286" alt="azure-ai-vs-local-flow-diagram" src="https://github.com/user-attachments/assets/5e7df5ab-a834-4a7e-803c-2062498b4d5a" />

