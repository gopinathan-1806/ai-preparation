# CrewAI — Simple Interview Notes

> **Goal:** Understand CrewAI in 10 minutes and remember how the pieces fit together.
>
> The simplest mental model is:
>
> **Flow controls the application → Crew manages the team → Agent does the job → Tool talks to the real world → LLM reasons.**

---

# 1. What is CrewAI?

CrewAI helps us build AI applications where **one or more AI agents perform real work**.

Instead of:

```text
User → LLM → Answer
```

we can build:

```text
User
  ↓
Flow
  ↓
Crew
  ↓
Agents
  ↓
Tools
  ↓
Real systems
  ↓
Result
```

CrewAI currently separates **Crews** (agent collaboration) from **Flows** (structured application orchestration). They can be used together. 

---

# 2. The 7 Concepts You Need to Remember

| Concept | Simple meaning | Example |
|---|---|---|
| **Agent** | AI worker | Kubernetes Engineer |
| **Task** | Work given to worker | Check failing pods |
| **Tool** | Way to interact with a system | `kubectl` |
| **Crew** | Team of AI workers | K8s + Log + Monitoring agents |
| **Process** | How the team works | Sequential |
| **Flow** | Controls the whole application | Start → investigate → approve → finish |
| **LLM** | Brain used by agents | OpenAI / Azure OpenAI / local LLM |

### Remember this:

```text
FLOW
 ↓
CREW
 ↓
AGENT
 ↓
TASK
 ↓
TOOL
 ↓
REAL SYSTEM
```

---

# 3. Agent

An **Agent is an AI worker with a specific responsibility**.

Example:

```python
k8s_agent = Agent(
    role="Kubernetes Engineer",
    goal="Investigate Kubernetes incidents",
    backstory="You are an experienced Kubernetes SRE.",
    tools=[kubectl_tool]
)
```

Think:

> **Agent = Who is doing the work?**

Examples:

```text
Kubernetes Agent
Security Agent
Database Agent
Cloud Agent
Research Agent
```

---

# 4. Task

A **Task is the actual work** given to an agent.

```python
task = Task(
    description="Check why payment-service pods are failing.",
    expected_output="Symptoms, evidence and possible cause.",
    agent=k8s_agent
)
```

Think:

> **Task = What work should be done?**

So:

```text
Agent = Who?
Task  = What?
```

---

# 5. Tool

A **Tool allows an agent to interact with the real world**.

Without a tool:

```text
Agent → LLM → Text
```

With a tool:

```text
Agent
  ↓
LLM decides
  ↓
Tool
  ↓
Kubernetes / GitHub / DB / API
  ↓
Result
  ↓
LLM
```

Examples:

```text
kubectl
GitHub API
AWS API
Azure API
SQL
Prometheus
Jira
Slack
```

Think:

> **Tool = How can the agent actually do something?**

---

# 6. Crew

A **Crew is a team of agents working together**.

Example:

```text
                 Crew
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      K8s       Logs     Monitoring
     Agent      Agent       Agent
```

Think:

> **Crew = Who works together?**

A Crew is useful when one AI worker does not have all the required expertise.

---

# 7. Process

Process defines **how tasks are executed**.

### Sequential

```text
Agent A
   ↓
Agent B
   ↓
Agent C
```

Example:

```text
K8s investigation
      ↓
Root cause analysis
      ↓
Remediation recommendation
```

### Hierarchical

```text
          Manager
        /    |    \
       ↓     ↓     ↓
      K8s   Logs  Cloud
```

The manager coordinates specialized agents.

---

# 8. Flow

This is the concept that often causes confusion.

A **Flow is the application workflow**.

Think of normal Python:

```python
if incident:
    investigate()

if critical:
    ask_human()

generate_report()
```

CrewAI Flow gives you a structured way to build this kind of application logic.

Example:

```text
Alert
 ↓
Flow
 ↓
Validate
 ↓
Run Crew
 ↓
Check result
 ↓
If critical → Human approval
 ↓
Final report
```

Think:

> **Flow = controls what happens from beginning to end.**

---

