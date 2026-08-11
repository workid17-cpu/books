# 📘 Chapter 7 Flashcards: Advanced Text Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 7

---

## Part 1: Terms → Definitions

**Q:** What is LangChain?
**A:** One of the earlier frameworks that simplifies working with LLMs through useful abstractions; named after its core "chains" method. Newer frameworks of note: DSPy and Haystack.

**Q:** What is quantization?
**A:** A method that reduces the number of bits needed to represent the parameters of an LLM while attempting to maintain most of the original information; costs some precision but makes the model faster, uses less VRAM, and is often nearly as accurate.

**Q:** What is a GGUF model?
**A:** A compressed (quantized) version of an original model, loaded here with llama-cpp-python + LangChain.

**Q:** What do "bits" represent in a model?
**A:** A series of 0s and 1s encoding values in binary form; more bits = wider range of values but more memory needed.

**Q:** What is the time analogy used for quantization?
**A:** Saying "14:16" is correct but not fully precise (vs "14:16 and 12 seconds"); quantization removes unneeded precision (seconds) while keeping vital info (hours and minutes).

**Q:** What bit-width variant of Phi-3 does the chapter use vs the original?
**A:** An 8-bit variant vs the original 16-bit, cutting memory requirements almost in half.

**Q:** What does FP16 in a GGUF filename mean?
**A:** It is the 16-bit variant of the model (e.g., `Phi-3-mini-4k-instruct-fp16.gguf`).

**Q:** What is the rule of thumb for bit-quantized models?
**A:** Look for at least 4-bit quantized models (good balance of compression and accuracy); 3-bit or 2-bit shows noticeable degradation.

**Q:** How do we load a GGUF model in LangChain?
**A:** `from langchain import LlamaCpp; llm = LlamaCpp(model_path="Phi-3-mini-4k-instruct-fp16.gguf", n_gpu_layers=-1, max_tokens=500, n_ctx=2048, seed=42, verbose=False)`.

**Q:** What does `n_gpu_layers=-1` do in LlamaCpp?
**A:** Places all layers of the model on the GPU.

**Q:** What is the function used to generate output in LangChain?
**A:** `invoke` (e.g., `llm.invoke("...")`).

**Q:** Why does `llm.invoke("Hi! My name is Maarten. What is 1 + 1?")` return no output?
**A:** Because Phi-3 requires its specific prompt template, which must be explicitly provided in LangChain (unlike transformers.pipeline which applies the chat template automatically).

**Q:** What is a chain in LangChain?
**A:** LangChain's main method; the most basic form connects an LLM with some additional tool, prompt, or feature; multiple chains can be connected together.

**Q:** What are the four main components of Phi-3's prompt template?
**A:** `<s>` (prompt start), `<|user|>` (start of user prompt), `<|assistant|>` (start of model output), `<|end|>` (end of prompt or output).

**Q:** What does the `<|user|>` token do?
**A:** Indicates the start of the user's prompt.

**Q:** What does the `<|assistant|>` token do?
**A:** Indicates the start of the model's output.

**Q:** What does the `<|end|>` token do?
**A:** Indicates the end of either the prompt or the model's output.

**Q:** What does the `<s>` token do?
**A:** Indicates when the prompt starts.

**Q:** What is a PromptTemplate?
**A:** A reusable prompt with variables (e.g., `{input_prompt}`, `{product}`) chained to the LLM, avoiding copy-pasting the template each time.

**Q:** How do you create a basic chain from a prompt and LLM in LangChain?
**A:** Using the pipe operator: `basic_chain = prompt | llm`.

**Q:** How do you invoke a basic chain?
**A:** `basic_chain.invoke({"input_prompt": "..."})` — passing the variable(s) defined in the PromptTemplate.

**Q:** What is LLMChain?
**A:** A LangChain class that links an LLM and a prompt (with optional memory and `output_key`); used for sequential multi-prompt chains here.

**Q:** What is `output_key` in an LLMChain?
**A:** The named key under which the chain's output is stored (e.g., `output_key="title"`), which later chains can reference.

