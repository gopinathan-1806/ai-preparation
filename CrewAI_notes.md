# CrewAI --- Interview Notes + Kubernetes Agent

> Practical interview notes for CrewAI, plus a complete real-world
> Kubernetes Incident Commander example.

## 1. What is CrewAI?

CrewAI is a Python framework for building agentic applications and
multi-agent systems.

``` text
Agent + Task + Tool
        ↓
      Crew
        ↓
     Process
        ↓
   Final Result
```

Current CrewAI has two complementary building blocks:

-   **Crews** --- teams of autonomous, role-based agents collaborating
    on a problem.
-   **Flows** --- structured, event-driven workflows that manage state,
    branching and execution order.

For production applications, the current CrewAI guidance is
**Flow-first**: use a Flow as the application backbone and invoke a Crew
when a step needs autonomous multi-agent collaboration.

------------------------------------------------------------------------

# 2. Core Concepts

## Agent

An Agent is a specialized AI worker with a role, goal, context and
tools.

``` python
agent = Agent(
    role="Kubernetes Reliability Engineer",
    goal="Investigate Kubernetes incidents and identify the likely root cause",
    backstory="You are an experienced Kubernetes SRE.",
    tools=[kubernetes_tool],
    verbose=True
)
```

**Interview:** Agent = AI specialist / team member.

### Role

Who the agent is.

### Goal

What the agent is trying to achieve.

### Backstory

Context that shapes the agent's behavior and expertise.

------------------------------------------------------------------------

## Task

A Task is a specific piece of work assigned to an agent.

``` python
task = Task(
    description="Investigate failing production pods.",
    expected_output="Structured incident analysis with evidence.",
    agent=agent
)
```

Remember:

``` text
Agent = Who?
Task  = What?
```

Important Task fields:

  Field               Purpose
  ------------------- -----------------------
  `description`       Work to perform
  `expected_output`   Expected result
  `agent`             Responsible agent
  `tools`             Tools available
  `context`           Previous task outputs

------------------------------------------------------------------------

## Tool

A Tool gives an agent the ability to interact with external systems.

``` text
Agent
  ↓
LLM decides to use tool
  ↓
Tool
  ↓
External system
  ↓
Tool result
  ↓
LLM
```

Examples:

``` text
kubectl
AWS APIs
Azure APIs
GitHub
SQL
Prometheus
Jira
Slack
Web search
```

For this project:

``` text
Kubernetes Agent
       ↓
   kubectl Tool
       ↓
Kubernetes Cluster
```

------------------------------------------------------------------------

## Crew

A Crew is a team of agents working together on tasks.

``` python
crew = Crew(
    agents=[agent1, agent2],
    tasks=[task1, task2],
    process=Process.sequential,
    verbose=True
)
```

**Interview:** Crew = AI team.

------------------------------------------------------------------------

## Process

Defines how tasks execute.

### Sequential

``` text
Agent A
   ↓
Agent B
   ↓
Agent C
```

Useful when the next task depends on the previous task.

### Hierarchical

``` text
          Manager Agent
          /     |     \
         ↓      ↓      ↓
      Agent A Agent B Agent C
```

Useful when a manager-style agent coordinates specialized agents.

------------------------------------------------------------------------

## Flow

A Flow is the application-level orchestration layer.

Use it for:

-   State management
-   Event-driven execution
-   Branching
-   Routing
-   Loops
-   Human approval
-   Calling Crews
-   Normal Python/business logic

Example:

``` text
API Request
    ↓
Flow
    ↓
Validate
    ↓
Run Crew
    ↓
Evaluate result
    ↓
IF Critical
    ↓
Human Approval
    ↓
Create incident ticket
```

### Crew vs Flow

  Crew                  Flow
  --------------------- -----------------------------
  AI team               Application workflow
  Agent collaboration   State + control
  Autonomous work       Deterministic orchestration
  Complex task          End-to-end application

**Interview answer:**

> I use a Flow for deterministic application orchestration and state
> management, and a Crew when a particular step benefits from autonomous
> collaboration between specialized agents.

------------------------------------------------------------------------

# 3. Memory

Memory allows an agent/application to retain useful context.

