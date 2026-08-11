# AI Agents — Practice Exam (Chapter 4)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 4 "Memory"
**Instructions:** Allow ~35–45 minutes. Section A is multiple choice; B is true/false; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

24. LLMs without memory modules are described as:
    a) Stateless / forgetful
    b) Persistent
    c) Episodic
    d) Procedural

25. Which memory type corresponds to "specific actions the agent has taken and their outcomes"?
    a) Working
    b) Semantic
    c) Episodic
    d) Procedural

26. Which memory type corresponds to "the capital of France"?
    a) Episodic
    b) Semantic
    c) Working
    d) Parametric only

27. Knowledge stored in the model's parameters is called:
    a) Working memory
    b) Parametric memory
    c) Episodic memory
    d) RAG

28. The context window is:
    a) The number of output tokens
    b) The max tokens an LLM can process (input + output)
    c) The vocabulary size
    d) The embedding dimension

29. Trimming as a memory technique:
    a) Summarizes history with an LLM
    b) Removes the first few interactions when history grows too large
    c) Embeds documents
    d) Adds more context

30. Summarization as a memory technique:
    a) Removes messages
    b) Uses an LLM to compress conversation history into a running summary
    c) Increases context window
    d) Re-ranks documents

31. RAG stands for:
    a) Retrieval-Augmented Generation
    b) Reinforcement-Augmented Generation
    c) Reasoned Agentic Generation
    d) Ranked Attention Gradient

32. The two stages of RAG are:
    a) Training and inference
    b) Ingestion and inference
    c) Embedding and decoding
    d) Search and vote

33. Cosine similarity measures:
    a) The distance between embeddings
    b) The angle between embeddings (smaller angle = more similar)
    c) The number of shared tokens
    d) The context length

34. RAG primarily helps reduce:
    a) Latency
    b) Hallucination
    c) Temperature
    d) Token count

35. The Ebbinghaus Forgetting Curve is often shown as:
    a) Linear
    b) Exponential (losing about half of what we learn each day)
    c) Logarithmic growth
    d) Flat

36. Spaced repetition:
    a) Increases forgetting
    b) Decreases the pace of forgetting via frequent recall
    c) Removes memories
    d) Fills the context window

37. MemoryBank decides memory persistence based on:
    a) Random chance
    b) How often a memory is retrieved/used
    c) Memory size
    d) Token count

38. Agentic RAG differs from vanilla RAG because:
    a) It uses larger models
    b) The agent accesses databases as tools and controls retrieval
    c) It uses no embeddings
    d) It only works in multi-agent systems

39. In A-MEM, atomicity means:
    a) Notes are huge
    b) Each note contains one unit of knowledge
    c) Notes have timestamps
    d) Notes are embedded

40. Search-o1 performs RAG:
    a) Before generation only
    b) During the reasoning process (using search tokens + Reason-in-Documents)
    c) Never
    d) After generation

41. Context engineering optimizes:
    a) Only the system prompt
    b) The entire context (input tokens) to maximize output quality
    c) The model weights
    d) The temperature

42. The needle-in-a-haystack test:
    a) Tests math reasoning
    b) Places a fact in a long context and asks the model to retrieve it
    c) Measures latency
    d) Trains embeddings

43. "Context rot" refers to:
    a) Models forgetting old info
    b) Performance dropping when the context window is arbitrarily filled
    c) Embedding decay
    d) Serial-position effect only

44. A re-ranker:
    a) Re-orders retrieved documents by relevance
    b) Re-embeds the query
    c) Trims messages
    d) Summarizes history

45. The MMR formula is:
    a) relevance + redundancy
    b) λ · relevance − (1 − λ) · redundancy
    c) λ · redundancy − (1 − λ) · relevance
    d) relevance / redundancy

46. The "lost-in-the-middle" phenomenon means:
    a) The middle of the prompt is best attended
    b) Beginning and end of prompts are attended best, middle lost
    c) Only the end is attended
    d) Order doesn't matter

47. The serial-position effect involves:
    a) Primacy and recency effects
    b) Only recency
    c) Context rot
    d) Episodic memory

48. Viewing context as "the specification" means:
    a) Context is throwaway
    b) The context communicates your intention and should be tracked
    c) Only output matters
    d) Context is only for debugging

---

## Section B: True/False (1 point each)

