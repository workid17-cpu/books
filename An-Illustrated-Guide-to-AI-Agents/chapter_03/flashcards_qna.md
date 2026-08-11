# AI Agents — Flashcards / Q&A (Chapter 3)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 3 "Reasoning Large Language Models"
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
