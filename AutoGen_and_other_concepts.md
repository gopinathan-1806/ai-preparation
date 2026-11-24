# Agentic AI and LLM Engineering Notes

A collection of learning notes covering self-hosted large language models, agent frameworks, memory, retrieval-augmented generation, deployment, and local model fine-tuning.

## Table of Contents

- [Self-Hosted LLMs](#self-hosted-llms)
- [CrewAI](#crewai)
- [Retrieval-Augmented Generation](#retrieval-augmented-generation)
- [Deployment](#deployment)
- [AutoGen](#autogen)
- [CrewAI and AutoGen Compared](#crewai-and-autogen-compared)
- [Other Agent Tools](#other-agent-tools)
- [Core Agent Files](#core-agent-files)
- [LLM Fine-Tuning](#llm-fine-tuning)

## Self-Hosted LLMs

Self-hosted LLM models running on a virtual machine (VM) cannot integrate directly with a user interface or understand the user's request on their own. **vLLM** can serve as the communication layer between the model and the UI.

![Illustration from source notes](images/image-1.png)

## CrewAI

### Handling Multiple Requests

CrewAI with vLLM can handle multiple requests.

![Illustration from source notes](images/image-2.png)

### Memory

CrewAI supports memory through a configuration option:

```python
memory=True
```

![Illustration from source notes](images/image-3.png)

Enabling memory stores the conversation context for later use.

![Illustration from source notes](images/image-5.png)

CrewAI memory has two types:

![Illustration from source notes](images/image-4.png)

- **Short-term memory:** Set `memory=True` to enable it.
- **Long-term memory:** Configure the relevant environment variable. This creates a folder in the project where memory is saved. When the code runs, it checks that memory before executing the agent's task.

### Knowledge Sources and RAG

For a CrewAI application that needs company or client information, export the information as facts and store it under `knowledge`. The knowledge can also be supplied as a file. The CrewAI module can then use this knowledge while responding to user questions.

![Illustration from source notes](images/image-6.png)

### Vector Databases

A vector database can store information during a CrewAI process. The workflow is to load the information, convert it into embeddings, store it in a vector database such as FAISS, and retrieve relevant information through an LLM agent.

### Human-in-the-Loop

After an AI agent prepares a report for a client, add a human approval step before sending the report. The workflow waits for approval before it sends.

### Logging and Debugging

Set `verbose=True` on agents and tasks to print detailed logs along with the output.

## Retrieval-Augmented Generation

Production-grade RAG can include the following components:

- **Guardrails:** Validate input and output.
- **Evaluations:** Test LLM behavior. DeepEval is noted as an evaluation tool.
- **LangSmith:** Track token cost, timeouts, and input/output responses.

## Deployment

### Docker

Docker is used for version control in an offline session. A group of images is called a **container**.

The notes identify two files as required for deployment:

- `Docker`
- `docker-compose.yaml`, which contains the frontend and backend Docker files.

### Supporting Tools

- **Uvicorn:** A command-line tool/package used to run FastAPI applications, including agentic AI applications.
- **Git and Docker:** Install these on the VMs.
- **Certbot:** Used to add an SSL certificate to the application.
- **Nginx:** Used to redirect incoming traffic.

## AutoGen

AutoGen is an agentic framework similar to CrewAI. The notes describe it as suitable for dynamic workloads and complex operations where CrewAI may not be suitable. AutoGen was developed by Microsoft.

![Illustration from source notes](images/image-7.png)

### Creating Agents

![Illustration from source notes](images/image-8.png)

![Illustration from source notes](images/image-9.png)

To create agents, assign each one a role, client, and prompt.

![Illustration from source notes](images/image-10.png)

### Images as Input

AutoGen can accept images from a local machine. Send an image with a question through the `multi_model_message` variable to get a response. Image-based questions are built on a function.

![Illustration from source notes](images/image-11.png)

### Observing Agent Activity

Using `await console...` in code prints logs during execution. The notes contrast this with a first method that provides only the final output.

![Illustration from source notes](images/image-12.png)

### Memory

The notes state that AutoGen does not have memory in place by default, so a memory parameter must be called to store information.

![Illustration from source notes](images/image-13.png)

### Teams and Collaboration

Create a group of agents as a team. For example, agents can check logs, monitor systems, perform root-cause analysis (RCA), and have a final agent consolidate the results. The team works together to achieve its goal.

![Illustration from source notes](images/image-14.png)

### Termination

Without a termination condition, agents running as a team may continue in a loop and consume all available tokens. Configure termination to stop execution at a defined point.

![Illustration from source notes](images/image-15.png)

In a multi-agent workflow, for example, one agent creates a result and a second agent checks and approves it. If the termination keyword is `APPROVE`, the workflow stops when that keyword is returned.

![Illustration from source notes](images/image-16.png)

![Illustration from source notes](images/image-17.png)

### Swarm

A manager agent decides how many agents a workload requires and creates agents based on those requirements.

### Managing State Between Sessions

To retain session data or conversation memory, the notes describe dumping it into a README file and starting a new chat that refers to that file for context. This is compared to storing state in BOB.

![Illustration from source notes](images/image-18.png)

## CrewAI and AutoGen Compared

| CrewAI | AutoGen |
| --- | --- |
| Agents do not communicate directly with one another. A manager assigns work to all agents. | Agents can communicate with one another. The manager assigns work to a specific agent, and the agents collaborate to reach the desired result. |

![Illustration from source notes](images/image-19.png)

![Illustration from source notes](images/image-20.png)

## Other Agent Tools

### Hermes Agent

- The notes describe Hermes as one of the most-used and advanced agents currently available.

![Illustration from source notes](images/image-22.png)
- Topics listed for further exploration: Hermes agent architecture and Hermes agent data flow.

![Illustration from source notes](images/image-23.png)
- The notes also refer to a list of agents available in the market.

![Illustration from source notes](images/image-21.png)

### Composio

[Composio](https://composio.dev/) provides more than 1,500 tools that can be connected to agents.

![Illustration from source notes](images/image-24.png) The notes compare it with manually configuring a Gmail secret and client key for an n8n project, which takes more time; Composio simplifies that connection.

## Core Agent Files

The four core files listed for agentic AI are:

- `SOUL.md`
- `USER.md`
- `MEMORY.md`
- `AGENTS.md`

## LLM Fine-Tuning

![Illustration from source notes](images/image-25.png)

![Illustration from source notes](images/image-26.png)

### Fine-Tuning Approaches

The notes identify **QLoRA** as a fine-tuning technique that reduces an LLM model's size from 5 GB to 2 GB and reduces precision from 32-bit to 4-bit, allowing it to run more easily on smaller GPUs.

![Illustration from source notes](images/image-27.png)

### Hardware Requirements

Training and fine-tuning are easier with a GPU. Managing and training models using only a CPU is described as very difficult.

![Illustration from source notes](images/image-28.png)

### Choosing a Model and Preparing Data

The notes identify these considerations for fine-tuning:

- Choose the right model for fine-tuning.

![Illustration from source notes](images/image-29.png)
- Prepare a dataset for fine-tuning.
- Create a dataset in the format required by the training workflow.

![Illustration from source notes](images/image-30.png)

For a chatbot, provide chat data in JSONL format, save the dataset, and import it through code.

![Illustration from source notes](images/image-31.png)

![Illustration from source notes](images/image-32.png)

### Five-Step Local LLM Fine-Tuning Plan

1. Load the LLM model and its tokenizer.
2. Provide the dataset that defines what the model will be trained on.
3. Set up LoRA, described in the notes as a simple note about the changes.
4. Train the model with an SFTT trainer.
5. Use the resulting fine-tuned local LLM model.

![Illustration from source notes](images/image-33.png)

## Further Reading

The notes include the following CrewAI examples repository:

- [CrewAI Framework examples](https://github.com/Thirumurugan240/CrewAI_Framework)
