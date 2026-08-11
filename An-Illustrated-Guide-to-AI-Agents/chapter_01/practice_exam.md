# AI Agents — Practice Exam (Chapter 1: Introduction)
**Source:** *An Illustrated Guide to AI Agents* (Chapter 1: Introduction)
**Instructions:** Allow ~25–35 minutes. Sections A (MCQ) and B (True/False) are quick checks; Section C is short answer; Section D is essay-style. Answers and marking notes are at the end — don't peek!
**Note:** Question numbers are retained from the combined source exam (`03_practice_exam.md`) for traceability; this file contains only the Chapter 1 questions.

---

## Section A: Multiple Choice (1 point each)
*Circle the best answer.*

1. Which of the following best defines an AI agent per Russell & Norvig?
   a) A program that generates the most likely next token
   b) Anything that perceives its environment through sensors and acts upon that environment through actuators
   c) A model trained on web text to predict the next word
   d) A chatbot that responds to user messages

2. In an LLM-backed agent, the "agent program" (brain) is typically:
   a) The tokenizer
   b) The reward model
   c) A reasoning LLM
   d) The tool-calling software

3. Which of the following is NOT one of the four components at the heart of agents?
   a) Environment
   b) Sensors
   c) Context window
   d) Actuators

4. LLMs generate text one token at a time and feed their own output back as input. This is called:
   a) Backpropagation
   b) Autoregression
   c) Attention
   d) Fine-tuning

5. The main breakthrough of OpenAI o1 and DeepSeek R1 was:
   a) Larger pre-training datasets
   b) Generating explicit reasoning/thinking tokens before the answer
   c) New tokenizers with 200k vocabulary
   d) Fully autonomous tool use without prompts

6. "Train-time scaling" refers to:
   a) Training the model to reason at inference time
   b) Scaling data, compute, and parameters during training for better performance
   c) Reducing training time with KV caching
   d) Using more experts during inference

7. LLMs are said to be "stateless" because:
   a) They have no weights
   b) Information is not persisted across calls — they don't natively remember conversations
   c) They cannot process images
   d) They reset after every token

8. The simplest way to give an LLM memory is:
   a) Retraining the model
   b) Adding the previous conversation to the current prompt
   c) Increasing the temperature
   d) Using a bigger vocabulary

9. Why can't an LLM use tools by itself?
   a) It lacks internet access
   b) It is a text-in/text-out function that can only express intent, requiring external software to parse and execute it
   c) Tool calling requires RLHF
   d) It can, but only with temperature zero

10. "Context engineering" is best described as:
    a) Designing the tokenizer
    b) Balancing the amount and quality of information given to the LLM
    c) Engineering the context length of the model
    d) Choosing the correct decoding strategy

11. An agent that is "partially autonomous" can:
    a) Execute only a single step but freely choose from tools
    b) Delete files without guardrails
    c) Plan and reflect without user input
    d) Run fully without any human checks

12. Which pair are the two main evaluation lenses for agents?
    a) Accuracy and precision
    b) Outcome evaluation and trajectory evaluation
    c) Training loss and validation loss
    d) Reliability and latency

13. The sequence of steps (thought → action → observation) an agent takes is called its:
    a) Prompt
    b) Trajectory
    c) Decoding path
    d) Token stream

14. In multi-agent collaboration, the agent that manages communication and assigns tasks is called the:
    a) Router
    b) Supervisor agent
    c) Orchestrator model
    d) Reward model

15. Which two capabilities make an LLM multi-modal?
    a) Text generation and code generation
    b) Understanding multiple modalities and generating multiple modalities
    c) Pre-training and post-training
    d) Reasoning and planning

16. "Vibe coding" refers to:
    a) Coding with a very high temperature
    b) Non-developers relying on coding agents to build software
    c) Optimizing the KV cache while coding
    d) Using code LLMs only for refactoring

34. Which of the following is NOT one of the book's agent harness types?
    a) Terminal-based
    b) Code-based
    c) Quantum-based
    d) Personal assistant

35. Which is the most famous personal assistant harness mentioned in the book?
    a) Cursor
    b) OpenClaw
    c) Codex CLI
    d) Replit

---

## Section B: True/False (1 point each)
*Write T or F, and if false, correct the statement briefly.*

42. Agents with full autonomy always perform better than those with guardrails. (T/F)
43. Reliability in agents asks whether the agent succeeds every time, not just once. (T/F)

---

## Section C: Short Answer (2–3 points each)
*Answer in 2–5 sentences.*

