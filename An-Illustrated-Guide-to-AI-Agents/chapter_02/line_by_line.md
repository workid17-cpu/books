# Chapter 2 — Line-by-Line Detailed Explanation (Parts 1 & 2)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 2 "Large Language Models".
**Note:** This combined file comprises BOTH parts of the Chapter 2 line-by-line notes: Part 1 (agent-developer level: the agent loop, tokens, system prompts, tool use, the OpenAI-compatible endpoint, Step/Trajectory, training phases) from `04b_chapter2_part1_line_by_line.md`, followed by Part 2 (deep dive: Transformer internals, self-attention math, KV caching, efficient attention, MoE) from `05_chapter2_part2_line_by_line.md`. Part 1 ends at the "END OF PART 1" marker below; Part 2 begins at "## 2.6 The Transformer Architecture".
**Note:** Each numbered item quotes a paragraph from the book, then gives (1) a plain-English explanation, (2) word meanings, (3) technical terms explained.

---

## 2.1 Opening & The Agent Loop

> "As we saw in Chapter 1, the LLM powers agents to go from observations to actions. In Figure 2-1, we can see this general workflow. Here, we color-code the areas where the agent interacts with the user (on the left) and where the agent interacts with the environment (on the right)."

**Explanation:** The chapter starts by placing the LLM inside the agent: it sits in the middle, connecting the user (left side) and the environment (right side).

**Word meanings:**
- **workflow** = the sequence of steps in a process.

---

> "Let's take a simple example to unroll the loop we see in the figure. The example is a simple information-seeking agent trying to answer a question for a user using a web search tool. In Figure 2-2, we can see that interaction: 1. The user asks the agent a question. 2. The agent uses a web search tool, effectively pulling in information from the environment. 3. The retrieved information is presented to the agent, which decides that it now has enough information to answer the user's question. 4. The agent prints out its answer to the user."

**Explanation:** A concrete 4-step cycle (the "agent loop"):
1. User asks → 2. Agent calls web search (a tool) → 3. Agent reads the results and judges it has enough → 4. Agent answers the user.

**Word meanings:**
- **unroll** = reveal step by step.
- **retrieved** = fetched/gotten back.

**Technical terms:**
- **agent loop** = the repeated cycle of observe (get input) → decide → act (tool call) → observe result → ... until done.

---

> "In LLM-backed agents, the LLM is tasked with processing the user query, choosing the right action, processing the feedback or observation resulting from the action, and communicating back to the user."

**Technical terms:**
- **observation** = the result/output that comes back after an action (e.g., the search results). You'll see this word again in the `Step` dataclass.

---

> "In this chapter, you'll learn how LLMs work and how they're created. We've structured the chapter in two parts with intentionally different audiences in mind. Part 1 offers a high-level overview of LLMs aimed at agent developers: the core intuitions, capabilities, and limitations you need to build and reason about agent systems—without requiring a deep understanding of the underlying machinery. Part 2 takes a significantly deeper dive into model internals and training techniques. This section assumes more comfort with machine learning concepts and is intended for readers who want to understand why LLMs behave the way they do, not just how to use them."

**Word meanings:**
- **intuitions** = natural, gut-level understandings.
- **internals** = the inner workings.
- **machinery** = here, the technical mechanisms.

---

## 2.2 Input and Output Tokens

> "Language models are systems trained to predict and generate text sequences. Their inputs and outputs are best understood initially as sequences of words, or more precisely, tokens–which can be words, parts of words, numbers, or punctuation."

**Technical terms:**
- **tokens** = the atomic units of text the model works with. Not always whole words: "flamingos" can be split into "flamingo" + "s".

---

> "We can see the perspective of an LLM's inputs and outputs in Figure 2-3. The input text (which can be a query from the user or feedback from the environment) is first broken down into tokens, then presented to the language model. The language model generates the first token, then the next one, then the next, until its response is completed."

**Explanation:** Everything going into the model (questions OR environment feedback like tool results) gets tokenized. Output comes out one token at a time.

---

> "This one step of generating this output text is actually a loop where the model generates a token, adds it to the input, and then processes this new input again to generate the next token."

> "Models that operate like this, consuming their own output from one step as inputs in a following step, are called autoregressive models."

**Word meanings:**
- **consuming** = using up/taking in.

**Technical terms:**
- **autoregressive model** = "auto" (self) + "regressive" (predicting from past values): the model generates one token, appends it to its own input, and repeats. This is *the* key mechanism of GPT-style LLMs.

---

## 2.3 From Language Modeling to Powering Agents

> "The LLMs that power agents start out as general language models as previously discussed. This is done in the first training phase of a language model and creates what's called a base language model. But for an LLM to be able to power an agent, it needs an additional set of capabilities to fit the expected role. LLM creators train LLMs in the subsequent phase of training, called post-training, to be able to parse inputs and generate outputs in a format that supports these expected capabilities."

**Word meanings:**
- **parse** = read and understand the structure of an input.

**Technical terms:**
- **base language model** = the raw model right after pre-training (good at predicting text, not yet good at following instructions).
- **post-training** = the second phase of training that adapts the base model (instruction-following, tool-use formats, etc.).

---

### System prompt

> "Language models are most often deployed by one entity, say a company, to serve a set of users, say the employees of the company. The party that deploys the model needs a way to describe the expected behavior of the model in a way that takes precedence over what the end users ask of the model. This is the role of the system prompt: a privileged input that shapes model behavior before the model sees a single token from the user."

**Word meanings:**
- **deployed** = put into service.
- **takes precedence over** = is more important than / overrides.
- **privileged** = given special status/priority.

**Technical terms:**
- **system prompt** = a special, high-priority instruction placed at the start of the input that defines the model's role/behavior (e.g., "You are a helpful assistant. Answer truthfully."). It outranks user messages.

> "Because it's baked into how the model is used rather than how it's trained, system prompts let deployers customize behavior without touching the underlying model."

**Explanation:** System prompts are a *usage-time* mechanism (part of the input), so you can change behavior without retraining.

---

### Multi-turn conversations

> "Popular LLM systems like ChatGPT are modeled as a conversation between a user and an AI chatbot. This way, the LLM can keep track of a longer conversation and identify who said what in the history of the conversation. In Chapter 4, we'll touch on this as being one way we give an LLM some form of memory to be able to recollect earlier messages in the conversation."

**Word meanings:**
- **recollect** = remember.

**Technical terms:**
- **multi-turn conversation** = a chat with several exchanges; the message history is part of the input, letting the model "remember" earlier turns.

---

### Tool use

> "The conversation formats define a specific way for a language model to take an action. The LLM outputs a specific format, choosing a software function to invoke. The agentic software wrapper around the language model looks for these patterns and calls the functions that the LLM is trying to invoke. These functions are usually listed in the system prompt telling the LLM what actions are available to it."

**Word meanings:**
- **invoke** = call/activate a function.
- **wrapper** = surrounding software that adds functionality around something (here: the code around the LLM).

**Technical terms:**
- **tool call / function calling** = the LLM emits a structured message naming a function and its arguments; the wrapper software detects it and actually executes that function.
- **software wrapper** = the agent harness code that surrounds the raw LLM.

> "In Figure 2-6, we can see an example message passed to an LLM. It has a system prompt, multiple turns in the chat history, and a final question asked by the user. The LLM responds by choosing to call a web search tool to retrieve the information required to answer the question."

**Example:** User asks about flamingos at Toronto Zoo → LLM doesn't answer directly; it outputs a `web_search` tool call with a structured query, which the agent executes, then the conversation continues.

---

## 2.4 The TinyAgent & The OpenAI-Compatible Endpoint

