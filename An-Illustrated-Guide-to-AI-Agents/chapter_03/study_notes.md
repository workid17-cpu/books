# AI Agents — Comprehensive Study Notes (Chapter 3)
**Source:** *An Illustrated Guide to AI Agents* (O'Reilly, Early Release)
**Scope:** Chapter 3 (Reasoning Large Language Models)

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

## High-Yield Vocabulary (Chapter 3)
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