65. Working memory is a type of long-term memory. (T/F)
66. Procedural memory for agents can be the system prompt or parameters. (T/F)
67. The context window counts both input and output tokens. (T/F)
68. RAG's ingestion stage embeds data and stores it in a vector database. (T/F)
69. MemoryBank is based on the Ebbinghaus Forgetting Curve. (T/F)
70. In vanilla RAG, the agent controls what is retrieved. (T/F)
71. Multi-agent RAG is always cheaper than single-agent RAG. (T/F)
72. A-MEM is derived from the Zettelkasten note-taking method. (T/F)
73. The RULER benchmark only tests simple retrieval. (T/F)
74. Filling the entire context window always improves performance. (T/F)
75. Prompt engineering optimizes the entire context, not just prompts. (T/F)

---

## Section C: Short Answer (2–3 points each)

86. List and describe the four types of agent memory.
87. What is the difference between short-term and long-term memory in agents?
88. Describe trimming and summarization as short-term memory techniques, including trade-offs.
89. Explain the two stages of RAG and the four steps of inference.
90. What is cosine similarity and how is it used in RAG?
91. Explain MemoryBank and how the forgetting curve/spaced repetition inform it.
92. What is agentic RAG, and how does it differ from vanilla RAG (including single vs multi-agent)?
93. Describe A-MEM and its three Zettelkasten principles.
94. What is Search-o1 and the Reason-in-Documents module?
95. Define context engineering and contrast it with prompt engineering.
96. Why is simply filling a huge context window a bad idea? Mention NIAH, RULER, context rot, cost/latency.
97. Explain the MMR formula and what λ controls.
98. What is the "lost-in-the-middle" phenomenon and the serial-position effect?
99. Why should context be treated as "the specification" of an agent's task?
100. List the four techniques for optimizing context and briefly describe each.

---

## Section D: Essay / Applied (5 points each)

105. **Memory types.** Explain the four types of agent memory (working, episodic, semantic, procedural) plus parametric memory, with human and agent examples for each. Explain why memory is foundational for agents.

106. **RAG.** Explain Retrieval-Augmented Generation end to end: ingestion (embedding model, vector database), inference (4 steps), cosine similarity, and how RAG reduces hallucination. Then contrast vanilla RAG with agentic RAG (A-MEM, Search-o1) and single vs multi-agent RAG pros/cons.

107. **Context engineering.** Define context engineering and contrast with prompt engineering. Discuss why large context windows are not a silver bullet (NIAH, RULER, context rot, cost/latency). Describe the four optimization techniques: tracking/storage, selection (re-ranking), compression (summarization, MMR, deduplication), and ordering (lost-in-the-middle). Explain "context as the specification."

---

## ANSWER KEY

### Section A: Multiple Choice
24. a — Stateless/forgetful.
25. c — Episodic.
26. b — Semantic.
27. b — Parametric memory.
28. b — Max tokens (input + output).
29. b — Remove first interactions.
30. b — LLM compresses history into a summary.
31. a — Retrieval-Augmented Generation.
32. b — Ingestion and inference.
33. b — Angle between embeddings.
34. b — Hallucination.
35. b — Exponential.
36. b — Decreases forgetting via recall.
37. b — How often a memory is retrieved/used.
38. b — Agent accesses databases as tools and controls retrieval.
39. b — One unit of knowledge per note.
40. b — During reasoning (Reason-in-Documents).
41. b — The entire context (input tokens).
42. b — Fact in long context, retrieve it.
43. b — Performance drops when context arbitrarily filled.
44. a — Re-orders documents by relevance.
45. b — λ·relevance − (1−λ)·redundancy.
46. b — Beginning/end attended best.
47. a — Primacy and recency.
48. b — Context communicates intention and should be tracked.

### Section B: True/False
65. **F** — Working memory is short-term.
66. **T** — System prompt or parameters.
67. **T** — Input + output tokens.
68. **T** — Embed + store in vector DB.
69. **T** — Ebbinghaus forgetting curve.
70. **F** — In vanilla RAG, retrieval is a static step; the agent has no agency. (Agentic RAG gives agency.)
71. **F** — Multi-agent is generally HIGHER cost (parallel agents).
72. **T** — Derived from Zettelkasten.
73. **F** — RULER tests multi-hop tracing, aggregation, etc. (beyond simple retrieval).
74. **F** — Context rot; filling the window hurts performance.
75. **F** — Prompt engineering optimizes prompts; CONTEXT engineering optimizes the entire context.

