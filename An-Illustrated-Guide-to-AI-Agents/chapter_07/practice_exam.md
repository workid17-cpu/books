# AI Agents — Practice Exam (Chapter 7)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 7 "Evaluating Agents"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. A benchmark is best defined as:
   a) A fixed set of tasks paired with a way to score them
   b) Any dataset of training examples
   c) A model's test-time compute budget
   d) A leaderboard website

2. The score on SWE-bench Verified is:
   a) The number of tests passed
   b) The average patch length
   c) The percentage of issues the agent solves
   d) The number of patches generated

3. To score a point on SWE-bench Verified, an agent must:
   a) Summarize a GitHub issue in plain text
   b) Answer a multiple-choice question
   c) Navigate a web browser
   d) Produce a patch that makes the project's hidden tests pass

4. Which benchmark places the agent inside a simulated computer desktop?
   a) BFCL
   b) MMLU
   c) OSWorld
   d) τ-bench

5. τ-bench is pronounced:
   a) "tee bench"
   b) "theta bench"
   c) "tau bench"
   d) "two bench"

6. Which benchmark tests function-calling accuracy across hundreds of API schemas?
   a) BFCL
   b) WebArena
   c) GAIA
   d) ARC-AGI

7. Which benchmark uses graduate-level science questions intentionally hard for domain experts?
   a) MMLU
   b) IFEval
   c) SWE-bench
   d) GPQA Diamond

8. MMLU-Pro makes tasks harder than MMLU by:
   a) Using open-ended answers
   b) Adding a time limit
   c) Requiring tool use
   d) Extending multiple-choice options to 10 (A–J)

9. "Needle-in-a-haystack" tests measure:
   a) Long-context retrieval of specific information
   b) Code patch correctness
   c) Multimodal reasoning
   d) Abstract reasoning puzzles

10. Which is NOT one of the questions for reading benchmark scores critically?
    a) Was the benchmark in the training data?
    b) Which agentic harness was used?
    c) Did the benchmark use a larger model than the one tested?
    d) What was the cost?

11. "Data contamination" means:
    a) The benchmark data was corrupted
    b) Benchmark examples appeared in the model's training data
    c) The agent's memory was poisoned
    d) The evaluation harness leaked the answers

12. The SWE Atlas paper shows that:
    a) Vendor harnesses always score lower
    b) Larger models never need harnesses
    c) The same model scores differently across harnesses and rankings can flip
    d) Code benchmarks are unreliable

13. A benchmark is considered "saturated" when leading models exceed roughly:
    a) 50%
    b) 70%
    c) 85%
    d) 99%

14. Outcome evaluation scores:
    a) The final result without regard to how it was reached
    b) The sequence of tool calls
    c) The reasoning tokens produced
    d) The efficiency of the trajectory

15. Trajectory evaluation examines:
    a) Only the final answer
    b) The training data
    c) The model's parameter count
    d) The full sequence of reasoning, tool calls, arguments, and intermediate outputs

16. Which human evaluation form is considered MORE reliable?
    a) Absolute scoring (e.g., 4/5)
    b) Preference / relative comparison between two outputs
    c) Single-sentence feedback
    d) Rating on a 10-point scale

17. Elo rating is borrowed from:
    a) Poker
    b) Competitive chess
    c) Tennis rankings
    d) Credit scoring

18. Exact match evaluation is best suited for:
    a) Writing documents
    b) Generating code
    c) Single-value answers like invoice totals
    d) Synthesizing information from many sources

19. Programmatic checks verify:
    a) Structural properties of output (e.g., valid JSON, unit tests)
    b) Whether output matches an exact string
    c) Human preferences
    d) Model latency

20. IFEval contains instructions that can be verified by:
    a) Human annotators only
    b) Heuristics/validators (e.g., ">400 words")
    c) Elo ratings
    d) Preference pairs

21. A key failure mode of LLM-as-a-judge is:
    a) Being too slow to run
    b) Needing training data
    c) Producing deterministic scores
    d) Position bias — preferring the first option presented

22. Which technique mitigates self-preference bias in LLM judges?
    a) Using a judge from a different model family
    b) Always using the same model as the judge
    c) Increasing temperature to 2.0
    d) Scoring only the final answer

23. Rubric-based evaluation improves reliability by:
    a) Asking one broad quality question
    b) Using only human judges
    c) Scoring binary pass/fail only
    d) Breaking judgments into smaller, more objective criteria

