# Comprehensive Study Notes — Chapter 10
**Source:** *An Illustrated Guide to AI Agents*, Chapter 10 "Code Agents and Code LLMs"

---

## CHAPTER 10 — CODE AGENTS AND CODE LLMs

### 10.1 The Big Picture

- **Coding agents are some of the most potent types of AI agents** in terms of likely impact. Code generation became one of the earliest applications of LLMs once they hit GPT-3's scale in 2020.
- Code generation was also a major component of reasoning problems (Ch 3). **Two reasons code is a key investment area:**
  1. An enormous range of knowledge work (software engineering, research, data analysis, visualization) can be expressed as code.
  2. **Code can be verified automatically** — run it or test it and get a clear signal of whether it worked. That verifiability makes coding more tractable than open-ended tasks with no objective check.
- This chapter: who uses and builds code agents, what makes software engineering agents distinct, building one, and how code LLMs are trained.

### 10.2 Users and Builders

- **Users** of coding agents fall into three groups, differing in how closely they work with the code (Figure 10-1):
  1. **Non-code output seekers** — want a chart/analysis, may never see the code.
  2. **Vibe coders** — want working software but build through natural language; the agent handles details.
  3. **Software engineers** — read, review, and direct the agent inside a codebase.
- **Builders** split in two:
  1. Those who **build the agents** — assembling tools, context, and loop around a model (done later in the chapter).
  2. Those who **train the coding LLMs** everything else depends on.
- **Evolution from code generation to code agents (Figure 10-2):**
  1. Asking an LLM (mostly in a playground UI) to write a specific piece of code.
  2. LLM generates code, **executes it in an execution environment**, presents results — opens agents to a massive non-developer audience.
  3. Agent solving **larger software-development tasks** with tens/hundreds of steps, relying on a richer execution environment.

### 10.3 Building Code Agents

#### Code Agents to Serve Non-Coders
- Data analysis example: asking an agent to make a plot (Figure 10-3). Main expected behaviors:
  1. **Data retrieval** — decide what tool to use:
     - **A) Hand the agent the file** (e.g., spreadsheet): a Python sandbox; the agent writes code to read/manipulate data with pandas or polars.
     - **B) The agent finds the files itself**: command-line tool on a VM with filesystem access; code uses command-line tools to search and navigate.
     - **C) Data is in a database**: a SQL tool; the agent writes SQL to retrieve and transform data.
  2. **Creating the plot** — write code to plot it and return the plot.
  3. **Troubleshooting** — plan for failure cases (unavailable data, mistakes in the query). If the agent gets an error from the execution environment and sends it back to the LLM, it can recover on the next step — **the ReAct loop from Ch 6**: the error is just another observation. But don't rely on that alone; plan for specific failure cases.
- Key point: the user doesn't necessarily see or interact with any code — a major lever that dramatically increases the number of people these systems help.

#### Code Tools
- Code tools tie an LLM to a software system: the model decides what to do, the tool carries it out against a real environment (running code in a sandbox, shell command, database query).
- Example (Figure 10-4): a two-step trajectory — first the SQL tool pulls sales data, then the Python tool plots it, returning the chart.

**File manipulation tools** — first tasks: reading, searching, editing files. Implemented as distinct tools with their own permissions/checks; often wrappers around command-line tools:
- Read entire file (e.g., `cat`)
- Read a section of a file (e.g., `sed`, `head`, `tail`) — to manage model context with long files
- List files in a directory (e.g., `ls`)
- Pattern-match file names (e.g., `glob`)
- Search contents (e.g., `grep`)

**Code interpreter sandbox environment** — more restricted than a full VM. Restrictions matter for security because **LLM-generated code is not guaranteed to be safe**: an agent could delete files, corrupt a system, or share private data (via malicious attacks/prompt injection, or well-meaning but naive actions). Sandboxes:
- Defined with limited resources (memory, processor, disk space).
- **Often ephemeral**, so that:
  1. A fresh environment is created per user/session → predictable initial state (packages) and separation of user A's data from user B's agents.
  2. Data is deleted at the end of the session; no expectation of persistence.
