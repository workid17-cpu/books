# Chapter 7 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 7 "Evaluating Agents"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings are paraphrased/annotated; every substantive paragraph is covered.

---

## Section 1: Introduction

1. **"The previous chapters built up the components of an agent one by one: how it reasons, how it uses tools, how it stores and retrieves memory, and how it plans across multiple steps. By now you have a mental model of how these pieces fit together to form the system we call an AI agent. The natural next question to ask is, how do we know if any of it is working?"**
   - **Plain-English:** We've assembled the full agent; now we need to test whether it actually works.
   - **Word meanings:** *components* = parts (reasoning, tools, memory, planning); *mental model* = an internal understanding of how the system works.
   - **Technical terms:** agent = LLM + reasoning + tools + memory + planning.

2. **"That's what this chapter is about... Evaluation is how you move from 'it seems to work' to 'I have evidence it works.' It's also how you catch the moment it stops working."**
   - **Plain-English:** Evaluation converts subjective impressions into objective evidence and detects regressions.
   - **Technical terms:** evaluation = measuring agent performance against defined criteria.

3. **"Evaluations are one of the most underinvested areas in agent development. Teams often rely on manual testing and intuition longer than they should, then find themselves unable to ship improvements confidently or diagnose regressions when they appear."**
   - **Plain-English:** Teams under-invest in evaluation; they test by hand and gut feel, and later can't safely ship or find what broke.
   - **Word meanings:** *underinvested* = not enough resources/time devoted; *regression* = something that previously worked now breaking.
   - **Technical terms:** regression = performance/correctness degradation after a change.

4. **Figure 7-1: "This chapter covers how LLMs and agents are evaluated and explores how we move from evaluating the output to evaluating an agent's intermediate steps (trajectory)."**
   - **Plain-English:** The chapter's arc: first judge final answers, then judge the journey (steps taken).
   - **Technical terms:** trajectory = the sequence of reasoning, tool calls, and intermediate outputs.

## Section 2: Public Benchmarks and Leaderboards

5. **"A benchmark is a fixed set of tasks paired with a way to score them, so that different models can be measured against the same yardstick and compared directly."**
   - **Plain-English:** A benchmark is a standard test + scoring rule used to compare models fairly.
   - **Word meanings:** *yardstick* = standard of comparison.
   - **Technical terms:** benchmark = dataset + scoring procedure + metric.

6. **"Take SWE-bench Verified... It's a collection of real GitHub issues from open source projects. To score a point, a model (or an agent built on top of it) has to read the description of the issue, explore the code repository, and produce a patch that makes the project's existing tests pass (they exist but are hidden from the model). The score is simply the percentage of issues the agent solves."**
   - **Plain-English:** SWE-bench Verified tests whether an agent can fix real GitHub bugs by writing a patch that passes hidden tests.
   - **Word meanings:** *patch* = a code change fixing the issue; *hidden tests* = tests not shown to the model.
   - **Technical terms:** SWE-bench Verified = curated version of SWE-bench; score = % of issues solved.

7. **"Most benchmarks follow the same outline: a dataset of problems, a procedure for attempting them, and a metric that collapses performance into a single number you can drop into a table."**
   - **Plain-English:** Every benchmark = problems + attempt procedure + a summary score.
   - **Technical terms:** metric = function summarizing performance as one number.

8. **"The problem is that reading tables that compare models on various benchmarks requires a bit of care and nuance. A score of 80% on SWE-bench Verified and a score of 80% on MMLU represent completely different claims about completely different capabilities..."**
   - **Plain-English:** Equal-looking percentages measure very different things; don't compare naively.
   - **Technical terms:** MMLU = knowledge exam benchmark; capabilities = different skills being tested.

9. **"Some scores come from running an agent once while other scores aggregate many runs... Some use deterministic test suites while others rely on an LLM judge that introduces its own biases. And some benchmarks... aren't really measuring agent capability at all, but rather the non-agentic capabilities of the underlying LLM that powers it."**
   - **Plain-English:** Scores differ in how they're produced (single vs many runs; deterministic vs LLM judge), and some measure only the raw LLM, not agentic skills.
   - **Technical terms:** deterministic = fixed, repeatable check; LLM judge = model scoring outputs; agentic = involving tool use/multi-step action.

10. **Figure 7-2: "A benchmark table from Google."**
    - **Plain-English:** Example of the kind of comparison table published in model releases.
    - **Technical terms:** leaderboard = ranked table of model scores.

### Coding and Software Engineering

11. **"Arguably the most mature family of agent benchmarks measures the ability to write, fix, and reason about code... the gold standard of agent evaluation precisely because code is verifiable: the code either executes and passes unit tests or it does not."**
    - **Plain-English:** Coding benchmarks are the most established because code outcomes are objectively checkable.
    - **Word meanings:** *mature* = well-developed; *verifiable* = checkable by executing.
    - **Technical terms:** unit tests = automated checks of code behavior.

12. **"When SWE-bench launched in 2023, best-in-class agents resolved around 2% of tasks, but by 2026, leading agents on the public leaderboard cracked 70% on SWE-bench Verified–a curated version of the benchmark designed to remove ambiguous or unreliable test cases."**
    - **Plain-English:** SWE-bench went from 2% → 70%+ solved tasks between 2023 and 2026.
    - **Word meanings:** *curated* = cleaned up; *ambiguous* = unclear.
    - **Technical terms:** SWE-bench Verified = filtered subset with reliable tests.

13. **"Terminal-Bench is another coding benchmark where the agent operates a Linux terminal to solve software, system, and environment issues."**
    - **Plain-English:** The agent works through a real terminal to fix system/software problems.
    - **Technical terms:** Terminal-Bench = terminal-based coding benchmark.

14. **"We cover coding agents and more of their evaluations in Chapter 10."**
    - **Plain-English:** Deeper coverage is deferred to the coding-agent chapter.

### Tool Use

15. **"Tool use benchmarks test whether an agent calls the right tools with the right arguments. This is a more targeted check on the specific capabilities of tool calling... instead of end-to-end task completion."**
    - **Plain-English:** These benchmarks check tool-calling skill specifically, not whole-task success.
    - **Word meanings:** *targeted* = focused on one capability; *end-to-end* = whole task.
    - **Technical terms:** tool call = invoking a function with arguments.

