# AI Agents — Practice Exam
**Source:** *An Illustrated Guide to AI Agents* (Chapters 1 & 2)
**Instructions:** Allow ~60–75 minutes. Sections A (MCQ) and B (True/False) are quick checks; Section C is short answer; Section D is essay-style. Answers and marking notes are at the end — don't peek!

---

## Section A: Multiple Choice (1 point each)
*Circle the best answer.*

1. Which of the following best defines an AI agent per Russell & Norvig?
   a) A program that generates the most likely next token
   b) Anything that perceives its environment through sensors and acts upon that environment through actuators
   c) A model trained on web text to predict the next word
   d) A chatbot that responds to user messages

2. In an LLM-backed agent, the "agent program" (brain) is typically:
   a) The tokenizer
   b) The reward model
   c) A reasoning LLM
   d) The tool-calling software

3. Which of the following is NOT one of the four components at the heart of agents?
   a) Environment
   b) Sensors
   c) Context window
   d) Actuators

4. LLMs generate text one token at a time and feed their own output back as input. This is called:
   a) Backpropagation
   b) Autoregression
   c) Attention
   d) Fine-tuning

5. The main breakthrough of OpenAI o1 and DeepSeek R1 was:
   a) Larger pre-training datasets
   b) Generating explicit reasoning/thinking tokens before the answer
   c) New tokenizers with 200k vocabulary
   d) Fully autonomous tool use without prompts

6. "Train-time scaling" refers to:
   a) Training the model to reason at inference time
   b) Scaling data, compute, and parameters during training for better performance
   c) Reducing training time with KV caching
   d) Using more experts during inference

7. LLMs are said to be "stateless" because:
   a) They have no weights
   b) Information is not persisted across calls — they don't natively remember conversations
   c) They cannot process images
   d) They reset after every token

8. The simplest way to give an LLM memory is:
   a) Retraining the model
   b) Adding the previous conversation to the current prompt
   c) Increasing the temperature
   d) Using a bigger vocabulary

9. Why can't an LLM use tools by itself?
   a) It lacks internet access
   b) It is a text-in/text-out function that can only express intent, requiring external software to parse and execute it
   c) Tool calling requires RLHF
   d) It can, but only with temperature zero

10. "Context engineering" is best described as:
    a) Designing the tokenizer
    b) Balancing the amount and quality of information given to the LLM
    c) Engineering the context length of the model
    d) Choosing the correct decoding strategy

11. An agent that is "partially autonomous" can:
    a) Execute only a single step but freely choose from tools
    b) Delete files without guardrails
    c) Plan and reflect without user input
    d) Run fully without any human checks

12. Which pair are the two main evaluation lenses for agents?
    a) Accuracy and precision
    b) Outcome evaluation and trajectory evaluation
    c) Training loss and validation loss
    d) Reliability and latency

13. The sequence of steps (thought → action → observation) an agent takes is called its:
    a) Prompt
    b) Trajectory
    c) Decoding path
    d) Token stream

14. In multi-agent collaboration, the agent that manages communication and assigns tasks is called the:
    a) Router
    b) Supervisor agent
    c) Orchestrator model
    d) Reward model

15. Which two capabilities make an LLM multi-modal?
    a) Text generation and code generation
    b) Understanding multiple modalities and generating multiple modalities
    c) Pre-training and post-training
    d) Reasoning and planning

16. "Vibe coding" refers to:
    a) Coding with a very high temperature
    b) Non-developers relying on coding agents to build software
    c) Optimizing the KV cache while coding
    d) Using code LLMs only for refactoring

17. The three major components of a Transformer decoder LLM are:
    a) Tokenizer, Transformer blocks, LM head
    b) Encoder, decoder, attention
    c) Embedding, softmax, sampling
    d) Pre-training, SFT, RL

18. The LM head produces:
    a) A single token
    b) A probability distribution over the entire vocabulary
    c) The KV cache
    d) The final answer directly

