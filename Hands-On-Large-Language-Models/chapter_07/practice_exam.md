# 📘 Practice Exam — Chapter 7: Advanced Text Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 7
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What is a GGUF model in this chapter?
   a) A fine-tuned variant of a model
   b) A compressed (quantized) version of the original model
   c) A tokenizer for the model
   d) A larger context-window version

2. What does quantization do to an LLM's parameters?
   a) Increases the number of bits per parameter
   b) Adds more layers to the network
   c) Reduces the number of bits needed to represent them
   d) Converts parameters to float 32-bit

3. What is the rule of thumb for quantization level?
   a) At least 4-bit
   b) Always 2-bit
   c) At least 16-bit
   d) 1-bit for all models

4. What does FP16 in the filename `Phi-3-mini-4k-instruct-fp16.gguf` indicate?
   a) The model has 16 layers
   b) A 16-token context window
   c) The model uses 16-bit precision for activations only
   d) It is the 16-bit variant of the model

5. In `LlamaCpp`, what does `n_gpu_layers=-1` mean?
   a) No layers run on the GPU
   b) All layers of the model run from the GPU
   c) Only the last layer runs on the GPU
   d) Layers are automatically quantized

6. Why does `llm.invoke("Hi! My name is Maarten. What is 1 + 1?")` return no output for Phi-3?
   a) The model is too small
   b) The seed is fixed at 42
   c) Phi-3 requires its specific prompt template, which LangChain does not apply automatically
   d) The context window is too small

7. Which set lists the four components of Phi-3's prompt template?
   a) `<s>`, `<|user|>`, `<|assistant|>`, `<|end|>`
   b) `<s>`, `<|system|>`, `<|user|>`, `<|end|>`
   c) `<|start|>`, `<|user|>`, `<|assistant|>`, `<|stop|>`
   d) `<s>`, `<|user|>`, `<|assistant|>`, `<|eos|>`

8. How is a basic LangChain chain created from a prompt and an LLM?
   a) `prompt.add(llm)`
   b) `LLMChain(prompt, llm)`
   c) `prompt + llm`
   d) `prompt | llm`

9. Which input variable does the basic `basic_chain` expect in `invoke()`?
   a) `{product}`
   b) `input_prompt`
   c) `chat_history`
   d) `{summary}`

10. Which is NOT one of the four main components of Phi-3's template?
    a) `<s>`
    b) `<|user|>`
    c) `<|system|>`
    d) `<|end|>`

11. The reusable business-name prompt defines which variable in its template?
    a) `{product}`
    b) `{name}`
    c) `{business}`
    d) `{input_prompt}`

12. In an `LLMChain`, what names the chain's output for later use by other chains?
    a) `input_variables`
    b) `template`
    c) `memory_key`
    d) `output_key`

13. In sequential chains, the output of one prompt is:
    a) Discarded after each call
    b) Used as the input for the next prompt
    c) Stored only in memory
    d) Sent to the tokenizer

14. What is an advantage of decomposing a complex prompt into sequential subtasks?
    a) Fewer total LLM calls
    b) No prompt template needed
    c) Individual components (e.g., the title) can be easily extracted
    d) Deterministic output guaranteed

15. LLMs used "out of the box" are described as:
    a) Stateless — no memory of previous conversations
    b) Stateful by default
    c) Unusable without fine-tuning
    d) Always deterministic

16. How does ConversationBufferMemory remind the LLM of the past?
    a) It fine-tunes the model on the conversation
    b) It summarizes the conversation into key points
    c) It trains a new embedding
    d) It appends the entire conversation history to the input prompt

17. What `memory_key` is used in the chapter's conversation-buffer examples?
    a) `memory`
    b) `chat_history`
    c) `history`
    d) `conversation`

18. What are the output keys of a chain using ConversationBufferMemory?
    a) `input`, `output`, `memory`
    b) `question`, `answer`, `history`
    c) `input_prompt`, `chat_history`, `text`
    d) `prompt`, `llm`, `text`

