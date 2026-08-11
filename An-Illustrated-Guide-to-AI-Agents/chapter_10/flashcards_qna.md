# Flashcards & Q&A — Chapter 10
**Source:** *An Illustrated Guide to AI Agents*, Chapter 10 "Code Agents and Code LLMs"
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.

## Part 1: Term → Definition

1. **Why are coding agents among the most potent types of AI agents?** → Huge likely impact: code generation (GPT-3, 2020) was an early LLM app; much knowledge work is expressible as code, and code can be verified automatically.
2. **The two reasons code is a key LLM investment area** → (1) A wide range of knowledge work can be expressed as code; (2) code can be verified automatically by running/testing it.
3. **Why is verifiability important?** → It makes coding more tractable than open-ended tasks with no objective check.
4. **The three groups of users of coding agents** → Non-code output seekers (charts/analysis, may never see code), vibe coders (build via natural language), software engineers (read/review/direct in the codebase).
5. **The two groups of builders** → Those who build the agents (tools, context, loop) and those who train the coding LLMs.
6. **Vibe coder** → Someone who wants working software but builds it through natural language, letting the agent handle the details.
7. **The three-step evolution from code generation to code agents (Figure 10-2)** → (1) Ask LLM to write code in a playground; (2) LLM generates, executes, and presents results; (3) agent solves large multi-step software tasks with a richer environment.
8. **Data retrieval: three cases for a plotting agent** → (A) User hands the file → Python sandbox with pandas/polars; (B) Agent finds files itself → command-line tools on a VM; (C) Data in a database → SQL tool.
9. **How does the ReAct loop handle execution errors?** → The error is just another observation returned to the LLM, which reasons about it and tries a corrected action.
10. **Code tool** → A tool that carries out the model's chosen action against a real environment (run code, shell command, database query).
11. **File manipulation tools** → read file (cat), read a section (sed, head, tail), list files (ls), pattern-match filenames (glob), search contents (grep).
12. **Why read only a section of a file?** → To better manage model context when dealing with long files.
13. **Code interpreter / sandbox** → A code execution environment more restricted than a full VM.
14. **Why are sandboxes restricted?** → LLM-generated code is not guaranteed safe — it could delete files, corrupt systems, or share private data (attacks/prompt injection or naive actions).
15. **Two properties of ephemeral sandboxes** → (1) Fresh environment per user/session (predictable state, user isolation); (2) data deleted at end of session (no persistence).
16. **The convenience-vs-security trade-off** → Every capability added (commands, internet) widens what a mistake or attack can do.
17. **Hosted sandbox service examples** → Modal, Daytona, E2B, Together Code Sandbox.
18. **Self-hosted interpreter options** → Open Interpreter or the e2b repo (isolated container you control).
19. **Why choose a self-hosted sandbox?** → Specific security or networking requirements that hosted services don't meet.
20. **RedCode (2024)** → Risky Code Execution and Generation Benchmark for Code Agents; probes dangerous behaviors (file deletion, privilege escalation, network misuse).
21. **SandboxEval (2025)** → Work securing test environments for untrusted code; three threat categories: exposing system/directory/metadata info; manipulating filesystem structures/contents/privileges; initiating external communication and dangerous operations.
22. **"Your Agent, Their Asset" (OpenClaw, 2026)** → First real-world safety evaluation of a personal agent; poisoning any single dimension of persistent state (capabilities, identity, knowledge) raises attack success rates from ~25% to 64% to 74%; exposure is inherent in the architecture.
23. **Why do IDE software agents ask for command approval?** → The developer is the ultimate decision-maker; but it offers little protection to non-technical users who can't judge commands.
24. **Principle of least privilege** → Grant the agent only the permissions and data access its task requires, and scope tasks so a wrong action stays contained.
25. **Computer use tools** → Interact with the OS UI and feed screenshots to the vision-language model (e.g., operating Excel, verifying UI rendering).
26. **ast-grep** → Pattern matching on the code's abstract syntax tree (AST) instead of raw text — ignores formatting, variable names, whitespace.
27. **Code semantic search** → Searching code via dense embeddings from a code-embedding model (e.g., "find a snippet that iterates over a list").
28. **CoIR** → Code Information Retrieval Benchmark with a leaderboard comparing code-retrieval models.
29. **SQL tool** → Executes agent-written SQL against one or more databases; can require multiple queries building intermediate results.
30. **Schema linking** → Relating user queries to the database's tables and columns.
31. **Semantic layer (SQL agents)** → Identifies relevant columns, indexes relevant data, and captures user language (e.g., "outerwear" maps to jackets/blazers/coats).
32. **"Arming Data Agents with Tribal Knowledge" (2026)** → Augments SQL agents with institutional knowledge learned from the agent's own query mistakes instead of a hand-built semantic layer.
33. **kv-caching / prompt-caching / prefix-caching** → Reusing cached computations over the input prefix to cut latency and cost.
34. **Why does caching pay off for code agents?** → Much context (system prompts, security behaviors, repository description) is static and repeats every call.
35. **Two requirements for the cache to work** → (1) Cached input must remain identical (no single token change); (2) all new input must be appended to the end.
36. **How to maximize cache hit rate** → Let the cache grow with the trajectory so the whole prior context is served from cache and only new output is computed.
37. **Repository map** → File/directory structure + additional info about relevant files, essential context for SE agents.
38. **Gitingest** → Tool that takes a GitHub repo URL and generates a repo map (directory list + short file contents).
39. **Aider's repo-map approach** → Use the AST to identify important parts (class/function signatures) as summaries, then rank/filter to only the most relevant definitions.
40. **CodeMonkeys (2025)** → Uses an LLM to read every file in the repo; cost amortized across many sampled solutions (~15% of total budget), then files are ranked and trimmed.
41. **Context compaction** → Summarizing long-conversation context (short-term memory) to continue; for code agents includes environment, code style preferences, test/commit preferences.
42. **Static/stable/dynamic trajectory sections** → Static stays put; stable shifts occasionally; dynamic turns over each step; compaction shrinks the dynamic section while static/stable stay cached.
43. **Why did early SE agents need close guidance?** → The multi-step software task wasn't well represented in their training data; they compensated with heavy external scaffolding.
44. **SWE-agent's Agent-Computer Interface** → A constrained set of commands making a repository navigable for a model that couldn't reliably navigate it alone.
45. **SWE-bench** → First widely adopted code-agent benchmark: hundreds of real GitHub issues + repo snapshots; the fix must pass the repo's own test suites.
46. **Why did SWE-bench catch on?** → It measures something real (actual issues, objective test-based grading) and early systems left plenty of headroom.
47. **Planning for SE agents** → Write a detailed concrete plan and keep returning to update status/plan as the agent learns; often worth assigning the best model and enough time.
48. **Planning prompt caveat** → Software tasks need exploration/investigation phases or user clarification; don't assume the approach is clear at planning time.
49. **Static workflow vs agent** → Static workflows run fixed pre-defined steps; for routine tasks an agent is overkill — pick the tool that fits.
50. **Agentless** → Three-phase software-engineering approach: localize relevant code, generate several candidate patches (repair), pick the best using generated unit tests.
51. **Why does Agentless sample multiple solutions?** → Multiple attempts improve the chance of resolving the bug (probabilistic nature of LLMs).
52. **Generated unit tests as a ranking signal** → The selected patch doesn't need to pass all generated tests — just more than the other candidates.
53. **The two toolkit ideas from Agentless** → (1) Sample multiple solutions (temperature allowing variety); (2) use generated unit tests as a ranking signal, not a pass/fail signal.
54. **The two most common TinyAgent coding tool categories** → File manipulation tools (read, list, write files) and a code interpreter (execute code).
55. **Why do tools return strings?** → So their output can be used in the messages structure as an observation indicating success/failure.
56. **requires_approval parameter** → Gates dangerous tools (write_file, execute_python) behind a user "Y"/"N" check before execution.
57. **The four TinyAgent coding tools** → read_file, list_files, write_file, execute_python.
58. **execute_python implementation** → Runs `[sys.executable, "-c", code]` via subprocess with a 30s timeout, returning stdout/stderr or exit code.
59. **Display class** → Color-codes ReAct loop phases with ANSI escape codes: THOUGHT (green), ACTION (red), OBSERVATION (yellow), ANSWER (purple).
60. **How is Display integrated into TinyAgent?** → Add self.display(...) calls at the tracked points (thinking, response, tool_call, observation).
61. **North Mini Code** → Cohere's open mixture-of-experts coding model: 30B parameters total, 3B active per token, Apache 2.0.
62. **The three training stages of a coding LLM** → Pre-training (base model), SFT (instruction-tuned), RL (agentic behaviors).
63. **Qwen3 Coder data composition** → Pre-trained on 7.5 trillion tokens, 70% of which is code.
64. **The four types of coding task data** → Tool-use data, multi-step data, unit-test data, software engineering tasks (repo-level issues + diffs).
65. **RLVR** → Reinforcement Learning with Verifiable Rewards — the main RL approach for coding LLMs (vs RLHF).
66. **Why do coding LLMs use RLVR?** → Verifiable rewards turn "code can be checked by running it" into a training signal.
67. **Other capabilities needed beyond code data** → Reasoning, multi-step tool calling, long-context, planning, and SE-practice trajectories.
68. **GLM-5 pipeline** → ~28 trillion pre-training tokens; mid-training extends context to 200K with long code + agent data; SFT; RL split into reasoning/agentic/general; final distillation.
69. **Synthetic data feedback loop** → Since ~2024 LLMs generate training data for the next generation (Llama 3 → 3.1, Qwen2.5 → Qwen3, DeepSeek-R1-Zero → R1).
70. **Open-source synthetic-data examples** → Nvidia Nemotron 3/2 Nano, AI21 code-guided synthetic multimodal data, OmniSQL (text-to-SQL data at scale).

