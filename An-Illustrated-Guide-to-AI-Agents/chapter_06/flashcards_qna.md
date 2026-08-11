# Flashcards & Q&A — Chapter 6
**Source:** *An Illustrated Guide to AI Agents*, Chapter 6 "Planning and Reflection"
**Note:** Split from the combined Ch 5 & 6 deck. Original question numbers are preserved.

## Part 2: Term → Definition (Ch 6)

71. **Task decomposition** → Splitting an initial query into smaller subtasks to simplify complicated tasks.
72. **How does CoT act as task decomposition?** → The step-by-step process breaks the problem into sequential reasoning substeps that build on each other.
73. **Self-consistency** → Sampling several CoT paths and selecting the answer the majority agree on.
74. **Tree of Thoughts (ToT)** → Exploring multiple branching reasoning paths (each node = a thought/subtask), pruning dead ends; can use Beam Search or Monte Carlo Tree Search with reward models.
75. **Least-to-Most (LtM) prompting** → Two stages: 1) problem reduction (decompose into subproblems), 2) solve sequentially, appending each previous subquestion and answer into the next prompt.
76. **How does LtM differ from CoT?** → LtM explicitly separates planning from execution and feeds previous answers back into subsequent prompts.
77. **Plan-and-Solve prompting** → A zero-shot extension of CoT: the LLM devises a plan first, then solves the problem step-by-step.
78. **The two steps of plan-and-solve** → 1) Prompting for reasoning generation (plan + solve), 2) answer extraction (process the output into the final answer).
79. **Action sequencing** → Determining a sequence of actions that transitions the agent from its current state to the desired goal state, after deciding (sub)goals.
80. **Why is action sequencing needed beyond LtM/plan-and-solve?** → Agents must sequence actions autonomously and continuously, interleaving plan and action, not just decompose once.
81. **ReAct (Reason and Act)** → A framework that interleaves reasoning traces and task-specific actions into an iterative thought-action-observation loop.
82. **The three components of a ReAct step** → Thought (reasoning about the situation), Action (actions to execute, e.g., tools), Observation (reasoning about the action's result).
83. **Why was ReAct a breakthrough?** → It fused reasoning (CoT) with acting (tools), creating the first truly autonomous agent systems.
84. **How is ReAct typically enabled?** → Prompting with few-shot examples of THOUGHT/ACTION/OBSERVATION cycles.
85. **`max_steps` in ReAct** → The maximum number of steps an agent may take before being forced to stop (prevents infinite loops).
86. **How does a ReAct agent complete a task?** → It must call the `final_answer` tool with its answer — the only way to finish, otherwise it loops forever.
87. **The `ReAct.parse` function** → Regex-extracts THOUGHT and ACTION from the response into `Response.reasoning` and `Response.content`.
88. **What is the autonomy loop in TinyAgent?** → A for-loop that iteratively calls the LLM until it decides to stop (final answer or no tool call).
89. **The four components of a complete agent** → Reasoning LLM (brain), Tools (environment interaction), Memory (prevents forgetting), Planning (task decomposition + ReAct-like frameworks).
90. **FireAct** → One of the first methods to fine-tune an LLM (Llama 2) on ReAct trajectories instead of using prompts.
91. **How did FireAct generate training data?** → GPT-4 generated different trajectory types (CoT, ReAct, Reflexion) from datasets; all converted to the same ReAct format.
92. **How is CoT converted into a ReAct trajectory in FireAct?** → Into a one-round ReAct: thought = intermediate reasoning, action = returns the answer, no observation.
93. **LoRA (Low-Rank Adaptation)** → An efficient fine-tuning method that updates only a small part of the model.
94. **FireAct's main result** → Fine-tuning on trajectories outperformed prompt-based ReAct and removed the need for few-shot examples.
95. **ETO (Exploration-based Trajectory Optimization)** → A two-step approach: SFT on ReAct data (behavior cloning) then RL (DPO) on exploration-generated trajectory pairs.
96. **ALFWorld** → A text-based environment mimicking typical households where agents perform tasks like "clean some tomato and put it on the countertop."
97. **Behavior cloning / imitation learning** → SFT that makes the LLM mimic the behaviors shown in training data.
98. **DPO (Direct Preference Optimization)** → An RL algorithm trained on preference pairs; ETO increases likelihood of successful trajectories and decreases failed ones.
99. **ETO's exploration phase** → The base agent interacts with the environment; failed trajectories are sampled and paired with correct ones.
100. **Native ReAct** → ReAct done natively: `NativeReAct` has an empty prompt and no parsing — the LLM was trained to reason, call tools, and stop itself.
101. **With native models, what replaces THOUGHT/ACTION/OBSERVATION?** → `Response.reasoning`, `Response.tool_call`, and tool output via the `tool` role.
102. **Reflection** → An internal process where the LLM is critical of its own output and processes (vs environment feedback per action).
103. **Self-Refine** → A prompting framework where the LLM acts as its own editor: generate → critique (feedback) → refine, repeating until a stopping criterion.
104. **Reflexion** → A prompting framework where agents verbally reflect on previous tasks using three LLMs.
105. **The three LLMs of Reflexion** → Actor LLM (executes actions via ReAct), Evaluator LLM (produces a scalar reward for trajectory quality), Self-Reflection LLM (generates nuanced feedback from the full trajectory).
106. **CRITIC** → A technique where the LLM revises its initial output using external tools (e.g., search) for more information.
107. **What is the limitation of prompt-based reflection techniques?** → They only guide behavior; they don't instill it into the model's parameters.
108. **TTRL (Test-Time Reinforcement Learning)** → Updating model parameters with RL at test time, learning as the model interacts, without supervised data.
109. **The four steps of TTRL** → 1) Repeated sampling (16 outputs, temp 0.6), 2) majority voting to pick the best, 3) generate a reward from alignment between the vote and outputs, 4) use rewards in GRPO to improve the model.
110. **"Lucky hit" in TTRL** → A correct per-sample reward signal produced even though the majority-voted (estimated) label is incorrect — correct rewards can come from an incorrect process.
111. **Why can vague rewards be useful?** → They signal the need for further exploration rather than continuous exploitation, preventing local minima.
112. **R-Zero** → A self-evolving reasoning LLM system with two coevolving models (Challenger and Solver) initialized from the same base model.
113. **The Challenger's job** → Generate synthetic queries that are difficult for the Solver to solve.
114. **The Solver's job** → Generate multiple answers and select the best via majority voting (like TTRL).
115. **The four rewards/signals for the Challenger** → Uncertainty reward (0–1), repetition penalty (0–1), format reward (0/1), composite reward = uncertainty − repetition penalty (min 0).
116. **The Solver's reward in R-Zero** → Simplified: 0/1 verifiable reward (correct or not).
117. **How is the Solver trained in R-Zero?** → Queries sampled from the Challenger are filtered (too easy or too hard removed) and used to fine-tune the Solver with GRPO.
118. **Why the reward asymmetry in R-Zero?** → The Challenger explores a range of difficulties (continuous rewards); the Solver must be precise and deterministic (binary reward).
119. **Test-time training paradigm** → Moving the training focus toward unlabeled data and inference, scaling compute during inference rather than only pre-training + SFT + RL.
120. **Local minimum (in agent context)** → A suboptimal solution or behavior the agent gets stuck in; feedback loops help avoid it.