# 9. Crew vs Flow — Very Simple

This is the one thing I want to remember for interviews:

### Crew

> **"I need a team of AI workers to solve this problem."**

```text
Crew
 ├── K8s Agent
 ├── Log Agent
 └── Monitoring Agent
```

### Flow

> **"I need to control the entire application process."**

```text
Start
 ↓
Validate
 ↓
Run Crew
 ↓
Check result
 ↓
Human approval
 ↓
Finish
```

### Together

```text
                 FLOW
                  ↓
              Run CREW
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
       Agent     Agent     Agent
```

---

# 10. Memory

Memory means:

> **The system remembers useful context.**

Example:

```text
Previous conversation
Previous investigation
Previous decision
Current request
```

Do not confuse:

```text
Memory ≠ RAG
```

### Memory

```text
"What did we discuss earlier?"
```

### RAG / Knowledge

```text
"What does the company runbook say?"
```

---

# 11. Knowledge / RAG

An agent can use company knowledge.

```text
Company Runbooks
      ↓
RAG
      ↓
Kubernetes Agent
      ↓
Investigation
```

Example:

> "What is our standard procedure for ImagePullBackOff?"

The agent can search the runbook before answering.

---

# 12. Guardrails

Guardrails control what the AI is allowed to do.

Example:

```text
User request
    ↓
Guardrail
    ↓
Is this allowed?
    ├── No → Reject
    └── Yes
          ↓
        Agent
```

For Kubernetes:

```text
READ operations → allowed

DELETE / SCALE / ROLLBACK
       ↓
Human approval
```

---

# 13. Human-in-the-Loop

For production systems, don't let the AI blindly execute dangerous actions.

### Unsafe

```text
AI
 ↓
kubectl delete pod
```

### Safer

```text
AI investigates
      ↓
AI recommends action
      ↓
Human approves
      ↓
Tool executes
```

---

# 14. Real-World Use Cases

## Use Case 1 — DevOps Incident Commander

This is the example we will build.

```text
Production Alert
      ↓
Incident Commander
      ↓
 ┌────┼────┐
 ↓    ↓    ↓
K8s  Logs  Monitoring
Agent Agent Agent
 └────┼────┘
      ↓
Root Cause
      ↓
Remediation
      ↓
Human Approval
      ↓
Incident Report
```

---

## Use Case 2 — GitHub Security Reviewer

```text
GitHub PR
    ↓
Security Crew
    ├── Secret Agent
    ├── Injection Agent
    ├── Auth Agent
    └── Dependency Agent
            ↓
       Security Report
            ↓
       GitHub Comment
```

---

## Use Case 3 — Interview Preparation

```text
Resume + JD
     ↓
Interview Crew
 ├── JD Agent
 ├── Resume Agent
 ├── Skill Gap Agent
 └── Question Agent
       ↓
Preparation Plan
```

---

# 15. Complete Example — Kubernetes Incident Commander

## Scenario

A production service is failing.

The system should investigate:

```text
1. Are pods healthy?
2. Are pods restarting?
3. Are there Kubernetes events?
4. Did a deployment recently change?
5. What is the likely root cause?
6. What should the engineer do?
```

---

# 16. Simple Architecture

```text
             Production Alert
                    ↓
                  FLOW
                    ↓
          Incident Commander
                    ↓
       ┌────────────┼────────────┐
       ↓            ↓            ↓
   K8s Agent     Log Agent   Monitoring Agent
       ↓            ↓            ↓
    kubectl        Logs       Prometheus
       └────────────┼────────────┘
                    ↓
              Root Cause
                    ↓
              Remediation
                    ↓
             Human Approval
                    ↓
              Final Report
```

---

# 17. Minimal Project Structure

```text
crewai-k8s/
│
├── .env
├── main.py
└── kubernetes_tool.py
```

For learning, keep it this simple first.

Don't start with 10 Python files.

---

# 18. Install

```bash
python3 -m venv .venv
source .venv/bin/activate

pip install crewai python-dotenv pydantic
```

Set your model credentials:

```bash
export OPENAI_API_KEY="your-key"
```

