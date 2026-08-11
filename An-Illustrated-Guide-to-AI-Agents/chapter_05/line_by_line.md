# Chapter 5 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 5 "Tool Usage, Learning, and Protocols"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings are paraphrased/annotated; every substantive paragraph is covered.

---

## 5.1 What Tools Are (Chapter Intro)

> "The tool module is such an interesting module for augmenting your LLM and generating autonomous behavior in agents. By themselves, regular LLMs are nothing more than functions that take in some text and output some text. Although that creates interesting chatbots, they cannot yet interact with their environment. Tools, in the form of functions and APIs, allow LLMs to step into the real world and interact with any system exposed to the LLM, ranging from codebases and internal tools to online databases and even home assistants."

**Explanation:** A plain LLM is a text-in/text-out function. Tools (functions/APIs) let an LLM interact with the real world — codebases, internal tools, databases, home assistants.

**Word meanings:**
- **augmenting** = extending/adding capabilities to.
- **autonomous** = self-directed, acting on its own.
- **exposed to** = made available to.

**Technical terms:**
- **APIs** = Application Programming Interfaces — standardized ways to call a service.
- **tool module** = the component that gives an agent access to external functions.

---

> "A key trait that defines an agent is its ability to autonomously search for, select, and utilize tools, allowing it to interact with and influence its environment. With enough capabilities, agents can even create their own set of tools to use. Generally, tools allow an LLM to take action and interact with the external environment or extract data and use external applications (see Figure 5-1). The benefit of tools is not contained to interaction with the environment. Tools are typically used to access external knowledge or memories, as we discussed in Chapter 4. We can even use tools to access specialized LLMs that have capabilities that extend beyond what the agent is capable of, such as multimodal LLMs or LLMs specialized for certain tasks (like coding)."

**Explanation:** What makes an entity an *agent* is autonomous tool search/selection/use. Tools let agents act, retrieve data, access external knowledge/memory, or call specialized LLMs (multimodal, coding-focused).

**Word meanings:**
- **trait** = distinguishing characteristic.
- **utilize** = make use of.
- **specialized** = tuned for a particular purpose.
- **multimodal** = handling more than text (images, audio, video).

**Technical terms:**
- **Figure 5-1 categorization** = tools split into "action" tools vs "retrieval" tools.

---

> "However, regular LLMs cannot search the web, use a calculator, or schedule appointments. They can only communicate the intention of doing so, without being able to act upon it. Without tools, agents can only think and plan autonomously but not act autonomously."

**Explanation:** An LLM can only *express* the desire to search/calculate/schedule; it can't do it. Tools provide the "act" half of "think and act."

**Word meanings:**
- **intention** = what it wants to do.
- **act upon** = carry out.

---

> "The degree of autonomy also decides how tools are used. As shown in Figure 5-2, if there is no autonomy but there is a fixed flow, then tools can be used in a predefined order. For instance, a research agent might always call tools like arXiv and Google to extract results, which are subsequently summarized."

**Explanation:** Low autonomy = fixed, predefined tool order (e.g., always arXiv then Google then summarize).

**Word meanings:**
- **predefined** = decided in advance.
- **subsequently** = afterwards.

**Technical terms:**
- **fixed flow** = a hardcoded sequence of tool calls.

---

> "In contrast, systems with larger degrees of autonomy allow agents to choose which tool to use and when. Illustrated in Figure 5-3, they are still sequences of LLM calls but with autonomous selection of tools decided by the agent."

**Explanation:** High autonomy = the agent itself picks which tool and when, still as a chain of LLM calls.

---

> "There's a lot more to tools than merely using them. How are they created? How is the output of a tool processed by the LLM or agent? How are tools selected? How many tools can an LLM effectively handle?"

**Explanation:** The chapter roadmap: creation, output processing, selection, and scale limits of tools.

---

> "Throughout this chapter, we'll not only explore several types of tools but also how LLMs and agents learn to use them and even how these tools can be standardized across different agentic systems. As shown in Figure 5-4, this allows interaction with the environment. It makes the next chapter, about planning out actions to take, more than a theoretical exercise. After all, how could an LLM act autonomously without tools?"

**Explanation:** Tools make planning (Ch 6) concrete — you can't act autonomously without tools.

**Word meanings:**
- **standardized** = given a common format/protocol.
- **theoretical exercise** = abstract idea with no practical effect.

**Technical terms:**
- **agentic systems** = systems built around agents.

---

## 5.2 Tool Usage (The Five Steps)

> "Tool usage consists of more steps than you might expect. It does not start with an LLM using a tool, but by actually creating and defining the tools, planning which tool to use instead, and eventually calling them. Although the steps in tool usage can be seen from many different perspectives, such as the comparison of tools to the human action system inspired by the human brain or as action modules that revolve around the agent's decisions. We, however, explore the following set of steps as it focuses on concrete implementations and executions of tools:
> Tool creation — Tool definition — Tool selection — Tool calling — Output processing"

**Explanation:** Tool usage isn't just "LLM calls tool." There are five steps, from building the tool to feeding results back. The book chooses these five for their focus on concrete implementation.

**Word meanings:**
- **perspectives** = viewpoints/frameworks.
- **concrete implementations** = actual, runnable code/designs.

---

> "Although agents might seem like they can do everything themselves (which they can attempt to a certain degree), they require help from the user or developer to use the tools. Specifically, an LLM does not actually call the tool but merely shows the intention of doing so. The actual tool call is typically executed by an external (automated) process."

**Explanation:** Key idea: the LLM only expresses *intent*; an external process performs the actual call.

**Word meanings:**
- **external (automated) process** = outside software (e.g., your Python code, the inference engine).

---

> "Note that each step may have various forms and implementations, many of which we will cover later. For now, let's explore the most common way tool usage happens in these steps and explore them in more detail."

---

### Tool Creation

> "Although tools can come in various forms, they are typically functions that can be accessed through either an API or some internal code. This can be as complex or minimal as is required for your application. LLMs, for instance, are not known for their mathematical capabilities. Let's create a simple function for multiplying two values that the LLM can use."

**Explanation:** Tools = functions, exposed via API or internal code. The book's example: a `multiply` function because LLMs are bad at math.

**Word meanings:**
- **minimal** = as simple as possible.

---

> "This function multiplies two values, taking them as strings and converting them to floats. The reason for this is that LLMs return only text, which will have to be converted. As such, these tools should be heavily documented and communicated well to the LLM. In some cases, the docstrings are passed automatically, so it is important to describe them well. A clear benefit is that it motivates users to focus more on writing great documentation, which is often not a priority when developing code. Although this is a rather simplified example, imagine codebases with functions that span hundreds of lines of code. Documentation is becoming increasingly important, as it allows the agent to understand which edge cases exist or implementation details that cannot be easily determined by examining the code."

**Explanation:** Tools take strings and convert internally (LLMs only output text). Docstrings matter — they're often passed to the LLM automatically — and documentation quality becomes critical as functions grow.

**Word meanings:**
- **floats** = decimal numbers (e.g., 3.14).
- **docstrings** = documentation text inside a function definition.
- **edge cases** = unusual inputs or boundary conditions.

**Technical terms:**
- **`def multiply(a: str, b: str) -> float: return float(a) * float(b)`** = a function taking two strings, converting to floats, returning their product.

---

> "This tool can be accessed by directly calling multiply(a, b), which is something the LLM cannot do since it only outputs strings, but it can be done by an automated system. Note that external API calls may also call tools. Either way, a tool is generally considered to be some form of function that can be called in various ways. In our example, the most straightforward method would be to save it in a dictionary so that we can access the function by a string. In this case, our database is the registry variable: `registry = {"multiply": multiply}`"

**Explanation:** The LLM can't call the function, but code can. Tools are stored in a dictionary keyed by name — a "registry" — so they can be looked up by string.

**Word meanings:**
- **straightforward** = simple/direct.
- **registry** = a name→function lookup table.

---

> "Although creating tools can be a straightforward process, the LLM should be taken into account when designing them. It's a similar process to designing code for people to use, where you may ask yourself: does the user understand what the tool does? Is it clear what is being returned? Are the variable names descriptive? Is there documentation? This is especially important for LLMs because they often explicitly need to be told what exists, what doesn't, and what the tools are capable of."

**Explanation:** Design tools for the LLM as you would for human users: clear names, clear returns, good docs, explicit about what exists.

**Word meanings:**
- **descriptive** = names that say what they represent.
- **explicitly** = directly/clearly stated.

---

> "Let's start by creating the first iteration of the Tools class that will give your TinyAgent access to tools."

