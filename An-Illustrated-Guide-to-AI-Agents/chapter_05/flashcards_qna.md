# Flashcards & Q&A — Chapter 5
**Source:** *An Illustrated Guide to AI Agents*, Chapter 5 "Tool Usage, Learning, and Protocols"
**Note:** Split from the combined Ch 5 & 6 deck. Original question numbers are preserved.

## Part 1: Term → Definition (Ch 5)

1. **Tool** → A function or API an LLM can invoke to take an action or retrieve information from its environment.
2. **What is the core limitation of a regular LLM regarding tools?** → It is text-to-text; it can only communicate the *intention* of using a tool, not actually execute it.
3. **What defines an agent in terms of tools?** → The ability to autonomously search for, select, and utilize tools to interact with and influence its environment.
4. **Tool registry** → A dictionary mapping tool names to their functions, so tools can be accessed by a string key.
5. **The five steps of tool usage** → 1) Tool creation, 2) Tool definition, 3) Tool selection, 4) Tool calling, 5) Output processing.
6. **Tool creation** → Writing the actual function (e.g., `def multiply(a: str, b: str) -> float`).
7. **Tool definition** → Communicating to the LLM what the tool does, how it is used, and in which context (via learning or prompting).
8. **Why do tool parameters get passed as strings?** → Because LLMs output only text; values must be converted (e.g., to floats) by the tool.
9. **What is a docstring and why does it matter?** → Documentation embedded in the function; it's often passed automatically to the LLM, so good descriptions help the agent understand edge cases and usage.
10. **The two main ways to define tools for an LLM** → Learning (during training) and prompting (natural language or structured JSON schemas).
11. **Natural-language tool definition** → Describing tools in the system prompt, e.g., "To use a tool, respond with JSON: `{"tool": "name", "kwargs": {...}}`". Requires a parser/validator.
12. **Structured function calling** → Defining each tool as a JSON Schema object (name, description, parameters with types/required fields) passed via a parameter like OpenAI's `tools`.
13. **Three best practices for tool definition** → 1) Document extensively, 2) minimize the number of tools (<10 is a good start), 3) minimize the scope/parameters of each tool.
14. **Tool selection** → The LLM choosing the right tool (or no tool) for a given query.
15. **How does RAG help with many tools?** → Store all tool schemas in a vector database and retrieve the most relevant ones for a query, instead of filling the context window.
16. **Tool calling** → The LLM outputs a string (often JSON) expressing intent to call a tool; an external process actually executes it.
17. **`parse` function** → Extracts the JSON tool call from the model's response text into a structured `Response.tool_call`.
18. **`execute` function** → Looks up the tool in the registry and runs it with the parsed kwargs, returning the result.
19. **Human-in-the-loop for tools** → `requires_approval` list; certain tools require user approval (e.g., `input("Allow? [y/N]")`) before execution.
20. **Two robust alternatives to regex for parsing tool calls** → `jsonschema` (JSON schema validation) and Pydantic (data validation / dataclasses).
21. **Why does the LLM not see the tool run?** → The tool executes outside the LLM's view; you must pretend the LLM executed it by adding the result to the messages.
22. **The `tool` role** → A message role used only by LLMs trained for native tool calling, holding the tool's output.
23. **The `OBSERVATION` tag** → Used with the `user` role for non-native models: `("user", "OBSERVATION: <result>")` to relay tool output.
24. **`is_done` function** → Stopping mechanism: stops if there is no tool call or if the agent calls the `final_answer` tool.
25. **The `final_answer` tool** → The tool an agent calls to signal completion and return its final answer (core to Ch. 6 autonomy).
26. **What problem does SummarizationMemory have with Tools?** → It overwrites the system prompt (to store the summary), removing the tool definitions the agent needs. Fix: update only a section of the system prompt.
27. **What do THOUGHT / ACTION / OBSERVATION / ANSWER stand for?** → The ReAct framework: reasoning, tool calls as JSON, output of actions, and the final answer.
28. **In-context learning (for tools)** → Few-shot prompting: give examples of tool-call format in context so the LLM mimics it, with no fine-tuning.
29. **Fake history trick** → Adding example messages as if the model already called tools, teaching it the format and expected outputs.
30. **Why use SFT for tool calling?** → It distills tool-calling knowledge into the model's weights, avoiding context-window fill and instruction-following fragility.
31. **Toolformer** → An SFT model that embeds tool calls and outputs inline in the generated text using `[`, `→`, `]` tokens.
32. **What does `[` mean in Toolformer?** → Start of a tool call; the model selects the tool and generates parameters in a fixed format.
33. **What does `→` mean in Toolformer?** → The model may stop generating while the user/automated system executes the tool.
34. **What does `]` mean in Toolformer?** → The tool output is inserted into the generated tokens as if the model produced it.
35. **Toolformer input/output example** → Input "What is 5.1 times 7.3?" → Output "5.1 * 7.3 is [Calculator(5.1*7.3) → 37.23] The answer is 37.23."
36. **How was Toolformer's training data created?** → Manual few-shot prompts per tool → sample outputs → filter by correctness of tool use, output, and loss decrease.
37. **Drawback of SFT for tool learning** → Mimicry: sensitive to exact wording/prompts, generalization is hard.
38. **TIR (Tool-Integrated Reasoning)** → Incorporating tools into the LLM's reasoning traces; a trajectory may involve multiple tool invocations.
39. **ToolRL** → A GRPO-based RL framework for tool learning using correctness + format rewards, fine-tuning Qwen2.5 on 4,000 TIR traces.
40. **What tokens does ToolRL use?** → `<thinking></thinking>`, `<tool_call></tool_call>`, `<answer></answer>`.
41. **ToolRL correctness reward** → Scores whether the correct tool names and parameters were used.
42. **ToolRL format reward** → Positive reward if all required fields appear in the correct order.
43. **ToolRL's finding on length rewards** → Longer reasoning traces do not consistently improve tool-use performance and may harm smaller models.
44. **Search-R1** → An efficient RL framework integrating search as a tool into LLM reasoning; the model learns to generate search queries during reasoning.
45. **Search-R1 prompt template** → `<think></think>` (reasoning), `<search></search>` (search call, output via `<information></information>`), `<answer></answer>`; interleaved.
46. **Why did Search-R1 focus on search?** → Because of the popularity of DeepResearch-style agentic systems (reasoning LLM + search engine).
47. **Search-R1 reward design** → Simplified outcome-based reward: only accuracy (no format reward; Qwen-2.5 already adheres well to structure).
48. **Loss masking for retrieved tokens** → Masking the search engine's output tokens during GRPO training so the model can't try to control them.
49. **Why is RL good for tool learning?** → Tool-calling rewards are easy to verify (right tool? right args?), enabling verifiable rewards; used by Qwen3 and GPT-OSS.
50. **Native tool calling** → An LLM trained with structured tool-call tokens; the inference engine converts your JSON schema into the model's chat template.
51. **The "magic" of inference engines (Ollama, llama.cpp)** → Converting standard tool definitions into the specific LLM's chat template tokens (e.g., `<think>` vs `<|think|>`, `<|tool>`/`<tool|>`).
52. **`tool_to_schema` function** → Converts a Python function to an OpenAI-compatible JSON schema by inspecting name, docstring, and parameters.
53. **TYPE_MAP** → Maps Python types to JSON schema types (str→string, int→integer, float→number, bool→boolean, list→array, dict→object).
54. **NativeTools vs Tools** → NativeTools uses native tool calling: `schemas` returns schemas, `prompt` is empty, `parse` reads `Response.tool_call["function"]`, `observation` returns the `tool` role.
55. **The N×M problem** → With N LLMs and M tools, you need custom integrations for every LLM×tool combination.
56. **MCP (Model Context Protocol)** → An open standard + framework by Anthropic (Nov 2024) that standardizes connecting tools and APIs to LLMs; the "USB-C port of AI".
57. **How many connections with MCP?** → N + M instead of N×M; maintenance moves from users to tool providers.
58. **MCP host** → The LLM application (Cursor, ChatGPT, GitHub Copilot) managing connections, interpreting schemas, and routing.
59. **MCP client** → Code within the host maintaining one-to-one connections with MCP servers; handles discovery and request forwarding.
60. **MCP server** → A lightweight program exposing tools, resources, and prompts via the MCP standard (often connected to a data source like arXiv).
61. **Three MCP server primitives** → Tools (actions the LLM can invoke), resources (data loaded as context), prompts (reusable templates).
62. **The MCP flow (summarized)** → Query → host → server lists tools → LLM gets prompt + tools → LLM chooses a tool → client tells server → server executes → output returns through client/host → LLM summarizes → user.
63. **What message structure does MCP use?** → JSON-RPC 2.0.
64. **A2A (Agent2Agent)** → Google's protocol standardizing inter-agent communication (contrast with MCP, which standardizes tool integration).
65. **What do Skills provide?** → Procedural knowledge — step-by-step instructions, scripts, and resources so the agent knows how to do a task in your context.
66. **Skills vs tool definitions** → Tool definitions say *what a tool does*; Skills say *how to get a job done* (the recipe, conventions, references for a recurring task).
67. **Progressive disclosure** → The agent starts with minimal info and loads more only when needed — three layers.
68. **The three layers of progressive disclosure** → Layer 1: metadata (always loaded, ~100 tokens); Layer 2: skill instructions (loaded when activated, <5,000 tokens); Layer 3: bundled resources (loaded when needed).
69. **SKILL.md** → Required file for every skill; starts with YAML metadata (name + description), then the instruction body.
70. **Why is skill metadata always loaded but instructions not?** → Context engineering: long instructions may be irrelevant to the query; the agent asks for them only when relevant.