16. **"BFCL (Berkeley Function Calling Leaderboard) is one key benchmark here, testing function-calling accuracy across hundreds of API schemas including nested parameters and ambiguous inputs."**
    - **Plain-English:** BFCL scores how accurately models call functions across many schemas.
    - **Technical terms:** API schema = formal description of a function's parameters/types.

17. **"τ-bench (pronounced 'tau bench') goes deeper, testing tool use across realistic multi-turn customer service scenarios that require following specific business policies and maintaining context over a long conversation."**
    - **Plain-English:** τ-bench tests tools in long, policy-driven customer-service conversations.
    - **Word meanings:** *multi-turn* = many back-and-forth messages.
    - **Technical terms:** τ-bench = tau bench, tool-use + policy-following benchmark.

18. **NOTE: "for some benchmarks, specific packages and harnesses were created in which you need to run your LLM. τ-bench, for instance, has an entire framework... different benchmarks might need different frameworks, which hinders tests of the generalizability of harnesses."**
    - **Plain-English:** Some benchmarks ship their own runner frameworks; this makes cross-benchmark harness comparison hard.
    - **Word meanings:** *hinders* = blocks/limits.
    - **Technical terms:** harness = software scaffold running the agent.

### Software and Computer-Use Environments

19. **"Benchmarks such as OSWorld place agents inside simulated computer environments where they must complete realistic tasks such as editing documents, managing files, or configuring software."**
    - **Plain-English:** OSWorld puts agents in a simulated desktop to do real computer tasks.
    - **Technical terms:** OSWorld = computer-use benchmark.

20. **"Unlike Terminal-Bench, however, this group of benchmarks often requires interacting with graphical interfaces, clicking buttons, navigating menus, and interpreting visual elements that appear in software designed for humans (e.g., Excel or online shops)."**
    - **Plain-English:** These benchmarks need GUI interaction, not just terminal commands.
    - **Word meanings:** *graphical interfaces* = on-screen visuals (buttons, menus).
    - **Technical terms:** GUI = graphical user interface.

21. **"The WebArena benchmark narrows the scope to browser-based tasks, placing agents inside realistic replicas of live websites (such as a Reddit forum or a GitLab instance) to complete multi-step goals."**
    - **Plain-English:** WebArena tests agents navigating realistic copies of real websites.
    - **Technical terms:** WebArena = browser-based benchmark; replicas = cloned environments.

### Real-World Task Completion

22. **"GAIA is one such benchmark that presents difficult (at the time), real-world questions that often require reading files and using web search to arrive at a verifiable final answer."**
    - **Plain-English:** GAIA asks hard real-world questions needing file reading and web search.
    - **Technical terms:** GAIA = real-world task benchmark; verifiable answer = checkable result.

23. **"GDPval is another one drawn from knowledge work of multiple professions from various sectors and domains."**
    - **Plain-English:** GDPval uses tasks from actual professional knowledge work.
    - **Technical terms:** GDPval = profession-derived benchmark.

24. **"A 2026 analysis titled How Well Does Agent Development Reflect Real-World Work? found that today's benchmarks cluster heavily around programming, while the occupations that employ the most people and generate the most economic value go almost untested. That gap is where much of the expected demand for evaluations is likely to land."**
    - **Plain-English:** Benchmarks focus on programming; the biggest/most valuable jobs are barely tested — an opportunity.
    - **Word meanings:** *cluster* = concentrate.
    - **Technical terms:** benchmark gap = mismatch between benchmarks and real work.

### Reasoning and Knowledge

25. **"MMLU (Massive Multitask Language Understanding) is one of the earliest and most widely cited examples, covering dozens of subjects ranging from physics and medicine to law and history."**
    - **Plain-English:** MMLU tests knowledge across many academic subjects.
    - **Technical terms:** MMLU = exam-style knowledge benchmark.

26. **"GPQA Diamond is a more recent benchmark that pushes the difficulty further. It focuses on graduate-level questions in fields such as physics, biology, and chemistry that are intentionally designed to be difficult even for domain experts and resistant to simple memorization."**
    - **Plain-English:** GPQA Diamond uses graduate-level science questions that even experts find hard.
    - **Word meanings:** *resistant to* = hard to beat with.
    - **Technical terms:** GPQA Diamond = hard science benchmark subset.

27. **"Humanity's Last Exam takes this even further, assembling extremely challenging questions across many disciplines."**
    - **Plain-English:** An even harder, cross-discipline benchmark.
    - **Technical terms:** Humanity's Last Exam = very hard benchmark.

28. **"One thing that's important to note is that these often test the underlying LLM, not necessarily the agentic abilities of tool use across multiple steps."**
    - **Plain-English:** Reasoning benchmarks mostly test the raw model, not agentic skills.
    - **Technical terms:** agentic ability = tool-use/multi-step capability.

### Other Groups of Benchmarks

29. **"Some focus on abstract reasoning puzzles, such as ARC-AGI, which attempt to measure general problem-solving on unfamiliar tasks. Others evaluate multimodal reasoning... as in MMMU. There are also benchmarks for long-context retrieval, sometimes called 'needle-in-a-haystack' tests..."**
    - **Plain-English:** More benchmarks cover abstract reasoning, multimodal reasoning, and long-context retrieval.
    - **Technical terms:** ARC-AGI = abstract reasoning benchmark; MMMU = multimodal benchmark; needle-in-a-haystack = long-context retrieval test.

30. **Table 7-1 (summary of categories):** Coding (SWE-bench Verified, Terminal-Bench) → patch correctness/test passage; Tool use (BFCL, τ-bench) → function-calling accuracy, multi-turn policy following; Computer use (OSWorld, WebArena) → action sequences in desktop/browser environments; Real-world (GAIA, GDPval) → reasoning/retrieval on open-ended tasks; Reasoning/knowledge (MMLU, GPQA Diamond, HLE) → exam-style questions; Specialized (ARC-AGI, MMMU, needle-in-a-haystack) → abstract reasoning, multimodal, long-context retrieval.
    - **Plain-English:** A one-glance mapping of benchmark families to what they measure.

### Reading Benchmark Scores Critically

31. **"How was the score produced? ... different evaluation setups can produce different results even on the same benchmark. Some come from deterministic checks such as unit tests or exact answers, while others rely on LLM judges..."**
    - **Plain-English:** Ask how a score was computed; different methods give different numbers.
    - **Technical terms:** deterministic check = fixed rule; LLM judge = model-based scoring.

