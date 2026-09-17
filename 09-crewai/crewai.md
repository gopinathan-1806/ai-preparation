# CrewAI

## Original Class Notes

### Crew AI

Crew AI is a python framework for building a team of agents that work together to finish the job.

### Demo

Here we’re creating AI agent just using python, while using crewai, we need to call agent, task and crew in the code.
For writer agent, create a goal, and then assign a task.

### Multi step workflow

Here, we’re creating 2 task, output of 1st task will be passed as input of 2nd task.

### Process Types

Two process types:
1. Sequential —> Above examples, everything happens in an order, output of 1st task goes to the input to 2nd task
2. Hierarchy —> Here, we create multiple agents, manager (LLM) will decide which agent need to perform the provided action, suitable for complex operations.

Here you, can see the two agents (researcher and writer2), we’re defining the process called “Hierarchical”, selecting our manager as “LLM”, LLM decides and sends the works to relevant agent.

Crew AI don’t have inbuilt tools for file read operations, web scraping, so we need to import that using crewai-tools library package to use them.

In this example, we’re importing the file read tool and reading the file

### Custom Tool

Say example, I want to perform some custom operation but that package is not available in python means we can create that using function and can use it as tool in our code.

Here, we just created simple add function and just adding (@tool) so it marks this function as tool

### Web search Tools on Crew AI

For webseach, we need serpAI API key for the web search, store it on .env file and load it on the step 1.
Here, we’re just calling that web search tool (search_tool)

## Additional Study Details

### Core components

The main mental model from the class is Agent + Task + Crew + Process. An Agent represents a role/goal, a Task describes work and expected output, a Crew orchestrates agents/tasks, and the Process determines execution style.

### Sequential workflow

Sequential execution is useful when task B depends on task A. Keep the data contract between tasks explicit so the second task receives the intended output.

### Hierarchical workflow

Hierarchical orchestration introduces a manager/decision layer that routes work among agents. It can help with complex workflows but introduces additional model calls, latency, cost, and failure modes.

### Tools

A custom tool can expose a controlled Python function to an agent. Tool interfaces should validate arguments, enforce permissions, handle errors, and return structured results.

### Web search

If a search tool requires an external API key, store credentials securely using environment variables or a secrets manager. Never hard-code API keys into source control.

### Multi-agent design trade-off

Multiple agents are not automatically better than a single agent. Use them when task specialization or orchestration provides a measurable benefit; otherwise a simpler workflow may be easier to test, debug, and operate.

### Interview focus

Be able to explain why you chose sequential vs hierarchical execution, how tools are secured, how failures are handled, how task outputs are passed, and how you evaluate the final result.
