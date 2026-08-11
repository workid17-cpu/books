# Flashcards & Q&A — Chapter 7
**Source:** *An Illustrated Guide to AI Agents*, Chapter 7 "Evaluating Agents"
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.

## Part 1: Term → Definition

1. **Benchmark** → A fixed set of tasks paired with a way to score them, so different models can be compared against the same yardstick.
2. **What is the three-part outline of most benchmarks?** → A dataset of problems, a procedure for attempting them, and a metric that collapses performance into a single number.
3. **SWE-bench Verified** → A curated coding benchmark of real GitHub issues; an agent must produce a patch that passes the project's hidden tests. Score = % of issues solved.
4. **What was the 2023 → 2026 progress on SWE-bench?** → Best-in-class agents went from ~2% resolved in 2023 to 70%+ on SWE-bench Verified by 2026.
5. **Terminal-Bench** → A coding benchmark where the agent operates a Linux terminal to solve software, system, and environment issues.
6. **BFCL (Berkeley Function Calling Leaderboard)** → Tool-use benchmark testing function-calling accuracy across hundreds of API schemas, including nested parameters and ambiguous inputs.
7. **τ-bench ("tau bench")** → Tool-use benchmark over realistic multi-turn customer service scenarios requiring business-policy following and long-conversation context maintenance.
8. **OSWorld** → Computer-use benchmark placing agents in simulated computer environments (editing documents, managing files, configuring software).
9. **WebArena** → Browser-based benchmark placing agents inside realistic replicas of live websites (e.g., Reddit, GitLab) for multi-step goals.
10. **GAIA** → Real-world benchmark with difficult open-ended questions often requiring reading files and using web search for a verifiable answer.
11. **GDPval** → Benchmark drawn from knowledge work of multiple professions across various sectors and domains.
12. **MMLU / MMLU-Pro** → Knowledge benchmarks; MMLU covers dozens of subjects; MMLU-Pro extends options to 10 (A–J) to prevent random guessing.
13. **GPQA Diamond** → Graduate-level science questions (physics, biology, chemistry) designed to be hard even for experts and resistant to memorization.
14. **Humanity's Last Exam** → Extremely challenging cross-discipline benchmark.
15. **ARC-AGI / MMMU / needle-in-a-haystack** → Abstract reasoning / multimodal reasoning / long-context retrieval benchmarks respectively.
16. **Data contamination** → Benchmark examples appearing in training data, so scores reflect memorization instead of capability.
17. **"Model-and-harness pair"** → The idea that a reported benchmark score belongs to both the model and the agentic harness it ran in; rankings can flip between harnesses.
18. **What does the SWE Atlas paper demonstrate?** → The same model scores differently in its vendor harness (Codex CLI, Claude Code, Gemini CLI) vs a minimal harness (mini-SWE-agent with only a bash tool) — and top-model rankings can flip.
19. **Benchmark saturation** → Once leaders exceed ~85%, score differences become noisy; the industry moves to a harder benchmark/variant.
20. **Outcome evaluation** → Scoring the final result of a task (patch, answer, report) without regard to how the agent got there.
21. **Trajectory evaluation** → Scoring the sequence of steps/tool calls an agent made along the way.
22. **Trajectory** → The full sequence of steps an agent took: reasoning produced, tools called, arguments passed, intermediate outputs generated.
23. **Human evaluation (direct scoring)** → A person reads an output and assigns a score (e.g., 4/5) based on criteria.
24. **Preference evaluation** → Presenting two outputs side by side and asking which is preferred; relative judgments are more reliable than absolute ones.
25. **Win rate** → Percentage of tasks in an evaluation set where one agent is preferred over another (e.g., A preferred on 80% of tasks).
26. **Elo rating** → Chess-derived system for ranking more than two agents via pairwise results; wins raise and losses lower a score, weighted by opponent strength.
27. **Chatbot Arena (LMSYS)** → Crowdsourced platform where thousands of users compare anonymous model outputs, producing Elo-like stable rankings.
28. **Why does human evaluation remain important despite being slow/expensive?** → It is the baseline/gold standard against which all automated methods are validated (e.g., new LLM judges must correlate with human judgment).
29. **Exact match** → Deterministic check: the output either matches the expected answer or it doesn't.
30. **Why is exact match brittle?** → Formatting differences ($1,482.81 vs 1,482.81 vs $1,482.81.) and open-ended outputs (documents, plans, synthesis) break strict string comparison.
31. **Programmatic checks / automated verifiers** → Code verifying structural properties of output (valid JSON, unit tests, word counts) instead of a single string.
32. **How does SWE-bench use programmatic checks?** → A suite of unit tests that fail before the patch is applied and must pass after.
33. **IFEval** → Instruction-following benchmark with verifiable instructions (e.g., ">400 words", "mention AI ≥3 times"), each checked by a validator/heuristic.
34. **LLM-as-a-judge** → Using another capable LLM to score an output or choose between two outputs.
35. **What qualities can an LLM judge assess that programmatic checks cannot?** → Fluency, logical coherence, information consistency given ground truth context, and tone.
36. **Three failure modes of LLM judges** → Confidence bias (preferring confident-but-wrong), position bias (preferring the first option), and self/family-model bias.
37. **Four mitigation techniques for LLM-judge biases** → Use a judge from a different model family; swap presentation order; ask for a written rationale or use a reasoning model; use ensembles of judges (vote + aggregate).
38. **Rubric-based evaluation** → Giving the judge an explicit structured set of criteria, each with defined score levels, instead of one "is this good?" question.
39. **ScholarQABench / HealthBench** → Notable rubric-based benchmarks with detailed structured rubrics scored by model judges.
40. **The four axes of the Figure 7-7 rubric** → Fluency, correctness, completeness, and groundedness.
41. **Why do rubrics improve evaluation reliability?** → They break a complex judgment into smaller, more objective criteria, enabling per-axis high-resolution failure signals.
42. **The three trajectory failure modes** → Unsound reasoning, inefficiency, and unnecessary overhead.
43. **What are the three example trajectory problems in Figure 7-9?** → Agent A made a lucky guess; Agent B used several tool calls where one would do; Agent C overthought before the right tool call.
44. **Works that formalize trajectory evaluation** → T-Eval (2024), AgentBoard (2024), TRACE (2025), AgentProcessBench (2026).
45. **pass@k** → The probability that at least one of k sampled attempts passes — a measure of capability.
46. **pass^k (pass-HAT-k)** → The probability that all k of k sampled attempts succeed — a measure of reliability.
47. **Why can't simple division compute pass@k for k>1?** → Simple division assumes independence between samples; the combinatorial estimator accounts for the dependency when drawing k candidates (breaks down for k>1).
48. **When is a pass@k estimate trustworthy?** → Only when the number of samples n is much larger than k (n >> k); small samples mislead.
49. **Which parameter enables pass@k sampling?** → Temperature set to allow variance (e.g., ~0.7).
50. **What does the spread between pass^k and pass@k represent?** → The model's reliability gap.
51. **What do pass@1 vs pass@3 tell you differently?** → pass@1 = single-attempt correctness; pass@3 = chance of at least one success in three tries (capability ceiling).
52. **In the Figure 7-12 example, why do GPT-5.4 and Opus 4.7 differ despite the same pass@1 of 40?** → GPT-5.4 is more reliable (pass^3 = 28 vs Opus 4.7's 23); Opus 4.7 has the highest pass@3 (60) — a capability ceiling.
53. **When training, which metric guides checkpoint selection? Why?** → pass@3 (higher capability ceiling indicates more headroom with further training).
54. **When comparing API vendors, which metric matters more? Why?** → Reliability (pass^k), because you run the model as it ships rather than train it.
55. **The pass^k formula** → C(c,k) / C(n,k): all-correct k-subsets divided by total k-subsets, where c = correct samples out of n.
56. **Where was pass^k introduced?** → τ-bench (Yao et al., 2024), to measure whether tool-using agents hold up across repeated trials.
57. **Why do agents deserve dedicated safety evaluation?** → Their actions are consequential: sending high-impact emails, deleting files, or dropping databases.
58. **The three safety threat models** → (1) Misuse (malicious user), (2) manipulation through data the agent reads, (3) no adversary — the agent errs on its own.
59. **AgentHarm** → Safety benchmark measuring refusal rate and harm score for misuse (malicious user requests).
60. **Prompt injection** → Planting instructions in content the agent processes (retrieved document, tool result, web page) to hijack its behavior.
61. **Memory poisoning** → Corrupting what the agent stores and later retrieves so it acts on false premises many steps later.
62. **AgentDojo / Agent Security Bench (ASB)** → Safety benchmarks reporting attack success rate; ASB covers prompt injection + memory poisoning; AgentDojo focuses on prompt injection and whether the agent stays on task while resisting.
63. **What does the "no adversary" safety category rely on?** → Your own test cases, guardrails, and harness choice/design (least standardized).
64. **What does "build your own evals" mean for test-case design?** → Aim for a small, well-curated set covering a wide range of failure modes and edge cases, not just successes.
65. **Eval harness** → Infrastructure that takes test cases, runs the agent on each, captures the full trajectory, applies scoring methods, and aggregates results.
66. **harbor** → Recent open-source agent eval harness: containerized trials at scale, standardized task/success-criteria format, and a registry (e.g., Terminal-Bench ships through it).

## Part 2: Short Answer

67. **Why is evaluation described as "one of the most underinvested areas in agent development"?** → Teams rely on manual testing and intuition too long, then can't ship improvements confidently or diagnose regressions.
68. **Explain the difference between outcome evaluation and trajectory evaluation with an example.** → Outcome scores the final result (patch, answer); trajectory scores the steps. Example: three agents all pass outcome evaluation, but one guessed and got lucky, one wasted tool calls, one overthought — only trajectory evaluation tells them apart.
69. **Why might two "80%" scores mean completely different things?** → They may measure different capabilities (SWE-bench vs MMLU) produced by different methods (single vs many runs; deterministic vs LLM judge; agentic vs non-agentic).
70. **Why is code evaluation the "gold standard" of agent evaluation?** → Code is verifiable: it either executes and passes unit tests or it doesn't — enabling objective automated checks that also guide training.
71. **Explain why relative (preference) human judgments are more reliable than absolute scores.** → "A is better than B" is easier than assigning A=4 and B=3; pairwise comparison avoids the difficulty of absolute scoring in isolation.
72. **How does Elo extend pairwise comparison to many agents?** → Each pairwise result updates both agents' scores (wins up, losses down) weighted by opponent strength, so you don't need to run every head-to-head pair.
73. **Walk through the Evaluator pattern used in the book.** → For each example: create a fresh agent (reset memory/trajectory), run it on the task, apply the scorer (bool or score), record pass/fail; average into pass_rate.
74. **Why must a new agent be created for each benchmark example?** → To reset its memory and trajectory so each example is evaluated independently.
75. **What are the trade-offs of exact match?** → Simple and deterministic, powers many benchmarks, but fails on formatting variations and open-ended outputs with many valid answers.
76. **Give an example of a programmatic check for a coding agent.** → Run unit tests on the generated function; for SWE-bench, a test suite that fails before the patch and passes after it is applied.
77. **Describe the LLM-as-a-judge implementation in the book.** → A Gemini 3.1 Flash-Lite judge (OpenAI-compatible endpoint) receives a prompt with the expected answer and the agent's response, and replies with a single 0.0–1.0 score that is parsed as a float.
78. **What did the MMLU-Pro open-ended demo reveal about LLM judges?** → A near-correct answer got 0.4 (not 0), and a "happiness" answer to a "good" (I) question got 1.0 — demonstrating judges are imperfect and that a mix of metrics is preferred.
79. **How does a rubric differ from a raw LLM judge?** → A rubric defines structured criteria with explicit score levels per axis (e.g., fluency, correctness, completeness, groundedness), making results more interpretable and pointing to specific failure modes.
80. **Explain the pass@k vs pass^k distinction in plain terms.** → pass@k asks "can it succeed at least once in k tries?" (capability). pass^k asks "does it succeed every time across k tries?" (reliability). The same runs give both, and they can rank models differently.
81. **Why are single-run scores untrustworthy for agents?** → Agent outputs are stochastic; one run can be a lucky fluke. Aggregated scores over many runs with variance estimates are more trustworthy.
82. **Why is a high partial-credit score potentially misleading for agents?** → Agents are used for multi-step tasks; a high partial-credit score can hide an agent that rarely completes tasks end to end.
83. **What questions should you ask when reading a benchmark score?** → How was it produced? Is partial success rewarded? Which harness? Single or multiple runs? Is it in the training data? Who ran it? Were prompts tuned for it? Does the scaffolding generalize? Is it saturated? Which variant? What was the cost?
84. **Explain the three safety threat models and their typical metrics.** → Misuse (AgentHarm: refusal rate + harm score); data manipulation — prompt injection & memory poisoning (AgentDojo/ASB: attack success rate); own-error with no adversary (least standardized; own test cases + guardrails).
85. **Why is building your own small eval better than none?** → Even 5–10 representative cases give eye-opening signals about performance and regressions; it's more important to start than to get it perfect.

## Part 3: Fill-in-the-Blank

86. "A benchmark is a fixed set of ______ paired with a way to score them." → tasks
87. "SWE-bench Verified score is the percentage of ______ the agent solves." → issues
88. "When SWE-bench launched in 2023, best-in-class agents resolved around ______ of tasks; by 2026, leading agents cracked ______ on SWE-bench Verified." → 2%; 70%
89. "MMLU-Pro extends the number of multiple-choice options to ______, labeled A through ______." → 10; J
90. "The full MMLU-Pro has more than ______ question/answer pairs." → 12,000
91. "______ evaluation scores the final result; ______ evaluation scores the sequence of steps." → Outcome; trajectory
92. "Human evaluators find ______ judgments more reliable than absolute ones." → relative (preference)
93. "______ is the most prominent Elo-style crowdsourced model ranking." → Chatbot Arena (LMSYS)
94. "The three trajectory failure modes are ______, ______, and ______." → unsound reasoning; inefficiency; unnecessary overhead
95. "A pass@k estimate is only trustworthy when the number of samples is much larger than ______." → k
96. "pass@1 = number of correct answers divided by the ______ number of samples." → total
97. "The pass^k estimator comes from ______ (Yao et al., 2024)." → τ-bench
98. "Once leading models exceed about ______ on a benchmark, score differences become noisy." → 85%
99. "AgentHarm measures ______ rate and ______ score for misuse." → refusal; harm
100. "______ plants instructions in content the agent processes; ______ corrupts what the agent stores and later retrieves." → Prompt injection; memory poisoning
101. "harbor is an open-source ______ designed specifically for agents, running trials in ______ environments." → eval harness; containerized
102. "Even a small custom eval of ______ cases can give eye-opening signals." → 5 or 10