## Part 2: Short Answer

71. **Why is verifiability what makes code more tractable than open-ended tasks?** → Code can be run/tested to get a clear pass/fail signal, enabling objective evaluation and RL with verifiable rewards — unlike open-ended tasks with no objective check.
72. **Walk through the non-coder data-analysis flow (make a plot).** → Data retrieval (file/pandas-polars, command-line filesystem search, or SQL), then write code to plot and return the plot, then troubleshoot — feeding execution errors back to the LLM as observations (ReAct).
73. **Explain the security trade-offs of sandboxes and why they're ephemeral.** → LLM code is not guaranteed safe; sandboxes restrict resources/commands/internet (each capability widens blast radius). Ephemeral = fresh per user/session (predictable state, isolation) and data deleted at end (no persistence).
74. **What does the OpenClaw (2026) study conclude about persistent state?** → Poisoning any single dimension of persistent state (capabilities, identity, knowledge) raises attack success rates from ~25% to 64% to 74%; the exposure is inherent in the architecture, not a fixable bug.
75. **Why is per-command approval limited as a safeguard?** → It works for expert developers who can judge each command, but offers little protection to non-technical users who can't evaluate what a command will do.
76. **What is the principle of least privilege and when is it applied?** → Grant only the permissions/data the task requires and scope tasks so a wrong action stays contained; it's a builder choice applied at design time, not something the agent decides.
77. **How does ast-grep differ from grep?** → ast-grep matches on the abstract syntax tree (code structure), ignoring formatting, variable names, and whitespace.
78. **Describe the two requirements for prompt caching to work.** → (1) The cached input must remain identical — can't change a single token; (2) new input must be appended to the end of the cached input.
79. **How does CodeMonkeys amortize the cost of reading every file?** → It samples many candidate solutions per issue, so having an LLM read every file once spreads that one-time cost across all downstream samples (~15% of total budget); files are then ranked and trimmed.
80. **What is a repository map and what tools/methods generate one?** → File/directory structure + file summaries. Gitingest (from GitHub URL); Aider's AST-signature summaries with ranking/filtering; CodeMonkeys' LLM summaries.
81. **How does SWE-bench grade a fix?** → Each example = a real GitHub issue + repository snapshot; the model must resolve it so the repository's own hidden test suites pass — objective, not subjective.
82. **Explain the Agentless three-phase process.** → (1) Localize — search the repo for snippets relevant to the issue (RAG-like); (2) repair — a coding LLM generates candidate patches (sampling multiple); (3) rank — generate unit tests and pick the candidate passing more tests than the others.
83. **Why use generated unit tests as a ranking signal instead of pass/fail?** → Generated tests' quality isn't fully trusted, so the chosen solution just needs to pass more tests than the other candidates, not all of them.
84. **What is the Display class and how does it visualize the loop?** → It color-codes each ReAct phase using ANSI escape codes — THOUGHT (green), ACTION (red), OBSERVATION (yellow), ANSWER (purple) — making the loop scannable in the terminal.
85. **Which TinyAgent tools require approval and why?** → write_file and execute_python — they're not safe by default (could corrupt files or take dangerous actions); requires_approval prompts Y/N before execution.
86. **What data does a coding LLM need that a generalist doesn't?** → Far more code data (e.g., 70% of Qwen3 Coder's 7.5T tokens) plus tool-use, multi-step, unit-test, and software-engineering (issue+diff) data, plus reasoning, long-context, and planning capabilities.
87. **Contrast RLHF and RLVR for coding LLMs.** → RLHF polishes behavior from human feedback; RLVR uses large-scale reinforcement learning with verifiable rewards — code can be automatically checked, so reward is objective.
88. **Describe GLM-5's training pipeline.** → Pre-training (~28T tokens); mid-training extending context to 200K with long code and agent data; SFT; RL split into reasoning, agentic, and general stages; final distillation step.
89. **Explain the synthetic data feedback loop.** → Since ~2024, LLMs generate much of the next generation's training data (e.g., Llama 3.1 post-training generated with Llama 3; Qwen2.5-Math/Coder → Qwen3; DeepSeek-R1-Zero traces → R1).
90. **Name three open-source synthetic-data projects.** → Nvidia Nemotron 3 Nano / Nemotron 2 Nano; AI21 code-guided synthetic multimodal data generation; OmniSQL (high-quality Text-to-SQL data at scale).