24. The four axes of the Figure 7-7 rubric are:
    a) Accuracy, speed, cost, safety
    b) Recall, precision, F1, latency
    c) Fluency, correctness, completeness, groundedness
    d) Clarity, length, format, citations

25. Which is NOT one of the three trajectory failure modes?
    a) Unsound reasoning
    b) Inefficiency
    c) Unnecessary overhead
    d) Data contamination

26. pass@k measures:
    a) Reliability (succeeds every time)
    b) Capability (succeeds at least once in k tries)
    c) Latency
    d) Token cost

27. pass^k measures:
    a) Capability
    b) The number of passes
    c) The probability all k attempts succeed (reliability)
    d) Harness quality

28. For LLM 1 with 2/2 samples correct and LLM 2 with 1/2, pass@1 is:
    a) 1.0 and 0.5
    b) 1.0 and 1.0
    c) 0.5 and 0.25
    d) 0.5 and 1.0

29. A pass@k estimate is only trustworthy when:
    a) k equals 1
    b) Temperature is 0
    c) The model is deterministic
    d) The number of samples is much larger than k

30. In Figure 7-12, GPT-5.4 and Opus 4.7 have the same pass@1 of 40, but:
    a) Opus 4.7 is more reliable (higher pass^3)
    b) They are identical in all metrics
    c) Neither is reliable
    d) GPT-5.4 is more reliable (higher pass^3 of 28 vs 23)

31. Which paper/source introduced pass^k?
    a) SWE Atlas
    b) The Leaderboard Illusion
    c) τ-bench (Yao et al., 2024)
    d) Evaluating LLMs Trained on Code

32. AgentHarm measures safety for which threat model?
    a) Prompt injection
    b) Memory poisoning
    c) No adversary
    d) Misuse — a malicious user asking for harmful tasks

33. Memory poisoning:
    a) Corrupts what the agent stores and later retrieves
    b) Plants instructions in web pages
    c) Deletes the agent's memory
    d) Slows the agent down

34. Which safety benchmark covers BOTH prompt injection and memory poisoning?
    a) AgentDojo
    b) Agent Security Bench (ASB)
    c) AgentHarm
    d) GAIA

35. The "no adversary" safety category:
    a) Is the most standardized
    b) Is measured by AgentHarm
    c) Only applies to chatbots
    d) Leans on your own test cases, guardrails, and harness design

36. harbor is:
    a) A benchmark dataset
    b) A coding agent
    c) An open-source agent eval harness
    d) A scoring rubric

37. The recommended starting size for a custom eval is:
    a) 100+ cases
    b) Exactly 1 case
    c) 1,000 cases
    d) 5 or 10 cases

38. Which benchmark places agents inside replicas of live websites like Reddit or GitLab?
    a) WebArena
    b) OSWorld
    c) Terminal-Bench
    d) GDPval

39. "A reported number is best read as a model-and-harness pair" means:
    a) Benchmarks are unreliable
    b) Harnesses don't matter
    c) Scores reflect both the model and its scaffold
    d) Only the model matters

40. Which is the most prominent Elo-style crowdsourced model ranking?
    a) Chatbot Arena (LMSYS)
    b) SWE-bench leaderboard
    c) Artificial Analysis
    d) GPQA leaderboard

---

## Section B: True/False (1 point each)

41. A benchmark score always measures agentic capability. (T/F)
42. Code is verifiable, which is why coding benchmarks became the gold standard of agent evaluation. (T/F)
43. Preference judgments are less reliable than absolute scores. (T/F)
44. Human evaluation is the baseline against which automated methods are validated. (T/F)
45. Exact match evaluation works well for open-ended outputs like documents and plans. (T/F)
46. LLM judges are perfect and never make errors. (T/F)
47. Rubrics break complex judgments into smaller, more objective criteria. (T/F)
48. pass@k rewards an agent for succeeding at least once in k tries. (T/F)
49. Reliability and capability are the same thing. (T/F)
50. Agents are more consequential than LLMs because they can take real-world actions. (T/F)
51. Prompt injection plants instructions in content the agent processes. (T/F)
52. Building your own evals should focus only on success cases. (T/F)
53. A single run score is sufficient to evaluate an agent's performance. (T/F)
54. The combinatorial pass@k estimator accounts for dependency between samples. (T/F)
55. When comparing API vendors, reliability (pass^k) matters because you run the model as it ships. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Explain the difference between outcome evaluation and trajectory evaluation, and give one example of a problem trajectory evaluation reveals that outcome evaluation misses.
57. List four of the questions you should ask when reading a benchmark score critically.
58. What is data contamination, and why is it hard to verify?
59. Describe the Evaluator pattern used in the book (Benchmark dataclass + Evaluator class). Why is a new agent created per example?
60. Contrast exact match with programmatic checks, and give one example of each.
61. What are the failure modes of LLM-as-a-judge, and name two mitigation techniques?
62. What is a rubric, and why does rubric-based evaluation improve reliability?
63. Explain the difference between pass@k and pass^k. Using the Figure 7-12 example, show how two models with the same pass@1 can differ in reliability.
64. Describe the three safety threat models and the benchmarks associated with each.
65. What is an eval harness, and what does harbor provide?

