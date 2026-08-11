# AI Agents — Practice Exam (Chapter 3)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 3 "Reasoning Large Language Models"
**Instructions:** Allow ~35–45 minutes. Section A is multiple choice; B is true/false; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. Which Kahneman system does a reasoning LLM most resemble?
   a) System 1 — automatic and quick
   b) System 2 — slow and deliberate
   c) Neither
   d) Both equally

2. The three factors of train-time compute are:
   a) Parameters, tokens, FLOPs
   b) Context length, temperature, sampling
   c) Weights, biases, activations
   d) RLHF, SFT, RLVR

3. Test-time compute refers to:
   a) Scaling data during pre-training
   b) Scaling compute spent during inference (thinking longer)
   c) Increasing the number of epochs
   d) Using a bigger tokenizer

4. Scaling laws generally take the form of:
   a) Exponential growth
   b) Power laws with diminishing returns
   c) Linear relationships
   d) Random distributions

5. According to the Chinchilla Scaling Law, for a fixed compute budget:
   a) Use the largest model possible
   b) Use a smaller model trained on much more data
   c) Double the compute
   d) Add more experts

6. In Jones's AlphaZero/Hex study, which finding supports test-time scaling?
   a) Only train-time compute matters
   b) Test-time compute alone is sufficient
   c) For a target Elo score, train-time and test-time compute must be balanced
   d) Elo scores are irrelevant

7. The two categories of test-time compute are:
   a) Pre-training and post-training
   b) Search against verifiers and modifying proposal distribution
   c) SFT and RLHF
   d) CoT and CoD

8. Which is output-focused?
   a) Modifying proposal distribution
   b) Search against verifiers
   c) Prompt engineering
   d) System prompts

9. An ORM (Outcome Reward Model) judges:
   a) Only the intermediate reasoning steps
   b) Only the final outcome
   c) Both steps and outcome
   d) The system prompt

10. A PRM (Process Reward Model) judges:
    a) The final answer only
    b) The intermediate reasoning steps (process)
    c) The temperature
    d) The tokenizer

11. Which prompting technique uses two or more examples?
    a) Zero-shot
    b) One-shot
    c) Few-shot
    d) Cold-start

12. Appending "Let's think step-by-step" is a form of:
    a) Few-shot prompting
    b) One-shot prompting
    c) Zero-shot prompting
    d) Rejection sampling

13. Self-consistency selects the best answer by:
    a) Using a reward model
    b) Majority vote over sampled answers
    c) Tree search
    d) Temperature 0

14. Which method requires NO reward model/verifier?
    a) Best-of-N with ORM
    b) Best-of-N with PRM
    c) Self-consistency
    d) Search against verifiers

15. Best-of-N samples involves:
    a) Sampling N answers and picking the highest-scoring one
    b) Sampling one answer with N reasoning steps
    c) Voting over N prompts
    d) Using N reward models

16. What did Flan-PaLM demonstrate?
    a) RL alone produces reasoning
    b) SFT on instruction templates (incl. CoT traces) across 1,800+ tasks improves reasoning
    c) Reasoning requires 1M samples
    d) Latent-space reasoning

17. The s1 method created a reasoning LLM with only:
    a) 5,000 samples
    b) 1,000 questions + reasoning traces
    c) 800,000 samples
    d) 10 samples

18. What was the "cold start" problem of DeepSeek-R1-Zero?
    a) Slow training
    b) Language mixing and poor readability from RL without SFT guidance
    c) Too much markdown formatting
    d) Overfitting

19. Which is the correct order of DeepSeek-R1's pipeline?
    a) RL → SFT → RL → SFT → RL
    b) Cold start prevention → reasoning RL → rejection sampling → SFT → RL for all scenarios
    c) SFT → RL → SFT → RL → SFT
    d) Rejection sampling → SFT → RL → SFT → RL

20. In Gemma 4's chat template, reasoning is enabled by:
    a) Adding `<|think|>` to the system turn
    b) Removing `<|turn|>`
    c) Setting temperature to 0
    d) Adding `<bos>`

21. Multimodal Chain-of-Thought (MCoT) is a:
    a) One-step process
    b) Two-stage framework (reasoning stage + final answer stage)
    c) RL-only approach
    d) Compression technique

22. Chain-of-Draft (CoD) differs from CoT by:
    a) Using more verbose reasoning
    b) Using short, draft-like reasoning steps (~5 words)
    c) Reasoning in latent space
    d) Removing all reasoning

23. Which describes latent space reasoning?
    a) Generating visible reasoning tokens
    b) Thinking in hidden representations with no visible reasoning tokens
    c) Using markdown files for plans
    d) Retrieval during reasoning

49. In DeepSeek-R1-Zero, the rewards used were:
    a) Human preference only
    b) Accuracy reward + format reward (rule-based)
    c) Length rewards
    d) Random

50. Which is NOT a type of test-time compute technique?
    a) Self-consistency
    b) Best-of-N
    c) Modifying proposal distribution
    d) Increasing pre-training data

---

## Section B: True/False (1 point each)

