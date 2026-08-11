# Chapter 6 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 6 "Planning and Reflection"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings are paraphrased/annotated; every substantive paragraph is covered.

---

## 6.1 Planning and Reflection (Chapter Intro)

> "In Chapter 3, we covered reasoning LLMs and their capabilities to demonstrate complex chains of thought. Advanced reasoning enables behavior that is vital in agentic systems, namely the ability to plan actions and reflect on them."

**Explanation:** Reasoning (Ch 3) enables two vital agent behaviors: planning actions and reflecting on them.

**Word meanings:**
- **vital** = essential.
- **reflect on** = think back over.

---

> "Agents are multi-step entities and generally require planning the actions they will take to complete their goals. Imagine you ask an agent to create a specific feature for your codebase. To bring this to completion, typical steps the agent takes include:
> 1. Clarify requirements
> 2. Analyze the existing codebase
> 3. Design the feature
> 4. Implement the feature
> 5. Test the feature
> 6. Update documentation
> To execute all these actions, the agent first needs to be aware of them and track the status of its current state. Planning is essential to solidify this behavior and to allow the agent to dynamically adjust the plan when necessary. After all, without a plan, how would it know what to do next?"

**Explanation:** An agent is multi-step: it must know the steps, track progress/state, plan, and adjust dynamically. Example: creating a feature → clarify → analyze → design → implement → test → document.

**Word meanings:**
- **solidify** = make firm/reliable.
- **dynamically** = in real time, responding to changes.
- **track the status** = keep aware of current progress.

---

> "This plan and the steps that it contains can often be taken multiple times, such as implementing and testing the feature whenever the agent encounters a bug. Iterative loops are similarly a fundamental component of what makes an agent. As such, the initial plans are made malleable and should be open to change when there is an issue with the plan or its execution. Reflection on what has been done and what could be improved is, therefore, vital for the autonomous nature of the agent. Planning and reflection go hand in hand, and by implementing a feedback loop, the system prevents ending up in a local minima."

**Explanation:** Steps repeat (e.g., implement/test when bugs appear). Plans must be malleable. Reflection + feedback loop prevent getting stuck in a local minima (a suboptimal solution).

**Word meanings:**
- **malleable** = flexible, open to change.
- **go hand in hand** = are closely connected.

**Technical terms:**
- **iterative loops** = repeating cycles of actions.
- **local minima** = a suboptimal state the system can't easily escape (metaphor from optimization).

---

> "In this chapter, we explore planning and reflection in agents and how they can be used to plan out multi-step actions and improve their behavior as they go through the necessary steps. Shown in Figure 6-1, this brings us to the final component of a single agent."

**Explanation:** Planning is the final core component of a single agent.

---

## 6.2 Planning

> "In Chapter 3, we explored how reasoning and extended thinking can bring LLMs to new heights. We briefly touched upon something vital in these systems, namely that advanced reasoning allows LLMs to plan out their behavior. There is a wide variety of methodologies to enable planning in reasoning LLMs effectively."

**Explanation:** Reasoning lets LLMs plan; many methodologies exist for planning.

### Task Decomposition

> "The first step in planning is to decompose an initial query into subtasks. Like the example at the beginning of the chapter, when you ask an agent to create a certain feature, it will split that query up into smaller tasks to execute. This is called task decomposition and allows the agent to simplify complicated tasks. Figure 6-2 shows an example of the reasoning LLM creating this plan, where each task may also have a set of subtasks to complete."

**Explanation:** Task decomposition = splitting a query into subtasks (which may themselves have subtasks) to simplify the work.

**Technical terms:**
- **task decomposition** = breaking a complex query into manageable subtasks.

---

> "The tasks and subtasks in this plan may describe a tool that the reasoning LLM may use. As shown in Figure 6-3, the plan comes first, after which each (sub)task can be processed using tools. In this example, the agent creates a three-step plan to add a new feature to a codebase. The first task, analyzing the existing codebase, is done with a tool call to retrieve the code. It continues to analyze that code and communicate the core features. After that, it may update the plan and continue with the next task."

**Explanation:** Plan first, then each task uses tools; the plan can be updated between tasks. Example: three-step plan for "add a new feature" — Task 1 retrieves code via a tool call.

---

### Chain-of-Thought

> "A major component of task decomposition, aside from having a reasoning LLM, is prompt engineering. We covered several of them in Chapter 3 from the perspective of reasoning traces, where several 'thoughts' are sampled. We also covered Chain-of-Thought (CoT) as a common approach to create these reasoning traces. Remember that CoT is a process where the LLM is asked to solve problems step-by-step. This can be achieved by either providing examples to the LLM (few-shot prompting) or by simply stating, 'Let's think step-by-step' (zero-shot prompting). This step-by-step process is, in fact, task decomposition. The original query is processed through separate reasoning traces that rely on the ones that came before (Figure 6-4). In other words, it breaks down the problem into sequential reasoning substeps."

**Explanation:** CoT = step-by-step solving, via few-shot (examples) or zero-shot ("Let's think step-by-step"). The step-by-step reasoning *is* task decomposition — sequential substeps building on each other.

**Technical terms:**
- **Chain-of-Thought (CoT)** = prompting the LLM to reason step by step.
- **few-shot prompting** = providing examples in the prompt.
- **zero-shot prompting** = no examples, just an instruction.

---

> "The original idea of CoT was to make the model 'think aloud' as a way to scale test-time compute and its performance. However, the 'thinking' may very well be used as part of the planning process. For instance, when asked to create a plan and end it with 'think step-by-step,' a reasoning LLM will first think about the steps needed to resolve the original query. This is the reasoning trace the LLM can leverage to create the plan."

**Explanation:** CoT's original purpose was "thinking aloud" to scale test-time compute — but the same reasoning trace can drive planning.

**Technical terms:**
- **test-time compute** = computation spent during inference/answering (vs. during training).

---

**Example (paraphrased):**
> "User: Add a new feature to my existing codebase. Let's think step-by-step.
> LLM (CoT): First, I need to clarify the requirements... Second, I should analyze the existing codebase... Third, I need to design the feature... Fourth, I should implement the feature... Fifth, I must test the feature... Finally, I need to update the documentation...
> LLM (Plan): The plan will be as follows: [ ] Clarify requirements / [ ] Analyze the existing codebase / [ ] Design the feature / [ ] Implement the feature / [ ] Test the feature / [ ] Update documentation"

**Explanation:** The CoT trace (thinking aloud) becomes the plan (task list).

---

> "As such, CoT is a flexible technique that can be used for task decomposition in general, not just for 'thinking aloud.' Since CoT, there have been many variants that similarly attempt to create more advanced reasoning traces for the purpose of task decomposition. These techniques, such as self-consistency and tree of thoughts, sample multiple CoT traces from the LLM to find the most optimal trace. As shown in Figure 6-5, the original task is decomposed into 'thoughts' (but could have been subtasks just the same), and from various 'thoughts,' the most optimal set is chosen."