---

# 19. Kubernetes Tool

`kubernetes_tool.py`

```python
import subprocess

from crewai.tools import tool


@tool("kubernetes_readonly")
def kubernetes_readonly(namespace: str = "default") -> str:
    """
    Read-only Kubernetes investigation.
    Gets pods, deployments and recent events.
    """

    commands = [
        ["kubectl", "get", "pods", "-n", namespace, "-o", "wide"],
        ["kubectl", "get", "deployments", "-n", namespace],
        [
            "kubectl", "get", "events",
            "-n", namespace,
            "--sort-by=.lastTimestamp"
        ],
    ]

    output = []

    for command in commands:
        result = subprocess.run(
            command,
            capture_output=True,
            text=True,
            timeout=30
        )

        output.append(
            f"$ {' '.join(command)}\n"
            f"{result.stdout}\n"
            f"{result.stderr}"
        )

    return "\n\n".join(output)
```

The important point is:

```text
CrewAI Agent
      ↓
kubernetes_readonly()
      ↓
kubectl
      ↓
Kubernetes cluster
```

---

# 20. Create the Agents

`main.py`

```python
import os

from crewai import Agent, Task, Crew, Process, LLM
from dotenv import load_dotenv

from kubernetes_tool import kubernetes_readonly

load_dotenv()

llm = LLM(
    model=os.getenv("OPENAI_MODEL", "gpt-4o-mini"),
    temperature=0
)


k8s_agent = Agent(
    role="Kubernetes Engineer",
    goal="Investigate the Kubernetes incident using evidence.",
    backstory=(
        "You are an experienced Kubernetes SRE. "
        "Never invent evidence."
    ),
    tools=[kubernetes_readonly],
    llm=llm,
    verbose=True
)


root_cause_agent = Agent(
    role="Root Cause Analyst",
    goal="Determine the most likely root cause from the evidence.",
    backstory=(
        "You correlate technical evidence and clearly separate "
        "facts from assumptions."
    ),
    llm=llm,
    verbose=True
)


remediation_agent = Agent(
    role="Remediation Advisor",
    goal="Recommend safe remediation based on the evidence.",
    backstory=(
        "You are a production SRE. "
        "Do not recommend destructive actions without human approval."
    ),
    llm=llm,
    verbose=True
)
```

---

# 21. Create the Tasks

Continue in `main.py`:

```python
investigate_task = Task(
    description="""
    Investigate the production Kubernetes incident
    in namespace {namespace}.

    Check:
    - Pod status
    - Restart counts
    - Deployments
    - Recent events

    Return:
    - Symptoms
    - Evidence
    - Suspicious resources
    - Possible causes
    """,
    expected_output="Structured Kubernetes investigation.",
    agent=k8s_agent
)


root_cause_task = Task(
    description="""
    Review the Kubernetes investigation.

    Identify the most likely root cause.

    Do not invent evidence.
    Clearly separate facts from hypotheses.
    """,
    expected_output=(
        "Root cause, evidence, confidence and alternative hypotheses."
    ),
    agent=root_cause_agent,
    context=[investigate_task]
)


remediation_task = Task(
    description="""
    Based on the root cause analysis,
    recommend safe remediation.

    Include:
    - Immediate action
    - Permanent fix
    - Rollback option
    - Risks
    - Validation steps
    - Human approval requirement
    """,
    expected_output="Safe production remediation plan.",
    agent=remediation_agent,
    context=[root_cause_task]
)
```

---

# 22. Create the Crew

```python
crew = Crew(
    agents=[
        k8s_agent,
        root_cause_agent,
        remediation_agent
    ],
    tasks=[
        investigate_task,
        root_cause_task,
        remediation_task
    ],
    process=Process.sequential,
    verbose=True
)
```

This means:

```text
Task 1
  ↓
Task 2
  ↓
Task 3
```

---

# 23. Run It

```python
result = crew.kickoff(
    inputs={
        "namespace": "production"
    }
)

print(result)
```

Run:

```bash
python main.py
```

---

# 24. What Happens?

This is the most important part.

### Step 1