**Q:** What are sequential chains?
**A:** Chains linked so that each link handles a subtask; the output of one prompt becomes the input of the next.

**Q:** Why decompose a complex prompt into sequential subtasks?
**A:** Multiple smaller prompts with intermediate outputs are easier to handle, and individual components (e.g., the title) can be extracted easily.

**Q:** What does "stateless" mean for LLMs?
**A:** Models have no memory of any previous conversation — they forget content between prompts.

**Q:** How do you make an LLM "stateful"?
**A:** By adding specific types of memory to the chain (e.g., conversation buffer or conversation summary).

**Q:** What is ConversationBufferMemory?
**A:** LangChain memory that appends the full conversation history to the input prompt to remind the LLM of what happened.

**Q:** What memory_key is used in the chapter's buffer examples?
**A:** `chat_history` (e.g., `ConversationBufferMemory(memory_key="chat_history")`).

**Q:** How does LangChain store conversation turns internally?
**A:** As interactions between you (`Human:`) and the LLM (`AI:`).

**Q:** What are the output keys of a chain using ConversationBufferMemory?
**A:** `input_prompt`, `chat_history`, and `text` (the generated output).

**Q:** What is ConversationBufferWindowMemory?
**A:** Memory that retains only the last k conversations (e.g., `k=2`) instead of the full chat history.

**Q:** What problem does ConversationBufferWindowMemory solve?
**A:** It minimizes the context window as the conversation grows toward the token limit.

**Q:** What is the limitation of ConversationBufferWindowMemory?
**A:** It only captures the last k interactions; older information is lost and there is no compression of the last k.

**Q:** What is ConversationSummaryMemory?
**A:** Memory that summarizes an entire conversation history into its main points using another LLM.

**Q:** How many calls happen per question with ConversationSummaryMemory?
**A:** Two: the user prompt and the summarization prompt.

**Q:** What template variables does the summary prompt use?
**A:** `{summary}` (current summary) and `{new_lines}` (new lines of conversation), producing a "New summary".

**Q:** How do you retrieve the current summary from ConversationSummaryMemory?
**A:** `memory.load_memory_variables({})`.

**Q:** What are the downsides of ConversationSummaryMemory?
**A:** An additional LLM call per interaction (slower); quality depends on the LLM's summarization capabilities; specific details must be inferred from the summary.

**Q:** What is the trade-off among the memory types?
**A:** Speed, memory, and accuracy: ConversationBufferMemory is instant but hogs tokens; ConversationSummaryMemory is slow but frees up tokens.

**Q:** What are agents in the context of LLMs?
**A:** Systems that leverage a language model to determine which actions they should take and in what order.

**Q:** What two vital components extend agents beyond chains/memory?
**A:** Tools (things the agent can use to do things it could not do itself) and the agent type (plans the actions to take or tools to use).

**Q:** How do agents differ from chains?
**A:** Agents can create and self-correct a roadmap to achieve a goal and interact with the real world via tools, rather than following a user-defined set of steps.

**Q:** Why is a calculator useful for an LLM?
**A:** LLMs are notoriously bad at mathematical problems; a calculator tool lets them solve math tasks accurately.

**Q:** What does ReAct stand for?
**A:** Reasoning and Acting — a framework from Shunyu Yao et al. ("ReAct: Synergizing reasoning and acting in language models," arXiv:2210.03629, 2022).

**Q:** What are the three iterative steps of ReAct?
**A:** Thought, Action, Observation (the cycle can repeat N times before a final answer).

**Q:** What is a Thought in ReAct?
**A:** The LLM's reasoning about what it thinks it should do next and why.

**Q:** What is an Action in ReAct?
**A:** An action triggered based on the thought, generally an external tool like a calculator or search engine.

**Q:** What is an Observation in ReAct?
**A:** The result of the action returned to the LLM, often a summary of whatever result was retrieved.

**Q:** What is AgentExecutor?
**A:** A LangChain component that handles executing the steps of an agent (the Thought/Action/Observation loop).

