# AI Agents — Flashcards / Q&A (Chapter 1)
**Source:** *An Illustrated Guide to AI Agents* (Chapter 1: Introduction)
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.
**Note:** Question numbers are retained from the combined source file (`02_flashcards_qna.md`) for traceability. This file contains only the Chapter 1 questions.

---

## Section A: Term → Definition

**Q1. What is an AI agent? (Russell & Norvig definition)**
**A:** "An agent is anything that can be viewed as perceiving its environment through sensors and acting upon that environment through actuators."

**Q2. What are the four components at the heart of an agent?**
**A:** Environment, Sensors, Actuators, and Agent program.

**Q3. In an LLM-backed agent, what does the agent program (brain) typically map to?**
**A:** A reasoning LLM.

**Q4. In an LLM-backed agent, what are the actuators?**
**A:** The LLM's tools.

**Q5. What are the sensors in an LLM-backed agent?**
**A:** Multimodal LLM capabilities that interpret more than text (images, sound, etc.).

**Q6. What is a token?**
**A:** The unit of LLM input/output — a word, part of a word, number, or punctuation (e.g., "flamingos" → "flamingo" + "s").

**Q7. What is an autoregressive model?**
**A:** A model that consumes its own previously generated output as input for generating the next token, one token at a time.

**Q8. What is train-time scaling?**
**A:** Improving LLM performance by scaling up data, compute, and parameters during training (e.g., GPT-3.5 lineage).

**Q9. What is the "ceiling effect" of train-time scaling?**
**A:** Continuously scaling model size becomes too expensive for the small performance gains it produces.

**Q10. What is test-time compute / reasoning in LLMs?**
**A:** Spending additional compute to generate explicit "thoughts" (reasoning tokens) before producing the answer — as in OpenAI o1 and DeepSeek R1.

**Q11. Why are LLMs considered "stateless"?**
**A:** Information is not persisted across calls; they do not natively remember past conversations.

**Q12. What is the simplest way to give an LLM memory?**
**A:** Adding the previous conversation to the current prompt.

**Q13. What is context engineering?**
**A:** Carefully balancing the amount and quality of information given to the LLM to help it tackle its task (avoiding information overload).

**Q14. Why can't an LLM use tools by itself?**
**A:** Because an LLM is a text-in/text-out function — it can only describe or show intent (e.g., output `multiply(2.3, 8.1)` as text); external software must parse and execute that text.

**Q15. What is the "augmented LLM" (Anthropic's term)?**
**A:** A reasoning LLM augmented with memory and tools — the building block that becomes an agent.

**Q16. What is planning / task decomposition?**
**A:** Breaking a large task into smaller, actionable steps, executing them one at a time while referring back to the plan.

**Q17. What is reflection in an agent?**
**A:** The agent uncovering faults in its past behavior/plan and attempting to fix them, updating the initial plan as it executes.

**Q18. What is the iterative agent loop involving planning and reflection?**
**A:** Plan → take actions → reflect on the output → update the plan.

**Q19. What are guardrails?**
**A:** Constraints on agent autonomy that steer it toward expected behaviors and prevent destructive actions (e.g., deleting important files).

**Q20. What is a hallucination?**
**A:** An LLM confidently generating incorrect information.

**Q21. What are the two main lenses for evaluating agents?**
**A:** Outcome evaluation (did the task get done?) and trajectory evaluation (were the steps/tool calls efficient and sound?).

**Q22. What two additional evaluation properties don't surface in a single run?**
**A:** Reliability (succeeds every time, given stochastic outputs) and Safety (avoids harm).

**Q23. What is a supervisor agent in multi-agent collaboration?**
**A:** An agent that manages communication among agents and is typically responsible for advanced behavior like planning, decomposing, and assigning tasks — often with the most capable LLM.

**Q24. What two capabilities make an LLM multi-modal?**
**A:** Understanding multiple modalities (via an encoder + connector) and generating multiple modalities (via a generator).

**Q25. What is "vibe coding"?**
**A:** Non-software engineers relying on coding agents to build software.

**Q26. What is an agent harness? Give the five types.**
**A:** The code/software implementing agent behavior. Types: terminal-based, code-based, personal assistant, hosted, and UI-based.

**Q27. Name the personal assistant harness examples given in the book.**
**A:** OpenClaw (~300k GitHub stars in months) and Hermes Agent.

---

## Section B: Short-Answer Questions (Concept Checks)

**Q61. Why is reasoning so important for AI agents?**
**A:** Agents must plan, select tools, reflect on mistakes, and revise plans — all of which require advanced reasoning. Reasoning LLMs are especially good at complex decision-making, multi-step problem decomposition, and generalizing to novel problems. Trade-off: "regular" LLMs are preferred when fast, cheap responses are needed.

**Q62. Explain the difference between understanding and generating multiple modalities.**
**A:** Understanding = the LLM can reason about several modalities simultaneously (e.g., "see" a website design); implemented with an encoder (converts modalities into numeric representations) plus a connector (links representations to the LLM). Generating = producing output in a non-text modality; implemented with a generator.

**Q66. Why is evaluating an agent harder than evaluating an LLM?**
**A:** Agents reason over multiple steps, call tools, and take sequences of actions — a single quality score on final text rarely captures whether the job was done. You must evaluate the whole system: outcome, trajectory, reliability, and safety.

**Q67. Why do autonomy and guardrails need to be balanced?**
**A:** Full autonomy can be overkill for the task or harmful. A system with many guardrails is often more effective because it steers the agent toward expected behaviors and away from undesired ones (also, human-in-the-loop becomes more necessary as autonomy grows).

**Q71. What makes an agent "multi-agent" vs "single-agent"?**
**A:** Multi-agent systems deploy multiple different agents, each responsible for different tasks, that interact and consult each other's specialties (often under a supervisor). Single-agent systems are one entity.

**Q72. Why would you choose a reasoning LLM vs a regular LLM?**
**A:** Reasoning LLMs: better at complex, multi-step, novel problems — but slower/more expensive. Regular LLMs: fast and cheap, fine for straightforward tasks.

---

## Section C: Fill-in-the-Blank

**Q76.** The LLM first breaks input into ______, which are subcomponents of words.
**A:** tokens

**Q77.** Generating one token at a time, feeding each output back as input, is called ______.
**A:** autoregression (autoregressive generation)

**Q78.** Models like OpenAI o1 and DeepSeek R1 generate ______ before deriving their final answer.
**A:** reasoning tokens (thoughts / a reasoning trace)

**Q79.** LLMs are ______ entities — information is not persisted across calls.
**A:** stateless

**Q80.** The discipline of carefully balancing what information to give an LLM is called ______.
**A:** context engineering