**Explanation:** CoT variants sample multiple traces to find the best one.

**Technical terms:**
- **self-consistency** = sample several CoT traces, pick the answer most agree on.
- **Tree of Thoughts (ToT)** = explore branching reasoning paths, pruning dead ends.

---

> "For self-consistency, the original task is broken down into subtasks, much like CoT, but multiple diverse paths are generated, and the most consistent answer is selected. In contrast, Tree of Thoughts explores multiple possible reasoning paths (where each node is a task or 'thought') instead of following a linear chain-of-thought. As we explored in Chapter 3, methodologies like Beam Search and Monte Carlo Tree Search using reward models can be used to choose the best path."

**Explanation:** Self-consistency = many diverse paths, vote the most consistent answer. ToT = branching search with pruning; Beam Search / MCTS + reward models pick the best path.

**Technical terms:**
- **Beam Search** = keeping the best K candidate paths at each step.
- **Monte Carlo Tree Search (MCTS)** = exploring paths via random sampling guided by rewards.
- **reward models** = models that score candidate outputs.

---

### Explicit Planning

**Least-to-Most (LtM):**
> "A CoT-like prompting technique for task decomposition that works quite well in the context of planning is least-to-most (LtM) prompting. Released in 2022, LtM builds upon CoT by first decomposing the problem into subproblems. Then, these subproblems are solved one by one. Compared to CoT, the solution of each subproblem is fed back into the LLM when attempting to solve the next problem (Figure 6-6)."

**Explanation:** LtM (2022) explicitly separates planning from execution: decompose, then solve subproblems sequentially, feeding each answer forward.

**Technical terms:**
- **least-to-most prompting** = decompose into subproblems, solve sequentially, carrying answers forward.

---

> "As in CoT prompting, the problem to be solved is decomposed into a set of subproblems that build upon each other. In a second step, these subproblems are solved one by one. Contrary to CoT, the solution of previous subproblems is fed into the prompt, trying to solve the next problem."

**Explanation:** Key difference vs CoT: previous sub-answers are fed into the prompt for the next problem.

---

> "A more practical example is shown in Figure 6-7, where instead of attempting to solve the problem directly, two stages are executed:
> - Problem reduction: The original problem is decomposed into a set of manageable tasks.
> - Sequentially solve subquestions: Each subtask is solved after the other. After each subtask, the previous subtask's question and answer are appended.
> To create this behavior, few-shot prompting is used, where examples are provided to the LLM to show how to decompose the initial tasks and subsequently solve them."

**Explanation:** Two LtM stages: (1) problem reduction (decompose), (2) sequential solving (append prior Q&A). Implemented via few-shot prompting.

**Word meanings:**
- **appended** = added to the end.
- **manageable** = easy to handle.

---

**Plan-and-Solve:**
> "A zero-shot approach to task decomposition in the context of planning is plan-and-solve prompting. This methodology is a direct extension of the zero-shot CoT, where instead of using 'Let's think step-by-step,' the LLM is asked to devise a plan first and then solve the problem step-by-step (Figure 6-8). It's a two-step process that is akin to the thinking and answer steps that we explored in Chapter 3:
> - Prompting for reasoning generation: The model generates a plan and then carries it out by solving the problem step-by-step.
> - Answer extraction: The output of the previous step is processed to create the final answer."

**Explanation:** Plan-and-solve = zero-shot: first make a plan, then solve. Two calls: reasoning generation (plan + solve) then answer extraction.

**Technical terms:**
- **plan-and-solve prompting** = zero-shot technique where the LLM plans first, then solves, then the answer is extracted.

---

> "Note how we again used the Q/A structure throughout these prompts. That was common in these early days of LLMs, where they were often trained primarily to autocomplete the prompt, so additional steering was needed for the model to understand that it should answer a given question. Their output is essentially the 'thinking' process we know of reasoning LLMs, but it works just the same for regular LLMs, albeit an implicit process. Nowadays, most LLMs are already capable of creating advanced plans without needing complex instructions or specialized prompts. However, as explored in Chapter 3, they often need to be trained on these CoT- or planning-like reasoning traces to effectively reason and plan."

**Explanation:** Q/A steering was needed for autocomplete-trained models. Modern LLMs plan well, but usually still need training on reasoning traces to do so effectively.

**Word meanings:**
- **albeit** = although.
- **implicit** = not explicitly stated.
- **steering** = guiding the model's behavior.

---

### Action Sequencing

> "Least-to-most prompting and plan-and-solve prompting decompose a given problem into subtasks that are solved sequentially. These work great for LLMs, but not yet for agents, who sequence actions autonomously one after the other. Specifically, after the agent decides the (sub)goals it needs to achieve, it then determines a sequence of actions to continuously transition it from its current state to the desired goal state. This process is called action sequencing and requires not only a careful plan but also an efficient set of steps for the agent to follow."

**Explanation:** LtM and plan-and-solve are one-shot planning — they work for LLMs but not autonomous agents. Action sequencing = deciding a sequence of actions to move from current state to goal state.

**Technical terms:**
- **action sequencing** = choosing and ordering actions that transition the agent toward its goal.

---

> "For example, a coding agent needs to decide which optimal sequence of actions should be taken. If it skips over reading the codebase, for instance, then the agent would be quite inefficient and this could potentially lead to redundancy. To minimize wasted resources and reduce execution time, there needs to be a well-thought-out sequence of actions relating to the plan. In this section, we'll explore methodologies for sequencing actions, of which important components are CoT-like reasoning and planning the sequence of actions."

**Explanation:** Bad ordering (e.g., skipping reading the codebase) → inefficiency and redundancy. Sequencing needs CoT-like reasoning + planning.

**Word meanings:**
- **redundancy** = wasteful repetition.
- **well-thought-out** = carefully designed.

---

> "We want the agent not only to create a plan but continuously solve each subtask while updating its current state. This idea is shown in Figure 6-9 and requires more sophisticated techniques than least-to-most prompting and plan-and-solve prompting. The idea of these techniques is that agents rely on their LLMs' reasoning capabilities to dynamically adjust their plans based on new information as a result of previous steps. It also explains how agents can act autonomously after having created the initial plan."

**Explanation:** Agents must *interleave* plan and action: execute a step, update the plan from the outcome, loop until done — beyond one-shot techniques.

**Word meanings:**
- **sophisticated** = advanced/complex.
- **interleave** = alternate between.

---

### Reason and Act with Prompting (ReAct)

