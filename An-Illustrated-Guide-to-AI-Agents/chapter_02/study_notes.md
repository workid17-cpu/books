# AI Agents — Comprehensive Study Notes
**Source:** *An Illustrated Guide to AI Agents* by Stéphane Donnet (O'Reilly, Early Release)
**Scope:** Chapter 2 (Large Language Models)

---

## Chapter 2: Large Language Models

### 2.0 Chapter Roadmap
- **Part 1 (high-level, for agent developers):** tokens, autoregression, system prompts, multi-turn conversations, tool use, OpenAI-compatible endpoint, training phases (pre-training, SFT, RL), Transformer overview.
- **Part 2 (deep dive):** self-attention math, efficient attention (GQA, MQA, Flash Attention, MLA, DSA), Mixture-of-Experts (MoE).

### 2.1 Input and Output Tokens
- Language models are systems trained to **predict and generate text sequences**.
- Inputs/outputs are **tokens** — words, parts of words, numbers, or punctuation. Example: "flamingos" → `flamingo` + `s`.
- Generation is a loop: the model generates a token, **adds it to the input**, processes again to generate the next token.
- Models that consume their own output as input are **autoregressive models** (Figure 2-4).

### 2.2 From Language Modeling to Powering Agents
- The first training phase produces a **base language model**.
- **Post-training** gives the model capabilities to power agents (parse inputs / generate outputs in agent-appropriate formats).
- Three key input/output structures for agents:
  1. **System prompt** — a privileged input that shapes model behavior *before* the user message; lets deployers customize behavior without retraining the model. Example instructs the model to "answer truthfully."
  2. **Multi-turn conversations** — conversation between user and AI chatbot; the LLM can track who said what in history (a form of memory, detailed in Ch 4).
  3. **Tool use** — the LLM outputs a specific format choosing a software function to invoke; the agentic wrapper looks for these patterns and calls the functions. Functions are usually listed in the system prompt.
- Example (Figure 2-6): instead of answering directly about Toronto Zoo flamingos, the LLM emits a `web_search` tool call with a structured query.

### 2.3 The OpenAI-Compatible Endpoint
- A standardization of LLM inference: same fields, parameters, responses.
- Standard fields: **base_url** (URL of your hosted LLM), **api_key**, **model** (name), **messages** (query).
- Hosting options: **Ollama**, **LMStudio** (good for devs + non-devs), **llama.cpp** (ideal for developers). Book assumes Ollama at `http://localhost:11434/v1/`.
- Model download example: `ollama pull gemma4:e4b` and `ollama pull gemma3:12b`.
- The `messages` structure auto-converts to each model's prompt template.
- Response: inspect `result["choices"][0]`.
- Book models: **Gemma 3 (12B)** — no native reasoning/tool calling; **Gemma 4 (E4B, ~4B effective params)** — trained for reasoning + tool calling.
- **Important philosophy**: the book first teaches reasoning/tool-calling via **prompting techniques** (explicit), *then* explores native capabilities — because (1) using the built-in `reasoning`/`tool_calls` fields is "magical" and doesn't teach how they work, and (2) some models don't support those fields natively but can still act as agents with prompting.

#### Code components built:
- **`Response` dataclass** — tracks `content`, `reasoning`, `tool_call`, `metadata` (model name, `prompt_tokens`, `completion_tokens`).
- **`LLM` class** — queries the OpenAI-compatible `/chat/completions` endpoint; supports `tools`, `reasoning_effort: "none"` when thinking disabled.
- **`Step` dataclass** — a single step in an agent's trajectory: `thought`, `action` (a tool call), `observation` (output of tool usage), `answer` (final answer = end of turn), `metadata`.
- **`Trajectory`** — records agent execution as a sequence of runs, each run with a query and list of Steps; used for debugging.
- **Terminology**: **action** = a tool call (for Chapter 6's thought → action → observation loops). **Trajectory** = the sequence of steps the agent takes.
- TinyAgent updated to hold `llm` and `trajectory`.

### 2.4 Training a Large Language Model
Two major phases: **pre-training** and **post-training** (Figure 2-7).

#### Pre-training: language modeling
- Task: given a sequence of tokens, **predict the next token** most likely to appear.
- Data: curated mixtures of web text, books, and code, filtered by quality.
- Training loop: hide the last token → model predicts → compare to correct token via **loss** → update **weights**. Done billions of times → **base model**.
- Most famous base model: **GPT-3 (2020)**.

#### Post-training: supervised fine-tuning (SFT)
- Also called **instruction-tuning**: trains the model to follow instructions in prompts.
- Training example = a **prompt + completion** showing desired behavior.
- Key difference from pre-training: **prompt tokens are included in context but excluded from the loss** — only completion/response tokens are trained on (Figure 2-9).
- Teaches instruction-following but not human preferences → need RL next.

#### Post-training: reinforcement learning (RL)
- No complete targeted response needed; instead a **reward** (quality score) evaluates model-generated responses, and weights update based on that score.
- **RLHF (Reinforcement Learning from Human Feedback)**: preference scores from humans or **reward models** (often LLMs themselves). Training example = prompt + a *preferred* completion + a *rejected* completion. Objective: increase likelihood of preferred output, decrease likelihood of rejected output. Polishes behaviors like response length, language style, safety.
- **RLVR (Reinforcement Learning with Verifiable Rewards)**: rewards obtained from **automated verifiers** — e.g., checking format, or matching a known-correct answer (math). Scalable to domains with objectively defined right/wrong (Figure 2-11).

#### GRPO (Group Relative Policy Optimization)
- Used to train DeepSeek-R1.
- At each training step, generates a **group of responses** for the same prompt using varied temperatures (diversity).
- Rewards are computed **relative to the group** — reinforces responses that score higher than others. (The "group" in the name.)
- Example (math, group size 5): format reward 0.3 if `<answer>42</answer>` format correct else 0; accuracy reward 0.7 if final answer correct else 0.

### 2.5 The Transformer Architecture
LLMs have predominantly used the **Transformer decoder** architecture since ~2020.

#### Three major components (Figure 2-14)
1. **Tokenizer** — breaks text into tokens; has a fixed **vocabulary** (e.g., 50,000 tokens). For agents, tuning includes support for code, multiple languages, and special tokens (e.g., `<|start|>`).
2. **Stack of Transformer blocks** — processes tokens; most of the model's computation.
3. **Language modeling (LM) head** — converts the final representation into a **probability distribution over the entire vocabulary**; each token gets a probability score.

- **Embeddings**: one vector per token in the vocabulary; numeric representations the model uses to process inputs.
- **Decoding strategy**: how we pick a token from the probability distribution.
  - **Temperature = 0**: always pick the highest-probability token (greedy).
  - **Higher temperature**: **sampling** — higher-probability tokens have higher chance of being picked.

#### Flow through the Transformer blocks (Figures 2-18, 2-19)
- Each token flows through its own **track**; the number of tracks = **context size** (e.g., context length 100,000 = 100,000 tokens simultaneously).
- For text generation, only the **final token's output** is passed to the LM head to predict the next token.
- **Context length** is a key selection property for agent developers — it limits how much information can be packed in the input → motivates **context engineering**.
- **KV caching** (a.k.a. prompt-caching / prefix-caching): after the first forward pass, cache previous token calculations; subsequent generation steps process only the **single new token's track**. Dramatically increases speed, reduces processing. Optimizing for the KV cache is a key responsibility for agent developers (speed + cost).

#### Inside a Transformer block (Figure 2-22)
Two major components:
1. **Self-attention layer** — lets each token gather context from other tokens in the sequence.
2. **Feed-forward neural network (FFNN)** — processes each token's representation independently.

##### Feed-forward neural network
- Does the "heavy lifting" in predicting the next token; stores **factual associations** learned during training.
- Example: given "The Shawshank", predicts "Redemption" (1994 film) — an oversimplified illustration.
- A **dense** model: one big network, all neurons active for every token (e.g., 512 inputs → 2048 hidden → back down).
- **Mixture-of-Experts (MoE)** alternative: many smaller specialized networks + a **router** that decides which expert(s) process each token; non-selected experts stay inactive. More total capacity without activating everything.

##### Self-attention (high-level)
- Resolves **context/ambiguity**: e.g., in "The dog chased the llama because it", "it" could refer to dog or llama — self-attention resolves which.
- Intuition: enriches the current token's vector with info from the **most relevant previous tokens**.
- Two steps: (1) **relevance scoring** — score relevance of previous tokens; (2) **combining information** — blend relevant positions into the current token's output vector in proportion to their relevance scores.

### 2.6 Part 2: A Deeper Dive

#### How Self-Attention Works (the math)
- Three learned **projection matrices** multiply the input vectors → **Queries (Q), Keys (K), Values (V)** matrices (Figure 2-30).
- **Step 1 — Relevance scoring**: multiply current token's Query by the Keys of all input tokens → a relevance score for each position (e.g., "dog" gets 40%). The famous formula: `softmax(Q·Kᵀ / √d_k)` — scaled by √d_k, then softmax.
- **Step 2 — Combining**: multiply each token's Value vector by its relevance score and **sum** → enriched output vector for the current position. Higher-scoring tokens contribute proportionally more.

#### KV-Caching Revisited
- **Vanilla attention** recomputes K and V for every previous token at each generation step — redundant (Figure 2-34).
- The **KV cache** stores previously computed K and V and reuses them, making inference much faster.
- **Query (Q) vectors do NOT need to be cached** — only the latest token's query is needed.

#### More Efficient Self-Attention
Motivated by KV cache memory and compute costs. Popular techniques: **Grouped-Query Attention** and **Flash Attention**; DeepSeek introduced **MLA** and **DSA**.

| Mechanism | Idea |
|---|---|
| **Multi-head Attention (MHA)** | Separate full Q, K, V projection per head; full-sized K and V per head. |
| **Multi-Query Attention (MQA)** | Shares K and V across query heads → less memory, some accuracy cost. |
| **Grouped-Query Attention (GQA)** | Shares K and V across groups of query heads → less memory, some accuracy cost. |
| **Multi-head Latent Attention (MLA)** | Low-rank joint compression of K and V into a smaller **Latent KV** that is cached instead of full K and V. First used in DeepSeek-V2, popularized by DeepSeek-R1. Often combined with quantization. More efficient than GQA. |
| **DeepSeek Sparse Attention (DSA)** | Instantiated under MLA; selects which tokens to attend to using a **Lightning Indexer** + **Top-K Selector**. |

##### MLA details (Figure 2-36)
- Compresses input embeddings into low-dimensional **Latent Q** and **Latent KV**.
- Cache **Latent KV** (small) instead of full K and V.
- **RoPE (Rotary Position Embedding)** is applied to a *decoupled component* of Latent Q (since Latent Q is recomputed every step), NOT to Latent K (which would break caching) — positional info is carried by a separate small key.
- Q, K, V are split across heads like standard MHA; content + positional components concatenated, then passed through standard multi-head attention.

##### DSA details (Figure 2-38)
- **Lightning Indexer**: scores how relevant each preceding token is to the current query, using Q/K values with RoPE applied, plus a scalar weight `w`.
- **Top-K Selector**: keeps only the KV entries corresponding to the Top-K index scores.
- Attention output computed over the **query token + a subset of KV entries** (sparse), e.g., 2048 tokens in the paper — drastically reduces computation.
- In DeepSeek's construction, MQA is used as the core attention (fewer K and V than MHA).

#### Mixture of Experts (MoE)
Two main components:
1. **Experts** — each feed-forward network is replaced by a set of smaller "experts" (FFNNs). During training, experts specialize (research suggests fine-grained specialization like verbs vs numbers, not whole domains).
2. **Router (gate network)** — a small FFNN trained to choose an expert for a given token; applies **softmax** to produce a probability distribution over experts; the highest-probability expert is selected and its output is **scaled by the router's probability** before passing forward.

- **Dense model** = single FFNN applied to every token (everything active).
- **Sparse model** = multiple smaller FFNNs; typically a fixed number of experts selected per token.
- **Load balancing** prevents a few experts from dominating:
  - **Expert capacity**: caps how many tokens each expert processes per batch; overflow tokens go to the next-highest-scoring expert.
  - **Auxiliary loss**: loss functions rewarding the router for equal expert distribution (or punishing repeated choice of the same expert). Also: adding Gaussian noise before router probabilities for occasional different choices.
- **Sparse parameters** = all parameters that must be **loaded into memory**; **active parameters** = those **activated during inference**. MoE doesn't make the model smaller, but it runs much faster (only a few experts activated at a time).
- Naming convention: **Qwen3-30B-A3B** = 30B sparse (loaded), 3B active (used).
- **DeepSeek-R1**: 256 experts, 8 chosen per token, plus a **shared expert** that bypasses the router and is always active (absorbs general knowledge; routed experts specialize).
- Recent models using MoE: OpenAI's GPT-OSS, NVIDIA's Nemotron 3, Llama 4 Maverick, Qwen 3, Kimi-K2, GLM 4.5, Mistral 3 Large, etc.

### Chapter 2 Key Takeaways
1. LLMs are autoregressive next-token predictors; input/output = tokens.
2. System prompts, multi-turn messages, and tool-call formats let an LLM serve as an agent's reasoning core.
3. Training: pre-training (next-token → base model) → SFT (instruction following) → RL (RLHF/RLVR + GRPO for alignment/reasoning).
4. Transformer = tokenizer + Transformer blocks (self-attention + FFNN) + LM head.
5. Context length limits input; the KV cache drives speed and cost — both critical for agents.
6. Attention math: Q, K, V projections → scaled softmax relevance → weighted sum of V.
7. Efficient attention: GQA/MQA share KV; MLA compresses KV into Latent KV; DSA sparsifies with Lightning Indexer + Top-K Selector.
8. MoE: router + experts; sparse (loaded) vs active (used) parameters; load balancing via expert capacity / auxiliary loss.

---

## High-Yield Vocabulary List (Chapter 2)
- **Base model**: result of pre-training (next-token prediction).
- **SFT (instruction tuning)**: training on prompt-completion pairs; prompt excluded from loss.
- **RLHF**: RL from human/reward-model preference scores (preferred vs rejected completions).
- **RLVR**: RL with automated verifiable rewards (format, correctness).
- **GRPO**: RL algorithm; generates a group of responses per step, rewards relative to the group.
- **System prompt**: privileged input shaping model behavior ahead of user messages.
- **KV cache**: caches keys/values to avoid recomputation; only new token's track computed.
- **Context length / context size**: number of token tracks flowing through the model simultaneously.
- **Self-attention**: allows each token to gather context from other tokens via Q/K/V.
- **MoE**: Mixture-of-Experts; router + experts; sparse vs active parameters.
- **MHA / MQA / GQA / MLA / DSA**: attention variants (see table above).
