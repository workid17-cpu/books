# AI Agents — Flashcards / Q&A
**Source:** *An Illustrated Guide to AI Agents* (Chapters 1 & 2)
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.

---

## Section A: Term → Definition

**Q1. What is an AI agent? (Russell & Norvig definition)**
**A:** "An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."

**Q2. What are the four components at the heart of an agent?**
**A:** Environment, Sensors, Actuators, and Agent program.

**Q3. In an LLM-backed agent, what does the agent program (brain) typically map to?**
**A:** A reasoning LLM.

**Q4. In an LLM-backed agent, what are the actuators?**
**A:** The LLM's tools.

**Q5. What are the sensors in an LLM-backed agent?**
**A:** Multimodal LLM capabilities that interpret more than text (images, sound, etc.).

**Q6. What is a token?**
**A:** The unit of LLM input/output — a word, part of a word, number, or punctuation (e.g., "flamingos" → "flamingo" + "s").

**Q7. What is an autoregressive model?**
**A:** A model that consumes its own previously generated output as input for generating the next token, one token at a time.

**Q8. What is train-time scaling?**
**A:** Improving LLM performance by scaling up data, compute, and parameters during training (e.g., GPT-3.5 lineage).

**Q9. What is the "ceiling effect" of train-time scaling?**
**A:** Continuously scaling model size becomes too expensive for the small performance gains it produces.

**Q10. What is test-time compute / reasoning in LLMs?**
**A:** Spending additional compute to generate explicit "thoughts" (reasoning tokens) before producing the answer — as in OpenAI o1 and DeepSeek R1.

**Q11. Why are LLMs considered "stateless"?**
**A:** Information is not persisted across calls; they do not natively remember past conversations.

**Q12. What is the simplest way to give an LLM memory?**
**A:** Adding the previous conversation to the current prompt.

**Q13. What is context engineering?**
**A:** Carefully balancing the amount and quality of information given to the LLM to help it tackle its task (avoiding information overload).

**Q14. Why can't an LLM use tools by itself?**
**A:** Because an LLM is a text-in/text-out function — it can only describe or show intent (e.g., output `multiply(2.3, 8.1)` as text); external software must parse and execute that text.