``` text
Current request
      +
Previous context
      +
Stored state
      ↓
Better decision
```

Do not confuse:

``` text
Memory ≠ Knowledge Base
```

**Memory:** conversation, previous decisions, user context, execution
state.

**Knowledge:** company documents, runbooks, policies and manuals.

------------------------------------------------------------------------

# 4. Knowledge

Knowledge gives agents information to reason over.

Example:

``` text
Company Runbooks
       ↓
Knowledge / RAG
       ↓
Kubernetes Agent
       ↓
Incident Investigation
```

It can be combined with:

``` text
Azure AI Search
Amazon Bedrock Knowledge Bases
Pinecone
FAISS
```

------------------------------------------------------------------------

# 5. Guardrails

Guardrails validate or restrict AI behavior.

Examples:

``` text
Input validation
Output validation
Safety checks
PII checks
Permission checks
Confidence thresholds
```

Example:

``` text
LLM Output
    ↓
Guardrail
    ↓
Valid?
    ├── Yes → Continue
    └── No  → Reject / Retry / Human review
```

For production, do not rely only on an LLM prompt for security
enforcement.

------------------------------------------------------------------------

# 6. Structured Output

Prefer machine-readable output when another system will consume the
result.

``` python
class IncidentResult(BaseModel):
    severity: str
    root_cause: str
    evidence: list[str]
    recommendation: str
```

Useful for APIs, databases, UI, downstream agents and automation.

------------------------------------------------------------------------

# 7. Delegation

An agent can delegate work to another agent when configured.

``` text
Incident Commander
        ↓
"Kubernetes Agent, investigate the pods."
        ↓
Kubernetes Agent
```

Useful when a manager needs to dynamically select specialists.

Trade-off: delegation can increase complexity, latency and LLM/tool
calls.

------------------------------------------------------------------------

# 8. Human-in-the-Loop

Very important for production.

Avoid:

``` text
AI
 ↓
kubectl delete
```

Prefer:

``` text
AI investigates
      ↓
AI recommends action
      ↓
Human approval
      ↓
Controlled remediation
```

High-risk actions should normally require explicit approval or tightly
controlled policies.

------------------------------------------------------------------------

# 9. Observability

Trace:

``` text
Flow
 ↓
Crew
 ↓
Agent
 ↓
Task
 ↓
LLM
 ↓
Tool
 ↓
Result
```

Monitor:

-   Agent execution
-   Task execution
-   LLM calls
-   Token usage
-   Latency
-   Tool calls
-   Errors
-   Retries
-   Final output

Production AI needs observability just like a production microservice.

------------------------------------------------------------------------

# 10. Real-Time Use Cases

## AI DevOps Incident Commander

``` text
Alert
 ↓
Incident Manager
 ├── Kubernetes Agent
 ├── Log Agent
 ├── Cloud Agent
 └── Monitoring Agent
       ↓
Root Cause Agent
       ↓
Remediation Agent
       ↓
Human Approval
       ↓
Incident Report
```

## AI Security Review Team

``` text
GitHub PR
 ↓
Security Manager
 ├── Secret Agent
 ├── Injection Agent
 ├── Auth Agent
 ├── Dependency Agent
 └── OWASP Agent
       ↓
Security Reviewer
       ↓
GitHub PR Comment
```

## AI Job Interview Team

``` text
Resume + JD
 ↓
Interview Manager
 ├── JD Analyst
 ├── Resume Analyst
 ├── Skill Gap Agent
 ├── Question Generator
 └── Mock Interview Agent
       ↓
Preparation Plan
```

## Enterprise Research Team

``` text
Question
 ↓
Research Manager
 ├── Web Research Agent
 ├── Enterprise RAG Agent
 ├── Knowledge Graph Agent
 └── Fact Checker
       ↓
Report Agent
```

------------------------------------------------------------------------

# 11. Complete Example --- AI Kubernetes Incident Commander

## Problem

A production Kubernetes service is experiencing errors.

Instead of an engineer manually checking:

``` text
kubectl
Application logs
Pod events
Deployments
Services
```

CrewAI coordinates specialized agents.

### Architecture