**Code (paraphrased):**
```python
from typing import Callable

class Tools:
    """Tool registry for the Agent."""
    def __init__(self, requires_approval: list[str] = []):
        self.registry = {}
        self.requires_approval = requires_approval

    def add_tool(self, name, func, description=""):
        self.registry[name] = {"function": func, "description": description}

    @property
    def schemas(self) -> None:
        """Used only for native tool-calling."""
        return None
```

**Explanation:** `Tools` class holds a registry, supports marking tools as requiring approval, and has a `schemas` property (placeholder for native calling, covered later).

**Technical terms:**
- **`add_tool`** = registers a tool (name, function, description).
- **`requires_approval`** = list of tool names needing human approval before execution.
- **`@property`** = a method accessed like an attribute.

---

> "Note that you can also select which tools require approval from the user before they can be run. We cover this behavior more extensively in Chapter 10, where we create a coding agent. We can then register the tool like so: `tools.add_tool(name="multiply", func=multiply, description="Multiplies two numbers: multiply(a: str, b: str)")`"

**Explanation:** Approval selection is available but detailed in Ch 10. Registration example shown.

### Tool Definition

> "After creating the tool, we need to inform the LLM what the tool does; this is called tool definition. Without communicating to the LLM which tools exist, what they do, how they are used, and in which context they should be used, the LLM isn't able to properly use them. There are several methods by which we can communicate this to the LLM: Learning, Prompting."

**Explanation:** Tool definition = telling the LLM about tools (existence, purpose, usage, context). Two channels: learning (training) and prompting (in the prompt).

**Word meanings:**
- **context** = the situations in which a tool is appropriate.

---

> "How to use (specific) tools and when they are needed is often learned during the fine-tuning stage of an LLM, where the model learns how to follow instructions and use tools. This learning stage can be split up into two aspects: we either tune the model to learn about specific tools or how to use tools in general. Learning about specific tools can be costly and limit how many tools you can add to the model. Instead, recent models (such as Qwen 3 and DeepSeek v3.2) typically focus on their improved instruction-following capabilities and general tool use. Instead of learning about specific tools, they learn to recognize definitions of tools in their prompts and dynamically use them."

**Explanation:** Two learning strategies: (1) learn specific tools (costly, limits count); (2) learn *general* tool use — recognize tool definitions in the prompt and use them dynamically. Modern models (Qwen 3, DeepSeek v3.2) take the general route.

**Word meanings:**
- **costly** = expensive (in data/training).
- **dynamically** = at runtime, adapting to whatever tools appear.

**Technical terms:**
- **fine-tuning** = continued training of a pretrained model on new data.

---

> "Using natural language: As (reasoning) LLMs are becoming easier to steer and dynamically learn to use tools, we can share the tool definitions through prompts. When you have a small set of tools the LLM should use, you can share that through the system prompt."

**Explanation:** Natural-language definition = describing tools in prose inside the system prompt. Good for small tool sets.

---

> "Note that the definition here isn't following a standardized procedure. Frankly, this is something we made up on the spot to show you that an LLM can call a tool anyway you decide. Assuming the model is capable enough of following those instructions, if it uses 'TOOL(value1, value2)' or perhaps structures like 'TOOL, value1, value2' it does not matter. The only thing you need is a small piece of code that recognizes when a tool is being called. This is called a validator or parser and requires the LLM to closely follow the usage instructions. It can therefore be quite error-prone because small errors in following the instructions might result in incomplete calls to the tools."

**Explanation:** Natural-language formats are arbitrary (any format works). You then need a parser/validator to detect the call — which is fragile, since small format slips break calls.

**Word meanings:**
- **standardized** = following an agreed format.
- **error-prone** = likely to contain errors.
- **incomplete calls** = calls that can't be executed because of malformed text.

**Technical terms:**
- **parser / validator** = code that detects and extracts a tool call from text.

**System prompt example (paraphrased):**
```
# Tool Definition
You can use the following tools:
- multiply(a, b): multiplies two numbers
- divide(a, b): divides a by b
# Tool Usage
When you need to use them, write: [function_name(value1, value2)]
```

---

> "Let's start by extending the Tools class so it can create this system prompt. The updated Tools class now has two additional functions: descriptions (a description of all registered tools) and prompt (the instructions sent to the LLM on how to call the tools)."

**Code (paraphrased):**
```python
class Tools(Tools):
    @property
    def descriptions(self) -> str:
        return "\n".join(f"`{tool}`: {self.registry[tool]['description']}" for tool in self.registry)

    @property
    def prompt(self) -> str:
        return f"""# Tools
If needed, you can only use the following tools to assist you in completing tasks:
{self.descriptions}
To use a tool, respond with JSON: {{"tool": "name", "kwargs": {{"param": "value"}}}}"""
```

**Explanation:** New `descriptions` lists all tools; new `prompt` builds the system prompt instructing the LLM to reply with JSON `{"tool": "name", "kwargs": {"param": "value"}}`.

**Word meanings:**
- **registry** = the stored tools dictionary.

**Technical terms:**
- **JSON tool-call format** = the standard structure the LLM outputs: tool name + keyword arguments.

---

> "Which gives us: `# Tools ... \`multiply\`: Multiplies two numbers ... \`get_weather\`: Gets weather ... To use a tool, respond with JSON: {"tool": "name", "kwargs": {"param": "value"}}`. This is the system prompt that the model will always have access to."

**Explanation:** Resulting prompt includes every registered tool's description plus the JSON instruction. Always in the model's context.

---

> "Using structured function calling: Another method of sharing the definition of the tools with the LLM is through structured function calling. Instead of messing around with the prompts ourselves and optimizing how we describe and format each tool, we can define each tool in structured schemas instead. These are typically JSON Schema objects that include the function's name, description, and its parameters (types, required fields, ranges, etc.)."

**Explanation:** Structured function calling = define each tool as a JSON Schema (name, description, parameter types, required fields). No hand-crafting prompts.

**Technical terms:**
- **JSON Schema** = a standardized format describing the structure of JSON data.

**Tool schema example (paraphrased):**
```json
{
  "type": "function",
  "function": {
    "name": "multiply",
    "description": "Multiply two numbers",
    "parameters": {
      "type": "object",
      "properties": {"a": {"type": "number", "description": "First number"},
                     "b": {"type": "number", "description": "Second number"}},
      "required": ["a", "b"]
    }
  }
}
```

---

> "These schemas are often passed as a separate parameter when using external APIs. OpenAI, for instance, uses the tools parameter where you can send over the JSON schemas of your tools. That allows the LLM or even the agent to treat it as special metadata and process it beforehand if necessary. In practice, however, these schemas are typically processed as if they were 'regular' prompts and put into, for example, the system prompt."

**Explanation:** Providers like OpenAI accept schemas via a dedicated `tools` parameter. In practice they end up treated like prompt text (e.g., in the system prompt).

**Word meanings:**
- **metadata** = data about data (here: description of the tool).

---

> "Although we are using structured JSON schemas for communication, the LLM still has to interpret how to use the tool and when, which makes tool usage a difficult task. Therefore, the more descriptive and clear the schema, the more likely the LLM will make the right tool call decisions."

**Explanation:** Even with schemas, the LLM must decide *when/how* to call. Clearer schemas → better calls.

---

> "At this point, the user can finally ask their question. Since they have access to the multiply function, let's keep the query simple: 'what is 5.1 times 7.3?' To illustrate how this is going to be processed, we make use of the messages structure that we explored in Chapter 4."

**Explanation:** With tool defined, the user asks "what is 5.1 times 7.3?" — processed through the messages structure (system/user/assistant roles) from Ch 4.

---

> "Regardless of whether you use a JSON schema or describe the tool, there are several best practices for defining functions to take into account:"

**Best practice 1 — Document your tool extensively:**
> "Document your tool with extensive descriptions of parameters, function names, and even examples. This allows your agent to have a better understanding of what the tool is capable of."

**Best practice 2 — Minimize the number of tools:**
> "Although having many tools expands the capabilities of your agent, it will become much more difficult to select and use the appropriate tool. Although there is no gold standard, aim to minimize the number of tools rather than maximize them. Using fewer than 10 tools is a good start. The next section, tool selection, becomes more important as the number of tools grows."

**Explanation:** Many tools → harder selection. Aim for fewer than 10.

**Best practice 3 — Minimize the scope of a tool:**
> "Complex tools with many parameters are difficult to use, even for individuals, let alone an agent. Having a function with dozens of parameters can be difficult to use, especially for smaller models."

**Explanation:** Few parameters per tool — dozens of params overwhelm especially small models.

### Tool Selection

> "Now that the tools are created and defined, we can start the process of calling the tool. The LLM first needs to select the right tool for a given query, which can be a difficult task. Especially with potentially dozens of complex tools available, the LLM not only needs to select the most appropriate one (if one at all) but also use it correctly. Although we can share an extensive JSON schema for each tool, the LLM needs to be capable enough to actually follow it through. This is where reasoning LLMs shine. They can spend any number of tokens 'thinking' about which tools to use and how to properly use them as long as they fit within the context window and leave enough for generating the answer."

