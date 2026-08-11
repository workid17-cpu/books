# AI Agents — Practice Exam (Chapter 6)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 6 "Planning and Reflection"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.
**Note:** Split from the combined Ch 5 & 6 exam. Original question numbers are preserved.

---

## Section A: Multiple Choice (1 point each)

29. The first step in planning is:
    a) Reflection
    b) Task decomposition
    c) Action sequencing
    d) Reward modeling

30. Which technique explicitly separates planning from execution and feeds previous answers forward?
    a) CoT
    b) Self-consistency
    c) Least-to-Most
    d) Tree of Thoughts

31. Plan-and-Solve prompting is best described as:
    a) Few-shot CoT
    b) Zero-shot: plan first, then solve
    c) A tree search method
    d) A reward model

32. Tree of Thoughts differs from CoT by:
    a) Using fewer tokens
    b) Exploring branching paths with pruning
    c) Using a single linear trace
    d) Requiring SFT

33. ReAct stands for:
    a) Reactive Action
    b) Reason and Act
    c) Reasoning Action Tree
    d) Recurrent Activation

34. The three components of a ReAct step are:
    a) Prompt, Model, Output
    b) Thought, Action, Observation
    c) Plan, Execute, Review
    d) Input, Process, Result

35. A ReAct agent finishes when it:
    a) Reaches max_steps
    b) Calls the `final_answer` tool
    c) Runs out of tokens
    d) Gets a high reward

36. The autonomy loop in TinyAgent is:
    a) A while-True recursion
    b) A for-loop
    c) A generator
    d) An async task

37. FireAct fine-tunes which model on ReAct trajectories?
    a) Llama 2
    b) GPT-4
    c) Qwen2.5
    d) Gemma 4

38. ETO trains with:
    a) SFT only
    b) SFT then DPO
    c) GRPO only
    d) PPO only

39. ALFWorld is:
    a) A search engine
    b) A text-based household environment
    c) A reward model
    d) A protocol

40. NativeReAct:
    a) Uses a complex ReAct prompt
    b) Has an empty prompt and no parsing
    c) Only works with Gemma 3
    d) Requires few-shot examples

41. In Reflexion, the scalar reward is produced by:
    a) Actor LLM
    b) Evaluator LLM
    c) Self-Reflection LLM
    d) The environment

42. Self-Refine is best described as:
    a) Using a second LLM to improve output
    b) The LLM acting as its own editor (generate → critique → refine)
    c) Training on self-generated data
    d) Tool-based revision only

43. TTRL stands for:
    a) Test-Time Reinforcement Learning
    b) Token-Time Reward Learning
    c) Task-Transfer Reasoning Loop
    d) Test-Time Response Labeling

44. In TTRL, how many outputs are sampled per query?
    a) 4
    b) 8
    c) 16
    d) 32

45. TTRL selects the best answer via:
    a) A reward model
    b) Majority voting
    c) Tree search
    d) Human feedback

46. In R-Zero, the model that generates hard queries is the:
    a) Solver
    b) Challenger
    c) Evaluator
    d) Actor

47. The Challenger's composite reward is:
    a) uncertainty + repetition penalty
    b) uncertainty − repetition penalty (min 0)
    c) format × uncertainty
    d) repetition penalty only

48. The Solver in R-Zero receives a reward that is:
    a) Continuous (0–1)
    b) Binary (0/1)
    c) Negative only
    d) None

49. A "lucky hit" in TTRL is:
    a) A correct reward from an incorrect process
    b) A model finding the right tool
    c) A format reward
    d) A local minimum

50. The evolution described at the end of Ch 6 moves training toward:
    a) More pre-training data
    b) Unlabeled data and inference-time learning
    c) Larger models only
    d) Pure prompting

---

## Section B: True/False (1 point each)

