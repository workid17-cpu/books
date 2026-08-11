# AI Agents — Practice Exam (Chapters 5 & 6)
**Source:** *An Illustrated Guide to AI Agents*, Chapters 5 & 6
**Instructions:** Allow ~60–75 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. A regular LLM calling a tool is best described as:
   a) Executing the tool itself
   b) Communicating the intention to call the tool
   c) Silently invoking an API
   d) Training on the tool's output

2. Which is NOT one of the five steps of tool usage?
   a) Tool creation
   b) Tool definition
   c) Tool evaluation
   d) Output processing

3. Tools typically take which parameter types from the LLM?
   a) Floats
   b) JSON objects
   c) Strings
   d) Binary data

4. The best practice for the number of tools is:
   a) As many as possible
   b) Fewer than 10
   c) Exactly 3
   d) At least 50

5. Which of the following is a method of tool definition?
   a) Structured function calling (JSON Schema)
   b) Temperature tuning
   c) Majority voting
   d) Embedding models

6. The `requires_approval` list in Tools implements:
   a) Automated execution
   b) Human-in-the-loop approval
   c) Tool caching
   d) Schema validation

7. Which two packages are suggested for robust tool-call parsing?
   a) numpy and pandas
   b) jsonschema and Pydantic
   c) regex and json
   d) requests and urllib

8. For non-native tool calling, tool output is relayed using:
   a) The `tool` role
   b) The `user` role with an "OBSERVATION" tag
   c) The `system` role
   d) The `assistant` role

9. The `is_done` function stops the agent when:
   a) It calls any tool
   b) There is no tool call or it calls `final_answer`
   c) max_steps is reached
   d) The temperature is 0

10. Which memory type conflicts with Tools?
    a) Conversation Memory
    b) SummarizationMemory
    c) RAGMemory
    d) Episodic memory

11. In-context learning for tools is also known as:
    a) Fine-tuning
    b) Few-shot learning/prompting
    c) Reinforcement learning
    d) Distillation

12. Toolformer embeds the tool call and output using which tokens?
    a) `<think>` and `<answer>`
    b) `[`, `→`, `]`
    c) `{` and `}`
    d) `|>` and `<|`

13. Toolformer's model was:
    a) Qwen2.5
    b) GPT-J
    c) Llama 2
    d) Gemma 4

14. TIR stands for:
    a) Tool-Integrated Reasoning
    b) Test-Integrated Retrieval
    c) Token-Instruction Ranking
    d) Task-Internal Reflection

15. ToolRL's two rewards are:
    a) Accuracy and format
    b) Correctness and format
    c) Helpfulness and harmlessness
    d) Length and format

16. What did ToolRL find about length rewards?
    a) Longer traces always help
    b) Longer traces don't consistently help and may harm small models
    c) Length rewards slow training permanently
    d) Length rewards are the best reward type

17. Search-R1 uses which single tool?
    a) Calculator
    b) Search engine
    c) Code interpreter
    d) Database query

18. Loss masking in Search-R1:
    a) Hides reasoning tokens
    b) Ignores search-engine output tokens during training
    c) Removes format rewards
    d) Masks the query

19. The "magic" of inference engines like Ollama is:
    a) Running models locally for free
    b) Converting standard tool definitions into the model's chat template
    c) Auto-training models
    d) Embedding documents

20. The N×M problem refers to:
    a) N models × M tools needing custom integrations
    b) N tools × M params
    c) N queries × M answers
    d) N tokens × M layers

21. MCP was developed by:
    a) OpenAI
    b) Google
    c) Anthropic
    d) DeepSeek

22. Which is NOT an MCP component?
    a) MCP host
    b) MCP client
    c) MCP server
    d) MCP router

23. The three MCP server primitives are:
    a) tools, resources, prompts
    b) models, data, APIs
    c) actions, inputs, outputs
    d) host, client, server

24. MCP's message structure follows:
    a) REST
    b) JSON-RPC 2.0
    c) gRPC
    d) SOAP

25. A2A standardizes:
    a) Tool integration
    b) Inter-agent communication
    c) Memory storage
    d) Model training

26. Skills provide:
    a) Tool definitions
    b) Procedural knowledge (step-by-step instructions)
    c) Reward models
    d) Vector databases

27. Which layer of progressive disclosure is always loaded?
    a) Layer 1 (metadata)
    b) Layer 2 (instructions)
    c) Layer 3 (bundled resources)
    d) None

28. Every skill requires:
    a) A script file
    b) A SKILL.md file
    c) A JSON schema
    d) An example folder

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