- Restricted in which commands can run; decide whether code has internet access. **Every restriction trades convenience for security** (the more capability you add, the wider a mistake or attack can be).
- **Gemini code execution tool**: accessed via API; `pip install google-genai`, requires a Gemini API key from Google AI Studio. Example: a personal math tutor system message, with `types.Tool(code_execution=types.ToolCodeExecution())`. The model writes code (e.g., computing profit), the execution result returns to the model, which then produces the final answer. Background: a container is created and charged per session (plus LLM inference cost metered in tokens).
- Other options: **hosted sandbox services** — Modal, Daytona, E2B, Together Code Sandbox (SDK/API receives code, returns result, hides environment management). Or **roll your own** with open-source tools like **Open Interpreter** or the **e2b repo** (isolated container you control) — right when you have specific security/networking requirements.

**Command-line tools** — the next step up in power: access to the command line for inspecting/changing the environment (e.g., installing Python packages not in the code-interpreter image). Software engineering agents like **Cursor or Claude Code** need command-line access to configure environments, run Python scripts and unit tests.
- **Security**: unfettered command-line access can be a security nightmare. Research treats the execution environment as an **adversarial surface**:
  - **RedCode: Risky Code Execution and Generation Benchmark for Code Agents** (2024) — probes agents for dangerous behaviors (file deletion, privilege escalation, network misuse).
  - **SandboxEval: Towards Securing Test Environment for Untrusted Code** (2025) — security-relevant properties: (1) exposing system/directory/metadata info; (2) manipulating structures/contents/privileges of filesystems; (3) initiating external communication and dangerous operations.
  - **"Your Agent, Their Asset: A Real-World Safety Analysis of OpenClaw"** (2026) — first real-world safety evaluation of a personal-agent system; **poisoning any single dimension of persistent state (capabilities, identity, or knowledge) raises attack success rates from ~25% to 64% to 74%**; the exposure is inherent in the architecture, not a fixable bug.
- **Mitigation**: software agents in IDEs ask the user to approve each command before running. The developer is the ultimate decision-maker — works when the approver has expertise, but offers little protection to non-technical users. Safeguard is mostly the **agent builder's responsibility at design time**: follow the **principle of least privilege** — grant only the permissions and data access the task requires, and scope tasks/workflows so a wrong action stays contained.

**Computer use tools** — like command-line tools on a VM, but interact with the OS **UI** and feed **screenshots** to a vision-language model. Useful for operating apps (e.g., Excel) or verifying how UIs render in a browser at a specific window size (Figure 10-6).

**Code search** — more powerful retrieval:
- **ast-grep**: pattern matching on the code **abstract syntax tree (AST)** rather than raw text — operates on code structure, ignoring formatting, variable names, whitespace.
- **Code semantic search**: dense embeddings from a code-embedding model; query like "find a code snippet that iterates over a list" returns relevant candidates.
- **CoIR (Code Information Retrieval Benchmark)** — measures code retrieval; maintains a leaderboard.

**SQL tool** — organizations' data lives in databases; agents write SQL. Whether single or multiple databases, an agent can write complex SQL, possibly over multiple queries (e.g., first inspect schema, then issue a sequence of statements building intermediate results — Figure 10-7). **Schema linking** research area:
- Simple agents: provide a general database schema (tables and columns).
- Advanced agents: a **semantic layer** that identifies relevant columns, indexes data, and captures user language. Example: "show me all outerwear products" — database has jackets, blazers, coats, but not "outerwear."
- **"Arming Data Agents with Tribal Knowledge"** (2026): augments an SQL agent with institutional knowledge (what columns mean, how users phrase requests) accumulated from the agent's own query mistakes rather than a hand-built semantic layer.
- Surveys: text-to-SQL surveys (2024).

