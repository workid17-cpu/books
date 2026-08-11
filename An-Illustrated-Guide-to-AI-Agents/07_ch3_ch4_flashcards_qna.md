# AI Agents — Flashcards / Q&A (Chapters 3 & 4)
**Source:** *An Illustrated Guide to AI Agents*, Chapters 3 & 4
**How to use:** Cover the answer, test yourself, then reveal.

---

## Section A: Term → Definition

**Q1. What is reasoning in LLMs?**
**A:** Mimicking human thinking by generating "thoughts" (reasoning tokens) before giving a final answer — the chain-of-thought that breaks a problem into smaller steps.

**Q2. What is System 1 thinking (Kahneman)?**
**A:** Automatic, quick thinking relying on intuition and learned associations for snap judgments.

**Q3. What is System 2 thinking (Kahneman)?**
**A:** Slower, more deliberate reasoning requiring conscious effort and attention.

**Q4. Which LLM type maps to System 1 vs System 2?**
**A:** Non-reasoning LLMs ≈ System 1 (fast, no explicit step-by-step); Reasoning LLMs ≈ System 2 (deliberate step-by-step analysis).

**Q5. What is train-time compute?**
**A:** Scaling resources (model size/parameters, dataset size/tokens, compute/FLOPs) during pre-training and post-training.

**Q6. What is test-time compute?**
**A:** Scaling resources (compute) spent during inference — letting the reasoning LLM "think longer."

**Q7. What is a FLOP?**
**A:** Floating point operation — a unit of computational work; FLOPs = operations per second.

**Q8. What is a scaling law?**
**A:** The relationship between a model's scale (compute, dataset size, parameters) and its performance, often a power law.

**Q9. What is diminishing returns in scaling laws?**
**A:** Each doubling of compute gives smaller gains than the previous doubling — a logarithmic relationship.

**Q10. What is the Kaplan Scaling Law?**
**A:** Performance improves predictably with compute/data/parameters but with diminishing returns; for a fixed compute budget, increase model size and train on as much data as possible without overfitting.

**Q11. What is the Chinchilla Scaling Law?**
**A:** Models were often undertrained; for a fixed compute budget, use a smaller model trained on much more data.

**Q12. What is Elo?**
**A:** A rating system (used in chess) that estimates a player's strength based on past results.

**Q13. What are the two categories of test-time compute in the book?**
**A:** (1) Search against verifiers and (2) modifying proposal distribution.

**Q14. What is "search against verifiers"?**
**A:** Sampling many answers/reasoning traces and selecting the best using a reward model (verifier). Output-focused.

**Q15. What is "modifying proposal distribution"?**
**A:** Tuning or prompting (mainly training) the model so it outputs better reasoning steps. Input-focused. The proposal distribution = the token probabilities sampled during generation.

**Q16. What is a reward model (RM / verifier)?**
**A:** A model (often a fine-tuned LLM) or rule-based system that scores the quality of a generated answer and/or reasoning trace.

**Q17. What is an Outcome Reward Model (ORM)?**
**A:** Judges only the final outcome; ignores intermediate reasoning steps.

**Q18. What is a Process Reward Model (PRM)?**
**A:** Judges only the intermediate reasoning steps (process) leading to the outcome.

**Q19. What is Chain-of-Thought (CoT) prompting?**
**A:** Prompting a model to explain its reasoning step-by-step; one of the first techniques to elicit reasoning in models not trained for it.

**Q20. One-shot vs few-shot vs zero-shot prompting?**
**A:** One-shot = a single example; few-shot = two or more examples (higher accuracy); zero-shot = no examples (e.g., "Let's think step-by-step").

**Q21. What is self-consistency?**
**A:** Sampling a number of answers (high temperature + CoT) and selecting the most frequent answer via majority vote. Uses NO verifier.

**Q22. What is Best-of-N samples?**
**A:** Generate N candidate answers; a verifier evaluates each; select the highest-scoring one. Can use ORM (score answers) or PRM (score reasoning traces).

**Q23. What is triplet-like data in SFT for reasoning?**
**A:** Training data containing the user's query, a reasoning trace, and an answer.

**Q24. What is Flan?**
**A:** "Fine-tuning language models" — instruction templates over 1,800+ tasks used to fine-tune LLMs; produced Flan-PaLM (from PaLM, 540B params).

**Q25. What is the s1 method?**
**A:** "Simple test-time scaling" — created a reasoning LLM with only 1,000 questions + reasoning traces (fine-tuning Qwen2.5 32B), using `<|im_start|>think` / `<|im_start|>answer` special tokens.

**Q26. What is DeepSeek-R1-Zero?**
**A:** An experimental model trained from DeepSeek-V3-Base using ONLY reinforcement learning (no SFT on reasoning data). Suffered from the "cold start" problem (language mixing, poor readability).

**Q27. What is the cold start problem?**
**A:** When RL is applied without initial SFT guidance, the model produces poor initial behavior (mixing languages, poor readability).

**Q28. What are the five training steps of DeepSeek-R1?**
**A:** (1) Cold start prevention, (2) reasoning-oriented reinforcement learning, (3) rejection sampling, (4) supervised fine-tuning, (5) reinforcement learning for all scenarios.