51. Reasoning LLMs always outperform non-reasoning LLMs. (T/F)
52. Train-time compute applies to both pre-training and post-training. (T/F)
53. Higher test-time compute always means generating more thinking tokens. (T/F)
54. On a log-log scale, a power law becomes a straight line. (T/F)
55. The Kaplan law suggests using the smallest model possible. (T/F)
56. ORMs ignore reasoning steps and judge only the outcome. (T/F)
57. Zero-shot prompting generally performs better than few-shot prompting. (T/F)
58. Self-consistency uses a reward model to pick the best answer. (T/F)
59. Flan-PaLM used instruction templates across more than 1,800 tasks. (T/F)
60. DeepSeek-R1-Zero used supervised fine-tuning before RL. (T/F)
61. In Gemma 4, removing `<|think|>` disables reasoning. (T/F)
62. Chain-of-Draft produces longer reasoning traces than CoT. (T/F)
63. Qwen-3's hybrid reasoning uses `/think` and `/no_think` tokens. (T/F)
64. Latent space reasoning produces visible reasoning tokens. (T/F)

---

## Section C: Short Answer (2–3 points each)

76. Explain the difference between System 1 and System 2 thinking and how they map to LLMs.
77. What are scaling laws? Explain Kaplan vs Chinchilla and what they concluded.
78. Describe the three steps of "search against verifiers."
79. Compare ORM and PRM. Give an example scenario where PRM would be more useful.
80. What is the difference between one-shot, few-shot, and zero-shot prompting? Which is generally most accurate?
81. Explain how self-consistency works and why it works without a verifier.
82. Describe DeepSeek-R1-Zero's training approach and its "cold start" problem.
83. List the five steps of the DeepSeek-R1 training pipeline.
84. What is native reasoning via chat templates? How does the `<|think|>` token work?
85. What is latent space reasoning, and what are Chain-of-Continuous-Thought and CODI?

---

## Section D: Essay / Applied (5 points each)

101. **Test-time compute.** Define train-time vs test-time compute. Explain scaling laws (power laws, diminishing returns), the Kaplan and Chinchilla findings, and the evidence for test-time scaling (OpenAI post, AlphaZero/Hex study, s1). Why did 2024–2025 mark a paradigm shift?

102. **Two categories of test-time compute.** Compare "search against verifiers" and "modifying proposal distribution." Include: output-focused vs input-focused, reward models (ORM/PRM), the methods under each (self-consistency, Best-of-N vs SFT/RL), and an example for each.

103. **DeepSeek-R1.** Describe DeepSeek-R1-Zero (RL-only, rewards, cold-start problem) and the full five-step DeepSeek-R1 pipeline (cold start prevention, reasoning RL, rejection sampling, SFT, RL for all scenarios). Explain the role of GRPO, format/accuracy rewards, and preference rewards.

104. **Reasoning frontiers.** Discuss the three upcoming fields: multi-modal reasoning (MCoT, Llava-CoT, Reason-RFT rewards), efficient reasoning (Chain-of-Draft, length rewards, hybrid reasoning), and latent-space reasoning (Chain-of-Continuous-Thought, CODI). What are the trade-offs (e.g., readability/debugging vs efficiency)?

---

## ANSWER KEY

### Section A: Multiple Choice
1. b — System 2.
2. a — Parameters, tokens (dataset), FLOPs (compute).
3. b — Scaling compute during inference.
4. b — Power laws with diminishing returns.
5. b — Smaller model, much more data (Chinchilla).
6. c — Balance train-time and test-time compute for a target Elo.
7. b — Search against verifiers and modifying proposal distribution.
8. b — Search against verifiers is output-focused.
9. b — ORM judges only the final outcome.
10. b — PRM judges the reasoning steps.
11. c — Few-shot.
12. c — Zero-shot prompting.
13. b — Majority vote over sampled answers.
14. c — Self-consistency uses no verifier.
15. a — Sample N, pick highest-scoring.
16. b — SFT on instruction templates with CoT traces (Flan).
17. b — 1,000 questions + reasoning traces.
18. b — Language mixing / poor readability from RL without SFT.
19. b — Cold start → reasoning RL → rejection sampling → SFT → RL.
20. a — Adding `<|think|>` to the system turn.
21. b — Two-stage framework.
22. b — Short, draft-like steps (~5 words).
23. b — Hidden representations, no visible reasoning tokens.
49. b — Accuracy + format (rule-based).
50. d — Increasing pre-training data is train-time compute.

### Section B: True/False
51. **F** — They can still arrive at the same conclusions; not always better.
52. **T** — Applies to both pre- and post-training.
53. **F** — It means scaling time/compute on inference (could be voting on many answers, not just more thinking tokens).
54. **T** — Power law → straight line on log-log.
55. **F** — Kaplan: increase model size, train on as much data as possible without overfitting (Chinchilla: smaller model).
56. **T** — ORM judges only the outcome.
57. **F** — Few-shot generally better than one-shot, which is better than zero-shot.
58. **F** — Self-consistency uses majority vote, no verifier.
59. **T** — 1,800+ tasks.
60. **F** — R1-Zero used ONLY RL, no SFT (that caused the cold-start problem).
61. **T** — Removing it disables reasoning.
62. **F** — CoD produces shorter traces.
63. **T** — `/think` and `/no_think`.
64. **F** — Latent space reasoning hides reasoning (no visible tokens).