32. **"Is partial success rewarded? ... A high partial-credit score can hide an agent that rarely completes a task end to end, which is usually what you need it to do."**
    - **Plain-English:** Partial credit can mask agents that rarely finish whole tasks.
    - **Technical terms:** partial credit = scoring part-way success.

33. **"Which agentic harness was used? The same model can post different scores on a benchmark depending on the agentic harness it runs in. How the harness constructs prompts, parses tool calls, handles retry logic, and manages context and conversation history all move the number even with the model held fixed."**
    - **Plain-English:** The scaffold around the model changes scores; always report model + harness together.
    - **Technical terms:** harness = scaffolding (prompting, parsing, retries, context management).

34. **"The SWE Atlas benchmark paper shows this directly: it runs each frontier model both in its vendor harness (Codex CLI, Claude Code, Gemini CLI) and in a common minimal harness (mini-SWE-agent, which exposes only a bash tool) to separate the model from its scaffold. Scores shift between the two, and the ranking of the top models can even flip, so a reported number is best read as a model-and-harness pair."**
    - **Plain-English:** SWE Atlas proves harness matters: rankings can flip between vendor harnesses and a minimal one.
    - **Technical terms:** model-and-harness pair = a score belongs to both; mini-SWE-agent = minimal bash-only harness.

35. **"Single run or multiple? A score from one run can flatter a lucky result. More trustworthy results would often report a metric that aggregates the score over many runs with a variance estimate. Some metrics are aggregate in nature, such as pass@k."**
    - **Plain-English:** Prefer many runs with variance estimates over single-run flukes.
    - **Technical terms:** pass@k = probability at least one of k attempts passes.

36. **"Is the benchmark in the training data? Data contamination is one of the most insidious issues in machine learning. If a model was trained on examples drawn from the benchmark, then the score is skewed by the model's memorization instead of being strictly measured on capability."**
    - **Plain-English:** If benchmark examples leaked into training data, scores reflect memorization, not ability.
    - **Word meanings:** *insidious* = sneaky/harmful in hidden ways.
    - **Technical terms:** data contamination = benchmark/train overlap.

37. **"Who ran the evaluation? Self-reported scores from the lab releasing a model deserve more scrutiny than third-party replications... Artificial Analysis and Chatbot Arena are two widely cited independent sources. Yet even independent leaderboards can be gamed, as described in The Leaderboard Illusion."**
    - **Plain-English:** Trust independent replication over self-reports, but even those can be gamed.
    - **Technical terms:** third-party replication = independent re-run; Leaderboard Illusion = paper on gamed leaderboards.

38. **"Were prompts tuned specifically for that benchmark? Prompt engineering against a known benchmark can meaningfully move scores without reflecting any generalizable capability improvement. If the same prompting approach is not applied to all systems in the comparison table, the comparison can be uneven."**
    - **Plain-English:** Prompt-tuning for a benchmark inflates its score without real capability gains.
    - **Technical terms:** prompt engineering = crafting prompts; uneven comparison = apples-to-oranges.

39. **"Does the scaffolding generalize? A system that includes a retrieval pipeline, a custom tool, or a verification step may score well on the benchmark while requiring significant engineering to transfer to a real deployment."**
    - **Plain-English:** Benchmark-only scaffolding may not transfer to production.
    - **Word meanings:** *transfer* = carry over to.
    - **Technical terms:** scaffolding = extra system components.

40. **"Is the benchmark saturated? Once leading models exceed a certain threshold, say 85%, on a benchmark, score differences start to become noisy and less indicative of actual performance. That's when you see the industry move to a more difficult benchmark..."**
    - **Plain-English:** Past ~85%, differences are noise; industry moves to harder tests.
    - **Word meanings:** *saturated* = maxed out.
    - **Technical terms:** saturation threshold ≈ 85%.

41. **"Which subset or variant was used? Many benchmarks have multiple versions that often vary in difficulty. Examples include GPQA versus GPQA Diamond, ARC Easy versus ARC Challenge, and MMLU versus MMLU-Pro."**
    - **Plain-English:** Always check which variant of a benchmark was used — difficulty varies.
    - **Technical terms:** variant = version of a benchmark (easy/challenge/diamond/pro).

42. **"What was the cost? A system that scores 20% more while spending 10 times the tokens or wall-clock time of a competitor isn't straightforwardly better. Some evaluations report this; many don't."**
    - **Plain-English:** Higher scores at 10× cost aren't clearly better; cost/latency matter in production.
    - **Word meanings:** *wall-clock time* = actual elapsed time.
    - **Technical terms:** token cost = compute/latency economics.

## Section 3: Outcome Evaluation

43. **"Outcome evaluation scores the final results of a task–the patch produced, the answer returned, the report written–without regard to how the agent got there. We contrast this with... trajectory evaluation, which examines the sequence of steps and tool calls the agent made along the way."**
    - **Plain-English:** Outcome = judge only the end result; trajectory = judge the journey.
    - **Technical terms:** outcome evaluation = endpoint scoring.

### Human Evaluation

44. **"The most direct way to score an agent's output is to have a person read it and judge it." (Figure 7-3: a human evaluator rates a single output against criteria, producing a direct quality score.)**
    - **Plain-English:** A human reads and scores the output (e.g., 4/5).
    - **Technical terms:** annotator/evaluator = human rater.

45. **"The challenge here is that it's often difficult to get an objective score in isolation, which creates the need for the second form of human evaluation."**
    - **Plain-English:** Absolute scoring is unreliable; relative scoring is better.

46. **"Preference evaluation... presents the evaluator with outputs from two agents side by side on the same task and asks a simpler question: which answer do you prefer? Relative judgements tend to be more reliable than absolute ones. It's easier for a human evaluator to say that 'A is better than B' than to decide that A deserves 4 points and that B deserves 3 points."**
    - **Plain-English:** Pairwise preference is easier and more reliable than absolute scoring.
    - **Technical terms:** preference evaluation = pairwise comparison.

47. **"Scaled across many tasks, we can begin to speak about win rates: on the tasks in a certain evaluation set, Agent A was preferred on 80% of the tasks while Agent B was preferred on 20%..."**
    - **Plain-English:** Aggregated preferences become win rates.
    - **Technical terms:** win rate = % of pairwise comparisons won.

