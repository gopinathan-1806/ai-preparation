# LangChain

## Original Class Notes

### LangChain

LangChain is an open-source framework that helps developers build applications using large language models (LLMs) by connecting them to external data, memory, and tools

### Components

Data Ingestion —> LangChain uses Document Loaders to fetch data from various formats (PDFs, CSVs, web pages, cloud storage) and standardize them into a standard Document data structure.

Types of Data source:
1. Structured Data —> tables, CSV, Excel
2. Semi structured data —> JSON, XML, YAML
3. Un structured data —> PDF, audio, docx, video

Text Splitter —> It will break the 100 page PDF into smaller chunks, so that it can go through it easily and any doubts asked by user will be cleared with more accuracy.

0 - 5000
0 - 500 | 400 - 900 | 800 - 1300 | … 5000
Chunk size - 800
Overlap - 100

Embeddings —> It turn raw text into number lists called vectors that capture semantic meaning for search and comparison.

[0.89 , 0.17] [0.81 , 0.98] [0.88

Vector Store —> Is a kind of serverless database which stores all the information

Retriver —> Fetches the most relevant text for our questions from vector store.
Chain / Pipeline —> A chain joins steps so they run one after another, like pipeline. The output of one step becomes the input of the next.

Pine cone —> Paid Vector DB —> Server
Vector DB —> FAISS and Chroma DB —> Serverless

### LangSmith

LangSmith is a tradable and monitoring tool used for monitor everything on your AI app, it’s like CCTV for our AI apps. Create an account with API key, integrate that API key into LangChain code, automatically, it will be integrated with Langsmith

## Additional Study Details

### RAG-oriented LangChain flow

A common flow is: load documents → split into chunks → create embeddings → store vectors → retrieve relevant chunks → optionally rerank/filter → build context → call the model → return the answer.

### Chunking

Chunk size and overlap are application-dependent. The 800/100 example in the class notes should be treated as a starting point, not a universal best setting. Evaluate chunking based on document structure, retrieval quality, context size, and downstream answer quality.

### Embeddings

An embedding converts text or another supported input into a numerical vector representing learned semantic features. Similarity measures such as cosine similarity are commonly used for retrieval.

### Vector stores

FAISS is a similarity-search library rather than a traditional hosted database. Chroma and Pinecone are examples of vector-storage/search systems with different deployment and operational models. Avoid categorizing every vector store as 'serverless'.

### Retrievers and reranking

A retriever selects candidate documents/chunks. Reranking can then reorder those candidates using a stronger relevance model or ranking function. This is especially useful when initial retrieval returns several plausible chunks.

### LangSmith

LangSmith can be used for tracing, debugging, evaluation, and monitoring of LLM application runs. It is useful for understanding prompts, retrieved context, tool calls, latency, and failures.