``` text
Production Alert / User Request
              ↓
           API / Webhook
              ↓
        CrewAI Flow
              ↓
      Incident Commander
              ↓
       ┌──────┼───────┐
       ↓      ↓       ↓
     K8s    Logs   Monitoring
    Agent   Agent    Agent
       └──────┼───────┘
              ↓
       Root Cause Agent
              ↓
      Remediation Agent
              ↓
       Human Approval
              ↓
        Final Report
```

------------------------------------------------------------------------

# 12. Project Structure

``` text
crewai-k8s-incident-commander/
├── .env
├── requirements.txt
└── src/
    ├── main.py
    ├── tools/
    │   └── kubernetes_tools.py
    ├── agents/
    │   └── agents.py
    └── tasks/
        └── tasks.py
```

------------------------------------------------------------------------

# 13. Installation

``` bash
python3 -m venv .venv
source .venv/bin/activate
pip install crewai python-dotenv pydantic
```

For current CrewAI project scaffolding:

``` bash
uv tool install crewai
crewai create crew k8s-incident-commander
```

------------------------------------------------------------------------

# 14. Environment

`.env`

``` bash
OPENAI_API_KEY=your_key_here
OPENAI_MODEL=gpt-4o-mini
```

Never commit `.env`.

------------------------------------------------------------------------

# 15. Kubernetes Tool --- Read Only

`src/tools/kubernetes_tools.py`

``` python
import json
import subprocess
from typing import Type

from pydantic import BaseModel, Field
from crewai.tools import BaseTool


class KubernetesInput(BaseModel):
    namespace: str = Field(
        default="default",
        description="Kubernetes namespace to investigate"
    )


class KubernetesInvestigationTool(BaseTool):
    name: str = "kubernetes_investigation"
    description: str = (
        "Performs read-only Kubernetes investigation using kubectl. "
        "Returns pods, recent events and deployments."
    )
    args_schema: Type[BaseModel] = KubernetesInput

    def _run(self, namespace: str = "default") -> str:

        commands = {
            "pods": [
                "kubectl", "get", "pods",
                "-n", namespace,
                "-o", "wide"
            ],
            "events": [
                "kubectl", "get", "events",
                "-n", namespace,
                "--sort-by=.lastTimestamp"
            ],
            "deployments": [
                "kubectl", "get", "deployments",
                "-n", namespace
            ]
        }

        result = {}

        for name, command in commands.items():
            try:
                completed = subprocess.run(
                    command,
                    capture_output=True,
                    text=True,
                    timeout=30,
                    check=False
                )

                result[name] = {
                    "return_code": completed.returncode,
                    "stdout": completed.stdout[-10000:],
                    "stderr": completed.stderr[-5000:]
                }

            except subprocess.TimeoutExpired:
                result[name] = {
                    "error": "kubectl command timed out"
                }

        return json.dumps(result, indent=2)
```

### Why read-only?

Do not initially expose:

``` text
kubectl delete
kubectl exec
kubectl scale
kubectl rollout undo
```

Use:

``` text
Investigation
    ↓
Recommendation
    ↓
Human approval
    ↓
Controlled execution
```

------------------------------------------------------------------------

# 16. Agents

`src/agents/agents.py`

``` python
import os

from crewai import Agent, LLM
from tools.kubernetes_tools import KubernetesInvestigationTool

llm = LLM(
    model=os.getenv("OPENAI_MODEL", "gpt-4o-mini"),
    temperature=0
)

kubernetes_tool = KubernetesInvestigationTool()

incident_commander = Agent(
    role="Production Incident Commander",
    goal=(
        "Coordinate the investigation of a Kubernetes production incident, "
        "identify the most likely root cause and produce an evidence-based summary."
    ),
    backstory=(
        "You are a senior SRE responsible for coordinating production incidents. "
        "You prioritize evidence and clearly separate facts from hypotheses."
    ),
    llm=llm,
    verbose=True
)

kubernetes_agent = Agent(
    role="Kubernetes Reliability Engineer",
    goal=(
        "Investigate Kubernetes resources and identify pod, deployment, "
        "service or cluster-level problems."
    ),
    backstory=(
        "You are an experienced Kubernetes engineer. "
        "You use kubectl evidence such as pod status, restarts, deployments "
        "and cluster events to diagnose failures."
    ),
    tools=[kubernetes_tool],
    llm=llm,
    verbose=True
)

root_cause_agent = Agent(
    role="Root Cause Analyst",
    goal=(
        "Correlate investigation evidence and determine the most likely "
        "root cause without inventing evidence."
    ),
    backstory=(
        "You are an SRE root-cause analyst. "
        "You correlate Kubernetes evidence and explicitly identify uncertainty."
    ),
    llm=llm,
    verbose=True
)

remediation_agent = Agent(
    role="Remediation Advisor",
    goal=(
        "Recommend safe remediation steps based only on available evidence."
    ),
    backstory=(
        "You are a senior production engineer. "
        "You never recommend destructive actions without explaining risk, "
        "rollback strategy and required human approval."
    ),
    llm=llm,
    verbose=True
)
```

