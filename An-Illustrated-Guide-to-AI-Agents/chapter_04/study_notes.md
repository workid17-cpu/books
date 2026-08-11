# AI Agents — Comprehensive Study Notes (Chapter 4)
**Source:** *An Illustrated Guide to AI Agents* (O'Reilly, Early Release)
**Scope:** Chapter 4 (Memory)

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

## High-Yield Vocabulary (Chapter 4)
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