46. Explain the difference between outcome evaluation and trajectory evaluation of an agent.
48. Why do we say "evaluating an agent is much more than evaluating a model"?
52. Explain the difference between understanding and generating multiple modalities, and name the components used for each.
55. What is the "ceiling effect" of train-time scaling, and what breakthrough addressed it?

---

## Section D: Essay / Applied (5 points each)
*Write structured answers with definitions, explanations, and examples where possible.*

56. **Components of an agent.** Define an AI agent using the Russell & Norvig definition, list its four core components, and explain how each maps to an LLM-backed agent (brain, tools, sensors, environment/user). Then explain how adding memory, tools, planning, and reflection turns a "regular" LLM into an agent.

---

## ANSWER KEY

### Section A: Multiple Choice
1. b — "Anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."
2. c — A reasoning LLM is the brain/agent program.
3. c — Context window is not one of the four components (Environment, Sensors, Actuators, Agent program).
4. b — Autoregression.
5. b — Reasoning LLMs generate explicit reasoning tokens before the answer (test-time compute).
6. b — Scaling data, compute, and parameters during training.
7. b — Stateless: information not persisted across calls.
8. b — Adding previous conversation to the current prompt.
9. b — Text-in/text-out functions can only express intent; external software parses/executes.
10. b — Balancing amount and quality of information given to the LLM.
11. a — Partial autonomy = execute a single step but freely choose tools.
12. b — Outcome and trajectory evaluation.
13. b — Trajectory.
14. b — Supervisor agent.
15. b — Understanding and generating multiple modalities.
16. b — Non-developers relying on coding agents to build software.
34. c — Quantum-based is not a harness type (types: terminal, code, personal assistant, hosted, UI).
35. b — OpenClaw.

### Section B: True/False
42. **F** — Full autonomy can be harmful/overkill; guardrails often make systems more effective and safe.
43. **T** — Reliability = succeeds every time, not just once (outputs are stochastic).

### Section C: Short Answer (model answers)
46. **Outcome vs trajectory evaluation.** Outcome evaluation asks whether the task actually got done — e.g., was the message sent or the record updated. Trajectory evaluation looks at the steps and tool calls the agent took to get there, judged on efficiency and soundness even when the outcome is correct. Example: an agent that gets the right answer by deleting files has a good outcome but a terrible trajectory.

48. **Why evaluating an agent ≠ evaluating a model.** A model is evaluated on single text outputs with benchmarks and scores. An agent reasons over multiple steps, calls tools, and takes action sequences in an environment — so a single quality score on final text rarely captures whether the job was done. You must evaluate the whole system: the outcome, the trajectory (efficiency/soundness), plus reliability (succeeds consistently) and safety (avoids harm).

52. **Understanding vs generating modalities.** Understanding multiple modalities = the LLM can reason about text, images, audio, video simultaneously; implemented with an **encoder** (converts modalities into numeric representations) and a **connector** (links those representations to the LLM). Generating non-text output uses a **generator**. Understanding lets an agent "see" its environment (e.g., a website design); generating lets it respond in a modality other than text (e.g., voice).

55. **Ceiling effect and the fix.** Continuously scaling model size (train-time compute) hit a point where gains were small relative to cost — a ceiling effect. The breakthrough was **reasoning LLMs** (o1, DeepSeek R1) that spend additional test-time compute generating explicit reasoning tokens ("thinking out loud") before answering, unlocking multi-step reasoning without brute-force scaling.

### Section D: Essay (grading notes)
56. **Components of an agent.** Expect: the Russell & Norvig definition; the four components (environment, sensors, actuators, agent program) defined; LLM mapping (brain = reasoning LLM, actuators = tools, sensors = multimodal interpretation, user = part of environment); then the augmentation story — memory (statelessness fix), tools (text-in/text-out → intent + parsing), planning (task decomposition), reflection (updating plans) — culminating in the definition: reasoning LLM + memory + tools + planning + reflection = AI agent.

---

### Scoring Guide
- Section A: 18 pts | Section B: 2 pts | Section C: 8–12 pts (4 questions at 2–3 pts each) | Section D: 5 pts. Total ≈ 33–37 pts.
- **85–100%**: Strong. Review only your missed items.
- **70–84%**: Good. Re-read the Chapter 1 study notes for weak areas (likely autonomy, memory, or evaluation lenses).
- **<70%**: Re-read Chapter 1 and the study notes, then retry this exam in 2–3 days.
