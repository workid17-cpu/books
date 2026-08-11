# AI Agents — Practice Exam (Chapter 10)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 10 "Code Agents and Code LLMs"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. Why is code a key investment area for LLMs?
   a) Code is easy to write by hand
   b) Code uses fewer tokens than text
   c) Code requires no evaluation
   d) Much knowledge work is expressible as code, and code can be verified automatically

2. Which user group wants a non-code output (like a chart) and may never see the code?
   a) Non-code output seekers
   b) Vibe coders
   c) Software engineers
   d) Data engineers

3. In the second evolution example (Figure 10-2), the LLM:
   a) Only writes code in a playground
   b) Generates code, executes it, and presents the results
   c) Reviews the developer's code
   d) Trains a new model

4. Which tool category handles a spreadsheet handed directly to the agent?
   a) Command-line tools on a VM
   b) SQL tools
   c) A Python sandbox with pandas/polars
   d) ast-grep

5. In the ReAct loop, an execution error is treated as:
   a) Just another observation the model reasons about
   b) A fatal failure that stops the agent
   c) A signal to delete the environment
   d) A user approval request

6. Which is a file-manipulation tool the book lists?
   a) grep for pattern-matching filenames
   b) glob for searching file contents
   c) sed for listing directories
   d) cat for reading an entire file

7. Why are code interpreter sandboxes restricted?
   a) To save on compute cost
   b) Because LLM-generated code is not guaranteed to be safe
   c) Because sandboxes are too slow otherwise
   d) To prevent user access

8. Two properties of ephemeral sandboxes:
   a) Persistent storage and shared environments
   b) Internet access and unlimited resources
   c) Fresh environment per session and data deleted at the end
   d) Full VM access and root privileges

9. Which is a hosted sandbox service mentioned in the book?
   a) E2B
   b) Cursor
   c) Gitingest
   d) ast-grep

10. The RedCode benchmark (2024) probes agents for:
    a) Code style violations
    b) Test coverage gaps
    c) Poor documentation
    d) Dangerous behaviors like file deletion and privilege escalation

11. The "Your Agent, Their Asset" (2026) study found poisoning persistent state raises attack success rates to roughly:
    a) 25% to 30%
    b) 64% to 74%
    c) 5% to 10%
    d) 90% to 100%

12. The principle of least privilege means:
    a) Grant the agent all permissions to avoid getting stuck
    b) Let the agent decide its own permissions
    c) Grant only the permissions and data access the task requires
    d) Use the smallest possible model

13. Computer use tools feed ______ to the vision-language model:
    a) Screenshots of the OS UI
    b) SQL schemas
    c) AST trees
    d) Base64 images only

14. ast-grep differs from grep because it operates on:
    a) Raw text lines
    b) Binary files
    c) Regular expressions only
    d) The code's abstract syntax tree

15. CoIR is a benchmark for:
    a) Code generation
    b) Code retrieval
    c) Code execution
    d) Unit testing

16. The "outerwear products" example illustrates the need for:
    a) A code interpreter
    b) A repository map
    c) A semantic layer in SQL agents
    d) Context compaction

17. Two requirements for prompt caching to work:
    a) Identical cached input and new input appended to the end
    b) New input inserted anywhere and shorter prefixes
    c) Changing one token per step and rotating the prefix
    d) Separate caches per step

18. Which tool generates a repository map from a GitHub repo URL?
    a) Aider
    b) CodeMonkeys
    c) ast-grep
    d) Gitingest

19. Aider summarizes files using:
    a) Full file contents
    b) AST-derived class and function signatures
    c) Random sampling
    d) Line counts

20. Context compaction for code agents may include:
    a) Environment info, code style preferences, test/commit preferences
    b) Only the final answer
    c) The full trajectory verbatim
    d) The LLM's weights

21. Why did early SE agents need heavy external scaffolding?
    a) The multi-step software task wasn't well represented in their training data
    b) The models were too large to run
    c) Repositories were too small
    d) Test suites were unavailable

22. The original SWE-agent introduced:
    a) A new benchmark
    b) The Display class
    c) Semantic search
    d) A purpose-built Agent-Computer Interface