19. How does LangChain store conversation turns internally?
    a) As `Human:` and `AI:` interactions
    b) As `User:` and `Model:` JSON blocks
    c) As a single concatenated string without roles
    d) As a separate database table

20. `ConversationBufferWindowMemory(k=2)` retains:
    a) The entire conversation history
    b) Only the first 2 conversations
    c) A summary of all conversations
    d) Only the last 2 conversations

21. In the windowed-buffer demo, after three conversations the model no longer knows the age because:
    a) Summarization removed it
    b) The first conversation fell outside the last-k window
    c) The name overwrote it
    d) The token limit was exceeded

22. What enables the summarization in ConversationSummaryMemory?
    a) The tokenizer
    b) A retrieval system
    c) Another LLM given the conversation history as input
    d) A rule-based algorithm

23. How many LLM calls occur for each question with ConversationSummaryMemory?
    a) Two — the user prompt and the summarization prompt
    b) One — only the user prompt
    c) Three — prompt, summary, verification
    d) None — memory is local only

24. The summary prompt template uses which variables?
    a) `{current}` and `{conversation}`
    b) `{history}` and `{input_prompt}`
    c) `{summary}` and `{chat_history}`
    d) `{summary}` and `{new_lines}`

25. How do you retrieve the most recent summary from ConversationSummaryMemory?
    a) `memory.summary()`
    b) `memory.load_memory_variables({})`
    c) `memory.get_summary()`
    d) `memory.read()`

26. What is a downside of ConversationSummaryMemory?
    a) It loses the last k conversations
    b) It requires a large-context LLM
    c) An additional call is necessary for each interaction
    d) It cannot capture the full history

27. Per Table 7-1, which is a PRO of ConversationBufferMemory?
    a) Easiest implementation
    b) Reduces tokens needed to capture full history
    c) Large-context LLMs not needed
    d) Enables long conversations

28. Per Table 7-1, which is a CON of Windowed Conversation Buffer?
    a) Slower generation speed
    b) Quality reliant on summarization
    c) Information loss within context window
    d) Only captures the last k interactions

29. Per Table 7-1, which is a PRO of Conversation Summary memory?
    a) Easiest implementation
    b) Captures the full history
    c) No additional calls
    d) Retains every word verbatim

30. What do agents in this chapter determine for themselves?
    a) The prompt template to use
    b) The memory type to apply
    c) Which actions they should take and in what order
    d) Which model to load

31. What are the two vital components agents add on top of model I/O, chains, and memory?
    a) Tools and the agent type
    b) Prompt templates and output keys
    c) Memory and embeddings
    d) Retrieval and fine-tuning

32. What does ReAct stand for?
    a) Reading and Acting
    b) Reacting and Adapting
    c) Reasoning and Automating
    d) Reasoning and Acting

33. What are the three iterative steps of ReAct?
    a) Plan, Execute, Review
    b) Thought, Action, Observation
    c) Prompt, Generate, Validate
    d) Question, Answer, Verify

34. What is an "Action" in ReAct generally?
    a) A prompt template
    b) A memory update
    c) An external tool such as a calculator or search engine
    d) A fine-tuning step

35. Why does the chapter use OpenAI's GPT-3.5 for the agent example instead of Phi-3?
    a) The small Phi-3 is not sufficient to follow complex instructions
    b) GPT-3.5 is open source
    c) Phi-3 cannot use tools at all
    d) GPT-3.5 is smaller and faster

36. Which tools does the chapter's agent use?
    a) Wikipedia and Google Scholar
    b) SQL and a database
    c) Weather API and file system
    d) DuckDuckGo search and an llm-math calculator

37. What are the input variables of the ReAct template?
    a) `tools`, `question`, `thought`, `answer`
    b) `tools`, `tool_names`, `input`, `agent_scratchpad`
    c) `input_prompt`, `chat_history`, `text`
    d) `summary`, `title`, `character`

38. What does the AgentExecutor do?
    a) Downloads the GGUF model
    b) Summarizes the conversation
    c) Handles executing the agent's steps (the Thought/Action/Observation loop)
    d) Balances the memory types

