# 📘 Chapter 7 Study Bundle: Advanced Text Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 7

---

## §1. Study Notes

### Core Theme
This chapter continues the previous chapter's approach of improving LLM output without fine-tuning. It introduces the LangChain framework and walks through four modular techniques that are the foundation of many LLM systems: **Model I/O** (loading quantized/GGUF models), **Chains** (prompt templates and sequential multi-prompt chains), **Memory** (helping LLMs remember conversations via conversation buffers, windowed buffers, and conversation summaries), and **Agents** (LLM-driven systems that choose and use external tools, powered by the ReAct framework of Thought/Action/Observation). The chapter ends by noting that retrieval/search is discussed in the next chapter.

### Key Definitions
- **LangChain:** One of the earlier frameworks that simplifies working with LLMs through useful abstractions; its core method is "chains." Newer frameworks of note: DSPy and Haystack.
- **GGUF:** A file format representing a compressed version of a model produced through quantization; loaded here with `llama-cpp-python` + LangChain.
- **Quantization:** A method that reduces the number of bits needed to represent the parameters of an LLM while attempting to maintain most of the original information; costs some precision but makes the model faster, smaller (less VRAM), and often nearly as accurate.
- **Bits:** A series of 0s and 1s that represent values by encoding them in binary form; more bits = wider range of values but more memory needed to store them.
- **FP16:** The 16-bit variant of the model (as in the filename `Phi-3-mini-4k-instruct-fp16.gguf`).
- **8-bit quantization:** The chapter mentions an 8-bit variant of Phi-3 compared to the original 16-bit variant, cutting memory requirements almost in half.
- **Chain:** LangChain's main method; the most basic form connects an LLM with some additional tool, prompt, or feature; multiple chains can be connected together.
- **Prompt template:** A reusable prompt with variables (e.g., `{input_prompt}`, `{product}`, `{summary}`) that is chained to the LLM instead of copy-pasting the template each time.
- **LLMChain:** A LangChain class used to link an LLM and a prompt (with optional memory and output key); in this chapter it is used for sequential multi-prompt chains.
- **Sequential chains:** Linking chains where each link handles a subtask; the output of one prompt becomes the input of the next.
- **output_key:** A named key used to store a chain's output (e.g., `output_key="title"`), which later chains can reference.
- **Stateless:** LLMs by default have no memory of previous conversations; they are "stateless."
- **Stateful:** Making an LLM remember conversations by adding memory to the chain.
- **ConversationBufferMemory:** LangChain memory that appends the full conversation history to the input prompt.
- **ConversationBufferWindowMemory:** Memory that retains only the last k conversations instead of the full history.
- **ConversationSummaryMemory:** Memory that summarizes an entire conversation history into its main points using another LLM.
- **Agents:** Systems that leverage a language model to determine which actions they should take and in what order; they use everything seen so far (model I/O, chains, memory) plus tools and an agent type.
- **Tools:** Things an agent can use to do things it could not do itself (e.g., web search, calculator/llm-math).
- **Agent type:** The component that plans the actions to take or tools to use.
- **ReAct (Reasoning and Acting):** A framework (Shunyu Yao et al., "ReAct: Synergizing reasoning and acting in language models," arXiv:2210.03629, 2022) that combines reasoning and acting; iteratively follows three steps: Thought, Action, Observation.
- **Thought:** The LLM's reasoning about what it thinks it should do next and why.
- **Action:** An action triggered based on the thought, generally an external tool (calculator, search engine).
- **Observation:** The result of the action returned to the LLM; often a summary of whatever result was retrieved.
- **AgentExecutor:** A LangChain component that handles executing the steps of an agent (running the Thought/Action/Observation loop).
- **Agent scratchpad:** A variable (`agent_scratchpad`) in the ReAct template holding the intermediate thoughts/actions/observations generated so far.
- **Human-in-the-loop:** The chapter notes that agents create relatively autonomous behavior, so there is no human in the loop to judge the quality of the output or reasoning process.