---

## Section D: Essay / Applied (5 points each)

66. **Reading a benchmark critically.** A model vendor announces "Our model scores 80% on SWE-bench Verified." Write an essay explaining at least five questions you would ask before accepting this claim, and why each matters. Include the concepts of data contamination, harness, single vs multiple runs, saturation, and cost.

67. **Outcome evaluation methods.** Describe the full spectrum of outcome evaluation methods: human evaluation (direct, preference, win rates, Elo), exact match, programmatic checks, LLM-as-a-judge, and rubric-based evaluation. For each, state when it is appropriate and one limitation.

68. **Capability vs reliability.** Explain the difference between pass@k and pass^k, including their formulas, why simple division only works for pass@1, and why the two metrics can rank models differently. Use the Figure 7-12 example of GPT-5.4 and Opus 4.7 to illustrate.

69. **Trajectory vs outcome evaluation.** Explain what a trajectory is, the three trajectory failure modes (unsound reasoning, inefficiency, unnecessary overhead), and how an LLM judge with a trajectory rubric can evaluate them. Why does trajectory evaluation matter for agents even when outcomes look correct?

70. **Designing your own eval.** You are building an agent that uses tools (web search, file operations) to complete open-ended research tasks. Design a small evaluation suite: which test cases would you include, which scoring methods would you apply, and how would you address reliability and the three safety threat models?

---

## ANSWER KEY

### Section A: Multiple Choice
1. a
2. c
3. d
4. c
5. c
6. a
7. d
8. d
9. a
10. c
11. b
12. c
13. c
14. a
15. d
16. b
17. b
18. c
19. a
20. b
21. d
22. a
23. d
24. c
25. d
26. b
27. c
28. a
29. d
30. d
31. c
32. d
33. a
34. b
35. d
36. c
37. d
38. a
39. c
40. a

### Section B: True/False
41. **F** — Some benchmarks measure only the underlying LLM's non-agentic capabilities.
42. **T** — Code either executes and passes tests or it doesn't.
43. **F** — Relative (preference) judgments tend to be MORE reliable than absolute ones.
44. **T** — Automated methods are validated by correlation with human judgment.
45. **F** — Exact match breaks down for open-ended outputs with many valid answers.
46. **F** — LLM judges have failure modes (confidence bias, position bias, self-preference).
47. **T** — Rubrics decompose judgments into objective criteria.
48. **T** — pass@k = at least one success in k tries.
49. **F** — Capability = can it succeed at all; reliability = succeeds every time.
50. **T** — Agents can send emails, delete files, drop databases.
51. **T** — Prompt injection plants instructions in processed content.
52. **F** — Include failure modes and edge cases, not just successes.
53. **F** — Single runs can flatter a lucky result; multiple runs with variance are more trustworthy.
54. **T** — The combinatorial estimator accounts for dependency (simple division assumes independence).
55. **T** — With vendor APIs you choose the model as it ships, so reliability matters.

