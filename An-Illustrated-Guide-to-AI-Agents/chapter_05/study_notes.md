# Comprehensive Study Notes — Chapter 5
**Source:** *An Illustrated Guide to AI Agents*, Chapter 5 "Tool Usage, Learning, and Protocols"

---

## CHAPTER 5 — TOOL USAGE, LEARNING, AND PROTOCOLS

### 5.1 Why Tools Matter

- **Regular LLMs are text-to-text functions**: they take text in and produce text out. That makes good chatbots, but they cannot *interact* with their environment.
- **Tools** (functions and APIs) let an LLM step into the real world: codebases, internal tools, online databases, even home assistants.
- **A defining trait of an agent** is its ability to *autonomously* search for, select, and use tools. With enough capability, agents can even create their own tools.
- Tools have three main uses:
  1. Take an **action** (e.g., schedule an appointment).
  2. **Retrieve information** (access external knowledge/memory — connects to Ch. 4).
  3. Access **specialized LLMs** (e.g., multimodal LLMs, coding-specialized models) that extend beyond the agent's own capability.
- **Without tools**: "agents can only think and plan autonomously but not act autonomously."
- **Autonomy ↔ tool use**: 
  - No autonomy + fixed flow → tools used in a **predefined order** (e.g., a research agent always calls arXiv then Google then summarizes).
  - More autonomy → the agent **chooses which tool to use and when**, still as sequences of LLM calls.

### 5.2 Tool Usage: The Five Steps

The book's framework for how tools work end-to-end:

1. **Tool creation** — define the actual function.
2. **Tool definition** — tell the LLM what the tool does and how to call it.
3. **Tool selection** — the LLM picks the right tool for the query.
4. **Tool calling** — the LLM expresses intent; an external system executes it.
5. **Output processing** — feed the tool's result back to the LLM.

**Key insight:** *An LLM does not actually call a tool.* It only shows the **intention** to do so (a string, often JSON). The actual call is executed by the user or an external automated process. This flow is adapted from OpenAI's tool-calling flow.

#### Step 1: Tool Creation
- Tools are typically **functions** accessed via an **API** or internal code.
- Example: `def multiply(a: str, b: str) -> float: return float(a) * float(b)`
  - Parameters are **strings** because LLMs output only text; they must be converted.
  - Tools should be **heavily documented** (docstrings) — good docs are increasingly important; they let the agent understand edge cases and implementation details that aren't obvious from code.
- Tools can be stored in a **registry** (a dict mapping name → function).
- **Design tools for the LLM**, like designing code for people: Is the purpose clear? Is the return value clear? Are variable names descriptive? Is there documentation?
- The book's `Tools` class:
  - `add_tool(name, func, description)` → registers into `self.registry`.
  - `schemas` property → for native tool calling (later).
  - Optional `requires_approval` list → some tools need human approval before running (human-in-the-loop; covered more in Ch. 10).

#### Step 2: Tool Definition
Two main ways to communicate a tool to the LLM:

**A. Learning** — tool definitions/usage learned during (fine-)training.
- Split into: learning **specific tools** vs learning **general tool use**.
- Learning specific tools is costly and limits how many tools you can add.
- Recent models (Qwen 3, DeepSeek v3.2) instead improve **instruction-following + general tool use**: they recognize tool definitions in prompts and use them dynamically.

**B. Prompting** — share definitions through prompts. Two variants:
- **Natural language (system prompt):** e.g., write in plain text "You can use multiply(a, b): multiplies two numbers. To use a tool, respond with JSON: `{"tool": "name", "kwargs": {"param": "value"}}`." 
  - Not standardized — you decide the format. Requires a **parser/validator** to detect tool calls.
  - Error-prone: small deviations from the instruction format break parsing.