39. In the agent example, what is the exchange rate used to convert USD to EUR?
    a) 0.85 EUR for 1 USD
    b) 0.95 EUR for 1 USD
    c) 1.15 EUR for 1 USD
    d) 1.05 EUR for 1 USD

40. What is the "double-edged sword" of agents mentioned in the chapter?
    a) They use too much VRAM
    b) They require multiple API keys
    c) They are slower than chains
    d) There is no human in the loop to judge output quality

---

## Section B: True/False (1 point each)

41. Quantization reduces the number of bits per parameter while trying to keep most original information. (T/F)
42. Using 3-bit or 2-bit quantized models is recommended as the best balance of compression and accuracy. (T/F)
43. An 8-bit variant of Phi-3 cuts memory requirements almost in half compared to the 16-bit variant. (T/F)
44. transformers.pipeline automatically applies the chat template, but LangChain does not. (T/F)
45. The `<|end|>` token indicates when the prompt or output ends. (T/F)
46. `prompt | llm` chains the prompt template and the LLM into a single LangChain chain. (T/F)
47. In sequential chains, each link requires input directly from the user. (T/F)
48. LLMs are stateful by default and remember previous conversations. (T/F)
49. ConversationBufferMemory appends the entire conversation history to the input prompt. (T/F)
50. With `ConversationBufferWindowMemory(k=2)`, only the last two conversations are retained. (T/F)
51. ConversationSummaryMemory requires an additional LLM call for each interaction. (T/F)
52. ConversationSummaryMemory stores the original conversation verbatim in the chat history. (T/F)
53. Per Table 7-1, ConversationBufferMemory is the easiest memory type to implement. (T/F)
54. Agents follow a fixed, user-defined set of steps like chains do. (T/F)
55. ReAct allows reasoning to affect acting and actions to affect reasoning. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. What is quantization, what trade-off does it involve, and what is the recommended minimum bit level?
57. Explain why `llm.invoke()` returned no output in LangChain and how chains solve this for Phi-3.
58. What are the four components of Phi-3's prompt template and what does each do?
59. Describe how the sequential story chain works (title → character → story) and one advantage of this decomposition.
60. What does it mean for an LLM to be stateless, and how does ConversationBufferMemory make it stateful?
61. Compare ConversationBufferWindowMemory and ConversationSummaryMemory: how each works, their trade-offs, and when each is preferable.
62. List the pros and cons of the three memory types from Table 7-1.
63. Define agents and name the two vital components that extend model I/O, chains, and memory.
64. Explain the ReAct framework: what it stands for, its three iterative steps, and the MacBook Pro example.
65. Why does the chapter use GPT-3.5 for the agent example, and what reliability concerns ("double-edged sword") does it raise?

---

## Section D: Essay / Applied (5 points each)

66. **Quantization and Model I/O.** Explain how the chapter loads Phi-3 as a quantized model: what quantization is (bits, precision, memory), the rule of thumb for bit level, the GGUF file used (FP16), the `LlamaCpp` loading parameters (`n_gpu_layers=-1`, `max_tokens`, `n_ctx`, `seed`, `verbose`), why the raw `invoke()` returned no output, and the ChatOpenAI fallback.
67. **Chains and prompt templates.** Describe how LangChain chains work: the basic `prompt | llm` chain with Phi-3's template (`<s>`, `<|user|>`, `<|assistant|>`, `<|end|>`), reusable prompt templates (the `{product}` business-name example), and sequential multi-prompt chains using `LLMChain` with `output_key` (the story title → character → story example). Include why decomposition is beneficial.
68. **Memory systems.** Contrast the three memory types: ConversationBufferMemory (append full history; `chat_history` key; Human/AI turns), ConversationBufferWindowMemory (last k interactions; the age-vs-name k=2 example), and ConversationSummaryMemory (LLM summarization with `{summary}`/`{new_lines}`; two calls per interaction; `load_memory_variables`). Reference Table 7-1 pros/cons and the speed/memory/accuracy trade-off.
69. **Agents and ReAct.** Define agents (LLM deciding which actions to take; tools + agent type; self-correcting roadmaps). Explain ReAct (Reasoning and Acting; Thought/Action/Observation loop; why acting requires tools). Walk through the LangChain implementation: loading GPT-3.5, the ReAct template (tools, tool_names, input, agent_scratchpad), creating DuckDuckGo and llm-math tools, `create_react_agent` and `AgentExecutor`, and the MacBook Pro USD→EUR output.
70. **Agent reliability and next steps.** Discuss the "double-edged sword" of autonomous agents (no human in the loop), reliability-improving system-design suggestions (return source URL, verify at each step), and how the chapter's techniques (Model I/O, chains, memory, agents) combine into LLM-based systems. Connect to the next chapter's topic (improving/build new search systems, i.e., retrieval).

