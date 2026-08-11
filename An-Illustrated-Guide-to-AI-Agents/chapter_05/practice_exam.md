# AI Agents — Practice Exam (Chapter 5)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 5 "Tool Usage, Learning, and Protocols"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.
**Note:** Split from the combined Ch 5 & 6 exam. Original question numbers are preserved.

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

---

## Section D: Essay / Applied (5 points each)

101. **The five-step tool-usage lifecycle.** Explain each step (creation, definition, selection, calling, output processing) with the `multiply` example. Include: registry, JSON parsing, OBSERVATION, human-in-the-loop, and why the LLM only expresses intent.

102. **Tool learning methods.** Compare in-context learning, SFT (Toolformer), and RL (ToolRL, Search-R1). Cover the training data challenges of SFT, why RL generalizes better, the specific rewards in ToolRL, and Search-R1's prompt template + loss masking.

103. **MCP and Skills.** Explain the N×M problem, the three MCP components and primitives, the MCP flow, and how MCP solves standardization. Then explain Skills, SKILL.md, and progressive disclosure, and contrast tools/MCP/Skills.

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

### Section D: Essay (grading notes)
101. Expect: all five steps with `multiply`; registry dict; LLM outputs JSON intent only; parse/execute split; human-in-the-loop; OBSERVATION + final_answer; why the LLM never sees the execution.
102. Expect: in-context (few-shot, fake history); SFT (Toolformer tokens `[ → ]`, data filtering by correctness/output/loss decrease, mimicry limitation, data-quality caveats); RL (TIR, ToolRL rewards + 4,000 traces + length finding, Search-R1 template + accuracy-only reward + loss masking).
103. Expect: N×M → N+M; host/client/server; tools/resources/prompts; JSON-RPC 2.0; the 12-step flow; provider-side maintenance. Skills: procedural knowledge; SKILL.md (YAML metadata + instructions); three-layer progressive disclosure with token budgets; tools vs MCP vs Skills.

---

## SCORING GUIDE
- Section A: 28 questions × 1 point = 28 points
- Section B: 12 questions × 1 point = 12 points
- Section C: 14 questions × 2–3 points = 28–42 points
- Section D: 3 essays × 5 points = 15 points
- **Total: 83–97 points.** Scale to percentage as needed.
