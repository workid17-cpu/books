# Comprehensive Study Notes — Chapter 6
**Source:** *An Illustrated Guide to AI Agents*, Chapter 6 "Planning and Reflection"

---

## CHAPTER 6 — PLANNING AND REFLECTION

### 6.1 Why Planning & Reflection Matter

- Reasoning (Ch. 3) enables the vital agent capabilities of **planning actions** and **reflecting** on them.
- Agents are **multi-step entities**. Example steps to "create a feature": clarify requirements → analyze codebase → design → implement → test → update docs.
- The agent must be aware of these steps and **track its current state**.
- Plans must be **malleable** (changeable) — iterative loops (e.g., implement → test → fix bug) are fundamental to agents.
- **Reflection** on what was done and what could improve is vital for autonomy. Planning + reflection = **feedback loop** that prevents getting stuck in a **local minimum**.
- Planning is the **final core component of a single agent**.

### 6.2 Planning

#### Task Decomposition
- The first step in planning: decompose an initial query into **subtasks**. Simplifies complicated tasks. Each task may have its own subtasks.
- Plan tasks can describe a **tool** to use: plan first, then execute each (sub)task with tools (e.g., "Add a new feature": Task 1 = analyze codebase via a retrieval tool call).

#### Chain-of-Thought (CoT) as Task Decomposition
- CoT (Ch. 3) = solve problems step-by-step via few-shot examples or "Let's think step-by-step."
- This step-by-step process **is** task decomposition: the query is broken into sequential reasoning substeps that build on each other.
- CoT was originally about scaling test-time compute, but "thinking" can be used for planning — e.g., asking for a plan + "think step-by-step."
- Variants that sample multiple traces to find the optimal one:
  - **Self-consistency** — sample several CoT paths, pick the answer most agree on (majority vote).
  - **Tree of Thoughts (ToT)** — explores branching paths (each node a thought/subtask), pruning dead ends; can use Beam Search or Monte Carlo Tree Search with reward models.

#### Explicit Planning
- **Least-to-Most (LtM) prompting** (2022): builds on CoT.
  - Step 1: **Problem reduction** — decompose the problem into subproblems.
  - Step 2: **Sequentially solve subquestions** — solve one by one; the previous subquestion's question *and answer* are appended to the next prompt (running state).
  - Differs from CoT: previous answers are fed back in.
  - Uses few-shot examples to show decomposition + solving.
- **Plan-and-Solve prompting** (zero-shot): extension of zero-shot CoT. Instead of "Let's think step-by-step," the LLM **devises a plan first, then solves step-by-step**.
  - Step 1: **Prompting for reasoning generation** — generate a plan, carry it out.
  - Step 2: **Answer extraction** — process the output into the final answer.
  - Akin to thinking + answer steps from Ch. 3. Happens inside one LLM call (plus one for extraction).
- Note: Q/A structures were common in early LLM days (models were trained to autocomplete; needed steering to understand they should answer). Most LLMs today can plan without complex instructions, but they often need to be *trained* on CoT/planning-like traces.

#### Action Sequencing
- LtM and plan-and-solve decompose into sequential subtasks — good for LLMs, but **agents sequence actions autonomously one after another**.
- **Action sequencing** = after deciding (sub)goals, determine a sequence of actions that transitions the agent from its current state to the desired goal state.
- Requires a careful plan + efficient steps (e.g., a coding agent that skips reading the codebase is inefficient/redundant).
- Agents interleave **plan and action**: execute next step → update plan based on outcome → loop until complete.

#### Reason and Act (ReAct) with Prompting
- Before ReAct, reasoning and acting were **separate**: CoT-style methods reasoned with no actions; action-focused methods (Toolformer) acted with no explicit reasoning.
- **ReAct (Reason and Act)** combines them: interleaves reasoning traces and task-specific actions → iterative thought-action process → the **first truly autonomous systems**.
- Each loop separates output into three components:
  - **Thought** — reasoning about the current situation.
  - **Action** — actions to execute (e.g., tools).
  - **Observation** — reasoning about the result of the action.
- Implemented via prompting (few-shot examples of T/A/O cycles are typically added to reinforce the pattern).
- ReAct agents can run for hundreds of cycles → **memory/context engineering** (Ch. 4) becomes vital.
- **The `ReAct` class** in TinyAgent:
  - `max_steps` — cap on steps (prevents infinite loops).
  - `.prompt` — describes the ReAct format (`## ReAct Format`) and completion (`## ReAct Completion`: use `ACTION: {"tool": "final_answer", "kwargs": "..."}` to finish; it's the only way to complete, otherwise you're stuck in a loop).
  - `.parse` — regex-extracts THOUGHT and ACTION into `Response.reasoning` and `Response.content`. Assumes prompt-based `Tools` (not NativeTools).