**Q:** What is `agent_scratchpad`?
**A:** A variable in the ReAct template that holds the intermediate thoughts/actions/observations generated so far.

**Q:** What is a Tool in LangChain agents?
**A:** An external capability (e.g., web search, calculator) exposed to the agent with a name, description, and run function.

**Q:** What is "human in the loop"?
**A:** A human reviewing/judging the agent's intermediate steps and output quality; agents typically lack this.

## Part 2: Short Answer

**Q:** Why does the book use GGUF (quantized) models in this chapter?
**A:** To load Phi-3 more efficiently. Quantization reduces bits per parameter, making the model faster, smaller in VRAM, and often nearly as accurate — with only some precision loss.

**Q:** What is the recommended minimum quantization level and why?
**A:** At least 4-bit. It offers a good balance between compression and accuracy; 3-bit/2-bit degradation is noticeable, so a smaller higher-precision model is preferable.

**Q:** What happens if you call `llm.invoke()` on a raw Phi-3 LLM in LangChain without a template?
**A:** You get no output — Phi-3 requires its specific prompt template, which LangChain does not apply automatically (unlike transformers.pipeline).

**Q:** How do you build a reusable prompt for generating funny business names?
**A:** `template = "Create a funny name for a business that sells {product}."` with `PromptTemplate(template=template, input_variables=["product"])`.

**Q:** In the story example, what are the three components and how are they connected?
**A:** Title (from `{summary}`), character description (from `{summary}` + `{title}`), and story (from `{summary}` + `{title}` + `{character}`); combined as `llm_chain = title | character | story`.

**Q:** What advantage does the sequential story chain provide over a single prompt?
**A:** Only a single short user input (the summary) is needed, and each component (title, character, story) is individually accessible/extractable.

**Q:** How does the chapter demonstrate LLM statelessness?
**A:** Tell the model "My name is Maarten", then ask "What is my name?" — the model doesn't know, because it has no memory of the previous conversation.

**Q:** How does ConversationBufferMemory work?
**A:** It appends the entire conversation history into the prompt via the `chat_history` variable, reminding the LLM of everything said so far.

**Q:** In the windowed-buffer demo, why does the model forget the age but remember the name?
**A:** With `k=2`, after three+ conversations the first interaction (which contained "33 years old") is dropped from the window, while the name (given in the second conversation) is still within the last two.

**Q:** How does ConversationSummaryMemory keep the chat history small?
**A:** A summarization LLM distills the whole conversation into its main points (using a template with `{summary}` and `{new_lines}`), so the prompt receives a compact summary instead of raw history.

**Q:** When is it a good idea to use a smaller LLM for summarization?
**A:** When you want to speed up computation — a smaller model can handle the summarization task while the main LLM answers the user prompt.

**Q:** What are the pros of ConversationBufferMemory per Table 7-1?
**A:** Easiest implementation; ensures no information loss within the context window.

**Q:** What are the cons of ConversationBufferMemory per Table 7-1?
**A:** Slower generation speed as more tokens are needed; only suitable for large-context LLMs; larger chat histories make information retrieval difficult.

**Q:** What are the pros of Windowed Conversation Buffer per Table 7-1?
**A:** Large-context LLMs are not needed unless chat history is large; no information loss over the last k interactions.

**Q:** What are the cons of Windowed Conversation Buffer per Table 7-1?
**A:** Only captures the last k interactions; no compression of the last k interactions.

**Q:** What are the pros of Conversation Summary per Table 7-1?
**A:** Captures the full history; enables long conversations; reduces tokens needed to capture full history.

**Q:** What are the cons of Conversation Summary per Table 7-1?
**A:** An additional call is necessary for each interaction; quality is reliant on the LLM's summarization capabilities.

**Q:** Why does the chapter use GPT-3.5 instead of Phi-3 for the agent example?
**A:** Agents need an LLM powerful enough to follow complex instructions; the relatively small Phi-3 is not sufficient (larger local LLMs exist but need significantly more compute/VRAM).

