# AI Agents — Comprehensive Study Notes
**Source:** *An Illustrated Guide to AI Agents* by Stéphane Donnet (O'Reilly, Early Release)
**Scope:** Chapter 1 (Introduction)

---

## Chapter 1: Introduction

### 1.1 The Big Picture
- In the mid-2020s, AI agents began reshaping AI. After ChatGPT (Nov 2022) revolutionized LLMs, AI agents took things further: they **act with autonomy, pursue goals, and interact with the world**.
- LLMs require significant "hand-holding"; AI agents autonomously decide **which** actions to take, **when** to take them, and **how**.
- This book covers **LLM-backed AI agents** specifically (there are other agent types that don't use LLMs, but "AI agent" in this book always means LLM-backed).

### 1.2 What Is an AI Agent? (Core Definition)
> **"An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."** — Russell & Norvig, *Artificial Intelligence: A Modern Approach*

The four components at the heart of agents:

| Component | Meaning |
|---|---|
| **Environment** | The world the agent interacts with |
| **Sensors** | Components used to observe the environment |
| **Actuators** | Tools the agent uses to interact with the environment |
| **Agent program** | The "brain" or rules used to go from observations to actions |

**Mapping to LLMs:**
- **Agent program (brain)** = a "reasoning" LLM capable of complex thinking
- **Actuators** = the LLM's tools
- **Sensors** = multimodal LLM capabilities (interpreting images, sound, etc.)
- **User** = part of the environment, often initiates the interaction

### 1.3 Large Language Models (the "Brain")
- An LLM predicts the **next word/token** given input text.
- It breaks input into **tokens** — subcomponents of words that let the model generalize to unseen words.
- It predicts the next token, appends it to the input, and continues — this iterative process is **autoregression** (Figure 1-4 shows one-token-at-a-time output).
- Covered in depth in Chapter 2.

### 1.4 Reasoning Large Language Models
- **Train-time scaling**: throwing more data, compute, and parameters at models (GPT-3.5-style). Led to predictable improvements but hit a **ceiling effect** — scaling became too expensive for the small performance gains.
- **Test-time compute / reasoning**: models like **OpenAI o1** and **DeepSeek R1** were trained to "think out loud" — generating **reasoning tokens** before producing the answer.
- Reasoning lets LLMs:
  - Write down their thoughts first via autoregressive behavior
  - Keep track of subproblems of a multi-step task
  - Handle more complex queries requiring multi-step reasoning
- In playgrounds like ChatGPT, thoughts are typically **hidden or summarized**; the final answer is a conclusion built on the reasoning trace.
- **Trade-off**: reasoning LLMs excel at complex decision-making and multi-step problems but are slower/more expensive; use "regular" LLMs for fast, cheap responses.
- Reasoning is central to agents: planning, tool selection, reflecting on mistakes, and revising plans all require advanced reasoning.

### 1.5 Augmenting the LLM
Text-only LLMs as static text-in/text-out entities have three gaps: **no memory, no environment control, no learning**. We augment them with:

#### Memory
- LLMs are **stateless** and forgetful — information is not persisted across calls (Figure 1-9).
- Simplest fix: **add the previous conversation to the current prompt** (Figure 1-10).
- Memory modules mimic human memory (short-term vs long-term), and how we process information.
- **Information overload**: too much information → poor decision-making, even for LLMs.
- **Context engineering**: carefully balancing the amount and quality of information given to the LLM.

#### Tools
- LLMs are **text-in/text-out functions** — they can only *describe/show intent* to take an action, e.g. output the string `multiply(2.3, 8.1)`.
- The LLM expresses **intent** (often as JSON); external software must **parse and execute** it.
- Tools range from simple calculators/search engines to command-shell and coding-environment tools.
- Chapter 5 covers tool use and the **Model Context Protocol (MCP)** for standardizing tools across LLMs.

#### The "Augmented LLM" (Anthropic's term)
- A reasoning LLM + memory + tools = **the augmented LLM** (Figure 1-14) — the building block we turn into an agent.

### 1.6 Planning and Reflection (Final Ingredient)
- **Planning = task decomposition**: breaking a large task into smaller actionable steps.
- The agent creates an initial plan, executes tasks **one at a time**, and refers back to the plan to track progress (Figure 1-15).
- After each task, the agent **reasons** over which step to take next (Figure 1-16).
- **Reflection**: the agent discovers faults midway (e.g., Google + arXiv insufficient → adds Semantic Scholar + PubMed) and updates the plan.
- **Iterative loop**: plan → take actions → reflect on output → update plan (Figure 1-17).

> **Together, a reasoning LLM augmented with memory, tools, planning, and reflection = an AI agent** (Figure 1-18).

### 1.7 An Agentic System

#### Autonomy (a spectrum)
- Agents have varying degrees of autonomy — from **partial** (can execute one step, free to choose tools) to **complete freedom** with no guardrails (Figure 1-19).
- **Guardrails** are often necessary to prevent destructive actions (e.g., deleting important files).
- A system is more "agentic" the more the LLM controls its actions, but as long as it exhibits **goal-directed behavior and makes decisions**, it qualifies as an agent. Partial autonomy in orchestrated workflows still counts as agency.

#### Why agents are useful
They thrive on **open-ended problems** where the goal is clear but the path is not. Common use cases:
1. **Coding** — assistants like Antigravity, Claude Code, Codex, Cursor. Writing + validating code; goals are clear with pre-defined requirements; problems are automatically verifiable.
2. **Deep research** — autonomous in-depth analysis across arXiv, PubMed, Google Scholar, etc.
3. **Automation** — standardizing processes (e.g., structuring heterogeneous patient data across hospitals).

#### Responsible development and usage
- **Human in the loop**: as autonomy grows, humans must authorize, check, and audit agent decisions.
- **Guardrails**: full autonomy can be overkill or harmful; a system with many guardrails steers the agent toward expected behavior.
- **Misinformation**: agents are still LLMs and can **hallucinate** — confidently generate incorrect information. Additional checks/balances needed where correctness is critical.

#### Evaluating agents
- Harder than evaluating models: agents reason over multiple steps, call tools, and take action sequences — a single text-quality score is not enough.
- Two main lenses:
  - **Outcome evaluation** — did the task actually get done (message sent, record updated)?
  - **Trajectory evaluation** — the steps and tool calls taken; judged on **efficiency and soundness** even when the outcome is correct.
- Two additional properties (not visible in a single run):
  - **Reliability** — does the agent succeed every time (outputs are stochastic)?
  - **Safety** — does it avoid harm (from malicious users, manipulated data, or its own mistakes)?
- **Key takeaway: evaluating an agent = evaluating an entire system, not just a model.** (Chapter 7.)

### 1.8 Book Structure
- **Part 1 (Ch 1–7): A single agent** — foundations, building it, evaluating it.
- **Part 2: Specializations**
  - **Multi-agent collaboration (Ch 8)**: multiple agents each responsible for different tasks; they interact and consult each other's specialties. Often a **supervisor agent** manages communication, planning, decomposition, and task assignment (usually has the most capable LLM). Orchestration can be structured or unstructured.
  - **Multi-modal agent (Ch 9)**: the agent is multimodal if its LLM can **process and/or generate** multiple modalities.
    - *Understanding* multiple modalities: uses an **encoder** (modalities → numeric info) + a **connector** (connects representations to the LLM).
    - *Generating* other modalities: uses a **generator**.
  - **Coding agent (Ch 10)**: can run programs in a dedicated environment, read codebases, generate functions, fix bugs, test. Led to **"vibe coding"** — non-developers relying on agents to build software. Benchmarked on **SWE-bench**.

### 1.9 The TinyAgent (Build-Along Project)
- You build a **TinyAgent** step by step from the book's principles; code is in the `illustrated-agents` package (pip/uv), plus GitHub notebooks.
- The code implementing agent behavior is called the **agent harness**. Types:
  - **Terminal-based**: Claude Code, Gemini CLI, OpenAI Codex CLI, OpenCode
  - **Code-based** (libraries to build your own): LangGraph, Smolagents, Pydantic AI
  - **Personal assistant** (persistent, retains memory/skills across sessions): OpenClaw, Hermes Agent
  - **Hosted** (cloud products): Replit, v0, Manus
  - **UI-based**: Antigravity, Cursor, Windsurf, GitHub Copilot
- Harnesses are trending toward **personal assistants** — persistent, always-on, chat via any messaging system (WhatsApp, Discord, Slack, email). OpenClaw is the most famous example (~300k GitHub stars in months).
- TinyAgent's harness is **code-based with a terminal implementation**, built from scratch with minimal dependencies.
- Initial skeleton (agent.py):
```python
class TinyAgent:
    """A minimal, modular, and educational agent framework."""
    def __init__(self):
        self.llm = None      # Chapter 2 & 3: Add LLM
        self.memory = None   # Chapter 4: Add Memory
        self.tools = None    # Chapter 5: Add Tools
        self.planner = None  # Chapter 6: Add Planning

    def run(self, task: str) -> str:
        """Run the agent on a task."""
        return self._step(task)

    def _step(self, task: str) -> str:
        """Perform a single step."""
        return f"Received: {task}"

    def _execute_action(self, action: str) -> str:
        """Execute a tool action."""
        return f"Executed action: {action}"
```
- Key methods: `run` (run on a task), `_step` (single step — may be answer or tool call), `_execute_action` (execute a tool action).
- Each chapter ends with a **"What We Built"** section summarizing TinyAgent changes.

### Chapter 1 Key Takeaways
1. Agent = perceives environment via sensors, acts via actuators (Russell & Norvig).
2. LLM-backed agent = reasoning LLM + memory + tools + planning + reflection.
3. Reasoning = test-time compute ("thinking out loud") vs train-time scaling ceiling.
4. LLMs are stateless → need memory; text-in/text-out → need tool-calling software.
5. Autonomy is a spectrum; guardrails and human-in-the-loop matter.
6. Evaluation: outcome + trajectory lenses, plus reliability and safety.

---

## High-Yield Vocabulary List (Chapter 1)
- **Agent**: perceives environment via sensors, acts via actuators.
- **Autoregressive model**: generates output by feeding its own prior output back as input.
- **Token**: word, part of word, number, or punctuation — the unit of LLM I/O.
- **Context engineering**: choosing the most relevant info to fit within context length.
- **Hallucination**: confidently generated incorrect information.
- **Trajectory**: sequence of steps (thought → action → observation) an agent takes.
- **Agent harness**: code/software implementing agent behavior (terminal, code, assistant, hosted, UI).