### Core Concepts & Frameworks
- **Model I/O — loading quantized models:** Use a GGUF variant of Phi-3 (quantized). Quantization reduces bits per parameter while keeping most information; analogy: giving the time as "14:16" (correct) rather than "14:16 and 12 seconds" (more precise) — removing seconds retains the vital info (hours/minutes). Rule of thumb: look for at least 4-bit quantized models (good balance of compression/accuracy); 3-bit or 2-bit shows noticeable degradation — prefer a smaller model with higher precision instead.
- **Downloading the model:** `wget https://huggingface.co/microsoft/Phi-3-mini-4k-instruct-gguf/resolve/main/Phi-3-mini-4k-instruct-fp16.gguf`. The link contains multiple files with different bit-variants; FP16 = the 16-bit variant.
- **Loading with llama-cpp-python + LangChain:** `from langchain import LlamaCpp; llm = LlamaCpp(model_path="Phi-3-mini-4k-instruct-fp16.gguf", n_gpu_layers=-1, max_tokens=500, n_ctx=2048, seed=42, verbose=False)`. Use `llm.invoke(...)` to generate.
- **No output by default:** `llm.invoke("Hi! My name is Maarten. What is 1 + 1?")` returns empty — Phi-3 requires a specific prompt template; unlike transformers.pipeline (which applies the chat template automatically), with LangChain we must explicitly use the template ourselves (via chains).
- **All examples can run with any LLM:** Phi-3 is the default, but ChatGPT, Llama 3, or anything else works; the Open LLM Leaderboard ranks open source LLMs. If no local GPU device, use `from langchain.chat_models import ChatOpenAI; chat_model = ChatOpenAI(openai_api_key="MY_KEY")`.
- **Chains — prompt template:** The Phi-3 template has four components: `<s>` (start of prompt), `<|user|>` (start of user prompt), `<|assistant|>` (start of model output), `<|end|>` (end of prompt or output). Build: `template = """<s><|user|>\n{input_prompt}<|end|>\n<|assistant|>"""`; `prompt = PromptTemplate(template=template, input_variables=["input_prompt"])`. Create a chain with the pipe operator: `basic_chain = prompt | llm`; call with `basic_chain.invoke({"input_prompt": "..."})`. Only the user/input prompts need to be defined; the template is constructed for you.
- **Note:** The template requirement is not universal — OpenAI's GPT-3.5 API handles the underlying template itself.
- **Reusable prompt templates:** Define other changing variables, e.g., `template = "Create a funny name for a business that sells {product}."` with `input_variables=["product"]` — avoids retyping the question for different products.
- **Chains — multiple prompts (sequential):** Break a complex prompt into smaller subtasks run sequentially (requires multiple LLM calls with smaller prompts and intermediate outputs). Example: generate a story with three components (title, character description, summary). Use `from langchain import LLMChain`. First link (only one needing user input): title from `{summary}` with `output_key="title"`. Second link: character from `{summary}` + `{title}` with `output_key="character"`. Third link: story from `{summary}` + `{title}` + `{character}` with `output_key="story"`. Combine: `llm_chain = title | character | story`; run with `llm_chain.invoke("a girl that lost her mother")` — returns all three components. Advantage: individual components (e.g., the title) can be extracted easily, which would be hard with a single prompt.
- **Memory — statelessness:** Out of the box LLMs forget what was said (tell it your name, then ask "What is my name?" → it doesn't know). Models are stateless — no memory of any previous conversation. Add memory types to the chain to make them stateful.
- **ConversationBufferMemory:** Append the entire conversation history to the input prompt. Update the prompt to hold chat history: `template = """<s><|user|>Current conversation:{chat_history}\n{input_prompt}<|end|>\n<|assistant|>"""` with `input_variables=["input_prompt", "chat_history"]`. Then `memory = ConversationBufferMemory(memory_key="chat_history")` and `llm_chain = LLMChain(prompt=prompt, llm=llm, memory=memory)`. Output dict contains `input_prompt`, `chat_history`, and `text` (the generated text). Under the hood LangChain saves interactions as `Human:` (you) and `AI:` (the LLM). First call: empty chat_history; after that, history accumulates; the LLM can then recall the name.
- **Windowed conversation buffer:** As conversations grow, the prompt grows toward the token limit. Use `ConversationBufferWindowMemory(k=2, memory_key="chat_history")` to keep only the last k conversations. Example: after three conversations with k=2, the first interaction (and the age given in it) is forgotten while the name is still remembered. Trade-off: reduces size but only retains the last few conversations — not ideal for lengthy conversations.
- **ConversationSummaryMemory:** Summarizes the whole conversation history into main points using another LLM. Whenever we ask a question, there are two calls: the user prompt + the summarization prompt. A smaller LLM can be used for summarization to speed things up (though the chapter uses the same LLM). Build a summary prompt template with `{summary}` and `{new_lines}` variables ("Summarize the conversations and update with the new lines... New summary:"). Use `memory = ConversationSummaryMemory(llm=llm, memory_key="chat_history", prompt=summary_prompt)`. Each step summarizes the conversation; `memory.load_memory_variables({})` retrieves the most recent summary. Caveats: since the original question isn't explicitly saved, the model must infer specific information from the summary; multiple calls to the LLM slow down computing time. Trade-off between speed, memory, and accuracy.
- **Table 7-1 memory comparison:** (1) Conversation Buffer — Pros: easiest implementation, ensures no information loss within the context window; Cons: slower generation speed as more tokens are needed, only suitable for large-context LLMs, larger histories make information retrieval difficult. (2) Windowed Conversation Buffer — Pros: large-context LLMs not needed unless history is large, no information loss over the last k interactions; Cons: only captures the last k interactions, no compression of the last k interactions. (3) Conversation Summary — Pros: captures the full history, enables long conversations, reduces tokens needed to capture full history; Cons: an additional call is necessary for each interaction, quality is reliant on the LLM's summarization capabilities.
- **Agents — concept:** Systems that leverage an LLM to determine which actions to take and in what order. Agents extend model I/O, chains, and memory with two vital components: tools (things the agent can use that it couldn't do itself) and the agent type (plans the actions/tools). Unlike chains (user-defined steps), agents can create and self-correct a roadmap to achieve a goal and interact with the real world via tools. Example: LLMs are bad at math, so give them a calculator; add dozens of tools (search engine, weather API) and capabilities increase significantly. Agents can be powerful general problem solvers.
- **ReAct:** Combines reasoning (LLMs are powerful at reasoning) and acting (LLMs can only generate text, so they must be instructed to use specific queries to trigger tools like a weather forecasting API). ReAct allows reasoning to affect acting and actions to affect reasoning. Iterative loop: Thought → Action → Observation (can repeat N times) → final answer. Example: on holiday in the US wanting a MacBook Pro price converted to EUR — agent searches the web for current price, then uses a calculator to convert USD→EUR given the exchange rate. The agent describes its thoughts, actions, and observations during the cycle.
- **ReAct in LangChain:** The small local LLM is insufficient for agents (needs to follow complex instructions), so use OpenAI's GPT-3.5: `from langchain_openai import ChatOpenAI; os.environ["OPENAI_API_KEY"] = "MY_KEY"; openai_llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)`. Larger local LLMs exist but need significantly more compute/VRAM; within a model family, increasing size leads to better performance. The ReAct template uses variables `{tools}`, `{tool_names}`, `{input}`, `{agent_scratchpad}` and the format Question/Thought/Action/Action Input/Observation/.../Final Answer.
- **Tools:** `from langchain.agents import load_tools, Tool; from langchain.tools import DuckDuckGoSearchResults`. Create `search = DuckDuckGoSearchResults()`; wrap in `Tool(name="duckduck", description="A web search engine...", func=search.run)`. Combine tools: `tools = load_tools(["llm-math"], llm=openai_llm)` (a calculator) then `tools.append(search_tool)`.
- **Agent executor:** `from langchain.agents import AgentExecutor, create_react_agent; agent = create_react_agent(openai_llm, tools, prompt); agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True, handle_parsing_errors=True)`. Invoke with `agent_executor.invoke({"input": "What is the current price of a MacBook Pro in USD? How much would it cost in EUR if the exchange rate is 0.85 EUR for 1 USD."})`. Intermediate steps (visible via verbose) let you debug and check tool usage; the final output: "$2,249.00 USD ... approximately 1911.65 EUR with an exchange rate of 0.85 EUR for 1 USD."
- **Double-edged sword:** Agents produce relatively autonomous behavior — no human in the loop to judge output/reasoning quality. Improve reliability via careful system design: e.g., have the agent return the website's URL where it found the price, or ask whether the output is correct at each step.
- **Next chapter:** Using LLMs to improve existing search systems and become the core of new, more powerful search systems (retrieval — Figure 7-1 notes retrieval is discussed next chapter).

### Important Numbers / Stats / Tokens
- Phi-3 GGUF variant used; FP16 = 16-bit variant (p.3). 8-bit variant cuts memory almost in half vs 16-bit (p.3).
- Rule of thumb: at least 4-bit quantized models; avoid 3-bit/2-bit (noticeable degradation; prefer smaller model with higher precision) (p.3).
- LlamaCpp params: `n_gpu_layers=-1` (all layers on GPU), `max_tokens=500`, `n_ctx=2048`, `seed=42`, `verbose=False` (p.4).
- Model file: `Phi-3-mini-4k-instruct-fp16.gguf` from `microsoft/Phi-3-mini-4k-instruct-gguf` (p.4).
- Phi-3 template tokens: `<s>`, `<|user|>`, `<|assistant|>`, `<|end|>` (p.6).
- Basic chain output: "The answer to 1 + 1 is 2." (p.7).
- Story example: summary = "a girl that lost her mother" → title `"Whispers of Loss: A Journey Through Grief"` (first run) / `"In Loving Memory: A Journey Through Grief"` (chained run) (p.9-10).
- Windowed buffer example: `k=2` → after three conversations, the first (age=33) is forgotten but the name is remembered (p.14-15).
- Table 7-1: three memory types with pros/cons (p.19).
- ReAct reference: Shunyu Yao et al. "ReAct: Synergizing reasoning and acting in language models." arXiv:2210.03629 (2022) (p.21).
- Agent model: OpenAI GPT-3.5 (`gpt-3.5-turbo`), `temperature=0` (p.23).
- Agent tools: DuckDuckGo search + `llm-math` calculator (p.24).
- Agent example result: MacBook Pro $2,249.00 USD → ~1,911.65 EUR at 0.85 EUR/USD (p.25).

### Algorithms & Formulæ
- **Quantization:** Reduce the number of bits per parameter (e.g., 16-bit → 8-bit) while keeping the vital information; trade precision for speed/VRAM; ~4-bit is the recommended balance.
- **Chain composition:** `prompt | llm` (single chain); `title | character | story` (sequential chain) — outputs flow forward, later prompts reference earlier `output_key`s.
- **Memory appending (buffer):** chat_history grows with every interaction (`Human: ...\nAI: ...`); windowed keeps only the last k; summary replaces history with a distilled summary.
- **ReAct loop:** For a given question: generate a Thought → choose an Action (one of the tool names) with an Action Input → get an Observation → repeat (N times) → final Thought → Final Answer. The loop repeats until the model produces a final answer.
- **Exchange-rate conversion:** price_USD × exchange_rate = price_EUR (example: 2249 × 0.85 ≈ 1911.65).

### Diagrams / Visuals
- **Figure 7-1** — LangChain is a complete framework for using LLMs; modular components can be chained together for complex LLM systems. Retrieval is discussed in the next chapter.
- **Figure 7-2** — Representing pi with float 32-bit vs float 16-bit; notice the lowered accuracy when halving the number of bits.
- **Figure 7-3** — A single chain connects some modular component (prompt template or external memory) to the LLM.
- **Figure 7-4** — Chaining a prompt template with an LLM: only the input prompts need to be defined; the template is constructed for you.
- **Figure 7-5** — The prompt template Phi-3 expects (`<s><|user|>...<|end|><|assistant|>`).
- **Figure 7-6** — An example of a single chain using Phi-3's template.
- **Figure 7-7** — Sequential chains: the output of a prompt is used as the input for the next prompt.
- **Figure 7-8** — Story chain: output of the title prompt is the input of the character prompt; to generate the story, the output of all previous prompts is used.
- **Figure 7-9** — Conversation with an LLM with memory vs without memory (forgetful behavior).
- **Figure 7-10** — Reminding an LLM by appending the entire conversation history to the input prompt (ConversationBufferMemory).
- **Figure 7-11** — Extending the LLM chain with memory by appending the entire conversation history to the input prompt.
- **Figure 7-12** — Instead of passing the conversation history directly to the prompt, use another LLM to summarize it first (ConversationSummaryMemory).
- **Figure 7-13** — Extending the LLM chain with memory by summarizing the entire conversation history before giving it to the input prompt.
- **Figure 7-14** — Giving LLMs the ability to choose which tools they use for a problem results in more complex and accurate behavior (agents).
- **Figure 7-15** — An example of a ReAct prompt template (Question/Thought/Action/Action Input/Observation/.../Final Answer).
- **Figure 7-16** — An example of two cycles in a ReAct pipeline (search for MacBook price → calculator for USD→EUR).
- **Figure 7-17** — An example of the ReAct process in LangChain (intermediate steps showing tool access).

### Common Exam Traps
- **GGUF ≠ a different model:** GGUF is a compressed (quantized) version of Phi-3; FP16 in the filename = the 16-bit variant, NOT a model name.
- **Quantization trade-off:** reduces precision but makes models faster, use less VRAM, and are often almost as accurate; 8-bit cuts memory almost in half vs 16-bit. Rule of thumb: at least 4-bit.
- **`llm.invoke()` with no template returns nothing** for Phi-3 because it requires its chat template; transformers.pipeline applied it automatically, LangChain does not — you must build the template (via chains).
- **`prompt | llm` (pipe operator)** creates a LangChain chain; `invoke({"input_prompt": ...})` calls it with the input variable defined in the PromptTemplate.
- **Chain vs chain-of-thought:** A LangChain "chain" here = connecting modular components (prompt template, memory) to an LLM; NOT the "chain-of-thought" reasoning technique from the previous chapter.
- **Sequential chains require multiple LLM calls** with smaller prompts + intermediate outputs; the first link needs user input; later links reference earlier outputs via `output_key`.
- **LLMs are stateless by default** — they forget previous conversation content; memory must be added to the chain.
- **ConversationBufferMemory** = full history appended (token-heavy, needs large context window); **Windowed** = only last k interactions (loses older info, no compression); **Summary** = another LLM summarizes (captures full history, fewer tokens, but extra call per interaction + quality depends on summarization).
- **Windowed buffer example:** with `k=2`, after 3 conversations the first one is forgotten — the age is lost but the name (in conversation 2) is remembered.
- **ConversationSummaryMemory makes TWO calls per question**: the user prompt + the summarization prompt; slows computing; specific details not in the summary must be inferred.
- **Table 7-1:** Conversation Buffer — easiest implementation, no info loss, but slow + needs large context; Windowed — keeps last k only; Summary — full history captured, fewer tokens, but extra call per interaction.
- **Agents** determine their own actions (unlike chains following user-defined steps); two vital components = tools + agent type; can self-correct a roadmap to reach a goal.
- **ReAct = Reasoning and Acting** (Yao et al., 2022); three iterative steps: Thought, Action, Observation (repeatable); the Action is generally an external tool (calculator, search engine).
- **Agents need a stronger LLM:** the small local Phi-3 is insufficient; the chapter uses OpenAI's GPT-3.5 (`ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)`). Within a model family, increasing size leads to better performance.
- **Tools in the example:** DuckDuckGo search (`DuckDuckGoSearchResults` wrapped in `Tool(name="duckduck", ...)`) + `load_tools(["llm-math"], llm=openai_llm)` (calculator).
- **`handle_parsing_errors=True`** in AgentExecutor helps when the model's output doesn't parse.
- **Agents are a "double-edged sword":** no human in the loop → no human to judge output/reasoning quality; reliability requires careful system design (e.g., return the source URL, or verify each step).
- **LangChain imports differ by version:** this chapter uses the older `from langchain import LlamaCpp`, `from langchain.chat_models import ChatOpenAI`, `from langchain.memory import ...` and `from langchain.agents import ...` API style.
- **Memory types are a trade-off between speed, memory, and accuracy:** Buffer = instant but hogs tokens; Summary = slow but frees up tokens.

### Chapter Summary
Chapter 7 shows how to further improve LLM output without fine-tuning, using LangChain's modular abstractions. **Model I/O** covers loading a quantized (GGUF) Phi-3 via `llama-cpp-python` and LangChain's `LlamaCpp`, explaining quantization (fewer bits per parameter → smaller, faster, almost as accurate; aim for at least 4-bit) and why the raw `invoke()` call returns nothing without Phi-3's prompt template. **Chains** let us attach modular components to an LLM — starting with a reusable `PromptTemplate` (the `<s>`, `<|user|>`, `<|assistant|>`, `<|end|>` template) chained via `prompt | llm`, and extending to sequential multi-prompt chains (`LLMChain` with `output_key` per step: title → character → story). **Memory** makes stateless LLMs stateful: `ConversationBufferMemory` (append full history), `ConversationBufferWindowMemory` (keep last k), and `ConversationSummaryMemory` (an LLM summarizes the whole history), with Table 7-1 summarizing the speed/memory/accuracy trade-offs. **Agents** leverage an LLM to choose its own actions and tools, powered by the ReAct framework (Thought → Action → Observation loop); the chapter builds a web-search + calculator agent on GPT-3.5 via `create_react_agent` and `AgentExecutor`, and discusses the reliability risks of autonomous agents. The next chapter moves to retrieval-based search systems (RAG).

### Confidence Check
- **Sure**: GGUF/quantization concept (fewer bits per param, 8-bit ≈ half memory, ≥4-bit rule of thumb); `LlamaCpp` loading params (`n_gpu_layers=-1`, `max_tokens=500`, `n_ctx=2048`, `seed=42`); Phi-3 template tokens (`<s>`, `<|user|>`, `<|assistant|>`, `<|end|>`); `prompt | llm` chaining and `invoke`; reusable templates (`{product}`); sequential chains (title → character → story, `output_key`); statelessness; the three memory types and Table 7-1 pros/cons; the windowed-buffer k=2 example (age forgotten, name remembered); two calls per interaction in ConversationSummaryMemory; agents (tools + agent type); ReAct Thought/Action/Observation; GPT-3.5 usage for agents; DuckDuckGo + `llm-math` tools; AgentExecutor; MacBook example ($2,249 → ~1,911.65 EUR at 0.85).
- **Uncertain**: exact page anchors for figures (PDF text-flow approximate); precise wording of some quoted output where extraction broke lines mid-sentence; the exact original example outputs (paraphrased from extraction).

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Downloading the GGUF model
```bash
!wget https://huggingface.co/microsoft/Phi-3-mini-4k-instruct-gguf/resolve/main/Phi-3-mini-4k-instruct-fp16.gguf
```
- **Explanation:** Downloads the 16-bit (FP16) quantized variant of Phi-3 in GGUF format from Hugging Face. The repo contains multiple bit-variants; this file is the one used.
- **Fits the architecture:** GGUF is the file format for quantized models, which LlamaCpp (llama-cpp-python) can load efficiently.

### Code Block 2: Loading the GGUF model with LangChain
```python
from langchain import LlamaCpp
# Make sure the model path is correct for your system!
llm = LlamaCpp(
    model_path="Phi-3-mini-4k-instruct-fp16.gguf",
    n_gpu_layers=-1,
    max_tokens=500,
    n_ctx=2048,
    seed=42,
    verbose=False
)
```
- **Explanation:** Loads the quantized Phi-3 GGUF file via LlamaCpp. `n_gpu_layers=-1` puts all layers on the GPU; `max_tokens=500` limits generated tokens; `n_ctx=2048` sets the context size; `seed=42` sets a fixed seed; `verbose=False` silences logs.
- **Fits the architecture:** This replaces the transformers pipeline from previous chapters for quantized models.

### Code Block 3: Invoking the raw LLM (no output)
```python
llm.invoke("Hi! My name is Maarten. What is 1 + 1?")
# ''
```
- **Explanation:** Direct invocation returns empty output because Phi-3 requires its chat prompt template; we must provide it ourselves (unlike transformers.pipeline).
- **Fits the architecture:** Leads to building chains with a PromptTemplate.

### Code Block 4: Fallback chat model
```python
from langchain.chat_models import ChatOpenAI
# Create a chat-based LLM
chat_model = ChatOpenAI(openai_api_key="MY_KEY")
```
- **Explanation:** Alternative to running locally — uses OpenAI's chat API (API key placeholder "MY_KEY").
- **Fits the architecture:** Shows models are swappable; the chapter defaults to Phi-3 locally.

### Code Block 5: Creating a prompt template (single chain)
```python
from langchain import PromptTemplate
# Create a prompt template with the "input_prompt" variable
template = """<s><|user|>
{input_prompt}<|end|>
<|assistant|>"""
prompt = PromptTemplate(
    template=template,
    input_variables=["input_prompt"]
)
```
- **Explanation:** Defines Phi-3's expected chat template with a single variable, `input_prompt`, which is where the user's question goes.
- **Fits the architecture:** The template will be chained to the LLM so it is constructed automatically each time.

### Code Block 6: Chaining prompt and LLM
```python
basic_chain = prompt | llm
```
- **Explanation:** The pipe operator (`|`) chains the prompt template and the LLM into a single LangChain chain.
- **Fits the architecture:** This is the most basic form of a chain: a modular component (prompt template) connected to the LLM.

### Code Block 7: Using the chain
```python
# Use the chain
basic_chain.invoke(
    {
        "input_prompt": "Hi! My name is Maarten. What is 1 + 1?",
    }
)
# The answer to 1 + 1 is 2. It's a basic arithmetic operation...
```
- **Explanation:** `invoke` with the `input_prompt` variable produces the response without unnecessary tokens (clean output).
- **Fits the architecture:** The template is applied automatically; no need to rebuild it each time.

### Code Block 8: Reusable business-name prompt
```python
# Create a Chain that creates our business' name
template = "Create a funny name for a business that sells {product}."
name_prompt = PromptTemplate(
    template=template,
    input_variables=["product"]
)
```
- **Explanation:** A prompt with a `{product}` variable so the same prompt can be reused for many products without retyping.
- **Fits the architecture:** Prompt templates allow other changing variables beyond the chat wrapper.

### Code Block 9: Title chain (first link of the sequential chain)
```python
from langchain import LLMChain
# Create a chain for the title of our story
template = """<s><|user|>
Create a title for a story about {summary}. Only return the title.<|end|>
<|assistant|>"""
title_prompt = PromptTemplate(template=template, input_variables=["summary"])
title = LLMChain(llm=llm, prompt=title_prompt, output_key="title")
```
- **Explanation:** First link; takes `{summary}` as input and stores its result under the key `"title"`.
- **Fits the architecture:** `LLMChain` pairs LLM + prompt and names the output via `output_key` so later links can use it.

### Code Block 10: Running the title chain
```python
title.invoke({"summary": "a girl that lost her mother"})
# {'summary': 'a girl that lost her mother',
#  'title': ' "Whispers of Loss: A Journey Through Grief"'}
```
- **Explanation:** Returns a dict with the input (`summary`) and the output (`title`).
- **Fits the architecture:** Demonstrates the intermediate outputs available when breaking a task into subtasks.

### Code Block 11: Character chain (second link)
```python
# Create a chain for the character description using the summary and title
template = """<s><|user|>
Describe the main character of a story about {summary} with the title {title}. 
Use only two sentences.<|end|>
<|assistant|>"""
character_prompt = PromptTemplate(
    template=template, input_variables=["summary", "title"]
)
character = LLMChain(llm=llm, prompt=character_prompt, output_key="character")
```
- **Explanation:** Second link uses both `{summary}` and the previously generated `{title}` to describe the main character, stored under `"character"`.
- **Fits the architecture:** Sequential chains pass earlier outputs forward into later prompts.

### Code Block 12: Story chain (third link)
```python
# Create a chain for the story using the summary, title, and character descrip
tion
template = """<s><|user|>
Create a story about {summary} with the title {title}. The main character is: 
{character}. Only return the story and it cannot be longer than one paragraph. 
<|end|>
<|assistant|>"""
story_prompt = PromptTemplate(
    template=template, input_variables=["summary", "title", "character"]
)
story = LLMChain(llm=llm, prompt=story_prompt, output_key="story")
```
- **Explanation:** Third link combines `{summary}`, `{title}`, and `{character}` to generate the final story, stored under `"story"`.
- **Fits the architecture:** The final link depends on all previous outputs — a chain of chains.

### Code Block 13: Combining the chains
```python
# Combine all three components to create the full chain
llm_chain = title | character | story
```
- **Explanation:** Pipes the three LLMChains together so the output of each flows into the next.
- **Fits the architecture:** This is a sequential (multi-prompt) chain; the user only provides one short input (`summary`).

### Code Block 14: Running the full story chain
```python
llm_chain.invoke("a girl that lost her mother")
# {'summary': 'a girl that lost her mother',
#  'title': ' "In Loving Memory: A Journey Through Grief"',
#  'character': ' The protagonist, Emily, is a resilient young girl ...',
#  'story': " In Loving Memory: A Journey Through Grief revolves around Emily, ..."}
```
- **Explanation:** One short input produces all three components (title, character, story); each component is separately accessible.
- **Fits the architecture:** Demonstrates the advantage of decomposition — individual components can be extracted.

### Code Block 15: Conversation buffer prompt with chat history
```python
template = """<s><|user|>Current conversation:{chat_history}
{input_prompt}<|end|>
<|assistant|>"""
prompt = PromptTemplate(
    template=template,
    input_variables=["input_prompt", "chat_history"]
)
```
- **Explanation:** Updated prompt template adds a `chat_history` variable where the conversation history is injected before the question.
- **Fits the architecture:** Provides a placeholder for the memory module to fill.

### Code Block 16: ConversationBufferMemory chain
```python
from langchain.memory import ConversationBufferMemory
# Define the type of memory we will use
memory = ConversationBufferMemory(memory_key="chat_history")
# Chain the LLM, prompt, and memory together
llm_chain = LLMChain(
    prompt=prompt,
    llm=llm,
    memory=memory
)
```
- **Explanation:** Creates buffer memory keyed to `chat_history` and chains LLM + prompt + memory together.
- **Fits the architecture:** The memory automatically appends the full conversation to the `chat_history` variable.

### Code Block 17: First conversation with memory
```python
llm_chain.invoke({"input_prompt": "Hi! My name is Maarten. What is 1 + 1?"})
# {'input_prompt': 'Hi! My name is Maarten. What is 1 + 1?',
#  'chat_history': '',
#  'text': " Hello Maarten! The answer to 1 + 1 is 2. Hope you're having a great day!"}
```
- **Explanation:** The `text` key holds generated output; `input_prompt` holds the question; `chat_history` is empty because it's the first interaction.
- **Fits the architecture:** Output keys now include the memory-managed chat_history.

### Code Block 18: Follow-up question (memory recall)
```python
llm_chain.invoke({"input_prompt": "What is my name?"})
# {'input_prompt': 'What is my name?',
#  'chat_history': "Human: Hi! My name is Maarten. What is 1 + 1?\nAI:  Hello Maarten! ...",
#  'text': ' Your name is Maarten.'}
```
- **Explanation:** With the conversation history in the prompt, the LLM correctly recalls the name.
- **Fits the architecture:** LangChain stores interactions as `Human:` (user) and `AI:` (LLM) turns.

### Code Block 19: Windowed conversation buffer
```python
from langchain.memory import ConversationBufferWindowMemory
# Retain only the last 2 conversations in memory
memory = ConversationBufferWindowMemory(k=2, memory_key="chat_history")
# Chain the LLM, prompt, and memory together
llm_chain = LLMChain(
    prompt=prompt,
    llm=llm,
    memory=memory
)
```
- **Explanation:** Keeps only the last 2 conversations in the chat_history, limiting prompt growth.
- **Fits the architecture:** Reduces token usage but loses older conversations (no compression of the last k).

### Code Block 20: Two conversations to build window history
```python
llm_chain.predict(input_prompt="Hi! My name is Maarten and I am 33 years old. What is 1 + 1?")
llm_chain.predict(input_prompt="What is 3 + 3?")
```
- **Explanation:** Uses `predict` to add two conversations (name + age in the first; 3+3 in the second).
- **Fits the architecture:** `predict` is shorthand for invoke that fills the input variables.

### Code Block 21: Checking remembered name with window memory
```python
llm_chain.invoke({"input_prompt":"What is my name?"})
# 'chat_history': "Human: Hi! My name is Maarten and I am 33 years old. ...\nAI: ...\nHuman: What is 3 + 3?\nAI: ...",
# 'text': ' Your name is Maarten, as mentioned at the beginning of our conversation.'
```
- **Explanation:** With 2 conversations in the window and now a third, the model still remembers the name from conversation 2.
- **Fits the architecture:** The window now holds the last two interactions (conversations 2 and 3).

### Code Block 22: Checking forgotten age with window memory
```python
llm_chain.invoke({"input_prompt":"What is my age?"})
# 'chat_history': "Human: What is 3 + 3?\nAI: ...\nHuman: What is my name?\nAI: Your name is Maarten.",
# 'text': " I'm unable to determine your age ... unless you choose to share it."
```
- **Explanation:** Since k=2 and we now have three+ conversations, the first interaction (containing age 33) has been dropped — the model no longer knows the age.
- **Fits the architecture:** Illustrates the windowed buffer's limitation (only last k retained).

### Code Block 23: ConversationSummaryMemory — summary prompt template
```python
summary_prompt_template = """<s><|user|>Summarize the conversations and update 
with the new lines.
Current summary:
{summary}
new lines of conversation:
{new_lines}
New summary:<|end|>
<|assistant|>"""
summary_prompt = PromptTemplate(
    input_variables=["new_lines", "summary"],
    template=summary_prompt_template
)
```
- **Explanation:** Template instructs an LLM to update a `{summary}` with `{new_lines}` and produce a new summary.
- **Fits the architecture:** This summarization LLM call happens in addition to the main user prompt (two calls per interaction).

### Code Block 24: ConversationSummaryMemory chain
```python
from langchain.memory import ConversationSummaryMemory
# Define the type of memory we will use
memory = ConversationSummaryMemory(
    llm=llm, 
    memory_key="chat_history", 
    prompt=summary_prompt
)
# Chain the LLM, prompt, and memory together
llm_chain = LLMChain(
    prompt=prompt,
    llm=llm,
    memory=memory
)
```
- **Explanation:** Summary memory requires an extra `llm` (the summarizer) and the `summary_prompt`. The same LLM is used for both summarizing and answering in the chapter, though a smaller one could speed summarization.
- **Fits the architecture:** The conversation is summarized before being placed into the prompt.

### Code Block 25: Testing the summary memory
```python
llm_chain.invoke({"input_prompt": "Hi! My name is Maarten. What is 1 + 1?"})
llm_chain.invoke({"input_prompt": "What is my name?"})
# 'chat_history': ' Summary: Human, identified as Maarten, asked the AI about 
# the sum of 1 + 1, which was correctly answered by the AI as 2 ...'
```
- **Explanation:** After each step, the chain summarizes the conversation up to that point; the summary (not raw history) is placed in `chat_history`.
- **Fits the architecture:** Keeps the history small; the summary quality depends on the summarizer LLM.

### Code Block 26: Continuing the conversation and checking the summary
```python
llm_chain.invoke({"input_prompt": "What was the first question I asked?"})
memory.load_memory_variables({})
```
- **Explanation:** After more turns, the summary grows to include later exchanges; `load_memory_variables({})` retrieves the most recent summary.
- **Fits the architecture:** The summary infers the original question from context (it wasn't stored verbatim) — a limitation when specific details must be preserved.

### Code Block 27: Loading OpenAI for agents
```python
import os
from langchain_openai import ChatOpenAI
# Load OpenAI's LLMs with LangChain
os.environ["OPENAI_API_KEY"] = "MY_KEY"
openai_llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)
```
- **Explanation:** The small local Phi-3 is insufficient for agent instructions; GPT-3.5 (temperature 0 for deterministic tool use) is used instead.
- **Fits the architecture:** Agents need a model strong enough to follow complex instructions (larger local LLMs exist but need far more compute/VRAM).

### Code Block 28: The ReAct template
```python
react_template = """Answer the following questions as best you can. You have 
access to the following tools:
{tools}
Use the following format:
Question: the input question you must answer
Thought: you should always think about what to do
Action: the action to take, should be one of [{tool_names}]
Action Input: the input to the action
Observation: the result of the action
... (this Thought/Action/Action Input/Observation can repeat N times)
Thought: I now know the final answer
Final Answer: the final answer to the original input question
Begin!
Question: {input}
Thought:{agent_scratchpad}"""
prompt = PromptTemplate(
    template=react_template,
    input_variables=["tools", "tool_names", "input", "agent_scratchpad"]
)
```
- **Explanation:** The ReAct prompt defines the tool list, the required Thought/Action/Action Input/Observation loop, and variables for `tools`, `tool_names`, `input`, and `agent_scratchpad` (intermediate steps).
- **Fits the architecture:** This template is the framework that makes the LLM produce reasoning and tool calls.

### Code Block 29: Defining the tools
```python
from langchain.agents import load_tools, Tool
from langchain.tools import DuckDuckGoSearchResults
# You can create the tool to pass to an agent
search = DuckDuckGoSearchResults()
search_tool = Tool(
    name="duckduck",
    description="A web search engine. Use this to as a search engine for gen
eral queries.",
    func=search.run,
)
# Prepare tools
tools = load_tools(["llm-math"], llm=openai_llm)
tools.append(search_tool)
```
- **Explanation:** Creates a DuckDuckGo search tool (wrapped in a `Tool` with a name, description, and run function) and a math/calculator tool via `load_tools(["llm-math"])`.
- **Fits the architecture:** Tools are how the agent interacts with the real world (acting in ReAct).

### Code Block 30: Creating the ReAct agent and executor
```python
from langchain.agents import AgentExecutor, create_react_agent
# Construct the ReAct agent
agent = create_react_agent(openai_llm, tools, prompt)
agent_executor = AgentExecutor(
    agent=agent, tools=tools, verbose=True, handle_parsing_errors=True
)
```
- **Explanation:** Builds the ReAct agent from the LLM, tools, and template, then wraps it in an AgentExecutor that runs the loop. `verbose=True` shows intermediate steps; `handle_parsing_errors=True` tolerates malformed outputs.
- **Fits the architecture:** The executor handles the Thought/Action/Observation execution cycle.

### Code Block 31: Invoking the agent
```python
agent_executor.invoke(
    {
        "input": "What is the current price of a MacBook Pro in USD? How much 
would it cost in EUR if the exchange rate is 0.85 EUR for 1 USD."
    }
)
```
- **Explanation:** The agent searches the web for the price, then uses the calculator to convert USD→EUR.
- **Fits the architecture:** The agent self-directs which tools to use and when (search first, then math).

### Code Block 32: Agent output
```python
{'input': 'What is the current price of a MacBook Pro in USD? ...',
 'output': 'The current price of a MacBook Pro in USD is $2,249.00. It would 
cost approximately 1911.65 EUR with an exchange rate of 0.85 EUR for 1 USD.'}
```
- **Explanation:** Final output combines web-sourced price ($2,249.00) and calculator conversion (~1,911.65 EUR at 0.85).
- **Fits the architecture:** Demonstrates the power of agents with just a search engine and calculator — and raises reliability concerns (no human in the loop).
