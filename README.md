# Social Eagle — AI Engineering Class Notes

> **Combined classroom notes + corresponding images.**
>
> The classroom wording is preserved from the source document. Each screenshot/image is inserted immediately where it occurs in the original DOCX.

---

Social Eagle

Picture - Nano Banana

Video - veo

As a product - Google AI studio (contains app building, speech, game building)

PPT - Gamma

Browser - Compact

Tooling.ai - > database for all AI tools

Hugging Face - This tool provides all alternative like video, image for free of cost

Doubt.socialeagle.ai —> portal to raise an issue

Assignment.socialeagle.ai —> To submit the weekly assignment

Saturday 8 AM to 9 AM —> Build a new application

Python VE

Virtual Environment —> The primary reason to create a virtual environment in Python is dependency isolation. If am working on different projects called A and B. If A need a version 1.0 and B need a version 2.0 then using VE will fix this issue.

## Python Modules:

Created a function, if that needs to be used in another py file, then we need to use python module. Say example, I have created a function called addition under module folder. If this needs to be called in another code, it will be starting with

from module.addition import add

So automatically, that function will be imported into our code. We need to create _init_.py file under modules, which will convert our function into package.

## Python File handling:

w —> write / overwrite

r —> read

a —> append the existing file

with open(“myfile.txt” , “w”) as file

## RPA

PyAutoGUI —> For operating Desktop Apps

Playwright —> For operating / automating our tasks on browser

PyAuto GUI —> Automate our task using lap mouse / keyboard —> its Python library

PyAutoGUI Functionalities —> Mouse control, keyboard control, multiple window control, screenshot

## API

API —> Communication between two servers Types: 1. REST API 2. Graph QL —> Meta is using this 3. SOAP 4. RPC 5. Web socket

REST API method on REST API

A. POST —> Creating the new entry

B. GET —> getting the details from server

C. PUT —> modifying the details on the server

D. DELETE —> deleting the record

4xx Client Errors

5xx Server Errors

2xx Success

3xx Redirection

POSTMAN tool used to test our API’s

Streamlit is used for building and sharing interactive web applications and dashboards entirely in Python, eliminating the need for frontend web development skills like HTML, CSS, or JavaScript.

Streamlit is easy frontend creating app without using HTML, CSS

## LLM

LLM —> Large Language Model is a type of artificial intelligence program trained on massive amounts of text data to understand, summarize, translate, and generate human-like language.

It has 5 stages of processing, each word is considered as token ? Eg. Write a python program ? —> 5 token will be used to process this.

![Social Eagle classroom screenshot](images/image-01.png)

## Types of LLMs

LMM —> Large Multimodel —> used for image / video generation —> nano banana

SLM —> Small Langage Model  —> Enterprise uses this

VLM —> Vision Language model

Open source LLM vs Cloud / Closed LLM

Opensource LLM means nothing but the free LLM models available in the market, we can download it locally and optimise it for our need, which will be running on our laptop or our server. Ollama and Hugging Face is the place where we can download our models.

Cloud / Closed LLM nothing but the models owned by companies called OpenAI, Google, we cannot download the model instead we can work with that.

![Social Eagle classroom screenshot](images/image-02.png)

Prompt Engineering

We need to provide proper prompt to get the better response. To provide the better prompt, use the below framework.

![Social Eagle classroom screenshot](images/image-03.png)

Role —> Give a role to AI model before providing the prompt, Act as Senior Cloud architect and design Netflix cloud architecture.

Task —> Define the task clearly, so that model understands it in better way