### Section C: Short Answer (model answers)
86. **Four memory types.** Working (chat history, short-term); episodic (past actions/outcomes); semantic (world knowledge, external DBs); procedural (how-to patterns: parameters/system prompt). Plus parametric memory (knowledge in weights).
87. **Short vs long-term.** Short-term = recent interactions/chat history (limited capacity, needs trimming/summarization). Long-term = external databases (RAG), memories of actions and knowledge persisting over time.
88. **Trimming vs summarization.** Trimming removes old messages (simple, but may lose early important info). Summarization compresses history via an LLM (efficient, but stacking summaries can still grow and re-summarizing can lose info).
89. **RAG stages.** Ingestion: embed unstructured text and store in a vector database. Inference: (1) embed query; (2) retrieve most relevant items; (3) combine into prompt; (4) generate answer.
90. **Cosine similarity.** Angle between embedding vectors; smaller angle = higher similarity. Computed as dot product ÷ (product of lengths). Used to rank retrieved documents.
91. **MemoryBank.** Long-term memory updated based on the Ebbinghaus forgetting curve; used memories persist longer, unused ones may be removed; spaced repetition principle. Maintains conversation history, LLM summaries, and a user portrait.
92. **Agentic RAG.** The agent treats databases as tools and decides what/when/how often to retrieve (agency), vs vanilla RAG's static step. Single-agent = router over sources; multi-agent = specialized retrieval agents coordinated by a capable agent. Pros/cons per Table 4-1 (single: cheap/simple but single point of failure; multi: modular/higher accuracy but costly/complex).
93. **A-MEM.** Zettelkasten-based: atomicity (one atom per note), hypertextual notes (links), personalization. Each note = interaction + timestamp + LLM keywords/tags/description, embedded; similarity search links notes; LLM updates links/tags after adding.
94. **Search-o1.** Retrieves info during reasoning using search tokens; Reason-in-Documents module condenses retrieved documents into focused reasoning steps so the flow stays intact.
95. **Context engineering.** Optimizing the entire input context (tokens) to maximize output quality; prompt engineering optimizes only prompts. Context engineering is developer-oriented, uses tracking/storage, selection, compression, ordering.
96. **Why not fill the window.** NIAH (retrieval test) overstates capability; RULER shows drops on harder tasks; context rot — performance falls as the window fills; cost/latency — more tokens = more processing, more VRAM. 
97. **MMR.** MMR = λ·relevance − (1−λ)·redundancy, using a relevance vector (query–doc similarity) and redundancy matrix (doc–doc similarity); iteratively selects relevant-but-diverse docs; λ balances relevance vs diversity.
98. **Lost-in-the-middle / serial-position.** LLMs (and humans) attend best to beginning and end; middle info is lost (primacy + recency effects).
99. **Context as specification.** The context communicates your intention and serves as the feature/PR specification; tracking it enables reproducibility, debugging, and understanding "why" the agent acted.
100. **Four optimization techniques.** (1) Tracking/storage — capture agent/user/knowledge/system info; (2) selection — RAG + re-ranking; (3) compression — summarization, MMR, deduplication; (4) ordering — place key info at beginning/end (avoid lost-in-the-middle).

### Section D: Essay (grading notes)
105. Expect: four types with human + agent examples (working = chat history; episodic = past actions/outcomes; semantic = external knowledge/DBs; procedural = params/system prompt), plus parametric memory; why foundational (no memory → no tools/planning).
106. Expect: full RAG pipeline (embedding model, vector DB, 4 inference steps, cosine similarity, hallucination reduction), then vanilla vs agentic RAG, A-MEM and Search-o1 details, single vs multi-agent pros/cons.
107. Expect: definition, prompt vs context engineering; large-window pitfalls (NIAH, RULER, context rot, cost/latency); four techniques with details (tracking, re-ranking selection, compression via summarization/MMR/dedup, ordering/lost-in-the-middle); context as specification with PLAN.md/REQUIREMENTS.md example and debugging value.

---

### Scoring Guide
- Sections A + B: 36 pts | Section C: 30–45 pts (choose ~9–12) | Section D: 15 pts.
- **85–100%**: Strong. Review missed items only.
- **70–84%**: Good. Re-read study notes for weak areas.
- **<70%**: Re-read chapters + study notes, retry in 2–3 days.
