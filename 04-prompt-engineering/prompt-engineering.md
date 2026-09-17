# Prompt Engineering

## Original Class Notes

### Prompt Engineering

We need to provide proper prompt to get the better response. To provide the better prompt, use the below framework.

Role —> Give a role to AI model before providing the prompt, Act as Senior Cloud architect and design Netflix cloud architecture.
Task —> Define the task clearly, so that model understands it in better way
Context —> Explain the context to the models (i.e.., write a leave letter (without context), write a leave letter to my manager for my health issue and admitted in hospital in standard format (with context).
Without context AI guess the answer
Without context, AI provides the best solution
Few Shots —> Nothing but providing some example while prompting, so that AI models knows the exact need
Response / Report —> How you need the output from ChatGPT, like you need its in bullet points or JSON format or README format or content plan format

### Reverse prompting

Instead of writing a big prompt with RTCFP format, you can ask GPT to create a RTCFP format prompt, it will provide the best prompt and that can be used.

### Prompt Injection

Nothing but, using modified or unauthorised keywords with LLM models and breaking its security and privacy. So, still companies are working on it.

### JSON Prompting

When providing the input through JSON, system understands it clearly and output is far better. Token utilisation is low when comparing to text prompts.

### Scale Framework

This framework is best for image generation,
S —> Subject —> context of image
C —> Composition —> camera angle —> top view, selfie, side angle
A —> Action —> Type of action
L —> Location

## Additional Study Details

### RTCFR framework

Your notes use Role → Task → Context → Few Shots → Response/Report. The practical idea is to make the model's job, context, examples, and required output format explicit.

### Few-shot prompting

Examples are useful when you need a particular structure, tone, classification pattern, or output format. Good examples should represent the desired behavior rather than simply increasing prompt length.

### Prompt injection

Prompt injection is an attack where untrusted input attempts to manipulate an LLM or agent into ignoring intended instructions or performing unintended actions. For production systems, combine prompt design with input validation, tool permissions, output validation, least privilege, and other security controls.

### Structured/JSON prompting

Structured input and output can make application integration easier because fields have explicit meaning. JSON itself does not automatically guarantee lower token usage or better answers; token usage depends on the actual content and model.

### Image prompting — SCALE

Your notes define S=Subject, C=Composition, A=Action, L=Location. Keep this framework as the class-specific image-prompting method and add details/examples as you practice.

### Prompt engineering vs system design

Prompt engineering improves instructions and output behavior, but production AI quality also depends on model selection, retrieval, tools, context management, guardrails, evaluation, latency, cost, and observability.