**Q15. What is the "augmented LLM" (Anthropic's term)?**
**A:** A reasoning LLM augmented with memory and tools — the building block that becomes an agent.

**Q16. What is planning / task decomposition?**
**A:** Breaking a large task into smaller, actionable steps, executing them one at a time while referring back to the plan.

**Q17. What is reflection in an agent?**
**A:** The agent uncovering faults in its past behavior/plan and attempting to fix them, updating the initial plan as it executes.

**Q18. What is the iterative agent loop involving planning and reflection?**
**A:** Plan → take actions → reflect on the output → update the plan.

**Q19. What are guardrails?**
**A:** Constraints on agent autonomy that steer it toward expected behaviors and prevent destructive actions (e.g., deleting important files).

**Q20. What is a hallucination?**
**A:** An LLM confidently generating incorrect information.

**Q21. What are the two main lenses for evaluating agents?**
**A:** Outcome evaluation (did the task get done?) and trajectory evaluation (were the steps/tool calls efficient and sound?).

**Q22. What two additional evaluation properties don't surface in a single run?**
**A:** Reliability (succeeds every time, given stochastic outputs) and Safety (avoids harm).

**Q23. What is a supervisor agent in multi-agent collaboration?**
**A:** An agent that manages communication among agents and is typically responsible for advanced behavior like planning, decomposing, and assigning tasks — often with the most capable LLM.

**Q24. What two capabilities make an LLM multi-modal?**
**A:** Understanding multiple modalities (via an encoder + connector) and generating multiple modalities (via a generator).

**Q25. What is "vibe coding"?**
**A:** Non-software engineers relying on coding agents to build software.

**Q26. What is an agent harness? Give the five types.**
**A:** The code/software implementing agent behavior. Types: terminal-based, code-based, personal assistant, hosted, and UI-based.

**Q27. Name the personal assistant harness examples given in the book.**
**A:** OpenClaw (~300k GitHub stars in months) and Hermes Agent.

**Q28. What is a base language model?**
**A:** The result of the pre-training phase (next-token prediction on vast text data).

**Q29. What is the purpose of the system prompt?**
**A:** A privileged input that shapes model behavior before it sees user tokens, letting deployers customize behavior without retraining the model.

**Q30. What fields does the OpenAI-compatible endpoint standardly include?**
**A:** base_url, api_key, model, and messages.

**Q31. What are the two models used in the book, and why?**
**A:** Gemma 3 12B (no native reasoning/tool calling) and Gemma 4 E4B (trained for reasoning and tool calling) — to teach both prompt-driven and model-driven approaches.

**Q32. What is supervised fine-tuning (SFT)?**
**A:** The first post-training phase, also called instruction tuning: training on prompt-completion pairs so the model follows instructions.

**Q33. In SFT, which tokens are excluded from the loss?**
**A:** The prompt tokens — only the completion (response) tokens are trained on.

**Q34. What is RLHF?**
**A:** Reinforcement Learning from Human Feedback — preferences from humans or reward models (preferred vs rejected completions) used to update the model.

**Q35. What is RLVR?**
**A:** Reinforcement Learning with Verifiable Rewards — rewards come from automated verifiers (e.g., format checks or known-correct answers), scalable to domains with objective right/wrong.

**Q36. What is GRPO?**
**A:** Group Relative Policy Optimization — generates a group of responses per prompt with varied temperatures and rewards them relative to the group, reinforcing higher-scoring ones.

**Q37. In the book's RLVR math example, what are the two reward components?**
**A:** Format reward (0.3 if correct format else 0) and accuracy reward (0.7 if correct answer else 0).

**Q38. What are the three major components of a Transformer decoder LLM?**
**A:** Tokenizer, stack of Transformer blocks, and language modeling (LM) head.

**Q39. What does the LM head do?**
**A:** Converts the final representations into a probability distribution over the entire vocabulary for the next token.

**Q40. What does a temperature of 0 do? What does higher temperature do?**
**A:** Temperature 0 = always choose the highest-probability token (greedy). Higher temperature = sampling, where higher-probability tokens are more likely but not certain.

**Q41. What is context length / context size?**
**A:** The number of token tracks that can flow through the model simultaneously — limits how much information can be in the input.

**Q42. What is KV caching (prompt/prefix caching)?**
**A:** Caching previously computed K and V so each generation step processes only the single new token's track, dramatically speeding up inference.

**Q43. What are the two components inside a Transformer block?**
**A:** A self-attention layer and a feed-forward neural network layer.

**Q44. What does the feed-forward network store? Give the book's example.**
**A:** Factual associations learned during training — e.g., "The Shawshank" → predicts "Redemption".

**Q45. What problem does self-attention solve? Give the book's example.**
**A:** Context/ambiguity — e.g., "The dog chased the llama because it": resolving what "it" refers to.

**Q46. What are the two steps of self-attention?**
**A:** (1) Relevance scoring — score how relevant each previous token is; (2) Combining information — blend relevant positions into the current token's vector proportional to their scores.

**Q47. What three matrices does self-attention use, and how are they produced?**
**A:** Queries, Keys, and Values — produced by multiplying input vectors by three learned projection matrices.

**Q48. Write the self-attention formula.**
**A:** `Attention = softmax(Q·Kᵀ / √d_k) · V`

**Q49. Which vectors need caching and which don't in the KV cache?**
**A:** K and V are cached; Q is not needed (only the latest token's query is used).

**Q50. What is the main redundancy in vanilla attention?**
**A:** It recomputes the K and V vectors for every previous token at each generation step.

**Q51. How do Multi-Query Attention (MQA) and Grouped-Query Attention (GQA) reduce memory?**
**A:** By sharing K and V across query heads (MQA: all heads; GQA: groups of heads) — at some cost to accuracy.

**Q52. What is Multi-head Latent Attention (MLA)?**
**A:** An attention variant (DeepSeek-V2/R1) using low-rank joint compression of K and V into a smaller Latent KV that is cached instead of full K and V.

**Q53. In MLA, why is RoPE not applied directly to Latent K?**
**A:** Because the cached K would then need to be recomputed every step, breaking the caching benefit. RoPE is applied to a decoupled component of Latent Q, with positional info carried by a separate small key.

**Q54. What are the two components of DeepSeek Sparse Attention (DSA)?**
**A:** A Lightning Indexer (scores relevance of preceding tokens to the query) and a Top-K Selector (keeps only the top-K KV entries).

**Q55. What are the two main components of a Mixture-of-Experts (MoE) layer?**
**A:** Experts (smaller feed-forward networks) and a Router (gate network) that decides which expert processes each token.

**Q56. In MoE, what are sparse parameters vs active parameters?**
**A:** Sparse parameters = all parameters loaded into memory; active parameters = those activated during inference. MoE models run faster because only a few experts are active per token.

**Q57. What does a model name like Qwen3-30B-A3B mean?**
**A:** 30B sparse (total) parameters, 3B active parameters.

**Q58. What two load-balancing techniques prevent expert domination?**
**A:** Expert capacity (capping tokens per expert per batch; overflow goes to next-highest-scoring expert) and auxiliary loss (rewarding equal distribution / penalizing repeated selection).

**Q59. Describe DeepSeek-R1's MoE configuration.**
**A:** 256 experts, 8 selected per token, plus a shared expert that always bypasses the router (absorbs general knowledge).

**Q60. What does the router do, and what happens to its output probability?**
**A:** It produces a softmax probability distribution over experts, selects the highest-probability expert, and the selected expert's output is scaled by the router's probability before passing forward.

---

## Section B: Short-Answer Questions (Concept Checks)

**Q61. Why is reasoning so important for AI agents?**
**A:** Agents must plan, select tools, reflect on mistakes, and revise plans — all of which require advanced reasoning. Reasoning LLMs are especially good at complex decision-making, multi-step problem decomposition, and generalizing to novel problems. Trade-off: "regular" LLMs are preferred when fast, cheap responses are needed.

**Q62. Explain the difference between understanding and generating multiple modalities.**
**A:** Understanding = the LLM can reason about several modalities simultaneously (e.g., "see" a website design); implemented with an encoder (converts modalities into numeric representations) plus a connector (links representations to the LLM). Generating = producing output in a non-text modality; implemented with a generator.

**Q63. Why does the book teach reasoning and tool calling via prompting before native model capabilities?**
**A:** (1) Using built-in `reasoning`/`tool_calls` fields is "magical" and doesn't teach how they work; (2) some models don't support those fields but can still act as agents with proper prompting. This builds understanding of what's happening under the hood.

**Q64. What is the difference between the `answer` and `observation` fields in a Step?**
**A:** `answer` is the final answer of the model and signals the end of the agent's turn; `observation` is the output of a tool call (consequence of executing an action).

**Q65. How does a full agent loop work (per Chapter 2's example)?**
**A:** (1) User asks a question; (2) agent calls a web search tool (pulls info from environment); (3) agent processes the retrieved information and decides it has enough; (4) agent prints the answer to the user.

**Q66. Why is evaluating an agent harder than evaluating an LLM?**
**A:** Agents reason over multiple steps, call tools, and take sequences of actions — a single quality score on final text rarely captures whether the job was done. You must evaluate the whole system: outcome, trajectory, reliability, and safety.

**Q67. Why do autonomy and guardrails need to be balanced?**
**A:** Full autonomy can be overkill for the task or harmful. A system with many guardrails is often more effective because it steers the agent toward expected behaviors and away from undesired ones (also, human-in-the-loop becomes more necessary as autonomy grows).

**Q68. Explain the role of the LM head and decoding in generation.**
**A:** After processing through the Transformer blocks, the LM head converts the final token representation into a probability distribution over the whole vocabulary. A decoding strategy (e.g., temperature) then selects one token, which is appended and the loop repeats.

**Q69. Why does the KV cache reduce inference cost?**
**A:** Without it, vanilla attention recomputes K and V for all previous tokens at every step. The KV cache reuses them, so only the new token's K, V, and Q are computed — less computation, faster speed, and reduced cost.

**Q70. How does MLA combine content and positional information?**
**A:** Content and positional components are concatenated after splitting Q, K, V across heads and projecting up, then fed through standard multi-head attention. RoPE is applied to a decoupled part of Latent Q (via a separate small key) so cached K stays cached.

**Q71. What makes an agent "multi-agent" vs "single-agent"?**
**A:** Multi-agent systems deploy multiple different agents, each responsible for different tasks, that interact and consult each other's specialties (often under a supervisor). Single-agent systems are one entity.

**Q72. Why would you choose a reasoning LLM vs a regular LLM?**
**A:** Reasoning LLMs: better at complex, multi-step, novel problems — but slower/more expensive. Regular LLMs: fast and cheap, fine for straightforward tasks.

**Q73. Describe the flow of tokens through a Transformer LLM.**
**A:** Text → tokenizer → token IDs → embeddings → Transformer blocks (self-attention + feed-forward per block, in sequence) → LM head → probability distribution → decoding → next token appended → repeat (autoregressive).

**Q74. What is the difference between a dense model and a sparse (MoE) model?**
**A:** A dense model has a single feed-forward network applied to every token (everything active). A sparse model has many smaller expert networks with a router activating only a subset per token — more total capacity without activating everything, so faster inference.

**Q75. Why does "load balancing" matter in MoE, and what happens without it?**
**A:** Without balancing, the same experts get chosen repeatedly, leaving other experts undertrained. Techniques like expert capacity and auxiliary loss ensure the router distributes tokens so all experts are properly trained.

---

## Section C: Fill-in-the-Blank

**Q76.** The LLM first breaks input into ______, which are subcomponents of words.
**A:** tokens

**Q77.** Generating one token at a time, feeding each output back as input, is called ______.
**A:** autoregression (autoregressive generation)

**Q78.** Models like OpenAI o1 and DeepSeek R1 generate ______ before deriving their final answer.
**A:** reasoning tokens (thoughts / a reasoning trace)

**Q79.** LLMs are ______ entities — information is not persisted across calls.
**A:** stateless

**Q80.** The discipline of carefully balancing what information to give an LLM is called ______.
**A:** context engineering

**Q81.** RLHF relies on ______ collected from humans or reward models.
**A:** preference scores

**Q82.** GRPO generates a ______ of responses per prompt and computes rewards ______ to that group.
**A:** group; relative

**Q83.** In the RLVR math example, the format reward is ______ and the accuracy reward is ______.
**A:** 0.3 (if correct format); 0.7 (if correct answer)

**Q84.** A model with a context length of 100,000 can have ______ tokens flowing through it simultaneously.
**A:** 100,000

**Q85.** Only the ______ token's output is passed to the LM head to predict the next token.
**A:** final (last)

**Q86.** The two components inside each Transformer block are ______ and ______.
**A:** self-attention layer; feed-forward neural network layer

**Q87.** In MoE, the ______ decides which expert processes each token; selected output is scaled by the router's ______.
**A:** router (gate network); probability

**Q88.** DeepSeek-R1 uses ______ experts, of which ______ are selected per token, plus one ______ expert.
**A:** 256; 8; shared (always-active)

**Q89.** In the book's example, self-attention resolves the ambiguity of "it" in "The dog chased the llama because it" by attending back to ______.
**A:** llama ("lama")

**Q90.** Qwen3-30B-A3B means ______ billion sparse parameters and ______ billion active parameters.
**A:** 30; 3
