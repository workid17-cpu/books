# Comprehensive Study Notes — Chapter 7
**Source:** *An Illustrated Guide to AI Agents*, Chapter 7 "Evaluating Agents"

---

## CHAPTER 7 — EVALUATING AGENTS

### 7.1 The Big Picture

- The previous chapters built up the components of an agent: reasoning, tools, memory, planning. The natural next question: **how do we know if any of it is working?**
- **Evaluation is how you move from "it seems to work" to "I have evidence it works."** It's also how you catch the moment it stops working.
- **Evaluations are one of the most underinvested areas in agent development.** Teams rely on manual testing and intuition too long, then can't ship improvements confidently or diagnose regressions.
- Figure 7-1: the chapter covers how LLMs and agents are evaluated, moving from **evaluating the output** to **evaluating an agent's intermediate steps (trajectory)**.
- Structure of the chapter: start with **benchmarks** (seen in model announcements/papers), then build toward the **tools and concepts to design your own evaluations**.

### 7.2 Public Benchmarks and Leaderboards

- **A benchmark is a fixed set of tasks paired with a way to score them**, so different models can be measured against the same yardstick and compared directly.
- **SWE-bench Verified example:** a collection of real GitHub issues from open source projects. To score a point, a model/agent must read the issue description, explore the repo, and produce a patch that makes the project's existing (hidden) tests pass. Score = percentage of issues solved.
- Most benchmarks follow the same outline: a **dataset of problems**, a **procedure for attempting them**, and a **metric** that collapses performance into a single number for a table.
- **Reading benchmark tables requires care:** 80% on SWE-bench Verified and 80% on MMLU represent completely different claims about completely different capabilities. Some scores come from a single run, others aggregate many runs; some use deterministic tests, others rely on an LLM judge (with its own biases); some aren't measuring agentic capability at all, but the non-agentic capability of the underlying LLM.
- Figure 7-2: a typical benchmark table from a model release (Google).

#### Benchmark categories (Table 7-1)

| Category | Benchmarks | What's measured |
|---|---|---|
| Coding and software engineering | SWE-bench Verified, Terminal-Bench | Patch correctness, test passage |
| Tool use | BFCL, τ-bench | Function-calling accuracy, multi-turn policy following |
| Computer use | OSWorld, WebArena | Action sequences in desktop/browser environments |
| Real-world task completion | GAIA, GDPval | Reasoning and retrieval on open-ended knowledge tasks |
| Reasoning and knowledge | MMLU, GPQA Diamond, Humanity's Last Exam | Exam-style questions across domains |
| Specialized | ARC-AGI, MMMU, needle-in-a-haystack | Abstract reasoning, multi-modal, long-context retrieval |

**Coding and Software Engineering**
- The most mature family; the "gold standard" of agent evaluation because **code is verifiable** — it either executes/passes tests or it doesn't.
- SWE-bench (2023): best-in-class agents resolved ~2% of tasks. By 2026, leading agents on the public leaderboard cracked **70%** on **SWE-bench Verified** (curated to remove ambiguous/unreliable tests).
- **Terminal-Bench**: the agent operates a Linux terminal to solve software, system, and environment issues.
- Coding agents and their evaluations are covered in detail in Chapter 10.