**Explanation:** Tool selection = the LLM picks the right tool (and uses it correctly). Reasoning LLMs excel here — they can "think" about tool choice using tokens within the context window.

**Word meanings:**
- **shine** = perform especially well.
- **context window** = the amount of text the model can consider at once.

---

> "Note that discovering the tools that exist or might be relevant becomes more important when the number of tools increases. As we discussed in Chapter 4, even when you have a large context window, filling it up to the brim with tool JSON schemas is bound to decrease the LLM's performance. As with context engineering, the process of discovering tools might be helped with methodologies like RAG, where you store all tool schemas in a separate database for the LLM to discover."

**Explanation:** With many tools, discovery matters. Stuffing all schemas into context hurts performance — RAG over a tool-schema database helps the LLM find relevant tools.

**Word meanings:**
- **to the brim** = completely full.
- **bound to** = certain to.

**Technical terms:**
- **RAG** = Retrieval-Augmented Generation: retrieve relevant snippets (here, tool schemas) from a database rather than loading everything.

---

> "Note that the planning capabilities of LLMs are vital to having a good selection. For simple queries, such as 'What is 1 + 1?', the selection of tools is not going to be a challenge. However, when planning a vacation with a budget, a multi-step process is going to be needed where the agent first needs to plan out its behavior. This planning (and reflection) behavior is going to be discussed in Chapter 6 in more detail."

**Explanation:** Hard tool-selection problems need planning; that's Ch 6's topic.

### Tool Calling

> "Next, when the LLM ends its thinking process, it can start creating the tool call. It will output a string, and if the LLM is capable enough, correctly format the tool call with the given arguments."

**Explanation:** Tool calling = the LLM outputs a (formatted) string representing the call.

---

> "However, this answer is merely a string and will not execute the tool. In fact, you can view this output as merely the intention of the LLM to call a tool. Without any help from the user or additional software, nothing will happen. We will have to extend the Tools class further with functions that extract and parse the JSON call."

**Explanation:** The string is only intent. The `Tools` class must parse the JSON and execute.

**Technical terms:**
- **parse** = convert a string into structured data (JSON object).

---

**Code (paraphrased) — `parse` and `execute`:**
```python
class Tools(Tools):
    def parse(self, response: Response) -> Response:
        text = response.content
        if '"tool":' in text:
            start, end = text.find("{"), text.rfind("}") + 1
            tool_call = json.loads(text[start:end])
            return Response(content=response.content, reasoning=response.reasoning,
                            tool_call=tool_call)
        return response

    def execute(self, response: Response) -> Any:
        tool_call = response.tool_call
        name, kwargs = tool_call["tool"], tool_call.get("kwargs", {})
        if name in self.registry and name in self.requires_approval:
            response = input(f"Allow {name}? [y/N] ").strip().lower()
            if response not in ("y", "yes"):
                return f"Tool '{name}' was denied by the user."
        if name in self.registry:
            return self.registry[name]["function"](**kwargs)
        return f"Tool '{name}' not found."
```

**Explanation:** `parse` extracts JSON from the string into a `tool_call`. `execute` runs the tool: checks approval (human-in-the-loop), looks up the function, calls it with the kwargs; returns a message if denied or not found.

**Word meanings:**
- **human-in-the-loop** = a person approving/denying before execution.

---

> "The updated Tools class now has two additional functions: parse (Parses the string into JSON) and execute (Executes the tool call. Note that we also add a human-in-the-loop that allows you to approve certain tools before execution. We cover the human-in-the-loop more extensively in Chapter 10 where we create a coding agent)."

---

> "We reinitialize the Tools class and reregister the tool. Then, let's demonstrate with an example of how the tool would be executed: `tools.execute(response)` → `20.150000000000002`. Great! The Response object contains the content of the model and parses that into a tool call. After parsing, the JSON can be used to actually call the tool."

**Explanation:** Demo: parse a fake LLM response, execute it → 20.150000000000002 (float imprecision from 3.1 * 6.5).

---

> "NOTE: Although regular expressions can be used to extract our tool, they will not work if the tool call has a very slightly different structure or if the JSON structure is more complex. Instead, more robust techniques might be needed to extract the arguments, such as jsonschema, which is a Python package for validating JSON schemas. Another common technique is to use Pydantic, a Python package for data validation. It can be used to create data classes where each piece of data can be validated. It allows you to create JSON schemas of your tools to feed to the LLM. Likewise, the output of the LLM can be converted to JSON and validated through Pydantic."

**Explanation:** Regex is fragile. Better: `jsonschema` (validates JSON structure) or `Pydantic` (typed data classes that both define schemas and validate outputs).

**Technical terms:**
- **regex** = regular expressions, pattern-matching text.
- **jsonschema** = Python package for JSON validation.
- **Pydantic** = Python library for data validation via type-annotated classes.

### Tool Output Processing

> "We still need to do something with the output of the tool call. In our example, the LLM ends with a tool call, which we process and call ourselves. To feed the output back into the LLM, we can use the messages structure that we explored in Chapter 4. Specifically, we can add messages with two roles: assistant (The assistant calls the multiply tool) and tool (The output of the tool is returned)."

**Explanation:** After execution, the result must go back to the LLM via messages: `assistant` role = the tool call; `tool` role = the tool's output.

---

> "By updating the messages to include this additional information, we're essentially informing the LLM that these steps were taken. Calling the tool was done outside of the LLM's view, and it therefore has no knowledge of what actually happened. As such, we pretend as if the LLM executed the tool while that was actually done by the user or an automated system."

**Explanation:** The LLM never saw the execution, so we insert the result as if the LLM had done it itself.

---

> "The specific tool role is used only by LLMs that were trained specifically to perform native tool calling (more on that in the section 'Tool Learning'). For LLMs that were not trained with specific tool calling tokens, we have to approach it a bit differently. Instead of using the tool role, we can use the user role and act as if we are explicitly telling the LLM what the output of the tool call was."

**Explanation:** `tool` role = only for native-tool-calling models. For others, use the `user` role with the result described as a user message.

---

**Code (paraphrased) — `observation` and `is_done`:**
```python
class Tools(Tools):
    def observation(self, result: str) -> tuple[str, str]:
        return "user", f"OBSERVATION: {result}"

    def is_done(self, response: Response) -> bool:
        if not response.tool_call:
            return True
        if response.tool_call["tool"] == "final_answer":
            response.content = response.tool_call.get("kwargs", "")
            return True
        return False
```

**Explanation:** `observation` wraps the result as a `user` message with an "OBSERVATION:" prefix. `is_done` is the stopping mechanism: stop when there's no tool call, or when the agent calls the `final_answer` tool (whose kwargs become the final content).

**Word meanings:**
- **stopping mechanism** = the condition that ends the agent loop.

**Technical terms:**
- **`final_answer` tool** = a special tool the agent calls to return its answer and finish.

---

> "The updated Tools class now has two additional functions: observation (Returns the output of the tool call) and is_done (Stops your TinyAgent if there is no tool call or it calls the final_answer tool. In Chapter 6, we'll explore the final_answer tool in more depth when the agent has to decide how to give back a final answer). Of note is the observation function, which returns the role (user) and the output of the tool call as an 'OBSERVATION' tag. This 'OBSERVATION' tells the model that the user has observed the tool call and gives back its result."

---

> "We then feed these messages back into the LLM to create our final answer. Although the tool has given us the appropriate output, the LLM might want to combine/summarize answers or present them in a nicer way or according to pre-decided instructions."

**Explanation:** The LLM then reformats/summarizes the raw tool output into the final answer.

### TinyAgent with Tools

**Code (paraphrased):**
```python
class TinyAgent:
    def __init__(self, llm, memory, tools):
        self.llm = llm
        self.memory = memory
        self.tools = tools
        self.planner = None  # Chapter 6: Add Planning

        self.trajectory = Trajectory()
        system_prompt = "You are a helpful assistant.\n\n" + self.tools.prompt
        self.memory.add("system", system_prompt)

    def run(self, task):
        self.memory.add("user", task)
        self.trajectory.initialize(task)
        return self._step()

    def _step(self):
        response = self.llm.generate(self.memory.get_messages(), tools=self.tools.schemas)
        self.memory.add("assistant", response.content, tool_call=response.tool_call)
        response = self.tools.parse(response)
        if self.tools.is_done(response):
            self.trajectory.add(response)
            return response.content
        return self._execute_action(response)

    def _execute_action(self, response):
        result = self.tools.execute(response)
        role, observation = self.tools.observation(result)
        self.memory.add(role, observation)
        self.trajectory.add(response, observation)
        return observation
```

