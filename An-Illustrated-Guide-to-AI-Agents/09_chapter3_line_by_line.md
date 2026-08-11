# Chapter 3 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 3 "Reasoning Large Language Models"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings and figure captions are paraphrased; every substantive paragraph is annotated.

---

## 3.1 Opening: What Reasoning LLMs Are

> "LLMs such as DeepSeek-R1, OpenAI GPT-5, and Google Gemini are prime examples of how LLMs can be scaled to new heights through reasoning frameworks. Reasoning in LLMs attempts to mimic human thinking by generating thoughts through tokens before giving a final answer. ... these tokens explain LLMs' chain-of-thought and allow reasoning LLMs to break down a problem into smaller steps (often called reasoning steps or thought processes)."

**Explanation:** Reasoning LLMs generate intermediate "thinking" tokens before the answer, mimicking human thought. Those tokens form a chain-of-thought — a visible trail of smaller steps. The author says the *way* we scale LLMs forward now is through reasoning.

**Word meanings:**
- **prime examples** = the best, most representative examples.
- **scaled to new heights** = pushed to higher levels of capability.
- **mimic** = imitate, copy.
- **break down** = split into smaller parts.

**Technical terms:**
- **DeepSeek-R1 / OpenAI GPT-5 / Google Gemini** = named frontier reasoning LLMs.
- **tokens** = the small units of text a model reads/writes.
- **chain-of-thought (CoT)** = a step-by-step reasoning trace the model generates before answering.
- **reasoning steps / thought processes** = the individual intermediate steps in that trace.

---

> "Reasoning LLMs first 'think' and generate intermediate information before finally answering the query."

**Explanation:** One sentence summary: input → think → answer.

**Word meanings:**
- **intermediate** = in the middle, between start and end.

**Technical terms:**
- (None new.)

---

> "Interestingly, the differences between non-reasoning and reasoning LLMs can be viewed through the lens of human cognition, specifically System 1 and System 2 thinking from Daniel Kahneman's dual-process theory, which describes two modes of human cognition."

**Explanation:** Kahneman's theory (book *Thinking, Fast and Slow*) says humans have two modes of thought. The author maps these onto LLMs as a way to understand the difference.

**Word meanings:**
- **through the lens of** = viewed using (that framework) as a filter.
- **cognition** = thinking, mental processing.
- **dual-process theory** = the theory that there are two distinct thinking systems.

**Technical terms:**
- **System 1 / System 2** = Kahneman's two modes of human thought: automatic/fast vs deliberate/slow.
- **Daniel Kahneman** = Nobel-winning psychologist who popularized this idea.

---

> "System 1, in human cognition, operates automatically and quickly, relying on intuition and learned associations to make snap judgments. Non-reasoning LLMs operate much like System 1 thinking and generate responses quickly based on patterns they learned during training. They produce immediate answers without step-by-step (explicit) thinking."

**Explanation:** System 1 is fast, automatic, instinctive. Non-reasoning LLMs are analogous: they instantly output the most likely answer from learned patterns, with no explicit step-by-step thinking.

**Word meanings:**
- **automatic** = without conscious effort.
- **intuition** = gut feeling, instinct.
- **snap judgments** = instant decisions.
- **explicit** = made visible/clear, stated openly.

**Technical terms:**
- **learned associations / patterns** = statistical connections the model absorbed during pre-training.

---

> "System 2, in human cognition, engages in slower, more deliberate reasoning that requires conscious effort and attention. Reasoning LLMs operate much like System 2 thinking and generate more deliberate step-by-step analyses before reaching conclusions. They explicitly work through problems and catch errors that a non-reasoning LLM might miss."

**Explanation:** System 2 is slow, effortful, logical. Reasoning LLMs work like this: they produce step-by-step analysis and can catch mistakes a quick model would miss.

**Word meanings:**
- **deliberate** = careful and intentional.
- **conscious effort** = effort you are aware of making.

**Technical terms:**
- (None new.)

---

> "System 1 is efficient for routine tasks but is prone to cognitive biases, whereas System 2 relies on logical reasoning and tends to result in more accurate decisions. Reasoning is critical in AI agents because it allows them to plan out behavior, decide which actions to take, and reflect on the actions they have taken. This involves enabling them to actively seek information, utilize diverse tools, and dynamically refine their reasoning."

**Explanation:** System 1 is fast but biased; System 2 is slower but more accurate. For *agents*, reasoning matters because they must plan, choose actions, and reflect — which requires seeking information and using tools.

**Word meanings:**
- **prone to** = likely to suffer from.
- **cognitive biases** = systematic errors in thinking.
- **reflect on** = think back about, evaluate.
- **utilize** = use.
- **dynamically refine** = adjust as you go.

**Technical terms:**
- **planning** = deciding a sequence of actions ahead of time (agent capability, Ch. 6).
- **reflection** = the agent reviewing its own past actions.

---

> "AI agents need reasoning to devise a plan, decide on the actions they take, and reflect on the results. This is especially true for planning out actions and reflecting on them. For instance, in typical AI-assisted coding environments, such as Cursor and Cline, the agents might create special markdown files to track their plan and revise when necessary."

**Explanation:** Concrete example: coding agents like Cursor and Cline write markdown plan files to keep track of what they intend to do, and update them.

**Word meanings:**
- **devise** = create, work out.
- **revise** = change/update.

**Technical terms:**
- **Cursor / Cline** = AI coding assistants that act as agents.
- **markdown files** = lightweight plain-text docs (e.g., a PLAN.md) agents use to persist their plan.

---

> "Before we go into how AI agents showcase autonomous behavior, we first explore how LLMs can demonstrate advanced thinking processes. In Chapters 5 and 6, we explore how reasoning enables tool selection and enables planning and reflection."

**Explanation:** This chapter sets up reasoning; later chapters build tool use (Ch. 5) and planning/reflection (Ch. 6) on top.

**Word meanings:**
- **autonomous** = acting on its own.

**Technical terms:**
- (None new.)

---

> "In this chapter, we uncover the history of reasoning LLMs through an important paradigm shift from training to inference and explore various methods for creating reasoning LLMs. We will also introduce coding examples for enabling and improving reasoning behavior for the section 'Search Against Verifiers'. Note that there will be no changes to the TinyAgent because this chapter demonstrates reasoning behavior."

**Explanation:** Roadmap: (1) the training→inference paradigm shift, (2) methods to build reasoning, (3) code examples. TinyAgent itself is unchanged this chapter.

**Word meanings:**
- **uncover** = reveal, bring to light.
- **paradigm shift** = a fundamental change in approach.

**Technical terms:**
- **inference** = using a trained model to produce outputs.
- **TinyAgent** = the educational agent built in Ch. 2.

> **NOTE (book):** Many terms appear abbreviated; the book spells them out with acronyms in parentheses at first use, and lists key terms in the repo.

---

## 3.2 The Paradigm Shift from Train-Time Compute to Test-Time Compute

> "Before we explore reasoning LLMs in more detail, it's important to discover why there is such a big focus on them currently. This relates to a fundamental shift that has changed our focus from scaling train-time compute to test-time compute."

**Explanation:** Reasoning models became the focus because the field switched where it spends compute: from training to inference.

**Word meanings:**
- **fundamental shift** = basic, deep change in direction.

**Technical terms:**
- **scaling** = increasing resources to improve performance.
- **train-time compute** = computational resources spent during training.
- **test-time compute** = computational resources spent during inference (generating answers).

### Train-Time Compute Versus Test-Time Compute

> "To improve the performance of LLMs during pre-training, developers used to focus on three main factors: Increasing the model size (number of parameters); Increasing the dataset size (number of tokens); Increasing compute (number of floating point operations per second [FLOPs]). This combination of factors, called train-time compute, has been a key strategy. The basic principle is that more pre-training resources lead to a better, more powerful model."

**Explanation:** Three levers: model size (parameters), data (tokens), and raw compute (FLOPs). "More of all three = better model" was the strategy.

**Word meanings:**
- **combination** = the three factors taken together.

**Technical terms:**
- **parameters** = the learned numbers inside a model that define it.
- **tokens** = units of text in the training dataset.
- **FLOPs** = floating-point operations per second — a measure of compute.
- **pre-training** = the initial, huge-scale training phase on massive text.

---

> "When we explore train-time compute, it does not only relate to pre-training but also what you can tweak during post-training. ... When you fine-tune a model, you can often decide how many of its parameters you would like to update. In a way, even during post-training, you can increase the 'model size.'"

**Explanation:** Train-time compute also covers post-training/fine-tuning — you can even choose how many parameters to update.

**Word meanings:**
- **tweak** = make small adjustments.

**Technical terms:**
- **post-training** = training done after pre-training (fine-tuning, RLHF, etc.).
- **fine-tune** = continue training a pre-trained model on more specific data.