------------------------------------------------------------------------

# 17. Tasks

`src/tasks/tasks.py`

``` python
from crewai import Task

from agents.agents import (
    incident_commander,
    kubernetes_agent,
    root_cause_agent,
    remediation_agent
)

investigate_kubernetes = Task(
    description="""
    Investigate the Kubernetes production incident in namespace {namespace}.

    Collect:
    - Pod status
    - Restart counts
    - Deployment status
    - Recent Kubernetes events

    Identify suspicious symptoms and provide raw evidence.
    Do not guess the root cause if evidence is unavailable.
    """,
    expected_output="""
    Structured Kubernetes investigation containing:
    1. Observed symptoms
    2. Evidence
    3. Suspicious resources
    4. Possible causes
    5. Evidence gaps
    """,
    agent=kubernetes_agent
)

analyze_root_cause = Task(
    description="""
    Analyze the Kubernetes investigation produced by the previous task.

    Determine the most likely root cause.

    Requirements:
    - Correlate evidence
    - Do not invent logs or metrics
    - Distinguish confirmed facts from hypotheses
    - Explain why the root cause is likely
    """,
    expected_output="""
    Root cause analysis containing:
    - Incident severity
    - Root cause
    - Supporting evidence
    - Confidence
    - Alternative hypotheses
    """,
    agent=root_cause_agent,
    context=[investigate_kubernetes]
)

recommend_remediation = Task(
    description="""
    Based on the root cause analysis, provide safe remediation steps.

    Include:
    - Immediate mitigation
    - Permanent fix
    - Rollback option if applicable
    - Risks
    - Validation steps
    - Human approval requirements
    """,
    expected_output="""
    Production-ready remediation plan with:
    - Immediate action
    - Permanent fix
    - Risk
    - Rollback
    - Validation
    - Human approval requirement
    """,
    agent=remediation_agent,
    context=[analyze_root_cause]
)

final_summary = Task(
    description="""
    Create a concise incident report for the engineering team.

    Include:
    - Incident summary
    - Severity
    - Root cause
    - Evidence
    - Recommended action
    - Validation plan
    - Open questions
    """,
    expected_output="Markdown incident report suitable for Slack, Jira or ServiceNow.",
    agent=incident_commander,
    context=[
        investigate_kubernetes,
        analyze_root_cause,
        recommend_remediation
    ]
)
```

------------------------------------------------------------------------

# 18. Run the Crew

`src/main.py`

``` python
from dotenv import load_dotenv
from crewai import Crew, Process

from agents.agents import (
    incident_commander,
    kubernetes_agent,
    root_cause_agent,
    remediation_agent
)

from tasks.tasks import (
    investigate_kubernetes,
    analyze_root_cause,
    recommend_remediation,
    final_summary
)

load_dotenv()

crew = Crew(
    agents=[
        incident_commander,
        kubernetes_agent,
        root_cause_agent,
        remediation_agent
    ],
    tasks=[
        investigate_kubernetes,
        analyze_root_cause,
        recommend_remediation,
        final_summary
    ],
    process=Process.sequential,
    verbose=True
)

if __name__ == "__main__":
    result = crew.kickoff(
        inputs={
            "namespace": "production"
        }
    )

    print("\n================ INCIDENT REPORT ================\n")
    print(result)
```

Run:

``` bash
python src/main.py
```

