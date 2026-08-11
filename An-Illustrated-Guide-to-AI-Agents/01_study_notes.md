# AI Agents — Comprehensive Study Notes
**Source:** *An Illustrated Guide to AI Agents* by Stéphane Donnet (O'Reilly, Early Release)
**Scope:** Chapter 1 (Introduction) and Chapter 2 (Large Language Models)

---

## Chapter 1: Introduction

### 1.1 The Big Picture
- In the mid-2020s, AI agents began reshaping AI. After ChatGPT (Nov 2022) revolutionized LLMs, AI agents took things further: they **act with autonomy, pursue goals, and interact with the world**.
- LLMs require significant "hand-holding"; AI agents autonomously decide **which** actions to take, **when** to take them, and **how**.
- This book covers **LLM-backed AI agents** specifically (there are other agent types that don't use LLMs, but "AI agent" in this book always means LLM-backed).

### 1.2 What Is an AI Agent? (Core Definition)
> **"An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."** — Russell & Norvig, *Artificial Intelligence: A Modern Approach*

The four components at the heart of agents:

| Component | Meaning |
|---|---|
| **Environment** | The world the agent interacts with |
| **Sensors** | Components used to observe the environment |
| **Actuators** | Tools the agent uses to interact with the environment |
| **Agent program** | The "brain" or rules used to go from observations to actions |

**Mapping to LLMs:**
- **Agent program (brain)** = a "reasoning" LLM capable of complex thinking
- **Actuators** = the LLM's tools
- **Sensors** = multimodal LLM capabilities (interpreting images, sound, etc.)
- **User** = part of the environment, often initiates the interaction

### 1.3 Large Language Models (the "Brain")
- An LLM predicts the **next word/token** given input text.
- It breaks input into **tokens** — subcomponents of words that let the model generalize to unseen words.
- It predicts the next token, appends it to the input, and continues — this iterative process is **autoregression** (Figure 1-4 shows one-token-at-a-time output).
- Covered in depth in Chapter 2.

### 1.4 Reasoning Large Language Models
- **Train-time scaling**: throwing more data, compute, and parameters at models (GPT-3.5-style). Led to predictable improvements but hit a **ceiling effect** — scaling became too expensive for the small performance gains.
- **Test-time compute / reasoning**: models like **OpenAI o1** and **DeepSeek R1** were trained to "think out loud" — generating **reasoning tokens** before producing the answer.
- Reasoning lets LLMs:
  - Write down their thoughts first via autoregressive behavior
  - Keep track of subproblems of a multi-step task
  - Handle more complex queries requiring multi-step reasoning
- In playgrounds like ChatGPT, thoughts are typically **hidden or summarized**; the final answer is a conclusion built on the reasoning trace.
- **Trade-off**: reasoning LLMs excel at complex decision-making and multi-step problems but are slower/more expensive; use "regular" LLMs for fast, cheap responses.
- Reasoning is central to agents: planning, tool selection, reflecting on mistakes, and revising plans all require advanced reasoning.

### 1.5 Augmenting the LLM
Text-only LLMs as static text-in/text-out entities have three gaps: **no memory, no environment control, no learning**. We augment them with:

#### Memory
- LLMs are **stateless** and forgetful — information is not persisted across calls (Figure 1-9).
- Simplest fix: **add the previous conversation to the current prompt** (Figure 1-10).
- Memory modules mimic human memory (short-term vs long-term), and how we process information.
- **Information overload**: too much information → poor decision-making, even for LLMs.
- **Context engineering**: carefully balancing the amount and quality of information given to the LLM.

#### Tools
- LLMs are **text-in/text-out functions** — they can only *describe/show intent* to take an action, e.g. output the string `multiply(2.3, 8.1)`.
- The LLM expresses **intent** (often as JSON); external software must **parse and execute** it.
- Tools range from simple calculators/search engines to command-shell and coding-environment tools.
- Chapter 5 covers tool use and the **Model Context Protocol (MCP)** for standardizing tools across LLMs.

#### The "Augmented LLM" (Anthropic's term)
- A reasoning LLM + memory + tools = **the augmented LLM** (Figure 1-14) — the building block we turn into an agent.

### 1.6 Planning and Reflection (Final Ingredient)
- **Planning = task decomposition**: breaking a large task into smaller actionable steps.
- The agent creates an initial plan, executes tasks **one at a time**, and refers back to the plan to track progress (Figure 1-15).
- After each task, the agent **reasons** over which step to take next (Figure 1-16).
- **Reflection**: the agent discovers faults midway (e.g., Google + arXiv insufficient → adds Semantic Scholar + PubMed) and updates the plan.
- **Iterative loop**: plan → take actions → reflect on output → update plan (Figure 1-17).

> **Together, a reasoning LLM augmented with memory, tools, planning, and reflection = an AI agent** (Figure 1-18).

### 1.7 An Agentic System

#### Autonomy (a spectrum)
- Agents have varying degrees of autonomy — from **partial** (can execute one step, free to choose tools) to **complete freedom** with no guardrails (Figure 1-19).
- **Guardrails** are often necessary to prevent destructive actions (e.g., deleting important files).
- A system is more "agentic" the more the LLM controls its actions, but as long as it exhibits **goal-directed behavior and makes decisions**, it qualifies as an agent. Partial autonomy in orchestrated workflows still counts as agency.

#### Why agents are useful
They thrive on **open-ended problems** where the goal is clear but the path is not. Common use cases:
1. **Coding** — assistants like Antigravity, Claude Code, Codex, Cursor. Writing + validating code; goals are clear with pre-defined requirements; problems are automatically verifiable.
2. **Deep research** — autonomous in-depth analysis across arXiv, PubMed, Google Scholar, etc.
3. **Automation** — standardizing processes (e.g., structuring heterogeneous patient data across hospitals).

#### Responsible development and usage
- **Human in the loop**: as autonomy grows, humans must authorize, check, and audit agent decisions.
- **Guardrails**: full autonomy can be overkill or harmful; a system with many guardrails steers the agent toward expected behavior.
- **Misinformation**: agents are still LLMs and can **hallucinate** — confidently generate incorrect information. Additional checks/balances needed where correctness is critical.

#### Evaluating agents
- Harder than evaluating models: agents reason over multiple steps, call tools, and take action sequences — a single text-quality score is not enough.
- Two main lenses:
  - **Outcome evaluation** — did the task actually get done (message sent, record updated)?
  - **Trajectory evaluation** — the steps and tool calls taken; judged on **efficiency and soundness** even when the outcome is correct.
- Two additional properties (not visible in a single run):
  - **Reliability** — does the agent succeed every time (outputs are stochastic)?
  - **Safety** — does it avoid harm (from malicious users, manipulated data, or its own mistakes)?
- **Key takeaway: evaluating an agent = evaluating an entire system, not just a model.** (Chapter 7.)

### 1.8 Book Structure
- **Part 1 (Ch 1–7): A single agent** — foundations, building it, evaluating it.
- **Part 2: Specializations**
  - **Multi-agent collaboration (Ch 8)**: multiple agents each responsible for different tasks; they interact and consult each other's specialties. Often a **supervisor agent** manages communication, planning, decomposition, and task assignment (usually has the most capable LLM). Orchestration can be structured or unstructured.
  - **Multi-modal agent (Ch 9)**: the agent is multimodal if its LLM can **process and/or generate** multiple modalities.
    - *Understanding* multiple modalities: uses an **encoder** (modalities → numeric info) + a **connector** (connects representations to the LLM).
    - *Generating* other modalities: uses a **generator**.
  - **Coding agent (Ch 10)**: can run programs in a dedicated environment, read codebases, generate functions, fix bugs, test. Led to **"vibe coding"** — non-developers relying on agents to build software. Benchmarked on **SWE-bench**.

### 1.9 The TinyAgent (Build-Along Project)
- You build a **TinyAgent** step by step from the book's principles; code is in the `illustrated-agents` package (pip/uv), plus GitHub notebooks.
- The code implementing agent behavior is called the **agent harness**. Types:
  - **Terminal-based**: Claude Code, Gemini CLI, OpenAI Codex CLI, OpenCode
  - **Code-based** (libraries to build your own): LangGraph, Smolagents, Pydantic AI
  - **Personal assistant** (persistent, retains memory/skills across sessions): OpenClaw, Hermes Agent
  - **Hosted** (cloud products): Replit, v0, Manus
  - **UI-based**: Antigravity, Cursor, Windsurf, GitHub Copilot
- Harnesses are trending toward **personal assistants** — persistent, always-on, chat via any messaging system (WhatsApp, Discord, Slack, email). OpenClaw is the most famous example (~300k GitHub stars in months).
- TinyAgent's harness is **code-based with a terminal implementation**, built from scratch with minimal dependencies.
- Initial skeleton (agent.py):
```python
class TinyAgent:
    """A minimal, modular, and educational agent framework."""
    def __init__(self):
        self.llm = None      # Chapter 2 & 3: Add LLM
        self.memory = None   # Chapter 4: Add Memory
        self.tools = None    # Chapter 5: Add Tools
        self.planner = None  # Chapter 6: Add Planning

    def run(self, task: str) -> str:
        """Run the agent on a task."""
        return self._step(task)

    def _step(self, task: str) -> str:
        """Perform a single step."""
        return f"Received: {task}"

    def _execute_action(self, action: str) -> str:
        """Execute a tool action."""
        return f"Executed action: {action}"
```
- Key methods: `run` (run on a task), `_step` (single step — may be answer or tool call), `_execute_action` (execute a tool action).
- Each chapter ends with a **"What We Built"** section summarizing TinyAgent changes.

### Chapter 1 Key Takeaways
1. Agent = perceives environment via sensors, acts via actuators (Russell & Norvig).
2. LLM-backed agent = reasoning LLM + memory + tools + planning + reflection.
3. Reasoning = test-time compute ("thinking out loud") vs train-time scaling ceiling.
4. LLMs are stateless → need memory; text-in/text-out → need tool-calling software.
5. Autonomy is a spectrum; guardrails and human-in-the-loop matter.
6. Evaluation: outcome + trajectory lenses, plus reliability and safety.

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

## High-Yield Vocabulary List
- **Agent**: perceives environment via sensors, acts via actuators.
- **Autoregressive model**: generates output by feeding its own prior output back as input.
- **Token**: word, part of word, number, or punctuation — the unit of LLM I/O.
- **Base model**: result of pre-training (next-token prediction).
- **SFT (instruction tuning)**: training on prompt-completion pairs; prompt excluded from loss.
- **RLHF**: RL from human/reward-model preference scores (preferred vs rejected completions).
- **RLVR**: RL with automated verifiable rewards (format, correctness).
- **GRPO**: RL algorithm; generates a group of responses per step, rewards relative to the group.
- **System prompt**: privileged input shaping model behavior ahead of user messages.
- **Context engineering**: choosing the most relevant info to fit within context length.
- **KV cache**: caches keys/values to avoid recomputation; only new token's track computed.
- **Context length / context size**: number of token tracks flowing through the model simultaneously.
- **Self-attention**: allows each token to gather context from other tokens via Q/K/V.
- **MoE**: Mixture-of-Experts; router + experts; sparse vs active parameters.
- **MHA / MQA / GQA / MLA / DSA**: attention variants (see table above).
- **Hallucination**: confidently generated incorrect information.
- **Trajectory**: sequence of steps (thought → action → observation) an agent takes.
- **Agent harness**: code/software implementing agent behavior (terminal, code, assistant, hosted, UI).
