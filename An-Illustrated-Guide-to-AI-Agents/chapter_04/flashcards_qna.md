# AI Agents — Flashcards / Q&A (Chapter 4)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 4 "Memory"
**How to use:** Cover the answer, test yourself, then reveal.

---

## Section A: Term → Definition

**Q39. Why is memory a key component for agents?**
**A:** LLMs are stateless/forgetful; without memory an agent can't remember conversations, understand a codebase, or avoid repeating actions.

**Q40. What are the four types of memory for agents?**
**A:** Working memory (short-term) + three long-term types: episodic, semantic, and procedural.

**Q41. What is working memory in LLMs?**
**A:** Short-term memory with limited capacity — typically the chat history continuously fed back to the LLM; data that persists across LLM calls.

**Q42. What is episodic memory in agents?**
**A:** Remembering specific events/experiences — the specific actions the agent has taken and their outcomes.

**Q43. What is semantic memory in agents?**
**A:** Knowledge about the world — querying external databases like Wikipedia or the codebase.

**Q44. What is procedural memory in agents?**
**A:** Patterns of how to do things — info hidden in its parameters (parametric memory) or the system prompt, persisting across calls.

**Q45. What is parametric memory?**
**A:** Knowledge already stored in the model's parameters (e.g., capital of France = Paris). Can be instilled via SFT but is not stable.

**Q46. What is the context window?**
**A:** The maximum number of tokens an LLM can process, including both input AND output tokens.

**Q47. What is trimming (as a memory technique)?**
**A:** Removing the first few interactions when the history grows too large, keeping only the most recent turns.

**Q48. What is summarization (as a memory technique)?**
**A:** Using another LLM to compress the conversation history into a running summary.

**Q49. What is RAG?**
**A:** Retrieval-Augmented Generation — the most common method for giving LLMs/agents long-term memory; consists of ingestion and inference.

**Q50. What is an embedding model?**
**A:** A special LLM variant that converts text into numerical vectors (embeddings) capturing semantic meaning; similar meanings → similar representations.

**Q51. What is a vector database?**
**A:** A database storing embeddings that can be queried for similar items; considered the LLM's long-term memory.

**Q52. What is cosine similarity?**
**A:** A measure of the angle between two embeddings; a smaller angle = higher similarity. Computed as dot product ÷ product of vector lengths.

**Q53. What is hallucination?**
**A:** An LLM confidently producing an answer that is actually incorrect. RAG helps minimize it.

**Q54. What is the Ebbinghaus Forgetting Curve?**
**A:** A curve (often exponential) showing the pace at which we forget — about half of what we learn is lost each day.

**Q55. What is spaced repetition?**
**A:** Actively recalling learned information frequently to decrease the pace of forgetting.

**Q56. What is MemoryBank?**
**A:** An RAG mechanism giving LLMs long-term memory that is continuously updated; inspired by the forgetting curve — used memories persist longer, unused ones may be removed.