---

> "Train-time compute means a focus on increasing pre-training and post-training resources, and little attention is spent on inference. This generally applies to non-reasoning LLMs."

**Explanation:** Non-reasoning LLMs spend nearly all effort on training, almost none on inference.

**Technical terms:**
- **non-reasoning LLMs** = models that answer directly without explicit step-by-step thought.

---

> "In contrast, test-time compute focuses more on scaling inference instead of pre-training and post-training. Scaling inference generally applies to reasoning LLMs and allows them to 'think longer' during inference."

**Explanation:** The new approach spends more compute at answer time — letting the model "think longer."

**Word meanings:**
- **in contrast** = by comparison.

---

> "Let's illustrate this with an example. Figure 3-7 shows how non-reasoning LLMs directly generate an answer without any intermediate generated tokens. To the question, 'What is 3+2?', it answers '5'. All of its compute is used to generate this one token."

**Explanation:** Concrete: "3+2" → "5". One token, no intermediate thinking.

**Word meanings:**
- (None new.)

**Technical terms:**
- (None new.)

---

> "In this example, scaling inference means generating more tokens in the output so that the model has spent more resources getting to the final answers. Simplified, if one token is one 'compute,' then dozens of tokens would mean dozens of 'compute.' 'Compute' can mean several things, but it generally refers to how much computational work the model performs during inference."

**Explanation:** Each generated token costs compute, so generating more tokens = more compute spent. "Compute" here = the work done during inference.

**Word meanings:**
- **simplified** = put in the simplest terms.

---

> "This is especially true for reasoning models, as they would use more tokens (which we can think of as 'thinking' tokens) to derive their answer."

**Explanation:** Reasoning models consume many "thinking" tokens to reach an answer.

**Technical terms:**
- **thinking tokens** = the intermediate tokens used for reasoning.

---

> "The underlying idea is that the more tokens you generate, the better the resulting performance will be. However, you cannot just produce many random tokens to get a better output. Instead, tokens need to be generated that, for instance, contain additional relationships and new thoughts about the original problem. In other words, it is giving the reasoning LLM more time to 'think.'"

**Explanation:** More tokens help only if they add useful reasoning, not random noise. The point is giving the model time to think.

**Word meanings:**
- **underlying** = the basic idea underneath.
- **random** = without pattern or purpose.

---

> "To sum up, train-time compute focuses on scaling resources (e.g., data and compute) spent during pre-training and post-training, while test-time compute focuses on scaling resources (e.g., compute) spent during inference."

**Explanation:** Summary sentence contrasting the two.

**Word meanings:**
- **to sum up** = in conclusion.

---

> "The question remains: 'Why has there been a paradigm shift from scaling train-time compute to test-time compute?' Let's look at scaling laws to answer this question."

**Explanation:** Transition to the evidence: scaling laws explain why training-only scaling hit a wall.

**Technical terms:**
- **scaling laws** = empirical formulas relating model scale to performance.

### Scaling Laws

> "The relationship between a model's scale (e.g., compute, dataset size, and parameter counts) and its performance is described by scaling laws. They often take the form of power laws, where increasing one variable (e.g., compute) leads to a proportional change in another (e.g., performance)."

**Explanation:** Performance scales with size, and the relationship is a power law (a specific mathematical form).

**Word meanings:**
- **proportional** = changing in a corresponding ratio.

**Technical terms:**
- **power laws** = relationships of the form y ∝ x^k; performance grows as a power of compute.

---

> "In these power laws, these relationships tend to follow diminishing returns: each doubling of compute gives smaller gains than the previous doubling. This describes a logarithmic relationship."

**Explanation:** Each doubling of compute yields less improvement than the last — diminishing returns (logarithmic).

**Word meanings:**
- **diminishing returns** = each extra unit of effort produces less benefit.

**Technical terms:**
- **logarithmic relationship** = growth that slows down over time; e.g., log(compute).

---

> "When plotted on regular axes (log-linear scale), this creates a curve that flattens out. But when both axes are put on a log-log scale, the power-law relationship becomes a straight line, making trends easier to see and compare across many orders of magnitude."

**Explanation:** Log-log plots turn a power law into a straight line, making trends visible.

**Word meanings:**
- **flattens out** = levels off, stops growing fast.
- **orders of magnitude** = factors of ten.

**Technical terms:**
- **log-log scale** = both axes plotted logarithmically.

---

> "The most influential scaling laws in train-time compute are the Kaplan and Chinchilla scaling laws."

**Explanation:** Names the two key papers.

**Technical terms:**
- **Kaplan Scaling Law** = 2020 paper by Jared Kaplan et al.
- **Chinchilla Scaling Law** = 2022 paper by Hoffmann et al.

---

> "Jared Kaplan found that a model's performance improves predictably as you increase compute, dataset size, and parameters, but with diminishing returns. It shows the same logarithmic relationship described by power laws. For a fixed compute budget, the best strategy was to keep increasing the model size and train on as much data as possible without overfitting."

**Explanation:** Kaplan: performance improves predictably but with diminishing returns. For fixed compute: bigger model + as much data as possible, avoiding overfitting.

**Word meanings:**
- **predictably** = in a consistent, foreseeable way.
- **fixed compute budget** = a set amount of compute you cannot exceed.

**Technical terms:**
- **overfitting** = the model memorizes training data instead of generalizing.

---

> "Following Kaplan's finding, the Chinchilla Scaling Law demonstrated similar findings and reinforced these logarithmic relationships. However, they showed that models were often undertrained and that for a fixed compute budget, it's better to use a smaller model and train it on much more data."

**Explanation:** Chinchilla's twist: models were undertrained; for fixed compute, prefer a *smaller* model with *more* data.

**Word meanings:**
- **reinforced** = strengthened/supported.
- **undertrained** = not trained on enough data for their size.

---

> "Both laws suggest that all three factors (compute, data, and model size) should be scaled up in tandem for optimal performance."

**Explanation:** Scale all three together, not one at a time.

**Word meanings:**
- **in tandem** = together, in coordination.

---

> "However, these power laws state that at some point there will be diminishing returns, which is what the field of LLM research started to see throughout 2024. Although compute, data, and model sizes have steadily grown, the gains have not grown linearly with them. ... we had likely reached a limit. Note that this assumes that there are no major architectural improvements to the models."

**Explanation:** By 2024, training-only scaling plateaued: resources grew but gains didn't keep pace.

**Word meanings:**
- **steadily** = continuously.
- **linearly** = in direct proportion.
- **plateaued** (implied) = reached a flat ceiling.

**Technical terms:**
- **architectural improvements** = changes to the model's structure (as opposed to its scale).

---

> "This meant that researchers had to look elsewhere. Unsurprisingly, test-time compute turned out to be a prime candidate to continue scaling LLMs."

**Explanation:** Since training scaling plateaued, test-time compute became the next lever.

**Word meanings:**
- **prime candidate** = the most promising option.

---

> "First, a post by OpenAI detailed that increasing test-time compute might affect performance the same as increasing train-time compute. They defined the train-time compute as more reinforcement learning (RL), and the test-time compute as more time spent thinking."

**Explanation:** OpenAI's post: more RL (train-time) and more thinking time (test-time) had similar performance effects.

**Technical terms:**
- **reinforcement learning (RL)** = training where a model learns from reward signals.

---

> "Second, an interesting paper titled 'Scaling Scaling Laws with Board Games' by Andy L. Jones explores AlphaZero and trains it to various degrees of compute to play a board game called Hex."

**Explanation:** Jones's paper used AlphaZero on the game Hex to study scaling both compute types.

**Technical terms:**
- **AlphaZero** = a famous RL system that mastered chess/shogi/Go.
- **Hex** = a two-player strategy game on a hexagonal grid.

---

> "Hex is a two-player board game in which players take turns placing stones on a hexagonal grid to form a path between opposite sides of the board before the opponent does. AlphaZero is a deep neural network that uses a tree-based search method to consider the moves it can take in this game of Hex."

**Explanation:** Background on the game and the model.

**Word meanings:**
- **opponent** = the other player.

**Technical terms:**
- **tree-based search** = exploring possible moves in a branching structure (like a game tree).

---

> "In Jones' research, train-time compute was identified as increasing the number of parameters in their model and training for more epochs. In contrast, test-time compute was defined as considering more solutions by scaling the depth of their tree search."

**Explanation:** Operational definitions: more params/epochs = train-time; deeper tree search = test-time.

**Technical terms:**
- **epochs** = full passes over the training data.
- **tree search depth** = how many moves ahead the search looks.

---

> "Their results showed that both forms of compute are tightly related. As shown in the annotated Figure 3-16, each dotted line represents the minimum compute needed to reach a given Elo score. ... for a target Elo score, train-time compute and test-time compute should be kept in balance. To maintain the same score, a decrease in one form of compute should be offset by an increase in the other. Likewise, for the best performance, both forms of compute should be increased together."