### Section C: Short Answer (model answers)
76. **System 1 vs System 2.** System 1 = automatic, quick, intuitive, snap judgments (prone to biases). System 2 = slow, deliberate, conscious effort, logical. Non-reasoning LLMs ≈ System 1 (fast pattern-based answers); reasoning LLMs ≈ System 2 (step-by-step analysis, catch errors). Reasoning is critical for agents (planning, tool selection, reflection).
77. **Scaling laws.** Describe the relationship between scale (compute, data, params) and performance; power laws with diminishing returns (logarithmic). Kaplan: performance improves predictably; for fixed compute, increase model size and data without overfitting. Chinchilla: models were undertrained; for fixed compute use a smaller model with more data. Both: scale all three in tandem. Result: diminishing gains led to the search for test-time scaling.
78. **Search against verifiers (3 steps).** (1) Sample multiple reasoning processes/answers; (2) a reward model (verifier) scores the outputs; (3) choose the best answer based on scores.
79. **ORM vs PRM.** ORM judges only final outcome; PRM judges intermediate reasoning steps. PRM useful when process quality matters (e.g., catching a model that reaches a right answer via wrong reasoning, or grading step-by-step reasoning in math where a later step can correct an earlier error).
80. **Prompting types.** Zero-shot = no examples; one-shot = one example; few-shot = two+ examples (tends to be most accurate, helps pattern recognition). Note: zero-shot < one-shot < few-shot generally.
81. **Self-consistency.** Sample N answers with high temperature + CoT; majority vote picks the most frequent answer. Works without a verifier because sampling reduces the chance of picking an infrequent incorrect answer; fails on tasks the model rarely gets right.
82. **DeepSeek-R1-Zero.** Started from DeepSeek-V3-Base; RL only (GRPO) with accuracy + format rewards; system prompt used `<think>...</think>` and `<answer>...</answer>`. The model discovered longer reasoning helps. Cold-start problem: without SFT, language mixing and poor readability.
83. **DeepSeek-R1 5 steps.** (1) Cold start prevention (SFT on ~5k CoT samples); (2) reasoning-oriented RL (GRPO + language-consistency reward); (3) rejection sampling (synthetic reasoning + non-reasoning data → 800k samples); (4) SFT on the 800k dataset; (5) RL for all scenarios with helpfulness/harmlessness rewards + summary of reasoning.
84. **Native reasoning / chat templates.** Model-specific special tokens format roles; e.g., Gemma 4: `<bos>`, `<|turn>system/user/model`, `<turn|>`, and `<|think|>` in the system turn enables reasoning (removing it disables). No prompting tricks needed because the model was trained on CoT examples.
85. **Latent space reasoning.** CoT becomes internal — the model reasons in hidden representations, no visible tokens. Chain-of-Continuous-Thought operates directly on the last hidden state (tokens only after `<eot>`). CODI trains a teacher (explicit CoT) and student (latent) with an additional loss, implicitly teaching explicit reasoning.

### Section D: Essay (grading notes)
101. Expect: definitions of train/test compute; scaling laws as power laws with diminishing returns; Kaplan (fixed compute → bigger model + more data, avoid overfitting) vs Chinchilla (smaller model + more data); evidence: OpenAI post (test-time ≈ train-time), Jones/AlphaZero Hex (balance both for target Elo), s1 (1,000 samples, terminating/lengthening thinking); paradigm shift in 2024–2025 toward reasoning models balancing training and inference.
102. Expect: search against verifiers (sample → verifier → best; output-focused; self-consistency, Best-of-N with ORM/PRM) vs modifying proposal distribution (SFT/RL; input-focused; reranking token probabilities toward reasoning tokens). Include examples (seating puzzle majority vote; roman_to_int verification; DeepSeek).
103. Expect: R1-Zero (RL-only from V3-Base, accuracy+format rewards, GRPO, cold-start problem); five steps with intermediate models (V3-1, V3-2), language-consistency reward, rejection sampling to build 800k dataset, SFT, final RL with helpfulness/harmlessness.
104. Expect: multimodal (MCoT two-stage; Llava-CoT four stages; Reason-RFT three reward types), efficient (CoD 5-word drafts; length rewards — Kimi k1.5, O1-Pruner, L1; hybrid reasoning /think /no_think), latent space (Chain-of-Continuous-Thought on hidden states; CODI teacher-student). Trade-offs: visible CoT aids debugging; efficient/latent reasoning saves compute but is harder to inspect.

---

### Scoring Guide
- Sections A + B: 39 pts | Section C: 20–30 pts (choose ~6–8) | Section D: 20 pts.
- **85–100%**: Strong. Review missed items only.
- **70–84%**: Good. Re-read study notes for weak areas.
- **<70%**: Re-read chapters + study notes, retry in 2–3 days.