**Q57. What is agentic RAG?**
**A:** An agent (not just an LLM) accesses external databases as tools and controls what information it retrieves (vs vanilla RAG's static retrieval step).

**Q58. What is A-MEM?**
**A:** An agentic memory system derived from Zettelkasten note-taking; uses atomicity and hypertextual notes; each note = one interaction with LLM-generated keywords, tags, description.

**Q59. What is atomicity (Zettelkasten/A-MEM)?**
**A:** Each note (Zettel) contains only one unit of knowledge — an "atom."

**Q60. What are hypertextual notes?**
**A:** Notes that refer to each other and expand on each other's content, creating an interconnected web.

**Q61. What is Search-o1?**
**A:** Agentic RAG performed DURING reasoning — retrieving context inside the reasoning trace using special search tokens, with a Reason-in-Documents module to compress retrieved docs into focused reasoning steps.

**Q62. What is context engineering?**
**A:** Finding the best context (input tokens) that maximizes the quality of the LLM's output for a given task — optimizing the entire context, not just the prompt.

**Q63. What is the needle-in-a-haystack (NIAH) test?**
**A:** Placing a random fact (needle) in the middle of a long context (haystack) and asking the model to retrieve it; measures long-context retrieval accuracy.

**Q64. What is the RULER benchmark?**
**A:** A long-context benchmark introducing tasks like multi-hop tracing and aggregation, going beyond simple retrieval; shows models dropping at long contexts.

**Q65. What is "context rot"?**
**A:** The finding that arbitrarily filling the context window hurts LLM performance.

**Q66. What is a re-ranker?**
**A:** A model (often an LLM) that takes the query + retrieved documents and re-ranks them by relevance to the query and to each other.

**Q67. What is Maximal Marginal Relevance (MMR)?**
**A:** A technique balancing relevance and diversity of retrieved documents: **MMR = λ · relevance − (1 − λ) · redundancy**, using a relevance vector and redundancy matrix.

**Q68. What is the "lost-in-the-middle" phenomenon?**
**A:** LLMs attend more to the beginning and end of prompts, losing information placed in the middle.

**Q69. What is the serial-position effect?**
**A:** People (and LLMs) recall the first (primacy effect) and last (recency effect) items best, middle items worst.

**Q70. Why is context considered "the specification"?**
**A:** The context given to an agent (query, PLAN.md, REQUIREMENTS.md, codebase) communicates your intention and serves as the specification of what the agent should build/do — a tool for communication and debugging.

---

## Section B: Short-Answer Questions

**Q79. Why is memory more than just conversation history?**
**A:** Memory includes past actions (episodic), external knowledge like documentation/issues (semantic), skills/procedures (procedural), and parameter knowledge (parametric). It also involves storing new info and deciding what to forget.

**Q80. How does the LLM "remember" in conversation memory?**
**A:** It doesn't truly remember — the entire conversation history is explicitly inserted into the prompt (via the messages structure) on every call.

**Q81. Explain the two stages of RAG.**
**A:** Ingestion: embed external unstructured text with an embedding model and store in a vector database. Inference (4 steps): (1) embed the query; (2) compare to the database and extract the most relevant items; (3) combine relevant items + query into the prompt; (4) generate the answer.

**Q82. How does MemoryBank decide what to remember?**
**A:** Based on the Ebbinghaus forgetting curve. Retrieved/used memories persist longer; memories not retrieved for a while may be removed. Also maintains conversation history, LLM-generated summaries of past events, and a dynamically updated "user portrait."

**Q83. What is the difference between vanilla RAG and agentic RAG?**
**A:** In vanilla RAG, retrieval is a static step — the LLM only receives pre-selected relevant info and has no agency over retrieval. In agentic RAG, an agent accesses databases as tools and dynamically decides what, when, and how many times to retrieve.

**Q84. What are the pros/cons of single-agent vs multi-agent RAG?**
**A:** Single-agent: cost-effective, simpler to debug; but single point of failure and lower accuracy ceiling. Multi-agent: modularity, higher accuracy ceiling (agents check each other's hallucinations); but higher cost and complexity.

**Q85. How does Search-o1 improve RAG during reasoning?**
**A:** It retrieves info inside the reasoning trace (using `<|begin_search_query|>`-style tokens) and uses a Reason-in-Documents module to condense retrieved documents into focused reasoning steps, keeping the reasoning flow intact.

**Q86. What is context engineering vs prompt engineering?**
**A:** Prompt engineering optimizes the system/user prompts only; context engineering optimizes the ENTIRE context (prompts + conversation history + retrieved info + past experiences + tools). Context engineering is developer-oriented; prompt engineering is more user-facing.

**Q87. Why is simply filling a huge context window a bad idea?**
**A:** (1) "Context rot" — performance drops as the window fills (NIAH overestimates, RULER shows drops). (2) Cost/latency — processing all tokens increases latency and VRAM cost. (3) Models struggle to sift through too much information.

**Q88. Explain the MMR formula and lambda.**
**A:** MMR = λ · relevance − (1 − λ) · redundancy. The relevance vector scores each document's similarity to the query; the redundancy matrix scores document-to-document similarity. λ tunes the balance between relevance and diversity when iteratively selecting documents.

**Q89. What is the "lost-in-the-middle" problem and how does it relate to human memory?**
**A:** LLMs (like humans via the serial-position effect) attend best to the start and end of a prompt and worst to the middle. It appears with long contexts; context engineering (placing key info strategically) mitigates it.

**Q90. Why should you track the context you give an agent (not just its output)?**
**A:** For reproducibility, communication, and debugging. The context (query, PLAN.md, codebase) is the specification of your intention; tracking it lets you understand WHY the agent chose certain tools/actions. We shouldn't "throw away the input to our function."

---

## Section C: Fill-in-the-Blank

**Q105.** The four memory types for agents are ______, ______, ______, and ______.
**A:** working; episodic; semantic; procedural

**Q106.** The Ebbinghaus Forgetting Curve is often shown as ______, losing about ______ of what we learn each day.
**A:** exponential; half

**Q107.** RAG has two stages: ______ and ______.
**A:** ingestion; inference

**Q108.** The MMR formula is ______.
**A:** MMR = λ · relevance − (1 − λ) · redundancy

**Q109.** The ______ phenomenon describes LLMs losing information placed in the middle of prompts.
**A:** lost-in-the-middle

**Q110.** The serial-position effect describes the ______ effect (first items) and ______ effect (last items).
**A:** primacy; recency
