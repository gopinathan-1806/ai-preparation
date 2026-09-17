# AI Automation

## Original Class Notes

### AI Automation

3 types
1. ETL Automation
2. API automation
3. UI Automation

### AI Automation Platforms

No code —> n8n, make, Zapier (n8n is the only open source free tool, others are paid one)
Code —> Playwright, playGUI, selenium

### n8n Installation methods

1. Local installation —> download the package and run in our local
2. Self hosted method —> Best method —> use Cloud VMs for hosting
3. Production n8n —> n8n.io —> but very expensive

### Types of nodes in n8n

1. Trigger node
2. Data flow node
3. Conditional node
4. Action
5. AI node (LLM, AI Agent)

### Trigger nodes

1. HTTP node —> Whenever we need, we should called HTTP
2. Webhook node —> Any changes in the system, web hook request will be initiated

### Authentication vs Authorization

Authentication —> If you can login to AWS console or not
Authorization —> After login to AWS, can you create EC2

### Types of authentication

1. API key —> Full access
2. Token access —> Token will be created with set of permissions like user need to read only this repo, so user can read only that repo.
3. Auth token —> High authentication, client ID and server will be created for validation

### UI Automation

If there is an issue with API and ETL automation, then we need to use this UI automation, this will run in our local laptop. For UI automation, we need web scraping to collect the data from internet.

Scrapling, Market Down, Scrapy —> Tools available to perform the scraping.

## Additional Study Details

### Automation decision tree

Prefer an API when a reliable supported API exists. Use ETL/data pipelines for scheduled movement and transformation of data. Use browser/UI automation when the workflow genuinely requires browser interaction and no suitable API exists.

### n8n

n8n represents workflows as connected nodes. Triggers start workflows; transformation/data-flow nodes process information; conditional nodes branch; action nodes call external systems; AI nodes can invoke models or agents.

### HTTP vs webhook

An HTTP request is an interaction initiated by a caller. A webhook is a callback/event mechanism where one system sends an HTTP request to another system when an event occurs.

### Authentication vs authorization

Authentication establishes identity; authorization determines permissions. An API key does not inherently mean full access—its privileges depend on how the server validates and scopes it. Tokens can also carry different scopes/permissions.

### UI automation and scraping

Browser automation tools such as Playwright can navigate pages, fill forms, click controls, and extract data. Scraping should respect site terms, access controls, robots policies where applicable, rate limits, and privacy requirements.

### Production automation concerns

For production workflows, think about retries, idempotency, timeouts, secrets management, logging, alerting, error handling, dead-letter/manual-review paths, rate limits, and auditability.