> "In previous examples, we explored how reasoning can be enabled with CoT and how LLMs can take actions (such as with the Toolformer discussed in Chapter 5). This split between reasoning and acting is shown in Figure 6-10."

**Explanation:** Before ReAct, reasoning (CoT) and acting (Toolformer) were separate capabilities.

---

> "However, having reasoning and acting separate makes it difficult for an agent to iterate. A common technique for determining an initial plan and continuously updating its action sequencing is the Reason and Act (ReAct) framework. Compared to CoT, which embeds reasoning within the planning, ReAct decouples reasoning and planning. The framework takes inspiration from CoT for reasoning and tool usage for acting and combines them. As shown in Figure 6-11, this framework interleaves both reasoning traces and task-specific actions to create an iterative process of thinking and taking actions. As a result, we get the first truly autonomous systems that drive AI agents."

**Explanation:** ReAct fuses reasoning (from CoT) with acting (tool usage) into an iterative thinking-acting loop — producing the first truly autonomous agents.

**Technical terms:**
- **ReAct** = Reason and Act — interleaves reasoning traces and actions.

---

> "The interleaving of reasoning and actions creates a feedback loop in which the model repeatedly cycles through a thought-action-observation process. In each loop, the model is asked to separate its textual output into three components:
> - Thought: A reasoning step about the current situation
> - Action: A set of actions to execute (e.g., tools)
> - Observation: A reasoning step about the result of the action
> This is achieved by prompting the model to create these three separate entities (Figure 6-12). Note that few-shot examples of thought, action, and observation cycles are typically added to make sure that the LLM matches the proposed behavior."

**Explanation:** Each loop cycle = Thought (reason) → Action (execute tools) → Observation (result). Enabled by prompting; few-shot examples reinforce the pattern.

**Word meanings:**
- **feedback loop** = a cycle where outputs feed back into inputs.
- **reinforce** = strengthen.

---

> "This type of prompt is at the core of a ReAct agent and steers the LLM's behavior toward cycles of thoughts, actions, and observations (see Figure 6-13). By iterating over thoughts and observations, the LLM can plan out actions, observe its output, and adjust accordingly. Then, the LLM will stop whenever it reaches a predefined goal."

**Explanation:** The ReAct prompt steers cycles; the LLM plans, acts, observes, adjusts — stopping at a predefined goal.

---

> "ReAct agents can theoretically go on for hundreds of cycles but need to remember their previous steps and outcomes to effectively continue when context becomes an issue. For agents that go on to perform dozens or potentially hundreds of steps, the memory component becomes vital, especially context engineering, as we discussed in Chapter 4."

**Explanation:** Long ReAct runs need memory + context engineering (Ch 4).

---

**Implementing ReAct:**
> "To give a bit more intuition about this ReAct flow and the final step in making your TinyAgent fully autonomous, let's implement it. For the most part, ReAct requires careful prompting to nudge the LLM's behavior toward loops of THOUGHT, ACTION, and OBSERVATION. To do so, let's create the ReAct class as the first planner class to use in your TinyAgent."

**Code (paraphrased) — `ReAct`:**
```python
class ReAct:
    def __init__(self, max_steps: int = 10):
        self.max_steps = max_steps

    @property
    def prompt(self) -> str:
        return """# ReAct (Reason and Act)
You are a ReAct agent that performs exactly ONE step per turn.
## ReAct Format
You use the following format for each step:
THOUGHT: [Your reasoning about what to do next]
ACTION: { "tool": "a_tool_name", "kwargs": {"param": "value"} }
An observation will be provided after each action. You do not generate the observation yourself.
## ReAct Completion
To provide the final answer to the task, use an action blob with "tool": "final_answer" tool.
It is the only way to complete the task, else you will be stuck on a loop.
... Use the `final_answer` tool when you are completely done with all subtasks ..."""

    def parse(self, response: Response) -> Response:
        patterns = {
            "THOUGHT": r"THOUGHT:\s*(.+?)(?=ACTION:|OBSERVATION:|$)",
            "ACTION": r"ACTION:\s*(.+?)(?=THOUGHT:|OBSERVATION:|$)",
        }
        result = {}
        for key, pattern in patterns.items():
            match = re.search(pattern, response.content, re.DOTALL)
            result[key] = match.group(1).strip() if match else ""
        response.content = result["ACTION"]
        response.reasoning = result["THOUGHT"]
        return response
```

**Explanation:** `ReAct` class: `max_steps` bounds the loop; `prompt` defines the THOUGHT/ACTION format and the `final_answer` completion rule; `parse` uses regex to split the response into THOUGHT and ACTION.

**Word meanings:**
- **nudge** = gently push.
- **blob** = a block/object (here, the JSON action).

**Technical terms:**
- **regex (re)** = regular expressions for pattern extraction.
- **`final_answer` tool** = the special tool that ends the agent and returns the answer.

---