48. **"Comparing two models like this in a pair-wise fashion can be extended to comparing more than two agents through an Elo rating system, borrowed from competitive chess. Rather than running every possible head-to-head comparison... each pairwise result updates a running score of both models/agents; wins increase the score and losses decrease it according to a specific formula that takes into consideration the relative strength of the opponent."**
    - **Plain-English:** Elo lets you rank many agents without comparing every pair; scores update by opponent strength.
    - **Technical terms:** Elo = rating formula; opponent strength weighting.

49. **"Chatbot Arena (LMSYS) is the most prominent example of this applied at scale, where thousands of users compare anonymous model outputs and stable rankings emerge over time (using a rating similar to Elo)."**
    - **Plain-English:** Chatbot Arena crowdsources Elo-style model rankings.
    - **Technical terms:** Chatbot Arena = crowdsourced leaderboard.

50. **"Human evaluation is slow and expensive, but it remains the baseline against which all automated methods are validated. When a new LLM judge or scoring metric is proposed, showing that it correlates with human judgement is often how researchers establish that it measures something real."**
    - **Plain-English:** Human judgment is the gold standard; automated metrics must correlate with it.
    - **Technical terms:** correlation with human judgment = validation standard.

### Automated Evaluation

51. **"Agent builders iterate over hundreds if not thousands of prompt changes, model swaps, versions of the agent, and example test points that can easily add up to requiring millions of complex judgements. That's where automated evaluation comes in."**
    - **Plain-English:** Automation is required to evaluate at iteration scale.
    - **Technical terms:** automated evaluation = programmatic/model-based scoring.

52. **"Automated evaluation covers a spectrum from static automated checks to judgements made by another model."**
    - **Plain-English:** Range: rule-based checks → model-based judgment.

**Building a simple agent evaluator**

53. **"We first need to create the Benchmark dataclass. The Benchmark contains information on the task (name), the examples in the benchmark to test (examples), and the method of performing automated evaluation (scorer). The scorer is a function that takes in a prediction and an example from the benchmark. Depending on the scorer, it should either always return a boolean to indicate whether the prediction is correct or not, or it should always return a score."**
    - **Plain-English:** `Benchmark` = name + examples + a scorer function (bool or numeric score).
    - **Technical terms:** dataclass = data container; scorer = prediction-vs-example function.

```python
from dataclasses import dataclass
from typing import Callable

# Type hint for the scorers
Scorer = Callable[[str, dict], bool | float]

@dataclass
class Benchmark:
    name: str
    examples: list[dict]
    scorer: Callable
```

54. **"We then need an Evaluator class that runs a set of examples against a selected scorer. We also need a new agent every time we run a benchmark to reset its memory and trajectory... The Evaluator will run a check for each example in the benchmark, determine whether the agent has generated the correct output, and simply track a pass or fail. The pass/fail rate over all examples is averaged into the 'pass_rate'."**
    - **Plain-English:** `Evaluator` runs each example on a fresh agent, scores it, and averages into a pass rate.
    - **Technical terms:** fresh agent = new instance (empty memory/trajectory); pass_rate = fraction correct.

```python
from illustrated_agents.chapters.ch2 import LLM
from illustrated_agents.chapters.ch4 import Memory
from illustrated_agents.chapters.ch5 import NativeTools
from illustrated_agents.chapters.ch6 import NativeReAct, TinyAgent

llm = LLM(model="gemma4:e4b", think=True)


class Evaluator:
    """Run a TinyAgent over a Benchmark and aggregate the results."""

    def __init__(self, create_agent: Callable):
        self.create_agent = create_agent

    def run(self, benchmark: Benchmark) -> dict:
        results = []
        for example in benchmark.examples:
            agent = self.create_agent()
            prediction = agent.run(example["task"]) or ""
            passed = benchmark.scorer(prediction, example)
            results.append({"prediction": prediction, "passed": passed})

        if results:
            pass_rate = sum(result["passed"] for result in results) / len(results)
        else:
            pass_rate = 0.0

        return {
            "name": benchmark.name,
            "pass_rate": pass_rate,
            "results": results,
        }


def create_agent():
    """Create a new instance of TinyAgent"""
    return TinyAgent(
        llm=llm,
        memory=Memory(),
        tools=NativeTools(),
        planner=NativeReAct(),
    )
```

**Exact match**

55. **"The most straightforward form of automated evaluation is a deterministic check: the output either matches the expected answer or it doesn't."**
    - **Plain-English:** Exact match = direct string comparison to expected answer.
    - **Technical terms:** exact match = deterministic equality check.

56. **"This goes quite far and powers a large number of benchmarks. Yet its beautiful simplicity is also its main drawback; many tasks in practice don't simply output a single value. If the agent, for example, was to write a document, generate code, produce a plan, or synthesize information across many sources, exact match breaks down because there are many valid ways to express a correct answer."**
    - **Plain-English:** Exact match works for single-value answers but fails for open-ended outputs.
    - **Word meanings:** *drawback* = disadvantage.

57. **"Exact match is also brittle against trivial formatting differences. For the invoice example, a prompt that doesn't pin down the format can yield answers such as 1,482.81 (no dollar sign), 1482.81 (no thousands separator), or $1,482.81. (with a trailing period), and a strict check flags every one of them as wrong."**
    - **Plain-English:** Formatting variations (dollar sign, comma, period) all fail a strict check.
    - **Word meanings:** *brittle* = fragile.
    - **Technical terms:** formatting differences = string-level variations.

58. **"MMLU-Pro... consists of many different tasks with multiple-choice answers... Compared to MMLU, it extends the number of multiple-choice options to 10 to make the tasks more difficult and prevent random guessing. Those options are labeled A through J."**
    - **Plain-English:** MMLU-Pro has 10 options (A–J) instead of 4, reducing guessability.
    - **Technical terms:** MMLU-Pro = 10-choice knowledge benchmark.

59. **"The exact_match_scorer gives back either True or False depending on whether the answer is correct. Since we know the answer beforehand, the check is relatively straightforward."**
    - **Plain-English:** Compare the model's letter to the known-correct letter.

```python
import re

def exact_match_scorer(prediction: str, example: dict) -> bool:
    """Return True if the answer matches the prediction, False otherwise"""
    match = re.search(r"\b([A-J])\b", prediction.upper())
    return match.group(1) == example["expected"]

mmlu_pro = Benchmark(
    name="MMLU Pro",
    examples=[
        {"task": "...pituitary gland? ... Answer with only the letter.", "expected": "J"},
        {"task": "...mean cranial capacity of Homo erectus?...", "expected": "E"},
        {"task": "...Moore's 'ideal utilitarianism'...", "expected": "I"},
    ],
    scorer=exact_match_scorer,
)

result = Evaluator(create_agent).run(mmlu_pro)
print(result)
# {'name': 'MMLU Pro', 'pass_rate': 0.333, 'results': [
#   {'prediction': 'J', 'passed': True},
#   {'prediction': 'H', 'passed': False},
#   {'prediction': 'G', 'passed': False}]}
```