**Explanation:** TinyAgent now wires tools in: system prompt includes tool definitions; `_step` = generate → parse → check done → else execute; `_execute_action` = execute + record observation to memory and trajectory.

**Word meanings:**
- **trajectory** = the record of steps/actions taken.

---

> "Out of all the changes to your TinyAgent, this chapter and the next include the largest set of them throughout the book. So, let's go through them one by one!"

**The system prompt:**
> "In the __init__ of your TinyAgent, we now add a system prompt. As we covered in the section 'Tool Definition', this serves as a nice way for the agent to keep track of what tools it has available. With self.tools.prompt, we add the tools' definitions to the system prompt as we did previously. As you will see in upcoming chapters (e.g., Chapter 6), the system prompt is going to be used for tracking more information than just tools."

**Explanation:** System prompt = central place to track tools (and later planner instructions).

---

> "NOTE: Note that the SummarizationMemory would actually not work well with Tools since it continuously overrides the system prompt to track the summarized conversation history. Tools requires keeping the tool definition in the system prompt; this would essentially remove any information the LLM has about tools! A solution would be to always update a section of the system prompt rather than replacing it entirely. Try it out yourself and see if you can update the SummarizationMemory so that it will work nicely with Tools."

**Explanation:** SummarizationMemory overwrites the system prompt, which would erase tool definitions. Fix: update only a section, don't replace the whole prompt.

**Word meanings:**
- **overrides** = replaces/overwrites.

---

**A single step:**
> "A single step (_step) consists of the following components:
> - Generate a response: The LLM generates a response with self.llm.generate. You can call this the 'THOUGHT' of the LLM, as it contains what it thinks about the current situation and how it would like to act.
> - Tool parsing: If there is a tool call, it is parsed with self.tools.parse. As we covered in the section 'Tool Calling', this converts the string to a structured JSON output.
> - Tool execution: If there is a parsed tool call, it is executed with _execute_action.
> - Stopping mechanism: If there is no tool call, we stop execution entirely. This is not behavior we need at this moment since your TinyAgent does not yet run autonomously. However, it will become important in the next chapter, where the agent can run continuously."

**Explanation:** One step = generate (THOUGHT) → parse → execute → possibly stop. The stop behavior matters in Ch 6 (autonomy).

---

**Executing an action:**
> "Executing an action (_execute_action) consists of two steps:
> - Execute tool call: Executing the JSON tool call with self.tools.execute(response).
> - Track the result: Track the output of the tool call. The LLM has to know what the output is in order to decide what to do next. As we covered in the section 'Tool Output Processing', we do this by adding the output as the user role and describing it with the prefix 'OBSERVATION.'
> Finally, we track the state of the agent with the Trajectory module and the conversation history with the Memory module."

**Explanation:** Execution = run the tool + feed the result back (as user/OBSERVATION). Trajectory and Memory track everything.

---

> "NOTE: You might have noticed various capitalized words scattered throughout the code in this chapter. In particular, 'THOUGHT', 'OBSERVATION', 'ACTION', and 'ANSWER'. These are all steps that relate to the Reason and Act (ReAct) framework that we will use in Chapter 6 to create autonomous behavior. As you might have guessed already, these mean the following:
> - THOUGHT: The reasoning of the LLM on what to do next.
> - ACTION: The action or tool calls of the LLM are structured as JSON.
> - OBSERVATION: The output of the ACTION the LLM took.
> - ANSWER: The final answer of the LLM after running for one or more steps."

**Explanation:** The capitalized words map to ReAct (Ch 6): THOUGHT/ACTION/OBSERVATION/ANSWER.

**Technical terms:**
- **ReAct** = Reason and Act — a framework interleaving reasoning and acting.

---

**Running your TinyAgent with tool calling capabilities:**
> "Now that you have a TinyAgent with tools, we can initialize it. Here, we choose a no-frills memory and simply track the entire conversation history: `llm = LLM(model="gemma3:12b")` ... `agent = TinyAgent(llm=llm, memory=memory, tools=tools)`."

**Explanation:** Setup uses Gemma 3 12B (no native thinking or tool calling), plain memory, registered multiply tool.

**Word meanings:**
- **no-frills** = plain/simple.

---

> "Let's run a simple query to see if it can properly choose, parse, and execute its tool: `agent.run("What is 5.1 times 7.3?")` → `'OBSERVATION: 37.23'`. This is the correct answer, but did the model do that calculation by itself, or did it use a tool? To find out, let's inspect the full conversation history."

**Explanation:** Query returns the observation 37.23. Inspecting memory reveals whether a tool was used.

---

> "It used the tool! Note that the assistant role returned a string with a JSON-like structured output. Moreover, the user role was added with an observation of this output."

**Explanation:** Conversation shows: system prompt + user query + assistant JSON tool call + user OBSERVATION.

---

> "With the many steps an LLM has to correctly follow to call the appropriate tool, it requires the LLM to be an effective orchestrator, which is dependent upon the model's reasoning capabilities and overall reliability (as explored in Chapter 3). A more stable approach tends to be to train a model on specific tool-calling tasks so it can generalize better. Before, we briefly mentioned the use of the tool role. In the next section, we explore in-depth how models learn to use that."

**Explanation:** Prompt-based calling depends on model reliability. Training on tool-calling tasks generalizes better — the topic of Tool Learning.

**Word meanings:**
- **orchestrator** = something that coordinates many parts.
- **generalize** = apply learning to new, unseen cases.

---

## 5.3 Tool Learning

> "Instilling tool-calling capabilities into LLMs can be a difficult task, especially when an LLM has not been trained to do so. Learning tools can be achieved through various methodologies. In this section, we'll explore the three most common categories of tool learning, namely in-context learning, supervised fine-tuning, and reinforcement learning."

**Explanation:** Three tool-learning categories: in-context learning, SFT, RL.

**Word meanings:**
- **instilling** = putting a capability into.

### In-context Learning

> "In-context learning, the ability of LLMs to learn from a few examples in their context, is an exceptionally useful technique to enable new behavior in LLMs without the need to fine-tune them. As we covered in Chapter 3, it is also known as few-shot prompting or few-shot learning, where you provide several examples to the LLM to learn from. Specifically, it's a prompt engineering technique where you give some examples of the behavior that you want the LLM to repeat. In our case, and as illustrated in Figure 5-13, we want the LLM to output the tool call in a specific format by following the examples we created ourselves."

**Explanation:** In-context learning = few-shot prompting: give example tool calls in context so the LLM mimics the format — no fine-tuning.

**Technical terms:**
- **few-shot prompting** = providing a few examples in the prompt to guide behavior.
- **in-context learning** = learning from examples present in the input context.

---

> "In-context learning is especially helpful when leveraging the messages structure that we explored previously. Remember when we pretended the assistant called a tool by adding it to the messages? We can apply the same concept and act as if the model called tools before. As shown in Figure 5-14, we can create our messages in such a way that the model thinks it has already used some of the tools before. So, when it subsequently gets a query, it has examples of how the tools should be called and what kinds of output can be expected."

**Explanation:** We can fake conversation history where the assistant already called tools — giving the model examples of call format and expected outputs.

**Word meanings:**
- **leveraging** = making use of.
- **fake** (history) = fabricated example messages.

---

> "Since LLMs are great at pattern recognition, providing these example tool calls helps them follow the structure of the tool call we had in mind. Note that in-context learning improves as the LLM becomes more capable."

**Explanation:** LLMs pick up patterns from examples; more capable models benefit more.

### Supervised Fine-tuning (SFT)

> "Although prompting is a straightforward method for enabling tool usage, it does require filling up the context window with additional instructions. As we explored in Chapter 4, we want to prevent filling up the context window, as it might degrade the performance of your model. There is also a risk of the model not following instructions through prompting if you have too many instructions. A great alternative is to train your model using supervised fine-tuning (SFT) to distill knowledge and capabilities on tool calling into the model itself. This was especially popular in 2023 and early 2024 because it proved to be an effective and relatively cheap method for adding tool calling capabilities. However, it does come with downsides that were addressed with reinforcement learning (RL), as we will discuss later."

**Explanation:** Prompting fills context and can fail with too many instructions. SFT bakes tool-calling into model weights (cheap, popular 2023–early 2024) but has downsides RL fixes.

**Word meanings:**
- **degrade** = worsen.
- **distill** = transfer knowledge into.

**Technical terms:**
- **supervised fine-tuning (SFT)** = training on labeled examples of the target behavior.

---

