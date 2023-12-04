# Pressure testing open-source instruction finetuned LLMs
This is derivative work of [Needle In A Haystack - Pressure Testing LLMs](https://github.com/gkamradt/LLMTest_NeedleInAHaystack) with a implementation for open-sourced models. Credit goes to him for laying out the groundwork! My goal is to test how well commonly used models in the LLM community fair in simple retrieval within their context window. From @gkamradt's work it's clear that Claude and GPT-4 are inconsistent in how well they handle their context window; but what about our favorite OS models?

## The Test
1. Place a random fact or statement (the 'needle') in the middle of a long context window (the 'haystack')
2. Ask the model to retrieve this statement
3. Iterate over various document depths (where the needle is placed) and context lengths to measure performance

## Roadmap 
An ongoing list of models to pressure test. 

```
1. Vicuna-13B-v1.5-16K
2. Vicuna-7B-v1.5-16K
3. OpenHermes-2.5-Mistral-7B-16k
4. Openchat_3.5-16k
5. Llama-2-7B-32K-Instruct
```

## System
I have been running through Colab but I should soon have access to a 4090 which should make things a lot easier. Unfortunately
I will be heavily limited in the models I can test due to VRAM. 