60. **"If we were to use the full benchmark with more than 12,000 question/answer pairs, running the evaluation would take significant time. Instead, we grab three examples from the dataset..."**
    - **Plain-English:** Full MMLU-Pro has 12,000+ questions; the book uses 3 for demonstration.
    - **Technical terms:** 12,000+ QA pairs = full benchmark size.

61. **"The model got one question right and two wrong. That is not too bad for such a small model. These types of questions test knowledge, which tend to be easier for larger models since they have more parameters to store information."**
    - **Plain-English:** Small model: 1/3 correct; larger models store more knowledge.
    - **Technical terms:** parameters = model memory capacity.

**Programmatic checks**

62. **"Programmatic checks extend exact matches into the broader category of automated verifiers. Rather than checking for a single string, you write code that verifies structural properties of the output."**
    - **Plain-English:** Programmatic checks verify output structure/properties, not exact strings.
    - **Technical terms:** automated verifier = code checking output properties.

63. **"If we expect a JSON string output, we can automatically check if the output is valid JSON. For a coding agent where we expect a function implementation, we run unit tests and validate that the function behaves as expected. For more advanced coding outputs such as SWE-bench... the programmatic checks are a suite of unit tests that fail before the patch and have to run successfully after the patch is applied."**
    - **Plain-English:** Checks range from JSON validity to unit tests (pre/post patch for SWE-bench).
    - **Technical terms:** pre/post test suite = tests failing before patch, passing after.

64. **"This approach is fast, cheap, and objective. It's one reason coding agents rose much faster than other kinds of agents–the code modality allows for automated checks, which proves useful for evaluation that also guides the training process."**
    - **Plain-English:** Speed/cost/objectivity let coding agents iterate fast and be used in training.
    - **Technical terms:** modality = output type (code is machine-checkable).

65. **"To implement programmatic checks, we'll use the IFEval benchmark. This benchmark contains verifiable instructions, such as 'write in more than 400 words' or 'mention the keywords of AI at least 3 times.' These instructions can be verified by heuristics and need a separate 'validator' for each of these instructions."**
    - **Plain-English:** IFEval gives instructions checkable by heuristics, each with a validator.
    - **Technical terms:** IFEval = instruction-following evaluation; validator/heuristic = rule-based check.

```python
def programmatic_scorer(prediction: str, example: dict) -> bool:
    """Check a prediction against its related check."""
    return example["check"](prediction)

ifeval = Benchmark(
    name="IFeval",
    examples=[
        {"task": "Write a funny song <10 sentences...", "check": lambda text: sum(1 for c in text if c in ".!?") < 10},
        {"task": "Write ad copy (≤150 words)...", "check": lambda text: len(text.split()) <= 150},
        {"task": "Japan itinerary...", "check": lambda text: "," not in text},
    ],
    scorer=programmatic_scorer,
)

result = Evaluator(create_agent).run(ifeval)
print(result)
# {'name': 'IFeval', 'pass_rate': 1.0, 'results': [all three passed True]}
```

66. **"The pass rate gives a score of 1, meaning that the model correctly followed all instructions!"**
    - **Plain-English:** The agent passed all three instruction checks.

**LLM-as-a-judge**

67. **"When the output is too open-ended for a programmatic check, a natural move is to ask another capable language model to evaluate it. This is the LLM-as-a-judge pattern. LLM-as-a-judge is typically used to produce a single score or to choose a preferred output from two agents."**
    - **Plain-English:** Use a second LLM to score open-ended outputs.
    - **Technical terms:** LLM-as-a-judge = model-based evaluation.

68. **"Strong language models can assess fluency, logical coherence, information consistency given a ground truth context, and tone in ways that no programmatic check can."**
    - **Plain-English:** LLM judges handle qualities programmatic checks can't (fluency, tone, coherence).
    - **Technical terms:** ground truth context = reference answer/info.

69. **"LLM judges have failure modes worth knowing... They can prefer outputs that sound confident over ones that are actually correct. They can be sensitive to presentation order (e.g., tending to prefer the first option presented to them). They inherit biases from their own training and may tend to prefer their own output or those from their own family of models to that of other models."**
    - **Plain-English:** Judge failure modes: confidence bias, position bias, self-family bias.
    - **Technical terms:** position bias = first-option preference; self-preference = favoring own family.

70. **"Several techniques help mitigate these drawbacks. Using a judge from a different model family reduces the tendency to favor its own outputs. Swapping presentation order of preference sets across evaluation runs controls for position bias. Asking a judge to produce a written rationale or using a reasoning model as judge can improve consistency. We can also use ensembles of judges, where multiple models vote and we aggregate their judgments."**
    - **Plain-English:** Mitigations: different-family judge, order swapping, rationale/reasoning judge, ensembles.
    - **Technical terms:** judge ensemble = voting across judges.

71. **"We'll choose a different and more capable judge of the output, namely Google's Gemini model... a free tier available that lets us choose a capable model, Gemini 3.1 Flash-Lite."**
    - **Plain-English:** The book uses Gemini 3.1 Flash-Lite (free tier) as the judge.

```python
judge = LLM(
    model="gemini-3.1-flash-lite",
    base_url="https://generativelanguage.googleapis.com/v1beta/openai",
    api_key="MY_API_KEY",
)
```

72. **"Instead of returning a boolean, we're now returning a single score between 0 and 1. This allows for a much more fine-grained perspective on how well the agent is doing."**
    - **Plain-English:** Numeric 0–1 scores give finer granularity than booleans.

```python
def judge_scorer(prediction: str, example: dict) -> bool:
    """The LLM-as-a-judge scorer."""
    prompt = f"""
Score the response from 0.0 to 1.0.

Expected: {example["expected"]}
Response: {prediction}

Reply with only a single number.
"""
    response = judge.generate([{"role": "user", "content": prompt}])
    score = float(response.content.strip().split()[0])
    return score
```