19. A temperature of 0 results in:
    a) Always choosing the highest-probability token
    b) Random sampling
    c) No output
    d) The longest possible response

20. The number of token tracks that can flow through a model simultaneously is the:
    a) Vocabulary size
    b) Hidden dimension
    c) Context length/size
    d) Number of experts

21. KV caching speeds up inference by:
    a) Caching tokenizer outputs
    b) Reusing previously computed Keys and Values so only the new token's track is computed
    c) Storing all model weights in GPU memory
    d) Reducing the vocabulary

22. In self-attention, the three matrices are:
    a) Q, K, V (Queries, Keys, Values)
    b) W, X, Y
    c) Input, hidden, output
    d) Token, embedding, context

23. The self-attention formula is:
    a) softmax(Q + K) · V
    b) softmax(Q·Kᵀ / √d_k) · V
    c) softmax(V·Kᵀ) · Q
    d) Q·V·K

24. In the KV cache, which vectors do NOT need to be cached?
    a) Keys
    b) Values
    c) Queries
    d) All three must be cached

25. Multi-head Latent Attention (MLA) reduces KV cache memory by:
    a) Sharing K and V across heads
    b) Compressing K and V into a smaller Latent KV that is cached
    c) Dropping attention for long contexts
    d) Quantizing the tokenizer

26. DeepSeek Sparse Attention (DSA) selects which tokens to attend to using:
    a) A router and experts
    b) A Lightning Indexer and Top-K Selector
    c) Random sampling
    d) A reward model

27. In an MoE layer, the component that decides which expert processes a token is the:
    a) Expert
    b) Router (gate network)
    c) LM head
    d) Shared expert

28. In MoE terms, "sparse parameters" are:
    a) Only the parameters used for math
    b) All parameters that must be loaded into memory
    c) The parameters of the router only
    d) Parameters with zero gradients

29. The model name "Qwen3-30B-A3B" means:
    a) 30B active, 3B total
    b) 30B sparse (total), 3B active
    c) 30 billion tokens, 3 billion parameters
    d) 30 experts, 3 activated

30. Two load-balancing techniques in MoE are:
    a) Expert capacity and auxiliary loss
    b) KV cache and quantization
    c) Temperature and top-k sampling
    d) SFT and RLHF

31. RLHF uses which type of training signal?
    a) Verifiable automated rewards
    b) Preference scores from humans or reward models
    c) Format checks
    d) Next-token prediction

32. GRPO stands for and refers to:
    a) Gated Regular Policy Optimization; single-response scoring
    b) Group Relative Policy Optimization; generating a group of responses scored relative to each other
    c) Gradient Refined Policy Optimization; gradient clipping
    d) Group Reward Policy Optimization; shared experts

33. In SFT, which tokens are excluded from the loss computation?
    a) Completion tokens
    b) Prompt tokens
    c) Special tokens
    d) None

34. Which of the following is NOT one of the book's agent harness types?
    a) Terminal-based
    b) Code-based
    c) Quantum-based
    d) Personal assistant

35. Which is the most famous personal assistant harness mentioned in the book?
    a) Cursor
    b) OpenClaw
    c) Codex CLI
    d) Replit

---

## Section B: True/False (1 point each)
*Write T or F, and if false, correct the statement briefly.*

36. A base language model is the result of the post-training phase. (T/F)
37. System prompts are a privileged input that shape model behavior before user tokens. (T/F)
38. In RLVR, rewards are obtained from automated verifiers rather than human raters. (T/F)
39. The router's selected expert output is scaled by the router's probability before being passed forward. (T/F)
40. In MLA, RoPE is applied directly to the cached Latent K. (T/F)
41. Higher temperature always produces the most accurate responses. (T/F)
42. Agents with full autonomy always perform better than those with guardrails. (T/F)
43. Reliability in agents asks whether the agent succeeds every time, not just once. (T/F)
44. Multi-Query Attention uses separate K and V matrices per head. (T/F)
45. DeepSeek-R1 uses 256 experts with 8 selected per token, plus a shared expert. (T/F)