## Part 3: Fill-in-the-Blank

91. "Code can often be ______ automatically: you can run it or test it and get a clear signal of whether it worked." → verified
92. "Users range from those who want a non-code output, to ______, to software engineers." → vibe coders
93. "In the second evolution example, the LLM ______ the code in an execution environment and presents the results to the user." → executes
94. "A ______ is a code execution environment more restricted than a full virtual machine." → code interpreter / sandbox
95. "Sandboxes are often ______ so a fresh environment is created for each user or session." → ephemeral
96. "Hosted sandbox services include ______, ______, ______, and Together Code Sandbox." → Modal; Daytona; E2B
97. "______ allows the agent to search file contents; ______ pattern-matches file names." → grep; glob
98. "Follow the principle of ______ by granting the agent only the permissions and data access its task requires." → least privilege
99. "ast-grep operates on the code's ______ rather than raw text." → abstract syntax tree (AST)
100. "______ is the Code Information Retrieval Benchmark." → CoIR
101. "The 'show me all outerwear products' example illustrates the need for a ______ layer in SQL agents." → semantic
102. "For the cache to work, the cached input must remain ______, and new input must be ______ to the end." → identical; appended
103. "______ generates a repo map from a GitHub repo URL." → Gitingest
104. "Aider summarizes files via ______ and ______ signatures, then ranks and filters." → class; function
105. "The original SWE-agent introduced a purpose-built ______." → Agent-Computer Interface
106. "SWE-bench grades a fix by passing the repository's own ______." → test suites
107. "Agentless's three phases are ______, ______, and ______." → localize; repair; (rank with) generated unit tests
108. "The selected Agentless solution only needs to pass ______ tests than the other candidates." → more
109. "The Display class uses ______ escape codes to color the output." → ANSI
110. "The ______ parameter gates tools like write_file and execute_python behind user approval." → requires_approval
111. "North Mini Code has ______ billion parameters total but only ______ billion active per token." → 30; 3
112. "Qwen3 Coder is pre-trained on 7.5 trillion tokens, ______% of which is code." → 70
113. "Coding LLMs are trained with ______ (Reinforcement Learning with Verifiable Rewards)." → RLVR
114. "GLM-5's mid-training extends context out to ______ tokens." → 200K
115. "Around ______, LLMs became a major source of training data for the next generation of LLMs." → 2024