**Toolformer:**
> "An interesting technique to explore SFT for tool calling (including some other tricks) is Toolformer, a model trained to decide which APIs to call and how. Before exploring the training process, let's first dive into their approach to calling a tool. In previous examples, the tool that should be called was structured using JSON schemas, and the output of the call was sent back to the LLM to process. Toolformer takes a different approach by embedding the call and its output in the text it's generating. It does so by using the [ and ] tokens to indicate the start and end of calling a tool."

**Explanation:** Toolformer differs: it embeds tool calls *inline in the generated text* using special tokens, instead of separate messages.

**Word meanings:**
- **embedding** = inserting within.

**Technical terms:**
- **Toolformer** = an SFT model trained to decide which APIs to call and how.

---

> "When given a prompt, 'What is 5.1 times 7.3?', it starts generating tokens until it reaches the [ token (shown in Figure 5-15). The [ token tells the LLM that it should start selecting the tool and generate the appropriate parameters following a fixed format. It does this until it reaches the → token (Figure 5-16). The → token tells the LLM that it can now completely stop generating tokens if the user or an automated system executes the selected tool. Instead of appending the output through the messages structure as we explored before, it's added to the previously generated tokens along with the ] token (shown in Figure 5-17). The ] token tells the LLM that the tool has been executed and that the tool call, along with the output, is now part of the previously generated tokens. Then, the LLM can decide to continue generating tokens if necessary."

**Explanation:** The three special tokens:
- `[` = start selecting tool + params.
- `→` = stop generating; the system executes the tool.
- `]` = tool executed; the call + output are now part of the generated tokens.

**Example (input/output):**
> "Input: 'What is 5.1 times 7.3?'
> Output: '5.1 * 7.3 is [Calculator(5.1*7.3) → 37.23] The answer is 37.23.'"

---

> "This process of adding the output while generating tokens is an important construct and is often used in various models. By embedding the output during generation, the model uses tools more naturally and generates fluent language. It also lends itself quite well for reasoning LLMs, where tools are called during the reasoning process instead of having to go back and forth between messages (remember Search-o1 from Chapter 4?)."

**Explanation:** Inline tool calling = more natural, fluent; fits reasoning LLMs that call tools mid-reasoning (like Search-o1).

**Word meanings:**
- **lends itself** = is well suited for.
- **fluent** = smooth and natural.

---

> "To enable this in-line tool calling behavior in Toolformer, the model needs to be fine-tuned first. To do so, the authors carefully generated a dataset with many different examples of tool use. For each tool, a few-shot prompt was manually created and used to sample outputs. By sampling different outputs (much like the sampling with search against verifiers we explored in the reasoning chapter), the best output could be extracted by filtering on the correctness of the tool use, output, and loss decrease."

**Explanation:** Training data: for each tool, few-shot prompts sample multiple candidate calls; filter by (1) correctness of tool use, (2) output correctness, (3) loss decrease.

**Word meanings:**
- **verifiers** = automated checkers of outputs.
- **loss decrease** = whether including the call reduces the model's training loss.

---

> "NOTE: In the context of SFT, getting quality data for tool calling can be a challenge. For many prompts, there is not a single correct tool call. Multiple tools, argument formats, or call sequences may all lead to valid outcomes. For instance, if we want to extract the README.md from a given GitHub repository, a model could call `get_readme(repo="owner/repo")` or it could use `get_contents(path="README.md", repo="owner/repo")`, both of which return the same file. Likewise, quality data often assumes that tools never fail, while in reality, tools may fail, return partial errors, change schemas, or behave inconsistently. These problems are less pronounced when using RL, which we cover in the next section."

**Explanation:** SFT data is ambiguous (multiple valid calls) and unrealistic (tools assumed never to fail). RL handles this better.

**Word meanings:**
- **ambiguous** = having multiple valid interpretations.
- **pronounced** = noticeable/severe.

---

> "In other words, the original data was adjusted to include tool calls instead of the LLM generating the answer by itself. All inputs were essentially updated to contain the appropriate tool. Finally, the LLM (GPT-J, a variant of GPT-3) was fine-tuned using this updated data using SFT. At the time, this technique showed significant improvements over zero-shot performance and competed with larger models. However, SFT on this data made generalization difficult. SFT tends to be sensitive to the exact wording and prompt that is being used because it attempts to re-create what it is being shown. As we will explore next, RL tends to be a much more stable technique for generalization in tool use."

**Explanation:** GPT-J was fine-tuned on tool-augmented data. Results beat zero-shot, competed with larger models — but SFT generalizes poorly (sensitive to wording). RL generalizes better.

**Word meanings:**
- **zero-shot** = no examples, direct answer.
- **wording** = exact phrasing.

**Technical terms:**
- **GPT-J** = an open-source GPT-3 variant.

### Reinforcement Learning (RL)

> "As we explored in Chapters 2 and 3, RL is an excellent method of training or fine-tuning to align your model to certain rewardable tasks. Compared to SFT, where an LLM trains on fixed examples with the correct answer, RL relies on trial and error. LLMs, through SFT, tend to mimic the input data and have difficulties generalizing what they learned. In RL, LLMs develop improved reasoning strategies not from being told exactly what to do (the mimicking behavior of SFT) but from repeatedly exploring feedback signals."

**Explanation:** RL = trial and error with feedback signals, vs SFT = mimic fixed examples. RL gives better generalization.

**Word meanings:**
- **trial and error** = trying actions and learning from outcomes.
- **feedback signals** = rewards/penalties.

---

> "In the context of RL for tool usage, tool-learning is often integrated into the thinking process of models that support advanced reasoning. This is called Tool-Integrated Reasoning (TIR), which involves incorporating tools into the reasoning traces of an LLM. Figure 5-20 illustrates this process of calling a tool during reasoning and continuing the reasoning process after receiving the output. Note that such a TIR trajectory might involve multiple tool invocations, where the final answer is determined by all these intermediate tool calls and outputs."

**Explanation:** TIR = tools embedded inside reasoning traces; a trajectory may invoke tools multiple times before the final answer.

**Technical terms:**
- **Tool-Integrated Reasoning (TIR)** = calling tools within the reasoning process.
- **trajectory** = the full sequence of reasoning/actions/outputs.

---

**ToolRL:**
> "A recent example showcasing how RL can be used to enable TIR is ToolRL. This framework uses GRPO, which we briefly covered in DeepSeek-R1's training procedure in Chapters 2 and 3. To differentiate between stages of thinking, tool calling, and answering the query, the developers used the <thinking></thinking>, <tool_call></tool_call>, and <answer></answer> tokens, respectively."

**Explanation:** ToolRL uses GRPO (as in DeepSeek-R1) and separates thinking/tool-calling/answering with special tokens.

**Technical terms:**
- **GRPO** = Group Relative Policy Optimization — an RL algorithm.
- **ToolRL** = an RL framework for tool-integrated reasoning.

---

> "Compared to DeepSeek-R1, their usage of rewards in GRPO is quite straightforward. Two rewards are defined to enable tool usage:
> - Correctness: Is a tool called correctly? Scores are given based on whether the correct tool names and parameters were used.
> - Format: Is the appropriate format used? A positive reward is given if all required fields appear in the correct order.
> To train the model, 4,000 samples of TIR traces were sampled from various datasets and used for fine-tuning various Qwen2.5 models."

**Explanation:** Two rewards: correctness (right tool + params) and format (fields in correct order). Trained on 4,000 TIR traces over Qwen2.5 models.

**Technical terms:**
- **correctness reward** = reward for choosing the right tool and arguments.
- **format reward** = reward for correct field ordering/structure.

---

> "Interestingly, the authors also experimented with length rewards to encourage longer reasoning traces but found that longer traces do not consistently improve task performance and may even harm smaller models. Long reasoning traces might therefore not be ideal for tool use tasks."

**Explanation:** Length rewards didn't help consistently and can hurt small models — long traces aren't ideal for tool use.

---

> "Note that GRPO is a very flexible framework and allows you to develop the rewards that are best suited for a given use case. As such, this strategy of using tool-based rewards in GRPO can also be used for non-reasoning models by simply removing or updating the format reward."

**Explanation:** GRPO is flexible; tool-based rewards adapt to non-reasoning models by dropping/altering the format reward.

---

**Search-R1:**
> "To further explore what RL is capable of, let's take a closer look at Search-R1, an efficient RL framework for integrating search as a tool into an LLM's reasoning process. In this framework, the LLM learns to generate one or more search queries during step-by-step reasoning autonomously."

**Explanation:** Search-R1 = RL framework that teaches the LLM to autonomously issue search queries during reasoning.

**Technical terms:**
- **Search-R1** = RL framework for search-integrated reasoning.

---