63. ReAct interleaves reasoning and acting. (T/F)
64. ReAct agents can run for hundreds of cycles without needing memory. (T/F)
65. FireAct requires few-shot examples at inference time. (T/F)
66. ETO uses failed and successful trajectory pairs for training. (T/F)
67. Prompt-based ReAct is brittle and uses system-prompt space. (T/F)
68. Reflection is only ever an external process. (T/F)
69. Self-Refine uses three different LLMs. (T/F)
70. TTRL needs supervised labels for its rewards. (T/F)
71. In TTRL, majority voting is guaranteed to be correct. (T/F)
72. The Solver in R-Zero is frozen while the Challenger trains. (T/F)
73. R-Zero filters out both too-easy and too-difficult queries. (T/F)
74. Tools are only useful for single-turn processes. (T/F)
75. ReAct is the final missing link for potential agent autonomy. (T/F)

---

## Section C: Short Answer (2–3 points each)

90. What is task decomposition, and give an example from the chapter?
91. Compare CoT, self-consistency, and Tree of Thoughts.
92. Contrast Least-to-Most and Plan-and-Solve prompting.
93. Explain action sequencing and why it requires more than prompting.
94. Describe the ReAct loop (Thought/Action/Observation) and how it ends.
95. What does FireAct do and what was its key result?
96. Explain ETO's two phases (SFT + exploration/DPO).
97. What is native ReAct, and how does NativeReAct simplify the implementation?
98. Contrast Self-Refine and Reflexion.
99. Explain the four steps of TTRL.
100. Explain the Challenger/Solver coevolution in R-Zero and the reward asymmetry.

---

## Section D: Essay / Applied (5 points each)

104. **Planning frameworks.** Compare task decomposition approaches: CoT, self-consistency, ToT, Least-to-Most, and Plan-and-Solve. Explain why they are insufficient for agents, motivating action sequencing and ReAct.

105. **ReAct: from prompting to training.** Describe prompt-based ReAct (THOUGHT/ACTION/OBSERVATION, final_answer, max_steps, for-loop autonomy). Then FireAct (SFT) and ETO (SFT + DPO), then native ReAct with Gemma 4. Discuss trade-offs of each.

106. **Reflection techniques.** Compare Self-Refine, Reflexion (Actor/Evaluator/Self-Reflection), and CRITIC. Explain what reflection is and why it prevents local minima, and discuss the limitation of prompt-based techniques.

107. **Self-improvement: TTRL and R-Zero.** Explain test-time RL, TTRL's four steps (sampling, majority vote, reward, GRPO), and "lucky hits." Then R-Zero's Challenger/Solver coevolution with all rewards. Discuss how these represent a shift to test-time training.

---

## ANSWER KEY

### Section A: Multiple Choice
29. **b** — task decomposition.
30. **c** — Least-to-Most.
31. **b** — zero-shot plan-then-solve.
32. **b** — branching paths with pruning.
33. **b** — Reason and Act.
34. **b** — Thought, Action, Observation.
35. **b** — calls final_answer.
36. **b** — a for-loop.
37. **a** — Llama 2.
38. **b** — SFT then DPO.
39. **b** — text-based household environment.
40. **b** — empty prompt, no parsing.
41. **b** — Evaluator LLM.
42. **b** — LLM as its own editor.
43. **a** — Test-Time Reinforcement Learning.
44. **c** — 16.
45. **b** — majority voting.
46. **b** — Challenger.
47. **b** — uncertainty − repetition penalty (min 0).
48. **b** — binary 0/1.
49. **a** — correct reward from an incorrect process.
50. **b** — unlabeled data and inference-time learning.

### Section B: True/False
63. **T** — reasoning + acting interleaved.
64. **F** — memory (and context engineering) becomes vital for long runs.
65. **F** — fine-tuned ReAct removes the need for few-shot examples.
66. **T** — failed + successful pairs.
67. **T** — brittle and takes system-prompt space.
68. **F** — reflection is an internal process (can combine with external feedback).
69. **F** — Self-Refine uses ONE LLM; Reflexion uses three.
70. **F** — TTRL produces its own rewards (no labels).
71. **F** — majority voting can be wrong (e.g., "lucky hits").
72. **T** — Solver frozen during Challenger training.
73. **T** — filters too-easy and too-hard queries.
74. **F** — ReAct turns single-turn tools into multi-turn processes.
75. **T** — ReAct decides when to stop/continue.

