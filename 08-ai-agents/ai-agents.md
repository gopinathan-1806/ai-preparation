# AI Agents, MCP and Agent Reliability

## Original Class Notes

### AI Agents

Will be running continuously to complete the task

User Input goes to the system prompt then goes to LLM. LLM will be performing the below operations
Think —> Action —> Observe the results —> If results looks good —> Output —> If the results are not meet —> again action —> observe the result

It will be happening in a loop, and token utilisation is high here.

TAOTR —> Think —> Action —> Observe —> Think again —> Result

### AI Agent Architecture

PBA —> Preception —> Brain —> Action

### MCP architecture

### Fall back agents

If we have an AI model (Open AI) in our agent due to any issue if our agent is not responding, it will impact our automation framework, so having fall back means another AI model (Gemini, Grok) will be on standby to run in case of any issues with primary model.

### Human In the Loop

Running some critical workload AI automation, add a simple step to get a human approval before proceeding.

### Pydantic

It a python library that checks your data and make sure its a correct type and shape, before sending it to Agents.

### Async Function

allows the code to run parallel instead of running one by one.

## Additional Study Details

### Agent mental model

An agent is an application that can select or sequence actions using a model, tools, instructions, and state/memory to achieve a goal. The exact loop may be implemented as ReAct-style reasoning/action/observation or another orchestration pattern.

### PBA

Your notes use Perception → Brain → Action as a simple agent architecture. Map this to receiving information, deciding what to do, and invoking an action/tool.

### MCP

The class diagram shows Host → MCP Client → MCP Server → external Resources/Tools. MCP provides a standardized way for an AI application to interact with external capabilities through MCP servers. Keep the diagram from the source notes in the assets folder for visual revision.

### Fallback models

Fallbacks can improve resilience when a model provider or model endpoint fails. Production design should also define timeout, retry, circuit-breaker, routing, compatibility, and output-validation behavior; blindly switching models can change quality or tool behavior.

### Human-in-the-loop

Use human approval for high-impact or irreversible operations such as production changes, financial actions, security actions, or sensitive communications. Define exactly what requires approval and what happens if approval is denied.

### Pydantic

Pydantic provides Python data validation and typed models. It is useful for validating structured inputs/outputs, tool arguments, configuration, and agent state.

### Async programming

Async programming is useful for I/O-bound work such as API calls, network requests, and file operations. It does not mean every task literally runs simultaneously; concurrency depends on the event loop and workload.

### Agent production checklist

Define tool permissions, input validation, output validation, timeouts, retries, state management, observability, cost limits, maximum iterations, human approval points, and failure handling.