#### Context Management
- As environments grow, work around the **LLM context window** and the **latency/cost of long contexts**.

**Context optimization for the LLM cache**
- A software engineering agent's context includes system/developer prompts, agent persona, tool descriptions, **desired security behaviors**, and a **description of the repository**. Much is static and repeats on every call — exactly what makes caching pay off.
- Terms: **kv-caching, prompt-caching, prefix-caching** (introduced in Ch 2). Cache the large fixed prefix (system prompt + repository structure); only newly generated tokens need fresh computation → cuts latency and cost (Figure 10-8).
- Even the first call can run to tens of thousands of tokens; parts repeat in every subsequent call (Figure 10-9).
- **Two requirements for the cache to work:**
  1. The cached input must remain **identical** — you can't change even a single token in existing methods.
  2. All new input must be **appended to the end** of the cached input.
- Each turn = LLM call + tool call; the cache grows with the trajectory. Best practice: **let the cache grow to cover the whole prior trajectory** to maximize cache hit rate (Figures 10-10, 10-11).

**The repository map**
- Representing repository structure is essential context. Key components: file/directory structure at some resolution + additional info about relevant files.
- **Gitingest**: takes a GitHub repo URL, generates a repo map (directory list + contents of short files).
- **Aider's approach** (one of the first command-line code agents, precursor to Claude Code): uses the **AST** to identify important parts (class and function signatures) as file summaries; then a follow-up step **ranks and filters** for only the most relevant definitions used most in the repository (Figure 10-12).
- **CodeMonkeys: Scaling Test-Time Compute for Software Engineering** (2025): uses an **LLM to summarize key files**. Efficiency from **amortization** — because the system samples many candidate solutions per issue, it can afford an LLM reading every file; that one-time cost spreads across all downstream samples (~15% of the total budget). Files are ranked and trimmed to fit context.

**Context compaction**
- Summarizing long-conversation context is a form of short-term memory (Ch 4). For code agents, the summary may include: environment info, **code style preferences**, preferences for when/how to run tests, and **when to commit** to source control.
- Combined with caching, keep different trajectory sections at different change frequencies (Figure 10-13): **static** section stays put, **stable** shifts now and then, **dynamic** turns over each step. After compaction, the dynamic section shrinks while the static/stable sections stay cached for more steps.

### 10.4 Code Agents for Software Engineering

- First encounter with a code agent is usually inside an **IDE/editor**: autocompletion (documentation, first draft of a function), then solving more complex problems requiring edits in multiple places.
- **Early agents needed close guidance** — the task wasn't well represented in training data; heavy external scaffolding compensated. The **original SWE-agent** introduced a purpose-built **Agent-Computer Interface** — a constrained set of commands to make a repository navigable for a model that couldn't reliably do it alone. Newer agents rely far less on this because models are now trained directly on tool use, multi-step trajectories, and software engineering tasks.
- **Software engineering skills**: environment management, unit testing, debugging and troubleshooting, reproducing bugs and investigating causes — reading enough of the right places of the repository to understand intent and flow.
- Figure 10-14: shift from function-level coding (single LLM call emitting code) to solving real GitHub issues (agent running tools against an environment, iterating over N steps, planning and reasoning).