---

## ANSWER KEY

### Section A: Multiple Choice
1. b
2. c
3. a
4. d
5. b
6. c
7. a
8. d
9. b
10. c
11. a
12. d
13. b
14. c
15. a
16. d
17. b
18. c
19. a
20. d
21. b
22. c
23. a
24. d
25. b
26. c
27. a
28. d
29. b
30. c
31. a
32. d
33. b
34. c
35. a
36. d
37. b
38. c
39. a
40. d

### Section B: True/False
41. **T** — Quantization reduces bits per parameter while attempting to maintain most original information.
42. **F** — 4-bit is recommended; 3-bit/2-bit shows noticeable degradation (prefer a smaller higher-precision model).
43. **T** — 8-bit cuts memory requirements almost in half vs 16-bit.
44. **T** — transformers.pipeline applies the chat template automatically; LangChain requires an explicit template.
45. **T** — `<|end|>` marks the end of the prompt or the model's output.
46. **T** — The pipe operator creates the chain.
47. **F** — Only the first link (title) requires user input; later links use earlier outputs.
48. **F** — LLMs are stateless by default; memory must be added.
49. **T** — ConversationBufferMemory appends the full history to the input prompt.
50. **T** — Windowed buffer retains only the last k interactions.
51. **T** — Each interaction triggers both a user prompt call and a summarization call.
52. **F** — It stores a summary; the original question must be inferred from context.
53. **T** — Per Table 7-1, easiest implementation is a pro of Conversation Buffer.
54. **F** — Agents determine their own actions (self-correcting roadmap); chains follow user-defined steps.
55. **T** — ReAct merges reasoning and acting so each affects the other.