23. SWE-bench is:
    a) A benchmark of LeetCode-style problems
    b) A collection of hundreds of real GitHub issues with repository snapshots and test suites
    c) A leaderboard for SQL agents
    d) A synthetic data generator

24. Why did SWE-bench catch on?
    a) It's easy to game
    b) It uses subjective grading
    c) It measures something real, graded objectively by running tests, with early headroom
    d) It only tests autocompletion

25. In software engineering agents, planning:
    a) Is unnecessary for small tasks
    b) Never changes after the first plan
    c) Is always done by a separate agent
    d) Requires returning to update the plan as the agent learns more

26. The planning prompt caveat is that software tasks:
    a) Often require exploration/investigation phases or user clarification
    b) Always have crystal-clear solutions at planning time
    c) Never need flexibility
    d) Only need planning after execution

27. Agentless's three phases are:
    a) Plan, execute, reflect
    b) Search, summarize, present
    c) Localize, repair, pick best using generated unit tests
    d) Write, run, debug

28. In Agentless, the selected solution needs to:
    a) Pass all generated tests
    b) Pass more tests than the other candidates
    c) Pass exactly half the tests
    d) Fail the unit tests

29. The two toolkit ideas from Agentless:
    a) Sample multiple solutions and use generated tests as a ranking signal
    b) Always use pass/fail grading and single samples
    c) Rely on the largest model and full context
    d) Skip localization and repair

30. The two most common TinyAgent coding tool categories are:
    a) SQL tools and computer use tools
    b) ast-grep and semantic search
    c) Display and CLI
    d) File manipulation tools and a code interpreter

31. Which TinyAgent tools require approval by default?
    a) read_file and list_files
    b) execute_python only
    c) write_file and execute_python
    d) all four tools

32. The execute_python tool uses subprocess with a timeout of:
    a) 5 seconds
    b) 30 seconds
    c) 60 seconds
    d) 300 seconds

33. In the Display class, the ANSWER phase is colored:
    a) Purple
    b) Green
    c) Red
    d) Yellow

34. North Mini Code has:
    a) 4B parameters total
    b) 70B parameters total
    c) 30B parameters total, 3B active per token
    d) 3B parameters total, 30B active per token

35. The three training stages of a coding LLM are:
    a) Tokenization, embedding, decoding
    b) Data collection, validation, deployment
    c) Encoding, connecting, generating
    d) Pre-training, SFT, RL

36. Qwen3 Coder is pre-trained on 7.5 trillion tokens, of which ______ is code:
    a) 30%
    b) 70%
    c) 50%
    d) 90%

37. Which is NOT one of the four types of coding task data?
    a) Tool-use data
    b) Multi-step data
    c) Unit-test data
    d) Image-caption data

38. Coding LLMs are trained with:
    a) Pure SFT
    b) RLVR (verifiable rewards)
    c) RLHF only
    d) No reinforcement learning

39. GLM-5's mid-training extends context out to:
    a) 32K tokens
    b) 128K tokens
    c) 200K tokens
    d) 1M tokens

40. The synthetic data feedback loop means:
    a) LLMs only use human-written data
    b) Training data is deleted after use
    c) LLMs generate much of the next generation's training data
    d) Data quality is never verified

---

## Section B: True/False (1 point each)

41. Code can be verified automatically by running it, which makes it more tractable than open-ended tasks. (T/F)
42. Vibe coders want to read and review every line of generated code. (T/F)
43. In the second evolution example, executing code opens coding agents to a massive non-developer audience. (T/F)
44. Sandboxes are usually persistent so agents can reuse environments across sessions. (T/F)
45. Every sandbox restriction trades convenience for security. (T/F)
46. ast-grep matches patterns on the abstract syntax tree, ignoring formatting and whitespace. (T/F)
47. For prompt caching to work, the cached input must stay identical and new input must be appended to the end. (T/F)
48. The repository map is the entire codebase loaded into context in full. (T/F)
49. SWE-bench grades fixes subjectively by human review. (T/F)
50. Agentless always relies on a full agentic loop with tool use. (T/F)
51. Generated unit tests are used in Agentless as a ranking signal, not a strict pass/fail. (T/F)
52. The Display class uses ANSI escape codes to color each ReAct phase. (T/F)
53. The requires_approval parameter lets the user answer Y/N before dangerous tools execute. (T/F)
54. Coding LLMs only need code data — reasoning and long-context are irrelevant. (T/F)
55. Around 2024, LLMs became a major source of training data for the next generation of LLMs. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Why is verifiability the key property that makes code more tractable than open-ended tasks for LLMs?
57. Name the three user groups and two builder groups of code agents.
58. Describe the three data-retrieval cases for a non-coder plotting agent (file, filesystem search, database).
59. Explain the two properties of ephemeral sandboxes and why each matters for security.
60. What did the OpenClaw (2026) study find about poisoning persistent state, and why is the exposure "inherent in the architecture"?
61. How does the principle of least privilege mitigate command-line risks, and why is per-command approval insufficient for non-experts?
62. Explain the two requirements for prompt caching to work and how to maximize the cache hit rate.
63. What is a repository map, and describe two methods used to generate file summaries (Aider, CodeMonkeys)?
64. Walk through the Agentless three-phase process and the two "probabilistic LLM" ideas behind it.
65. Describe the Display class and the four color-coded ReAct phases; which TinyAgent tools require approval and why?