#### SWE-bench
- **First widely adopted code agent benchmark.** Collection of **hundreds of software engineering issues from open source repositories** (actual user-reported issues). Each example: a single GitHub issue + a snapshot of the repository at a point in time. The model must resolve the issue so it **passes a set of test suites** verifying the fix.
- Why it caught on: measures something **real** (actual issues + each repo's own test suite) → fixes graded **objectively** by running tests, not subjectively. That verifiability + early systems solving only a small fraction (headroom) made it the benchmark labs compete on.

#### Planning for software engineering agents
- Larger multi-step problems made **planning** a key component. A coding agent must write a detailed concrete plan and **keep returning to update its status or the whole plan** as it learns more.
- UIs started showing plans and to-do lists as distinct elements so users can approve or edit before execution (Figure 10-15).
- Some agentic code systems assign planning to a **dedicated planning agent** and steps to dedicated code agents. Even in a single agent, the plan is one of the most important components — often worth assigning the **best available model** and giving it reasonable time to think.
- **Planning prompt design caveat**: some tasks require investigation to know how to solve them (software tasks especially need exploration/research phases, or asking the user for clarification). Don't assume the approach is crystal clear at planning time; include exploration/investigation steps and build in flexibility.

#### Task-Specific Workflows
- Sometimes a problem is simpler with a **static workflow** (fixed set of steps laid out in advance) than an agent. Routine tasks don't need an agent; a proficient agent developer picks the tool that fits rather than shoehorning an agent into every job.

**Agentless**
- A software-engineering approach that competes with agentic approaches via a simple **three-phase process** (Figure 10-16):
  1. **Localize** — find code snippets in the repository relevant to the issue (works like RAG: a search step).
  2. **Repair** — hand snippets + issue to a coding LLM that generates a candidate solution. **Sample multiple solutions** (multiple attempts).
  3. **Rank with generated unit tests** — coding LLMs are great at generating unit tests; generate a suite of unit tests to validate solutions, then use them to **rank and choose** the best. The selected solution doesn't need to pass all tests — just **more than the other candidates** (tests are generated, so quality isn't fully trusted).
- **Two powerful ideas using the probabilistic nature of LLMs:**
  1. **Sampling multiple possible solutions** (at temperature values allowing reasonable variety).
  2. **Generated unit tests as a ranking signal, not a pass/fail signal.**

### 10.5 TinyAgent with Code Tools

- Core of the coding agent = tools. Most common categories: **file manipulation tools** (read, list, write files) and a **code interpreter** (execute code the agent writes).
- Tools return a string whether they succeed or not — used in the messages structure as an **observation**.
- `execute_python` and `write_file` are **not safe by default** → use the **`requires_approval` parameter** (Ch 5): before execution, the user replies "Y" (yes) or "N" (no) after inspecting whether the action is safe.

**Code (coding tools):**
```python
import subprocess
import sys
from pathlib import Path


def read_file(path: str) -> str:
    """Read a file's contents."""
    target = Path(path)
    if not target.exists():
        return f"Error: '{path}' not found."
    return target.read_text(encoding="utf-8")


def list_files(directory: str = ".") -> str:
    """List files in a directory."""
    target = Path(directory)
    if not target.is_dir():
        return f"Error: '{directory}' is not a directory."
    entries = sorted(target.iterdir())
    return "\n".join(p.name + ("/" if p.is_dir() else "") for p in entries) or "(empty)"


def write_file(path: str, content: str) -> str:
    """Write content to a file."""
    target = Path(path)
    target.parent.mkdir(parents=True, exist_ok=True)
    target.write_text(content, encoding="utf-8")
    return f"Written to '{path}'."


def execute_python(code: str) -> str:
    """Execute Python code and return output."""
    try:
        result = subprocess.run(
            [sys.executable, "-c", code],
            capture_output=True,
            text=True,
            timeout=30,
        )
    except subprocess.TimeoutExpired:
        return "Error: Code execution timed out (30s limit)."

    if result.returncode != 0:
        return (
            f"Exit code {result.returncode}\n"
            f"STDOUT:\n{result.stdout}\n"
            f"STDERR:\n{result.stderr}"
        ).strip()
    return result.stdout.strip() or "(no output)"
```

**Code (registering tools with approval):**
```python
tools = NativeTools(requires_approval=["write_file", "execute_python"])
tools.add_tool("read_file", read_file)
tools.add_tool("list_files", list_files)
tools.add_tool("write_file", write_file)
tools.add_tool("execute_python", execute_python)
```