---

## Section C: Short Answer (2–3 points each)
*Answer in 2–5 sentences.*

46. Explain the difference between outcome evaluation and trajectory evaluation of an agent.
47. What is the difference between RLHF and RLVR? Give one example of a verifiable reward.
48. Why do we say "evaluating an agent is much more than evaluating a model"?
49. Explain what the KV cache is and why it matters for agent developers.
50. Describe how MLA (Multi-head Latent Attention) reduces KV cache memory and how it handles positional information (RoPE).
51. What is load balancing in MoE, why is it needed, and name two techniques used to achieve it?
52. Explain the difference between understanding and generating multiple modalities, and name the components used for each.
53. Why does the book teach reasoning and tool calling via prompting before using native fields like `reasoning` and `tool_calls`?
54. Describe the full path a piece of input text takes from tokenizer to generated token (autoregressive loop).
55. What is the "ceiling effect" of train-time scaling, and what breakthrough addressed it?

---

## Section D: Essay / Applied (5 points each)
*Write structured answers with definitions, explanations, and examples where possible.*

56. **Components of an agent.** Define an AI agent using the Russell & Norvig definition, list its four core components, and explain how each maps to an LLM-backed agent (brain, tools, sensors, environment/user). Then explain how adding memory, tools, planning, and reflection turns a "regular" LLM into an agent.

57. **Training pipeline.** Describe the full training pipeline of an LLM: pre-training (next-token prediction), SFT, and reinforcement learning (RLHF and RLVR). Include what each phase produces, what data/signal it uses, and how GRPO fits into this picture.

58. **The Transformer.** Explain the three components of a Transformer decoder LLM (tokenizer, Transformer blocks, LM head). Then describe what happens inside a Transformer block — self-attention and the feed-forward network — and explain the role of context length and the KV cache for agent developers.

59. **Efficient attention.** Compare Multi-head Attention, Multi-Query Attention, Grouped-Query Attention, Multi-head Latent Attention (MLA), and DeepSeek Sparse Attention (DSA). For MLA and DSA, explain the specific mechanisms (Latent KV, Lightning Indexer, Top-K Selector) and their trade-offs.

60. **MoE.** Explain the Mixture-of-Experts architecture: experts, router, dense vs sparse models, sparse vs active parameters, and load balancing (expert capacity, auxiliary loss). Use DeepSeek-R1 and a model like Qwen3-30B-A3B as examples.

---

## ANSWER KEY