> "Although it might seem like a lot, only three things are happening within this ReAct class.
> First, the max_steps parameter is used to decide the maximum number of steps your TinyAgent will get to perform its autonomous behavior. The agent can still decide to stop early, but this prevents it from being stuck in a loop.
> Second, the .prompt property, much like in Tools, is used to provide sufficient context on how to approach the ReAct behavior. It specifically describes the ReAct format (## ReAct Format) and how it should provide a final answer (## ReAct Completion). Note that this ReAct implementation assumes that the actions are with Tools and not the NativeTools class that we covered in Chapter 5.
> Third, the .parse function is used to convert the THOUGHT/ACTION description into separate fields so they can be used as the Response.reasoning and Response.content fields, respectively. Remember from Chapter 5 that the Tools class assumes that the tool call is initially in the Response.content field since Tools uses prompt-based tool calling rather than native tool calling."

**Explanation:** Three roles of `ReAct`: (1) `max_steps` bounds steps; (2) `prompt` gives format + completion rules (designed for `Tools`, not `NativeTools`); (3) `parse` splits THOUGHT/ACTION into `reasoning`/`content` for the `Tools` parser.

---

**Autonomy in TinyAgent:**
> "Now that we have the ReAct class, let's implement it in your TinyAgent to create your autonomous agent. The most important step to create a fully autonomous agent is…a for-loop! Truly, with the capabilities of agents these days, it all boils down to iteratively calling an LLM until it decides to stop the loop."

**Explanation:** Autonomy = a simple **for-loop** iteratively calling the LLM until it stops.

**Word meanings:**
- **boils down to** = ultimately reduces to.

---

> "With the ReAct framework, we structure the for-loop into the THOUGHT/ACTION/OBSERVATION steps to ensure the model has a clear goal and a clear set of steps to reach it. The implementation of this into your TinyAgent will require three main changes:
> - Add the ReAct instruction to the system prompt.
> - Add an autonomy loop to the .run function.
> - Parse the LLM's response into THOUGHT/ACTION."

**Code (paraphrased) — TinyAgent with autonomy:**
```python
class TinyAgent:
    def __init__(self, llm, memory, tools, planner):
        self.llm = llm
        self.memory = memory
        self.tools = tools
        self.planner = planner
        self.trajectory = Trajectory()

        system_prompt = "You are a helpful assistant.\n\n"
        system_prompt += self.planner.prompt
        system_prompt += self.tools.prompt
        self.memory.add("system", system_prompt)

    def run(self, task):
        self.memory.add("user", task)
        self.trajectory.initialize(task)
        # *Autonomy* loop
        for step in range(self.planner.max_steps):
            result = self._step()
            if result is not None:
                return result
        return "Max steps reached without completion."

    def _step(self):
        response = self.llm.generate(self.memory.get_messages(), tools=self.tools.schemas)
        self.memory.add("assistant", response.content, tool_call=response.tool_call)
        response = self.planner.parse(response)   # ReAct parse
        response = self.tools.parse(response)     # tool parse
        if self.tools.is_done(response):
            self.trajectory.add(response)
            return response.content
        return self._execute_action(response)

    def _execute_action(self, response):
        result = self.tools.execute(response)
        role, observation = self.tools.observation(result)
        self.memory.add(role, observation)
        self.trajectory.add(response, observation)
        return None
```

**Explanation:** Three changes: ReAct prompt added to system prompt; `run` now loops up to `max_steps` (returns when a step returns content); `_step` parses with both `planner.parse` (THOUGHT/ACTION) and `tools.parse` (tool call).

---

> "We highlighted the sections that were added to indicate how little actually is needed to go from a tool calling LLM to an autonomous agent."

**Explanation:** Minimal code turns a tool-calling LLM into an autonomous agent.

---

**Running the agent:**
> "Let's try the agent out and ask it to use several tools to answer a given question. We keep it simple and give it the add, multiply, and subtract tools. ... `agent.run("What is (4.6 + 6.685) x 4, and then subtract 3.14 from the result?")` → `'42.0'`. That is correct!"

**Explanation:** With add/multiply/subtract tools, the agent answers "(4.6 + 6.685) × 4 − 3.14 = 42.0".

---

> "Although this is the correct answer, we're much more interested in the full trajectory of the agent. Did it correctly use the tools one at a time and decide to stop when it got the answer? To find out, let's print out the trajectory. ... The trajectory shows that the agent took four steps:
> - add: The agent using the THOUGHT field to already make up a plan to execute, of which the first was to add values together.
> - multiply: It reflects on its previous action and continues on with the next.
> - subtract: It reflects on its previous action and continues on with the next.
> - Returned the final answer: The agent saw that it calculated all the necessary steps and returned the final with the final_answer tool."

**Explanation:** Trajectory: add → multiply → subtract → final answer. Each step: THOUGHT plan + tool action + OBSERVATION result.

---

> "With the ReAct framework, we have all the major components to create a truly autonomous system that is capable of independently taking actions:
> - Reasoning LLM: The agent's brain that is capable of advanced decision making and planning
> - Tools: Used to interact with the agent's environment
> - Memory: Prevents the LLM from forgetting past actions and observations
> - Planning: Task decomposition to create plans and frameworks, such as ReAct, to create autonomous behavior
> When all these components are combined in one system, and that system is capable of running continuously (imagine a while-loop or a for-loop), you have an agent!"

**Explanation:** The four pillars of an autonomous agent: reasoning LLM, tools, memory, planning — plus a continuous loop.

---

> "Where tools, as covered in Chapter 5, were all about single-turn processes, ReAct-like frameworks turn them into multi-turn processes that allow agents to make complete use of memory, tools, and planning."

**Explanation:** Tools = single-turn; ReAct = multi-turn, fully using memory/tools/planning.

**Word meanings:**
- **single-turn / multi-turn** = one-step vs. iterative processes.

---

### Reason and Act with Supervised Fine-tuning (FireAct)

> "ReAct-like frameworks have been quite popular for performing action sequencing. However, much like we explored in the tooling chapter, prompt engineering is only the first step in steering an LLM's behavior. Prompt engineering, much like is done in ReAct, often requires few-shot prompting to get the correct behavior. This can waste tokens in the context and be difficult to get right. FireAct is one of the first methodologies to fine-tune an LLM on ReAct trajectories as a way to instill the ReAct framework into a small LLM. The method is quite straightforward and consists of two steps."

**Explanation:** Prompt ReAct needs few-shot examples (wastes tokens, brittle). **FireAct** fine-tunes a small LLM on ReAct trajectories instead. Two steps.

**Word meanings:**
- **instill** = build a behavior into.
- **brittle** = fragile/breakable.

**Technical terms:**
- **FireAct** = fine-tuning an LLM on ReAct trajectories.

---

> "In step 1, a LLM (GPT-4) is used to generate different kinds of trajectories (e.g., CoT and ReAct) based on questions from several datasets. The idea is that for different questions, different methodologies might be needed to solve them. A straightforward question that requires no agentic behavior would need only CoT-like inferencing, whereas a potential multi-turn question would require a ReAct framework to solve the problem. In Figure 6-15, you can see how various questions are processed through different prompt templates (CoT, ReAct, and Reflexion, an extension of ReAct that also includes feedback into the cycles) to generate their respective trajectories. A trajectory for a question processed with ReAct, for example, would contain sequences of thoughts, actions, and observations with a final answer."

**Explanation:** Step 1: GPT-4 generates trajectories using different templates (CoT for simple, ReAct for multi-turn, Reflexion for feedback) per question.

**Word meanings:**
- **respective** = each its own.

**Technical terms:**
- **Reflexion** = a ReAct extension adding verbal feedback into cycles.

---

> "These generated trajectories serve as the training data used for fine-tuning a smaller LM. The trajectories, however, are first converted to all have the same ReAct format. CoT, for instance, is turned into a one-round ReAct trajectory where the 'thought' is the intermediate reasoning and 'action' that returns the answer. Note that it does not have an observation since there is only one round. After formatting, this data is then used to fine-tune a smaller model (Llama 2). The authors experimented with both a full fine-tune as well as fine-tuning only a small part of the entire model using Low-Rank Adaptation. From their experiments, fine-tuning on trajectories outperformed the prompt-based ReAct framework. Perhaps more importantly, there was no need to add few-shot examples to create the ReAct cycles, which makes inference more efficient and prevents needlessly filling up the context."

**Explanation:** Step 2: normalize all trajectories to ReAct format (CoT → one-round ReAct), then fine-tune Llama 2 (full or LoRA). Result: outperformed prompt ReAct, no few-shot needed — more efficient inference.

**Technical terms:**
- **Low-Rank Adaptation (LoRA)** = fine-tuning a small subset of parameters.
- **Llama 2** = the small base model fine-tuned.

---

### Reason and Act with Reinforcement Learning (ETO)

> "As we explored in Chapter 5, a limitation of SFT is that the LLM learns to mimic the example data instead of adapting through experiences. It lacks the ability to explore the environment, which tends to lead to suboptimal policies. In contrast, RL is a powerful approach for enabling LLMs to refine their strategies by learning from the feedback rather than simply mimicking behavior. By receiving rewards for actions and refining its policy through trial and error, the model is encouraged to actively explore its environment."

**Explanation:** SFT mimics data (suboptimal); RL learns from feedback through trial and error.

**Word meanings:**
- **suboptimal** = less than best.
- **policy** = the strategy the model uses to choose actions.

---

> "An interesting technique to explore ReAct through the lens of RL is Exploration-based Trajectory Optimization (ETO). Compared to FireAct, ETO uses RL to encourage the LLM to learn and explore trajectories rather than attempt to mimic behavior. ETO consists of two steps."

**Technical terms:**
- **ETO** = Exploration-based Trajectory Optimization — RL for ReAct.

---

> "In step 1, the authors applied SFT, using ReAct data, on an LLM (Llama-2-7B Chat), to create a base agent that has inherent planning capabilities. The datasets used were tasks that require multi-step planning and actions, such as ALFWorld, a text-based environment mimicking typical households where agents are to perform specific tasks, such as 'clean some tomato and put it on the countertop' or 'examine an alarm clock with the desk lamp'."

**Explanation:** Step 1: SFT on ReAct data over Llama-2-7B Chat → base agent with planning. Data from **ALFWorld**, a text-based household environment.

**Word meanings:**
- **inherent** = built-in.

**Technical terms:**
- **ALFWorld** = a text-based simulation of household tasks.

---

> "As shown in Figure 6-16, the initial model was fine-tuned using successful ReAct trajectories that include the task and the ReAct trajectory. The authors refer to SFT as behavior cloning, since this technique encourages the LLM to mimic the behavior shown in the data instead of having it figure out the correct answer on its own. In this context, SFT is therefore often referred to as imitation learning."

**Explanation:** SFT here = behavior cloning / imitation learning — mimicking successful trajectories.

**Technical terms:**
- **behavior cloning** = training by imitating demonstrated behavior.
- **imitation learning** = another name for behavior cloning.

---

**ALFWorld example (paraphrased):**
> "You are in the middle of a room. Looking quickly around you, you see a safe 1, a shelf 4, a drawer 2, a bed...
> Your task is to: examine an alarmclock with the desklamp.
> > go to desk 1 → You arrive at loc 8. ...
> > take alarmclock 2 from desk 1 → You pick up the alarmclock 2 from the desk 1.
> > go to sidetable 2 → ...
> > use desklamp 1 → You won!"

**Explanation:** ALFWorld interaction: observations + commands (`go to`, `take`, `use`) until the task is solved.

---

> "Step 2 is an iterative process that switches between the exploration and training phases (as shown in Figure 6-17). During exploration, the base agent interacts with the environment and attempts to solve the given tasks. From the ReAct trajectories that are generated in this process, the failed trajectories are sampled. These are then paired with correct trajectories that were previously collected for these tasks. During the training phase, the pairs of trajectories are then used to further fine-tune the LLM using an RL algorithm, namely Direct Preference Optimization (DPO). During this fine-tuning, the authors aimed to increase the likelihood of successful trajectories and decrease the likelihood of failed trajectories. In other words, the agent learns contrastive information from the failure/success trajectory pairs to update the RL policy."

**Explanation:** Step 2: iterative — explore (sample failed trajectories), then train with **DPO** on failed/success pairs to prefer successful ones.

**Technical terms:**
- **Direct Preference Optimization (DPO)** = RL from preference pairs (chosen vs. rejected).
- **contrastive information** = learning from the difference between positive and negative examples.

---

> "As we have explored several times before, using SFT followed by RL is a strong learning paradigm for these models. By starting with SFT, the LLM learns the right type of behaviors and formatting it should use. It gives the foundation it needs to then explore and improve upon itself by using exploration and exploitation in RL."

**Explanation:** SFT-then-RL: SFT gives foundation; RL adds exploration/exploitation.

**Technical terms:**
- **exploration** = trying new actions.
- **exploitation** = using known-good actions.

---

> "A major benefit of using SFT, RL (or both) is that there is no need for ReAct via prompting. As covered previously, we used a specific prompting scheme (THOUGHT/ACTION/OBSERVATION) to create this ReAct-like behavior. This was possible because LLMs are great at instruction-following, so that we could steer the model toward this specific behavior. However, it can be quite brittle and takes up a fair bit of the system prompt. Training a model to do this natively is an answer to this. Fortunately, we already created many of the needed components to use the native ReAct capabilities of newer LLMs, Gemma 4 in particular."

**Explanation:** SFT/RL remove the need for brittle ReAct prompting. Native ReAct (trained into the model) is the alternative — via Gemma 4.

**Word meanings:**
- **brittle** = fragile.

---

### Native ReAct

> "The first step to do this is to choose a model that is actually capable of native reasoning and tool calls. The model we explored in previous chapters is Gemma 4 E4B, so let's choose that. `llm = LLM(model="gemma4:e4b", think=True)`."

**Technical terms:**
- **Gemma 4 E4B** = a model with native thinking and tool calling.

---

> "We previously implemented loops of:
> - THOUGHT: A reasoning step about the current situation
> - ACTION: An action to execute (e.g., a tool)
> - OBSERVATION: A generated observation (typically the output of a tool)
> However, with native tool calling, there is no need for explicit ACTION, and with native reasoning, there is no need for explicit THOUGHT. We can replace what we already have for each of them with the following:
> - THOUGHT → Replace with Response.reasoning and NativeReAct
> - ACTION → Replace with Response.tool_call and NativeTools
> - OBSERVATION → Replace with adding the output of a tool to memory using Response.observation, which gives us the tool role"

**Explanation:** Native replacements: THOUGHT → `Response.reasoning`; ACTION → `Response.tool_call` (NativeTools); OBSERVATION → tool role via `Response.observation`.

---

> "Since we already have NativeTools, we only need to create NativeReAct. In that class, it essentially removes all behavior from the previously generated ReAct class. With native tool calling and reasoning, there's no need for a ReAct-specific prompt or parsing it into THOUGHT/ACTION/OBSERVATION. Instead, all the behavior it needs is to define the maximum number of steps."

**Code (paraphrased) — `NativeReAct`:**
```python
class NativeReAct(ReAct):
    """ReAct using native LLM reasoning instead of text-based parsing."""
    @property
    def prompt(self) -> str:
        return ""

    def parse(self, response):
        return response
```

**Explanation:** `NativeReAct` keeps only `max_steps`; `prompt` is empty and `parse` is a passthrough — native models already handle reasoning and tool calls.

---

> "What is left is a for-loop that uses native tool calling and reasoning each step. The LLM decides what to use when and will stop only when there is no tool call. Since the LLM was trained specifically using SFT and RL to perform ReAct, it already knows how to do this. This means that the model can choose at every step whether to perform:
> - Reasoning: As captured in Response.reasoning
> - Tool calling: As captured in Response.tool_call
> - Providing a final answer: As captured in Response.content"

**Explanation:** Just a for-loop; the trained model chooses per step: reason, call tool, or answer.

---

**Running NativeReAct:**
> "Let's demonstrate it with an example: `agent.run("What is (4.6 + 6.685) x 4, and then subtract 3.14 from the result?")` → `'The result is 42.0.'` As before, the answer is correct. Your TinyAgent now does ReAct natively. To see what happened behind the scenes, let's explore the trajectory."

**Explanation:** Same query answered natively; answer 42.0.

---

> "Looking at the steps, you can see something similar to the ReAct loop we had before:
> - Step 1: Reasoning, tool call (add), and observation
> - Step 2: Tool call (multiply) and observation
> - Step 3: Tool call (subtract) and observation
> - Step 4: Reasoning and answer
> Although similar to loops of THOUGHT/OBSERVATION/ACTION, it foregoes reasoning when it calls the tool. Each LLM will have different behavior, but they do generally boil down to these kinds of loops."

**Explanation:** Native trajectory: add → multiply → subtract → answer. Unlike prompt ReAct, it often skips reasoning when calling tools (model-dependent).

**Word meanings:**
- **foregoes** = does without / skips.

---

> "The true foundation of your TinyAgent is now complete! By exploring both prompt-based and native capabilities, you get to see how an agent actually works behind the curtain. If we had jumped directly into native capabilities, much of the behavior would have seemed like a closed box."

**Explanation:** Studying both prompt-based and native ReAct reveals how agents actually work.

**Word meanings:**
- **behind the curtain** = hidden workings.
- **closed box** = opaque, unexplained.

---

## 6.3 Agents That Continuously Improve

> "As AI agents evolve, instead of relying on training phases or reward models for improvement, these systems start relying on self-generated feedback loops to continuously improve their performance as they act. Feedback prevents agents from ending up in local minima and provides valuable information on potentially better solutions to pursue. In practice, not all feedback is equivalent, and continuous self-improvement is tricky to pursue for LLMs that are fundamentally static when they act. In this section, we explore methods for AI agents to reflect on their behavior and pursue self-improvement as they act with their environment(s)."

**Explanation:** Shift: self-generated feedback loops instead of training/reward models. Feedback prevents local minima. Challenge: LLMs are static during acting.

**Word meanings:**
- **equivalent** = of equal value.
- **fundamentally static** = unchanged at inference time.

---

## 6.4 Reflection

> "The feedback that an agent gets from their environment is typically different from process feedback. The environment gives feedback based on an action: 'when I do X (action), the environment gives back Y (feedback).' Feedback, as covered in this section, is often referred to as 'reflection' to demonstrate that it is typically an internal process where the LLM should be critical of its own output and processes. After all, reflecting on past behavior helps us learn from prior failings. Reflection, however, can also be in tandem with external sources to supply feedback."

**Explanation:** Environment feedback = action→result. Reflection = internal, critical self-assessment (can combine with external feedback).

**Word meanings:**
- **in tandem with** = together with.
- **prior failings** = past mistakes.

---

> "The techniques covered in this section are all based on prompting techniques and are therefore relatively straightforward to implement."

**Explanation:** Reflection techniques here are prompt-based.

### Self-Refine

> "An elegant approach to feedback is Self-Refine, a prompting framework where the LLM provides feedback and refines its own results. The approach is quite straightforward and lets an LLM iteratively improve its answer by acting as its own editor. The idea was inspired by how people draft and revise their solutions."

**Explanation:** Self-Refine: the LLM acts as its own editor — draft, critique, revise.

---

> "In this framework, the LLM generates an initial answer and then proceeds to critique its own output (feedback). Based on that reflection step, the LLM refines its answer and incorporates the necessary change (refine). This cycle repeats until a stopping criterion has been reached, such as the number of steps or an LLM-guided stopping mechanism. This cycle between feedback and refine is shown in Figure 6-18."

**Explanation:** Loop: generate → feedback (critique) → refine → repeat until a stopping criterion (step count or LLM judgment).

---

> "The same LLM generates the initial output, the refined output, and feedback, hence the name Self-Refine."

**Explanation:** One LLM does everything — hence "self."

---

> "As you might have noted, this framework was built around LLMs refining and improving their own outputs, but no mention is given of an agentic system. Next, let's explore how ReAct can use similar feedback mechanisms to improve agentic trajectories."

**Explanation:** Self-Refine = LLM-focused; Reflexion extends feedback to agentic trajectories.

### Reflexion

> "A more involved but quite successful attempt at instilling reflective behavior in agents is Reflexion, a prompting framework where agents verbally reflect on previous tasks through internal and external feedback. The authors used three entities within this framework:
> - Actor LLM: A ReAct LLM in charge of executing actions based on observations and trajectory data. This can be considered the main 'brain' of the agent.
> - Evaluator LLM: An LLM that assesses the quality of the current trajectory and output generated by the Actor LLM.
> - Self-Reflection LLM: This LLM generates more nuanced and specific feedback based on the full trajectory, including the output of the Evaluator LLM."

**Explanation:** Reflexion uses three LLMs: Actor (executes ReAct), Evaluator (scores), Self-Reflection (nuanced feedback).

**Word meanings:**
- **nuanced** = subtle and detailed.

---

> "As shown in Figure 6-20, the Actor LLM is based on a ReAct agent and will convert the initial task into actions and interact with the environment. The resulting observations are saved in the full trajectory for which the Evaluator LLM rewards a score that reflects the performance of the Actor LLM given the task. The Evaluator LLM is essentially asked, 'How well did the Actor LLM do?' and produces a scalar reward. The current trajectory, including the reward, is given to the Self-Reflection LLM to analyze the trajectory and produce a reflective summary for the Actor LLM to use. This process continues until the Evaluator LLM deems the answer of the Actor LLM to be correct."

**Explanation:** Loop: Actor acts → Evaluator scores (scalar reward) → Self-Reflection produces a summary → Actor uses it → repeat until Evaluator says correct.

**Technical terms:**
- **scalar reward** = a single number score.

---

> "These three LLMs are vital to provide the actions (Actor), the stopping mechanism (Evaluator), and the reflective feedback (Self-Reflector). They are tied together by the different forms of memory, where the trajectory is saved in short-term memory (conversation memory) and the reflective experiences based on the trajectories within the long-term memory. Together, these components allow Reflexion agents to verbally reflect on trajectory observations, which are maintained in episodic memory to promote improved decision-making in subsequent trials."

**Explanation:** Roles map to: actions (Actor), stopping (Evaluator), feedback (Self-Reflector). Memory types: short-term (conversation) for trajectory, long-term/episodic for reflective experiences.

**Technical terms:**
- **episodic memory** = long-term memory of past experiences/trials.

---

> "Self-Refine and Reflexion are common techniques to easily create agents that have some reflective capabilities. Many other prompt-based techniques exist that attempt something similar, such as CRITIC, which starts with an initial output and revises it through the use of external tools, like search, to get more information. As such, feedback is an important component of agents that can reflect, and as seen in the Reflexion framework, feedback can come from many places, including both itself and its environment."

**Explanation:** Other techniques: **CRITIC** — revises output using external tools (e.g., search). Feedback can come from the model itself or its environment.

**Word meanings:**
- **revises** = corrects/improves.

---

> "However, like all prompt-based techniques we have explored thus far, they only guide the agent toward behavior that we previously described without instilling that behavior into its parameters. Let's explore techniques that allow reflective behavior to be a part of an agent's nature through fine-tuning the model."

**Explanation:** Prompt-based reflection guides but doesn't bake behavior into weights. Next: training-based self-improvement.

---

## 6.5 Self-Improvement

> "Reflecting on its behavior is the first step to the continuous self-improvement of agents, and although prompt-level heuristics are an important component, more fundamental changes are needed to guide agents toward continual self-improvement. To create this self-sustaining loop of reflection, reasoning, and task generation, RL has been shown to enable seemingly unbounded self-improvement with limited need for human-labeled data."

**Explanation:** Reflection alone isn't enough; RL enables self-sustaining, scalable self-improvement with little labeled data.

**Word meanings:**
- **heuristics** = rules of thumb.
- **self-sustaining** = self-reinforcing loop.
- **unbounded** = without limit.

---

> "There have been initial approaches attempting to mimic or use RL by iterating over attempts and updating policies, such as RISE, which includes additional introspection steps to continuously improve outputs. More recent approaches, like R-ZERO and TTRL, generate their own training data and use RL to continuously self-improve and evolve as it critiques and improves its own output. Let's explore these methods to get an understanding of how RL can be used for continuous self-improvement."

**Explanation:** Earlier: RISE (introspection). Recent: R-Zero and TTRL — generate own data + RL.

**Technical terms:**
- **RISE** = Recursive Introspection — introspection steps to improve outputs.
- **introspection** = self-examination of outputs.

---

### Test-Time Reinforcement Learning (TTRL)

> "Labeled data for agentic traces that generalize well to other experiences is hard to create and generally a costly task, as it requires significant manual labor. This motivates the need for LLMs to learn by themselves rather than being directed purely by labeled data. The idea of self-play and self-experience is central in RL but often requires supervised data (such as query/reasoning/answer triplets) that are difficult to collect for complex and large-scale real-world tasks. This poses a substantial barrier to the self-improvement of LLMs."

**Explanation:** Labeled agentic data is costly. RL self-play usually needs supervised triplets (query/reasoning/answer) that are hard to collect — a barrier TTRL addresses.

**Word meanings:**
- **substantial barrier** = major obstacle.
- **self-play** = learning by playing against itself.

---

> "Test-Time Reinforcement Learning (TTRL) is a framework that proposes to update models at test-time using RL. Instead of having models remain static entities during inference, TTRL allows LLMs to learn as they interact with their environment without the need for supervised data. Instead of learning through their memory modules, their parameters are updated using RL."

**Explanation:** TTRL updates the model's *parameters* during inference via RL — no supervised data, no static model.

**Technical terms:**
- **test-time RL** = updating model parameters at inference time using RL.

---

> "TTRL is a four-step process. First, given a query, it applies repeated sampling to generate several candidate outputs. The authors generated 16 responses per query using a temperature of 0.6 to make sure that the outputs differ sufficiently. In their experiments, they used models from several model families, including Qwen (e.g., Qwen2.5-7B), LLaMA (e.g., LLaMA-3.1-8B-Instruct), Mistral (e.g., Mistral-8b-Instruct-2410), and DeepSeek (e.g., DeepSeek-R1-LLaMa-8B)."

**Explanation:** Step 1: sample 16 outputs per query (temperature 0.6) across Qwen, LLaMA, Mistral, DeepSeek families.

**Technical terms:**
- **temperature** = sampling randomness; higher = more diverse.

---

> "Second, a majority voting strategy is used to select the best answer among the 16 generated outputs. Third, a reward is generated based on the alignment between the voted output and the 16 generated outputs. Finally, the calculated rewards are used as a signal during RL (GRPO) to improve the model as it acts with its environment."

**Explanation:** Steps 2–4: majority vote → reward from alignment → GRPO update.

**Technical terms:**
- **majority voting** = the most frequent answer among samples wins.
- **GRPO** = Group Relative Policy Optimization — the RL update algorithm.

---

> "As you might have noticed, we already covered each of these steps in some form over the previous chapters. Majority voting is a common strategy for scaling test-time compute and improving the outputs, whereas GRPO has been a popular strategy for supervised RL. Elegantly combining these techniques allows TTRL to learn as it interacts with its environment without the need for labeled data since it produces those itself (the calculated rewards)."

**Explanation:** TTRL = combination of known pieces (majority voting + GRPO), producing its own rewards — no labels.

---

> "However, majority voting strategies are not flawless by definition. The majority does not always need to be correct. The authors argue that rewards in RL can be vague to a certain extent, as they signal the need for further exploration instead of continuous exploitation, which prevents models from being stuck in local minima. Likewise, even if the majority is incorrect and no correct predictions are made at all, then the reward will still be negative. This is referred to as a 'lucky hit,' and although they were created from an incorrect process, they are still 'correct' rewards."

**Explanation:** Majority can be wrong. Vague rewards signal exploration (preventing local minima). **"Lucky hit"**: when the majority-voted label is wrong, a sample that disagrees with the wrong label still receives the correct reward — valid even though the majority-vote process was incorrect. The correct signal comes from an incorrect process, not from a correct minority answer.

**Word meanings:**
- **flawless** = without defects.
- **vague** = imprecise.

**Technical terms:**
- **lucky hit** = a correct reward signal produced even though the majority-vote process was wrong.

---

> "Due to this elegant technique, TTRL can apply on-the-fly adaptation by fine-tuning the model as it encounters new problems. It is essentially an online approach to RL focused on learning during inference."

**Explanation:** TTRL = online RL — learns during inference as new problems appear.

**Word meanings:**
- **on-the-fly** = in real time.

### R-Zero

> "The idea of self-evolving reasoning LLMs without labeled data was further explored in R-Zero. This technique uses two independent models with distinct roles to coevolve and challenge each other's outputs. These models are initialized from the same base model and take on the roles of a Challenger and a Solver. The authors experimented with models from the Qwen3 family and the Llama-3.1 family."

**Explanation:** R-Zero: two models (Challenger + Solver) initialized from the same base, coevolving by challenging each other. Qwen3 / Llama-3.1.

**Word meanings:**
- **coevolve** = develop together, each pushing the other.
- **distinct roles** = different jobs.

**Technical terms:**
- **Challenger** = generates hard queries.
- **Solver** = solves them.

---

> "The Challenger is tasked with generating synthetic queries that are difficult for the Solver to solve. The Solver generates multiple answers and selects the best one using majority voting, much like is done with TTRL. During this process, the Challenger is trained with RL (GRPO) based on the rewards it receives from the Solver."

**Explanation:** Challenger invents hard queries; Solver answers with majority voting; Challenger trained via GRPO on rewards from the Solver.

---

> "There are several rewards/signals created during this process:
> - Uncertainty reward (a value between 0 and 1): This indicates how certain the Solver is that it has created the correct answer frequently from all of its sampled answers. It is essentially the fraction of the Solver's responses that match the most common answer. This reward aims to guide the Challenger to create difficult but solvable queries.
> - Repetition penalty (a value between 0 and 1): This penalty makes sure that the Challenger generates diverse queries. Otherwise, it would be stuck continuously optimizing the same difficult queries.
> - Format reward (either 0 or 1): A reward given based on whether the Challenger generates queries between <question> and </question> tags.
> - Composite reward (a value between 0 and 1): A reward given to the Challenger by subtracting the repetition penalty from the uncertainty reward and making sure it does not go below 0."

**Explanation:** Four Challenger signals: uncertainty (fraction of Solver answers matching the majority), repetition penalty (diversity), format reward (0/1 for `<question>` tags), composite (uncertainty − repetition, min 0).

**Technical terms:**
- **uncertainty reward** = how often the Solver's answers agree with the majority (0–1).
- **repetition penalty** = penalty for generating similar queries.
- **composite reward** = uncertainty − repetition penalty, floor 0.

---

> "During this process, the Challenger is trained while the Solver is frozen and merely used as a reward model when fine-tuning the Challenger. This process is illustrated in Figure 6-23."

**Explanation:** While training the Challenger, the Solver is frozen (acts as a reward model).

---

> "After training the Challenger, a dataset of queries is sampled from the Challenger to train the Solver. For each query, answers are sampled by passing them through the Solver. Based on a majority vote, if the query is too difficult (too few correct answers) or if the query is too easy (almost all answers are correct), then they are filtered out. This process improves the quality of the training data by focusing on queries that are just difficult enough."

**Explanation:** Solver training data = Challenger queries filtered to "just difficult enough" (remove too-easy and too-hard).

**Word meanings:**
- **curated** = carefully selected.

---

> "The Solver is then fine-tuned on the curated dataset of challenging queries. As before, GRPO is used for training. Compared to the Challenger, the reward is simplified to a value of either 0 or 1. Like TTRL, this verifiable reward for each correct answer and 0 otherwise. As shown in Figure 6-24, this is a much simpler process and akin to TTRL."

**Explanation:** Solver reward = binary (1 correct, 0 otherwise) — verifiable, like TTRL.

**Word meanings:**
- **akin to** = similar to.

---

> "Together, this creates a coevolving system where the Challenger continuously makes harder queries, and the Solver gets better at solving them. In turn, the Challenger adapts to make new challenging queries, and so on. The beauty of this system is that the Challenger gets continuous rewards (composite uncertainty scores from 0 to 1) to encourage generating queries at the edge of difficulty, while the Solver gets a binary reward (0 or 1) to encourage getting the answer exactly right. This asymmetry allows the Challenger to explore a range of difficulties, while the Solver should be precise and deterministic in its answers."

**Explanation:** The reward asymmetry: Challenger gets continuous rewards (explore difficulty range), Solver gets binary rewards (be exact). Coevolution loop.

**Word meanings:**
- **asymmetry** = difference between the two sides.
- **deterministic** = consistent, precise.

---

> "Methods such as TTRL and R-Zero are, at the time of writing, newer techniques that have tremendous potential for self-improving agents. These are potential paradigm shifts that move the training focus toward unlabeled data and inference rather than purely focusing on the traditional pre-training combined with SFT and RL. The evolution from 'traditional' training with SFT and RL all the way to this potential test-time training paradigm is shown in Figure 6-26."

**Explanation:** TTRL/R-Zero = paradigm shift: training moves toward unlabeled data and inference-time learning (test-time training).

**Technical terms:**
- **paradigm shift** = a fundamental change in approach.
- **test-time training** = learning during inference rather than only during pre-training.

---

## 6.6 Summary

> "In this chapter, we explored how LLMs can plan out behavior and reflect on actions taken when executing this plan. We covered how task decomposition allows LLMs to break down complex problems into small problems that each can be solved on its own. CoT-like techniques were explored and demonstrated how they can be used to perform explicit planning. We then explored how, after planning, agents can perform sequences of actions through a widely-used technique, Reason and Act. Several techniques were explored from the domains of prompting, SFT, and RL to enable this behavior of action sequencing in agents. ReAct is the final missing link in what makes agents potentially autonomous, as it decides on its own when to stop and how long to continue within these ReAct-like frameworks."

**Explanation:** Recap: task decomposition, CoT-like planning, action sequencing via ReAct (prompt/SFT/RL). ReAct decides when to stop/continue — the "final missing link" for autonomy.

**Word meanings:**
- **missing link** = the crucial missing piece.

---

> "We then discussed how agents can be taken a step further and reflect on their sequences of actions to get into a mode of self-improvement. In that, reflection is vital for LLMs and agents to prevent being stuck in local minima of solutions and thought processes. We finished this chapter with an exciting new paradigm of agents that continuously improve as they interact with their environment. This test-time training paradigm has the potential to bring agents into a new realm of performance and generalization."

**Explanation:** Recap: reflection prevents local minima; test-time training paradigm promises new performance/generalization.

---

> "In the next chapter, we'll cover various methodologies of evaluating agents. Due to their potentially complex behaviors, agents are significantly more difficult to evaluate than LLMs. We'll explore what to look for when evaluating your agent and agentic system."

**Explanation:** Bridge to Ch 7: evaluating agents is harder than evaluating LLMs.