- With write/execute capabilities, TinyAgent becomes a basic coding agent — but the playground interface isn't ideal; more advanced interfaces (Cursor, terminal agents) are common. Implement a terminal interface.

**Display class** — shows what the agent is doing, separating ReAct loop steps with color coding (ANSI escape codes, e.g., `"\033[35m"`):
- **THOUGHT** = the agent's reasoning → GREEN
- **ACTION** = the tool the agent wants to use → RED
- **OBSERVATION** = output of the tool use → YELLOW
- **ANSWER** = final answer → PURPLE

**Code (Display class):**
```python
BOLD = "\033[1m"
RESET = "\033[0m"
GREEN = "\033[32m"   # THOUGHT
RED = "\033[31m"     # ACTION
YELLOW = "\033[33m"  # OBSERVATION
PURPLE = "\033[35m"  # ANSWER


class Display:
    """Chat interface with styling."""

    def __call__(self, event: str, data: str | Response = None) -> None:
        if event == "thinking":
            print(f"  Thinking...\n{RESET}")
        elif event == "response":
            print(f"{BOLD}{GREEN}{'▒▒ THOUGHT ▒▒':<13}{RESET}")
            print(f"{data.reasoning}{RESET}\n")
            if data.content:
                print(f"{BOLD}{PURPLE}{'▒▒ ANSWER ▒▒':<13}{RESET}")
                print(f"{data.content}{RESET}\n")
        elif event == "tool_call" and data:
            tool = data.tool_call["tool"]
            kwargs = data.tool_call["kwargs"]
            print(f"{BOLD}{RED}{'▒▒ ACTION ▒▒':<13}{RESET}")
            print(f"{tool}({kwargs}){RESET}\n")
        elif event == "observation":
            print(f"{BOLD}{YELLOW}{'▒▒ OBSERVATION ▒▒':<13}{RESET}")
            print(f"{data}{RESET}\n")
            print(f"{'─' * 40}STEP{'─' * 40}{RESET}\n")
```

- The Display is integrated into TinyAgent by adding `self.display(...)` calls at the points we want to track (thinking, response, tool_call, observation).

**TinyAgent with Display** — the `__init__` takes `display: Display`, builds the system prompt from planner + tools, and the `run`/`_step`/`_execute_action` loop calls `self.display("thinking")`, `self.display("response", response)`, `self.display("tool_call", response)`, `self.display("observation", observation)` at the respective points.

**Code (TinyAgent init and step):**
```python
class TinyAgent:
    """A minimal, modular, and educational agent framework."""

    def __init__(self, llm, memory, tools, planner, display):
        self.llm = llm
        self.memory = memory
        self.tools = tools
        self.planner = planner
        self.display = display
        self.trajectory = Trajectory()
        system_prompt = "You are a helpful assistant.\n\n"
        system_prompt += self.planner.prompt
        system_prompt += self.tools.prompt
        self.memory.add("system", system_prompt)

    def run(self, task: str, image_data: str = None) -> str:
        self.memory.add("user", task, image_data=image_data)
        self.trajectory.initialize(task)
        for step in range(self.planner.max_steps):
            result = self._step()
            if result is not None:
                return result
        return "Max steps reached without completion."

    def _step(self) -> str | None:
        self.display("thinking")
        response = self.llm.generate(
            self.memory.get_messages(), tools=self.tools.schemas
        )
        self.memory.add("assistant", response.content, tool_call=response.tool_call)
        self.display("response", response)
        response = self.planner.parse(response)
        response = self.tools.parse(response)
        if self.tools.is_done(response):
            self.trajectory.add(response)
            return response.content
        return self._execute_action(response)

    def _execute_action(self, response: Response) -> None:
        self.display("tool_call", response)
        result = self.tools.execute(response)
        role, observation = self.tools.observation(result)
        self.memory.add(role, observation)
        self.trajectory.add(response, observation)
        self.display("observation", observation)
        return None
```