CrewAI starts the first task:

```text
Investigate Kubernetes
```

### Step 2

Kubernetes Agent decides it needs information.

```text
Agent
 ↓
kubectl tool
```

### Step 3

Kubernetes returns:

```text
Pods
Deployments
Events
```

### Step 4

Root Cause Agent receives the investigation.

```text
Investigation
      ↓
Root Cause Agent
```

### Step 5

Remediation Agent receives the root cause.

```text
Root Cause
     ↓
Remediation Agent
```

Final result:

```text
Incident
   ↓
Evidence
   ↓
Root Cause
   ↓
Recommended Fix
```

---

# 25. Example

Suppose the cluster returns:

```text
payment-service-abc
CrashLoopBackOff

Restart Count: 8

Event:
Failed to pull image

Deployment:
payment-service:v2.4.1
```

The system may produce:

```text
ROOT CAUSE

Container image pull failure.

Evidence:
- Pod is CrashLoopBackOff
- Recent event reports image pull failure
- Deployment uses v2.4.1

RECOMMENDATION

1. Check imagePullSecret.
2. Verify registry credentials.
3. Verify image v2.4.1 exists.
4. Consider rollback if service impact continues.

Approval:
Required before rollback.
```

---

# 26. Where Each CrewAI Concept Appears

This is the easiest way to remember the project:

```text
                    CREWAI
                       │
                       ▼
                     FLOW
                "Control workflow"
                       │
                       ▼
                      CREW
                  "AI team"
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       K8s Agent    Root Cause    Remediation
          │            │            │
        Task         Task          Task
          │
        Tool
          │
       kubectl
          │
    Kubernetes
```

So in our project:

| CrewAI concept | Our example |
|---|---|
| Agent | Kubernetes Agent |
| Task | Investigate pods |
| Tool | `kubectl` |
| Crew | Incident-response team |
| Process | Sequential |
| Flow | Overall incident workflow |
| LLM | Reasoning engine |
| RAG | Company runbooks |
| Guardrail | Read-only / approval |
| Memory | Previous incident context |

---

# 27. Interview Cheat Sheet

### What is CrewAI?

> A Python framework for building AI agents and multi-agent applications.

### What is an Agent?

> A specialized AI worker with a role, goal and tools.

### What is a Task?

> A specific piece of work assigned to an agent.

### What is a Tool?

> A capability that lets an agent interact with an external system.

### What is a Crew?

> A team of agents working together.

### What is a Flow?

> The application-level workflow that controls execution, state and decisions.

### Crew vs Flow?

> Crew = AI team. Flow = application workflow.

### Why multiple agents?

> To separate responsibilities and give each specialist the right tools and context.

### Why not one agent?

> If one agent can solve the problem reliably, multiple agents add unnecessary complexity and cost.

### How do you safely automate Kubernetes?

> Start with read-only investigation. Let the AI recommend remediation, then require human approval before destructive actions.

---

# 28. Final 30-Second Mental Model

Don't try to memorize everything.

Just remember:

```text
                 FLOW
                  ↓
                CREW
                  ↓
        ┌─────────┼─────────┐
        ↓         ↓         ↓
      AGENT     AGENT     AGENT
        ↓         ↓         ↓
       TASK      TASK      TASK
        ↓
       TOOL
        ↓
   REAL SYSTEM
        ↓
      RESULT
```

### In plain English:

> **Flow decides what happens.**
>
> **Crew organizes the AI team.**
>
> **Agent does a specialized job.**
>
> **Task tells the agent what to do.**
>
> **Tool lets the agent interact with the real world.**
>
> **LLM provides the reasoning.**

That's the CrewAI abstraction you should have in your head before an interview.

---

## Official references

- [CrewAI Documentation](https://docs.crewai.com/)
- [CrewAI Agents](https://docs.crewai.com/en/concepts/agents)
- [CrewAI Tasks](https://docs.crewai.com/en/concepts/tasks)
- [CrewAI Crews](https://docs.crewai.com/en/concepts/crews)
- [CrewAI Flows](https://docs.crewai.com/en/concepts/flows)
