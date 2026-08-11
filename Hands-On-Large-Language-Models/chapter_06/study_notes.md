# 📘 Chapter 6 Study Bundle: Prompt Engineering
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 6

---

## §1. Study Notes

### Core Theme
This chapter explores generative models in depth through prompt engineering. It covers the basics of using text generation models (model selection, loading via transformers, controlling output with temperature/top_p/top_k), the fundamental and advanced components of prompt engineering (instruction-based prompting, specificity, persona/context/format/audience/tone/data), in-context learning (zero-shot/one-shot/few-shot), chain prompting (breaking problems up between prompts), reasoning techniques with generative models (chain-of-thought, zero-shot chain-of-thought, self-consistency, tree-of-thought), and output verification (structured output, valid output, ethics, accuracy — controlled via examples, grammar/constrained sampling, or fine-tuning).

### Key Definitions
- **Generative pre-trained transformers (GPT):** Models trained primarily for text generation with the ability to generate text in response to prompts.
- **Prompt engineering:** Designing prompts to enhance the quality of generated text; also a tool to evaluate output and design safeguards; an iterative process of prompt optimization requiring experimentation.
- **Prompt:** The input to an LLM (a question, statement, or instruction) used to elicit a useful response.
- **Foundation model:** An LLM pretrained on vast amounts of text data, often fine-tuned for specific applications; often released in several sizes.
- **Proprietary model:** A closed model (e.g., GPT-4); generally more performant.
- **Open source model:** A freely usable model offering more flexibility; the book's focus.
- **Chat template:** The prompt template (with special tokens like `<|user|>`, `<|assistant|>`, `<|end|>`) used during training that indicates who said what and when to stop generating.
- **Temperature:** Controls the randomness/creativity of generated text; defines how likely less-probable tokens are chosen. 0 = always most likely word (same response); higher (e.g., 0.8) = more diverse; lower (e.g., 0.2) = more deterministic.
- **top_p (nucleus sampling):** A sampling technique that controls which subset of tokens (the nucleus) the LLM can consider — until their cumulative probability reaches top_p. Lower = fewer tokens/less creative; 1 = all tokens.
- **top_k:** Controls exactly how many tokens the LLM can consider (e.g., top 100 most probable).
- **do_sample:** Parameter controlling whether sampling is done; `do_sample=False` selects only the most probable token (deterministic); `True` enables temperature/top_p.
- **Hallucination:** LLMs generating incorrect information confidently; reduced by asking the model to only answer if it knows.
- **Primacy effect:** LLMs tend to focus on information at the beginning of a prompt.
- **Recency effect:** LLMs tend to focus on information at the end of a prompt.
- **Specificity:** Accurately describing what you want to achieve — the most important aspect of prompt design.
- **Persona:** Describing what role the LLM should take on (e.g., "You are an expert in astrophysics").
- **Instruction:** The task itself; make it as specific as possible.
- **Context:** Additional information describing the context of the problem/task; answers "What is the reason for the instruction?"
- **Format:** The format the LLM should use to output text; without it, the LLM chooses its own format (troublesome for automated systems).
- **Audience:** The target of the generated text; describes the output level (e.g., ELI5 = "Explain it like I'm 5").
- **Tone:** The tone of voice the LLM should use in the generated text.
- **Data:** The main data related to the task itself.
- **Emotional stimuli:** Prompt components like "This is very important for my career." (from EmotionPrompt, Li et al., 2023).
- **In-context learning:** Providing the LLM with correct examples instead of describing the task (Brown et al., 2020 — "Language models are few-shot learners").
- **Zero-shot prompting:** Prompting without any examples.
- **One-shot prompting:** Prompting with a single example.
- **Few-shot prompting:** Prompting with two or more examples.
- **Chain prompting:** Breaking a problem up between prompts — taking the output of one prompt as input for the next, creating a chain of interactions.
- **Response validation:** Ask the LLM to double-check previously generated outputs.
- **Parallel prompts:** Create multiple prompts in parallel and do a final pass to merge them.
- **System 1 thinking:** Automatic, intuitive, near-instantaneous process (Kahneman, 2011); resembles generative models generating tokens without self-reflection.
- **System 2 thinking:** Conscious, slow, logical process; akin to brainstorming and self-reflection.
- **Chain-of-thought (CoT):** A method to have the generative model "think" first — providing examples that demonstrate reasoning before the response (Wei et al., 2022).
- **Zero-shot chain-of-thought:** Asking the model to reason without examples, commonly via "Let's think step-by-step" (Kojima et al., 2022). Alternatives: "Take a deep breath and think step-by-step" (Yang et al., 2023, "Large language models as optimizers").
- **Self-consistency:** Asking the generative model the same prompt multiple times and taking the majority result as the final answer (Wang et al., 2022). Can be combined with chain-of-thought; becomes n times slower.
- **Tree-of-thought (ToT):** Breaking a problem into pieces and, at each step, prompting the model to explore different solutions, voting for the best, and continuing (Yao et al., 2023). Zero-shot version emulates a conversation between multiple experts who question each other until consensus.
- **Output verification:** Verifying/controlling model output for production robustness; reasons include structured output, valid output, ethics, and accuracy.
- **Constrained sampling (grammar):** Defining grammars/rules the LLM must adhere to when choosing its next token; packages include Guidance, Guardrails, LMQL.
- **Quantization:** Compression of models (see Chapter 12); GGUF format is generally used for compressed (quantized) models.
- **llama-cpp-python:** A library similar to transformers used to efficiently load compressed models (GGUF format); can apply a JSON grammar via `response_format={"type": "json_object"}`.