> "The framework starts with specifying how the model should interleave reasoning with the search engine call. As illustrated in Figure 5-22, the prompt template is structured into three parts. First, the reasoning traces are created with <think></think> tokens, then the search engine calling function with <search></search> where the output is reintegrated with <information></information>, and finally, the answer through <answer></answer> tokens. Note that the reasoning traces and the search engine can be interleaved several times."

**Explanation:** Prompt template uses `<think>`, `<search>` (with `<information>` for reintegrated output), and `<answer>` tokens; reasoning and search interleave multiple times.

---

> "What makes this template particularly interesting is that the authors focus on a single tool, search. The reason for this was the upcoming popularity of DeepResearch, a framework where reasoning LLMs are coupled with search engines to create agentic systems that allow for in-depth research on various topics. The authors of Search-R1 created this framework as a strong open source alternative to the proprietary systems out there."

**Explanation:** Single-tool focus (search) mirrors DeepResearch; built as an open-source alternative.

**Word meanings:**
- **proprietary** = closed/private systems.

---

> "The result of such a template is, like ToolRL, tool-interleaved reasoning with multi-turn search engine calls. Note that the <search></search> tool can be any application, like the open-access archive of academic papers, arXiv, or a combination of various sources."

**Explanation:** Result = tool-interleaved reasoning with multi-turn search; `<search>` can target arXiv or combined sources.

---

> "The approach of training the algorithm with RL is rather straightforward. The authors adopted a simplified outcome-based reward function. Instead of creating all different kinds of formatting and accuracy rewards (like DeepSeek-R1), only accuracy rewards were used (based on the task). Since the underlying model (Qwen-2.5) already has strong structural adherence, there was no need for formatting rewards."

**Explanation:** Only accuracy (outcome-based) rewards — Qwen-2.5 already follows structure, so no format reward needed.

**Word meanings:**
- **outcome-based** = reward based on final result.
- **structural adherence** = following the required format reliably.

---

> "Among others, the authors explored GRPO (which we covered in Chapter 2) as the RL algorithm for fine-tuning the model. Note that the losses in GRPO are typically calculated over the entire sequence of tokens, including the output of the search engine. In Search-R1, the tokens of the search engine's output were masked (ignored) to prevent the model from attempting to control the search engine's output, which were not directly LLM-generated (which can create unexpected dynamics). This is called loss masking for retrieved tokens."

**Explanation:** GRPO normally computes loss over all tokens, including search output. Search-R1 *masks* search output tokens so the model can't try to influence tokens it didn't generate.

**Technical terms:**
- **loss masking** = ignoring certain tokens (here, retrieved ones) when computing gradients.
- **unexpected dynamics** = unstable training behavior from trying to optimize non-model tokens.

---

> "These kinds of frameworks and methodologies leveraging RL have become increasingly popular as the reward structure for tool calling is generally quite straightforward. It's trivial to check if a tool has been called correctly and if the correct arguments have been used (as in ToolRL). Moreover, RL works well for rewards that are verifiable, such as coding and tool calling. As such, there has been an increase in models that were trained using RL and additionally adopted tool-based rewards, such as the strong open source Qwen3 and GPT-OSS models."

**Explanation:** RL for tools is popular because correctness is easy to verify. Examples: Qwen3, GPT-OSS.

**Word meanings:**
- **trivial** = very easy.
- **verifiable** = can be automatically checked.

---

## 5.4 TinyAgent with Native Tool-Calling Capabilities

> "SFT and RL are great techniques for instilling tool-calling behavior into the models themselves without needing to perform explicit CoT instructions in your prompts. This process, as illustrated previously, does create a bit of 'magic' when running these tool-calling capabilities with inference engines like Ollama or llama.cpp. This 'magic' we refer to is the conversion of a standard method for calling tools to the chat template that the specific LLM uses. As we explored previously, some LLMs use <think> tokens for instance, while others use something like <|think|>. Each LLM is trained differently, with different tokens, and the inference engine 'hides' that from the user. Although that makes for an easy UX, it doesn't serve well for the educational nature of this book. So let's uncover the 'magic!'"

**Explanation:** Inference engines (Ollama, llama.cpp) transparently convert standard tool definitions into each model's chat template tokens. The book removes this abstraction to teach what's happening.

**Word meanings:**
- **magic** = hidden complexity the engine handles for you.
- **UX** = user experience.

**Technical terms:**
- **chat template** = the specific token format a model was trained on.
- **inference engine** = software that runs the model (Ollama, llama.cpp).

---

> "Most inference engines make use of a standardized format in describing tools, namely JSON. As we discussed in the section 'Tool Definition', we can use a JSON schema to describe a tool and pass it to the LLM for it to use. As such, all we have to do is use the tool_schema variable we defined in the section 'Tool Definition' and pass it along to the OpenAI endpoint. We make use of Gemma 4 E4B, which unlike Gemma 3 12B, was trained specifically to perform tool calling."

**Explanation:** Pass the JSON `tool_schema` to the OpenAI-compatible endpoint; Gemma 4 E4B was trained for native tool calling (unlike Gemma 3 12B).

**Technical terms:**
- **OpenAI endpoint** = an API compatible with OpenAI's chat/tool format.
- **Gemma 4 E4B** = a model trained for native reasoning and tool calling.

---

> "Note how the output 'just' outputs the tool call without us having to do anything special. This is because Ollama handles various things, but two stand out. First, it communicates to the LLM which tools exist. Second, it converts the LLM's tool call to this parsed output."

**Explanation:** Response contains `tool_calls` directly — Ollama communicated tools to the model and parsed its call.

---

> "To uncover the 'magic,' let's start from Gemma 4 E4B's chat template. As shown in Figure 5-25, it uses several special tokens (e.g., <|turn>) to parse the input and output of the model. To define a given tool, such as the multiply function we covered previously, it expects the format as shown in Figure 5-26 (the tool declaration between the <|tool> and <tool|> tokens). Under the hood, inference engines convert the JSON schema we used in tool_schema to whatever the LLM expects. How this translation is done is up to the inference provider, but it always needs to convert to whatever the LLM is trained on."

**Explanation:** Gemma 4 E4B uses special tokens like `<|turn>` and `<|tool>`/`<tool|>`. Engines translate our JSON schema into the model's expected token format.

---

**tool_to_schema:**
> "Now, let's explore how we can use the native tool-calling capabilities of an LLM and use them in your TinyAgent. Among others, the following needs to be implemented:
> - Creating a tool_to_schema function: A function for converting a Python function to an OpenAI-compatible JSON schema
> - Creating the NativeTools class: A variant of Tools that creates and parses tool calls
> Let's start with the tool_to_schema function. It is a helper function that easily allows us to convert any given function to a JSON schema by inspecting the function's name, docstrings, and parameters."

**Code (paraphrased):**
```python
TYPE_MAP = {str: "string", int: "integer", float: "number", bool: "boolean", list: "array", dict: "object"}

def tool_to_schema(function):
    signature = inspect.signature(function)
    properties, required = {}, []
    for name, parameter in signature.parameters.items():
        properties[name] = {"type": TYPE_MAP.get(parameter.annotation, "string")}
        if parameter.default is inspect.Parameter.empty:
            required.append(name)
    return {
        "type": "function",
        "function": {
            "name": function.__name__,
            "description": inspect.getdoc(function),
            "parameters": {"type": "object", "properties": properties, "required": required},
        },
    }
```

**Explanation:** `tool_to_schema` introspects a Python function (name, docstring, parameters via `inspect`) and produces an OpenAI-compatible JSON schema. `TYPE_MAP` maps Python types to JSON Schema type names; parameters without defaults become required.

**Word meanings:**
- **introspect** = examine a function's own metadata.
- **annotation** = the type hint on a parameter.

**Technical terms:**
- **`inspect`** = Python's module for examining live objects.

---

> "You can run it like so: `print(tool_to_schema(multiply))`. This gives the standard OpenAI-compatible format. This standardized format is OpenAI-compatible, and the inference engine will convert this to the model's chat template. Next, let's make the NativeTools class that creates this schema and parses the output so it can be used to execute tool calls."

**Code (paraphrased) — `NativeTools`:**
```python
class NativeTools(Tools):
    @property
    def schemas(self):
        return [tool_to_schema(tool["function"]) for tool in self.registry.values()]

    @property
    def prompt(self):
        return ""  # no prompt needed for native tool calling

    def parse(self, response):
        if not response.tool_call:
            return response
        args = response.tool_call["function"]["arguments"]
        if isinstance(args, str):
            args = json.loads(args)
        tool_call = {"tool": response.tool_call["function"]["name"], "kwargs": args}
        return Response(content=response.content, reasoning=response.reasoning, tool_call=tool_call)

    def observation(self, result):
        return "tool", str(result)  # native results use the 'tool' role

    def is_done(self, response):
        return not response.tool_call
```