- **Structured function calling:** define each tool as a **JSON Schema** (name, description, parameters: types/required/ranges). Passed as a separate parameter (OpenAI's `tools` parameter) or embedded in the system prompt.
  - The clearer the schema, the more likely the LLM makes the right call.

**Best practices for tool definition:**
1. **Document extensively** — descriptions, parameter details, examples.
2. **Minimize the number of tools** — many tools make selection harder; fewer than 10 is a good start.
3. **Minimize the scope of each tool** — tools with dozens of parameters are hard for small models to use.

#### Step 3: Tool Selection
- The LLM must choose the right tool (or no tool) and use it correctly — hard with dozens of tools.
- **Reasoning LLMs shine here**: they can spend tokens "thinking" about which tool to use and how.
- As the number of tools grows, **discovering** relevant tools matters more. Filling the context window with all schemas hurts performance (Ch. 4 lesson) → use **RAG over tool schemas** (store all schemas in a vector database; retrieve the most relevant ones for a query).
- **Planning capabilities** are vital for selection of multi-step tasks (link to Ch. 6).

#### Step 4: Tool Calling
- The LLM ends its thinking and **outputs a string** formatted as a tool call (e.g., `{"tool": "multiply", "kwargs": {"a": "5.1", "b": "7.3"}}`).
- This is only *intent* — it does not execute.
- The `Tools` class is extended with:
  - `parse(response)` — extracts JSON from the model's text into `Response.tool_call`.
  - `execute(response)` — looks up the function in the registry, runs it with kwargs, and returns the result. Human-in-the-loop approval: if a tool is in `requires_approval`, ask the user before running.
- **NOTE:** regex extraction breaks on slightly different structures or complex JSON. Robust options: `jsonschema` (validates JSON schemas) and **Pydantic** (data validation; create schemas, convert/validate LLM output).

#### Step 5: Tool Output Processing
- The tool executes *outside* the LLM's view; the LLM has no knowledge of what happened — you must **pretend** the LLM executed it.
- Use the messages structure (Ch. 4):
  - `assistant` role → "the assistant calls the multiply tool."
  - `tool` role → the tool's output (only for LLMs trained for **native tool calling**).
- For non-native models: use the `user` role with an **`OBSERVATION:`** tag:
  - `observation(result)` returns `("user", f"OBSERVATION: {result}")`.
  - `is_done(response)` → stop if there's no tool call, or if it calls the `final_answer` tool (used more in Ch. 6).
- The LLM then processes the observation — it may combine, summarize, or reformat the result.

### 5.3 TinyAgent with Tools
- The system prompt is built from `self.tools.prompt` (tool definitions) — the system prompt will track more than tools in later chapters.
- A **single step** (`_step`):
  1. **Generate a response** (the "THOUGHT").
  2. **Tool parsing** (`planner.parse`/`tools.parse`) — convert string to JSON.
  3. **Tool execution** (`_execute_action`) — run the tool, track the result.
  4. **Stopping mechanism** — stop if no tool call (`is_done`).
- `_execute_action`: execute the tool call, then add the observation to memory + trajectory.
- **NOTE:** `SummarizationMemory` from Ch. 4 breaks with Tools because it overwrites the system prompt (which must keep tool definitions). Fix: update only a section of the system prompt rather than replacing it entirely.
- The capitalized words THOUGHT / OBSERVATION / ACTION / ANSWER map to the **ReAct framework** (detailed in Ch. 6):
  - **THOUGHT** = reasoning on what to do next.
  - **ACTION** = tool calls structured as JSON.
  - **OBSERVATION** = the output of the action.
  - **ANSWER** = the final answer after one or more steps.
- Demo result: query "What is 5.1 times 7.3?" → assistant outputs the JSON tool call → user role gets "OBSERVATION: 37.23". Effective tool use requires the LLM to be an effective **orchestrator** (depends on reasoning + reliability from Ch. 3).

### 5.4 Tool Learning

Three categories for teaching an LLM to call tools:

#### 1. In-Context Learning (few-shot)
- Learn from a few examples in context — no fine-tuning needed.
- Few-shot prompting: give examples of the desired tool-call format.
- Trick: **fake history** — add example messages as if the model already called tools, so it mimics the format and knows expected outputs.
- LLMs are great at pattern recognition; works better as the model gets more capable.

#### 2. Supervised Fine-Tuning (SFT)
- Motivations: prompting fills the context window and risks instruction-following failures; SFT **distills** tool-calling knowledge into the model.
- Popular 2023–early 2024: effective and cheap; but has downsides (generalization) later fixed with RL.
- **Toolformer** (a key example):
  - Embeds the tool call *and its output* into the generated text (in-line tool calling) using special tokens:
    - `[` = start of tool call (then select tool + params in fixed format)
    - `→` = the model may stop generating while the user/automated system executes the tool
    - `]` = the tool output is inserted as if it were a token the model generated
  - Example: Input "What is 5.1 times 7.3?" → Output "5.1 * 7.3 is [Calculator(5.1*7.3) → 37.23] The answer is 37.23."
  - Data creation: manually created few-shot prompts per tool → sample outputs → filter by correctness of tool use, output, and **loss decrease**. Fine-tuned GPT-J.
  - Benefits: natural language generation, no message round-trips — great for reasoning LLMs (recall Search-o1 in Ch. 4).
  - **Drawback**: SFT is sensitive to exact wording and prompt (mimicry), so generalization is hard.
- **NOTE (data quality):** For many prompts there's no single correct tool call (multiple tools/argument formats can all be valid). Also, quality data assumes tools never fail — but real tools fail, return partial errors, change schemas, behave inconsistently. RL handles these better.

#### 3. Reinforcement Learning (RL)
- RL uses **trial and error** + feedback signals instead of mimicking fixed examples → better generalization.
- **Tool-Integrated Reasoning (TIR):** incorporating tools into the reasoning traces of the LLM; a TIR trajectory may involve multiple tool invocations.

**ToolRL** (GRPO-based):
- Uses GRPO (DeepSeek-R1's algorithm). Tokens: `<thinking></thinking>`, `<tool_call></tool_call>`, `<answer></answer>`.
- Two rewards:
  - **Correctness** — correct tool names + parameters.
  - **Format** — required fields in the correct order.
- Trained on 4,000 TIR traces to fine-tune Qwen2.5 models.
- **Length reward experiments:** longer traces do NOT consistently help tool use and may harm small models.
- Flexible: can drop format reward for non-reasoning models.

**Search-R1** (efficient RL for search-as-a-tool):
- The LLM learns to generate search queries *during* step-by-step reasoning.
- Prompt template: `<think></think>` (reasoning), `<search></search>` (search engine call, output reintegrated via `<information></information>`), `<answer></answer>`. Interleaved multiple times.
- Motivation: **DeepResearch**-style agentic systems (reasoning LLM + search engines).
- Training: simplified **outcome-based** reward — only accuracy (no format reward, since Qwen-2.5 already adheres to structure).
- Uses GRPO with **loss masking for retrieved tokens**: search-engine output tokens are masked (ignored) so the model can't try to control them.
- Why RL for tools is popular: rewards for tool calling are easy to verify (did it call the right tool with the right args?) → models like Qwen3 and GPT-OSS trained this way.

### 5.5 TinyAgent with Native Tool-Calling Capabilities

- "Magic" of inference engines (Ollama, llama.cpp): converting a standard tool definition to the model's specific chat template (each LLM uses different tokens, e.g., `<think>` vs `<|think|>`).
- Most engines standardize on **JSON** for describing tools.
- With Gemma 4 E4B (trained specifically for tool calling), you pass `tools=[tool_schema]` to the OpenAI-compatible endpoint and the model "just" outputs the tool call (handled by Ollama).
- Under the hood, the engine converts the JSON schema to whatever the LLM was trained on (e.g., `<|tool>`/`<tool|>` tokens for Gemma 4).
- Implementation pieces:
  - **`tool_to_schema(function)`** — converts a Python function to an OpenAI-compatible JSON schema by inspecting name, docstrings, parameters (via `inspect`), with a `TYPE_MAP` (str→string, int→integer, float→number, bool→boolean, list→array, dict→object).
  - **`NativeTools(Tools)`**:
    - `schemas` → list of tool schemas for native calling.
    - `prompt` → empty (inference engine handles it).
    - `parse` → extracts tool name/args from `Response.tool_call["function"]`.
    - `observation` → returns the `"tool"` role (not user).
  - With native calling, memory shows `tool_calls` and a `tool` role message.

### 5.6 Model Context Protocol (MCP)

- **Problem it solves — the N×M problem:** with N LLMs and M tools, you'd need custom integrations for every LLM/tool combination. Also: manual tracking, manual description (JSON schemas), manual updates when APIs change.
- **MCP = the "USB-C port of AI"**: an open standard + framework by **Anthropic** (Nov 2024) that standardizes connecting tools/APIs to LLMs. It facilitates two-way communication between tools and LLMs.
- With MCP: **N + M connections** instead of N×M. Maintenance moves from users to **tool providers** — an API change is fixed once by the maintainer, then rolled out to all users.
- Not the only protocol — e.g., **Agent2Agent (A2A)** standardizes inter-agent communication. (MCP arguably the most popular in 2026.)
- **Three components:**
  1. **MCP host** — the LLM application (Cursor, ChatGPT, Claude, GitHub Copilot) that manages connections, interprets tool schemas, manages routing. The "brain."
  2. **MCP client** — code within the host maintaining one-to-one connections with servers; handles connection management, discovery, request forwarding.
  3. **MCP server** — a lightweight program exposing APIs/tools via the MCP standard (often connects to a data source like arXiv). 
- **Server primitives** (exposed to clients):
  - **tools** — actions the LLM can invoke (e.g., `/list_commits`).
  - **resources** — data the host can load as context (files, docs, datasets).
  - **prompts** — reusable templates.
- **The MCP flow** (12 steps, using "Summarize the 5 latest commits"):
  1. User query → MCP host.
  2. Host asks server (via client) which tools are available.
  3. Server returns the list (e.g., `/list_commits`, `/create_pr`).
  4. Prompt + available tools → LLM.
  5. LLM chooses `/list_commits`.
  6. Client communicates the action to the server.
  7. Server executes the command.
  8. Tool output returned to the server.
  9. Server communicates back to client/host.
  10. LLM receives results; may run another tool or finish.
  11. LLM summarizes the five commits.
  12. Summary returned to the host, then the user.
- Uses **JSON-RPC 2.0** as the message structure; the MCP client is the middleman between the LLM and the protocol.

### 5.7 Skills

- **Problem:** tools/MCP give actions, but not *when* to use them or how they fit a larger workflow (team conventions, preferred tools, coding standards).
- **Skills (Anthropic):** a set of instructions, scripts, and resources the agent uses as **context** to perform a task. They contain **procedural knowledge** — step-by-step instructions for how to approach a task in your specific context.
- Example: a "make a presentation" workflow = search web (tool) → summarize (self) → create slides (tool). The skill is the "recipe."
- **Progressive disclosure** — the agent starts with minimal info and gets more when needed. Three layers:
  - **Layer 1 (always loaded):** metadata (name + short description) — ~100 tokens.
  - **Layer 2 (loaded when activated):** main skill instructions (markdown) — <5,000 tokens.
  - **Layer 3 (loaded when needed):** bundled resources (scripts, references) — as needed.
- **SKILL.md** — every skill requires one:
  - Start = metadata (name, description) in **YAML**; always shared with the agent (often via system prompt) so it can decide relevance.
  - Body = the instructions, only given when the agent asks for it (context engineering: don't load irrelevant long instructions).
- **Bundled resources:** extra files in the skill directory (Python scripts, markdown formats, etc.) referenced from SKILL.md; loaded only when needed.
- Directory example: `meeting_notes/` with `SKILL.md`, `scripts/extract_action_items.py`, `formats/one_on_one_manager.md`, etc.
- **Summary distinction:** tool definitions tell an agent *what a tool does*; Skills tell it *how to get a job done* — the recipe of steps, conventions, and references for a recurring task.

### Chapter 5 Key Takeaways
- LLMs only express intent to call tools; external systems execute.
- Tool usage = create → define → select → call → process output.
- Definition methods: learning (SFT/RL) vs prompting (natural language or JSON schemas).
- Tool learning: in-context learning, SFT (Toolformer), RL (ToolRL, Search-R1).
- MCP standardizes tool integration (host/client/server), solving the N×M problem.
- Skills provide procedural knowledge with progressive disclosure (SKILL.md + bundles).

---

## High-Yield Vocabulary (Ch 5)

| Term | Meaning |
|---|---|
| Tool | Function/API the LLM can invoke (action or information retrieval) |
| Tool registry | Dict mapping tool names to functions |
| Tool definition | Communicating what a tool does to the LLM |
| JSON Schema | Structured tool description (name, params, types, required) |
| Parser / validator | Code that extracts/validates tool calls from LLM output |
| Human-in-the-loop | Requiring human approval before executing certain tools |
| OBSERVATION | User-role message relaying tool output back to the LLM |
| final_answer tool | The tool that stops the agent and returns its final answer |
| In-context learning | Few-shot learning from examples in the prompt |
| Toolformer | SFT model embedding `[call → output]` inline in text |
| TIR | Tool-Integrated Reasoning — tools inside the reasoning trace |
| ToolRL | GRPO framework with correctness + format rewards |
| Search-R1 | RL framework integrating search as a tool (loss masking) |
| Loss masking | Ignoring search-engine output tokens during training |
| Native tool calling | LLM trained with structured tool-call tokens |
| N×M problem | Custom integration needed for every LLM×tool pair |
| MCP | Model Context Protocol (Anthropic) — "USB-C port of AI" |
| MCP host / client / server | App / connector / tool-provider program |
| Skills | Procedural knowledge (SKILL.md) via progressive disclosure |
