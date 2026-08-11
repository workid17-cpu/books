# AI Agents — Comprehensive Study Notes (Chapters 3 & 4)
**Source:** *An Illustrated Guide to AI Agents* (O'Reilly, Early Release)
**Scope:** Chapter 3 (Reasoning Large Language Models) and Chapter 4 (Memory)

---

# Chapter 3: Reasoning Large Language Models

## 3.1 What Reasoning Is
- Reasoning LLMs (DeepSeek-R1, OpenAI GPT-5, Google Gemini) scale LLMs to new heights through **reasoning frameworks**.
- Reasoning **mimics human thinking** by generating "thoughts" (tokens) before giving a final answer. These are the **chain-of-thought** — the model breaks a problem into smaller steps called **reasoning steps / thought processes** (Figure 3-1).
- Reasoning LLMs first "think" and generate intermediate information before answering. They may still arrive at the same conclusions as non-reasoning LLMs — but with more deliberation.

### System 1 vs System 2 (Kahneman's dual-process theory)
| | System 1 | System 2 |
|---|---|---|
| Human cognition | Automatic, quick, intuitive, snap judgments | Slow, deliberate, conscious effort |
| LLM analogy | **Non-reasoning LLMs** — respond fast from learned patterns, no explicit step-by-step thinking | **Reasoning LLMs** — deliberate step-by-step analysis, catch errors a System-1 model would miss |
| Strengths/weaknesses | Efficient for routine tasks, prone to **cognitive biases** | Relies on logical reasoning, more accurate decisions |

- **Why agents need reasoning:** to plan behavior, decide which actions to take, and reflect on actions taken. Example: coding agents (Cursor, Cline) create markdown files to track and revise plans.
- **Note:** Reasoning is foundational for tool selection (Ch 5) and planning/reflection (Ch 6). Without reasoning, reflection methods are less accurate.

## 3.2 The Paradigm Shift: Train-Time Compute → Test-Time Compute
- **Train-time compute** = scaling resources during **pre-training AND post-training**. Three main factors (Figure 3-4):
  1. Model size (number of parameters)
  2. Dataset size (number of tokens)
  3. Compute (FLOPs — floating point operations per second)
- More pre-training resources → more powerful model. Little attention is spent on inference. This applies to **non-reasoning LLMs**.
- **Test-time compute** = scaling **inference** instead. Lets reasoning LLMs **"think longer."** Non-reasoning LLMs directly generate the answer (e.g., "What is 3+2?" → "5"); all compute goes to one token. Reasoning models spend more tokens ("thinking" tokens) to derive the answer.
- **Key idea:** the more tokens generated, the better the performance — but only if the tokens contain relationships and new thoughts, not random tokens. It gives the LLM more **time to "think."**
- **What "compute" means:** the amount of computational work the model performs during inference; each token requires computational resources.

### Scaling Laws
- Describe the relationship between a model's scale (compute, dataset, parameters) and performance.
- Take the form of **power laws** with **diminishing returns**: each doubling of compute gives smaller gains than the previous → a **logarithmic relationship**.
- On a **log-log scale** the power law becomes a **straight line**, making trends easier to compare over many **orders of magnitude**.
- **Kaplan Scaling Law** (Jared Kaplan, 2020): performance improves predictably with compute/data/parameters, with diminishing returns. For a fixed compute budget: keep increasing model size and train on as much data as possible **without overfitting**. Balance between data, parameters, and compute is key.
- **Chinchilla Scaling Law** (Hoffmann et al., 2022): models were often **undertrained**; for a fixed compute budget it's better to use a **smaller model trained on much more data**.
- Both: scale all three factors **in tandem** for optimal performance.
- **The problem:** throughout 2024, gains stopped growing linearly with compute/data/model size — likely reached a **limit/plateau** (assuming no major architectural improvements).
- **The answer:** test-time compute. OpenAI's post: increasing test-time compute may affect performance the same as increasing train-time compute (train-time = more RL; test-time = more thinking time), possibly scaling even further.
- **"Scaling Scaling Laws with Board Games"** (Jones): trained **AlphaZero** to play **Hex**. Train-time compute = more parameters + epochs; test-time compute = deeper tree search (more solutions). Result: both are tightly related — for a target **Elo score**, a decrease in one must be offset by an increase in the other; for best performance, scale both together.
- **Paradigm shift happened in 2024–2025** toward reasoning models that balance training with inference.
- **NOTE:** test-time compute doesn't only mean thinking longer — it means scaling time/compute on inference. Alternatives: generate many answers and vote on the best one (self-consistency).

## 3.3 Categories of Test-Time Compute
Two categories used throughout the chapter:

### 1. Search against verifiers (output-focused)
- Sample many answers and/or **reasoning traces**; select the best answer using a **reward model (RM) / verifier** — a model that scores answer/reasoning quality.
- Output-focused (focus on generated outputs).
- Doesn't necessarily *enable* reasoning; it's more like non-human behavior — programmatically goes through many answers and scores them. Reminds us of **majority voting** or **tree search**.
- Three steps: (1) sample multiple reasoning processes/answers; (2) verifier scores output; (3) best answer chosen.
- **No retraining needed**; easy to scale up/down by sampling more/fewer answers.

### 2. Modifying proposal distribution (input-focused)
- Tune or prompt the model so it outputs better reasoning steps. The **proposal distribution** = the token probabilities from which tokens are sampled.
- Achieved mainly by **training** (SFT or RL) — may use RMs during training.
- Input-focused (focus on how the model generates).

### Reward Models (RMs / verifiers)
- Score LLM-generated reasoning traces and answers.
- Can be (fine-tuned) LLMs (judge reasoning) or rule-based systems (e.g., unit tests that test the output).
- Two types:
  - **Outcome Reward Model (ORM)** — judges ONLY the final outcome, ignores reasoning steps.
  - **Process Reward Model (PRM)** — judges ONLY the intermediate reasoning steps (process). Often a mix of ORM and PRM is preferred.
- Example (flamingos): "I saw 6 flamingos. 2 flew away. 1 hid behind a tree. How many can I see?" → reasoning steps can be individually scored; a step that makes a mistake can be scored low, then a later step corrects it.

## 3.4 Prompt Engineering
- **Chain-of-Thought (CoT)** — one of the first techniques to elicit reasoning in LLMs not trained for reasoning. The prompt asks the model to explain its reasoning process.
- LLMs follow examples well:
  - **One-shot prompting** — a single example demonstrating desired behavior.
  - **Few-shot prompting** — two or more examples; tends to result in higher accuracy (helps the model recognize patterns).
  - **Zero-shot prompting** — no examples; e.g., simply appending **"Let's think step-by-step."** Works with some models.
- **Performance note:** zero-shot < one-shot < few-shot (generally).
- **Key insight:** non-reasoning LLMs DO have reasoning capabilities hidden in their parameters — prompting extracts them ("let's think step-by-step" works).
- TinyAgent reasoning: append the "thinking" prompt to the task; Chapter 6 uses separate "THOUGHT" and "ANSWER" fields.

## 3.5 Search Against Verifiers (methods)

### Self-Consistency
- First method; does NOT use a reward model/verifier.
- Samples a user-defined number of answers (high temperature + CoT prompting) and performs a **majority vote** to select the most frequent answer.
- Scales by generating more answers.
- **Works surprisingly well** because sampling reduces the chance of selecting an infrequent, incorrect answer. But for very complex tasks the LLM seldom gets right, self-consistency won't help much.

### Best-of-N Samples
- Generate **N candidate answers** (high/varying temperature for diversity); a verifier evaluates each; select the highest-scoring one.
- Two forms:
  - **Best-of-N with ORM** — only answers are scored (e.g., by LLM, unit tests, compiler); highest-scoring answer chosen.
  - **Best-of-N with PRM** — only processes (thoughts) are scored; each trace scored and averaged; answer with highest-scoring traces selected.
- Example: generate a `roman_to_int` function; verify with unit test cases (score = fraction of passing tests); pick the highest-scoring answer.
- Flexible: can weigh each candidate by PRM and aggregate scores for answers appearing multiple times (**weighted Best-of-N**).

## 3.6 Modifying Proposal Distribution (methods)

### Supervised Fine-Tuning (SFT)
- Model exposed to **triplet-like data**: user query + reasoning trace + answer.
- Downside: collecting large amounts of reasoning traces is hard — often manually labeled, hundreds of thousands of samples.

**Flan-PaLM** (Chung et al.):
- **Flan** = Fine-tuning language models (not the FLAN models). Uses a variety of instruction templates across 1,800+ tasks.
- Included annotated CoT traces: arithmetic reasoning, multi-hop reasoning, natural language inference (determining truthfulness of a statement).
- Fine-tuned PaLM (540B parameters) → "Flan-PaLM." Models were easier to prompt to reason (e.g., "let's think step-by-step").
- Data was mostly non-reasoning examples mixed with reasoning traces — so it's premature to call it a reasoning model.

**s1: Simple test-time scaling**:
- Created a reasoning LLM using only **1,000 questions + reasoning traces** (fine-tuned Qwen2.5 32B-Instruct).
- Used **special tokens** to separate thinking from answering: `<|im_start|>think` ... `<|im_start|>answer`.
- Test-time scaling exploration: forcefully **terminating** the thinking process, or **lengthening** it by adding "Wait" when the model tries to end.
- Result: more test-time compute → better performance (consistent with OpenAI's experiment).

### Reinforcement Learning (RL)
- **DeepSeek-R1-Zero**:
  - Started from DeepSeek-V3-Base; relied **solely on RL** (no SFT on reasoning data).
  - System prompt enclosed thinking in `<think>...</think>` and answer in `<answer>...</answer>` — but did NOT specify what the reasoning should look like; the model figures that out.
  - Used the same two rule-based rewards as Chapter 2: **accuracy reward** + **format reward**. Algorithm: **GRPO**.
  - Result: the model discovered longer, more complex reasoning leads to better answers (longer traces emerged) — evidence for test-time scaling.
  - **Cold start problem:** without SFT guidance, the model mixed languages and had poor readability (no markdown formatting).
- **DeepSeek-R1** (5-step pipeline):
  1. **Cold start prevention** — SFT on ~5,000 high-quality samples with long CoT traces (→ intermediate model DeepSeek-V3-1).
  2. **Reasoning-oriented RL** — GRPO (like R1-Zero); added a **language-consistency reward** to prevent language mixing (→ DeepSeek-V3-2). Excels at reasoning but not at non-reasoning tasks (translation/writing).
  3. **Rejection sampling** — DeepSeek-V3-2 generates synthetic reasoning data; a reward model (DeepSeek-V3-Base) selects high-quality traces; DeepSeek-V3-Base also samples mostly non-reasoning data → **800,000 samples** (reasoning + non-reasoning).
  4. **SFT** — on the 800k dataset → first version of DeepSeek-R1.
  5. **RL for all scenarios** — additional reward signals for **helpfulness and harmlessness** (alignment); model asked to summarize its reasoning to prevent readability issues → final DeepSeek-R1.
- **In sum:** DeepSeek-R1 = SFT on DeepSeek-V3-Base + GRPO with format, accuracy, and preference rewards.
- Note: the first three steps were purely to create synthetic data.

### Native Reasoning (chat templates)
- After training, reasoning is enabled via the model's **chat template** with special tokens. Example: Gemma 4 E4B tokens:
  - `<bos>` — beginning of sequence.
  - `<|turn>system` / `<|turn>user` / `<|turn>model` — start of each role's turn.
  - `<turn|>` — end of a turn.
  - `<|think|>` — add to system turn to ENABLE reasoning; remove to DISABLE.
- Ollama normally parses queries via the chat template automatically.
- With native reasoning, you don't need prompting "tricks" like CoT — the model was trained on CoT examples.

## 3.7 Upcoming Fields in Reasoning Research
Three key areas:
1. **Reasoning in multi-modal LLMs** — reason with images, audio, video.
2. **Efficient reasoning** — better reasoning with less computation.
3. **Reasoning in latent space** — thinking in compressed, abstract, non-textual forms.

### Reasoning in Multi-modal LLMs
- Without reasoning grounding, multimodal reasoning performance is usually worse.
- Enhanced via prompting, search against verifiers, or modifying the proposal distribution (SFT/RL).
- **Multimodal Chain-of-Thought (MCoT)** — two-stage framework combining text + vision: (1) generate an explicit reasoning process from language + visual input; (2) append that rationale to the original language input and use it with the same visual input to infer the final answer. Both stages use same-architecture models, trained separately with SFT.
- **Llava-Chain-of-Thought** — used GPT-4o to create synthetic data with four reasoning stages: **summary, caption, reasoning, answer** → 100,000 records to fine-tune Llama 3.2v (a vision LLM).
- **Reason-RFT** — two-step approach (like DeepSeek-R1): (1) SFT with CoT activates reasoning; (2) RL generalizes it. Three reward types:
  - **Mathematical** — larger scores for exact matches, lower for small errors.
  - **Function-based** — compare predicted vs target transformation steps (exact, partial, function-only matches get different weights).
  - **Discrete-valued** — binary scoring for categorical/integer answers (exact match only).

### Efficient Reasoning
- Test-time compute is expensive (thousands of extra tokens). Goal: similar gains with selective reasoning and **adaptive computation** (reduce reasoning trace length).
- **Chain-of-Draft (CoD)** — inspired by human cognition; concise intermediate thoughts; keep each reasoning step to a **draft of ~5 words** (guideline, not enforced). Shorter traces, similar performance. Downsides: harder to balance verbosity vs accuracy; harder for users to read.
- **Token-budget-aware LLMs** — trained to adaptively change reasoning trace length based on problem complexity; may be trained to prefer shorter traces via **length rewards**.
- **Length reward designs** — reward short correct answers, penalize long/wrong answers.
  - **Kimi k1.5** (closed-source multimodal LLM) — RL gives extra rewards to correct short answers, penalizes wrong long answers most; gradually warmed up the length penalty because it slowed early learning.
  - **O1-Pruner** — compares answer length to a reference model; longer → penalty, same → no change, shorter → high reward; balanced with a **dynamic accuracy reward** (so long correct answers also rewarded for hard problems).
  - **L1** — reasoning LLM trained by telling it up front how many tokens to think ("Think for 30 tokens"); rewarded for correctness AND matching the requested length.
- **Hybrid reasoning (on/off switch):** Qwen-3 family — special tokens `/think` and `/no_think`; thinking mode uses CoT, non-thinking mode gives the answer directly (like traditional LLMs).

### Reasoning in Latent Space
- Explicit CoT = listening to someone work through a problem out loud (visible tokens).
- **Latent space reasoning** = making CoT internal; hidden representations replace visible reasoning steps; the model thinks in its "mind's eye" and skips straight from question to answer.
- **Chain-of-Continuous-Thought** — instead of decoding embeddings into tokens and adding them to input, it directly operates on the **last hidden state**: generates the last hidden state and uses it as input. Special tokens `<bot>` (beginning of thought) and `<eot>` (end of thought). No tokens produced until `<eot>`.
- **CODI (Continuous Chain-of-Thought via Self-Distillation)** — trains a **teacher** and a **student** LLM simultaneously:
  - Teacher: trained on explicit CoT data (cross-entropy loss), must produce a correct answer with a reasoning trace.
  - Student: follows Chain-of-Continuous-Thought-like process (no explicit CoT; reasons on last hidden states).
  - The teacher's answers are compared with the student's, and an additional loss is added — the explicit CoT is implicitly taught to the student.
- **Trade-off:** visible reasoning aids debugging; latent-space reasoning is more efficient and not limited to text (could reason in symbolic language or equations).

## Chapter 3 Key Takeaways
1. Reasoning = System 2 (deliberate) thinking vs non-reasoning = System 1 (fast).
2. Paradigm shift: from train-time compute to test-time compute ("thinking longer").
3. Scaling laws (Kaplan, Chinchilla) show diminishing returns → limit reached → test-time scaling.
4. Two categories of test-time compute: **search against verifiers** (output-focused) and **modifying proposal distribution** (input-focused).
5. Verifiers: **ORM** (outcome) vs **PRM** (process).
6. Prompting: CoT, zero/one/few-shot; "Let's think step-by-step."
7. Search methods: self-consistency (majority vote), Best-of-N.
8. Training methods: SFT (Flan-PaLM, s1) and RL (DeepSeek-R1-Zero cold-start problem; DeepSeek-R1 5-step pipeline).
9. Native reasoning via chat templates / special tokens.
10. Frontiers: multimodal reasoning (MCoT, Reason-RFT), efficient reasoning (CoD, length rewards, hybrid reasoning), latent-space reasoning (Chain-of-Continuous-Thought, CODI).

---

# Chapter 4: Memory

## 4.1 Why Memory Matters
- LLMs are **forgetful / stateless** — they don't remember past conversations or actions. A locally loaded LLM can't remember your name without explicit memory.
- Hosted LLMs (ChatGPT, Claude) aren't "regular" LLMs — they're **augmented with memory and tools**.
- Without memory: a personal assistant can't remember conversations; a coding agent can't understand the whole codebase; an agent forgets it already took an action and repeats it.
- **Broad definition:** memory includes not only past actions of the agent but also external information beyond agent–environment interactions (e.g., a coding agent's memory includes hosted documentation and issues pages).
- Memory = remembering AND storing newly generated information; also deciding **what to store, what to remember, and what to forget/delete**.
- Memory is **application-specific** and **iterative**; enables agents to learn from past errors and experiences.
- Memory is the **foundation** of the agent — without it, tool usage and planning are impossible.

## 4.2 Types of Memory
Modeled after human memory ("Cognitive Architectures for Language Agents" paper) — four types of external memory:

| Type | Human example | Agent example |
|---|---|---|
| **Working memory** (short-term) | limited-capacity system holding info for decision-making/reasoning | chat history continuously fed back to the LLM; data that persists across LLM calls |
| **Episodic memory** (long-term) | specific events/experiences (your last birthday party) | specific actions the agent has taken and their outcomes |
| **Semantic memory** (long-term) | knowledge about the world (capital of France) | querying an external database: Wikipedia, your codebase, organization documents |
| **Procedural memory** (long-term) | patterns of how to do things (writing Python) | info hidden in its parameters (parametric memory) or in the system prompt, persists across calls |

- **Parametric memory** — knowledge already in the model's parameters (e.g., "What is the capital of France?" → Paris). Can technically be instilled via SFT, but it's not stable — we're not sure which info is retained vs incorrectly reconstructed.
- These types may not be stored as such — could be one big pile of information or separated databases.

## 4.3 Short-Term Memory

### Conversation Memory
- Conversation history = context for generating responses; formatted as messages (system / user / assistant).
- Messages are updated each turn and fed back into the LLM for the next query.
- **Key point:** the LLM does NOT truly remember — it's *told* the conversation by inserting it into the prompt.
- **Memory class** (TinyAgent): a list of dicts tracking role + content; `.add()` and `.get_messages()`; optional tool call support (for Chapter 5).
- In TinyAgent: `run` and `_step` call `memory.add(...)` to track history and `memory.get_messages()` to give full context to the LLM.

### Context Window Limits
- **Context window** = max number of tokens the LLM can process (input + output). (Context *length* = length of input, output, or both.)
- Example: 13 tokens out of a possible 8,192.
- As history grows, it may exceed the window → answer cut short or prompt unprocessable.
- More info in prompt → harder for the LLM to attend to everything → need techniques.

### Trimming
- Simply remove the first few interactions when the history grows too large.
- Keeps memory minimal but might remove important early information.
- Implementation: `TrimmingMemory(Memory)` uses **inheritance**; keeps system message + last two turns (4 messages: `turns[-4:]`).
- Note: information can still leak through the assistant's own outputs (e.g., it keeps mentioning flamingos).
- **Keep raw conversations in the trajectory** — memory may be stripped or processed.

### Summarization
- Use another LLM to summarize the conversation history after each turn; add to a running summary.
- Summary shown together with the query.
- Summaries can still fill the context window over time (stacking), but slower than raw history. Re-summarizing can compress too much and remove important info.
- Variations: summarize last five conversations; maintain one summary updated after each conversation (partial summarization balances old summarized + new uncompressed info).
- Implementation: `SummarizationMemory` uses the `system` role to track the summary; after each assistant turn, asks an LLM to update the summary.

## 4.4 Long-Term Memory

### Retrieval-Augmented Generation (RAG)
- Most common method for giving agents/LLMs long-term memory.
- **Two stages: ingestion and inference.**
- **Ingestion:** external data (typically unstructured text) is embedded into numerical representations and stored in a **vector database**.
  - **Embedding model** — a special LLM variant that converts text into numeric vectors (**embeddings**) capturing semantic meaning; words/phrases with similar meaning get similar representations.
- **Inference (4 steps):**
  1. Embed the user's query (same embedding model).
  2. Compare embedded query to the external database; extract most relevant items (relevancy = similarity between query and external embeddings; hybrid systems can combine with Bag-of-Words approaches).
  3. Combine relevant items + user's query into the prompt (provides context for generation).
  4. Model generates output using the augmented prompt.
- **Purpose:** minimize **hallucination** (confidently producing incorrect answers) by providing external (assumed-true) information.
- **Cosine similarity** — measures the angle between embeddings; smaller angle = higher similarity. Formula: dot product of embeddings ÷ product of their lengths (normalization).
- **RAGMemory class:** (1) embed all external documents; (2) embed user's query; (3) compare embeddings, create a similarity matrix; (4) return highest-similarity documents (top-k); (5) add those documents to the prompt. Augments user queries with retrieved context.
- **Advantage/disadvantage:** minimizes context passed to model, but no guarantee the context is good enough — e.g., you could set a minimum similarity threshold instead of always taking top 3.

### MemoryBank
- RAG for chatbots; long-term external database rather than short-term conversation history.
- Memories continuously **updated** to selectively preserve memory, inspired by the **Ebbinghaus Forgetting Curve** theory:
  - Exponential decay — we lose ~half of what we learn each day.
  - **Spaced repetition** — actively recalling learned info frequently decreases the forgetting rate.
- Mechanism: if a memory item is retrieved/used during conversations, it **persists longer**; if not retrieved for a while, it may be **removed entirely**.
- Three variants of memory: **conversation history** (raw multi-turn), **summaries of past events** (LLM-generated), **user's portrait** (personality traits and emotions summarized by the LLM).
- Summaries and conversation turns are embedded for retrieval; user portrait is dynamically updated and always passed as context.
- Vanilla RAG = straightforward implementation; other forms: **Graph RAG**, **Multimodal RAG**.

### Agentic RAG
- **Vanilla RAG:** the LLM is only given info relevant to the query — it has **no agency** over what's retrieved (retrieval is a static step).
- **Agentic RAG:** an **agent** (not just an LLM) accesses the external database **as a tool** and controls what it retrieves. Knowledge sources become tools rather than a static pre-generation step.
- Single-agent: acts as a **router** deciding which of several knowledge sources to use; can run subsequent searches based on previous results.
- Multi-agent: multiple retrieval agents coordinated by a more capable agent; each specialized in specific sources (vector DB, web search, API like Slack/Gmail).
- Can dynamically decide **how many times** to query until it has enough context.
- **Table 4-1 (single vs multi-agent RAG):**
  - Single-agent advantages: cost-effective (fewer API calls), simpler system (easier to debug). Disadvantages: single point of failure, lower accuracy ceiling (context window overloaded).
  - Multi-agent advantages: modularity (specialized LLMs, easily replaced), higher accuracy ceiling (agents give each other feedback, check hallucinations). Disadvantages: higher costs (parallel agents), complexity (harder to find errors).

### A-MEM (agentic memory)
- Derived from the **Zettelkasten** note-taking method, with three components:
  - **Atomicity** — each Zettel (note) contains only one unit of knowledge (an "atom").
  - **Hypertextual notes** — notes refer to/expand each other, creating an interconnected web.
  - **Personalization** — notes tailored to one's own ideas.
- In agents, each note = a piece of memory containing: the original interaction (one turn), timestamp, LLM-generated **keywords**, LLM-generated **tags**, LLM-generated **contextual description**.
- All info (except timestamp) concatenated → single embedding per note. Timestamp kept as metadata.
- Note embedding used as one of the main IDs; links made via similarity search (Top-K) + LLM decides which candidate memories to link.
- After adding, the LLM updates tags/keywords/description based on the new memory → **evolutionary approach** (new memories link to old, old ones update).

### Search-o1 (agentic RAG during reasoning)
- Retrieves relevant context **during the LLM's reasoning process** (not just in the prompt).
- Agent instructed to use `<|begin_search_query|>`, `<|end_search_query|>`, `<|begin_search_result|>`, `<|end_search_result|>` tokens to search and mark retrieved info.
- Uses synchronous retrieval tools + structured model calls; can refine reasoning iteratively within a single call (autonomously, unlike regular agentic RAG which iterates over calls).
- **Problem:** retrieved documents can be large and contain irrelevant info, disrupting reasoning flow.
- **Solution: Reason-in-Documents module** — uses the search query, retrieved documents, and reasoning trace to condense info into focused reasoning steps; the reasoning LLM processes retrieved docs to align with its reasoning traces (compresses context, keeps trace flow intact).
- Example: "Why are flamingos pink?" → searches Wikipedia during reasoning → finds pigments from diet → second call to arXiv clarifies carotenoid pigments from brine shrimp.

## 4.5 Context Engineering
- **Definition:** finding the best context (input tokens) such that it maximizes the quality of the LLM's output for a given task. Optimizing input tokens to produce the best output tokens.
- The LLM can be seen as a **function**: context (tokens in) → output (tokens out). Optimize either the LLM (training/fine-tuning) or the input (context).
- **Prompt engineering** optimizes system/user prompts; **context engineering** optimizes the ENTIRE context.
- Sources of context: system prompt (procedural memory), conversation history + internal thoughts (working memory), past experiences (episodic memory), retrieved information via RAG (semantic memory).

### Large Context Windows ≠ Better
- Gemini 1.5 reached a 1M-token context window (Feb 2024) → temptation to fill it up.
- **Needle-in-a-haystack (NIAH) test** — place a random fact (needle) in the middle of a long context (haystack), ask the model to retrieve it. Measured retrieval accuracy across positions and lengths. Used by Claude 2.1 and Gemini 1.5. Shows degradation at higher context lengths (especially the middle).
- NIAH is only a retrieval task — not indicative of complex long-context understanding (e.g., reasoning over hundreds of thousands of tokens).
- **RULER benchmark** — introduces tasks like multi-hop tracing and aggregation; models that did well on NIAH showed significant drops on RULER at long contexts.
- **"Context rot"** — arbitrarily filling the context window hurts performance; even with quality info, models struggle to sift through it.
- **Cost/latency:** processing all tokens increases latency; more VRAM needed → higher cost. "Just dumping everything in the context is a recipe for failure."
- **Goal:** optimize the context with the right information, at the right place, in the right format — without overwhelming the model.

### Techniques for Optimizing Context
**1. Context tracking and storage**
- Track and store context first. Episodic memory (actions taken) relates to conversation history.
- Types to track: agent behavior (tool usage, tool outputs/intermediate results, inter-agent interactions, internal reasoning steps, conversation history, failures/successes); user behavior (intent, feedback: edits/approvals/rejections); knowledge sources (database snapshots, external documents, structured artifacts like PLAN.md/REQUIREMENTS.md); system-level (configuration, policies/guardrails). Include privacy and safety constraints.

**2. Context selection**
- Have a system that selects the right context.
- **Re-ranker** — often a language model; takes query + retrieved documents and re-ranks them by relevance to the query and to each other. Operates on far fewer documents than the whole DB. Used in deep research.
- Other methods: structure the output; business rules giving extra weight to certain info (like a system prompt); isolate context across specialized agents.

**3. Context compression**
- **Summarization** (LLM summaries of conversation history or RAG output).
- **Reducing redundancy:** top results may be very similar.
- **Maximal Marginal Relevance (MMR):**
  - Uses a **relevance vector** (similarity between query embedding and each document embedding) and a **redundancy matrix** (similarity between all pairs of retrieved document embeddings).
  - Formula: **MMR = λ · relevance − (1 − λ) · redundancy**
  - **λ (lambda)** controls diversity vs relevance (higher λ = more diversity... actually higher λ weights relevance; a higher redundancy penalty = higher diversity). Iteratively selects the most relevant-but-dissimilar documents.
  - Example: reduce 5 documents to 3.
- **Deduplication** — remove essentially duplicate contexts.

**4. Context ordering**
- **"Lost-in-the-middle" phenomenon** — LLMs pay more attention to the beginning and end of a prompt, losing information in the middle.
- Matches human behavior: **serial-position effect** — people recall first (primacy effect) and last (recency effect) items best, middle items worst.
- Context engineering helps prevent this (it usually appears with long contexts).

### Context as the Specification
- Shift in mindset: the context given to an agent is a **tool for communication** — to the agent AND to the people you work with.
- The query, PLAN.md, REQUIREMENTS.md, codebase, etc. all communicate your intention and serve as the **specification** of the feature/PR.
- As agents become more autonomous, track the *intention* behind their behavior through context.
- **Prompt engineering = user-facing; context engineering = developer-oriented.**
- Don't throw away the input (context) and only keep the output — context tracking aids reproducibility, communication, understanding WHY the agent chose certain tools/actions, and debugging (transparency of intention).
- **Domain-specific:** no single framework; healthcare needs different context than law (patient data vs research papers).

## Chapter 4 Key Takeaways
1. Memory is the foundation of agents; LLMs are stateless by default.
2. Four memory types: working, episodic, semantic, procedural (+ parametric).
3. Short-term memory: conversation history, trimming, summarization; context window limits.
4. Long-term memory: RAG (ingestion + 4-step inference), embeddings, cosine similarity, MemoryBank (forgetting curve/spaced repetition), agentic RAG (A-MEM, Search-o1).
5. Context engineering: right information, right place, right format; avoid context rot; NIAH vs RULER; re-ranking, MMR (λ·relevance − (1−λ)·redundancy), lost-in-the-middle, context as specification.

---

## High-Yield Vocabulary (Ch 3 & 4)
- **CoT (Chain-of-Thought)** — reasoning trace generated before the answer.
- **System 1 / System 2** — fast/intuitive vs slow/deliberate thinking (Kahneman).
- **Train-time compute / Test-time compute** — scaling during training vs inference.
- **FLOPs** — floating point operations per second (compute measure).
- **Scaling laws / power laws / diminishing returns** — performance vs scale relationships.
- **Kaplan / Chinchilla scaling laws** — model-size-vs-data trade-offs.
- **Elo** — chess-style rating of player/model strength.
- **Proposal distribution** — token probability distribution sampled during generation.
- **ORM / PRM** — outcome vs process reward model (verifiers).
- **One-shot / few-shot / zero-shot prompting** — number of examples in a prompt.
- **Self-consistency** — sample many answers, majority vote.
- **Best-of-N** — sample N, pick highest-scoring.
- **Rejection sampling** — use a reward model to select high-quality generated samples.
- **Cold start problem** — poor initial behavior when RL is applied without SFT.
- **Chat template / special tokens** — model-specific token format; `<|think|>` enables reasoning.
- **MCoT** — multimodal chain-of-thought (two-stage).
- **CoD (Chain-of-Draft)** — short, draft-like reasoning steps.
- **Hybrid reasoning** — on/off reasoning (`/think`, `/no_think`).
- **Latent space reasoning** — hidden-state-based thinking (no visible tokens).
- **Chain-of-Continuous-Thought / CODI** — latent reasoning methods.
- **Embeddings / embedding model** — numeric vectors capturing meaning.
- **Vector database** — DB storing embeddings for similarity search.
- **Cosine similarity** — angle-based similarity between vectors.
- **RAG** — retrieval-augmented generation.
- **Hallucination** — confident incorrect output.
- **Ebbinghaus forgetting curve / spaced repetition** — memory decay & review.
- **Agentic RAG** — agent controls retrieval as a tool.
- **A-MEM / Zettelkasten / atomicity / hypertextual notes** — agentic memory.
- **Search-o1 / Reason-in-Documents** — RAG during reasoning.
- **NIAH / RULER** — long-context benchmarks.
- **Context rot** — performance drop from filling context window.
- **Re-ranker** — model that reorders retrieved documents.
- **MMR** — Maximal Marginal Relevance (λ·relevance − (1−λ)·redundancy).
- **Lost-in-the-middle / serial-position effect / primacy / recency** — ordering effects.