73. **"Instead of using multiple-choice questions in the benchmark, we now simply remove them and make them open-ended questions. That way, the answer is more difficult to score programmatically or using an exact match and instead requires a judge."**
    - **Plain-English:** The same questions, made open-ended, now need a judge.
    - **Technical terms:** open-ended = free-form answers.

74. **"The pass rate is now lower than the multiple-choice variant, as it gave the incorrect answer to example two. It's somewhat close, which might be the reason why it got a score of 0.4 instead of 0."**
    - **Plain-English:** A near-miss answer got 0.4 instead of 0 — numeric scores are nuanced.
    - **Technical terms:** partial credit via judge scoring.

75. **"What's especially interesting about this output is that a score of 1 was given to the final example. The multiple-choice variant specifically stated that the answer should be 'good' (I) and not 'happiness' (G). Although 'good' is mentioned, the answer specifies 'happiness' at the start. As such, this demonstrates a failure mode of using LLMs to judge LLMs. They are not perfect by any means, just like human evaluation. A mix of evaluation metrics and benchmarks is generally preferred to limit the downsides of any one benchmark."**
    - **Plain-English:** The judge gave 1.0 to a technically wrong answer — a real LLM-judge failure; use mixed metrics.
    - **Technical terms:** failure mode = systematic judge error.

**Rubric-based evaluation**

76. **"A refinement on the raw LLM-as-a-judge approach is to give the judge an explicit rubric: a structured set of criteria, each with defined score levels. Instead of asking a single 'Is this output good?' you score the output against multiple criteria, each capturing a different axis of quality."**
    - **Plain-English:** Rubrics decompose quality into criteria with defined score levels.
    - **Technical terms:** rubric = structured scoring criteria.

77. **"You ask something more like 'On a scale of 1-3, does this response correctly cite evidence from the provided sources? Score 1 if no sources are cited, score 2 if sources are cited but not directly relevant, score 3 if sources are cited and directly support the claim.'"**
    - **Plain-English:** Example: level-by-level evidence-citation rubric.
    - **Technical terms:** rubric levels = defined scoring anchors.

78. **"Notable rubric-based benchmarks include ScholarQABench and HealthBench, both of which provide detailed and structured rubrics that score many facets of the targeted task. Each of these rubric items can be scored by a model judge."**
    - **Plain-English:** ScholarQABench and HealthBench use detailed rubrics scored by judges.
    - **Technical terms:** ScholarQABench / HealthBench = rubric-based benchmarks.

79. **"In Figure 7-7, we see an example of using a rubric that scores outputs based on four categories: fluency, correctness, completeness, and groundedness."**
    - **Plain-English:** Four rubric axes: fluency, correctness, completeness, groundedness.
    - **Technical terms:** groundedness = output supported by sources.

80. **Rubric example (ScholarQABench Coverage): 1 Severely lacking (misses core lines of research, fixates on one work); 2 Partial (misses significant research, stays narrow); 3 Acceptable (several representative works, satisfactory overview); 4 Good (variety of representative papers, missing only minor areas); 5 Comprehensive (diverse papers/viewpoints, even surfaces points beyond the question).**
    - **Plain-English:** Five defined levels from "severely lacking" to "comprehensive."
    - **Technical terms:** Likert-style rubric scale (1–5).

81. **"Rubrics improve evaluation reliability because they break a complex judgement into smaller, more objective criteria. They also allow us to evaluate individual sub-behaviors of the agent, which can give us a much more high-resolution signal about where specifically the agent is performing well or breaking down."**
    - **Plain-English:** Rubrics = smaller objective criteria + per-axis failure signals.
    - **Word meanings:** *high-resolution* = fine-grained detail.

## Section 4: Trajectory Evaluation

82. **"Outcome evaluation tells you whether the agent succeeded. Trajectory evaluation tells you how it got there or how it failed. A trajectory is the full sequence of steps an agent took: the reasoning it produced, the tools it called, the arguments it passed, and the intermediate outputs it generated along the way."**
    - **Plain-English:** Trajectory = complete record of reasoning, tool calls, args, intermediate outputs.
    - **Technical terms:** trajectory = step-by-step action sequence.

83. **"One way to analyze an agent's behavior is to look at statistics of its tool calls across a number of runs. In Figure 7-8 from SWE-Atlas, we can see how three different agents behave when tackling the same set of tasks. We can see the GPT-5.4 on the left tends to search and conduct file operations heavily early in its trajectories and undertakes execution steps after gaining enough context about the repo it's operating in. We miss such patterns if we look exclusively at outcomes."**
    - **Plain-English:** Tool-use statistics reveal behavioral patterns invisible in outcome scores.
    - **Technical terms:** tool-use distribution = how tool calls vary over trajectory steps.

84. **"In Figure 7-9, we can see multiple trajectories that pass outcome evaluation, but trajectory inspection reveals deeper problems. In the first example, the agent output was a lucky guess. In the second, the agent spends a large number of steps and tool calls to arrive at an answer that should have only required one tool call. In the third, the model overthinks before making the right tool call."**
    - **Plain-English:** Three correct outcomes, three hidden trajectory problems: lucky guess, inefficiency, overthinking.
    - **Technical terms:** trajectory inspection = analyzing steps, not just results.

85. **"These three failure modes are distinct enough to warrant separate measurement: unsound reasoning, inefficiency, and unnecessary overhead."**
    - **Plain-English:** The three failure modes have names: unsound reasoning, inefficiency, unnecessary overhead.
    - **Technical terms:** unsound reasoning / inefficiency / unnecessary overhead = trajectory failure modes.

86. **"Measuring a trajectory means scoring the steps along the way, not just the endpoint. A few axes do most of the work. Did the agent reach for the right tools and pass them valid arguments? How efficiently did it get there, in tool calls, steps, and reasoning tokens? Did each step follow reasonably from what the agent learned in the steps before it?"**
    - **Plain-English:** Score axes: right tools/args, efficiency (calls/steps/tokens), step-by-step coherence.
    - **Technical terms:** reasoning tokens = thinking tokens spent.

87. **"These questions can be put to an LLM judge that is handed the full trajectory to observe. As we saw earlier, that is a rubric, just one that examines the steps and not just the final output."**
    - **Plain-English:** A trajectory rubric given to an LLM judge scores the steps.
    - **Technical terms:** trajectory rubric = criteria over steps.