### Section C: Short Answer (model answers)
56. **Quantization** reduces the bits per parameter while keeping most original information. Trade-off: some precision loss in exchange for faster inference, less VRAM, and near-original accuracy (analogy: "14:16" vs "14:16 and 12 seconds"). Rule of thumb: at least 4-bit (good compression/accuracy balance); below that, prefer a smaller higher-precision model.
57. **No output** because Phi-3 requires its chat template, which LangChain does not apply automatically (unlike transformers.pipeline). Chains solve this: wrap the template in a `PromptTemplate` (`<s><|user|>{input_prompt}<|end|><|assistant|>`) and chain it with the LLM via `prompt | llm`, then `invoke({"input_prompt": ...})`.
58. **Four components:** `<s>` (prompt start), `<|user|>` (start of user prompt), `<|assistant|>` (start of model output), `<|end|>` (end of prompt or output).
59. **Story chain:** title link uses `{summary}` (`output_key="title"`); character link uses `{summary}`+`{title}` (`output_key="character"`); story link uses all three (`output_key="story"`); combined as `title | character | story`. Advantage: one short user input; each component is individually extractable (e.g., the title).
60. **Stateless** means no memory of previous conversations (tell it your name, then it can't recall it). ConversationBufferMemory makes it stateful by appending the entire conversation history (key `chat_history`) into the prompt, stored as Human/AI turns.
61. **Windowed buffer** keeps only the last k interactions — smaller prompts, no info loss over the window, but older info is lost (e.g., k=2 drops the first conversation and the age). **Summary memory** uses another LLM to distill the whole history into main points — captures full history with fewer tokens, enables long conversations, but adds a call per interaction and depends on summarization quality. Use windowed for limited token budgets where recent context matters; use summary for long conversations needing full-history awareness.
62. **Conversation Buffer:** Pros — easiest implementation, no info loss within context window; Cons — slower as tokens grow, needs large-context LLM, retrieval difficult with large histories. **Windowed Buffer:** Pros — no large-context LLM needed unless history is huge, no loss over last k; Cons — only last k captured, no compression of last k. **Conversation Summary:** Pros — captures full history, enables long conversations, fewer tokens; Cons — extra call per interaction, quality depends on the summarizer LLM.
63. **Agents** leverage an LLM to determine which actions to take and in what order; they build on model I/O, chains, and memory. The two vital extra components are **tools** (things the agent can use that it couldn't do itself) and the **agent type** (plans the actions/tools).
64. **ReAct = Reasoning and Acting** (Yao et al., 2022). Steps: Thought (what to do and why) → Action (external tool, e.g., calculator or search) → Observation (result returned to the LLM), repeatable N times, ending in a Final Answer. MacBook Pro example: search the web for the USD price, then use a calculator to convert to EUR at the given exchange rate.
65. **GPT-3.5 used** because the small Phi-3 is insufficient for complex agent instructions (larger local LLMs need far more compute/VRAM). Reliability concern: agents are autonomous — no human in the loop to judge output/reasoning quality; mitigation via careful design (return source URLs, verify each step).

### Section D: Essay (grading notes)
66. **Expect** quantization definition (fewer bits per parameter, precision vs speed/VRAM, near-equal accuracy; time analogy; 8-bit ≈ half memory vs 16-bit; ≥4-bit rule of thumb); GGUF format; FP16 = 16-bit variant file `Phi-3-mini-4k-instruct-fp16.gguf`; `LlamaCpp` params (n_gpu_layers=-1 all layers on GPU, max_tokens=500, n_ctx=2048, seed=42, verbose=False); empty `invoke()` due to missing template; `ChatOpenAI(openai_api_key="MY_KEY")` fallback.
67. **Expect** basic chain `prompt | llm`; Phi-3 template tokens (`<s>`, `<|user|>`, `<|assistant|>`, `<|end|>`); `PromptTemplate` with `input_variables`; reusable `{product}` business-name template; `LLMChain` with `output_key`; sequential story chain (title from summary; character from summary+title; story from summary+title+character; `title | character | story`); benefits (single user input, individual component extraction).
68. **Expect** statelessness and the memory goal; buffer = full history appended (memory_key chat_history, Human/AI turns, output keys input_prompt/chat_history/text); windowed = last k (age-vs-name k=2 example); summary = LLM summarizes via `{summary}`/`{new_lines}` template, two calls per interaction, `load_memory_variables({})`, inference of non-verbatim details; Table 7-1 pros/cons; speed/memory/accuracy trade-off (buffer instant but token-hungry; summary slow but token-efficient).
69. **Expect** agent definition (LLM decides actions/order); tools + agent type; self-correcting roadmap; ReAct meaning and Thought/Action/Observation loop (repeatable N times); why LLMs need tools to act; implementation: `ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)`, ReAct template variables (`tools`, `tool_names`, `input`, `agent_scratchpad`), DuckDuckGo tool (`Tool(name="duckduck", ...)`) + `load_tools(["llm-math"])`, `create_react_agent`, `AgentExecutor(verbose=True, handle_parsing_errors=True)`; MacBook output ($2,249.00 → ~1,911.65 EUR at 0.85).
70. **Expect** double-edged sword: autonomous behavior without human-in-the-loop quality checking; reliability design (return source URL, ask for correctness at each step); how Model I/O + chains + memory + agents combine into LLM-based systems (techniques strongest combined); next chapter: using LLMs to improve existing search systems and become the core of new, more powerful search systems (retrieval).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely quantization/GGUF, the memory types and Table 7-1, or the ReAct agent flow).
- **<70%**: Re-read the chapter and study notes, then retake Sections A and B before attempting C and D again.