**Explanation:** Key finding: train-time and test-time compute are interchangeable up to a point — you can trade one for the other, and both together give best performance.

**Word meanings:**
- **tightly related** = strongly connected.
- **offset by** = compensated by.

**Technical terms:**
- **Elo score** = a rating system (from chess) estimating player/model strength.

---

> "Research in scaling laws for test-time compute all point to the significant benefit of scaling test-time compute. As a result, a paradigm shift happened in 2024 and 2025 toward reasoning models that focus on using more test-time compute. Through this paradigm shift, instead of focusing purely on train-time compute (pre-training and fine-tuning), these reasoning models instead balance training with inference."

**Explanation:** Conclusion: the field shifted in 2024–2025 to models that balance training and inference.

**Word meanings:**
- (None new.)

> **NOTE (book):** Test-time compute ≠ only thinking longer. It means scaling time/compute spent on inference generally — e.g., generating many answers and voting on the best one also scales inference compute and stabilizes output.

---

## 3.3 Categories of Test-Time Compute

> "There are many methods for scaling test-time compute, such as search-based techniques, reward modeling, self-improvement, and more. To keep things manageable, these can also be put into the following two categories that we will use throughout this chapter:
> - **Search against verifiers:** Sampling generations and selecting the best answer
> - **Modifying proposal distribution:** Trained 'thinking' process"

**Explanation:** Two umbrella categories organize all methods.

**Technical terms:**
- **sampling generations** = generating multiple candidate outputs.
- **reward modeling** = scoring outputs with a model.

---

> "Search against verifiers is a set of techniques that revolve around sampling many answers and/or reasoning traces. Selecting the best answer is done through some form of reward model (RM; or verifier), a model that scores the quality of the given answer and/or reasoning trace."

**Explanation:** Search-against-verifiers: generate many candidates, score them with a reward model (verifier), keep the best.

**Word meanings:**
- **revolve around** = are centered on.

**Technical terms:**
- **reward model (RM) / verifier** = a model or system that scores answers/traces.

---

> "Modifying proposal distribution involves tuning or prompting the model in such a way that it outputs improved reasoning steps. The proposal distribution is commonly referred to the token probabilities, which propose the tokens to generate next. This distribution from which tokens are sampled is modified to show more advanced reasoning traces. Compared to search against verifiers, it may also use RMs during training to score more promising reasoning traces."

**Explanation:** Modifying proposal distribution changes *which* tokens the model tends to pick (by training/prompting) so reasoning tokens get chosen more often.

**Word meanings:**
- **promising** = likely to succeed.

**Technical terms:**
- **proposal distribution** = the probability distribution over next tokens from which sampling happens.

---

> "Due to its focus on sampling generations, search against verifiers is output-focused, whereas modifying the proposal distribution is input-focused since it focuses on either training or prompting the model."

**Explanation:** Output-focused (score what was generated) vs input-focused (change the model so it generates better).

**Word meanings:**
- (None new.)

### Reward Models

> "Reward models are used to score the LLM-generated reasoning traces and answers. They can be many things but are generally (fine-tuned) LLMs or rule-based systems. LLMs can be used to judge the reasoning traces, whereas rule-based systems, like unit tests, can be used to test the output."

**Explanation:** Verifiers come in two flavors: LLM judges or rule-based checks (e.g., unit tests, compilers).

**Technical terms:**
- **rule-based systems** = deterministic checks (unit tests, code compilation).

---

> "There are two types of verifiers that we will explore for both methods of test-time compute:
> - **Outcome Reward Model (ORM)**
> - **Process Reward Model (PRM)**"

**Technical terms:**
- **ORM** = scores only the final answer.
- **PRM** = scores the reasoning steps.

---

> "As the name implies, the Outcome Reward Model judges only the outcome of the LLM and ignores any reasoning steps. ... the number of reasoning traces created and their quality is not taken into account when judging the quality of the final answer."

**Explanation:** ORM = final answer only.

**Word meanings:**
- **taken into account** = considered.

---

> "The Process Reward Model tends to focus on the processes that lead to a given outcome. In the context of reasoning models, those processes are the reasoning steps leading to the final answer. ... Note that the final answer is not always taken into account, and that often a mix between a Process Reward Model and Outcome Reward Model might be preferred."

**Explanation:** PRM judges steps, not necessarily the answer; a mix is often best.

**Technical terms:**
- (None new.)

---

> "To make these reasoning steps a bit more explicit, let's use the example we saw at the beginning of this chapter. The question we ask the reasoning LLM is: 'I saw 6 flamingos. 2 flew away. 1 hid behind a tree. How many can I see?' ... How many reasoning steps it has will depend on the specific model because some are more verbose than others."

**Explanation:** Example problem reused; the number of steps varies by model verbosity.

**Word meanings:**
- **verbose** = wordy, using many words.

---

> "Note how each Process Reward Model scores a specific part of these reasoning steps and aggregates the scores. Although it starts out strong, reasoning step 2 makes a mistake and is scored low by the PRM. However, the very next reasoning step corrects its mistakes, which is scored highly by the PRM."

**Explanation:** PRM scores each step; a bad step is caught, but a later step can fix the error. This is the advantage over ORM.

**Word meanings:**
- **aggregates** = combines together.

**Technical terms:**
- (None new.)

---

> "How these scores are then used to judge the quality of the answer or select the best answer depends on the specific category of test-time compute."

**Explanation:** Scores feed into whichever selection strategy you use.

### Prompt Engineering

> "In some of our previous examples, non-reasoning LLMs were assumed to be able to output reasoning traces (thoughts). This was especially true of techniques in search against verifiers that sample many reasoning traces and answers before selecting the best one. So, how are these non-reasoning LLMs able to output reasoning traces? Prompt engineering plays a vital role in this process, with a technique called Chain-of-Thought (CoT). This is one of the first techniques to elicit reasoning in LLMs that were not trained for reasoning tasks."

**Explanation:** Even non-reasoning LLMs can be made to reason via prompting — specifically Chain-of-Thought prompting.

**Word meanings:**
- **elicit** = draw out, provoke.

**Technical terms:**
- **prompt engineering** = crafting prompts to guide model behavior.
- **Chain-of-Thought (CoT) prompting** = asking the model to reason step by step before answering.

---

> "Chain-of-Thought prompting requires the model to explain its reasoning process by specifying that in the prompt. LLMs are quite adept at following examples. By demonstrating the desired reasoning style, it increases the likelihood that the LLM will replicate it. Such demonstrations in a prompt are typically referred to as one-shot prompting or few-shot prompting. One-shot prompting uses a single example to demonstrate a task or targeted behavior while few-shot prompting uses two or more examples, which tends to result in higher accuracy as it helps the model recognize patterns."

**Explanation:** Show the model an example of good reasoning and it will imitate it. One example = one-shot; two+ = few-shot (more accurate).

**Word meanings:**
- **adept at** = skilled at.
- **replicate** = copy, reproduce.
- **likelihood** = probability.

**Technical terms:**
- **one-shot prompting** = one example in the prompt.
- **few-shot prompting** = two or more examples in the prompt.

---

> "We can use the LLM we created in Chapter 2 to demonstrate the behavior. Let's first load in the LLM. Note that we set the `temperature` to 1 as that will allow us to generate different answers that we can use later for sampling. If we were to set it to 0, then the output would always be the same regardless of how many times we run it."

**Explanation:** Code setup; temperature 1 gives varied outputs (useful for sampling), temperature 0 is deterministic.

**Technical terms:**
- **temperature** = a parameter controlling output randomness; higher = more varied.

---

> "After doing so, let's give the LLM a riddle and ask it to be concise in its answer as an example. ... This is a form of few-shot learning where you give examples of what you want the answer to the interaction to be. This gives us: [the wrong answer '3'→ actually model answered]. Note how it got the answer wrong! Without any extensive reasoning, the LLM directly goes to answering the question without properly thinking of this puzzle."

**Explanation:** With a concise-answer example, the model jumps straight to an answer and gets the riddle wrong — no reasoning happened.

**Word meanings:**
- **concise** = brief, short.
- **riddle/puzzle** = a brain-teaser problem.

---

> "Instead of using a concise answer, let's give the LLM a more thorough one that mimics the type of reasoning we would like to see. ... This is the correct answer! Just by giving the model a couple of examples on how to reason, it follows that behavior and allows for a reasoning process."

**Explanation:** Showing a worked-out reasoning example makes the model reason and get it right.

**Word meanings:**
- **thorough** = detailed, complete.
- **mimics** = imitates.

---

