# LLM Fundamentals

## Original Class Notes

### LLM

LLM —> Large Language Model is a type of artificial intelligence program trained on massive amounts of text data to understand, summarize, translate, and generate human-like language.

It has 5 stages of processing, each word is considered as token ?
Eg. Write a python program ? —> 5 token will be used to process this.

### Types of LLMs

LMM —> Large Multimodel —> used for image / video generation —> nano banana
SLM —> Small Langage Model —> Enterprise uses this
VLM —> Vision Language model

### Open source LLM vs Cloud / Closed LLM

Opensource LLM means nothing but the free LLM models available in the market, we can download it locally and optimise it for our need, which will be running on our laptop or our server. Ollama and Hugging Face is the place where we can download our models.

Cloud / Closed LLM nothing but the models owned by companies called OpenAI, Google, we cannot download the model instead we can work with that.

## Additional Study Details

### Tokens

LLMs process tokenized text rather than simply treating every word as one token. A token can be a whole word, part of a word, punctuation, or another token-unit. Tokenization affects context limits, latency, and cost.

### Conceptual inference flow

A useful mental model is: input text → tokenization → model processing through transformer layers → next-token probability generation → decoding/detokenization into readable output. This is a conceptual learning model rather than a claim that the original five-stage diagram is universal.

### Model categories

VLM means Vision-Language Model. Multimodal models can process or generate multiple modalities such as text, image, audio, or video depending on the model. SLM usually refers to smaller language models that trade some capability for lower compute, latency, or deployment cost.

### Open-weight vs proprietary

Do not equate 'open source' with 'free'. Some models provide downloadable weights under licenses with different restrictions; proprietary/cloud models are typically accessed through hosted APIs or products. Always check the model license before commercial use or modification.

### Local model tooling

Ollama is commonly used to run supported models locally, while Hugging Face provides model repositories and tooling. Local inference introduces practical considerations such as RAM/VRAM, quantization, model size, latency, and hardware capacity.