**CLI** — a `main()` loop reads `input()`, prints an ASCII-art NAME banner, runs `agent.run(query)` (handles KeyboardInterrupt/EOFError, "exit"/"quit"), run via `python .../ch10.py`. Running it surfaces THOUGHT/ACTION/OBSERVATION per step.

**Model note:** The LLM makes a large difference. **North Mini Code** — an open mixture-of-experts model from Cohere built for agentic coding: **30B parameters total, 3B active per token, Apache 2.0**. Purpose-built for code-focused, multi-step, tool-using work. (Co-author Jay was part of the core team that built North Mini Code.)

**What We Built:** TinyAgent/ — agent.py (updated: add Display), cli.py (new), display.py (new), toolbox.py (updated: coding tools), plus evaluator.py, llm.py, memory.py, planning.py, tools.py, trajectory.py.

### 10.6 Building Code LLMs

- A coding agent's quality is limited by its model: must be good at writing code **and** skilled at tool use and agentic software engineering.
- **Training stages (Figure 10-18):** pre-training (language modeling → base model) → **SFT** (instruction-tuned) → **RL** (gives the behaviors agentic work depends on). Same recipe for generalist or code-focused LLMs.

**The data**
- **Data composition:** code models see far more code data across training stages. **Qwen3 Coder** pre-trained on **7.5 trillion tokens, 70% of which is code**.
- **Coding task data** (data that looks like what the model sees in use):
  - **Tool-use data** — trajectories where the model calls a tool and uses the result (e.g., running grep to locate a function, then acting on what it finds).
  - **Multi-step data** — tasks over several turns (reproduce a bug, edit a file, run tests, read failure, patch again).
  - **Unit-test data** — code paired with the tests that verify it (a function alongside pytest cases).
  - **Software engineering tasks** — repository-level issues + the diffs that resolve them (a GitHub issue paired with the diff that closes it).

**Reinforcement learning**
- Now a major component. Coding LLMs are trained with large-scale **Reinforcement Learning with Verifiable Rewards (RLVR)** (Ch 2) rather than only RLHF. Verifiable rewards turn the chapter's opening observation (code can be checked by running it) into a training signal.
- **Other important capabilities:** reasoning, multi-step tool calling, long-context, planning, and trajectories showing proper software engineering practices (reproducing issues, using coding tools and common libraries).
- **GLM-5 pipeline example (Figure 10-20):** pre-training on the order of **28 trillion tokens**; mid-training extends context to **200K** with long code + agent data; SFT; then RL split into **reasoning, agentic, and general** stages before a final **distillation** step.

**Bootstrapping Intelligence: The Synthetic Data Feedback Loop**
- Around **2024**, LLMs became a major source of training data for the next generation of LLMs:
  - Much of **Llama 3.1's post-training data** was generated with Llama 3.
  - **Qwen2.5-Math and Qwen2.5-Coder** were essential in creating training data for Qwen3 and Qwen3-Coder.
  - **DeepSeek-R1-Zero** generated long chains of reasoning traces used by DeepSeek-R1 (Ch 3).
- A synthetic data generation recipe uses existing models to produce data feeding both pre-training and post-training across categories (code, reasoning, multi-step tool use, etc.) (Figure 10-21).
- Open source projects sharing code/details: **Nvidia's Nemotron 3 Nano and Nemotron 2 Nano**, AI21's "Scaling Text-Rich Image Understanding via Code-Guided Synthetic Multimodal Data Generation," and **OmniSQL** (synthesizing high-quality Text-to-SQL data at scale).