> "The LLM is the agent's brain, and it's the first thing we want to implement in your TinyAgent. To do so, we will need to consider which LLM to use, as there are hundreds of options depending on your hardware, use case, and the LLM's capabilities. Newer models such as Gemma 4 have been trained to perform native reasoning and tool calling and have various options for different hardware. However, rather than only showing an LLM that 'just does' reasoning and tool calling, we also want to show you what it's like to build the tool calling and reasoning behavior yourself."

**Word meanings:**
- **native** = built-in from the factory, not added externally.

**Technical terms:**
- **Gemma** = a family of open-weight LLMs from Google.
- **native reasoning / tool calling** = the model was trained to produce reasoning text and tool calls directly.

> "We decided to show you both! Throughout the book, we will demonstrate these capabilities both from a prompt-driven and model-driven perspective. By doing it yourself through various prompting techniques, you'll gain insight into how these newer models have learned to perform reasoning and tool calling natively."

**Word meanings:**
- **prompt-driven** = achieved by cleverly worded prompts.
- **model-driven** = achieved by built-in model capability.

> "As such, we choose two models to use throughout this book: one that cannot natively perform any reasoning or tool calling whatsoever (Gemma 3) and one that has been trained specifically to perform agentic tasks through reasoning and tool calling (Gemma 4). They both have various sizes, where bigger models tend to be stronger than smaller models. For Gemma 3, we opt for the 12 billion parameter model (Gemma 3 12B), and for Gemma 4, we opt for the smaller model that has effectively 4 billion parameters (Gemma 4 E4B)."

**Word meanings:**
- **whatsoever** = at all.
- **opt for** = choose.
- **effectively** = roughly, in practice.

**Technical terms:**
- **parameter (billion)** = model size measure. "12B" = 12 billion parameters; "E4B" = effective ~4 billion parameters (often an MoE model — see Part 2).
- **MoE (Mixture of Experts)** = an architecture with "effective" vs "total" parameter counts (detailed in Part 2).

---

### The OpenAI-compatible endpoint

> "Before we start creating the main class for the brain, the LLM, we first explore a common way of interacting with LLMs, namely through the OpenAI-endpoint. It is a standardization of LLM inference that includes set fields, parameters, and responses so that you always get the same standardized format back. OpenAI, with the release of GPT 3.5 back in early 2022, set this standard for parsing an LLM's response."

**Word meanings:**
- **inference** = the process of running a trained model to get an output (as opposed to training).

**Technical terms:**
- **OpenAI-compatible endpoint** = an HTTP API that follows OpenAI's format (URL, request/response JSON), so any compatible server can be used interchangeably.

> "This endpoint generally contains the following pieces of information: **base_url** — The URL of your hosted LLM; **api_key** — The key for accessing your hosted LLM; **model** — The name of your LLM; **messages** — The query to send to your LLM."

**Word meanings:**
- **hosted** = running on a server you control (local or cloud).

**Technical terms:**
- **base_url** = the web address where the LLM server lives.
- **api_key** = a secret access token for the API.
- **messages** = an array of message objects, each with a `role` (e.g., "system", "user", "assistant") and `content`.

> "Before you can use the LLM, you will first have to spin up a server that hosts your LLM. Don't worry, this is much easier than it sounds. There are many different ways to download and host your LLM on your device. Popular options include Ollama, LMStudio, and llama.cpp. Ollama and LMStudio work well for both developers and non-developers, whereas llama.cpp is ideal for developers."

**Word meanings:**
- **spin up** = start/launch (from spinning up a machine).

**Technical terms:**
- **Ollama** = a user-friendly tool for running LLMs locally.
- **LMStudio** = a GUI app for running local LLMs.
- **llama.cpp** = a C++ library for running LLMs efficiently, favored by developers.

> "Throughout this book, we'll assume you use Ollama, which hosts its LLMs locally on http://localhost:11434/v1/. However, you can use any inference engine, both locally and hosted, as long as it is OpenAI-compatible (which most are)."

> "All LLMs have a specific prompt template that decides how to format the text that passes through the LLM. Instead of having to look up what this template is for each model, most inference engines (like Ollama) do this for you with the messages structure. These messages are a common format for structuring multiple turns of questions and responses between you (user) and the LLM (assistant). You can then use the messages structure to talk to the LLM, which will automatically convert it to the prompt template of the LLM."

**Technical terms:**
- **prompt template** = each model's special formatting for wrapping messages (special tokens, role markers). The messages API hides this from you.

> "We'll use this structure to call the OpenAI-endpoint, which, in turn, will return a response. This response can contain multiple answers in parallel but is generally a single answer. As such, we focus on inspecting its one answer, namely result['choices'][0]."

**Technical terms:**
- `result["choices"][0]` = the standard place in the API response holding the first (main) generated message. `choices` is a list (multiple candidates possible); `[0]` picks the first.

**Code walkthrough — calling the LLM:**

```python
import json
import urllib.request

data = json.dumps({
    "model": "gemma4:e4b",
    "messages": [{"role": "user", "content": "Hi! How's life?"}]
}).encode("utf-8")

req = urllib.request.Request(
    url="http://localhost:11434/v1/chat/completions",
    data=data,
    headers={"Content-Type": "application/json"})

with urllib.request.urlopen(req) as response:
    result = json.loads(response.read())

print(result["choices"][0])
```
- `import json` / `import urllib.request` — Python's standard libraries for JSON and HTTP.
- `json.dumps(...)` — converts a Python dict into a JSON text string.
- `.encode("utf-8")` — turns the string into bytes (required for HTTP).
- `urllib.request.Request(url=..., data=..., headers=...)` — builds an HTTP request to the `/chat/completions` endpoint.
- `urllib.request.urlopen(req)` — actually sends the request and opens the response.
- `json.loads(response.read())` — parses the returned JSON into a Python dict.
- `/v1/chat/completions` — the standard OpenAI-compatible chat endpoint path.

> "Depending on the capabilities of the underlying LLM, it might output more fields than just the content and reasoning. Some models are capable of returning tool calls, which get a field of their own."

**Technical terms:**
- **tool_calls field** = a special API field models can return containing a structured tool invocation.

**The book's two reasons for teaching prompting-first (memorize these!):**
> "1. Using the reasoning and tool_calls fields is a bit 'magical' and does not teach you how these are actually created and used. Instead, we're going to show you how to do this explicitly through prompting techniques. 2. Some models actually do not support reasoning and tool_calls, but with proper prompting, can still be used as agents. So instead of blindly relying on these fields, we want to show you how to nudge the model to do explicit reasoning and tool calling with prompting."

**Word meanings:**
- **magical** = here, hidden/opaque — you see the result but not how it works.
- **nudge** = gently push.

---

### The Response dataclass

> "After we get the output, we are going to create a Response dataclass where we will track the content, reasoning, tool_call, and other metadata the model may provide."

> "For now, we are only interested in using the content of the model, as we want to show you how to have a non-reasoning model that still shows reasoning behavior. To do that, we are adding a parameter to the LLM class we'll be building that can disable thinking. Lastly, we also track metadata that the backend might provide, such as the model's name ('model'), the number of tokens in the prompt ('prompt_tokens'), and the number of tokens that were generated ('completion_tokens')."

**Word meanings:**
- **metadata** = data about data (extra info describing the response).

**Technical terms:**
- **dataclass** = a Python feature for defining classes that mainly hold data (with auto-generated `__init__`, etc.).
- **prompt_tokens / completion_tokens** = usage counters: tokens sent in vs. tokens generated.
- **reasoning_effort: "none"** = an API parameter to turn off a model's reasoning behavior.

```python
from dataclasses import dataclass

@dataclass
class Response:
    """Structured response from LLM calls."""
    content: str = ""
    reasoning: str | None = None
    tool_call: dict | None = None
    metadata: dict | None = None
```
- `@dataclass` — decorator that auto-generates boilerplate for a data-holding class.
- Fields: `content` (the main text), `reasoning` (thoughts, if any), `tool_call` (structured tool invocation, if any), `metadata` (model name, token usage).
- `str | None` — type annotation meaning "a string OR None" (field is optional).