### Section A: Multiple Choice
1. b — "Anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."
2. c — A reasoning LLM is the brain/agent program.
3. c — Context window is not one of the four components (Environment, Sensors, Actuators, Agent program).
4. b — Autoregression.
5. b — Reasoning LLMs generate explicit reasoning tokens before the answer (test-time compute).
6. b — Scaling data, compute, and parameters during training.
7. b — Stateless: information not persisted across calls.
8. b — Adding previous conversation to the current prompt.
9. b — Text-in/text-out functions can only express intent; external software parses/executes.
10. b — Balancing amount and quality of information given to the LLM.
11. a — Partial autonomy = execute a single step but freely choose tools.
12. b — Outcome and trajectory evaluation.
13. b — Trajectory.
14. b — Supervisor agent.
15. b — Understanding and generating multiple modalities.
16. b — Non-developers relying on coding agents to build software.
17. a — Tokenizer, Transformer blocks, LM head.
18. b — Probability distribution over the vocabulary.
19. a — Greedy: always highest-probability token.
20. c — Context length/size.
21. b — Reuses cached K and V; only new token's track computed.
22. a — Queries, Keys, Values.
23. b — softmax(Q·Kᵀ / √d_k) · V.
24. c — Queries do not need caching (only the latest token's query is used).
25. b — Compresses K and V into a smaller Latent KV.
26. b — Lightning Indexer + Top-K Selector.
27. b — Router (gate network).
28. b — All parameters that must be loaded into memory.
29. b — 30B sparse (total), 3B active.
30. a — Expert capacity and auxiliary loss.
31. b — Preference scores from humans or reward models.
32. b — Group Relative Policy Optimization; group of responses scored relative to each other.
33. b — Prompt tokens are excluded; only completion tokens are trained on.
34. c — Quantum-based is not a harness type (types: terminal, code, personal assistant, hosted, UI).
35. b — OpenClaw.

### Section B: True/False
36. **F** — A base model is the result of the **pre-training** phase. Post-training (SFT/RL) shapes it.
37. **T** — System prompt is a privileged input that shapes behavior before user tokens.
38. **T** — RLVR uses automated verifiers (format, correctness).
39. **T** — Selected expert output is scaled by the router's probability.
40. **F** — RoPE is applied to a decoupled component of Latent Q (via a separate small key); NOT to Latent K (which would break caching).
41. **F** — Higher temperature increases randomness/sampling; it does not guarantee accuracy (temperature 0 = deterministic/greedy).
42. **F** — Full autonomy can be harmful/overkill; guardrails often make systems more effective and safe.
43. **T** — Reliability = succeeds every time, not just once (outputs are stochastic).
44. **F** — MQA shares K and V across all query heads (GQA shares across groups).
45. **T** — 256 experts, 8 selected, plus a shared expert.

### Section C: Short Answer (model answers)
46. **Outcome vs trajectory evaluation.** Outcome evaluation asks whether the task actually got done — e.g., was the message sent or the record updated. Trajectory evaluation looks at the steps and tool calls the agent took to get there, judged on efficiency and soundness even when the outcome is correct. Example: an agent that gets the right answer by deleting files has a good outcome but a terrible trajectory.

47. **RLHF vs RLVR.** RLHF uses preference scores collected from humans or reward models (preferred vs rejected completions) and updates the model toward preferred outputs. RLVR uses automated verifiers to score outputs — e.g., checking a required format (like `<answer>42</answer>`) or whether the final answer equals a known-correct value (e.g., a math problem's answer). RLVR scales to domains with objectively defined right/wrong.

48. **Why evaluating an agent ≠ evaluating a model.** A model is evaluated on single text outputs with benchmarks and scores. An agent reasons over multiple steps, calls tools, and takes action sequences in an environment — so a single quality score on final text rarely captures whether the job was done. You must evaluate the whole system: the outcome, the trajectory (efficiency/soundness), plus reliability (succeeds consistently) and safety (avoids harm).

49. **KV cache.** After the first forward pass, the model caches the Keys and Values of previously seen tokens. Each subsequent generation step then processes only the new token's track (and its query), reusing cached K and V instead of recomputing everything (which vanilla attention does). This dramatically increases speed and reduces cost — key concerns for agent developers whose agents make many tool calls over long contexts.

50. **MLA.** MLA compresses the input embeddings into low-rank Latent Q and Latent KV representations; the smaller Latent KV is cached instead of full K and V (often combined with quantization). For positional info, RoPE is applied to a decoupled component of Latent Q and carried by a separate small key — NOT applied to Latent K, because a positional key would have to be recomputed every step, breaking the caching benefit. Content and positional components are concatenated and passed through standard multi-head attention.

51. **Load balancing in MoE.** If the router always picks the same experts, the others become undertrained. Load balancing distributes tokens across experts. Two techniques: (1) **expert capacity** — caps how many tokens each expert processes per batch; overflow tokens are sent to the next-highest-scoring expert; (2) **auxiliary loss** — a loss term that rewards equal distribution across experts (or penalizes repeatedly choosing the same one); adding Gaussian noise before router probabilities is another simple approach.

52. **Understanding vs generating modalities.** Understanding multiple modalities = the LLM can reason about text, images, audio, video simultaneously; implemented with an **encoder** (converts modalities into numeric representations) and a **connector** (links those representations to the LLM). Generating non-text output uses a **generator**. Understanding lets an agent "see" its environment (e.g., a website design); generating lets it respond in a modality other than text (e.g., voice).

53. **Why prompt-first.** (1) Using built-in `reasoning` and `tool_calls` fields is "magical" and doesn't teach how these capabilities are created and used — prompting shows the underlying mechanism. (2) Some models don't support those native fields but can still act as agents with proper prompting. So prompting builds understanding and robustness, before exploring native capabilities.

54. **Path of input text.** Input text → tokenizer splits into tokens (from a fixed vocabulary) → tokens looked up in the embeddings table → embedding vectors flow through a stack of Transformer blocks (each with self-attention + feed-forward layers) → final token's output vector goes to the LM head → LM head produces a probability distribution over the vocabulary → a decoding strategy selects the next token (temperature-dependent) → token is appended to input → the loop repeats (autoregression) until the response completes.

55. **Ceiling effect and the fix.** Continuously scaling model size (train-time compute) hit a point where gains were small relative to cost — a ceiling effect. The breakthrough was **reasoning LLMs** (o1, DeepSeek R1) that spend additional test-time compute generating explicit reasoning tokens ("thinking out loud") before answering, unlocking multi-step reasoning without brute-force scaling.

### Section D: Essay (grading notes)
56. **Components of an agent.** Expect: the Russell & Norvig definition; the four components (environment, sensors, actuators, agent program) defined; LLM mapping (brain = reasoning LLM, actuators = tools, sensors = multimodal interpretation, user = part of environment); then the augmentation story — memory (statelessness fix), tools (text-in/text-out → intent + parsing), planning (task decomposition), reflection (updating plans) — culminating in the definition: reasoning LLM + memory + tools + planning + reflection = AI agent.
57. **Training pipeline.** Expect: pre-training (next-token prediction → base model, e.g., GPT-3); SFT/instruction tuning (prompt-completion pairs, prompt tokens excluded from loss); RL — RLHF (human/reward-model preferences, preferred vs rejected) and RLVR (automated verifiers); GRPO (group of responses, relative rewards, varied temperature). Include the math example (format 0.3 / accuracy 0.7) if remembered.
58. **Transformer.** Expect: three components (tokenizer, Transformer blocks, LM head) with roles; inside a block: self-attention (context gathering, Q/K/V, relevance scoring + combining) and feed-forward (factual associations); context length limits input → context engineering; KV cache (cache K/V, only new token computed) → speed and cost for agents.
59. **Efficient attention.** Expect a comparison table: MHA (full K/V per head), MQA/GQA (share K/V across heads, less memory, some accuracy loss), MLA (low-rank Latent KV compression, RoPE decoupled to avoid breaking cache), DSA (Lightning Indexer + Top-K Selector, sparse attention over subset of tokens, e.g., 2048). Trade-offs: memory vs accuracy vs compute.
60. **MoE.** Expect: experts (smaller FFNNs, specialized) + router (softmax over experts, output scaled by probability); dense vs sparse; sparse (loaded) vs active (used) parameters; load balancing (expert capacity, auxiliary loss); examples — DeepSeek-R1 (256 experts, 8 active, shared expert), Qwen3-30B-A3B naming, and others from Table 2-1 (e.g., Mistral 8x7B, Llama 4 Maverick, GPT-OSS).

---

### Scoring Guide
- Section A: 35 pts | Section B: 10 pts | Section C: 30 pts (choose any ~10 questions or weight proportionally) | Section D: 20–30 pts.
- **85–100%**: Strong. Review only your missed items.
- **70–84%**: Good. Re-read the study notes for weak areas (likely training phases or attention variants).
- **<70%**: Re-read both chapters and the study notes, then retry this exam in 2–3 days.