## Part 3: Short Answer (Ch 5)

121. **Describe the full tool-usage flow (5 steps) with a one-line example.** → Create the function (`multiply`); define it (JSON schema or prompt); the LLM selects it for the query; the LLM outputs a JSON call that an external system executes; the result is fed back as an observation (e.g., "OBSERVATION: 37.23").
122. **Why can't an LLM actually call a tool?** → It is a text-to-text model; it only outputs strings expressing intent. The actual call is executed by the user or external automated software.
123. **Contrast natural-language prompting vs structured function calling for tool definition.** → NL: tools described in prose in the system prompt; format is up to you; needs a custom parser; error-prone. Structured: JSON Schema per tool passed as a parameter; standardized, machine-readable, clearer for the LLM.
124. **Why does too many tools hurt?** → Selection becomes harder and context windows get overloaded with schemas (hurting performance). Use <10 tools and RAG-based tool discovery.
125. **How does human-in-the-loop work in `execute`?** → If the tool name is in `requires_approval`, the code prompts the user (`Allow name? [y/N]`) and returns a denial message if rejected.
126. **Explain the assistant/tool/user roles in output processing.** → `assistant` = the model's tool call; `tool` = tool output (native models only); for non-native, `user` role with "OBSERVATION: result" pretends the user observed the tool call.
127. **What is in-line tool calling (Toolformer) and its benefit?** → Tool call + output embedded directly in the generated text (`[Calc(5.1*7.3) → 37.23]`). Produces fluent language and suits reasoning LLMs (no message round-trips).
128. **Why does RL generalize better than SFT for tools?** → SFT mimics fixed examples (sensitive to wording); RL learns from trial-and-error feedback signals, allowing exploration and better generalization.
129. **Explain ToolRL's reward design.** → Two rewards in GRPO: correctness (right tool names/params) and format (fields in correct order). Trained on 4,000 TIR traces; length rewards found unhelpful.
130. **What is loss masking in Search-R1 and why?** → Mask the search engine's output tokens during GRPO training so the model can't try to control tokens it didn't generate (avoids unexpected dynamics).
131. **Describe the MCP flow for "summarize the 5 latest commits".** → Query → host asks server for tools → server lists them → LLM sees prompt + tools → LLM picks `/list_commits` → client tells server → server executes → output flows back → LLM summarizes → user gets summary.
132. **Why is MCP called the "USB-C port of AI"?** → Universal standard: any LLM can implement any tool that follows the protocol, avoiding custom N×M integrations.
133. **Explain progressive disclosure in Skills with token numbers.** → Layer 1 metadata always loaded (~100 tokens) → Layer 2 instructions on activation (<5,000 tokens) → Layer 3 bundled files on demand (as needed).
134. **What is the difference between a tool, MCP, and a Skill?** → A tool is a function the LLM can call; MCP standardizes how tools are connected/communicated; Skills bundle procedural instructions (with tools inside) for how to accomplish a recurring task.
135. **How does planning relate to tool selection?** → Multi-step tasks require a plan; the plan determines which tools are selected and in what order, and reasoning LLMs think about tool choices.

## Part 4: Fill-in-the-Blank (Ch 5)

155. "An LLM does not actually call the tool but merely shows the **intention** of doing so."
156. "To use a tool, respond with **JSON**: `{"tool": "name", "kwargs": {"param": "value"}}`."
157. "Tools are typically used to access external **knowledge** or **memories** (Chapter 4)."
158. "Aim to minimize the number of tools — using **fewer than 10** tools is a good start."
159. "The **`OBSERVATION`** tag tells the model that the user has observed the tool call."
160. "The **`final_answer`** tool is the only way for a ReAct agent to complete a task."
161. "**MCP** is often referred to as the '**USB-C port of AI**.'"
162. "With MCP, it becomes **N + M** connections instead of **N×M**."
163. "MCP servers expose three kinds of primitives: **tools**, **resources**, and **prompts**."
164. "Every skill is required to have a **SKILL.md** file."
165. "Skills contain **procedural knowledge** — step-by-step instructions."