---

### The LLM class

```python
class LLM:
    def __init__(self, model: str, base_url: str = "http://localhost:11434/v1",
                 api_key: str = "no_key", think: bool = False):
        self.model = model
        self.base_url = base_url
        self.api_key = api_key
        self.think = think

    def generate(self, messages: list[dict], tools: list | None = None) -> Response:
        body = {"model": self.model, "messages": messages}
        if tools:
            body["tools"] = tools
        if not self.think:
            body["reasoning_effort"] = "none"

        request = urllib.request.Request(
            f"{self.base_url}/chat/completions",
            data=json.dumps(body).encode(),
            headers={
                "Content-Type": "application/json",
                "Authorization": f"Bearer {self.api_key}",
            },
        )
        with urllib.request.urlopen(request) as response:
            data = json.loads(response.read())

        message = data["choices"][0]["message"]
        tool_calls = message.get("tool_calls")
        tool_call = tool_calls[0] if tool_calls else None
        metadata = {
            "model": data["model"],
            "prompt_tokens": data["usage"]["prompt_tokens"],
            "completion_tokens": data["usage"]["completion_tokens"],
        }
        return Response(
            content=message.get("content"),
            reasoning=message.get("reasoning"),
            tool_call=tool_call,
            metadata=metadata,
        )
```
**Explanation:**
- `__init__` — sets model, base_url, api_key, and a `think` flag (default False → reasoning off).
- `generate(messages, tools=None)` — builds the request body; optionally adds `tools`; sets `reasoning_effort: "none"` when thinking is disabled.
- `f"{self.base_url}/chat/completions"` — an f-string building the endpoint URL.
- `"Authorization": f"Bearer {self.api_key}"` — the standard HTTP auth header format (`Bearer` token).
- `message = data["choices"][0]["message"]` — extracts the assistant's message object.
- `tool_calls[0] if tool_calls else None` — takes the first tool call if any exist.
- `metadata` — collects model name and token usage counts.
- Returns a `Response` dataclass instance.

---

### Step and Trajectory

> "Remember that we are creating an agentic harness and a vital component of any harness is that you can easily debug what is happening during inference. An agent might run for several turns at a time and run into an incorrect tool usage. To easily debug that, we want to keep track of everything the model has done so far."

**Word meanings:**
- **debug** = find and fix problems in code/behavior.

> "The Step is very much like the Response object with one major difference, the answer and observation fields. The answer is the final answer of the model and represents that the agent has reached the end of its turn. The observation is the output of tool usage. They are consequences of any processing or results we get from the TinyAgent. Finally, we use the term action to describe a tool call because in Chapter 6 we will explore loops of thought → action → observation that allow for autonomous behavior."

**Technical terms (important distinctions):**
- **thought** = the model's reasoning.
- **action** = a tool call.
- **observation** = the result returned by the tool.
- **answer** = the final reply (signals end of the turn).
- **thought → action → observation** = the loop that enables autonomy (Chapter 6).

> "To reach its final answer, an agent might need various steps and use different actions. This sequence of steps is called the agent's trajectory. We likewise track this information as it allows for easily debugging this sequence of steps to identify where and why an agent might have failed."

**Technical terms:**
- **trajectory** = the ordered record of all steps the agent took (like a flight path) — invaluable for debugging.

```python
@dataclass
class Step:
    """A single step in an agent's trajectory."""
    thought: str = ""
    action: dict | None = None
    observation: str | None = None
    answer: str | None = None
    metadata: dict | None = None

class Trajectory:
    """Records agent execution as a sequence of runs."""
    def __init__(self) -> None:
        self.runs: list[dict] = []

    def initialize(self, query: str) -> None:
        """Register a new run with the given query."""
        self.runs.append({"query": query, "steps": []})

    def add(self, response: Response, observation: str | None = None) -> None:
        """Record a step from a Response, optionally with an observation."""
        step = Step(thought=response.reasoning or "", metadata=response.metadata)
        if observation is not None:
            step.action = response.tool_call
            step.observation = observation
        else:
            step.answer = response.content
        self.runs[-1]["steps"].append(step)
```
**Explanation:**
- `Step` — holds one step's `thought`, `action`, `observation`, `answer`, `metadata`.
- `Trajectory` — holds `runs` (a list, each run = one query + its steps).
- `initialize(query)` — starts a new run with an empty steps list.
- `add(response, observation=None)` — records a step. If an observation was returned (tool was used), store action + observation; otherwise store the content as the final `answer`.
- `self.runs[-1]` — the most recently added run (Python negative indexing).

---

### Updating your TinyAgent

```python
class TinyAgent:
    """A minimal, modular, and educational agent framework."""
    def __init__(self, llm: LLM):
        self.llm = llm
        self.memory = None  # Chapter 4: Add Memory
        self.tools = None   # Chapter 5: Add Tools
        self.planner = None # Chapter 6: Add Planning
        self.trajectory = Trajectory()

    def run(self, task: str) -> str:
        """Run the agent on a task."""
        self.trajectory.initialize(task)
        return self._step(task)

    def _step(self, task: str) -> str:
        """Perform a single step."""
        messages = [{"role": "user", "content": task}]
        response = self.llm.generate(messages)
        self.trajectory.add(response)
        return response.content

    def _execute_action(self, action: str) -> str | None:
        """Execute a tool action."""
        return f"Executed action: {action}"
```
**Explanation:**
- `__init__(self, llm: LLM)` — now takes an `llm` and creates a `Trajectory`.
- `run(task)` — initializes a new trajectory run, then does a single `_step`.
- `_step(task)` — builds a one-message prompt, calls `llm.generate`, records the response in the trajectory, returns the content.
- `_execute_action` — still a placeholder (tools come in Chapter 5).
- At this stage the agent is **single-step with no autonomy**; multi-step autonomy arrives in Chapter 6.

**Helper functions (from the `illustrated-agents` package):**
> "As you progress through the chapters, you might add just a single line of code to TinyAgent. That becomes difficult to spot as the lines of code in TinyAgent grows. We prepared a number of helper functions in the illustrated-agents package that will help you quickly see what (small) changes were made between two versions."
- `tinyagents_diff` / `DiffViewer` — shows the difference between TinyAgent across chapters.
- `TrajectoryViewer` — visualizes trajectories to avoid them being "unwieldy" (hard to manage) with many steps.

**Word meanings:**
- **unwieldy** = hard to handle due to size/complexity.

---

## 2.5 Training a Large Language Model

> "LLMs go through two major phases of training: pre-training and post-training (Figure 2-7). Pre-training is where they get their name from, because they are trained on a language processing task called language modeling."

**Technical terms:**
- **language modeling** = the task of predicting the next token given previous tokens.

### 2.5.1 Pre-training: language modeling

> "Language modeling is the task of presenting a model with a sequence of words (or more precisely, tokens) and asking it to predict the next word that is most likely to appear. This phase requires vast amounts of data and it takes a major part of the computing done during the training process."

**Word meanings:**
- **vast** = enormous.

> "Pre-training data is drawn from curated mixtures of sources including web text, books, and code, each filtered to satisfy quality criteria. From this text data, we can extract different spans of text and use each one as a training example."

**Word meanings:**
- **curated** = carefully selected.
- **criteria** = standards/requirements.
- **spans** = sections of text.

> "Say, for example, we handsomely pay O'Reilly Media to license the data on its website describing our earlier book... We can take one span of that text and present it to the model. We hide the last token, give the model all the previous tokens, and ask it to predict the next word in the sequence. At the start, the model under training would almost always guess incorrectly, which is part of the process. Now that we have its wrong prediction and the correct word, we plug them both in our loss calculation and update the weights of the model so that next time it has a better chance of making the right prediction. This is done billions of times and in the end results in a base model. The most famous and transformational of these is OpenAI's GPT3, released in 2020."