**Explanation:** `NativeTools`:
- `schemas` = OpenAI-compatible schema list.
- `prompt` = empty (engine handles it).
- `parse` = converts the native `tool_call` into `{tool, kwargs}`.
- `observation` = uses the `tool` role (native).
- `is_done` = done when no tool call.

---

> "The NativeTools class follows Tools closely and makes a couple of changes:
> - schemas: Create an OpenAI-compatible schema in JSON.
> - prompt: No prompt is needed because that is handled by the inference engine.
> - parse: Converts the LLM's tool call into JSON and stores it in the Response class.
> - observation: The observation is now tracked in the tool role.
> Usage is exactly the same as the Tools class. All we need to do is add the tools and give them to your TinyAgent. Then, we can ask it the same query we did before to see if it uses a tool."

**Explanation:** Usage identical to `Tools`; after running, memory shows `tool_calls` (identifying tool + params) and the `tool` role observation.

---

> "The tool was called successfully! Note how it uses tool_calls to identify the tool and parameters while the tool role is used to observe the output."

---

## 5.5 Model Context Protocol (MCP)

> "In the previous sections, we explored how to connect tools to LLMs, making them capable of much more than text generation. Although their ability to then use tools is incredible, it's not a free lunch. Imagine you have developed several prompts to instruct your LLM on how to use tools. As is typical in this field, a new LLM is released that you would like to try out, together with all the tools. Now imagine it has a new way of calling tools, which means you will have to create new integrations for all your tools. This is the NxM problem, where N is the number of LLMs and M is the number of tools available. You would have to write custom integrations for every LLM/tool combination."

**Explanation:** Tools aren't a "free lunch": every new LLM may require new integrations for every tool — the **N×M problem** (N LLMs × M tools = custom integrations).

**Word meanings:**
- **free lunch** = benefit without cost.
- **integrations** = code connecting a tool to an LLM.

**Technical terms:**
- **N×M problem** = the multiplicative integration burden across models and tools.

---

> "Model Context Protocol (MCP) solves this problem by standardizing how you would connect tools and APIs with different structures to your LLM. MCP is an open standard and framework developed by Anthropic and released in November 2024. As a protocol, it facilitates two-way communication between tools and LLMs. It's often referred to as the 'USB-C port of AI' due to its universal nature, allowing for any LLM to implement any tool that follows this protocol. Shown in Figure 5-27, instead of manually creating connections between LLMs and tools, MCP creates only a single connection that can be maintained indefinitely by the tool provider."

**Explanation:** MCP (Anthropic, Nov 2024) standardizes tool↔LLM connections — the "USB-C port of AI." One connection maintained by the tool provider.

**Word meanings:**
- **facilitates** = enables.
- **indefinitely** = for an unlimited time.

**Technical terms:**
- **MCP** = Model Context Protocol — open standard for connecting tools to LLMs.
- **USB-C port of AI** = metaphor for a universal connector.

---

> "By having the MCP server handle the integrations and communicate the tools to the LLMs, it becomes N+M connections that need to be maintained instead of NxM connections. Moreover, as long as the tool provider has an MCP server, connecting the server to your LLM is relatively straightforward, but more on that later."

**Explanation:** N×M → **N+M** connections: each LLM connects once, each tool provider maintains one server.

---

> "The maintenance of the integration, therefore, also moves from the user to the tool provider. Without MCP, if arXiv's API were to suddenly change drastically, then each user would have to adjust their integrations. With MCP, any changes to the API would need to be resolved only once by the maintainers of that API. Then, the updates can be rolled out to all users without any intervention from their side."

**Explanation:** Maintenance shifts to tool providers: one API change is fixed once, benefiting all users.

**Word meanings:**
- **intervention** = manual action.
- **rolled out** = distributed/deployed.

---

> "To illustrate this point a bit further, if you were to add tools to your LLM manually, all tools would have to be:
> - Manually tracked and fed to the LLM
> - Manually described (including its expected JSON schema)
> - Manually updated whenever its API changes
> As shown in Figure 5-28, this can be quite the hassle for maintaining your tools."

**Explanation:** Manual tool integration = track, describe, and update everything by hand.

**Word meanings:**
- **hassle** = annoying burden.

---

> "Thus, MCP not only solves the NxM problem but also the problem of standardization. Note that MCP is not the only protocol for standardizing communication, like Agent2Agent (A2A) for standardizing interagent communication. Although there are others, in 2026, it is arguably one of the most popular protocols out there. There are MCP servers for Figma, GitHub, Home Assistant, and many others. You can find a nice overview of MCP servers on GitHub."

**Explanation:** MCP also solves standardization. Related but different: **A2A** standardizes agent-to-agent communication. MCP servers exist for Figma, GitHub, Home Assistant, etc.

**Word meanings:**
- **arguably** = probably.

**Technical terms:**
- **A2A** = Agent2Agent — protocol for inter-agent communication.

### Core Components

> "To achieve all these useful capabilities, MCP consists of three components:
> - MCP host: LLM application (such as Cursor) that manages connections, interprets tool schema, and manages routing
> - MCP client: Maintains one-to-one connections with MCP servers
> - MCP server: Provides context, tools, and capabilities to the LLMs"

**Explanation:** The three MCP components: host (LLM app), client (connection handler), server (tool provider).

**Technical terms:**
- **host** = the LLM application (ChatGPT, Claude, Cursor, GitHub Copilot).
- **client** = per-server connection code inside the host.
- **server** = lightweight program exposing tools/resources/prompts.

---

> "The MCP host is any application that uses an LLM to use external tools. Typical examples are chat assistants like ChatGPT or Claude and IDE extensions like Cursor or GitHub Copilot. This is the 'brain' of the MCP flow and it makes calls to the MCP servers via the MCP clients. The MCP client maintains connections with the MCP servers. They exist within the host and handle the connection management, discovery of tool capabilities, request forwarding, etc. Compared to the host, a client is a piece of code that handles the communication with the MCP servers, whereas the MCP host only initiates the communication. The MCP server is a lightweight program that exposes APIs and tools via the MCP standard. These servers often connect to a specific data source or service. For instance, an MCP server might connect to all API endpoints of arXiv to search, load, and view academic papers."

**Explanation:** Host = brain (initiates); client = per-server code (manages connections, discovery, forwarding); server = exposes tools/APIs (e.g., an arXiv server).

**Word meanings:**
- **initiates** = starts.
- **discovery** = finding what tools exist.

---

> "Servers expose three kinds of primitives to clients: tools (actions the LLM can invoke, like /list_commits), resources (data the host can load as context, like files, documents, and datasets), and prompts (reusable templates). To summarize, the MCP host (e.g., GitHub Copilot) contains one MCP client per server it connects to. The MCP servers expose tools that may provide access to resources."

**Explanation:** The three server primitives: **tools** (actions), **resources** (context data), **prompts** (reusable templates). One client per connected server.

**Technical terms:**
- **primitives** = the fundamental building blocks exposed by MCP servers.

---

### The MCP Flow

> "MCP can be a bit of a mystery, even when showing and describing the core components. Instead, let's go through an example of what it would be like to use the MCP to discover and call tools. Imagine you want your AI assistant (perhaps GitHub Copilot or Claude code) to summarize the five latest commits from your repository."

**Example flow (12 steps):**
> "It would all start with the user's query: 'Summarize the 5 latest commits'. Seen in Figure 5-30, this prompt is sent to the MCP host (1), which asks the MCP server, through the MCP client, which tools are available (2). The MCP server is connected to the set of tools (GitHub) and returns the list of all available API calls back to the MCP host (3). API calls might include common methodologies like listing all commits (/list_commits) or creating a pull request (/create_pr). Then, the initial prompt, together with available tools, is sent to the LLM (4)."

**Explanation:** Steps 1–4: query → host → (client) asks server for tools → server lists them → host sends prompt + tools to the LLM.

---

> "Next, the LLM may choose to use any of the tools that were returned. Since the user's query is about commits, the LLM decides that it wants the MCP server to use the /list_commits tool (5). The MCP client communicates this action to the MCP server (6), which finally executes the command (7). The output of the tool usage is returned to the MCP server (8), which communicates it back to the MCP client and host through the MCP (9)."

**Explanation:** Steps 5–9: LLM picks /list_commits → client forwards to server → server executes → output returns server → client → host.

---

> "When the LLM receives the results (10), it can choose to run another tool or return the output to the MCP host and then to the user. In our example, the LLM decides to summarize the five latest commits that it received (11) and return the summary to the user (12)."

**Explanation:** Steps 10–12: LLM processes results, summarizes, returns to host → user.

---

