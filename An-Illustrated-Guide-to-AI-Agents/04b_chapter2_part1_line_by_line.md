# Chapter 2 — Line-by-Line Detailed Explanation (Part 1)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 2 "Large Language Models" — Part 1 (agent-developer level) plus training.
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