---

## Section D: Essay / Applied (5 points each)

66. **Code tools.** Describe the main code tool categories: file manipulation, sandboxed code interpreters, command-line tools, computer use tools, code search (ast-grep and semantic search), and SQL tools. For each, give an example use case and one security consideration.
67. **Context management.** Explain how software engineering agents manage context: prompt caching (the two requirements and growing the cache), repository maps (Gitingest, Aider's AST approach, CodeMonkeys' amortization), and context compaction (static/stable/dynamic sections). Why does each matter for cost and latency?
68. **SWE-bench and planning.** Describe SWE-bench: what it measures, how a fix is graded, and why it caught on. Then explain the role of planning in software engineering agents, including why the plan should be flexible and include exploration steps, and when a static workflow like Agentless beats a full agent.
69. **TinyAgent coding agent.** Describe how the book builds a coding agent from the TinyAgent: the four tools, which require approval and why, the Display class with ANSI colors, and how the agent loop surfaces THOUGHT/ACTION/OBSERVATION. What role does the model (e.g., North Mini Code) play in the agent's capability?
70. **Building code LLMs.** Explain the training recipe for coding LLMs: pre-training data composition (Qwen3 Coder), the four coding task data types, SFT, RL with verifiable rewards (RLVR vs RLHF), and the GLM-5 pipeline. Finally, explain the synthetic data feedback loop and its implications for future LLM generations.

---

## ANSWER KEY

### Section A: Multiple Choice
1. d
2. a
3. b
4. c
5. a
6. d
7. b
8. c
9. a
10. d
11. b
12. c
13. a
14. d
15. b
16. c
17. a
18. d
19. b
20. a
21. a
22. d
23. b
24. c
25. d
26. a
27. c
28. b
29. a
30. d
31. c
32. b
33. a
34. c
35. d
36. b
37. d
38. b
39. c
40. c

### Section B: True/False
41. **T** — Running/testing code gives a clear objective signal.
42. **F** — Vibe coders build through natural language and let the agent handle the details.
43. **T** — Executing and presenting results opens agents to a massive non-developer audience.
44. **F** — They are often ephemeral: fresh per session, data deleted afterward.
45. **T** — Every restriction trades convenience for security.
46. **T** — AST matching ignores formatting, variable names, whitespace.
47. **T** — Identical cached input; new input appended to the end.
48. **F** — A repo map is a compact structure (files + summaries), not the full codebase.
49. **F** — Fixes are graded objectively by running the repo's own test suites.
50. **F** — Agentless is a static three-phase workflow, sometimes without a full agent.
51. **T** — The best solution passes more generated tests than others, not necessarily all.
52. **T** — ANSI escape codes color THOUGHT/ACTION/OBSERVATION/ANSWER.
53. **T** — The user inspects and answers Y/N before dangerous tools run.
54. **F** — Coding LLMs also need reasoning, multi-step tool calling, long-context, and planning.
55. **T** — Around 2024 LLMs became a major source of next-gen training data.