> "What makes these sets of steps so special is that the LLM can discover tools that exist, choose which one to use, and does not have to think much about deprecated API functionalities. Note that the LLM should still have tool-calling capabilities. Whenever it wants to execute a given tool, it should follow the MCP, which follows a JSON-like structure. This structure, following the JSON-RPC 2.0 Specification, is also communicated by the MCP client, which serves as the middleman between the LLM and the protocol."

**Explanation:** MCP lets the LLM discover/choose tools without worrying about deprecated APIs. Messages follow **JSON-RPC 2.0**, with the client as middleman.

**Word meanings:**
- **deprecated** = outdated, scheduled for removal.
- **middleman** = intermediary.

**Technical terms:**
- **JSON-RPC 2.0** = a JSON-based remote procedure call protocol.

---

## 5.6 Skills

> "With modules like tools and MCP, we can give an agent access to various actions it can take. However, when exactly to use those actions and how they fit into a larger workflow is not covered by any of these modules. Your agent is not intimately familiar with your team's workflow, preferred tools, or coding standards. Having to repeat to your agent, every time you initialize it, that it should use uv over pip can be quite the annoyance. This is where Skills come in."

**Explanation:** Tools/MCP give actions; Skills give *when* and *how* — your team's workflow, preferred tools, coding standards.

**Word meanings:**
- **intimately** = deeply.
- **annoyance** = irritating thing.

**Technical terms:**
- **Skills** = Anthropic's bundles of instructions, scripts, and resources for recurring tasks.

---

> "Created by Anthropic, Skills are a set of instructions, scripts, and resources the agent can use as context to perform a given task. They contain procedural knowledge, which are step-by-step instructions that allow the agent to understand how to best approach a task given in your specific context."

**Explanation:** Skills contain procedural (step-by-step) knowledge for your context.

**Technical terms:**
- **procedural knowledge** = knowledge of how to do something step by step.

---

> "For instance, if you want it to create a presentation, you might need it to first search the web for relevant information (this is a tool), then summarize this information (it can do this by itself), and then finally create the slides one at a time (this is also a tool). This workflow or 'recipe' for creating a presentation might therefore include a set of tools but also instructions on how to use them and in what order. As such, skills teach your agent what to do, when to do it, and how."

**Explanation:** A skill = a "recipe": which tools, in what order, plus instructions — teaching *what*, *when*, *how*.

---

> "A skill revolves around progressive disclosure. This means that the agent starts with minimal information on what the skill may provide and when it is accessed provides more information on how to execute it. This progression typically follows three layers of depth:
> - Layer 1 (always loaded): Metadata about the skill (e.g., name and description)
> - Layer 2 (loaded when activated): The main skill instructions
> - Layer 3 (loaded when needed): Additional resources to use (e.g., scripts and references)"

**Explanation:** Progressive disclosure = reveal info in layers: metadata (always), instructions (on activation), resources (when needed).

**Technical terms:**
- **progressive disclosure** = loading increasing detail only as needed, to save context.

---

> "To explore these layers and the anatomy of a skill, let's use an example. Imagine you have many meetings to keep track of every week and want to convert your transcripts and rough notes into structured meetings notes with actions items. Your agent does not know beforehand how you want the notes structured, the context of the people you're meeting, etc. A skill would give the context needed to perform the task depending on the type of meeting you have. The first thing needed to create a skill is the SKILL.md file."

**Explanation:** Example skill: "meeting_notes" converts transcripts into structured notes with action items. Skills start with a **SKILL.md** file.

---

### The SKILL.md

> "Every skill is required to have a SKILL.md file. The start of this file contains the skill's metadata, namely the name and a short description. For instance, the name of your skill could be 'meeting_notes' and its description 'Use this skill when you want to turn meeting transcripts and notes into structured decisions and action items.' When you first interact with your agent, only this name and description are provided so that the agent can decide whether it's worth exploring it in more detail for a given task. Regardless of how many skills you add, their names and descriptions are always shared with the agent (often as a system prompt). This metadata section is formatted as YAML, so you can clearly separate this metadata from the rest."

**Explanation:** SKILL.md's top = YAML metadata (name + description), always shared so the agent decides relevance.

**Technical terms:**
- **YAML** = a human-readable data format for metadata.

---

> "The body of the SKILL.md file contains the instructions relevant to its metadata. For instance, the 'meeting_notes' skill would contain information on how to perform the summarization of the transcripts and the things to look out for. This information is only given to the agent when it asks for it. Imagine that it's a tool that the agent can call to provide the much-needed context for a given task. The reason this information is not already given to the agent has to do with context engineering. Some skills have very long descriptions and tasks that might not be relevant for your query. By only accessing the instructions when asked for, the agent minimizes the amount of information it does not need."

**Explanation:** Instructions load only on request — context engineering keeps the window lean.

**Word meanings:**
- **much-needed** = highly important.

---

### The Bundled Resources

> "Skills can get quite large and unwieldy quickly. In our example of 'meeting_notes', there can be instructions about different types of meetings. A 1:1 with your manager requires a different structure than a 1:1 with one of your colleagues or a 1:1 with your employee. The same applies to calls with external partners, team meetings, etc. Instead of trying to put everything into the SKILL.md file, we can bundle additional files within the skills directory and reference them from the SKILL.md file. These files can be anything and don't require anything other than that the agent should be able to access them. Typical files are Python files for executing specific functions and markdown files for additional instructions. As such, the agent can decide to load this information when needed but it's necessary to use the skill."

**Explanation:** Layer 3 = bundled files (Python scripts, markdown instructions) in the skill directory, referenced from SKILL.md, loaded only when needed.

**Word meanings:**
- **unwieldy** = hard to manage.
- **bundled** = packaged together.

**Skill directory example:**
```
meeting_notes/
├── SKILL.md
├── scripts/
│   └── extract_action_items.py
└── formats/
    ├── one_on_one_manager.md
    ├── one_on_one_colleague.md
    ├── one_on_one_report.md
    ├── team_meeting.md
    └── external_call.md
```

---

> "In the skill's directory, only a SKILL.md is needed. However, as context grows and more information is needed for certain tasks, additional files can be added that the agent loads only when needed. Progressive disclosure in the form of these three layers helps keep the amount of context used to a minimum."

**Token budget (Table 5-1):**
| Layer | File | Activation | Tokens |
|-------|------|------------|--------|
| 1 | SKILL.md metadata (YAML) | Always loaded | ~100 |
| 2 | SKILL.md instruction (markdown) | Loaded when activated | Fewer than 5,000 |
| 3 | Bundled files (markdown, scripts, data) | Loaded when needed | As needed |

**Explanation:** Token budgets: metadata ~100 tokens; instructions <5,000; bundles on demand.

---

## 5.7 Summary

> "In this chapter, we explored how tool calling gives LLMs the capabilities to interact with the world. We first covered the fundamentals of tool calling and how LLMs call tools in practice. We saw that as text-to-text entities, LLMs merely communicate the intent to call a tool. The act of actually calling the tool falls either to the user or to external software that automates this process. The flow of calling a tool involved the tool creation, definition, selection, calling, and output processing."

**Explanation:** Recap: LLMs only communicate intent; execution is external; five-step flow.

---

> "Then, we explored three methods of having LLMs learn those tool-calling capabilities. First was in-context learning, where you can define the tool's schemas and definition in the prompt for the LLM to follow. Second was SFT, where an LLM is fine-tuned on specific tool-calling capabilities. We covered Toolformer as one of the first successful attempts to use SFT for tool calling. Lastly, we covered RL as one of the most prominent techniques for instilling tool-calling capabilities. Of note were ToolRL and Search-R1, which both adopt GRPO, a popular RL algorithm used in DeepSeek-R1."

**Explanation:** Recap: in-context learning, SFT (Toolformer), RL (ToolRL, Search-R1 — both GRPO).

---

> "We then looked at one of the most exciting things in the realm of tool calling, the MCP. We explored how MCP standardizes the usage of tools, which led to the widespread use of tools without the need for custom solutions. We finally looked at Skills, Anthropic's approach to giving agents the procedural knowledge they need to operate in your specific context. Where tool definitions tell an agent what a tool does, Skills tell it how to get a job done—the recipe of steps, conventions, and references it should follow for a recurring task. Through progressive disclosure across three layers—metadata, instructions, and bundled resources—Skills surface this guidance only when the agent decides a skill is relevant, keeping the context window lean."

**Explanation:** Recap: MCP = standardization; Skills = procedural knowledge ("how to get a job done") via three-layer progressive disclosure.

**Word meanings:**
- **realm** = domain/area.
- **lean** = minimal/uncluttered.

---

> "Along the way, your TinyAgent gained both prompt-based and native tool-calling capabilities, setting the stage for Chapter 6, where it will start choosing and sequencing those tools on its own."

**Explanation:** Bridge to Ch 6: the agent will choose and sequence tools autonomously.