## Part 3: Short Answer (Ch 6)

136. **Explain task decomposition and give the feature-creation example.** → Splitting a query into subtasks; e.g., create a feature = clarify requirements → analyze codebase → design → implement → test → update docs.
137. **Compare CoT, self-consistency, and Tree of Thoughts.** → CoT = one linear trace; self-consistency = several traces, majority-vote the answer; ToT = branching paths with pruning (Beam/MCTS).
138. **How does Least-to-Most differ from plan-and-solve?** → LtM decomposes then solves subproblems sequentially, feeding previous answers forward (few-shot). Plan-and-solve makes a plan and solves in one call (zero-shot), then extracts the answer in a second call.
139. **Why is action sequencing harder than prompting decomposition?** → Agents must autonomously choose and sequence actions one after another, continuously updating the plan based on observations — an interleaved loop, not a one-shot decomposition.
140. **Write the ReAct loop conceptually.** → Repeat: Thought (reason) → Action (tool call) → Observation (result) until the agent calls `final_answer`.
141. **Why does ReAct need memory?** → Agents can run hundreds of steps; they must remember previous steps and outcomes, so memory and context engineering (Ch. 4) become vital.
142. **Explain how a for-loop creates autonomy.** → Iteratively call the LLM; each step may call a tool and get an observation; the loop ends when the model signals completion (no tool call or `final_answer`).
143. **What did FireAct prove and what problem did it solve?** → Fine-tuning a small LLM on ReAct trajectories beats prompt-based ReAct and eliminates few-shot examples — solving the fragility/context cost of prompting.
144. **Explain ETO's two phases.** → Phase 1: SFT on successful ReAct trajectories (behavior cloning) to create a base agent. Phase 2: alternate exploration (sample failed trajectories) and training (DPO on failed/success pairs) to prefer successful trajectories.
145. **What is DPO and how does ETO use it?** → DPO = Direct Preference Optimization, RL from preference pairs; ETO uses failed vs successful trajectory pairs to increase success likelihood and decrease failure likelihood.
146. **How does native ReAct remove prompting?** → `NativeReAct` has an empty prompt and passthrough parse; the model was trained (SFT+RL) to reason, call tools, and stop, so only `max_steps` is needed.
147. **Contrast environment feedback vs reflection.** → Environment feedback is per-action ("do X → get Y"); reflection is internal, where the LLM critiques its own output and processes (can also combine with external feedback).
148. **Describe Self-Refine's loop.** → Generate initial answer → feedback (critique) → refine → repeat until stopping criterion (max steps or LLM-guided).
149. **Describe Reflexion's three-LLM architecture.** → Actor LLM runs ReAct actions; Evaluator LLM scores the trajectory (scalar reward); Self-Reflection LLM produces nuanced feedback from the full trajectory; loop until the Evaluator says correct.
150. **What is TTRL and why is it "test-time"?** → RL that updates model parameters during inference (test time) instead of during training, enabling on-the-fly learning without labeled data.
151. **Walk through TTRL's four steps with numbers.** → Sample 16 outputs (temperature 0.6) → majority-vote the best → compute reward from vote-output alignment → update via GRPO.
152. **Explain the Challenger/Solver dynamic in R-Zero.** → Challenger generates hard queries, trained via GRPO with composite reward; Solver (frozen during Challenger training) solves via majority voting; then Solver is fine-tuned on filtered "just right" queries with a binary reward. They coevolve.
153. **What filters queries in R-Zero's Solver training?** → Queries that are too difficult (too few correct answers) or too easy (almost all correct) are removed by majority-vote filtering.
154. **Why do TTRL/R-Zero represent a paradigm shift?** → They move training toward unlabeled data and inference-time learning, scaling compute during inference rather than relying only on pre-training + SFT + RL.

## Part 4: Fill-in-the-Blank (Ch 6)

166. "The autonomy loop in TinyAgent is **a for-loop**!"
167. "ReAct stands for **Reason and Act**."
168. "The three components of a ReAct step are **Thought**, **Action**, and **Observation**."
169. "**FireAct** fine-tunes an LLM on ReAct trajectories."
170. "**ETO** uses **SFT then DPO** to optimize ReAct trajectories."
171. "**Self-Refine** lets an LLM act as its **own editor**."
172. "**Reflexion** uses an Actor LLM, an Evaluator LLM, and a **Self-Reflection LLM**."
173. "**TTRL** stands for **Test-Time Reinforcement Learning**."
174. "In TTRL, **majority voting** selects the best answer among the sampled outputs."
175. "**R-Zero** uses two coevolving models: a **Challenger** and a **Solver**."