### Core Concepts & Frameworks
- **Choosing a model:** Choose between proprietary and open source models; book focuses on open source for flexibility and free use. Hundreds/thousands of fine-tuned models exist. Advice: start with a small foundation model — Phi-3-mini (3.8 billion parameters, runs on devices up to 8 GB VRAM). Scaling up is nicer than scaling down.
- **Loading a model:** `AutoModelForCausalLM.from_pretrained("microsoft/Phi-3-mini-4k-instruct", device_map="cuda", torch_dtype="auto", trust_remote_code=True)` + `AutoTokenizer` + `pipeline("text-generation", ..., return_full_text=False, max_new_tokens=500, do_sample=False)`.
- **Prompt template processing:** `pipe.tokenizer.apply_chat_template(messages, tokenize=False)` shows the underlying template: `<s><|user|> ... <|end|> <|assistant|>`. The template was used during training, provides information about who said what, and indicates when to stop (via `<|end|>`). transformers.pipeline handles chat template processing for the user.
- **Controlling output via parameters:**
  - `do_sample=False` → deterministic (only most probable token).
  - High temperature (e.g., 1) → diverse, creative, stochastic (output changes every run).
  - Low temperature (e.g., 0.2) → deterministic, conservative.
  - `top_p` (nucleus sampling): 0.1 → considers tokens until cumulative probability 0.1; 1 → all tokens. Lower = fewer tokens/less creative.
  - `top_k=100` → only the top 100 most probable tokens.
  - Table 6-1 combinations: Brainstorming (High/High — diverse, creative, unexpected), Email generation (Low/Low — predictable, focused, conservative), Creative writing (High/Low — creative but coherent), Translation (Low/High — coherent with wider vocabulary/linguistic variety).