**Q29. What is rejection sampling?**
**A:** Generating many samples and using a reward model to select high-quality ones (used to create DeepSeek-R1's 800,000-sample dataset).

**Q30. What is a chat template?**
**A:** A model's special token format for differentiating roles (system/user/model) and enabling behaviors. E.g., Gemma 4's `<|turn>system`, `<turn|>`, `<|think|>`.

**Q31. What does the `<|think|>` token do?**
**A:** Adding it to the system turn ENABLES reasoning; removing it DISABLES reasoning (Gemma 4).

**Q32. What is Multimodal Chain-of-Thought (MCoT)?**
**A:** A two-stage framework combining text + vision: (1) generate explicit reasoning from language + visual input; (2) append the rationale to the original language input and infer the final answer.

**Q33. What is Chain-of-Draft (CoD)?**
**A:** Efficient reasoning using concise intermediate thoughts — each reasoning step kept to a short draft (~5 words). Shorter traces, similar performance.

**Q34. What is a token-budget-aware LLM?**
**A:** A model trained to adaptively change reasoning trace length based on problem complexity, often using length rewards.

**Q35. What is hybrid reasoning (Qwen-3)?**
**A:** An on/off switch for reasoning using `/think` and `/no_think` special tokens; thinking mode uses CoT, non-thinking mode answers directly.

**Q36. What is latent space reasoning?**
**A:** Making explicit CoT internal — the model thinks in hidden representations (its "mind's eye") rather than visible reasoning tokens, going straight from question to answer.

**Q37. What is Chain-of-Continuous-Thought?**
**A:** A latent-space reasoning method that skips decoding; generates the last hidden state and uses it directly as input (no visible tokens until `<eot>`).

**Q38. What is CODI?**
**A:** Continuous Chain-of-Thought via Self-Distillation — trains a teacher LLM (explicit CoT) and student LLM (latent space) simultaneously; explicit reasoning is implicitly taught to the student.

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

**Q71. Why is reasoning critical for AI agents?**
**A:** It allows agents to plan behavior, decide which actions to take, and reflect on actions taken. It also enables tool selection (Ch 5) and planning/reflection (Ch 6). Without reasoning, reflection methods are less accurate.

**Q72. Why did the field shift from train-time to test-time compute?**
**A:** Scaling laws (Kaplan, Chinchilla) show diminishing returns — compute/data/model-size growth stopped giving linear gains (2024 plateau). Test-time compute (more "thinking") proved to scale performance similarly or further (OpenAI post; AlphaZero/Hex study; s1).

**Q73. Explain the two steps/forms of Best-of-N samples.**
**A:** Generate N candidate answers at high/varying temperature. (1) ORM version: score only the answers (LLM, unit tests, compiler) and pick the highest-scoring. (2) PRM version: score only the reasoning traces, average per trace, and pick the answer with highest-scoring traces.

**Q74. Why does self-consistency work even without a verifier?**
**A:** It selects the most frequent answer via majority vote; sampling many answers reduces the chance of selecting an infrequent, incorrect answer. Limitation: for tasks the LLM seldom gets right, it won't help.

**Q75. How does "modifying proposal distribution" differ from "search against verifiers"?**
**A:** Modifying the proposal distribution is input-focused — it trains/prompts the model (SFT or RL) to re-rank tokens so reasoning tokens are more likely. Search against verifiers is output-focused — it generates many outputs and scores them with a reward model, choosing the best. (Training = learned behavior; search = sampling + scoring.)

**Q76. What was the cold start problem in DeepSeek-R1-Zero, and how did DeepSeek-R1 fix it?**
**A:** R1-Zero applied RL directly to the base model without SFT, causing language mixing and poor readability. R1 added a cold-start-prevention SFT step (~5,000 high-quality CoT samples) before RL, plus a language-consistency reward.

**Q77. Compare ORM and PRM. When might you mix them?**
**A:** ORM judges only the final outcome; PRM judges only intermediate reasoning steps. PRM gives credit for good process (and can catch errors the final answer hides); ORM is simpler. A mix is often preferred because process quality and outcome quality can differ.

**Q78. What does the chat template of a model do, and how does it enable reasoning?**
**A:** It formats roles (system/user/model) with special tokens. Reasoning can be enabled/disabled by adding/removing a special token like `<|think|>` in the system turn. It removes the need for prompting "tricks" like CoT because the model was trained on CoT examples.

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

**Q91.** Reasoning LLMs generate ______ before giving a final answer.
**A:** thoughts / reasoning tokens (chain-of-thought)

**Q92.** Non-reasoning LLMs operate much like ______ thinking; reasoning LLMs like ______ thinking.
**A:** System 1; System 2

**Q93.** The three factors of train-time compute are ______, ______, and ______.
**A:** model size (parameters); dataset size (tokens); compute (FLOPs)

**Q94.** Scaling laws often take the form of ______ with ______.
**A:** power laws; diminishing returns (logarithmic relationships)

**Q95.** The Chinchilla Scaling Law says for a fixed compute budget, use a ______ model trained on ______ data.
**A:** smaller; much more

**Q96.** In Jones's Hex study, train-time compute = more ______ and epochs; test-time compute = deeper ______.
**A:** parameters; tree search

**Q97.** Search against verifiers is ______-focused; modifying proposal distribution is ______-focused.
**A:** output; input

**Q98.** An ______ judges only the final answer; a ______ judges only the reasoning steps.
**A:** Outcome Reward Model (ORM); Process Reward Model (PRM)

**Q99.** Appending "______" to a prompt is a form of zero-shot prompting.
**A:** Let's think step-by-step

**Q100.** Self-consistency selects the most ______ answer by ______.
**A:** frequent; majority vote

**Q101.** DeepSeek-R1's training used ~5,000 samples for ______, then GRPO with format, accuracy, and ______ rewards.
**A:** cold start prevention (SFT); language-consistency/preference (helpfulness, harmlessness)

**Q102.** In Gemma 4's chat template, adding ______ to the system turn enables reasoning.
**A:** `<|think|>`

**Q103.** Chain-of-Draft keeps each reasoning step to about ______ words.
**A:** five

**Q104.** Qwen-3's hybrid reasoning uses ______ and ______ tokens.
**A:** `/think`; `/no_think`

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