51. Tools allow agents to act on their environment, not just retrieve information. (T/F)
52. The LLM executes the tool call itself. (T/F)
53. Docstrings are unimportant because the LLM can read the code. (T/F)
54. Natural-language tool definitions always require a parser/validator. (T/F)
55. Learning specific tools is cheap and scales well. (T/F)
56. The `tool` role is used only by LLMs trained for native tool calling. (T/F)
57. Regex is fully reliable for extracting tool calls. (T/F)
58. SFT for tool calling generalizes better than RL. (T/F)
59. Toolformer's output is inserted inline as if the model generated it. (T/F)
60. MCP reduces connections from N×M to N+M. (T/F)
61. With MCP, API changes must still be fixed by every user. (T/F)
62. Skill instructions are always loaded into the context. (T/F)
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

76. List the five steps of tool usage and briefly describe each.
77. Why must tools accept strings as parameters, and what are the three best practices for tool definition?
78. Explain the difference between natural-language tool definition and structured function calling.
79. How does human-in-the-loop approval work in the `Tools` class?
80. Explain output processing: the `assistant`, `tool`, and `user`/`OBSERVATION` roles.
81. What is the `final_answer` tool and why is it important?
82. Describe Toolformer's in-line tool calling and its three special tokens.
83. Why does RL generalize better than SFT for tool use?
84. Explain ToolRL's reward design and its finding about length rewards.
85. What is loss masking in Search-R1 and why is it used?
86. Describe the three MCP components and their roles.
87. Walk through the MCP flow for a user query.
88. What is progressive disclosure, and what are the three layers with token costs?
89. Explain the difference between a tool, MCP, and a Skill.
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

101. **The five-step tool-usage lifecycle.** Explain each step (creation, definition, selection, calling, output processing) with the `multiply` example. Include: registry, JSON parsing, OBSERVATION, human-in-the-loop, and why the LLM only expresses intent.

102. **Tool learning methods.** Compare in-context learning, SFT (Toolformer), and RL (ToolRL, Search-R1). Cover the training data challenges of SFT, why RL generalizes better, the specific rewards in ToolRL, and Search-R1's prompt template + loss masking.

103. **MCP and Skills.** Explain the N×M problem, the three MCP components and primitives, the MCP flow, and how MCP solves standardization. Then explain Skills, SKILL.md, and progressive disclosure, and contrast tools/MCP/Skills.

104. **Planning frameworks.** Compare task decomposition approaches: CoT, self-consistency, ToT, Least-to-Most, and Plan-and-Solve. Explain why they are insufficient for agents, motivating action sequencing and ReAct.

105. **ReAct: from prompting to training.** Describe prompt-based ReAct (THOUGHT/ACTION/OBSERVATION, final_answer, max_steps, for-loop autonomy). Then FireAct (SFT) and ETO (SFT + DPO), then native ReAct with Gemma 4. Discuss trade-offs of each.

106. **Reflection techniques.** Compare Self-Refine, Reflexion (Actor/Evaluator/Self-Reflection), and CRITIC. Explain what reflection is and why it prevents local minima, and discuss the limitation of prompt-based techniques.

107. **Self-improvement: TTRL and R-Zero.** Explain test-time RL, TTRL's four steps (sampling, majority vote, reward, GRPO), and "lucky hits." Then R-Zero's Challenger/Solver coevolution with all rewards. Discuss how these represent a shift to test-time training.

---

## ANSWER KEY