- **Basic ingredients of a prompt:** An LLM is a prediction machine — a few words suffice to elicit a response, but a specific task needs a structured prompt: instruction + data, and possibly output indicators (e.g., prefix "Text:" and add "Sentiment:" to force "negative"/"positive"). Components are merely examples — the creativity of designing them is key. Think of prompts as pieces of a larger puzzle.
- **Instruction-based prompting:** Using prompts to have the LLM answer a specific question or resolve a task (e.g., classification). Use cases require different prompting formats/questions.
- **Prompting techniques (non-exhaustive):** Specificity (most important — restrict and specify what the model should generate); Hallucination mitigation (ask "only answer if you know"); Order (begin or end with the instruction; middle info is often forgotten due to primacy/recency effects).
- **Advanced prompt components:** Persona, Instruction, Context, Format, Audience, Tone, Data — demonstrated in a complex summary prompt (variables persona, instruction, context, data_format, audience, tone, text, data; query = concatenation). Prompting is modular — add/remove components, judge effect, iterate. Emotional stimuli can help. Note: prompts work differently per model (different training data/purposes); it's an attempt to reverse engineer what the model learned.
- **In-context learning:** Show the task instead of describing it. Zero-shot (no examples), one-shot (1 example), few-shot (2+). Example: made-up word "Gigamuru"/"screeg" — user/assistant message roles differentiate question from answer. Even with examples, the model can "choose" to ignore instructions via random sampling.
- **Chain prompting:** Break the problem up between prompts: product features → name+slogan → sales pitch. Benefit: each call can have different parameters (e.g., short name/slogan vs. long pitch). Use cases: response validation, parallel prompts (merge via final pass), writing stories. Next chapter automates this with memory, tool use, etc. Chain-of-thought, self-consistency, and tree-of-thought are more complex chaining methods.
- **Reasoning with generative models:** LLM reasoning is generally considered emergent behavior from memorization of training data and pattern matching (not "true" reasoning), but is referred to as reasoning capabilities. System 1 thinking ≈ automatic token generation; system 2 thinking ≈ self-reflection/brainstorming; prompting mimics system 2.
- **Chain-of-thought:** Provide reasoning examples ("thoughts") in the prompt before the answer; helps complex tasks (e.g., math). Each additional reasoning token stabilizes output. Example: Roger's tennis balls / cafeteria apples. Model provides explanation before the answer, leveraging generated knowledge.
- **Zero-shot chain-of-thought:** Append "Let's think step-by-step" to get reasoning without examples. Alternatives exist ("Take a deep breath and think step-by-step", "Let's work through this problem step-by-step").
- **Self-consistency:** Ask the same prompt multiple times (with varying temperature/top_p for diversity), take majority result. Can add chain-of-thought, only using the answer for voting. Disadvantage: n times slower (n = number of samples).
- **Tree-of-thought:** Explore multiple reasoning paths (tree-based structure); at each step, generate intermediate thoughts to be rated, keep the most promising, prune the lowest. Helpful for multiple paths (stories, creative ideas). Disadvantage: many calls to the model (slower). Zero-shot ToT: emulate a conversation between multiple experts who question each other until consensus (single prompt).
- **Output verification reasons:** Structured output (e.g., JSON — by default models create free-form text); Valid output (should not come up with a third option when asked for one of two); Ethics (no guardrails in some open source models → profanity, PII, bias, cultural stereotypes); Accuracy (factually accurate, coherent, free from hallucination).
- **Three ways to control output:** (1) Examples — provide examples of the expected output; (2) Grammar — control the token selection process (constrained sampling); (3) Fine-tuning — tune a model on expected-output data (Chapter 12).
- **Providing examples:** Zero-shot JSON request yielded truncated invalid JSON (model stopped after "charisma"); one-shot with a format template yielded perfectly structured JSON. Model still chooses whether to adhere to the format — some models follow instructions better.
- **Grammar: constrained sampling:** Packages like Guidance, Guardrails, LMQL constrain/validate output; some leverage LLMs to validate their own output (retrieve output as new prompts, validate against predefined guardrails). Validation can control formatting by generating parts of the format ourselves. During token sampling, grammars/rules constrain the next-token choice (e.g., only "positive"/"neutral"/"negative" for sentiment). Still affected by top_p/temperature.
- **llama-cpp-python example:** Clear VRAM (`del model, tokenizer, pipe; gc.collect(); torch.cuda.empty_cache()`), load Phi-3 GGUF (`Llama.from_pretrained(repo_id="microsoft/Phi-3-mini-4k-instruct-gguf", filename="*fp16.gguf", n_gpu_layers=-1, n_ctx=2048, verbose=False)`), generate with `response_format={"type": "json_object"}` and `temperature=0`, verify with `json.dumps(json.loads(output), indent=4)`.
- **Chapter theme:** Prompt engineering is a crucial aspect of working with LLMs — it allows us to effectively communicate needs and preferences, unlock LLM potential, and generate high-quality responses.
- **Next chapter:** Explore more advanced techniques — how LLMs can use external memory and tools.