### Section C: Short Answer (model answers)
56. **Verifiability.** Code can be run or tested to get a clear signal of whether it worked, enabling objective evaluation and, later, RL with verifiable rewards — unlike open-ended tasks with no objective check.
57. **Users and builders.** Users: non-code output seekers, vibe coders, software engineers. Builders: agent builders (tools, context, loop) and coding-LLM trainers.
58. **Data retrieval cases.** (A) File handed to agent → Python sandbox with pandas/polars; (B) agent finds files itself → command-line tools on a VM with a filesystem; (C) data in a database → SQL tool.
59. **Ephemeral sandbox properties.** (1) Fresh environment per user/session → predictable initial state (packages) and user-data isolation; (2) data deleted at end of session → no persistence, so nothing lingers to leak or be corrupted.
60. **OpenClaw (2026).** Poisoning any single dimension of persistent state (capabilities, identity, or knowledge) raises attack success rates from ~25% to 64% to 74%; the exposure is inherent in the architecture rather than a fixable bug.
61. **Least privilege.** Grant only the permissions/data the task requires and scope workflows so a wrong action stays contained — a builder choice at design time. Per-command approval only helps experts who can judge each command; non-experts can't evaluate what a command will do.
62. **Caching requirements.** (1) Cached input must remain identical (can't change a single token); (2) new input must be appended to the end. To maximize hit rate, let the cache grow to cover the whole prior trajectory so only new output is computed.
63. **Repository map.** File/directory structure + info about relevant files. Aider: AST-derived class/function signatures as summaries, then ranking/filtering for the most relevant definitions. CodeMonkeys: LLM reads every file, cost amortized across many samples (~15% of budget), files ranked and trimmed.
64. **Agentless.** (1) Localize — search repo for snippets relevant to the issue (RAG-like); (2) repair — generate candidate patches, sampling multiple; (3) rank — generate unit tests and pick the candidate passing more than the others. Ideas: use LLM probability (sampling at temperature variety) and use generated tests as a ranking signal, not pass/fail.
65. **Display class.** Color-codes ReAct phases with ANSI codes: THOUGHT (green), ACTION (red), OBSERVATION (yellow), ANSWER (purple). write_file and execute_python require approval because they're not safe by default (could corrupt files/dangerous actions).

### Section D: Essay (grading notes)
66. **Expect** each category: file manipulation (cat/sed/head/tail/ls/glob/grep; use case reading/editing files; security: per-tool permissions); sandboxed interpreters (restricted, ephemeral; security: LLM code unsafe — deletion/corruption/data leaks); command-line tools (environment config; security: adversarial surface — RedCode/SandboxEval/OpenClaw, least privilege); computer use tools (screenshots to VLM; operating Excel/verifying UIs); code search (ast-grep AST vs grep; semantic search via embeddings; CoIR); SQL tools (querying databases; schema linking/semantic layer).
67. **Expect** caching: static repeated context makes caching pay off; two requirements (identical prefix, appended new input); grow cache with trajectory for max hit rate. Repo maps: Gitingest, Aider AST signatures + ranking, CodeMonkeys amortization (~15%). Compaction: summarize long context; sections at different frequencies (static/stable/dynamic); after compaction dynamic shrinks, others cached.
68. **Expect** SWE-bench: real GitHub issues + repo snapshots; hidden test suites grade objectively; caught on due to real data, verifiability, headroom. Planning: detailed plans updated as agent learns; surfaced as to-do lists; dedicated planner possible; best model + time; include exploration/investigation and flexibility; static workflows (Agentless) fit simpler tasks rather than shoehorning an agent.
69. **Expect** four tools (read_file, list_files, write_file, execute_python); write_file/execute_python require approval (Y/N); Display with ANSI colors for THOUGHT/ACTION/OBSERVATION/ANSWER; loop surfaces phases; the model is the biggest lever (North Mini Code 30B/3B MoE Apache 2.0 purpose-built for agentic coding).
70. **Expect** pre-training (Qwen3 Coder 7.5T tokens, 70% code); four data types (tool-use, multi-step, unit-test, SE issues+diffs); SFT; RLVR (verifiable rewards, vs RLHF); GLM-5 (28T tokens, 200K context, RL reasoning/agentic/general, distillation); synthetic data loop (Llama 3.1 from Llama 3; Qwen2.5→Qwen3; DeepSeek-R1-Zero→R1; Nemotron, OmniSQL examples).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely code tools or the training pipeline).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
