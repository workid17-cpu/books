# AI Agents — Flashcards / Q&A (Chapter 2)
**Source:** *An Illustrated Guide to AI Agents* (Chapter 2: Large Language Models)
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.
**Note:** Question numbers are retained from the combined source file (`02_flashcards_qna.md`) for traceability. This file contains only the Chapter 2 questions.

---

## Section A: Term → Definition

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

**Q63. Why does the book teach reasoning and tool calling via prompting before native model capabilities?**
**A:** (1) Using built-in `reasoning`/`tool_calls` fields is "magical" and doesn't teach how they work; (2) some models don't support those fields but can still act as agents with proper prompting. This builds understanding of what's happening under the hood.

**Q64. What is the difference between the `answer` and `observation` fields in a Step?**
**A:** `answer` is the final answer of the model and signals the end of the agent's turn; `observation` is the output of a tool call (consequence of executing an action).

**Q65. How does a full agent loop work (per Chapter 2's example)?**
**A:** (1) User asks a question; (2) agent calls a web search tool (pulls info from environment); (3) agent processes the retrieved information and decides it has enough; (4) agent prints the answer to the user.

**Q68. Explain the role of the LM head and decoding in generation.**
**A:** After processing through the Transformer blocks, the LM head converts the final token representation into a probability distribution over the whole vocabulary. A decoding strategy (e.g., temperature) then selects one token, which is appended and the loop repeats.

**Q69. Why does the KV cache reduce inference cost?**
**A:** Without it, vanilla attention recomputes K and V for all previous tokens at every step. The KV cache reuses them, so only the new token's K, V, and Q are computed — less computation, faster speed, and reduced cost.

**Q70. How does MLA combine content and positional information?**
**A:** Content and positional components are concatenated after splitting Q, K, V across heads and projecting up, then fed through standard multi-head attention. RoPE is applied to a decoupled part of Latent Q (via a separate small key) so cached K stays cached.

**Q73. Describe the flow of tokens through a Transformer LLM.**
**A:** Text → tokenizer → token IDs → embeddings → Transformer blocks (self-attention + feed-forward per block, in sequence) → LM head → probability distribution → decoding → next token appended → repeat (autoregressive).

**Q74. What is the difference between a dense model and a sparse (MoE) model?**
**A:** A dense model has a single feed-forward network applied to every token (everything active). A sparse model has many smaller expert networks with a router activating only a subset per token — more total capacity without activating everything, so faster inference.

**Q75. Why does "load balancing" matter in MoE, and what happens without it?**
**A:** Without balancing, the same experts get chosen repeatedly, leaving other experts undertrained. Techniques like expert capacity and auxiliary loss ensure the router distributes tokens so all experts are properly trained.

---

## Section C: Fill-in-the-Blank

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