### Important Numbers / Stats / Tokens
- Phi-3-mini: 3.8 billion parameters; runs on devices up to 8 GB VRAM (p.2).
- Pipeline: `max_new_tokens=500`, `do_sample=False` initially (p.2-3).
- Chat template tokens: `<s>`, `<|user|>`, `<|assistant|>`, `<|end|>` (p.3).
- Temperature examples: high ~1.0 (diverse), 0.8 (more diverse), 0.2 (deterministic); 0 = same response every time (p.5).
- top_p: 0.1 considers tokens until cumulative probability 0.1; 1 = all tokens (p.5).
- top_k: e.g., 100 = only top 100 most probable tokens (p.6).
- Table 6-1 use cases: Brainstorming (High/High), Email generation (Low/Low), Creative writing (High/Low), Translation (Low/High) (p.6).
- In-context learning: zero-shot (0 examples), one-shot (1), few-shot (2+) (p.14-15).
- Chain prompting example: product name+slogan "MindMeld Messenger" / "Unleashing Intelligent Conversations, One Response at a Time", then sales pitch (p.17-18).
- CoT example: Roger 5 balls + 2 cans of 3 = 11; cafeteria 23 apples − 20 + 6 = 9 (p.20).
- Zero-shot CoT phrase: "Let's think step-by-step" (p.21); alternatives "Take a deep breath and think step-by-step", "Let's work through this problem step-by-step" (p.22).
- Self-consistency: n times slower (n = number of output samples) (p.23).
- One-shot RPG JSON template: keys description, name, armor, weapon (p.27).
- llama-cpp-python: `n_gpu_layers=-1` (all layers on GPU), `n_ctx=2048`, `repo_id="microsoft/Phi-3-mini-4k-instruct-gguf"`, `filename="*fp16.gguf"`, `response_format={"type": "json_object"}`, `temperature=0` (p.29-30).

### Algorithms & Formulæ
- **Token generation:** Each token assigned a likelihood score; the model chooses the next token based on likelihood scores. With `do_sample=False`, only the most probable next token is selected.
- **Temperature:** Higher → more likely to choose less probable tokens (more diverse); lower → more deterministic.
- **top_p (nucleus sampling):** Consider tokens until their cumulative probability reaches top_p; lower value = fewer tokens.
- **top_k:** Consider only the top-k most probable tokens.
- **Chain-of-thought:** Prompt with reasoning examples → model "thinks" (generates reasoning tokens) before answering; each reasoning token stabilizes output.
- **Zero-shot CoT:** Append "Let's think step-by-step" to prime reasoning.
- **Self-consistency:** Same prompt sampled n times (with diversity); majority vote = final answer.
- **Tree-of-thought:** At each step, generate multiple intermediate thoughts, rate them, keep best, prune lowest. Zero-shot variant: single prompt emulating multi-expert consensus conversation.
- **Constrained sampling:** Define grammar/rules for next-token selection (e.g., JSON grammar via `response_format={"type": "json_object"}`); still affected by top_p/temperature.

### Diagrams / Visuals
- **Figure 6-1** — Foundation models are often released in several different sizes.
- **Figure 6-2** — The template Phi-3 expects when interacting with the model (`<s><|user|>...<|end|><|assistant|>`).
- **Figure 6-3** — The model chooses the next token based on likelihood scores ("I am driving a…" → car/truck higher, elephant lower).
- **Figure 6-4** — Higher temperature increases the likelihood that less probable tokens are generated.
- **Figure 6-5** — Higher top_p increases the number of tokens that can be selected.
- **Figure 6-6** — A basic example of a prompt (no instruction → LLM completes the sentence).
- **Figure 6-7** — Two components of a basic instruction prompt: the instruction and the data it refers to.
- **Figure 6-8** — Extending the prompt with an output indicator (Text: … Sentiment: …) for a specific output.
- **Figure 6-9** — Use cases for instruction-based prompting.
- **Figure 6-10** — Prompt examples of common use cases (structure/location of instruction can change).
- **Figure 6-11** — An example of a complex prompt with many components (persona, instruction, context, format, audience, tone, data).
- **Figure 6-12** — Iterating over modular components is a vital part of prompt engineering.
- **Figure 6-13** — In-context learning: zero-shot, one-shot, and few-shot prompting.
- **Figure 6-14** — Chain prompts: product features → name → slogan → sales pitch.
- **Figure 6-15** — Chain-of-thought prompting uses reasoning examples to persuade the model to reason.
- **Figure 6-16** — Zero-shot chain-of-thought: "Let's think step-by-step" primes reasoning.
- **Figure 6-17** — Self-consistency: sampling from multiple reasoning paths, majority voting extracts the most likely answer.
- **Figure 6-18** — Tree-of-thought: tree-based structure generates intermediate thoughts to be rated; best kept, lowest pruned.
- **Figure 6-19** — Use an LLM to check whether the output correctly follows our rules (guardrails).
- **Figure 6-20** — Use an LLM to generate only the pieces of information we do not know beforehand.
- **Figure 6-21** — Constrain the token selection to only three possible tokens: "positive," "neutral," and "negative."