> "This process can be further simplified by foregoing any examples of reasoning steps and instead simply stating: 'Let's think step-by-step.' This is a form of zero-shot prompting where no examples are used that has a chance of working with some models."

**Explanation:** Even simpler: just append "Let's think step-by-step" — zero-shot prompting.

**Word meanings:**
- **foregoing** = giving up, doing without.

**Technical terms:**
- **zero-shot prompting** = no examples at all; just an instruction.

> **NOTE (book):** zero-shot tends to perform worse than one-shot, which performs worse than few-shot. Examples help guide the model, though it can still behave similarly without them.

---

> "What makes prompting special in the context of non-reasoning LLMs is that non-reasoning LLMs do have reasoning capabilities hidden somewhere in their parameters. It's a matter of prompting it in an elegant way to extract those capabilities."

**Explanation:** Non-reasoning LLMs *do* have latent reasoning ability — prompting just unlocks it.

**Word meanings:**
- **elegant** = clever and neat.
- **extract** = pull out.

---

> "Let's see if we can reproduce the behavior we explored before by simply adding, 'Let's think step-by-step' to the prompt. ... Also the correct answer! Now instead of having to create a bunch of examples, you can just append 'Let's think step-by-step.' to the prompt."

**Explanation:** Confirms zero-shot CoT works.

---

> "To enable the TinyAgent's reasoning, all we have to do is add that 'thinking' prompt to the task like we did before. We'll use this idea of zero-shot Chain-of-Thought more in Chapter 6, where we will enable reasoning by telling the model to use separate 'THOUGHT' and 'ANSWER' fields."

**Explanation:** Trick applied to TinyAgent; Ch. 6 uses structured THOUGHT/ANSWER fields.

---

> "A test-time compute category that relies heavily on Chain-of-Thought-like prompting is search against verifiers. By sampling many reasoning traces through Chain-of-Thought, much of its instability can be prevented."

**Explanation:** Sampling many CoT traces stabilizes results.

**Word meanings:**
- **instability** = variability/unreliability of single answers.

### Search Against Verifiers

> "Search against verifiers refers to a family of techniques that involve generating multiple candidate answers or reasoning traces and then evaluating them to choose the best answer. The scorer is typically referred to as the reward model (RM), also called the verifier. However, as we will explore in the first example, not all of these techniques have to use a reward model."

**Explanation:** General idea: generate many candidates, evaluate, choose best. Note: not every technique needs a verifier (e.g., self-consistency).

**Word meanings:**
- (None new.)

---

> "Search against verifiers does not necessarily enable reasoning but is more akin to what we expect of non-human behavior. It programmatically goes through many answers and scores the best ones. As such, the techniques that we will explore might remind you of traditional methods, like majority voting or tree-search techniques."

**Explanation:** This is mechanical "try many, score, pick best" — like majority voting or tree search, not human-like reasoning.

**Word meanings:**
- **akin to** = similar to.
- **programmatically** = via an algorithm.

**Technical terms:**
- **majority voting** = pick the most common answer.
- **tree-search** = exploring branching candidate options.

---

> "As seen in Figure 3-23, search against verifiers typically consists of three steps:
> 1. Sampling multiple reasoning processes and/or answers.
> 2. A reward model (verifier) scores the generated output.
> 3. The best answer based on the generated scores is chosen."

**Explanation:** The canonical 3-step pipeline.

---

> "The verifier is typically an LLM, fine-tuned for either judging the outcome (ORM) or the process (PRM). This fine-tuning can be through actual training or by creating a specialized prompt with few-shot examples."

**Explanation:** Verifiers are usually fine-tuned LLMs — either truly trained or prompt-based.

---

> "To sample many different thoughts and answers to a given question, LLMs can use different temperature values. The temperature parameter in LLMs controls the randomness of the model's output by changing the distribution of the model's next token probabilities. Seen in Figure 3-24, low temperatures focus on high-probability tokens and tend to generate similar texts, while high temperatures allow for more creative and different texts to be generated."

**Explanation:** Temperature shapes the next-token distribution; low = deterministic, high = diverse.

**Word meanings:**
- **creative** = here, varied/nonstandard.

---

> "A major advantage of using verifiers is that there is no need to retrain or fine-tune the LLM that you use for answering the question. Likewise, you can easily scale the test-time compute up by sampling more answers or restricting it by sampling fewer."

**Explanation:** Verifiers don't require training; you scale compute by sampling more/fewer answers.

### Self-Consistency

> "One of the first methods for search against verifiers is called self-consistency and actually does not use a reward model or verifier. Instead, it samples a user-defined number of answers and performs a majority vote to select the most frequent answer."

**Explanation:** Self-consistency: sample N answers, majority-vote the most frequent. No verifier needed.

**Word meanings:**
- **self-consistency** = relying on agreement among the model's own answers.

---

> "To generate many different reasoning traces and answers, a high temperature is usually combined with Chain-of-Thought-like prompting. Note that varying values of temperature can also be used to get more randomized behavior. Self-consistency can then easily be scaled by generating more answers, each with reasoning steps."

**Explanation:** High temperature + CoT prompting → diverse traces; scale by adding more samples.

---

> "Let's explore how we would do this in practice. We use a seating puzzle that the model has to reason through. The model is queried 10 times and each time can generate a different reasoning trace and answer. We ask the model to put its answer after 'Answer:' so we can easily separate the answer from the reasoning. The answers are then counted. The most common answer is 5 (which is correct). However, in some runs the model has produced the incorrect answer. By rerunning the model several times, we increase the likelihood that the model gets the answer correct."

**Explanation:** Code demo: 10 runs of a seating puzzle; answers counted with `Counter`; the most frequent (5) is correct.

**Word meanings:**
- (None new.)

**Technical terms:**
- **Counter** = a Python collections tool that counts occurrences.

> **NOTE (book):** Self-consistency selects the most frequent, not necessarily the best, answer. Sampling reduces the chance of picking an infrequent, incorrect answer. But for very complex tasks the LLM seldom gets right, self-consistency is unlikely to help. In the example, the model leans toward the correct answer (5) but sometimes produces too many incorrect ones; with temperature 1 results vary run to run.

### Best-of-N Samples

> "A common and straightforward method for using a verifier is called Best-of-N samples. In this method, the LLM generates N candidate answers, typically using a high or varying temperature to encourage diversity. Then, a verifier evaluates each answer and selects the highest-scoring one."

**Explanation:** Best-of-N: generate N answers, score each with a verifier, keep the top-scoring.

**Technical terms:**
- **Best-of-N samples** = pick the best of N sampled candidates.

---

> "There are two main forms of Best-of-N samples, using:
> - **Outcome Reward Models (ORM):** Only the answers are scored using a verifier (e.g., LLM, unit tests, compiler) and the highest scoring answer is chosen.
> - **Process Reward Models (PRM):** Only the processes (thoughts) are scored using verifiers. When there are multiple reasoning traces for a given question, each trace is scored and averaged. The answer that has traces with the highest scores is selected."

**Explanation:** ORM-Best-of-N scores only answers; PRM-Best-of-N scores and averages each trace, selecting the answer whose traces score highest.

**Technical terms:**
- (None new.)

---

> "Let's go through an example on how we could verify the output of an LLM and choose the best one through sampling. We will ask the model to generate a function that converts Roman numerals (e.g., IV) to integers (e.g., 4). There are many different ways you can verify this output, such as using another LLM, creating tests, or checking whether the code compiles. The choice depends on what you have available. Checking if the code compiles is a great first step but does not check the correctness of the output. Let's assume that we have a couple of test cases."

**Explanation:** Example: generate a `roman_to_int` function; verify via test cases. Compilation checks syntax, not correctness.

**Technical terms:**
- **Roman numerals** = number system using I, V, X, L, C, D, M.

---

> "Using those test cases, we can run a similar loop as we did with the self-consistency example. When you run this, the model creates 10 answers, each of which is judged with the verify function. In our case, this gives the following range of scores: [0.25, ..., 1.0]. Note that we have found only one answer that is correct. Without having a verify function and without sampling, it's unlikely the model would have gotten the correct answer."

**Explanation:** Of 10 generated functions, only one scored 1.0. Sampling + verification rescued the right answer.

**Word meanings:**
- (None new.)

---

> "The great thing about the Best-of-N samples method is that it allows for a wide degree of creativity and flexibility in how you structure the application of these verifiers. For instance, we can also weigh each answer candidate by the Process Reward Model and use that to create an aggregate score for answers that appear multiple times."

**Explanation:** Best-of-N is flexible — e.g., weighted PRM scoring aggregated across repeated answers (weighted Best-of-N).

**Word meanings:**
- **aggregate** = combined total.

### Modifying Proposal Distribution