Context —> Explain the context to the models (i.e.., write a leave letter (without context), write a leave letter to my manager for my health issue and admitted in hospital in standard format (with context).

Without context AI guess the answer

Without context, AI provides the best solution

Few Shots —> Nothing but providing some example while prompting, so that AI models knows the exact need

Response / Report —> How you need the output from ChatGPT, like you need its in bullet points or JSON format or README format or content plan format

Attaching the example on how the prompt should be provided based on RTCFR framework

![Social Eagle classroom screenshot](images/image-04.png)

## Reverse prompting:

Instead of writing a big prompt with RTCFP format, you can ask GPT to create a RTCFP format prompt, it will provide the best prompt and that can be used.

## Prompt Injection:

Nothing but, using modified or unauthorised keywords with LLM models and breaking its security and privacy. So, still companies are working on it.

## JSON Prompting:

When providing the input through JSON, system understands it clearly and output is far better. Token utilisation is low when comparing to text prompts.

## Scale Framework:

This framework is best for image generation,

S —> Subject —> context of image

C —> Composition —> camera angle —> top view, selfie, side angle

A —> Action —> Type of action

L —> Location

## LangChain

LangChain is an open-source framework that helps developers build applications using large language models (LLMs) by connecting them to external data, memory, and tools

Below are the components from LangChain

Data Ingestion —> LangChain uses Document Loaders to fetch data from various formats (PDFs, CSVs, web pages, cloud storage) and standardize them into a standard Document data structure.

## Types of Data source:

Structured Data —> tables, CSV, Excel

Semi structured data —> JSON, XML, YAML

Un structured data —> PDF, audio, docx, video

Text Splitter —> It will break the 100 page PDF into smaller chunks, so that it can go through it easily and any doubts asked by user will be cleared with more accuracy.

0 - 5000

0 - 500 | 400 - 900 | 800 - 1300 | … 5000

Chunk size - 800 Overlap - 100

Embeddings  —> It turn raw text into number lists called vectors that capture semantic meaning for search and comparison.

[0.89 , 0.17] [0.81 , 0.98] [0.88

Vector Store —> Is a kind of serverless database which stores all the information

Retriver —> Fetches the most relevant text for our questions from vector store. Chain / Pipeline —> A chain joins steps so they run one after another, like pipeline. The output of one step becomes the input of the next.

Pine cone —> Paid Vector DB —> Server

Vector DB —> FAISS and Chroma DB —> Serverless

## LangSmith

LangSmith is a tradable and monitoring tool used for monitor everything on your AI app, it’s like CCTV for our AI apps. Create an account with API key, integrate that API key into LangChain code, automatically, it will be integrated with Langsmith

## RAG

Retrieval-Augmented Generation (RAG) is an AI framework that improves large language models (LLMs) by fetching relevant data from external knowledge bases before generating a response

Types of RAG

![Social Eagle classroom screenshot](images/image-05.png)

Self RAG —> AI check its own work before providing the output to client, we’ll get the high accuracy outcome through Self RAG

Corrective RAG —> Checks the data quality at the retrieval stage.

![Social Eagle classroom screenshot](images/image-06.png)

Fusion RAG —> Fusion RAG asks the question in many different ways, searches with each version separately. It combines all results using RRF.

BM25 + ReRank (best suited for production) —> is a keyword-based ranking function used to score and re-order documents based on exact term matches and word rarity.

Temperature  —> (0 - Focused, 1 - creative / imagination)

## Messages:

![Social Eagle classroom screenshot](images/image-07.png)

## Guardrails:

Guardrails are safety boundaries or control mechanisms designed to prevent systems, vehicles, or AI models from veering into dangerous, harmful, or unintended territory

There are two types of Guardrails

Input Guardrails —> It will stop the dangerous prompt from user and won’t be sent to LLM (token will be saved). Example - How to create a bomb

Output Guardrails —> It validates the output generated by LLM is correct or not, if dangerous, it will block the output

![Social Eagle classroom screenshot](images/image-08.png)

## Evaluation:

Whenever we’re done with our GenAI application, we have to evaluate it with 10,000+ automated test runs to check it accuracy. Score will be calculated  between 0 to 1. Getting a result near to 95% is ready for production deployment.

## Types of RAG:

Vector DB —> Types —> Self, Corrective, Fuse, BM25 —> Its normal pipelines process like Data injection —> Splitting —> Emedding —> Vector DB

Knowledge graph

Knowledge graph RAG - Knowledge graph RAG is an advanced way of helping AI find information by connecting facts like a map of ideas instead of just guessing based on similar words. It will create relational DB to create the uniqueness. Each round is called Node and the connection is called EDGE.

Vector DB just stores the data if you twist the question it won’t answer properly that’s why Knowledge Graph came into picture, where the entire data will be exacted, relationship will be made like below and then storage it in DB and query it. These are internal LLM operations we no need to worry about it.

![Social Eagle classroom screenshot](images/image-09.png)

## AI Automation

3 types

ETL Automation

API automation

UI Automation

AI Automation Platforms

No code —> n8n, make, Zapier (n8n is the only open source free tool, others are paid one)

Code —> Playwright, playGUI, selenium

n8n Installation methods

Local installation —> download the package and run in your local

Self hosted method —> Best method —> use Cloud VMs for hosting

Production n8n —> n8n.io —> but very expensive

Types of nodes in n8n

Trigger node

Data flow node

Conditional node

Action

AI node (LLM, AI Agent)

Two types of Trigger node

HTTP node —> Whenever we need, we should called HTTP

Webhook node —> Any changes in the system, web hook request will be initiated

Authentication vs Authorization

Authentication —> If you can login to AWS console or not

Authorization —> After login to AWS, can you create EC2

Types of authentication

API key —> Full access

Token access —> Token will be created with set of permissions like user need to read only this repo, so user can read only that repo.

Auth token —> High authentication, client ID and server will be created for validation

UI Automation

If there is an issue with API and ETL automation, then we need to use this UI automation, this will run in our local laptop. For UI automation, we need web scraping to collect the data from internet.

Scrapling, Market Down, Scrapy —> Tools available to perform the scraping.

## AI Agents:

Will be running continuously to complete the task

User Input goes to the system prompt then goes to LLM. LLM will be performing the below operations  Think —> Action —> Observe the results—> If results looks good —> Output —> If the results are not meet —> again action —> observe the result

It will be happening in a loop, and token utilisation is high here.

TAOTR —> Think —> Action —> Observe —> Think again —> Result

![Social Eagle classroom screenshot](images/image-10.png)

## AI Agent Architecture

PBA —> Preception —> Brain —> Action

![Social Eagle classroom screenshot](images/image-11.png)

## MCP architecture

![Social Eagle classroom screenshot](images/image-12.png)

## Fall back agents:

If we have an AI model (Open AI) in our agent due to any issue if our agent is not responding, it will impact our automation framework, so having fall back means another AI model (Gemini, Grok) will be on standby to run in case of any issues with primary model.

## Human In the Loop:

Running some critical workload AI automation, add a simple step to get a human approval before proceeding.

## AI Agents:

![Social Eagle classroom screenshot](images/image-13.png)

![Social Eagle classroom screenshot](images/image-14.png)

Pydantic —> It a python library that checks your data and make sure its a correct type and shape, before sending it to Agents.

Async Function —> allows the code to run parallel instead of running one by one.

![Social Eagle classroom screenshot](images/image-15.png)

## Crew AI

Crew AI is a python framework for building a team of agents that work together to finish the job.

![Social Eagle classroom screenshot](images/image-16.png)

![Social Eagle classroom screenshot](images/image-17.png)

## Demo:

Here we’re creating AI agent just using python, while using crewai, we need to call agent, task and crew in the code.

For writer agent, create a goal, and then assign a task.

![Social Eagle classroom screenshot](images/image-18.png)

Crew AI architecture

![Social Eagle classroom screenshot](images/image-19.png)

All the crew AI components in single table

![Social Eagle classroom screenshot](images/image-20.png)

Tasks in crew AI

![Social Eagle classroom screenshot](images/image-21.png)

Demo / code

![Social Eagle classroom screenshot](images/image-22.png)

![Social Eagle classroom screenshot](images/image-23.png)

Step 3 : Run it with a  crew

![Social Eagle classroom screenshot](images/image-24.png)

Multi step workflow

Here, we’re creating 2 task, output of 1st task will be passed as input of 2nd task.

## Creating agent:

![Social Eagle classroom screenshot](images/image-25.png)

## Creating task:

![Social Eagle classroom screenshot](images/image-26.png)

Creating crew

Here we got the result from task 2 (short tweet)

![Social Eagle classroom screenshot](images/image-27.png)

## Process Types

## Two process types:

Sequential —> Above examples, everything happens in an order, output of 1st task goes to the input to 2nd task

Hierarchy  —> Here, we create multiple agents, manager (LLM) will decide which agent need to perform the provided action, suitable for complex operations.

Here you, can see the two agents (researcher and writer2), we’re defining the process called “Hierarchical”, selecting our manager as “LLM”, LLM decides and sends the works to relevant agent.

![Social Eagle classroom screenshot](images/image-28.png)

Crew AI don’t have inbuilt tools for file read operations, web scraping, so we need to import that using crewai-tools library package to use them.

![Social Eagle classroom screenshot](images/image-29.png)

In this example, we’re importing the file read tool and reading the file

![Social Eagle classroom screenshot](images/image-30.png)

## Custom Tool

Say example, I want to perform some custom operation but that package is not available in python means we can create that using function and can use it as tool in our code.

Here, we just created simple add function and just adding (@tool) so it marks this function as tool

![Social Eagle classroom screenshot](images/image-31.png)

![Social Eagle classroom screenshot](images/image-32.png)

![Social Eagle classroom screenshot](images/image-33.png)

Web search Tools on Crew AI

For webseach, we need serpAI API key for the web search, store it on .env file and load it on the step 1.

Here, we’re just calling that web search tool (search_tool)

![Social Eagle classroom screenshot](images/image-34.png)

---

# Additional Technical Details

> These are additions for deeper revision and interview preparation. They do **not** replace or delete the classroom notes above.

## Key clarifications

- **Tokens:** a token is not necessarily one word; tokenization affects context size, latency and cost.
- **Open source vs free:** these are not automatically the same. Model licenses and usage rights should be checked.
- **RAG:** a common flow is Load → Split → Embed → Store → Retrieve → Rerank/Filter → Prompt → Generate.
- **Chunking:** values such as chunk size 800 and overlap 100 are starting points, not universal settings.
- **BM25:** lexical retrieval can be combined with vector retrieval and reranking for hybrid search.
- **Guardrails:** input/output checks are useful, but production systems also need validation, permissions, least privilege and tool controls.
- **Evaluation:** there is no universal requirement that every GenAI application use exactly 10,000 test runs or a 95% threshold; acceptance criteria should be application-specific.
- **Knowledge Graph:** nodes represent entities and edges represent relationships; graph retrieval can help with relationship-heavy and multi-hop questions.
- **Agents:** production agents should have iteration limits, timeouts, retries, permissions, validation, observability and cost controls.
- **Authentication vs authorization:** authentication establishes identity; authorization determines permissions.

## Revision pattern

**Concept → Architecture → Example → Code → Production consideration → Interview question → Project usage**

---

## Source Preservation

- `original-readme-notes.docx` is included as the original source.
- **34 classroom images** are extracted into `images/`.
- Images use relative Markdown paths and will render directly on GitHub.