- **The autonomy loop is "a for-loop!"** — iteratively call the LLM until it decides to stop (calls `final_answer` or no tool call). Three changes to TinyAgent: (1) add ReAct instructions to system prompt; (2) add an autonomy loop in `.run`; (3) parse responses into THOUGHT/ACTION.
- Demo: "(4.6 + 6.685) x 4 − 3.14" → agent does add → multiply → subtract → final_answer. Trajectory shows each step's thought/action/observation.
- **Components of a complete agent:** Reasoning LLM (brain) + Tools (environment interaction) + Memory (prevents forgetting) + Planning (task decomposition + ReAct-like frameworks for autonomous behavior). Running continuously (while/for loop) = an agent.

#### ReAct with Supervised Fine-Tuning — FireAct
- Prompt-based ReAct needs few-shot examples (wastes tokens, hard to get right).
- **FireAct**: one of the first methods to fine-tune an LLM on ReAct trajectories.
  - Step 1: GPT-4 generates different trajectory types (CoT, ReAct, Reflexion) from questions across datasets — different questions need different methods (simple → CoT; multi-turn → ReAct).
  - Step 2: convert all to the same ReAct format (CoT → a one-round ReAct: thought = intermediate reasoning, action = returns the answer, no observation), then fine-tune a smaller model (Llama 2). Experiments used both full fine-tuning and **LoRA** (Low-Rank Adaptation).
  - **Result:** fine-tuning on trajectories outperformed prompt-based ReAct; **no few-shot examples needed**, more efficient inference, less context used.