**Word meanings:**
- **handsomely** = generously (here: a lot of money).
- **license** = legally buy the right to use.

**Technical terms (critical to memorize):**
- **next-token prediction** = the training objective: guess the masked/next token.
- **loss** = a number measuring how wrong the prediction was; training minimizes it.
- **weights** = the model's learned parameters; updated so predictions improve.
- **base model** = the output of pre-training.
- **GPT-3** = OpenAI's 2020 model, the most famous base model; GPT = Generative Pre-trained Transformer.

> "Since then, the later steps in the training process were widely adopted to lead to a model that requires less prompt engineering and behaves more in line with how people (and agent scaffolds) expect it to behave."

**Word meanings:**
- **prompt engineering** = crafting prompts carefully to get good behavior.
- **in line with** = matching.

**Technical terms:**
- **agent scaffolds** = the surrounding software structures that agents run in (harnesses).

### 2.5.2 Post-training: supervised fine-tuning (SFT)

> "Base models have strong latent capabilities, including reasoning, summarization, translation, and code generation, that often require heavy prompt engineering to unlock. Post-training helps polish these model behaviors using a much smaller set of training examples."

**Word meanings:**
- **latent** = hidden, present but not active.
- **polish** = refine/improve.

**Technical terms:**
- **SFT (supervised fine-tuning)** = the first post-training step.
- **instruction-tuning** = another name for SFT — training the model to follow instructions.

> "An SFT training example is composed of a prompt and a completion that shows how we want the model to behave if it sees such a prompt. The update rule is almost identical to that of pre-training. The key difference is that while prompt tokens are included in the input context, they are excluded from the loss computation so the model is trained on only the response tokens."

**Technical terms:**
- **prompt-completion pairs** = training examples of (input instruction, desired output).
- **Key SFT detail**: prompt tokens are fed as context but their loss is **masked** — the model only learns to predict the completion tokens.

> "The SFT training phase is essential yet not enough to produce a cutting-edge model. It teaches the model to follow instructions, but it cannot fully capture human preferences about what makes a response genuinely helpful, accurate, or well-reasoned. The reinforcement learning step that follows is what pushes the model in that direction."

**Word meanings:**
- **cutting-edge** = most advanced.

### 2.5.3 Post-training: reinforcement learning (RL)

> "In SFT, we show the model what the desired output looks like. Reinforcement learning (RL) is a different training methodology that doesn't require having a complete targeted response. Instead, we use methods that evaluate the quality of a model generated response and update the model's weights based on that quality score, called a reward."

**Word meanings:**
- **methodology** = a system of methods.

**Technical terms:**
- **reinforcement learning (RL)** = training by trial: generate → score with a reward → update weights.
- **reward** = a numeric quality score for a generated response.

#### RLHF

> "One of the most commonly used RL methods in training LLMs is Reinforcement Learning from Human Feedback (RLHF), which relies on preference scores collected from humans or specialized models called reward models, themselves often being LLMs as well. A single training example in an RLHF step can be a prompt and two completions for it: one that is preferred, and another that is rejected. The RLHF training objective is to update the model to increase the likelihood of generating output like the preferred response and decrease the likelihood of generating output like the rejected response. This can be used to polish model behaviors such as response length, type of language the model uses, safe type of completions to generate or to avoid, and many other types of behaviors."

**Word meanings:**
- **likelihood** = probability.

**Technical terms:**
- **RLHF (Reinforcement Learning from Human Feedback)** = RL using human preference scores.
- **reward models** = separate models (often LLMs) trained on human preferences that score outputs automatically.
- **preferred vs rejected completions** = the two outputs in a comparison example; training increases the probability of the preferred one.

#### RLVR

> "The other major RL method widely used in post-training today's leading LLMs is Reinforcement Learning with Verifiable Rewards (RLVR). Similar to RLHF, it updates the model based on a reward score. The difference here is that the rewards can be obtained from automated ways of verifying the output; for example, if it follows a certain format, or the final answer it results in is equal to the correct answer (that we knew beforehand, in the case of math problems, for example)."

**Word meanings:**
- **verifiable** = can be checked objectively.

**Technical terms:**
- **RLVR (Reinforcement Learning with Verifiable Rewards)** = RL where rewards come from automated verifiers (format checks, known-correct answers) rather than human raters. Scalable to domains with objective right/wrong.

#### GRPO

> "To better grasp the intuitions behind RLVR, we can look at a more specific example for a model that solves math problems. Let's say we have a set of math problems and the final correct answer for each of them. We want to train the model to solve them correctly and to print the answer in the format <answer>42</answer>, if 42 was the answer to a specific problem, for example."

**Word meanings:**
- **grasp** = understand.

> "In an RLVR training step, we take one of the problems and pass it to the model with instructions about the format that we want. Once the model generates its output, we can automatically verify whether it followed the requested format and if it arrived at the correct solution of the problem. In this example we had two kinds of rewards, a format reward scoring whether the model uses the correct format for the output and assigning a reward of 0.3 if correct or 0 if incorrect. Then there's an accuracy reward that compares the final result to the actual result and assigns a reward of 0.7 if correct and 0 if incorrect. In Chapter 3, we'll see how this method was used to train DeepSeek-R1, one of the first open-weight reasoning LLMs."

**Technical terms (memorize the numbers):**
- **format reward** = 0.3 if output matches the required format (e.g., `<answer>42</answer>`), else 0.
- **accuracy reward** = 0.7 if the final answer is correct, else 0.
- **open-weight** = model weights publicly released.

> "The Group Relative Policy Optimization (GRPO) algorithm has another key ingredient beyond reward assignment. Rather than scoring a single response, at each training step GRPO generates a group of responses for the same prompt using varied temperature settings to ensure diversity among them. Rewards are then computed relative to this group, reinforcing responses that score higher than the others. This comparative approach is what the word 'group' in the name refers to."

**Word meanings:**
- **diversity** = variety.
- **relative** = compared to the others.

**Technical terms:**
- **GRPO (Group Relative Policy Optimization)** = RL algorithm that generates a *group* of responses per prompt (with varied temperatures) and computes rewards *relative to the group*, reinforcing the higher-scoring ones. Used to train DeepSeek-R1.
- **temperature** = a sampling parameter controlling randomness (see Part 2 for decoding).

> "In Figure 2-13, we can see this intuition for a group size of five. We generate five answers from the model, assign a reward to each one, and use those scores to update the model."

---

**END OF PART 1.** Continue with Part 2 (Transformer internals, self-attention, efficient attention, MoE) in the companion file `05_chapter2_part2_line_by_line.md`.
# Chapter 2 — Line-by-Line Detailed Explanation (Part 2)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 2 "Large Language Models" — Part 2 (deep dive: Transformer internals, self-attention, efficient attention, MoE).
**Note:** Each numbered item quotes a paragraph from the book, then gives (1) a plain-English explanation, (2) word meanings, (3) technical terms explained.

---

## 2.6 The Transformer Architecture

> "LLMs have predominantly been neural networks built in the Transformer decoder architecture since around 2020."

**Word meanings:**
- **predominantly** = mostly.

**Technical terms:**
- **neural network** = a computing system modeled loosely on the brain, made of many interconnected "neurons" (nodes) that learn from data.
- **Transformer** = the architecture introduced in the 2017 paper "Attention Is All You Need" that powers modern LLMs.
- **decoder** = in the Transformer, the part that generates text by predicting the next token. GPT-style LLMs are *decoder-only* (no separate encoder). Contrast: the original Transformer had both an encoder (reads input) and decoder (writes output).

### The tokenizer, Transformer blocks, and language modeling head

> "A Transformer decoder has three major components: a tokenizer, a stack of Transformer blocks, and a language modeling head (LM head)."