**Tool Use**
- Tests whether an agent calls the right tools with the right arguments (a targeted check of Ch. 5's tool calling, not end-to-end completion).
- **BFCL (Berkeley Function Calling Leaderboard)**: function-calling accuracy across hundreds of API schemas, including nested parameters and ambiguous inputs.
- **τ-bench ("tau bench")**: tool use across realistic multi-turn customer service scenarios — following specific business policies and maintaining context over a long conversation.
- **NOTE:** some benchmarks require specific packages/harnesses (e.g., τ-bench has its own framework). Different benchmarks needing different frameworks hinders testing harness generalizability.

**Software and Computer-Use Environments**
- The agent must navigate a digital environment (desktop, browser, app UI) and conduct a sequence of actions to achieve a goal.
- **OSWorld**: simulated computer environments — editing documents, managing files, configuring software. Often requires interacting with graphical interfaces (clicking buttons, menus, interpreting visual elements).
- **WebArena**: browser-based tasks inside realistic replicas of live websites (e.g., a Reddit forum or GitLab instance) to complete multi-step goals.

**Real-World Task Completion**
- Messy, open-ended tasks resembling real knowledge work: mixture of reasoning, information retrieval, navigating ambiguity, and synthesis across multiple steps and sources.
- **GAIA**: difficult real-world questions that often require reading files and using web search to reach a verifiable final answer.
- **GDPval**: drawn from knowledge work of multiple professions across sectors and domains.
- A 2026 analysis (*How Well Does Agent Development Reflect Real-World Work?*) found today's benchmarks cluster heavily around programming, while the most-populated/highest-economic-value occupations go almost untested — that gap is where much expected evaluation demand will land.

**Reasoning and Knowledge**
- Measures a model's ability to reason about complex questions and apply knowledge across domains — typically difficult exam-style questions.
- **MMLU (Massive Multitask Language Understanding)**: one of the earliest and most widely cited; dozens of subjects from physics to law.
- **GPQA Diamond**: graduate-level physics/biology/chemistry questions intentionally hard even for domain experts and resistant to simple memorization.
- **Humanity's Last Exam**: even harder, extremely challenging questions across many disciplines.
- **Important caveat:** these often test the underlying LLM, not necessarily the agentic abilities of tool use across multiple steps.

**Other Groups of Benchmarks**
- **ARC-AGI**: abstract reasoning puzzles; general problem-solving on unfamiliar tasks.
- **MMMU**: multimodal reasoning — combining visual and textual information.
- **Needle-in-a-haystack**: long-context retrieval — locating specific info within very large documents.

### 7.3 Reading Benchmark Scores Critically

Seasoned builders ask questions instead of taking numbers at face value:

1. **How was the score produced?** Deterministic checks (unit tests, exact answers) vs LLM judges for subjective/open-ended outputs. Different setups produce different results on the same benchmark.
2. **Is partial success rewarded?** All-or-nothing vs partial credit. Matters most for agents (multi-step tasks): a high partial-credit score can hide an agent that rarely completes tasks end to end.
3. **Which agentic harness was used?** The same model can post different scores depending on harness (prompt construction, tool-call parsing, retry logic, context management). The **SWE Atlas** paper runs each model in its vendor harness (Codex CLI, Claude Code, Gemini CLI) AND in a minimal harness (mini-SWE-agent, only a bash tool). Scores shift; top-model rankings can even flip. A reported number is best read as a **model-and-harness pair**.
4. **Single run or multiple?** One run can flatter a lucky result. Prefer metrics aggregating many runs with a variance estimate (e.g., pass@k).
5. **Is the benchmark in the training data?** **Data contamination** skews scores toward memorization. Hard to verify except for fully open models disclosing training datasets.
6. **Who ran the evaluation?** Self-reported scores from the releasing lab deserve more scrutiny than third-party replications. Independent sources: **Artificial Analysis**, **Chatbot Arena**. Even independent leaderboards can be gamed (*The Leaderboard Illusion*).
7. **Were prompts tuned for that benchmark?** Prompt engineering can move scores without generalizable capability gains. If not applied uniformly to all systems, the comparison is uneven.
8. **Does the scaffolding generalize?** A retrieval pipeline, custom tool, or verification step may score well but require heavy engineering to transfer to real deployment.
9. **Is the benchmark saturated?** Once leaders exceed ~85%, score differences get noisy and less indicative. The industry then moves to a harder benchmark/variant.
10. **Which subset or variant was used?** GPQA vs GPQA Diamond, ARC Easy vs ARC Challenge, MMLU vs MMLU-Pro.
11. **What was the cost?** A system scoring 20% more while spending 10× the tokens or wall-clock time isn't straightforwardly better. Cost, token count, latency matter in production.

### 7.4 Outcome Evaluation: Did the Agent Get the Right Output?

- **Outcome evaluation scores the final result** — the patch, the answer, the report — **without regard to how the agent got there**. Contrasted with trajectory evaluation (the sequence of steps and tool calls).

#### Human Evaluation
- **The most direct way:** a person reads the output and judges it. Figure 7-3: a human annotator assigns a direct quality score (e.g., 4/5) based on criteria.
- Works well with an absolute sense of quality and consistent criteria, but objective scores in isolation are hard.
- **Preference evaluation** (Figure 7-4): present outputs from two agents side by side; ask which is preferred. **Relative judgments are more reliable than absolute ones** ("A is better than B" is easier than "A=4, B=3").
- **Win rates** (Figure 7-5): scaled across many tasks — Agent A preferred on 80% of tasks, Agent B on 20%.
- **Elo rating** (borrowed from chess): extended to comparing more than two agents. Each pairwise result updates both scores; wins increase and losses decrease per a formula accounting for opponent strength. **Chatbot Arena (LMSYS)** is the most prominent at-scale example.
- **Human evaluation is slow and expensive, but it remains the baseline against which all automated methods are validated.** Showing a new LLM judge/metric correlates with human judgment is how researchers establish it measures something real.

#### Automated Evaluation
- Needed because human evaluation doesn't scale to continuous feedback loops (hundreds/thousands of prompt changes, model swaps, versions → millions of complex judgments).
- Covers a spectrum from **static automated checks** to **judgements made by another model**.

**Building a simple agent evaluator**
- `Benchmark` dataclass: `name` (task), `examples` (test cases), `scorer` (evaluation function). The scorer takes a prediction + an example and returns either a boolean (correct/incorrect) or a score.
- `Evaluator` class: runs a set of examples against a scorer. Creates a **new agent instance per example** to reset memory and trajectory. Tracks pass/fail per example; averages into `pass_rate`.
- The evaluator pattern: for each example → new agent → run on task → scorer compares → collect pass/fail.

**Exact match**
- The most straightforward deterministic check: output either matches the expected answer or it doesn't (e.g., invoice total = $1,482.81).
- Powers many benchmarks; its simplicity is also its drawback — many real tasks (writing documents, generating code, planning, synthesizing info) have many valid answers.
- **Brittle against formatting differences:** `1,482.81`, `1482.81`, `$1,482.81.` all flagged wrong by a strict check.
- Example: **MMLU-Pro** — multiple-choice questions with 10 options (A–J), extending MMLU to prevent random guessing. The `exact_match_scorer` uses regex to find a single letter (A–J) in the prediction and compares it to `example["expected"]`.
- The full MMLU-Pro has 12,000+ question/answer pairs (running it takes significant time); the book grabs 3 examples to showcase the flow.
- Result: pass_rate 0.33 (one of three correct) for the small model.

**Programmatic checks**
- Extend exact match into the broader category of **automated verifiers**: write code that verifies **structural properties** of the output, not a single string.
- Examples: valid JSON check; unit tests for a function implementation; for SWE-bench, a suite of tests that fail before the patch and pass after.
- **Fast, cheap, objective.** One reason coding agents rose faster than other agents — the code modality enables automated checks, useful for evaluation that also guides training.
- Example: **IFEval** (Instruction-Following Evaluation) — verifiable instructions like "write more than 400 words" or "mention the keyword 'AI' at least 3 times", each checked by a heuristic/validator. Three examples: <10 sentences, ≤150 words, no comma. Result: pass_rate 1.0.

**LLM-as-a-judge**
- When output is too open-ended for programmatic checks, ask another capable LLM to evaluate it. Typically used to produce a **single score** or to **choose a preferred output**.
- Strong LLMs can assess fluency, logical coherence, information consistency (given ground truth context), and tone.
- Figure 7-6: a judge model takes outputs + criteria and evaluates as pairwise preference or absolute score.
- **Failure modes** (some shared with human judges):
  - Prefer outputs that sound confident over ones that are correct.
  - Sensitive to presentation order (position bias — tends to prefer the first option).
  - Inherit biases from training; may prefer their own model family's outputs.
- **Mitigations:**
  - Use a judge from a **different model family** (reduces self-preference).
  - **Swap presentation order** across runs (controls position bias).
  - Ask for a **written rationale** or use a **reasoning model** as judge (improves consistency).
  - Use **ensembles of judges** (multiple models vote, aggregate).
- Book implementation: Google's **Gemini 3.1 Flash-Lite** as judge (free tier; OpenAI-compatible endpoint at `generativelanguage.googleapis.com/v1beta/openai`). Or run a local, more capable model like Gemma 4 26B A4B if you have enough (V)RAM.
- The `judge_scorer` prompts the judge to score 0.0–1.0 and parses the numeric reply → **fine-grained** scores (vs boolean).
- Illustration: same MMLU-Pro questions made **open-ended** (remove the choices, provide the actual answer as `expected`). Results show nuance: 1.0, 0.4 (close-but-wrong answer scored 0.4, not 0), 1.0.
- **Demonstrated failure mode:** the final example was scored 1.0 even though the agent answered "happiness" instead of "good" — the multiple-choice variant would have marked it wrong. LLM judges are not perfect, just like human evaluation. **A mix of evaluation metrics/benchmarks is generally preferred.**

**Rubric-based evaluation**
- A refinement of raw LLM-as-a-judge: give the judge an **explicit rubric** — a structured set of criteria, each with defined score levels.
- Instead of "is this output good?", you score against multiple criteria, each capturing a different axis: e.g., "On a scale of 1–3, does this response correctly cite evidence? 1 = no sources cited, 2 = cited but not relevant, 3 = cited and directly supporting."
- Notable rubric-based benchmarks: **ScholarQABench** and **HealthBench**.
- Figure 7-7: an example rubric scoring four categories — **fluency, correctness, completeness, groundedness**. A low completeness score points to a different failure mode than a low groundedness score → more interpretable results.
- Example rubric levels (from ScholarQABench's Coverage rubric): 1 = Severely lacking (misses core lines of research), 2 = Partial, 3 = Acceptable, 4 = Good, 5 = Comprehensive (spans diverse papers/viewpoints, even surfaces important points beyond the question).
- **Rubrics improve reliability** by breaking a complex judgment into smaller, more objective criteria, and allow evaluating individual sub-behaviors → higher-resolution signal about where the agent breaks down.

### 7.5 Trajectory Evaluation: Did It Get There the Right Way?

- **Outcome evaluation: did it succeed. Trajectory evaluation: how it got there or how it failed.**
- **A trajectory is the full sequence of steps an agent took**: reasoning produced, tools called, arguments passed, intermediate outputs generated.
- **Analyzing behavior at scale:** look at statistics of tool calls across many runs. Figure 7-8 (SWE Atlas): three different agents on the same tasks — GPT-5.4 searches/file-operates heavily early, then executes after gaining repo context. Missed if you look only at outcomes.
- **Individual trajectory inspection** (Figure 7-9): three trajectories that all pass outcome evaluation, but with deeper problems:
  1. **Agent A**: output was a **lucky guess**.
  2. **Agent B**: spent several tool calls where **one would do**.
  3. **Agent C**: made the right tool call but **overthought** its way there.
- **Three distinct failure modes warrant separate measurement:** **unsound reasoning**, **inefficiency**, and **unnecessary overhead**.
- **Measuring a trajectory = scoring the steps along the way, not just the endpoint.** Key axes: Did the agent reach for the right tools with valid arguments? How efficiently (tool calls, steps, reasoning tokens)? Did each step follow reasonably from prior steps?
- These questions can be put to an **LLM judge handed the full trajectory** — that is, a **rubric that examines the steps, not just the final output**.
- Formalizing works: **T-Eval (2024), AgentBoard (2024), TRACE (2025), AgentProcessBench (2026)** — introduce measures for individual trajectory steps.

### 7.6 Reliability: Does It Succeed Every Time?

- **Agent outputs are stochastic** (like LLMs): the same input can produce different outputs across runs. A single-run score can flatter or unfairly penalize.
- Running **multiple trials** of the same task reveals two different things:
  - **Capability**: whether the agent can succeed at all.
  - **Reliability**: whether it succeeds every time.

**Measuring Capability with pass@k**
- For tasks with a clear pass/fail check, pass@k measures capability while accounting for the probabilistic nature of the model.
- Figure 7-10: two models, each sampled twice on one problem. LLM 1 passes both samples; LLM 2 passes one. **pass@1** = correct/total = 2/2 = 1.0 vs 1/2 = 0.5. **pass@2** = at least one correct across two attempts = 1.0 for both.
- **Sampling needs temperature variance** (e.g., ~0.7).
- **Small samples mislead:** a pass@k estimate is trustworthy only when n_samples >> k. With more samples (Figure 7-11: 100 per model), estimates settle: LLM 1 passes 85/100 (pass@1 0.85, pass@2 0.98); LLM 2 passes 45/100 (pass@1 0.45, pass@2 0.70). With 100 samples you can compute higher k (4, 8, 16, 32...).
- **Statistical formula:** pass@k for k>1 uses a combinatorial estimator (from *Evaluating Large Language Models Trained on Code*, 2021), more robust than simple division because it accounts for dependency between samples when drawing k candidates.

```python
import numpy as np

def pass_at_k(n_samples, n_correct_samples, k):
    if n_samples - n_correct_samples < k:
        return 1.0
    return 1.0 - np.prod(
        1.0 - k / np.arange(n_samples - n_correct_samples + 1, n_samples + 1)
    )
```

**Measuring Reliability with pass^k**
- pass@k rewards succeeding **at least once in k tries** — it climbs as you grant more attempts, so it measures **capability, not reliability**.
- **pass^k (pass-HAT-k)**: the probability that **all k of k** sampled attempts succeed. The same runs give both metrics, and they can tell very different stories.
- Figure 7-12 (SWE Atlas Codebase QnA, 10 models): GPT-5.4 and Opus 4.7 have the **same pass@1 of 40** — but pass^3 shows GPT-5.4 is more reliable (28 vs 23). Opus 4.7 has the **highest pass@3 (60)** — given 3 tries it lands at least one success on more tasks.
- **How to use the signal:** in the middle of training, choose the higher pass@3 checkpoint (capability ceiling → more performance with further training). Comparing API vendors, choose based on reliability since you run the model as it ships.
- **Formula (from τ-bench, Yao et al., 2024):** pass^k = chance that all k samples are correct = C(c,k) / C(n,k), where c = correct samples out of n.

```python
from math import comb

def pass_hat_k(n_samples, n_correct_samples, k):
    """Probability that all k randomly drawn samples are correct."""
    if n_correct_samples < k:
        return 0.0
    return comb(n_correct_samples, k) / comb(n_samples, k)
```

- τ-bench introduced pass^k to measure whether tool-using agents hold up across repeated trials, not only on their best attempt.

### 7.7 Safety: Does It Avoid Harm?

- **Agents are more consequential than LLMs**: their actions could send a high-impact email, delete a file, or drop an entire database. Safety evaluation deserves its own place in your eval suite from the start.
- **"Safety" covers distinct questions, separated by who is trying to cause the harm** — each a different threat model with its own metric:

1. **Misuse** — a malicious user asks the agent to carry out a harmful task; does it refuse? **AgentHarm** measures this with a **refusal rate** and a **harm score**.
2. **Manipulation through the data the agent reads** — an attacker steers the agent via content it processes:
   - **Prompt injection**: plants instructions in a retrieved document, tool result, or web page to hijack behavior.
   - **Memory poisoning**: corrupts what the agent stores and later retrieves, so it acts on false premises many steps later.
   - **AgentDojo** and **Agent Security Bench (ASB)** measure these and report an **attack success rate**. ASB covers both surfaces; AgentDojo focuses on prompt injection and tracks whether the agent stays on its real task while resisting.
3. **No adversary at all** — on a benign task, the agent harms the user through its own error (deleting wrong files, taking an irreversible step it should have paused on). The **least standardized**; leans on your own test cases, guardrails, and harness design.

### 7.8 Building Your Own Evals

- **The best signal** about how well an agent matches your target task is a **custom evaluation representative of your specific problem**.
- A small, well-curated set of test cases reflecting real usage/data helps you: pick the best LLM and agent framework, and catch regressions when you update any part of the system.
- **Aim for cases capturing a wide set of failure modes and edge cases, not just successes.**
- Score these tests with the earlier methods: exact match, programmatic checks, LLM-as-a-judge, rubrics.
- **Run and update the test suite automatically on meaningful changes** — like unit tests in the software development lifecycle — so improvements are confirmed and breaking changes caught early.
- **Getting started beats getting it right:** even 5–10 custom cases give eye-opening signals; grow the suite as you learn where the system fails.
- **Eval harness:** the infrastructure that takes test cases, runs the agent on each, captures the full trajectory, applies scoring methods, and aggregates results. **harbor** is a recent open-source harness for agents: containerized trials at scale, standardized format for tasks and success criteria. Popular benchmarks like Terminal-Bench ship through the harbor registry, so you can run established benchmarks alongside custom evals.

### Chapter 7 Key Takeaways
- Evaluation moves you from "it seems to work" to "I have evidence it works" — and catches when it stops working.
- Benchmarks = dataset of problems + procedure + metric; read scores critically (harness, contamination, saturation, cost, variant, single vs multiple runs).
- Major benchmark categories: coding (SWE-bench Verified, Terminal-Bench), tool use (BFCL, τ-bench), computer use (OSWorld, WebArena), real-world tasks (GAIA, GDPval), reasoning (MMLU, GPQA Diamond, Humanity's Last Exam), specialized (ARC-AGI, MMMU, needle-in-a-haystack).
- Outcome evaluation methods: human (direct, preference, win rates, Elo), exact match, programmatic checks, LLM-as-a-judge, rubrics.
- Trajectory evaluation surfaces unsound reasoning, inefficiency, and unnecessary overhead that outcome scores miss.
- Reliability: pass@k measures capability; pass^k measures consistency (succeeds every time).
- Safety threat models: misuse, data manipulation (prompt injection, memory poisoning), and own-error harm.
- Build your own small, curated eval suite, scored with a mix of methods, run automatically on every change.

---

## High-Yield Vocabulary (Ch 7)

| Term | Meaning |
|---|---|
| Benchmark | Fixed set of tasks + a scoring method, for comparing models/agents |
| SWE-bench Verified | Curated coding benchmark: solve real GitHub issues with passing patches |
| Terminal-Bench | Coding benchmark where the agent operates a Linux terminal |
| BFCL | Berkeley Function Calling Leaderboard — function-calling accuracy |
| τ-bench | Tool-use benchmark over realistic multi-turn customer service |
| OSWorld | Computer-use benchmark: agents act in simulated desktops |
| WebArena | Browser-based benchmark in replicas of live websites |
| GAIA | Real-world open-ended questions requiring files + web search |
| GDPval | Benchmark from knowledge work across professions |
| MMLU / MMLU-Pro | Knowledge benchmark; Pro has 10 options (A–J) |
| GPQA Diamond | Graduate-level science questions, hard for experts |
| Humanity's Last Exam | Extremely hard cross-discipline benchmark |
| ARC-AGI / MMMU / needle-in-a-haystack | Abstract reasoning / multimodal / long-context retrieval |
| Data contamination | Benchmark examples in training data → memorization skews scores |
| Model-and-harness pair | A score reflects both the model and its agentic scaffold |
| Outcome evaluation | Scores the final result, ignoring how it was reached |
| Trajectory evaluation | Scores the sequence of steps/tool calls along the way |
| Trajectory | Full sequence: reasoning, tool calls, arguments, intermediate outputs |
| Human evaluation | Person judges output (direct score, preference, win rate, Elo) |
| Win rate | % of tasks where an agent is preferred over another |
| Elo | Chess-derived rating updated by pairwise results |
| Exact match | Deterministic check: output == expected answer |
| Programmatic check | Code verifying structural properties (JSON validity, unit tests) |
| Automated verifier | Any programmatic check of output structure/behavior |
| LLM-as-a-judge | Another LLM scores/ranks agent output |
| Position bias | Judge tendency to prefer the first-presented option |
| Judge ensemble | Multiple judges vote; aggregate their judgments |
| Rubric | Structured criteria with defined score levels per axis |
| Unsound reasoning / inefficiency / overhead | Trajectory failure modes |
| pass@k | Probability at least one of k samples passes (capability) |
| pass^k (pass-HAT-k) | Probability all k samples pass (reliability) |
| Reliability gap | Spread between pass^k and pass@k |
| Misuse | Malicious user; agent should refuse |
| Prompt injection | Instructions planted in data the agent reads |
| Memory poisoning | Corrupting stored memory retrieved later |
| AgentHarm | Safety benchmark: refusal rate + harm score |
| AgentDojo / ASB | Prompt-injection / agent-security benchmarks |
| Eval harness | Infrastructure running agents on test cases and aggregating scores |
| harbor | Open-source agent eval harness (containerized trials) |