### Section C: Short Answer (model answers)
56. **Outcome vs trajectory.** Outcome evaluation scores the final result (patch, answer) ignoring the path. Trajectory evaluation scores the steps (reasoning, tool calls, args, intermediate outputs). Example: Figure 7-9 shows three agents that all pass outcome evaluation — one guessed and got lucky (unsound reasoning), one wasted tool calls (inefficiency), one overthought (unnecessary overhead). Only trajectory evaluation reveals these.
57. **Critical-reading questions (any four):** How was the score produced? Is partial success rewarded? Which agentic harness was used? Single run or multiple? Is the benchmark in the training data (contamination)? Who ran the evaluation? Were prompts tuned for the benchmark? Does the scaffolding generalize? Is it saturated? Which subset/variant? What was the cost?
58. **Data contamination.** Benchmark examples appear in the model's training data, so scores reflect memorization instead of capability. Hard to verify because most models don't disclose training datasets; only fully open models make this checkable.
59. **Evaluator pattern.** `Benchmark` = name + examples + scorer (function returning bool or score). `Evaluator.run()` creates a fresh agent per example (resetting memory and trajectory), runs it on the task, applies the scorer, collects pass/fail, and averages into `pass_rate`. A new agent per example ensures independent evaluation without cross-example memory/trajectory leakage.
60. **Exact match vs programmatic checks.** Exact match = deterministic string comparison (e.g., invoice total = $1,482.81) — simple but brittle to formatting and open-ended answers. Programmatic checks = code verifying structural properties (valid JSON, unit tests that fail-before/pass-after a patch; IFEval word/sentence/character heuristics) — fast, cheap, objective, and extendable.
61. **LLM-judge failure modes & mitigations.** Failure modes: confidence bias, position bias (prefers first option), self/family-model bias. Mitigations: different-family judge, swap presentation order, ask for written rationale / use reasoning model, use judge ensembles (vote + aggregate).
62. **Rubric.** A structured set of criteria with defined score levels (e.g., fluency, correctness, completeness, groundedness, each 1–5). It improves reliability by breaking complex judgments into smaller objective criteria, giving high-resolution per-axis failure signals (a low completeness score means a different failure than a low groundedness score).
63. **pass@k vs pass^k.** pass@k = P(at least one of k passes) → capability; formula for k=1 is correct/total; for k>1 use the combinatorial estimator. pass^k = P(all k pass) = C(c,k)/C(n,k) → reliability. In Figure 7-12, GPT-5.4 and Opus 4.7 share pass@1 = 40, but GPT-5.4 has pass^3 = 28 vs Opus 4.7's 23 → GPT-5.4 is more reliable, while Opus 4.7 has the highest pass@3 (60) → highest capability ceiling.
64. **Three safety threat models.** (1) Misuse — malicious user; AgentHarm (refusal rate + harm score). (2) Data manipulation — prompt injection & memory poisoning; AgentDojo and Agent Security Bench (attack success rate). (3) No adversary — agent's own error on benign tasks; least standardized, relies on your own test cases, guardrails, harness design.
65. **Eval harness.** Infrastructure that takes test cases, runs the agent against each, captures the full trajectory, applies scoring methods, and aggregates results. harbor = open-source harness for agents: containerized trials at scale, standardized task/success-criteria format, and a registry (Terminal-Bench ships through it), letting you run established benchmarks alongside custom evals.

### Section D: Essay (grading notes)
66. **Expect** any five of: how the score was produced (deterministic vs LLM judge); whether partial success is rewarded; which harness (model-and-harness pair — SWE Atlas evidence); single vs multiple runs (variance); data contamination (memorization); who ran it (self-report vs third-party, Leaderboard Illusion); prompt tuning for the benchmark; whether scaffolding generalizes; saturation (~85%); which variant; cost (tokens/latency).
67. **Expect** each method with use case + limitation: human direct scoring (gold standard but slow/expensive); preference/win rates/Elo (relative judgments reliable, scales via Chatbot Arena but still costly); exact match (simple, powers many benchmarks, brittle to format/open-ended); programmatic checks (fast/cheap/objective; needs machine-checkable outputs like code/JSON); LLM-as-a-judge (handles open-ended qualities; failure modes: confidence/position/self-bias); rubrics (structured criteria, interpretable; more design effort).
68. **Expect** definitions, formulas (pass@1 = correct/total; combinatorial pass@k; pass^k = C(c,k)/C(n,k)), why simple division breaks for k>1 (independence assumption), the GPT-5.4/Opus 4.7 example (same pass@1 40; GPT-5.4 pass^3 28 > 23; Opus pass@3 60 = capability ceiling), and the training-vs-vendor interpretation.
69. **Expect** trajectory definition; the three failure modes; how a trajectory rubric handed to an LLM judge scores steps (right tools/args, efficiency in calls/steps/reasoning tokens, step-by-step coherence); and why it matters (outcomes can look correct while behavior is unsound/inefficient/overhead-heavy); mention T-Eval/AgentBoard/TRACE/AgentProcessBench.
70. **Expect** a concrete plan: curated test cases covering success + failure modes + edge cases; scoring mix (exact match where single-value, programmatic checks for structure, LLM-as-a-judge + rubrics for open-ended); reliability via pass@k/pass^k across multiple runs (temperature ~0.7); safety tests for misuse (refusals), prompt injection in retrieved content, and own-error scenarios; automated running on every change; optionally via an eval harness like harbor.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely benchmark categories or pass@k/pass^k).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