------------------------------------------------------------------------

# 19. Runtime Flow

``` text
kickoff()
   ↓
Kubernetes Investigation Task
   ↓
Kubernetes Agent
   ↓
kubectl Tool
   ↓
Kubernetes Cluster
   ↓
Evidence
   ↓
Root Cause Task
   ↓
Root Cause Agent
   ↓
Root Cause
   ↓
Remediation Task
   ↓
Remediation Agent
   ↓
Recommendation
   ↓
Final Summary Task
   ↓
Incident Commander
   ↓
Incident Report
```

------------------------------------------------------------------------

# 20. Example Incident

Suppose:

``` text
payment-service
5 pods

3 → CrashLoopBackOff
2 → Running
```

Recent events:

``` text
Failed to pull image
```

The Kubernetes Agent might identify:

``` text
Symptoms:
- 3 pods are CrashLoopBackOff
- Deployment references image v2.4.1
- Recent events show image pull failures

Likely cause:
Container registry authentication failure.
```

Root Cause Agent:

``` text
Severity: HIGH

Root Cause:
Container registry authentication failure.

Evidence:
- ImagePullBackOff events
- Failure began after deployment v2.4.1
- Existing running pods are from the previous version
```

Remediation Agent:

``` text
Immediate:
Verify imagePullSecret and registry credentials.

Alternative:
Rollback deployment if service capacity is affected.

Do not:
Delete pods before confirming the registry issue.
```

------------------------------------------------------------------------

# 21. Production Evolution

Add specialized integrations:

``` text
                    Incident Commander
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
 Kubernetes Agent      Log Agent        Cloud Agent
        ↓                  ↓                  ↓
      K8s              ELK/Loki          AWS/Azure
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ↓
                    Root Cause Agent
                           ↓
                    Remediation Agent
                           ↓
                     Human Approval
```

Possible tools:

``` text
Kubernetes
Prometheus
Grafana
ELK / Loki
AWS
Azure
IBM Cloud
GitHub
Jira
ServiceNow
Slack
```

------------------------------------------------------------------------

# 22. CrewAI + RAG

Agents can use enterprise runbooks through RAG.

``` text
Kubernetes Agent
       ↓
Company Runbooks
       ↓
RAG
       ↓
"How does our company handle
ImagePullBackOff?"
```

Architecture:

``` text
                    Agent
                      │
              ┌───────┴───────┐
              ↓               ↓
            Tools             RAG
              ↓               ↓
          Kubernetes      Runbooks
              │               │
              └───────┬───────┘
                      ↓
                     LLM
```

------------------------------------------------------------------------

# 23. CrewAI + MCP

Conceptually:

``` text
CrewAI Agent
     ↓
MCP Client
     ↓
MCP Server
     ↓
Tool / Data Source
```

Possible integrations:

``` text
GitHub
Kubernetes
Database
Internal APIs
Cloud APIs
```

------------------------------------------------------------------------

# 24. Interview Questions

### What is CrewAI?

> CrewAI is a Python framework for building agentic applications and
> multi-agent systems. The main concepts are Agents, Tasks, Tools, Crews
> and Flows.

### What is an Agent?

> An autonomous AI worker with a defined role, goal, context and tools.

### Agent vs Task?

> Agent is the worker; Task is the work assigned to the worker.

### What is a Crew?

> A Crew is a team of agents collaborating on a set of tasks.

### Crew vs Flow?

> A Crew focuses on autonomous agent collaboration. A Flow provides
> higher-level application orchestration, state management and control
> flow.

### Sequential vs Hierarchical?

> Sequential executes tasks in a defined order. Hierarchical introduces
> manager-style coordination between agents.

### Why use tools?

> Tools allow agents to interact with real systems such as Kubernetes,
> GitHub, databases, cloud APIs and monitoring platforms.

### How would you safely automate Kubernetes remediation?

> Separate investigation from remediation. Give agents read-only tools
> initially. Let the remediation agent recommend an action, then require
> human approval or a tightly controlled policy before destructive
> operations.

### Why not use one agent?

> Use multiple agents when responsibilities, tools, data sources or
> validation requirements are genuinely different. Multi-agent systems
> add latency, token usage and complexity, so they should be introduced
> only when they provide real value.