### Common Exam Traps
- **Model selection:** The chapter uses Phi-3-mini (3.8B params, up to 8 GB VRAM), NOT a proprietary model; open source favored for flexibility/free use. Scaling up is nicer than scaling down; start small.
- **do_sample=False means deterministic** — only most probable token selected; temperature/top_p require do_sample=True.
- **Temperature:** 0 → same response every time (always most likely word); higher (e.g., 0.8/1) → more diverse; lower (0.2) → deterministic. Temperature introduces stochastic behavior — output changes every run.
- **top_p vs top_k:** top_p = cumulative-probability nucleus (subset until cumulative prob); top_k = exact number of tokens. Lower top_p → fewer tokens, less creative. top_p=1 → all tokens.
- **Table 6-1 combos:** Brainstorming High/High; Email Low/Low; Creative writing High/Low (creative but coherent); Translation Low/High (coherent, wider vocabulary). Don't confuse Creative writing (High/Low) with Translation (Low/High).
- **Prompt order effect:** info in the middle is often forgotten; begin or end with the instruction (primacy/recency effects).
- **Specificity is the most important aspect** of prompt design — restricting/specifying output reduces off-target generation.
- **Hallucination mitigation:** ask the model to only answer if it knows, else respond "I don't know" — NOT eliminate hallucinations entirely.
- **In-context learning counts:** zero-shot = 0 examples, one-shot = 1, few-shot = 2+.
- **Chain prompting:** breaks the problem up BETWEEN prompts (output of one = input of next), not within a single prompt; each call can have different parameters.
- **LLM "reasoning"** is generally emergent behavior via memorization + pattern matching, not "true" reasoning.
- **System 1 vs System 2:** System 1 = automatic/intuitive/near-instantaneous (like plain token generation); System 2 = conscious/slow/logical (brainstorming, self-reflection). Prompting mimics System 2.
- **Chain-of-thought:** requires examples of reasoning; zero-shot CoT uses "Let's think step-by-step" without examples.
- **Self-consistency:** majority vote over multiple samples; n times slower; can combine with CoT (only answers voted).
- **Tree-of-thought:** explores multiple paths, rates intermediate thoughts, keeps best/prunes lowest; many model calls (slower); zero-shot variant = multi-expert consensus conversation in one prompt.
- **Output verification reasons:** structured output (JSON), valid output (not a third option), ethics (profanity/PII/bias/stereotypes), accuracy (factual, coherent, no hallucination).
- **Three ways to control output:** Examples, Grammar (constrained sampling), Fine-tuning (Ch 12). NOT temperature/top_p — those are sampling parameters, not output-control methods per se (though they affect generation).
- **Grammar packages:** Guidance, Guardrails, LMQL. llama-cpp-python applies a JSON grammar via `response_format={"type": "json_object"}`.
- **GGUF format** is used for compressed (quantized) models loaded by llama-cpp-python; `n_gpu_layers=-1` = all layers on GPU; `n_ctx` = context size.
- **Zero-shot JSON output may be truncated/invalid** — providing a one-shot format template yields consistent structured output; but the model still chooses whether to adhere.
- **Constrained sampling is still affected** by top_p and temperature.