### Section C: Short Answer (model answers)
90. **Task decomposition:** splitting a query into subtasks; e.g., create a feature → clarify requirements, analyze codebase, design, implement, test, update docs.
91. **CoT:** single linear trace. **Self-consistency:** several traces, majority-vote the answer. **ToT:** branching paths, pruning dead ends (Beam/MCTS with reward models).
92. **LtM:** two stages — decompose into subproblems, then solve sequentially appending prior Q&A (few-shot). **Plan-and-Solve:** zero-shot — make a plan then solve in one call; second call extracts the answer.
93. **Action sequencing:** after setting subgoals, the agent chooses/orders actions to move from current to goal state, interleaving plan and action and updating the plan — beyond one-shot decomposition.
94. **ReAct loop:** repeat Thought (reason) → Action (tool call) → Observation (result) until the agent calls `final_answer`; `max_steps` prevents infinite loops.
95. **FireAct:** fine-tunes Llama 2 on GPT-4-generated ReAct trajectories; result — outperforms prompt-based ReAct and removes few-shot needs.
96. **ETO:** Phase 1 SFT on successful ReAct trajectories (behavior cloning); Phase 2 alternates exploration (sample failed trajectories) and training with DPO on failed/success pairs to prefer successful ones.
97. **Native ReAct:** the model was trained to reason/call tools/stop, so NativeReAct has an empty prompt and passthrough parse — only `max_steps` remains.
98. **Self-Refine:** one LLM iteratively critiques and refines its own output. **Reflexion:** three LLMs — Actor (executes), Evaluator (scalar reward), Self-Reflection (nuanced feedback); loop until Evaluator says correct.
99. **TTRL:** (1) sample 16 outputs (temp 0.6); (2) majority-vote the best; (3) generate a reward from alignment between vote and outputs; (4) update via GRPO.
100. **R-Zero:** Challenger generates hard queries (trained via GRPO with uncertainty − repetition penalty composite + format reward); Solver answers via majority voting, is frozen during Challenger training, then fine-tuned on filtered "just right" queries with a binary reward. Asymmetry: Challenger explores difficulty, Solver must be precise.

### Section D: Essay (grading notes)
104. Expect: CoT = linear decomposition; self-consistency (sampling + vote); ToT (branching/pruning, Beam/MCTS); LtM (decompose + sequential with feedback); plan-and-solve (zero-shot plan then solve); why these are one-shot → need action sequencing.
105. Expect: prompt ReAct (T/A/O, final_answer, max_steps, for-loop); FireAct (GPT-4 trajectories, format conversion, Llama 2 + LoRA, fewer few-shot tokens); ETO (SFT behavior cloning on ALFWorld + DPO exploration/training); native ReAct (NativeReAct, Response.reasoning/tool_call/observation); trade-offs (brittle prompts vs training cost).
106. Expect: reflection definition (internal, against local minima); Self-Refine (own editor); Reflexion (three LLMs, short-term + episodic memory); CRITIC (external tools); limitation = prompts guide but don't instill behavior.
107. Expect: test-time RL rationale (no labels, costly traces); TTRL four steps with numbers + lucky hits + vague rewards; R-Zero Challenger/Solver with all four Challenger signals and binary Solver reward, query filtering; paradigm shift to inference-time training.

---

## SCORING GUIDE
- Section A: 22 questions × 1 point = 22 points
- Section B: 13 questions × 1 point = 13 points
- Section C: 11 questions × 2–3 points = 22–33 points
- Section D: 4 essays × 5 points = 20 points
- **Total: 77–88 points.** Scale to percentage as needed.