**Q:** What is the ReAct template's required format?
**A:** Question, Thought, Action, Action Input, Observation (repeatable N times), a final Thought, and a Final Answer.

**Q:** What tools does the chapter's agent use and how are they created?
**A:** DuckDuckGo search (`DuckDuckGoSearchResults()` wrapped in `Tool(name="duckduck", description=..., func=search.run)`) and a math/calculator tool via `load_tools(["llm-math"], llm=openai_llm)`.

**Q:** How do you construct and run the ReAct agent?
**A:** `agent = create_react_agent(openai_llm, tools, prompt)`; `agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True, handle_parsing_errors=True)`; then `agent_executor.invoke({"input": "..."})`.

**Q:** What is the output of the MacBook Pro agent example?
**A:** "The current price of a MacBook Pro in USD is $2,249.00. It would cost approximately 1911.65 EUR with an exchange rate of 0.85 EUR for 1 USD."

**Q:** What is the "double-edged sword" of agents?
**A:** Agents are autonomous — there is no human in the loop to judge the quality of the output or reasoning process — so reliability requires careful system design.

**Q:** What reliability improvements does the chapter suggest for agents?
**A:** Have the agent return the website's URL where it found the price, or ask whether the output is correct at each step.

## Part 3: Fill-in-the-Blank

**Q:** A GGUF model is a ________ version of an original model produced through ________.
**A:** compressed; quantization.

**Q:** ________ reduces the number of bits needed to represent the parameters of an LLM while attempting to maintain most of the original information.
**A:** Quantization.

**Q:** The rule of thumb is to look for at least ________-bit quantized models.
**A:** 4.

**Q:** The model file used is `Phi-3-mini-4k-instruct-________.gguf`.
**A:** fp16.

**Q:** In LlamaCpp, `________=-1` places all layers on the GPU.
**A:** n_gpu_layers.

**Q:** Phi-3's template begins with the ________ token and ends the model output with the ________ token.
**A:** `<s>`; `<|assistant|>` (or `<|end|>` for end of output).

**Q:** A basic LangChain chain is created with the pipe operator: `basic_chain = ________`.
**A:** `prompt | llm`.

**Q:** The business-name prompt uses the variable ________ in the template.
**A:** `{product}`.

**Q:** In the story chain, the ________ link is the only component that requires user input.
**A:** title.

**Q:** The story chain is combined as `llm_chain = ________`.
**A:** `title | character | story`.

**Q:** LLMs are ________ by default — they have no memory of any previous conversation.
**A:** stateless.

**Q:** ConversationBufferMemory stores all conversations under the memory key ________.
**A:** `chat_history`.

**Q:** LangChain stores turns as ________ (you) and ________ (the LLM).
**A:** Human; AI.

**Q:** ConversationBufferWindowMemory retains only the last ________ conversations.
**A:** k (e.g., 2).

**Q:** With ConversationSummaryMemory, each question triggers two calls: the user prompt and the ________ prompt.
**A:** summarization.

**Q:** The summary template updates a current ________ with ________ lines of conversation.
**A:** summary; new.

**Q:** Agents extend model I/O, chains, and memory with two vital components: ________ and the ________ type.
**A:** tools; agent.

**Q:** ReAct iteratively follows the steps Thought, ________, and Observation.
**A:** Action.

**Q:** The MacBook Pro example uses a ________ to find the price and a ________ to convert USD to EUR.
**A:** web search engine; calculator.

**Q:** For agents, the chapter loads OpenAI's ________ model with temperature 0.
**A:** GPT-3.5 (gpt-3.5-turbo).

**Q:** The ReAct template ends with the ________ answer to the original input question.
**A:** Final.

**Q:** The tools list includes ________ search and the ________-math calculator.
**A:** DuckDuckGo; llm.

**Q:** `handle_parsing_errors=________` in AgentExecutor tolerates malformed model output.
**A:** True.

**Q:** Agents have no ________ in the loop to judge the quality of the output.
**A:** human.

**Q:** The next chapter discusses using LLMs to improve existing ________ systems (retrieval).
**A:** search.