**Technical terms (three components — memorize):**
1. **Tokenizer** = software that splits input text into tokens.
2. **Stack of Transformer blocks** = many identical processing layers applied in sequence.
3. **Language modeling head (LM head)** = the final layer converting representations into a probability distribution over the vocabulary.

> "The tokenizer is the piece of software responsible for breaking the text input into tokens. It is carefully optimized earlier in the training process of the LLM to help bring out the capabilities we need in the trained LLM. The vast majority of agent developers will work with a ready-made tokenizer prepared by the model provider."

> "For an LLM intended to power agents, careful tuning for a tokenizer includes support for generating software code, the various languages intended for use, and adding special tokens (like <|start|>) that are used in the specified data format."

**Word meanings:**
- **tuning** = adjusting/optimizing.

**Technical terms:**
- **special tokens** = reserved tokens not representing words, used for formatting structure (e.g., `<|start|>`, `<|end|>`, role markers).

> "A tokenizer has a set number of tokens in its vocabulary, say 50,000. We can also see in the figure that the model has an equal number of vector embeddings– one per token in our vocabulary. These embeddings vectors are numeric representations of each token and they are what the model uses to calculate language and how it processes its inputs."

**Technical terms:**
- **vocabulary** = the fixed set of tokens a tokenizer knows (e.g., 50,000 entries).
- **embeddings** = numeric vectors (lists of numbers) representing each token; the model's "initial view" of a token.
- **vector** = an ordered list of numbers, often representing a point in high-dimensional space.

> "Almost the entirety of the processing done inside a language model is conducted inside the stack of Transformer blocks. But what results from that process is a single vector containing information on what the next token should be."

> "That vector is passed to the LM head to interpret. It makes a simple calculation, which results in a probability score for each token in its vocabulary. That score is informed by everything the model learned in its training phases, and tokens with high probabilities are the ones most likely to appear as a completion in response to the input tokens."

**Technical terms:**
- **probability score** = how likely each vocabulary token is to be next. The LM head maps the final vector to these scores (logits → softmax).

> "The next natural step would be to actually pick the output token as informed by these probabilities. We may choose the highest probability token, but there are often good reasons for picking other tokens. The method to pick tokens from the probability distribution is called a decoding strategy."

**Technical terms:**
- **decoding strategy** = the rule for choosing a token from the probability distribution (e.g., greedy vs sampling).

> "If you've played with the temperature setting of an LLM, then you have interacted with these probabilities. A temperature value of zero leads to always choosing the single token with the highest probability. Increasing that temperature allows sampling, which means choosing from the distribution in a way where higher probability tokens have a higher chance of being picked."

**Technical terms:**
- **temperature** = a knob controlling randomness:
  - **0** → greedy: always pick the top-probability token (deterministic).
  - **higher** → sampling: lower-probability tokens get a chance.
- **sampling** = randomly choosing a token weighted by its probability.

---

### Processing through the Transformer blocks

> "A neural network processes inputs and produces an output in a forward pass. This means that the calculation flows sequentially through the various layers. This is exactly what happens with the Transformer blocks. First the first block starts processing, then it passes its results of processing to the next block, and so on. The final block passes its result to the LM head, and we proceed to decoding as we've seen in the previous section."

**Technical terms:**
- **forward pass** = running input through the network once, layer by layer, to get output.

> "It also shows how each token can be seen flowing through its own track. The number of tracks that a model supports is commonly known as its context size. So, a model that has a context length of 100,000 can have only that number of tokens flowing through it simultaneously, which generally limits its input and output capacity to text of that size."

**Word meanings:**
- **simultaneously** = at the same time.

**Technical terms:**
- **track** = a token's own parallel lane through the Transformer blocks.
- **context size / context length** = the max number of tokens that fit in the model at once. Example: 100,000-token context = 100,000 token tracks.
- **context window** = the same idea; the model can only "see" this much input.

> "For text generation, the processing flow of the last token is what's used to generate the next token. That calculation is informed, however, by all the processing conducted on the previous tokens as we'll see next when looking closer at the insides of a Transformer block."

**Explanation:** To predict the next token, only the *final* token's output vector is sent to the LM head — but that vector has been enriched with information from all earlier tokens via self-attention.

> "For agent developers, context length is a key property for selecting the best model to build an agent around. It sets a limit on how much information we can pack in the input of the model. This limitation gives rise to a whole discipline of context engineering that we'll revisit repeatedly in this book. This is the discipline of choosing the most relevant information for the model to fit within this architectural limitation of the Transformer model."

**Word meanings:**
- **gives rise to** = causes/creates.
- **discipline** = here, a field/practice.

**Technical terms:**
- **context engineering** = deliberately choosing what information to include in the prompt so it fits the context window and stays relevant.

> "Another key concept for agent developers is to realize that after that first pass where all the input tokens are processed, it's common to cache the results so that in the next forward pass, we process the information along only the one track associated with the new token we're generating. This is referred to as prompt-caching, prefix-caching, or more technically, kv-caching. In Figure 2-20, we can see how only one track is active and information from previous tracks is cached and used for that one active calculation. This leads to dramatically increased speed and reduces the amount of processing required."

**Technical terms:**
- **kv-caching** = caching previously computed Keys and Values (details in section 2.7) so each generation step only computes the new token's track.
- **prompt-caching / prefix-caching** = other names for the same optimization.

> "Optimizing for the kv-cache is a key responsibility for agent developers to increase the speed and reduce the cost of the agents they build. In Chapter 10, we cover some strategies used by developers of software engineering agents to optimize for this cache, reducing the latency and improving the economics of their agents."

**Word meanings:**
- **latency** = delay before a response.
- **economics** = here, cost-effectiveness.

---

### Inside the Transformer block

> "There are two major components inside a Transformer block: a self-attention layer and a feed-forward neural network layer."

**Technical terms (memorize):**
- **self-attention layer** = lets each token gather context from other tokens in the sequence.
- **feed-forward neural network (FFNN) layer** = processes each token's representation independently.

> "We've seen how the input text is broken down into tokens. And we've also seen that each token has an associated static embedding vector in the model. The Transformer operates on these embedding vectors. The first Transformer block is presented with the embedding vectors associated with the tokens in the input text. As we can see in Figure 2-21, the first block does a bit of processing on these input vectors and hands off the results of its processing (as the same number and size of vectors) to the next Transformer block. This goes on block by block until the last block in the stack."

**Explanation:** Blocks transform vectors but keep their count and size constant — only the *values* change as representations are refined.

---

### The Transformer block: the feed-forward neural network

> "Let's first talk about the feed-forward neural network because it's simpler, even though in the architecture it comes after self-attention. This layer does the heavy lifting in predicting the next token because the training process shapes its ability to predict the patterns encoded in the training dataset. If we train a feed-forward neural network on vast amounts of web data, when we give it the words 'The Shawshank', it would be able to predict that the next word would be 'Redemption', because the 1994 film with that name is the most common occurrence of this sequence of tokens."

**Word meanings:**
- **heavy lifting** = the main hard work.

**Technical terms:**
- **factual associations** = the feed-forward layers are thought to store learned facts/patterns (e.g., "The Shawshank" → "Redemption").

> "In the original Transformer as well as in the majority of LLMs at the time of writing, the feed-forward neural network layer is one big neural network. The training process prunes and adapts its connections in a way that encodes the patterns in language. Recall that by now, we're no longer talking about a specific human language, we're talking about a model that supports multiple human languages, multiple programming languages, as well as the patterns we need to define tool calling, multi-turn conversations, and, as we'll see in Chapter 3, reasoning patterns."

**Word meanings:**
- **prunes** = trims away (connections).

**Technical terms:**
- **dense model** = one big feed-forward network where all neurons are active for every token.