### Section A: Multiple Choice
1. **b** — communicates intention.
2. **c** — Tool evaluation is not a step.
3. **c** — strings (converted by the tool).
4. **b** — fewer than 10.
5. **a** — structured function calling.
6. **b** — human-in-the-loop approval.
7. **b** — jsonschema and Pydantic.
8. **b** — user role with OBSERVATION tag.
9. **b** — no tool call or final_answer.
10. **b** — SummarizationMemory overwrites the system prompt.
11. **b** — few-shot learning/prompting.
12. **b** — `[`, `→`, `]`.
13. **b** — GPT-J.
14. **a** — Tool-Integrated Reasoning.
15. **b** — correctness and format.
16. **b** — longer traces don't consistently help; may harm small models.
17. **b** — search engine.
18. **b** — masks search-engine output tokens.
19. **b** — converting standard definitions into the model's chat template.
20. **a** — N models × M tools.
21. **c** — Anthropic.
22. **d** — MCP router is not a component.
23. **a** — tools, resources, prompts.
24. **b** — JSON-RPC 2.0.
25. **b** — inter-agent communication.
26. **b** — procedural knowledge.
27. **a** — Layer 1 metadata.
28. **b** — SKILL.md.
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
51. **T** — action or retrieval.
52. **F** — external systems execute; the LLM only expresses intent.
53. **F** — docstrings are very important (passed to the LLM).
54. **T** — parser/validator needed.
55. **F** — learning specific tools is costly and limits tool count.
56. **T** — native-tool-calling-trained LLMs only.
57. **F** — regex is brittle; use jsonschema/Pydantic.
58. **F** — RL generalizes better than SFT (mimicry problem).
59. **T** — output inserted as if generated.
60. **T** — N+M.
61. **F** — the tool provider fixes it once.
62. **F** — instructions load only when activated (progressive disclosure).
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
76. **Five steps:** Creation (write the function), Definition (tell the LLM via learning or prompting), Selection (LLM picks the right tool), Calling (LLM outputs intent; external system executes), Output processing (feed result back as observation).
77. **Strings:** LLMs output only text, so parameters are strings converted by the tool (e.g., `float(a)`). Best practices: document extensively; minimize the number of tools (<10); minimize tool scope (few params).
78. **Natural language:** tools described in prose in the system prompt; your format; needs a parser; error-prone. **Structured:** JSON Schema per tool (name/description/params) passed as a parameter; standardized and clearer.
79. **Human-in-the-loop:** if the tool is in `requires_approval`, `execute` prompts the user (`Allow? [y/N]`); if denied, returns "Tool 'name' was denied by the user."
80. **Output processing:** `assistant` = the model's tool call; `tool` = native tool output; for non-native models, `user` role with "OBSERVATION: <result>" pretends the user observed the call.
81. **final_answer tool:** the tool the agent calls to return its final answer and stop; the only way for a ReAct agent to complete (else it loops forever).
82. **Toolformer:** in-line tool calling with `[` (start call, select tool + params), `→` (stop generating; system executes), `]` (insert output as if generated). Example: "[Calculator(5.1*7.3) → 37.23]".
83. **RL vs SFT:** SFT mimics fixed examples (wording-sensitive, poor generalization); RL learns via trial-and-error feedback, enabling exploration and generalization.
84. **ToolRL:** GRPO with correctness reward (right tool/params) + format reward (fields in order); trained on 4,000 TIR traces of Qwen2.5. Length rewards didn't consistently help and could harm small models.
85. **Loss masking:** in Search-R1, the search engine's output tokens are masked during GRPO so the model can't try to control tokens it didn't generate.
86. **MCP components:** Host = LLM app (manages connections, interprets schemas, routing); Client = per-server connection code (discovery, forwarding); Server = lightweight program exposing tools/resources/prompts.
87. **MCP flow:** query → host asks server for tools → server lists them → LLM gets prompt+tools → LLM picks a tool → client forwards to server → server executes → output returns via client/host → LLM processes → answer to user.
88. **Progressive disclosure:** Layer 1 metadata always loaded (~100 tokens); Layer 2 instructions on activation (<5,000 tokens); Layer 3 bundled files on demand.
89. **Tool vs MCP vs Skill:** Tool = a callable function; MCP = standardization of tool connections (host/client/server); Skill = procedural knowledge bundle (with tools inside) for a recurring task via SKILL.md.
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
101. Expect: all five steps with `multiply`; registry dict; LLM outputs JSON intent only; parse/execute split; human-in-the-loop; OBSERVATION + final_answer; why the LLM never sees the execution.
102. Expect: in-context (few-shot, fake history); SFT (Toolformer tokens `[ → ]`, data filtering by correctness/output/loss decrease, mimicry limitation, data-quality caveats); RL (TIR, ToolRL rewards + 4,000 traces + length finding, Search-R1 template + accuracy-only reward + loss masking).
103. Expect: N×M → N+M; host/client/server; tools/resources/prompts; JSON-RPC 2.0; the 12-step flow; provider-side maintenance. Skills: procedural knowledge; SKILL.md (YAML metadata + instructions); three-layer progressive disclosure with token budgets; tools vs MCP vs Skills.
104. Expect: CoT = linear decomposition; self-consistency (sampling + vote); ToT (branching/pruning, Beam/MCTS); LtM (decompose + sequential with feedback); plan-and-solve (zero-shot plan then solve); why these are one-shot → need action sequencing.
105. Expect: prompt ReAct (T/A/O, final_answer, max_steps, for-loop); FireAct (GPT-4 trajectories, format conversion, Llama 2 + LoRA, fewer few-shot tokens); ETO (SFT behavior cloning on ALFWorld + DPO exploration/training); native ReAct (NativeReAct, Response.reasoning/tool_call/observation); trade-offs (brittle prompts vs training cost).
106. Expect: reflection definition (internal, against local minima); Self-Refine (own editor); Reflexion (three LLMs, short-term + episodic memory); CRITIC (external tools); limitation = prompts guide but don't instill behavior.
107. Expect: test-time RL rationale (no labels, costly traces); TTRL four steps with numbers + lucky hits + vague rewards; R-Zero Challenger/Solver with all four Challenger signals and binary Solver reward, query filtering; paradigm shift to inference-time training.