> "The second category of scaling test-time compute is called modifying proposal distribution and is a popular method of creating reasoning LLMs. Instead of searching for the correct reasoning steps with verifiers (output-focused), the model is trained to demonstrate advanced reasoning steps (input-focused). Remember that both the Process Reward Model and the Outcome Reward Model focus on the output generated by the LLM, the reasoning traces, and the final answer, respectively."

**Explanation:** The second category trains the model itself to reason, rather than searching over its outputs.

---

> "The term 'modifying proposal distribution' refers to the distribution from which tokens are sampled. Imagine that we have a question that we pass to the LLM. As seen in Figure 3-28, it creates a distribution of token probabilities from which we can sample. A common strategy would be to select the token with the highest probability."

**Explanation:** The model produces a probability distribution over next tokens; you sample (often the argmax).

**Word meanings:**
- (None new.)

---

> "However, some of the tokens in this distribution are not a direct answer but instead the start of reasoning behavior. ... if we choose a token that answers the question directly, then it immediately generates a stop token. However, if a token is chosen that is the start of reasoning, then it completes its reasoning until it reaches an answer. As such, you can view them as 'answer' and 'reason' tokens, respectively."

**Explanation:** Some tokens launch an immediate answer (then stop), others launch a reasoning process. "Answer" tokens vs "reason" tokens.

---

> "When we modify the proposal distribution (the token probability distribution), we are essentially making it so that the model re-ranks the distribution such that 'reasoning' tokens are selected more frequently."

**Explanation:** Training reranks probabilities so reasoning tokens are chosen more often.

**Word meanings:**
- **re-ranks** = reorders by priority.

---

> "We can achieve modifying or reranking this distribution by training the LLM. Two common methods of training are:
> - **Supervised fine-tuning (SFT):** Reasoning behavior is mimicked through supervised fine-tuning, where the LLM is asked to reproduce pre-defined reasoning traces. The model learns to approximate the reasoning patterns present in the training data.
> - **Reinforcement learning (RL):** The LLM discovers reasoning itself through a reward system. During training, the LLM discovers good reasoning strategies by being rewarded for generating high-quality responses."

**Explanation:** Two training routes: SFT (copy examples) and RL (discover via rewards).

**Word meanings:**
- **approximate** = get close to.
- **pre-defined** = fixed in advance.

**Technical terms:**
- **SFT** = supervised fine-tuning.
- **RL** = reinforcement learning.

---

> "In other words, supervised fine-tuning stabilizes the model outputs by serving as a memory process to uncover learned reasoning behavior, while reinforcement learning enables self-learning and generalization."

**Explanation:** SFT = stabilize/uncover; RL = self-learn and generalize.

**Word meanings:**
- **generalization** = applying learning to new, unseen cases.

### Supervised Fine-Tuning

> "The most straightforward method for enabling reasoning behavior in LLMs is to apply supervised fine-tuning. Similar to what we saw in Chapter 2, the model is exposed to triplet-like data, which contains the user's query, a reasoning trace, and an answer."

**Explanation:** SFT for reasoning uses (query, reasoning trace, answer) triplets.

**Technical terms:**
- **triplet-like data** = data grouped in threes (query, trace, answer).

---

> "Although using supervised fine-tuning is quite straightforward, collecting these large amounts of reasoning traces is quite difficult, as they generally have to be manually created by potentially labeling hundreds of thousands of samples."

**Explanation:** The bottleneck: creating reasoning traces by hand is expensive.

**Word meanings:**
- **bottleneck** (implied) = the limiting factor.

#### Flan-PaLM

> "An early example of using supervised fine-tuning on Chain-of-Thought-like data was the Flan-PaLM paper. Chung et al.'s main method of fine-tuning is called Flan (Fine-tuning language models; not to be confused with the FLAN models), in which a variety of instruction templates are used on more than 1,800 tasks to fine-tune LLMs."

**Explanation:** Flan fine-tuned LLMs on instruction templates across 1,800+ tasks.

**Word meanings:**
- (None new.)

**Technical terms:**
- **Flan** = "Fine-tune LAnguage Net" (instruction fine-tuning method).
- **instruction templates** = formatted prompts that give the model a task.

---

> "In several of these tasks, examples were added that included annotated Chain-of-Thought traces that the model could train on. They include arithmetic reasoning, multi-hop reasoning, and natural language inference (determining the truthfulness of a given statement)."

**Explanation:** Some tasks included CoT traces: arithmetic, multi-hop reasoning, NLI.

**Technical terms:**
- **annotated** = labeled with the correct reasoning.
- **multi-hop reasoning** = reasoning requiring several linked steps.
- **natural language inference (NLI)** = judging if a statement follows from evidence.

---

> "Chung et al. fine-tuned various LLMs, including PaLM, which has 540 billion parameters, and prefixed the fine-tuned models with 'Flan.'"

**Technical terms:**
- **PaLM** = Google's "Pathways Language Model".
- **540 billion parameters** = model size.
- **prefixed** = named with a prefix.

---

> "By mixing in Chain-of-Thought data in their pool of training data, the resulting models could be more easily prompted to demonstrate reasoning (e.g., through prompting 'let's think step-by-step'). ... Although this model can demonstrate Chain-of-Thought through proper prompting, it's still a bit premature to call this a reasoning model."

**Explanation:** Flan-PaLM reasons only when prompted; it's not yet a true "reasoning model."

**Word meanings:**
- **premature** = too early.

#### s1: Simple test-time scaling

> "Since Flan-PaLM, reasoning in LLMs has gained more traction. However, creating reasoning data is quite difficult as it typically requires human annotators for large amounts of query/answer pairs. Training data can easily reach hundreds of thousands of examples and adding reasoning traces to each can become costly."

**Explanation:** Reasoning data collection is the expensive bottleneck.

**Word meanings:**
- **traction** = momentum, growing interest.
- **annotators** = people who label data.

---

> "The authors of 's1: Simple test-time scaling' demonstrate that you could get a reasoning LLM using only small data. Using only 1,000 questions paired with reasoning traces, they successfully fine-tuned the Qwen2.5 32B-Instruct LLM to demonstrate stable reasoning."

**Explanation:** s1: just 1,000 question+reasoning-trace pairs suffice to fine-tune a reasoning LLM.

**Technical terms:**
- **Qwen2.5 32B-Instruct** = a 32-billion-parameter open model.

---

> "Compared to Flan-PaLM, they used special tokens to separate the thinking stage from the answering stage. To do so, the thinking stage was enclosed with `<|im_start|>think` while the following answer was enclosed with `<|im_start|>answer`."

**Explanation:** s1 uses special tokens to delimit thinking vs answering.

**Technical terms:**
- **special tokens** = reserved tokens with a structural meaning.

---

> "Using special types of tokens to separate thinking from the final answer is a common method you will see employed by recent LLMs, such as Qwen3 and GPT-OSS."

**Technical terms:**
- **GPT-OSS** = OpenAI's open-source release (2025).

---

> "Additionally, the authors applied test-time scaling explorations by controlling test-time compute in two ways:
> - Forcefully terminating the model's thinking process
> - Lengthening the thinking process by spending 'Wait' multiple times to the generation when it tries to end"

**Explanation:** s1 controls thinking length: force-stop it, or force it to continue by injecting "Wait."

**Word meanings:**
- **forcefully terminating** = stopping early.
- **lengthening** = making longer.

---

> "Going back to OpenAI's experiment, the s1 paper showed similar results in scaling test-time compute; more test-time compute resulted in a better performance."

**Explanation:** s1 confirms: more test-time compute → better performance.

### Reinforcement Learning

> "In Chapter 2, we saw how RLVR uses verifiable rewards to update model weights and met GRPO as the algorithm DeepSeek used. Here we'll see what that looks like in practice and how it produces reasoning behavior as an emergent property."