#### ReAct with Reinforcement Learning — ETO
- **Exploration-based Trajectory Optimization (ETO)**: uses RL to encourage learning/exploring trajectories (vs FireAct's mimicry).
  - Step 1: **SFT on ReAct data** (Llama-2-7B-Chat) to create a base agent with planning capabilities. Datasets: multi-step planning tasks like **ALFWorld** (text-based household environment: "clean some tomato and put it on the countertop"). SFT here = **behavior cloning / imitation learning**.
  - Step 2: iterative loop of **exploration** and **training**:
    - Exploration: base agent interacts with environment; sample the **failed** ReAct trajectories.
    - Training: pair failed trajectories with correct ones; fine-tune with **Direct Preference Optimization (DPO)** — increase likelihood of successful trajectories, decrease failed ones (contrastive learning).
  - Pattern: **SFT then RL** is a strong paradigm (foundation + exploration).

#### Native ReAct
- Prompt-based ReAct is **brittle** and takes up system-prompt space. Training models to do ReAct natively fixes this.
- With a native model (Gemma 4 E4B, `think=True`):
  - THOUGHT → `Response.reasoning` (NativeReAct).
  - ACTION → `Response.tool_call` (NativeTools).
  - OBSERVATION → tool output added to memory via the `tool` role.
- **`NativeReAct(ReAct)`** removes the prompt and parsing entirely — all it needs is `max_steps`. The LLM was trained (SFT+RL) to reason, call tools, and stop when there's no tool call.
- Demo shows the model reasoning on step 1, then just tool-calling steps 2–3, then reasoning + answer. Each LLM's behavior differs slightly but boils down to the same loops.

### 6.3 Reflection

- **Environment feedback vs internal feedback:** the environment gives feedback per action ("when I do X, environment returns Y"). **Reflection** = internal process where the LLM is critical of its own output and processes. Reflection can also combine with external feedback sources.
- The techniques here are all **prompting-based**.

#### Self-Refine
- **Self-Refine**: the LLM provides feedback and refines its own results — acting as its **own editor**.
- Loop: generate initial answer → **critique** it (feedback) → **refine** incorporating changes → repeat until a stopping criterion (max steps or LLM-guided).
- The *same* LLM generates the initial output, feedback, and refined output (hence "Self-Refine").
- No explicit agentic system — focused on output refinement.

#### Reflexion
- **Reflexion**: agents **verbally reflect** on previous tasks via internal and external feedback. Three LLM entities:
  1. **Actor LLM** — a ReAct LLM that executes actions based on observations and trajectory data (the main "brain").
  2. **Evaluator LLM** — assesses trajectory/output quality; produces a **scalar reward** ("How well did the Actor do?").
  3. **Self-Reflection LLM** — generates nuanced, specific feedback from the full trajectory + the Evaluator's reward.
- Loop continues until the Evaluator deems the Actor's answer correct.
- Tied together by memory: trajectory in **short-term memory** (conversation memory), reflective experiences in **long-term memory**. Trajectory observations maintained in **episodic memory**.
- **CRITIC** is a related technique: starts with an initial output and revises it using **external tools** (e.g., search) for more information.
- Limitation: prompt-based techniques only *guide* behavior — they don't instill it into parameters.

### 6.4 Self-Improvement

- Reflection is the first step toward **continuous self-improvement**. Prompt-level heuristics aren't enough — more fundamental changes are needed.
- **RL enables seemingly unbounded self-improvement with limited human-labeled data.**
- Earlier approaches: **RISE** (includes introspection steps to continuously improve outputs). Recent: **R-ZERO** and **TTRL** — generate their own training data and use RL to self-improve.

#### Test-Time Reinforcement Learning (TTRL)
- Problem: labeled agentic trace data is hard and costly to create; self-play/self-experience often needs supervised data that's hard to collect for complex real-world tasks.
- **TTRL**: update models at **test time** using RL — models learn as they interact, **no supervised data needed**. Parameters are updated using RL (not just memory modules).
- **Four steps:**
  1. **Repeated sampling** — generate several candidate outputs (16 per query, temperature 0.6). Models tested: Qwen2.5-7B, LLaMA-3.1-8B-Instruct, Mistral-8b-Instruct-2410, DeepSeek-R1-LLaMa-8B.
  2. **Majority voting** — select the best answer among the 16.
  3. **Reward generation** — based on alignment between the voted output and the 16 outputs.
  4. **RL (GRPO)** — use the rewards to improve the model as it acts.
- Combines previously-covered techniques: majority voting (test-time compute scaling, Ch. 3) + GRPO (Ch. 2/3).
- **"Lucky hit"**: even if the majority-voted (estimated) label is incorrect, the per-sample rewards can still be correct — a sample that disagrees with the wrong estimated label still receives the correct reward. So correct reward signals can be produced by an incorrect process. Authors argue vague rewards signal further exploration rather than exploitation → prevents local minima.
- TTRL = **online approach** to RL, learning during inference (on-the-fly adaptation).

#### R-Zero
- **R-Zero**: self-evolving reasoning LLMs without labeled data. Two models with distinct roles, initialized from the **same base model**, **coevolving**:
  - **Challenger** — generates synthetic queries that are *difficult* for the Solver.
  - **Solver** — generates multiple answers, selects the best via majority voting (like TTRL).
- The Challenger is trained with **GRPO** based on rewards from the Solver. Signals:
  - **Uncertainty reward** (0–1) — fraction of Solver's responses matching the most common answer; guides the Challenger to make difficult-but-solvable queries.
  - **Repetition penalty** (0–1) — ensures the Challenger generates *diverse* queries.
  - **Format reward** (0/1) — whether queries are between `<question></question>` tags.
  - **Composite reward** (0–1) — uncertainty − repetition penalty (not below 0).
- The Solver is **frozen** (used as a reward model) while training the Challenger.
- After the Challenger is trained: sample queries from it → pass through Solver → filter out queries that are too difficult or too easy (majority vote) → fine-tune the Solver on the curated "just difficult enough" dataset with GRPO, using a **simplified 0/1 verifiable reward**.
- **Asymmetry:** Challenger gets continuous composite rewards (explore a range of difficulties); Solver gets binary reward (be precise/deterministic).
- Together: coevolving system where the Challenger keeps making harder queries and the Solver keeps getting better.

#### Test-Time Training Paradigm
- TTRL and R-Zero are newer techniques with "tremendous potential" — potential **paradigm shifts** moving training focus toward **unlabeled data and inference** rather than only pre-training + SFT + RL.
- Evolution: traditional training (SFT + RL) → **test-time training** (more compute during inference, easier to scale existing models).

### Chapter 6 Key Takeaways
- Planning = task decomposition (CoT, LtM, plan-and-solve) + action sequencing (ReAct, FireAct, ETO, native ReAct).
- ReAct interleaves THOUGHT/ACTION/OBSERVATION — the core of autonomous agents.
- Reflection = Self-Refine (self-editing), Reflexion (Actor/Evaluator/Self-Reflection LLMs), CRITIC (external tools).
- Self-improvement = TTRL (test-time RL: sampling → majority vote → reward → GRPO) and R-Zero (Challenger/Solver coevolution).
- Complete single agent = Reasoning LLM + Tools + Memory + Planning, running in a loop.

---

## High-Yield Vocabulary (Ch 6)

| Term | Meaning |
|---|---|
| Task decomposition | Splitting a query into subtasks |
| Least-to-Most (LtM) | Decompose then solve sequentially, feeding answers forward |
| Plan-and-Solve | Zero-shot: make a plan, then solve step-by-step |
| Action sequencing | Ordering actions to transition state to goal |
| ReAct | Reason-and-Act: interleave Thought / Action / Observation |
| FireAct | SFT on ReAct trajectories |
| LoRA | Low-Rank Adaptation (efficient fine-tuning) |
| ETO | Exploration-based Trajectory Optimization (SFT + DPO) |
| ALFWorld | Text-based household task environment |
| DPO | Direct Preference Optimization (RL from preference pairs) |
| Self-Refine | LLM critiques and refines its own output |
| Reflexion | Actor + Evaluator + Self-Reflection LLMs |
| CRITIC | Self-correction using external tools |
| TTRL | Test-Time Reinforcement Learning (RL during inference) |
| R-Zero | Challenger/Solver coevolving self-improvement |
| Local minimum | A suboptimal solution the agent can get stuck in |