### Chapter Summary
Chapter 6 explores generative models and prompt engineering. It starts with using text generation models: choosing open source over proprietary (Phi-3-mini, 3.8B params), loading via transformers, and controlling output through temperature (randomness), top_p (nucleus sampling), and top_k. Prompt engineering basics cover the ingredients of a prompt (instruction, data, output indicators), instruction-based prompting, and techniques like specificity, hallucination mitigation, and prompt ordering (primacy/recency effects). Advanced topics include modular prompt components (persona, instruction, context, format, audience, tone, data), in-context learning (zero-shot/one-shot/few-shot), and chain prompting (breaking problems between prompts). The chapter then covers reasoning: chain-of-thought, zero-shot chain-of-thought ("Let's think step-by-step"), self-consistency (majority voting), and tree-of-thought (multiple reasoning paths or multi-expert consensus). Finally, output verification addresses structured output, valid output, ethics, and accuracy via examples and grammar-based constrained sampling (e.g., JSON grammar with llama-cpp-python). The next chapter explores external memory and tools for LLMs.

### Confidence Check
- **Sure**: Phi-3-mini specs (3.8B params, 8 GB VRAM); loading via AutoModelForCausalLM + pipeline; chat template tokens (<s>, <|user|>, <|assistant|>, <|end|>); temperature semantics (0 deterministic, higher = more diverse); top_p nucleus sampling mechanics; top_k exact count; Table 6-1 combos; prompt components (persona/instruction/context/format/audience/tone/data); specificity importance; primacy/recency effects; in-context learning shot counts; chain prompting mechanics; system 1/2 thinking; CoT, zero-shot CoT ("Let's think step-by-step"), self-consistency (majority vote, n× slower), tree-of-thought (rate/prune; zero-shot expert consensus); output verification reasons (structured output, valid output, ethics, accuracy); three control methods (examples, grammar, fine-tuning); Guidance/Guardrails/LMQL packages; llama-cpp-python GGUF loading and JSON grammar.
- **Uncertain**: exact page anchors for some figures (PDF text flow approximate); precise quoted wording where extraction broke lines mid-sentence; the exact original product/sales-pitch text length (paraphrased from extraction).

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading a text generation model
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline
# Load model and tokenizer
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
# Create a pipeline
pipe = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=False,
    max_new_tokens=500,
    do_sample=False,
)
```
- **Explanation:** Loads Phi-3-mini-4k-instruct on GPU with automatic dtype, plus its tokenizer, and wraps them in a text-generation pipeline with return_full_text=False, max 500 new tokens, and deterministic sampling off.
- **Fits the architecture:** The standard way to load and run an open source causal LM for prompt-based generation.

### Code Block 2: Basic prompt and generation
```python
# Prompt
messages = [
    {"role": "user", "content": "Create a funny joke about chickens."}
]
# Generate the output
output = pipe(messages)
print(output[0]["generated_text"])
```
- **Explanation:** Passes a simple user message to the pipeline; the model responds with a joke.
- **Fits the architecture:** Messages with roles (user/assistant) are the standard chat format.

### Code Block 3: Inspecting the chat template
```python
# Apply prompt template
prompt = pipe.tokenizer.apply_chat_template(messages, tokenize=False)
print(prompt)
```
- **Explanation:** Shows the underlying prompt template the pipeline builds: `<s><|user|>...<|end|><|assistant|>`.
- **Fits the architecture:** The template was used during training, encodes who said what, and signals where to stop (`<|end|>`).

### Code Block 4: High temperature generation
```python
# Using a high temperature
output = pipe(messages, do_sample=True, temperature=1)
print(output[0]["generated_text"])
```
- **Explanation:** Enables sampling with temperature 1 → more diverse, creative output that changes on every run.
- **Fits the architecture:** Temperature controls the randomness/creativity of token selection.

### Code Block 5: High top_p generation
```python
# Using a high top_p
output = pipe(messages, do_sample=True, top_p=1)
print(output[0]["generated_text"])
```
- **Explanation:** Enables sampling with top_p=1 → considers all tokens (most flexible nucleus).
- **Fits the architecture:** top_p (nucleus sampling) controls the subset of tokens the LLM can consider.

### Code Block 6: The complex prompt (all components)
```python
# Prompt components
persona = "You are an expert in Large Language models. You excel at breaking 
down complex papers into digestible summaries.\n"
instruction = "Summarize the key findings of the paper provided.\n"
context = "Your summary should extract the most crucial points that can help 
researchers quickly understand the most vital information of the paper.\n"
data_format = "Create a bullet-point summary that outlines the method. Follow 
this up with a concise paragraph that encapsulates the main results.\n"
audience = "The summary is designed for busy researchers that quickly need to 
grasp the newest trends in Large Language Models.\n"
tone = "The tone should be professional and clear.\n"
text = "MY TEXT TO SUMMARIZE"
data = f"Text to summarize: {text}"
# The full prompt - remove and add pieces to view its impact on the generated 
output
query = persona + instruction + context + data_format + audience + tone + data
```
- **Explanation:** Builds a complex prompt from modular components: persona, instruction, context, data_format, audience, tone, and data — concatenated into a single query.
- **Fits the architecture:** Prompting is modular; add/remove components and observe their effect on output.

### Code Block 7: One-shot in-context learning (made-up word)
```python
# Use a single example of using the made-up word in a sentence
one_shot_prompt = [
    {
        "role": "user",
        "content": "A 'Gigamuru' is a type of Japanese musical instrument. An 
example of a sentence that uses the word Gigamuru is:"
    },
    {
        "role": "assistant",
        "content": "I have a Gigamuru that my uncle gave me as a gift. I love 
to play it at home."
    },
    {
        "role": "user",
        "content": "To 'screeg' something is to swing a sword at it. An example 
of a sentence that uses the word screeg is:"
    }
]
print(tokenizer.apply_chat_template(one_shot_prompt, tokenize=False))
# Generate the output
outputs = pipe(one_shot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Provides one example (user → assistant) of using a made-up word, then asks for a new one; the model correctly generates a sentence using "screeg".
- **Fits the architecture:** Differentiating user/assistant roles in the template makes in-context learning work.

### Code Block 8: Chain prompting — name and slogan
```python
# Create name and slogan for a product
product_prompt = [
    {"role": "user", "content": "Create a name and slogan for a chatbot that 
leverages LLMs."}
]
outputs = pipe(product_prompt)
product_description = outputs[0]["generated_text"]
print(product_description)
```
- **Explanation:** First prompt creates a product name and slogan ("MindMeld Messenger" / "Unleashing Intelligent Conversations...").
- **Fits the architecture:** Output of one prompt becomes input for the next.

### Code Block 9: Chain prompting — sales pitch
```python
# Based on a name and slogan for a product, generate a sales pitch
sales_prompt = [
    {"role": "user", "content": f"Generate a very short sales pitch for the 
following product: '{product_description}'"}
]
outputs = pipe(sales_prompt)
sales_pitch = outputs[0]["generated_text"]
print(sales_pitch)
```
- **Explanation:** Uses the previous output (name + slogan) to generate a sales pitch. Two calls, each with different parameters.
- **Fits the architecture:** Chaining lets the LLM spend more time on each individual question.

### Code Block 10: Chain-of-thought prompting
```python
# Answering with chain-of-thought
cot_prompt = [
    {"role": "user", "content": "Roger has 5 tennis balls. He buys 2 more cans 
of tennis balls. Each can has 3 tennis balls. How many tennis balls does he 
have now?"},
    {"role": "assistant", "content": "Roger started with 5 balls. 2 cans of 3 
tennis balls each is 6 tennis balls. 5 + 6 = 11. The answer is 11."},
    {"role": "user", "content": "The cafeteria had 23 apples. If they used 20 
to make lunch and bought 6 more, how many apples do they have?"}
]
# Generate the output
outputs = pipe(cot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Shows a worked reasoning example, then asks a new math question; the model explains before answering (23 − 20 = 3, + 6 = 9).
- **Fits the architecture:** Reasoning examples persuade the model to think before responding.

### Code Block 11: Zero-shot chain-of-thought
```python
# Zero-shot chain-of-thought
zeroshot_cot_prompt = [
    {"role": "user", "content": "The cafeteria had 23 apples. If they used 20 
to make lunch and bought 6 more, how many apples do they have? Let's think 
step-by-step."}
]
# Generate the output
outputs = pipe(zeroshot_cot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Appends "Let's think step-by-step" to get stepwise reasoning without examples.
- **Fits the architecture:** A single phrase primes chain-of-thought-like reasoning.

### Code Block 12: Zero-shot tree-of-thought
```python
# Zero-shot tree-of-thought
zeroshot_tot_prompt = [
    {"role": "user", "content": "Imagine three different experts are answering 
this question. All experts will write down 1 step of their thinking, then share 
it with the group. Then all experts will go on to the next step, etc. If any 
expert realizes they're wrong at any point then they leave. The question is 
'The cafeteria had 23 apples. If they used 20 to make lunch and bought 6 more, 
how many apples do they have?' Make sure to discuss the results."}
]
# Generate the output
outputs = pipe(zeroshot_tot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** A single prompt emulates a discussion among three experts who share steps, correct each other, and reach consensus (answer: 9 apples).
- **Fits the architecture:** Mimics tree-of-thought multi-path exploration without many model calls.

### Code Block 13: Zero-shot JSON (no examples)
```python
# Zero-shot learning: Providing no examples
zeroshot_prompt = [
    {"role": "user", "content": "Create a character profile for an RPG game in 
JSON format."}
]
# Generate the output
outputs = pipe(zeroshot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Asks for JSON without an example; the output is truncated mid-attribute and is not valid JSON.
- **Fits the architecture:** Demonstrates why free-form output needs guidance.

### Code Block 14: One-shot JSON (format template)
```python
# One-shot learning: Providing an example of the output structure
one_shot_template = """Create a short character profile for an RPG game. Make 
sure to only use this format:
{
  "description": "A SHORT DESCRIPTION",
  "name": "THE CHARACTER'S NAME",
  "armor": "ONE PIECE OF ARMOR",
  "weapon": "ONE OR MORE WEAPONS"
}
"""
one_shot_prompt = [
    {"role": "user", "content": one_shot_template}
]
# Generate the output
outputs = pipe(one_shot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Providing a one-shot format template yields a perfectly structured JSON character profile.
- **Fits the architecture:** Few-shot learning improves the structure of output, not only its content.

### Code Block 15: Clearing VRAM before loading GGUF
```python
import gc
import torch
del model, tokenizer, pipe
# Flush memory
gc.collect()
torch.cuda.empty_cache()
```
- **Explanation:** Deletes the loaded model/tokenizer/pipeline and flushes GPU memory before loading a new model.
- **Fits the architecture:** Loading a new (GGUF) model requires free VRAM.

### Code Block 16: Loading Phi-3 GGUF with llama-cpp-python
```python
from llama_cpp.llama import Llama
# Load Phi-3
llm = Llama.from_pretrained(
    repo_id="microsoft/Phi-3-mini-4k-instruct-gguf",
    filename="*fp16.gguf",
    n_gpu_layers=-1,
    n_ctx=2048,
    verbose=False
)
```
- **Explanation:** Loads the quantized GGUF version of Phi-3 via llama-cpp-python; n_gpu_layers=-1 puts all layers on the GPU; n_ctx=2048 sets the context size.
- **Fits the architecture:** llama-cpp-python efficiently loads compressed models and supports JSON grammars.

### Code Block 17: JSON grammar constrained sampling
```python
# Generate output
output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": "Create a warrior for an RPG in JSON for
mat."},
    ],
    response_format={"type": "json_object"},
    temperature=0,
)['choices'][0]['message']["content"]
import json
# Format as json
json_output = json.dumps(json.loads(output), indent=4)
print(json_output)
```
- **Explanation:** Uses response_format={"type": "json_object"} (a JSON grammar) to guarantee valid JSON output, then parses and pretty-prints it.
- **Fits the architecture:** Constrained sampling makes model output reliably adhere to required formats.

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*

## §4. Practice Exam
*(Separate file: `practice_exam.md`)*