### How would you productionize CrewAI?

> Put a Flow around the application, use structured state and outputs,
> implement authentication and guardrails, use human approval for
> high-risk actions, add observability and evaluation, and deploy behind
> an API layer.

------------------------------------------------------------------------

# 25. One-Minute Interview Answer

> "CrewAI is a Python framework for building agentic applications and
> multi-agent systems. The main concepts are Agents, Tasks, Tools, Crews
> and Flows. An Agent is a specialized AI worker with a role and goal. A
> Task defines the work. Tools allow agents to interact with external
> systems. A Crew groups agents that collaborate on complex problems,
> while a Flow provides higher-level application orchestration, state
> management and control. For production systems, I would generally use
> a Flow as the application backbone and invoke a Crew for steps that
> benefit from autonomous collaboration."

------------------------------------------------------------------------

# 26. Quick Revision Cheat Sheet

``` text
AGENT
= AI specialist

TASK
= Work assigned to an agent

TOOL
= External capability/API/system

CREW
= Team of agents

PROCESS
= How crew tasks execute

FLOW
= Application orchestration + state + control

MEMORY
= Retained context/state

KNOWLEDGE
= Information available to agents

GUARDRAIL
= Validate/control AI behavior

STRUCTURED OUTPUT
= Predictable machine-readable result

DELEGATION
= Agent assigns work to another agent

HUMAN-IN-THE-LOOP
= Human approval before sensitive actions

OBSERVABILITY
= Trace agents, tasks, tools, LLM calls and failures
```

------------------------------------------------------------------------

# 27. Final Architecture

![CrewAI Kubernetes Incident Commander
Architecture](crewai-k8s-incident-architecture.png)

The complete flow:

``` text
                 USER / PRODUCTION ALERT
                           │
                           ▼
                    API / WEBHOOK
                           │
                           ▼
                AUTH + RATE LIMITING
                           │
                           ▼
                    CREWAI FLOW
              (state + orchestration)
                           │
                           ▼
                INCIDENT COMMANDER
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
         K8S AGENT      LOG AGENT    MONITORING
              │            │            │
              ▼            ▼            ▼
           kubectl      ELK/Loki    Prometheus
              │            │            │
              └────────────┼────────────┘
                           ▼
                    EVIDENCE / RAG
                           │
                           ▼
                  ROOT CAUSE AGENT
                           │
                           ▼
                 REMEDIATION AGENT
                           │
                           ▼
                  HUMAN APPROVAL
                           │
                           ▼
                 CONTROLLED ACTION
                           │
                           ▼
                  INCIDENT REPORT
                           │
                           ▼
              LOGGING / TRACING / METRICS
```

------------------------------------------------------------------------

# 28. Final Mental Model

``` text
                 FLOW
                  │
          "What happens next?"
                  │
                  ▼
                 CREW
                  │
          "Who should collaborate?"
                  │
          ┌───────┼────────┐
          ▼       ▼        ▼
        AGENT   AGENT    AGENT
          │       │        │
          ▼       ▼        ▼
        TOOLS   TOOLS    TOOLS
          │       │        │
          └───────┼────────┘
                  ▼
                 LLM
                  │
                  ▼
            Structured Result
                  │
                  ▼
          Guardrail / Approval
                  │
                  ▼
              Real Action
```

> **Interview memory trick:** Flow controls the application. Crew
> coordinates the team. Agent performs the role. Task defines the work.
> Tool connects the agent to the real world. LLM provides reasoning.
> Guardrails and human approval control risk. Observability tells us
> what happened.

------------------------------------------------------------------------

## Official references

-   [CrewAI Documentation](https://docs.crewai.com/)
-   [CrewAI GitHub](https://github.com/crewAIInc/crewAI)
-   [Agents](https://docs.crewai.com/en/concepts/agents)
-   [Tasks](https://docs.crewai.com/en/concepts/tasks)
-   [Crews](https://docs.crewai.com/en/concepts/crews)
-   [Flows](https://docs.crewai.com/en/concepts/flows)
-   [Production
    Architecture](https://docs.crewai.com/en/concepts/production-architecture)