> "In a dense model, the feed-forward network expands the token representation to a larger hidden dimension (here, 512 inputs expanded to 2048) before compressing back down, with all neurons active for every token. Research suggests these layers play a significant role in storing factual associations learned during training, though the full picture of how knowledge is distributed across a model remains an open question."

**Technical terms:**
- **hidden dimension** = the internal width of the network (e.g., 512 → 2048 → 512).

> "In recent years, there has been a trend away from having a single large network, and replacing it with a large number of smaller, more specialized networks. This architecture is called a mixture-of-experts (MoE) architecture, and we talk about it more in the second part of this chapter. We mention this here because as you select a model to power your agent, you may come across a choice of a dense model and an MoE model, especially if you're looking at open source models."

**Word meanings:**
- **trend** = general direction.

**Technical terms:**
- **mixture-of-experts (MoE)** = replacing one big feed-forward net with many smaller "expert" nets plus a router.

> "In Figure 2-25, we see a basic MoE feed-forward layer that contains four sublayers; each is called an expert. These are preceded by a router that looks at the input token and decides which expert (or set of experts) is best suited to process this particular token."

**Technical terms:**
- **expert** = a smaller, specialized feed-forward network.
- **router** = the network that picks which expert handles each token.

---

### The Transformer block: an overview of self-attention

> "Language encodes a lot of information in the order of words in a sequence and the context that a word is used in. A word such as bank can mean a financial institution or can mean a riverbank. We would only know which is meant by looking at the context where the word is used. The self-attention layer allows the Transformer to make these distinctions."

**Technical terms:**
- **disambiguation** (implied) = resolving which meaning of a word is intended. Example: "bank" (money) vs "bank" (river).

> "As we can see in Figure 2-26, a model is presented with an input sentence that is, 'The dog chased the llama because it'. When processing the word it in its own processing track, the model needs to know whether it refers to the dog or the llama. Self-attention is tuned to resolve this kind of problem."

> "The high-level intuition is that it attends to the most relevant previous tokens in the sequence. It learns that relevance from the training process. Self-attention enriches the information encoded in the input vector with information from the most relevant previous tokens in the sequence."

**Word meanings:**
- **attends to** = focuses on / pays attention to.
- **enriches** = adds to / enhances.

> "Self-attention does this by taking two steps: first, it scores the relevance of the previous tokens and then proceeds to a step of combining the relevant information into the token we're processing."

**Technical terms (two steps — memorize):**
1. **relevance scoring** = how much attention to pay to each previous position.
2. **combining information** = blending the relevant positions' representations into the current token's output vector, proportional to their scores.

> "Self-attention is one of the most resource-intensive operations in the Transformer. That's why it's commonly one of the areas most targeted for improvement."

**Word meanings:**
- **resource-intensive** = uses a lot of memory/compute.

---

## 2.7 How Self-Attention Works (the math)

> "A trained model is able to attend properly by utilizing three projection matrices that resulted from the training process. We multiply the input vectors by each of these projection matrices, resulting in matrices we call the Queries, Keys, and Values matrices."

**Technical terms (memorize):**
- **projection matrices** = learned weight matrices that transform input vectors into new spaces.
- **Queries (Q)** = "what am I looking for?" — the current token's search query.
- **Keys (K)** = "what do I contain?" — labels of other tokens describing their content.
- **Values (V)** = "what do I actually contribute?" — the actual information content of each token.

> "The first step of self-attention, relevance scoring, involves multiplying the query for the token we're currently processing by the key vectors associated with all the input tokens. This results in a relevance score for each vector. Here, 'dog' receives the highest score (40%), indicating it is the most relevant preceding token to the current position."

**Explanation:** Query × Key = a score measuring how related the current token is to each other token. "Dog" wins in the book's example.

> "After deciding the relevance values, self-attention then proceeds to combine information from these tokens, weighted by how relevant they are, and merge them into the vector of the current token we're processing. We see that weighted summation calculation. This vector becomes the output of the self-attention layer."

**Explanation:** Multiply each Value by its relevance weight and add them up — that weighted sum is the enriched output vector.

> "Figure 2-33 presents this in another way, closer to the mathematical formula you'll often see in LLM literature. The current and previous tokens are projected into Queries, Keys, and Values; the query-key product (scaled by √d_k and passed through softmax) produces the relevance scores, which are then multiplied by the Values to produce the attention output for the current position."

**The famous formula (memorize):**
```
Attention(Q, K, V) = softmax(Q · Kᵀ / √d_k) · V
```
**Word meanings:**
- **literature** = published research papers.

**Technical terms:**
- **Q · Kᵀ** = the dot product of Queries with transposed Keys (measures similarity).
- **√d_k** = square root of the key dimension — a scaling factor to prevent scores from getting too large (which would make softmax too "peaked"/extreme).
- **softmax** = a function that turns raw scores into a probability distribution (all positive, summing to 1).
- **d_k** = the dimension (size) of the key vectors.

---

## 2.8 KV-Caching Revisited

> "What we've described so far are major components of self-attention as described in the Transformer paper, which is often called 'vanilla attention.' This early form of self-attention, however, does a lot of redundant calculations of the K and V weights for each generated attention score if we apply it naively to text generation. Specifically, at each step, attention is recalculated for the entire sequence despite having calculated the K and V vectors of the previously seen tokens before."

**Word meanings:**
- **vanilla** = basic/original version (from vanilla = plain flavor).
- **redundant** = unnecessary because already done.
- **naively** = in the simplest, unoptimized way.

**Explanation of the problem:** When generating token-by-token, the model re-runs attention over the whole sequence every step, recomputing K and V for earlier tokens that never changed. That's wasted work.

> "This is where the KV cache comes in. Instead of having to recalculate those vectors, we can simply cache and reuse them for subsequent decoding steps. This makes inference much faster by reducing the redundant computation."

**Word meanings:**
- **cache** = store for later reuse.

> "Also note that the Query (Q) vectors do not need to be cached because they become unnecessary in subsequent iterations. Specifically, we need only the query vector of the latest token to compute the self-attention."

**Key point (memorize):** Cache **K and V**, not **Q** — only the newest token's query is ever needed.

> "Although such a KV cache can make inference much faster, it does require significantly more memory if all KV values are cached. For that, there are many different forms of attention created that attempt to reduce the calculations needed, which should therefore also reduce the KV cache that needs to be maintained."

**Explanation:** The KV cache speeds up time but costs memory → motivates the efficient-attention variants next.

---

## 2.9 More Efficient Self-Attention

> "Variants of attention mechanisms have since been developed to alleviate the memory issue of the KV cache and optimize the attention calculations. Popular techniques include Grouped-Query Attention and Flash Attention. More recently, however, DeepSeek introduced two attention mechanisms that showed tremendous improvements in the efficiency of attention calculations, namely Multi-head Latent Attention (MLA) and DeepSeek Sparse Attention (DSA)."

**Word meanings:**
- **alleviate** = reduce (a problem).

**Technical terms (overview table):**
- **Flash Attention** = an IO-aware algorithm that makes attention fast and memory-efficient at the GPU level (by avoiding writing the full attention matrix to memory). (Book: Dao et al., 2022.)
- **Grouped-Query Attention (GQA)** = shares K and V across groups of query heads.
- **Multi-Query Attention (MQA)** = shares a single K and V across all query heads.
- **Multi-head Latent Attention (MLA)** = compresses K and V into a small latent vector for caching (DeepSeek-V2).
- **DeepSeek Sparse Attention (DSA)** = selects only the most relevant tokens to attend to (DeepSeek-V3.2).

### Multi-head Latent Attention (MLA)

> "MLA is a variant of Multi-head Attention, which maintains separate Q, K, and V projection matrices for each attention head, producing a different Q, K, and V matrix per head. MLA uses low-rank joint compression of the keys and values to reduce the KV cache during inference. At its core, it compresses the keys and values into a smaller latent representation that is cached in place of the full K and V. In practice, this compressed cache is often combined with quantization (reducing the numerical precision of stored values) for additional memory savings."