88. **"Works such as T-Eval (2024), AgentBoard (2024), TRACE (2025), and AgentProcessBench (2026) formalize this approach, introduce measures for individual steps in the trajectory, and show how this line of evaluation has been tracking over the last several years."**
    - **Plain-English:** Four papers formalize step-level trajectory evaluation.
    - **Technical terms:** T-Eval / AgentBoard / TRACE / AgentProcessBench = trajectory-eval frameworks.

## Section 5: Reliability

89. **"Just like LLMs, agent outputs are stochastic: the same input can produce different outputs across runs, which means a single run score can flatter or unfairly penalize an agent."**
    - **Plain-English:** Stochastic outputs mean single runs can be misleading.
    - **Technical terms:** stochastic = random/inconsistent across runs.

90. **"From these trials you can read two different things: how capable the agent is, meaning whether it can succeed at all, and how reliable it is, meaning whether it succeeds every time. The following two metrics measure each."**
    - **Plain-English:** Capability = can it ever succeed; Reliability = does it always succeed.
    - **Technical terms:** capability vs reliability distinction.

**pass@k**

91. **"Imagine that we're comparing two models on a single problem... So let's sample two solutions for the same problem from each model... We're assuming a temperature value that allows for a certain amount of variance, so say something like 0.7."**
    - **Plain-English:** Sample multiple solutions per problem at temperature ~0.7.
    - **Technical terms:** temperature ≈ 0.7 = sampling variance.

92. **"We can use the two samples to compute a pass@1, which shows us that LLM 1 is better than LLM 2 in this problem. For pass@1, the formula is simply to divide the number of correct answers by the total number of samples. So for LLM 1, that's 2/2 = 1 and for LLM 2 that's 1/2 = 0.5."**
    - **Plain-English:** pass@1 = correct/total samples (2/2 = 1.0 vs 1/2 = 0.5).
    - **Technical terms:** pass@1 = fraction of samples correct.

93. **"Note that using the same data, we can also compute a pass@2 score, which is 1 for both models, since each produced at least one correct sample across its two attempts. But this is exactly where small samples mislead. A pass@k estimate is only trustworthy when the number of samples is much larger than k..."**
    - **Plain-English:** pass@2 = at least one correct in two tries (both = 1.0); small samples mislead; need n >> k.
    - **Technical terms:** pass@k = P(at least one of k passes); sample-size requirement n >> k.

94. **"A more rigorous way would be to run, say, 100 samples, then calculate the pass@k... LLM 1 passes 85 of its 100 samples (pass@1 0.85, pass@2 0.98) and LLM 2 passes 45 (pass@1 0.45, pass@2 0.70)."**
    - **Plain-English:** With 100 samples: LLM 1 (85/100): pass@1 .85, pass@2 .98; LLM 2 (45/100): pass@1 .45, pass@2 .70.
    - **Technical terms:** 100-sample pass@k estimates.

95. **"To calculate pass@k for values of k over 1, we can use the following snippet adapted from Evaluating Large Language Models Trained on Code (2021), which is statistically more robust than the simple division (which works perfectly for pass@1) because it accounts for the dependency between samples when drawing k candidates, while simple division assumes independence and breaks down once k exceeds 1."**
    - **Plain-English:** The combinatorial estimator is robust for k>1; simple division only works for pass@1.
    - **Technical terms:** combinatorial pass@k estimator (Chen et al., 2021).

```python
import numpy as np

def pass_at_k(n_samples, n_correct_samples, k):
    if n_samples - n_correct_samples < k:
        return 1.0
    return 1.0 - np.prod(
        1.0 - k / np.arange(n_samples - n_correct_samples + 1, n_samples + 1)
    )
```

**pass^k**

96. **"The pass@k metric rewards an agent for succeeding at least once in k tries, so it climbs as you grant more attempts. That makes it a measure of capability, not reliability. For reliability, you want the answer to the opposite question: does the agent succeed every single time? That is pass^k (pass-HAT-k, sometimes written pass), the probability that all k of k sampled attempts succeed."**
    - **Plain-English:** pass^k = probability ALL k attempts succeed (reliability); pass@k = at least one (capability).
    - **Technical terms:** pass^k = P(all k pass).

97. **"Figure 7-12 shows pass^3, pass@1, and pass@3 for a number of models on Codebase QnA. Notice how the first and third models, GPT-5.4 and Opus 4.7 have the same pass@1 score of 40. If we stopped the evaluation there, we'd think they're tied at the same level of performance. But by looking at their pass^3 values we can see that GPT-5.4 is more reliable, with a pass^3 of 28 compared to Opus 4.7's score of 23."**
    - **Plain-English:** Equal pass@1 (40) hides different reliability: GPT-5.4 pass^3 = 28 vs Opus 4.7 = 23.
    - **Technical terms:** reliability gap = pass^k vs pass@k spread.

98. **"Paradoxically, the highest pass@3 in the list belongs to Opus 4.7, at 60: given 3 tries, it lands at least one success on more tasks than any other model here."**
    - **Plain-English:** Opus 4.7 has the highest capability ceiling (pass@3 = 60).
    - **Technical terms:** capability ceiling = best-case performance.

99. **"If we were in the middle of a training process and choosing between candidate checkpoints, the higher pass@3 would be the one we select because that capability ceiling indicates we can get more performance from the model with further training. When comparing between API vendors, however, that ceiling is mostly out of reach because we're choosing a model to run as it ships, not to train."**
    - **Plain-English:** During training, pick higher pass@3; when choosing a vendor API, value reliability.
    - **Technical terms:** checkpoint selection vs vendor selection.

100. **"Computing pass^k follows the same combinatorial logic as pass@k, flipped. pass@k estimates the chance that at least one of k samples drawn from your n is correct; pass^k estimates the chance that all of them are. From c correct samples out of n, that is the count of all-correct k-subsets over the count of k-subsets in total, C(c, k) / C(n, k)."**
     - **Plain-English:** pass^k = C(c,k)/C(n,k) — all-correct k-subsets over total k-subsets.
     - **Technical terms:** combinatorial ratio C(c,k)/C(n,k).

```python
from math import comb

def pass_hat_k(n_samples, n_correct_samples, k):
    """Probability that all k randomly drawn samples are correct."""
    if n_correct_samples < k:
        return 0.0
    return comb(n_correct_samples, k) / comb(n_samples, k)
```

101. **"This estimator comes from τ-bench (Yao et al., 2024), which introduced pass^k to measure whether tool-using agents hold up across repeated trials instead of only on their best attempt."**
     - **Plain-English:** τ-bench introduced pass^k for repeated-trial tool-use reliability.
     - **Technical terms:** τ-bench source of pass^k.