**Technical terms:**
- **RLVR** = Reinforcement Learning with Verifiable Rewards.
- **GRPO** = Group Relative Policy Optimization (DeepSeek's RL algorithm).
- **emergent property** = a behavior that arises by itself from training.

#### Reasoning with DeepSeek-R1 Zero

> "A key stepping stone to DeepSeek-R1 was an experimental variant called DeepSeek-R1-Zero. It began with the DeepSeek-V3-Base model, but instead of applying supervised fine-tuning on large reasoning datasets, the authors relied solely on reinforcement learning to enable reasoning behavior."

**Explanation:** R1-Zero: no SFT, pure RL from DeepSeek-V3-Base.

**Word meanings:**
- **solely** = only.

---

> "In their training procedure, they used a custom system prompt that outlined how the LLM should respond. To do so, the thinking stage was enclosed with `<think>` and `</think>` while the following answer was enclosed with `<answer>` and `</answer>`. Note that the tags are mentioned but not what the reasoning process itself should look like. That's for the LLM to figure out during training."

**Explanation:** System prompt sets `<think>...</think>` / `<answer>...</answer>` tags but does not dictate *how* to reason — RL figures that out.

---

> "DeepSeek used the same two rule-based rewards we saw in Chapter 2: an accuracy reward for arriving at the correct answer, and a format reward for using the `<think>` and `<answer>` tags correctly. ... The algorithm is GRPO."

**Explanation:** Two rule-based rewards (accuracy + format), trained with GRPO.

**Word meanings:**
- (None new.)

---

> "Again, note how the rewards state only whether the correct format is used and whether the output is correct. How it gets there (through reasoning) is something for the model to figure out."

**Explanation:** Rewards say nothing about the reasoning itself.

---

> "By providing these indirect rewards related to Chain-of-Thought-like behavior, the model found that longer and more complex reasoning processes would often lead to better answers."

**Explanation:** The model discovered on its own that longer reasoning → better answers.

---

> "Interestingly, this graph can also be used to reinforce test-time scaling, where the model scales test-time compute itself through lengthening its response."

**Explanation:** R1-Zero self-scales test-time compute by writing longer responses.

---

> "The model, however, still had a significant drawback. By skipping over supervised fine-tuning and going directly to reinforcement learning, the model suffered from a 'cold start' problem. Without any initial guidance, the model started to mix languages (e.g., mixing English and French in its reasoning traces) and showed poor readability because it lacked markdown formatting to highlight answers for users."

**Explanation:** The cold-start problem: no SFT guidance → language mixing and poor readability.

**Word meanings:**
- **drawback** = disadvantage.

**Technical terms:**
- **cold start** = starting training with no good initialization/examples.

---

> "This is an interesting perspective because they assumed that any reasoning trace should also be readable by users and not merely used to improve its own output. There is something to say for reasoning that is not comprehensible by users, such as reasoning in latent space, but we'll get to this later in this chapter."

**Explanation:** DeepSeek wanted readable reasoning; later the chapter discusses reasoning meant to be invisible (latent space).

**Word meanings:**
- **comprehensible** = understandable.

#### DeepSeek-R1

> "Using the lessons learned from DeepSeek-R1-Zero, the authors come up with the following five training steps to create DeepSeek-R1:
> 1. Cold start prevention
> 2. Reasoning-oriented reinforcement learning
> 3. Rejection sampling
> 4. Supervised fine-tuning
> 5. Reinforcement learning for all scenarios"

**Explanation:** The 5-step R1 pipeline.

**Technical terms:**
- **rejection sampling** = generating many samples and keeping only good ones.

---

> "In step 1, to prevent the cold start problem, DeepSeek-V3-Base was fine-tuned with a small but high-quality reasoning dataset containing about 5,000 samples with long Chain-of-Thought traces. This supervised fine-tuning initialization acts as a reliable starting point to improve readability and monolingual consistency before the model enters the reinforcement learning training. Let's call this resulting model DeepSeek-V3-1 as the intermediate model."

**Explanation:** Step 1: SFT on ~5k high-quality CoT samples → DeepSeek-V3-1 (prevents cold start, improves readability + monolingual consistency).

**Word meanings:**
- **reliable starting point** = dependable initialization.
- **monolingual** = single language.

---

> "In step 2, DeepSeek-V3-1 was further fine-tuned using GRPO, much like was done in DeepSeek-R1-Zero. To further prevent mixing languages in the reasoning traces, a reward was added to ensure the target language remains consistent. The resulting model (DeepSeek-V3-2) is a model trained purely for reasoning, much like DeepSeek-R1-Zero. Like DeepSeek-R1-Zero, this has its pros and cons. Although the model excels at advanced reasoning tasks, it does not do well on tasks that do not require extensive reasoning, such as translation or writing tasks. It did, however, outperform DeepSeek-R1-Zero in most instances that required extensive reasoning and had similar performance on all others."

**Explanation:** Step 2: GRPO + language-consistency reward → DeepSeek-V3-2, a pure reasoning model (great at reasoning, weak at writing/translation).

**Word meanings:**
- **excels at** = is very good at.
- **pros and cons** = advantages and disadvantages.

---

> "In step 3, the reasoning DeepSeek-V3-2 model was used to generate synthetic reasoning data because it excelled at reasoning tasks. A reward model (DeepSeek-V3-Base) was used to select the high-quality reasoning traces. To make the model more adept at non-reasoning tasks, such as writing and translation, DeepSeek-V3-Base was used to sample mostly non-reasoning data that could be trained on. The result was 800,000 samples that contained both reasoning and non-reasoning data."

**Explanation:** Step 3: rejection sampling — DeepSeek-V3-2 generates reasoning data, reward model filters good traces; V3-Base samples non-reasoning data → 800k mixed samples.

**Word meanings:**
- **synthetic** = artificially generated.

---

> "In step 4, this dataset of 800,000 samples was used to perform supervised fine-tuning of the DeepSeek-V3-Base model. This was done, in part, to resolve the cold start problem and to expose the model to these kinds of reasoning traces. The resulting model was the first version of DeepSeek-R1."

**Explanation:** Step 4: SFT on the 800k dataset → first version of DeepSeek-R1.

---

> "Finally, in step 5, reinforcement learning was done on the first version of DeepSeek-R1 so it could develop its own reasoning traces instead of mimicking them from the data. However, to align with human preferences, additional reward signals were added that focused on helpfulness and harmlessness. The model was also asked to summarize the reasoning process to prevent readability issues."

**Explanation:** Step 5: RL for all scenarios with helpfulness/harmlessness rewards and summarized reasoning for readability.

**Word meanings:**
- (None new.)

---

> "This resulted in the final DeepSeek-R1 model. Interestingly, the first three steps of the training pipeline were purely done to create the synthetic data to eventually fine-tune DeepSeek-V3. In sum, DeepSeek-R1 is created through first using supervised fine-tuning on DeepSeek-V3-Base and then applying GRPO with format, accuracy, and preference rewards."

**Explanation:** Recap: steps 1–3 only build the training data; the final recipe = SFT + GRPO with format/accuracy/preference rewards.

### Native Reasoning

> "We have explored various techniques in Supervised Fine-Tuning and Reinforcement Learning. However, after the model has learned to reason, how do you then actually use it? We covered an important part of that during the fine-tuning examples and in Chapter 2, namely the chat template."

**Explanation:** Once trained, reasoning is activated through the model's chat template (special tokens).

**Technical terms:**
- **chat template** = the token structure that formats system/user/model turns.

---

> "Let's illustrate this with the Gemma 4 E4B model. Its chat template (when you don't consider any tool usage) uses, among others, the following special tokens:
> - `<bos>` Signals the beginning of the sequence.
> - `<|turn>system` The start of a system prompt.
> - `<|turn>user` The start of the user turn.
> - `<|turn>model` The start of the model's turn.
> - `<turn|>` Signals the end of a turn.
> - `<|think|>` Adding this token in the system turn enables reasoning. Removing this token in the system turn disables reasoning."

**Explanation:** Gemma 4's special tokens; crucially `<|think|>` in the system turn toggles reasoning.

**Technical terms:**
- **Gemma 4 E4B** = a Google open model (12B in the code examples).
- **bos** = "beginning of sequence".

---

> "Like DeepSeek-R1, Gemma 4 was trained with these special tokens to make sure it understands when it's your turn and when it's the model's turn. Ollama has been parsing our queries automatically based on this chat template, but let's explore what it would look like if it did not automatically parse the queries."

**Explanation:** Ollama normally handles template parsing automatically; here we see the raw format.

**Technical terms:**
- **Ollama** = the local model runner used in the book's code.

---

> "The Gemma 4 E4B model expects the following chat template to differentiate between roles and to enable thinking: `<bos><|turn>system\n<|think|>\nSYSTEM PROMPT<turn|>\n<|turn>user\nUSER PROMPT<turn|>\n<|turn>model`. Note how there are separate turns for the system, user, and model. The thinking can be enabled by adding the `<|think|>` token to the system turn. If you want to disable thinking, you would only have to remove that token."

**Explanation:** Full template walkthrough — `<|think|>` in the system turn is the reasoning on/off switch.

---

> "The model would then respond with a structure like so: `<|channel>thought ... <channel|>` then the answer. As such, any training and fine-tuning that we saw before can now be enabled by the prompt template of that specific model. Instead of needing to use 'tricks' like Chain-of-Thought, the model was trained on Chain-of-Thought examples instead."

**Explanation:** The response has a thought channel then the final answer. Because the model was *trained* on CoT, no prompting tricks are needed.

**Word meanings:**
- (None new.)

## 3.4 Upcoming Fields in Reasoning Research

> "Reasoning in LLMs has started to advance beyond text-only reasoning, now using new modalities, improved architectures, and more abstract ways of representing information. In this section, we look at three key areas pushing LLM reasoning forward:
> - **Reasoning in multi-modal LLMs:** Reasoning LLMs learn to reason with modalities other than text, like images, audio, and video
> - **Efficient reasoning:** Focuses on improving reasoning while using less computation
> - **Reasoning in latent space:** Models think in compressed, abstract forms and often non-textual forms to reach deeper insights"

**Explanation:** Three frontier directions: multimodal reasoning, efficient reasoning, latent-space reasoning.

**Word meanings:**
- **modalities** = forms of input (text, image, audio, video).
- **abstract** = not concrete, conceptual.

### Reasoning in Multi-modal LLMs

> "The LLMs serving as the main brains of agentic systems do not only need to reason about text. They may need to think about the best way to design a website or interpret what is happening in a given picture. This requires reasoning in ways that differ from textual inputs as more modalities might be processed. As we will discuss in Chapter 9, there are many methods for making LLMs multimodal, which often naturally extend them to include reasoning behavior. However, without any reasoning grounding, performance on multimodal reasoning tasks is usually worse."

**Explanation:** Agents need to reason about images etc. Multimodal reasoning needs grounding; otherwise performance suffers.

**Word meanings:**
- **grounding** = connecting reasoning to actual perceived inputs.

**Technical terms:**
- **multimodal LLMs** = LLMs handling text + image/audio/video.

---

> "As with monomodal LLMs, reasoning is often enhanced through methods like those we discussed before, such as prompting, search against verifiers, or modifying the proposal distribution with supervised fine-tuning and/or reinforcement learning."

**Explanation:** Same enhancement toolbox applies to multimodal models.

**Word meanings:**
- **monomodal** = single-modality.

---

> "A common method for multimodal prompting is called Multimodal Chain-of-Thought (MCoT). Multimodal Chain-of-Thought, compared to traditional Chain-of-Thought, attempts to incorporate text and vision into a two-stage framework. ... the first step creates a reasoning by combining the original language and visual input to produce an explicit reasoning process. In the second step, this rationale is appended to the original language input and, together with the same visual input, is used to infer the final answer. Both stages use models with the same architecture, trained separately on annotated rationale and answer data using supervised fine-tuning."

**Explanation:** MCoT = two stages: (1) generate reasoning from text+image; (2) append that reasoning to text+image to produce the final answer.

**Technical terms:**
- **MCoT (Multimodal Chain-of-Thought)** = two-step text+vision reasoning.
- **rationale** = the reasoning/justification.

---

> "Making vision language models reason step-by-step through supervised fine-tuning was further covered in Llava-Chain-of-Thought. Here, the authors used GPT-4o to create synthetic data based on input images through four reasoning stages: summary, caption, reasoning, and answer. These four structured reasoning stages allow for the separation of thinking types and help the model to prevent errors in thinking. As shown in Figure 3-43, this annotated dataset of 100,000 records is then used to fine-tune Llama 3.2v, a vision LLM."

**Explanation:** Llava-CoT: GPT-4o generates synthetic CoT data in 4 stages (summary, caption, reasoning, answer), then fine-tunes Llama 3.2v.

**Technical terms:**
- **Llava-CoT** = a vision-language reasoning model.
- **GPT-4o** = an OpenAI multimodal model (data generator).
- **Llama 3.2v** = a vision-capable Llama model.

---

> "Following this two-step supervised fine-tuning approach is Reason-RFT, a reinforcement fine-tuning approach for visual reasoning that follows the two-step approach of DeepSeek-R1 quite closely. Its two-step approach is meant to go from a base vision LLM to a reasoning vision LLM. The first step is supervised fine-tuning with Chain-of-Thought that activates the reasoning capabilities of the vision LLM. The second step uses reinforcement learning to further generalize the learned reasoning capabilities. Compared to DeepSeek-R1, which included coding-based accuracy rewards, Reason-RFT focuses on three types of rewards:
> - **Mathematical:** Scores numerical answers and gives larger scores for exact matches and lower scores for small errors
> - **Function-based:** Compares predicted and target transformation steps, rewarding exact, partial, and function-only matches with different weights
> - **Discrete-valued:** Uses binary scoring for categorical or integer answers, giving credit only for exact matches"

**Explanation:** Reason-RFT = SFT (activate reasoning) + RL (generalize). Three reward types: math (numeric), function-based (transformation steps), discrete (binary exact match).

**Technical terms:**
- **Reason-RFT** = Reinforcement Fine-Tuning for visual reasoning.
- **binary scoring** = correct/incorrect, 1/0.

---

> "As such, enabling reasoning in multimodal LLMs is much like in text-only LLMs, but the types of Chain-of-Thought data need to be adapted for the multimodal use cases. Often, existing Chain-of-Thought data is for text-only inputs and not multimodal inputs."

**Explanation:** Same principles, but CoT data must be adapted for multimodal inputs.

### Efficient Reasoning

> "Scaling test-time compute can significantly improve performance and has been a main focus of scaling LLMs. However, it can be quite expensive to calculate these additional thousands of tokens or to explore multiple solutions and reasoning paths. Consequently, there has been a growing interest in strategies that achieve similar gains but through more select reasoning and adaptive computation. Typically, this involves reducing the length of the reasoning trace. A nice example is the ever-increasing length of reasoning we explored in DeepSeek-R1. If left without any additional constraints, reasoning LLMs might learn to create unnecessarily large reasoning traces during reinforcement learning."

**Explanation:** Reasoning costs tokens; RL models may bloat reasoning traces. Efficient reasoning aims for the same gains with less compute.

**Word meanings:**
- **select** = careful, targeted.
- **adaptive** = adjusting to the situation.

---

> "The most straightforward method to reduce the reasoning length is through prompting. Traditionally, Chain-of-Thought-like prompting techniques emphasize verbose, step-by-step reasoning, which can create long and expensive reasoning traces that contain redundant information. Chain-of-Draft (CoD), inspired by human cognitive processes, instead employs a more efficient strategy by drafting concise intermediate thoughts."

**Explanation:** CoD replaces verbose CoT with concise draft-like thoughts.

**Technical terms:**
- **Chain-of-Draft (CoD)** = prompting that keeps each reasoning step to ~5 words.

---

> "In Figure 3-45, you can see the system prompts used for Chain-of-Draft compared to regular prompting and Chain-of-Thought. Note how similar it is to Chain-of-Thought, but it adds that each reasoning step should be kept to a draft and use five words at most. Other values are possible, but the authors of Chain-of-Draft used five words as a general guideline to create short prompts rather than strictly enforcing it."

**Explanation:** CoD = CoT but each step limited to ~5 words (a guideline, not hard rule).

---

> "Compared to Chain-of-Thought, Chain-of-Draft generates shorter reasoning traces with less verbosity while having similar performance."

**Explanation:** Same performance, shorter output.

---

> "The balance between minimizing verbosity and keeping the accuracy of reasoning traces is difficult to maintain in Chain-of-Draft. Moreover, it is more difficult to read for users compared to the verbose reasoning traces of traditional Chain-of-Thought."

**Explanation:** Trade-off: CoD is less readable.

---

> "The length of reasoning traces can also be controlled more directly during training. These are often referred to as token-budget-aware LLMs that have been trained to adaptively change the length of the reasoning trace they create based on the complexity of the initial problem. Likewise, they may be trained to prefer shorter reasoning traces through specific length rewards."

**Explanation:** Token-budget-aware models adjust reasoning length to problem complexity; length rewards can incentivize brevity.

**Technical terms:**
- **token-budget-aware LLMs** = models trained to control reasoning length.
- **length rewards** = rewards for short/correct answers.

---

> "As shown in Figure 3-47, the idea of reinforcement learning with length reward designs generally rewards short, correct answers while penalizing lengthy or wrong answers to achieve efficient reasoning. Often, short, correct answers are given greater rewards than long, correct answers."

**Explanation:** Reward design: short+correct best; long or wrong penalized.

---

> "Many such methods include a length reward. Kimi k1.5, a closed-sourced multimodal LLM, was trained using reinforcement learning to give extra rewards to correct, short answers while penalizing wrong, long answers the most. Interestingly, this length reward slowed down early learning, so they gradually warmed up the length penalty during training, which alleviated this problem. Similarly, the O1-Pruner technique compares the length of the model's answer to what a reference model typically produces and rewards based on the difference between them. Longer answers get a penalty while same lengths get no change in rewards and shorter answers get a high reward. They balance this with a dynamic accuracy reward so that not only short, correct answers get high rewards but also long, correct answers when it is needed for hard problems. Finally, L1 is a reasoning LLM that is trained by telling it up front how long, in terms of tokens, it should think (e.g., 'Think for 30 tokens'). The model gets rewarded for both being correct and matching the requested length."

**Explanation:** Three examples: Kimi k1.5 (length reward, warmed up gradually), O1-Pruner (compare length to a reference model + dynamic accuracy reward), L1 (explicit "think for N tokens" instruction + rewards for correctness and length match).

**Word meanings:**
- **alleviated** = reduced, eased.
- **up front** = in advance.

**Technical terms:**
- **Kimi k1.5** = a closed-source multimodal reasoning model.
- **O1-Pruner** = a length-harmonizing fine-tuning technique.
- **L1** = a reasoning model that controls how long it thinks.

---

> "Some models attempt to go in a different direction to control the length of reasoning and implement an on/off switch for reasoning that can either fully disable or enable reasoning. A great example is the Qwen-3 family of models, which, during their release in April 2025, were known to be state-of-the-art for their sizes. They introduced special tokens for thinking mode (`/think`) and non-thinking mode (`/no_think`), where each is trained respectively with and without Chain-of-Thought data. This is also called hybrid reasoning, where thinking mode uses additional reasoning whereas no reasoning is used for the non-thinking mode, which will only provide the answer as it was the case with traditional LLMs. Think of it as essentially turning thinking off and on."

**Explanation:** Qwen-3 hybrid reasoning: `/think` (reason) vs `/no_think` (answer directly), trained on CoT vs non-CoT data — an on/off switch for reasoning.

**Technical terms:**
- **hybrid reasoning** = a model that can toggle thinking on/off.

### Reasoning in Latent Space

> "Chain-of-Thought, as we explored thus far, has been explicit. It's like we are listening to someone work through a problem out loud. The reasoning LLM 'talks to itself' in tokens so we can trace its logic. However, it's not like we are always making our thoughts explicit; we often internalize our thinking processes. An upcoming and interesting take on reasoning is to make the explicit Chain-of-Thought of LLMs internal via latent space reasoning. Here, the explicit Chain-of-Thought steps are replaced by hidden representations. Instead of producing visible reasoning steps, the model thinks entirely in its 'mind's eye,' its latent space. So instead of producing these long reasoning traces, the model will skip straight from the question to the answer and perform all of its reasoning without us being able to see it."

**Explanation:** Latent-space reasoning hides the CoT: the model reasons in hidden representations, skipping straight from question to answer with nothing visible.

**Word meanings:**
- **internalize** = make internal.
- **mind's eye** = the imagination.

**Technical terms:**
- **latent space** = the model's internal, high-dimensional representation space.
- **hidden representations** = the model's internal vector states.

---

> "To explore latent space reasoning, let's first recap Chain-of-Thought reasoning. As seen in Figure 3-48, Chain-of-Thought reasoning starts from a given query and autoregressively generates a token at a time by continuously appending the output to the updated input. After processing an input, the model produces an output by sampling from its last hidden state. The output, a token, is then embedded together with the rest of the input. In other words, the last hidden state is only used to generate the next token."

**Explanation:** Recap of how CoT works mechanically: token-by-token, the last hidden state samples the next token, which gets embedded back into the input.

**Technical terms:**
- **autoregressively** = generating one token at a time, feeding each output back in.
- **last hidden state** = the final internal vector of the model for the current input.

---

> "One of the first methods of latent space reasoning is Chain-of-Continuous-Thought, a method that skips over the decoding of the embeddings and instead directly operates on the last hidden state. As seen in Figure 3-49, instead of embedding new output tokens and adding them to the input, it generates the last hidden state and uses it directly as the input. To differentiate between thoughts and answers, special tokens are used (`<bot>` for beginning of thought and `<eot>` for end of thought). By using the last hidden state as the input, the model does not produce any tokens until it reaches the `<eot>`. After that, it can still generate intermediate reasoning steps if necessary or directly give back the output."

**Explanation:** Chain-of-Continuous-Thought feeds the last hidden state back directly instead of decoding to text tokens. `<bot>`/`<eot>` delimit thinking; no visible tokens until `<eot>`.

**Technical terms:**
- **Chain-of-Continuous-Thought (CoCoT)** = latent-space reasoning on hidden states.
- **`<bot>` / `<eot>`** = begin-of-thought / end-of-thought tokens.

---

> "This methodology was further extended by Continuous Chain-of-Thought via Self-Distillation (CODI). CODI simultaneously trains a teacher and student LLM. The teacher LLM is trained on explicit Chain-of-Thought data and sees the reasoning during training. The teacher LLM learns from this annotated Chain-of-Thought data using cross-entropy loss and has to produce a correct answer with a reasoning trace. In contrast, the student follows a Chain-of-Continuous-Thought-like process and does not produce any explicit Chain-of-Thought but does so instead on the last hidden states. Then, the answers of the teacher LLM and student LLM are compared, and an additional loss is factored in during training. In other words, the explicit Chain-of-Thought that the teacher learns is implicitly being taught to the student LLM."

**Explanation:** CODI = teacher (explicit CoT) + student (latent). The student is trained to match the teacher's answers, transferring explicit reasoning implicitly.

**Word meanings:**
- **distillation** = transferring knowledge from a bigger/expert model to another.
- **implicitly** = without stating directly.

**Technical terms:**
- **CODI** = Continuous Chain-of-Thought via Self-Distillation.
- **cross-entropy loss** = a standard training objective for token prediction.
- **teacher/student** = a distillation setup where one model supervises the other.

---

> "Latent space reasoning is a relatively new field, and new technologies are seemingly being released daily. One interesting perspective is how to balance the implicit and explicit Chain-of-Thought-like behavior of an LLM, given what users might want to see. There is a big advantage to actually seeing how a model reasons, as it will make debugging much easier. However, reasoning in latent space can be a more efficient process and does not limit the LLM to text-based reasoning. Imagine the LLM not reasoning in text but in symbolic language instead, or perhaps purely in latent space through mathematical equations."

**Explanation:** Trade-off: visible reasoning helps debugging; latent reasoning is more efficient and not limited to text.

**Word meanings:**
- (None new.)

## 3.5 Summary

> "In this chapter, we explored how LLMs could develop advanced reasoning. We first covered the paradigm from a focus on train-time compute to test-time compute, of which scaling reasoning is an important component. Reasoning is often shown as either short or long Chain-of-Thought traces. We covered two broad categories for scaling test-time compute. The first, search against verifiers, involved expanding the output of LLMs dynamically through methods such as self-consistency and Best-of-N samples. The second, modifying the proposal distribution, typically involves training or fine-tuning an LLM to demonstrate advanced reasoning traces. We covered two categories of modifying the proposal distribution, namely Supervised Fine-tuning (e.g., Flan-PaLM, s1) and Reinforcement Learning (e.g., DeepSeek-R1 Zero, DeepSeek-R1)."

**Explanation:** Chapter recap in one paragraph.

---

> "We ended the chapter by exploring upcoming fields in reasoning LLMs, including reasoning in latent space, making reasoning more efficient, and methods for multimodal reasoning. In the next three chapters, we start from reasoning LLMs and iteratively give them more capabilities until they reach the state of being called an agent. In Chapter 4, we explore methods to give them memory and track the actions they have taken. In Chapter 5, we show how to give the tools to use and what best practices are for doing so. In Chapter 6, everything comes together where advanced reasoning is used to create plans for agents and reflect to them; reasoning LLMs are especially important here."

**Explanation:** Recap of frontier topics and roadmap: Ch. 4 memory, Ch. 5 tools, Ch. 6 planning/reflection.

**Word meanings:**
- **iteratively** = step by step, building up.

---

## Key Chapter 3 References
- Kahneman, *Thinking, Fast and Slow* (System 1/2).
- Kaplan et al. 2020, "Scaling Laws for Neural Language Models" (Kaplan law).
- Hoffmann et al. 2022, "Training Compute-Optimal Large Language Models" (Chinchilla).
- Jones 2021, "Scaling Scaling Laws with Board Games" (AlphaZero + Hex).
- Wei et al. 2022, "Chain-of-Thought Prompting Elicits Reasoning in LLMs."
- Kojima et al. 2022, "Large Language Models Are Zero-Shot Reasoners."
- Wang et al. 2022, "Self-Consistency Improves Chain of Thought Reasoning."
- Lightman et al. 2023, "Let's Verify Step by Step" (PRM).
- Chung et al. 2024, "Scaling Instruction-Finetuned Language Models" (Flan-PaLM).
- Muennighoff et al. 2025, "s1: Simple Test-Time Scaling."
- Guo et al. 2025, "DeepSeek-R1."
- Xu et al. 2025, "Chain of Draft."
- Kimi Team 2025 (k1.5); Luo et al. 2025 (O1-Pruner); Aggarwal & Welleck 2025 (L1).
- Yang et al. 2025, "Qwen3 Technical Report."
- Hao et al. 2024 (Chain-of-Continuous-Thought); Shen et al. 2025 (CODI).