**Word meanings:**
- **joint** = together (both keys and values).

**Technical terms:**
- **Multi-head Attention (MHA)** = standard attention with several parallel "heads," each with its own Q, K, V.
- **attention head** = one independent attention computation, run in parallel with others.
- **low-rank** = a smaller, compressed representation that captures most of the important information.
- **latent representation** = a compressed/hidden, lower-dimensional representation.
- **quantization** = storing numbers at lower precision (fewer bits) to save memory.

> "MLA first compresses the input embeddings into lower-dimensional representations called the Latent Q and Latent KV. These representations are significantly smaller than the full Q and KV, which allows us to cache the Latent KV instead of the full K and V. Positional information via Rotary Position Embedding (RoPE) is applied to a decoupled component of the Latent Q, since the Latent Q is recomputed at every step. RoPE is not applied to the Latent K itself, because the cached K would then need to be recomputed at every step, breaking the benefit of caching. Therefore, positional information is carried by a separate small key instead. Note that at this step, the Q, K, and V representations are split across multiple attention heads, much like standard Multi-head Attention. Finally, the content and positional components are concatenated and passed through standard Multi-head Attention."

**Technical terms (memorize the MLA details):**
- **Latent Q** = compressed query; recomputed every step.
- **Latent KV** = compressed keys+values; this is what gets cached (instead of full K, V).
- **RoPE (Rotary Position Embedding)** = a method for injecting positional information (token order) into Q and K. In MLA it is applied to a **decoupled component of Latent Q** (because Latent Q is recomputed each step) — **NOT to Latent K** (which would force recomputation of the cache, destroying the benefit). Position is carried by a **separate small key**.
- **decoupled** = separated out from the main pathway.
- **concatenated** = joined together (content + positional parts) before standard multi-head attention.

> "As such, MLA is essentially Multi-head Attention but with a compressed KV cache containing the previously mentioned Latent KV representation. This compression reduces the KV cache quite a bit and allows for much faster inference. It's also more efficient than previous methods, such as Grouped-Query Attention."

> "Note that Grouped-Query Attention and Multi-Query Attention share K and V across query heads to reduce the memory necessary for the KV cache but tend to be less accurate."

**Summary of the four attention approaches (memorize):**
| Approach | How it saves memory | Trade-off |
|---|---|---|
| MHA | None (full K and V per head) | Most memory/accurate |
| MQA | Shares one K, V across all heads | Less memory, less accuracy |
| GQA | Shares K, V across groups of heads | Middle ground |
| MLA | Caches small compressed Latent KV, projects back up | Very efficient, more accurate than GQA |

---

### DeepSeek Sparse Attention (DSA)

> "The next step in making MLA more efficient was first introduced in DeepSeek-V3.2. This new attention mechanism, called DeepSeek Sparse Attention (DSA), is instantiated under MLA and is an additional module to more efficiently select the tokens to attend to. It has two main components, a lightning indexer and a Top-K Selector."

**Word meanings:**
- **instantiated** = implemented/created as a specific case.
- **sparse** = not dense; only a subset used.

**Technical terms:**
- **sparse attention** = attention computed over only a subset of tokens instead of the full sequence.

> "The lightning indexer determines how relevant each preceding token is to the current query. For that, it uses the Q/K values that both have RoPE applied to them. Likewise, it takes in a scalar weight parameter w that helps the lightning indexer make better decisions about which tokens to select for the full attention mechanism. The output of the lightning indexer is scores fed to the Top-K Selector, which in turn retrieves only the KV entries that correspond to the Top-K index scores. As a result, the attention output is computed through the query token and a subset of KV entries."

**Technical terms:**
- **lightning indexer** = a lightweight scorer that rates how relevant each preceding token is to the current query (uses RoPE-applied Q/K and a scalar weight `w`).
- **Top-K Selector** = keeps only the K highest-scoring KV entries.
- **Top-K** = selecting the top K items from a ranked list.

> "Together, the Lightning Indexer and Top-K Selector reduce the number of tokens to attend to K. In the paper referenced, the authors selected 2048 tokens to attend to, which drastically reduces the computation necessary for the attention computations."

**Memorize:** DSA attends to only K tokens (e.g., 2048) rather than the entire sequence.

> "Note that since MLA is used, there is already a significant memory benefit due to the compression of the KV-cache. In their construction of MLA, instead of using MHA as the core attention mechanism, MQA was used. The authors mentioned optimizing for computational efficiency, which is true for MQA, considering it uses fewer keys and values than MHA."

**Key point:** DeepSeek builds DSA on top of MLA, and uses **MQA** (not MHA) as the core attention for maximum computational efficiency (fewer K and V).

---

## 2.10 Mixture of Experts (MoE)

> "A technique that is becoming more mainstream to create more efficient LLMs is MoE. MoE uses several submodels or 'experts' to improve the quality and efficiency of LLMs. There are two main components of an MoE: **Experts** — Each feed-forward neural network in an LLM is replaced by a set of 'experts'; **Router or gate network** — Determines which tokens are sent to which experts."

**Word meanings:**
- **mainstream** = widely adopted.
- **submodels** = smaller component models.

**Technical terms (two components — memorize):**
1. **Experts** = the set of smaller feed-forward networks replacing the single one.
2. **Router (gate network)** = decides which expert handles each token.

> "Remember that a typical Transformer-based LLM uses Self-Attention followed by a feed-forward neural network. We call this a dense model because everything is activated. In a dense Transformer block, the feed-forward neural network is a single network applied to every token. Here, a 512-dimensional input is expanded to 2048 and compressed back down, with every connection and neuron active for every token that passes through."

**Technical terms:**
- **dense model** = all parameters/neurons active for every token.

> "A sparse model, in contrast, may deploy several feed-forward neural networks instead. These are typically smaller than a regular network, but together they tend to be bigger. Each feed-forward neural network in a sparse model is typically referred to as an 'expert' because, during training, each 'expert' learns different information and may specialize in the processing of certain tokens. For instance, one expert might be used for processing numbers, whereas another processes verbs. It's still a bit unclear what these experts actually learn, but there has been some research suggesting that they specialize in fine-grained information such as verbs versus numbers rather than each learning an entirely different domain."

**Word meanings:**
- **fine-grained** = very specific/detailed.

**Technical terms:**
- **sparse model** = multiple experts, only some active per token.
- **specialization** = each expert learns to handle certain kinds of tokens (e.g., numbers vs verbs).

> "To choose a subset of experts during inference, we make use of the router (also called a gate network). This is a small feed-forward neural network that is trained to choose an expert for a given token. The router, together with the experts, makes up the MoE layer."

> "The router is arguably the most important component because the experts are nothing more than just small feed-forward neural networks. So, how exactly does the router then choose which expert to use for each token? The router, as a neural network, will have its own weight matrix, which is used to multiply the input token embeddings. Applying a softmax on the output will result in a probability distribution per expert. This probability distribution provides the likelihood that an expert will be chosen given an input token. The highest-probability expert (FFNN 1 at 0.45) is selected to process the token, and its output is scaled by the router's probability before being passed forward."

**Word meanings:**
- **scaled by** = multiplied by.

**Technical terms (memorize the router mechanics):**
- The router multiplies the token embedding by its own weight matrix, applies **softmax** → a probability over experts.
- The highest-probability expert is selected.
- **Crucially, the selected expert's output is scaled by the router's probability** before being passed forward.

> "Note that any number of experts can be selected, but generally a fixed number are selected for training and inference. When selecting multiple experts, there is a need to balance how much each expert is trained. If the same set of experts is always chosen during training and inference, then all other experts are undertrained."

**Word meanings:**
- **undertrained** = not trained enough (because rarely used).

**Technical terms:**
- **fixed number of experts** = e.g., always activate 2 out of 8 (or 8 out of 256) per token.

