# Pressure Testing: Open LLMs 💢

This is derivative work of [Needle In A Haystack - Pressure Testing LLMs](https://github.com/gkamradt/LLMTest_NeedleInAHaystack), a project where @gkamradt explored the in-context retrieval abilities of GPT-4 and Claude 2. I was impressed by the insights gained from this test, and as an open-source enthusiast, I felt compelled to extend the experiment to the broader open-source LLM market. As such, this project examines the in-context retrieval capabilities of popular open-source models. My primary aim is to evaluate how these widely-used models in the LLM community perform in terms of simple retrieval within their context window. I welcome suggestions for additional models to include in our study, particularly those with larger context windows and the ability to run with 24GB VRAM + 64GB RAM.

**Note:** As a response to @gkamradt's work, Anthropic ran their own pressure tests, covered in [this](https://www.anthropic.com/index/claude-2-1-prompting) blog post. They were able to massivively improve in-context retrieval performance by priming the model response with `Here is the most relevant sentence in the text:`. All my tests using this retrieval priming technique will be suffixed with `rp`.

## The Test 📝

1. Place a random fact or statement (the 'needle') in the middle of a long context window (the 'haystack')
2. Ask the model to retrieve this statement using the following prompt format

```
You are provided with a text of some essays, admist these essays is a sentence
that contains the answer to the user's question. I will now provide the text
(delimited with XML tags) followed by the user question.

[TEXT]
{content}
[/TEXT]


User: {prompt}

Assistant: {retrival primer}
```

3. Iterate over various document depths (where the needle is placed) and context lengths to measure performance

## Roadmap 🛣️

An ongoing list of models to pressure test.

```
1. Mistral 7B Instruct v0.2
```

## Results 📊

Each test consists of a retrieval, at certain depth percentage, for a given context length. The results are combined into a pivot table illustrating how well the model response was, judged by GPT-4. The scoring system is defined as

```
Score 1: The answer is completely unrelated to the reference.
Score 3: The answer has minor relevance but does not align with the reference.
Score 5: The answer has moderate relevance but contains inaccuracies.
Score 7: The answer aligns with the reference but has minor omissions.
Score 10: The answer is completely accurate and aligns perfectly with the reference.
```

I have slightly adjusted @gkamradt's visualization code to work for this project. The code can be found [here](/utils/visualize.ipynb). The raw results are found in `results/`.

### Qwen-1.5-4B @ 7k [RP]

Qwen doesn't have any attention optimizations (SHA, MHA, MQA, GQA), hence scaling contexts is super expensive in terms of VRAM :( 

![](/img/qwen1.5-4b-rp.png)

### Qwen-1.5-7B @ 7k [RP]

Wish I could test how well this does at higher contexts, all Qwen 1.5 support contexts up to 32k in practice.

![](/img/qwen1.5-7b.png)

### Mistral-7B-Instruct-v0.2 @ 16k

This model is trained on 8k context but features a theoretical context window of up to 128k, made possible through sliding window attention.

![](/img/mistral_7b_16k.png)

### Mistral-7B-Instruct-v0.2 @ 16k [RP]

Using the retrieval priming technique from Anthropic, results improve **tremendously**.The model is capable of handling contexts exceeding 8k. However, its performance is characterized by volatility; it tends to either achieve flawless success or encounter complete failure.

![](/img/mistral_7b_16k_rp.png)

### OpenChat 7B 3.5-1210 @ 8k

![](/img/openchat-3.5-1210_8k.png)

### OpenChat 7B 3.5-1210 @ 8k [RP]

![](/img/openchat-3.5-1210_8k_rp.png)

### Starling LM 7B Alpha @ 8k

Starling is finetuned from OpenChat 3.5 and is one of the best 7B models on Chatbot Arena.

![](/img/starling-lm-7b-alpha.png)

### Starling LM 7B Alpha @ 8k [RP]

![](/img/starling-lm-7b-alpha_rp.png)

### Toppy 7B @ 16k

![](/img/toppy-7b.png)

## Implementation

Just a quick note on the implementation. @gkamradt refactored and cleaned the code significantly since I originally started working on this. I don't plan to sync this with his more polished version. This code works fine but it's hacky.