### Chapter 10 Key Takeaways
- Code agents are among the most impactful AI agents; code is verifiable, which makes it tractable for training.
- Users: non-code output seekers, vibe coders, software engineers. Builders: agent builders and coding-LLM trainers.
- Code tools: file manipulation (cat, sed/head/tail, ls, glob, grep), sandboxed code interpreters (ephemeral, restricted, security trade-offs; Gemini code execution, Modal/Daytona/E2B/Together, Open Interpreter), command-line tools (adversarial surface — RedCode, SandboxEval, OpenClaw 2026; least privilege), computer use tools, code search (ast-grep AST, semantic search, CoIR), SQL tools (schema linking, semantic layer, tribal knowledge).
- Context management: prompt caching (identical cached prefix + append new input; grow cache with trajectory), repository maps (Gitingest, Aider AST summaries, CodeMonkeys amortization), context compaction (static/stable/dynamic sections).
- Software engineering agents: IDE autocompletion → multi-file edits; early scaffolding (SWE-agent Agent-Computer Interface); SWE-bench benchmark; planning (dedicated planner, exploration steps); Agentless three-phase workflow (localize, repair with sampling, rank with generated unit tests); static workflows vs agents.
- TinyAgent coding agent: read_file/list_files/write_file/execute_python tools, requires_approval, Display class with ANSI colors for THOUGHT/ACTION/OBSERVATION/ANSWER, terminal CLI; North Mini Code (30B/3B MoE, Apache 2.0).
- Code LLM training: pre-training (Qwen3 Coder 7.5T tokens, 70% code) → SFT (tool-use, multi-step, unit-test, SE-task data) → RL (RLVR); GLM-5 (28T tokens, 200K context, distillation); synthetic data feedback loop (Llama 3.1, Qwen2.5, DeepSeek-R1).

---

## High-Yield Vocabulary (Ch 10)

| Term | Meaning |
|---|---|
| Code agent | Agent that generates, executes, and debugs code across multiple steps |
| Vibe coder | User who builds software via natural language, letting the agent handle details |
| Code tool | Tool carrying out the LLM's chosen action against a real environment |
| Sandbox / code interpreter | Restricted, often ephemeral code-execution environment |
| Ephemeral sandbox | Fresh per user/session; data deleted at end; predictable state |
| Least privilege | Granting only the permissions and data access a task requires |
| RedCode | 2024 benchmark probing agents for dangerous code behaviors |
| SandboxEval | 2025 work on securing test environments for untrusted code |
| OpenClaw analysis (2026) | Real-world safety eval; poisoning persistent state raises attack success 25%→64%→74% |
| Computer use tool | Interacts with OS UI; feeds screenshots to a vision-language model |
| ast-grep | AST-based code pattern matching (ignores formatting/whitespace) |
| Code semantic search | Dense-embedding retrieval over code |
| CoIR | Code Information Retrieval Benchmark with leaderboard |
| SQL tool | Executes agent-written SQL against databases |
| Schema linking | Relating user queries to database tables/columns |
| Semantic layer | Identifies relevant columns, indexes data, captures user language |
| kv/prompt/prefix-caching | Reusing cached prefix computations to cut latency and cost |
| Repository map | File/directory structure + file summaries in the prompt |
| Gitingest | Tool generating a repo map from a GitHub URL |
| Context compaction | Summarizing context (short-term memory) for long trajectories |
| SWE-agent | Early agent with a purpose-built Agent-Computer Interface |
| SWE-bench | First widely adopted code-agent benchmark; GitHub issues + test suites |
| Agentless | 3-phase workflow: localize, repair (sampling), rank with generated tests |
| RLVR | Reinforcement Learning with Verifiable Rewards |
| RLHF | Reinforcement Learning from Human Feedback |
| SFT | Supervised fine-tuning (instruction tuning) |
| Tool-use data | Training trajectories where the model calls and uses tools |
| Multi-step data | Training tasks spanning several turns |
| Unit-test data | Code paired with verifying tests |
| Synthetic data feedback loop | LLMs generating training data for the next generation of LLMs |
| North Mini Code | Cohere open MoE coding model: 30B total, 3B active, Apache 2.0 |