### Load balancing

> "To balance the distribution of training among experts, the router will have to dynamically balance which expert to choose and when. This is referred to as load balancing. To prevent one expert from dominating the training time, there are two main techniques that are often employed in one way or another, namely expert capacity and auxiliary loss."

**Word meanings:**
- **dominating** = taking over / being used too much.

**Technical terms (two load-balancing techniques — memorize):**
1. **Expert capacity** — a limit on how many tokens each expert can process per batch. When an expert is full, overflow tokens go to the next-highest-scoring expert.
2. **Auxiliary loss** — an extra loss term added to the router that rewards equal distribution of tokens across experts (or punishes repeatedly choosing the same expert).

> "Expert capacity gives each expert in the MoE layer a limit to how many tokens it can process. Instead of having a single expert do all the work, the tokens are somewhat more equally distributed. For instance, by the time an expert has reached capacity, each subsequent token routed to it will be sent to the next-highest scoring expert."

> "In contrast, instead of limiting the experts, the router can also be adjusted to account for this probability imbalance. A straightforward technique is to add Gaussian noise just before the router produces its probabilities. By introducing noise, the distributions will slightly change, and by (slight) chance, sometimes choose different experts to use. A more advanced technique to balance how the router selects experts is called auxiliary loss. These are loss functions that can be added to the router to reward it for equally distributing the experts during training or punish it when the same expert is chosen."

**Technical terms:**
- **Gaussian noise** = random values added to the router input (a simple balancing trick).
- **auxiliary loss** = a penalty/reward term steering the router toward balance.

### Sparse vs active parameters

> "The main benefit of MoE is its computational requirements. Although using MoE does not make the resulting model smaller, it runs much faster because only a few experts are activated at a given time. All parameters that a MoE model has need to be loaded into memory and are called sparse parameters. The active parameters, in contrast, are those that are activated only during inference."

**Technical terms (memorize):**
- **sparse parameters** = ALL parameters, which must all be loaded into memory (e.g., 30B).
- **active parameters** = the subset actually used during inference for a given token (e.g., 3B).
- MoE models aren't smaller — they just use less compute per token because only a few experts are active.

> "Most recent models tend to use MoE layers, such as OpenAI's GPT-OSS and NVIDIA's Nemotron 3. Often, you'll see models like Qwen3-30B-A3B that put the number of sparse parameters (30 billion) and active parameters (3 billion) in their name. As such, even though 30 billion parameters need to be loaded in memory, only 3 billion are used, which makes it much faster for inference."

**Example (memorize):** **Qwen3-30B-A3B** = 30B sparse (loaded into memory) + 3B active (used per token). Naming convention: `ModelName-SparseB-ActiveB`.

> "Another example of MoE is the previously discussed DeepSeek-R1. As shown in Figure 2-45, DeepSeek-R1 has 256 experts, of which 8 are always chosen. Note that there is also a shared expert bypassing the router. This expert is always chosen, which often helps the model divert all general knowledge to that expert and more specialized knowledge to all others."

**Technical terms (memorize):**
- **shared expert** = an expert that bypasses the router and is *always* active; it tends to absorb general knowledge while the routed experts specialize.
- **DeepSeek-R1 config**: 256 experts, 8 activated per token, plus 1 shared expert.

> "In practice, there are many different choices for the number of experts chosen for a given LLM and the sizes of each expert compared to the overall size of the LLM."

**From the book's Table 2-1 (selected examples):**
| Model | Sparse params | Active params | Experts activated | Shared experts |
|---|---|---|---|---|
| Mistral 8x7B | 46.7B | 12.8B | 4 | 0 |
| DeepSeek-R1 | 671B | 37B | 8 | 0 (well, plus shared) |
| Llama 4 Maverick | 400B | 17B | 8 | 0 |
| Qwen 3 235B-A22B | 235B | 22B | 8 | 0 |
| Kimi-K2 | 1000B | 32B | 4 | 2 |
| GPT-OSS 120B | 120B | 5.1B | 4 | 0 |
| GPT-OSS 20B | 20B | 3.6B | 8 | 0 |
| GLM 4.5 | 335B | 12B | 10 | 0 |
| Mistral 3 Large | 671B | 41B | 6 | 2 |

*(Note: the book's table numbers are illustrative of the sparse/active convention; the key takeaway is the sparse-vs-active distinction and that different models pick different expert counts.)*

---

## 2.11 Summary (Chapter 2)

> "This chapter covered the engine that powers every agent in this book: the LLM. Part 1 looked at LLMs from the perspective of an agent developer. Language models consume and produce tokens, and the formats built on top of those tokens–system prompts, multi-turn conversations, and tool calls–are what let a language model serve as the reasoning core of an agent. We then walked through how LLMs are created across two training phases. Pre-training via next-token prediction produces a base model. Post-training via SFT and RL shapes the base model into something that follows instructions and generates responses aligned with human preferences. We looked at how RLVR and the GRPO algorithm use verifiable signals such as format and correctness to push models toward reliable behavior. We also opened up the Transformer itself, tracing how tokens flow through a stack of blocks, each containing a self-attention layer and a feed-forward neural network, before the LM head converts the final representation into a next-token probability distribution. For agent developers, the two architectural properties worth carrying into later chapters are context length, which limits how much information we can pack into a model's input, and the KV cache, which shapes the economics and latency of every agent we build."

**Word meanings:**
- **aligned with** = matched to.

**Memorize these two key architectural properties:**
1. **Context length** — the input size limit (drives context engineering).
2. **KV cache** — drives speed/cost of every agent.

> "Part 2 went deeper into the internals. We saw how self-attention is actually computed through the Queries, Keys, and Values produced by three projection matrices and how relevance scoring and information combining make up the two steps of the attention operation. We then looked at how self-attention has evolved to address its memory and compute costs. The KV cache itself avoids redundant recomputation. Grouped-Query and Multi-query Attention shrink the cache by sharing K and V across heads. DeepSeek's Multi-head Latent Attention takes a different approach, caching a low-rank compressed representation that gets projected back up when needed. DeepSeek Sparse Attention adds a Lightning Indexer and Top-K Selector to attend to only the most relevant tokens instead of the entire sequence. We closed with MoE architectures, which replaced the dense feed-forward layer with a router and a set of smaller expert networks. This produces models where the total and active parameter counts can differ by an order of magnitude, a distinction that matters when selecting a model to deploy."

**Word meanings:**
- **order of magnitude** = roughly 10× (a factor of ten).

---

## Quick-Reference Glossary (Chapter 2)
- **LLM** = Large Language Model; predicts/generates tokens.
- **Token** = word/part-word/number/punctuation unit of I/O.
- **Autoregressive** = generates by feeding its own output back in.
- **Base model** = after pre-training (next-token prediction).
- **SFT** = supervised fine-tuning / instruction tuning (prompt excluded from loss).
- **RLHF** = RL from human/reward-model preferences.
- **RLVR** = RL with automated verifiable rewards.
- **GRPO** = group-relative policy optimization (groups + relative rewards).
- **Tokenizer** = text → token IDs.
- **Embeddings** = numeric vectors per token.
- **Transformer blocks** = self-attention + feed-forward layers.
- **LM head** = final layer → probability distribution over vocabulary.
- **Decoding** = picking a token (greedy at temperature 0; sampling above).
- **Context length** = max simultaneous tokens.
- **KV cache** = cached K and V for fast generation (Q not cached).
- **Self-attention** = Q·Kᵀ/√d_k → softmax → ·V.
- **MHA / MQA / GQA / MLA / DSA** = attention variants (see table).
- **RoPE** = Rotary Position Embedding (position info).
- **MoE** = router + experts; sparse vs active parameters.
- **Shared expert** = always-active expert bypassing the router.
- **Load balancing** = expert capacity / auxiliary loss.