## Section 6: Safety

102. **"Agents are more consequential than LLMs. Their actions could be sending a high-impact email, deleting a file, or dropping an entire database. This is why safety evaluation deserves its own place in your eval suite from the start."**
     - **Plain-English:** Agents act in the world; safety evaluation is essential from the start.
     - **Word meanings:** *consequential* = having real-world effects.

103. **"'Safety' covers a few distinct questions, and the cleanest way to separate them is by who, if anyone, is trying to cause the harm. Each is a different threat model with its own metric, so you build test cases for the ones that match how your agent will be used."**
     - **Plain-English:** Separate safety by threat model (who causes the harm).
     - **Technical terms:** threat model = attacker/scenario assumption.

104. **"The first is misuse: a malicious user asks the agent to carry out a harmful task, and the question is whether it refuses. Benchmarks such as AgentHarm measure this with a refusal rate and a harm score."**
     - **Plain-English:** Misuse = malicious user; measure refusal rate + harm score (AgentHarm).
     - **Technical terms:** AgentHarm, refusal rate, harm score.

105. **"The second is manipulation through the data the agent reads. Prompt injection plants instructions in content the agent processes, such as a retrieved document, a tool result, or a web page, to hijack its behavior. Memory poisoning corrupts what the agent stores and later retrieves, so it acts on false premises many steps later. AgentDojo and Agent Security Bench (ASB) measure these and report an attack success rate; ASB covers both surfaces, while AgentDojo focuses on prompt injection and also tracks whether the agent stays on its real task while resisting."**
     - **Plain-English:** Data-manipulation attacks: prompt injection (AgentDojo, ASB) and memory poisoning (ASB); metric = attack success rate.
     - **Technical terms:** prompt injection; memory poisoning; attack success rate; AgentDojo; ASB.

106. **"The third has no adversary at all: on a benign task, the agent harms the user through its own error, deleting the wrong files or taking an irreversible step it should have paused on. This is the least standardized of the three, and it leans on your own test cases, guardrails, and harness choice and design."**
     - **Plain-English:** Self-error harm with no adversary; least standardized; rely on your own tests/guardrails.
     - **Word meanings:** *benign* = harmless-intent.
     - **Technical terms:** own-error safety.

## Section 7: Building Your Own Evals

107. **"The best signal you'll get about how well an agent matches your target task is to build your own evaluation that's actually representative of the specific problem you're building for."**
     - **Plain-English:** Custom evals give the best signal for your specific task.

108. **"A small, well-curated set of test cases that reflect real usage and data will help you pick the best LLM and agent framework for your deployment, as well as help you catch regressions when you update any part of the system."**
     - **Plain-English:** A curated test set supports model/framework choice and regression catching.
     - **Technical terms:** curated test cases = representative examples.

109. **"Aim for cases that capture a wide set of failure modes and edge cases, not just successes."**
     - **Plain-English:** Include failure modes and edge cases, not just happy paths.

110. **"And just like unit tests in the software development life cycle, run and update the test suite automatically on meaningful changes to the system so that improvements are confirmed and breaking changes are caught as early as possible."**
     - **Plain-English:** Automate the eval suite like unit tests (CI for agents).
     - **Technical terms:** automated test suite / regression testing.

111. **"Getting started is more important than getting it immediately right. Even a small custom eval of 5 or 10 cases can give eye-opening signals about your agent's performance–and you can grow it as you learn more about where the system fails."**
     - **Plain-English:** Start small (5–10 cases); grow over time.
     - **Technical terms:** minimum viable eval.

112. **"Running evals at any scale requires an eval harness: the infrastructure that takes a set of test cases, runs the agent against each one, captures the full trajectory, applies your scoring methods, and aggregates the results."**
     - **Plain-English:** Eval harness = infrastructure for running + scoring + aggregating evals.
     - **Technical terms:** eval harness components.

113. **"harbor is a recent open source harness designed specifically for agents, with infrastructure for running trials in containerized environments at scale and a standardized format for defining tasks and their success criteria. Popular benchmarks such as Terminal-Bench ship through the harbor registry, so you can run established benchmarks alongside your own custom eval suite."**
     - **Plain-English:** harbor = open-source agent eval harness (containerized, standardized, registry).
     - **Technical terms:** harbor; containerized trials; harbor registry.

## Section 8: Summary (key lines)

114. **"In this chapter, we examined how evaluation is the foundation of confident agent development and the difference between intuition and evidence."**
     - **Plain-English:** Evaluation = evidence over intuition.

115. **"We started with benchmarks... mapping the major categories from coding and tool use to computer use and real-world task completion. We then outlined the questions worth asking when reading benchmark scores critically, from data contamination and prompt tuning to cost and benchmark saturation."**
     - **Plain-English:** Benchmarks + critical-reading questions recap.

116. **"We then broke down the core methods for outcome evaluation. We saw how human evaluation, despite its cost, remains the gold standard against which automated methods are validated. We covered the automated evaluation spectrum from exact match and programmatic checks, through LLM-as-a-judge, to rubric-based evaluation..."**
     - **Plain-English:** Outcome methods recap: human (gold standard) → exact match → programmatic → LLM-judge → rubrics.

117. **"We contrasted outcome evaluation with trajectory evaluation, which examines the sequence of steps an agent took and can surface failure modes like unsound reasoning, inefficiency, and unnecessary overhead that outcome scores miss entirely."**
     - **Plain-English:** Trajectory eval surfaces failure modes outcomes miss.

118. **"Reliability asks not just whether an agent can succeed but whether it succeeds every time: because outputs are stochastic, running a task repeatedly lets you separate a capability ceiling (pass@k) from a measure of consistency (pass^k), and the two can rank models differently."**
     - **Plain-English:** pass@k (capability) vs pass^k (consistency) can rank models differently.

119. **"Safety is best organized by who is causing the harm: a malicious user the agent should refuse, an attacker steering it through injected or poisoned data, or no adversary at all when the agent errs on a benign task."**
     - **Plain-English:** Safety = three threat models: misuse, data manipulation, own error.

120. **"We closed with building your own evaluations, where a small, well-curated set of representative cases, scored with the methods covered earlier and run automatically on every change, beats waiting for a perfect suite."**
     - **Plain-English:** Start small, score with mixed methods, automate on every change.
