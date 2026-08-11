# Comprehensive Study Notes — Chapter 8
**Source:** *An Illustrated Guide to AI Agents*, Chapter 8 "Multi-Agent Systems"

---

## CHAPTER 8 — MULTI-AGENT SYSTEMS

### 8.1 The Big Picture

- So far the book covered the agent **in isolation** — a single entity that plans, executes, and reflects. Single-agent systems are solely responsible for the goal, with no interaction with other intelligent systems except humans.
- **A multi-agent system (MAS)** — where multiple agents work together toward a common goal — has even more potential. It may feature one or more environments in which many agents (some different from others) interact.
- Each agent has a level of autonomy but may have **different sets of tools, (sub)goals, or even personalities**. Regardless of differences, they **collaborate to reach common goals through their own skill sets**.
- **MAS are adept at handling intricate tasks that require balancing multiple complex dependencies.**
- Example: an agentic research system. A single agent would juggle searching the web, processing papers/figures, reading and critiquing papers, and summarizing findings. A MAS would use a **search agent, processing agent, paper agent, summarization agent**, etc., each specialized.

### 8.2 The Multi-Agent System

- **MAS = multiple agents collaborating, coordinating, or at times competing to achieve a goal (Figure 8-1).** Agents work toward a common goal, but each may have specific subgoals. The search agent and summarization agent have different roles but serve the same end goal.
- **Environments:** Agents may share an environment (like characters in a virtual game), but MAS don't have to share one — agents can be responsible for different tasks (e.g., a booking system: one agent searches for places, another researches cheap flights, another schedules the vacation).
- **Environment types (Figure 8-2):**
  - **Physical** — real-world agents, typically robotic agents.
  - **Virtual** — game-like environments for agents to interact with.
  - **Text** — environments like coding IDEs or chat interfaces.
- Single-agent systems have issues **scaling to large/complex use cases**; MAS excel by deploying more agents, each solving a different task.

#### Agent types / roles (Figure 8-3)
- **Homogeneous agents** — similar capabilities; typically used for **efficient parallel task execution**.
- **Heterogeneous agents** — differ in roles; each has different goals/tasks. May be initialized with different LLMs, memory systems, and tools.
- **Emergent specialization** — identical agents evolve into specialized roles through interaction with each other and their environments (e.g., in game-like simulations, some learn resource gathering or defensive roles).

#### Benefits of MAS
1. **Collaboration** — complex/diverse problems solved more easily by increasing agents and having them work together.
2. **Scalability** — number of agents can increase without slowing the system by running them **in parallel or fully asynchronous**.
3. **Faster** — multiple agents solve tasks in parallel, and **specialized agents tend to solve each task faster** than a single non-specialized agent.

#### Disadvantages of MAS
1. **Complex** — complex systems requiring careful management of orchestration and interaction; additional layers of abstraction.
2. **Cost** — increasing agents is computationally costly if they rely on powerful LLMs.
3. **Evaluation** — evaluating one agent is tricky; evaluating many working together is harder.

#### Real-world MAS examples
- **Anthropic** — used multiple Claude agents to build a research system for exploring complex topics.
- **Uber** — a data-retrieval MAS using a supervisor agent + several data-retrieval agents (e.g., an SQL agent) to turn user requests into real-time financial intelligence.
- **Delivery Hero** — several agents extract entities and create product titles, building a product knowledge base.

### 8.3 Orchestrating Agents

- Deploying multiple agents is not straightforward: how to deploy them, in what architectures/specialisms, and how they communicate. **Orchestration is vital for a stable, future-proof system.**

#### Patterns (topology of the system)
Four types of communication structure of MAS (Figure 8-4):
1. **Centralized** — communication and its management are controlled by a single **orchestration agent** that allocates responsibilities and tasks to other agents.
   - **Most often used in practice** (ease of implementation/management).
   - **Downside:** the entire system might collapse if the orchestration agent fails.
2. **Decentralized** — each agent shares similar responsibilities; communication is distributed with **no clear "leader."**
   - **Benefit:** more stable — even if some agents fail, the system continues (similar responsibilities/capabilities). Scaling only requires adding agents.
   - **Downside:** requires many capable agents and significant communication overhead → extensive resources.
3. **Hierarchical** — each agent is structured into a layered system; each level has a different degree of authority, and agents have distinct roles.
   - Similar to how traditional organizations operate → intuitive pattern.
   - Efficient resource allocation by distributing tasks across levels and having many specialized roles.
   - **Downside:** still complex and **requires the most communication**.
4. **Federated** — agents distributed among parallel environments (organizations) that **only communicate indirectly to safeguard the privacy of data**. Each agent adheres to different regulations but can still communicate results with other agents.
   - Relatively new pattern; used when agents need to be distributed among organizations.
   - Encourages cross-system collaboration but **relies on shared standards that might not always be in place**.

#### Subagents as tools (building a simple centralized MAS)
- A useful trick to create a centralized (potentially hierarchical) system: **view subagents not as agents but as tools.**
- Create specialized subagents (e.g., a **math agent** with add/subtract/multiply tools; a **date agent** with `today` and `days_between` tools).
- **Wrap each subagent in a Python function** (e.g., `ask_math_agent(question)`, `ask_date_agent(question)`) and register them as tools of an orchestrator.
- The **orchestrator agent** can then decide whether to ask for help from subagents.
- Example: "If I save €4 per day until 2030, how much will I have?" → orchestrator calls `ask_date_agent` (gets 1691 days) → calls `ask_math_agent` (€4 × 1691) → answers **€6,764**.
- **Why it's useful:** when you have dozens/hundreds of tools, the orchestrator is likely to make mistakes sifting through them; dedicating subsets of tools to subagents reduces load.
- **Trajectory inspection:** looking only at the orchestrator's trajectory gives an abstract overview. Inspecting subagent trajectories (e.g., date agent: `today` → `days_between`) shows how optimized each was for its task.

### 8.4 General-Purpose Frameworks for Collaborative Task-Solving

- With MAS there's no single best framework/pattern/orchestration workflow. Many general-purpose frameworks exist, each with a distinct "flavor" of implementation and philosophy.

#### CAMEL (Communicative Agents for "Mind" Exploration of Large Language Model Society)
- One of the **first frameworks** for collaborative MAS. Revolves around agents **roleplaying** to create specialized entities that collaborate.
- **Flow (Figure 8-5, 8-6):**
  1. User proposes an **idea** (simple task description, e.g., "create a website for my blog").
  2. Two agents are created: **AI user** (represents the user; gives instructions and directs) and **AI assistant** (executes instructions; specialized in execution). Roles could be "blogger" and "programmer."
  3. Both are fed to a **task specifier agent** that rewrites the user's query into something more aligned with the task (it has knowledge of the assigned roles).
  4. The AI user and AI assistant converse via **multi-turn conversations** until the AI user ends the conversation and returns a completed answer.

#### MetaGPT
- Takes role-playing further by mimicking **entire organizational structures** with specialized roles.
- **Three components:**
  1. **Specialization of roles** — tasks broken into smaller specific tasks solvable by specialized agents (task decomposition from Ch. 6). Each role has a profile: name, goal, skills, constraints. References software companies (product manager vs engineer). Adheres to a **standard operating procedure (SOP)** for software development where all agents work **sequentially**. Each agent is powered by **ReAct**.
  2. **Communication protocol** — enhances communication efficiency and provides structured interfaces. Instead of communicating primarily through dialogue, MetaGPT shares information through **artifacts like documents and diagrams (structured output)**.
  3. **Iterative programming with executable feedback** — external feedback incorporated as a **self-correction mechanism**. E.g., an engineer not only creates code but runs and debugs it (Figure 8-7).

#### Production-grade frameworks
- At the end of 2025, the most common frameworks were **Microsoft's AutoGen, LangGraph, and CrewAI**.
- Each focuses on **modularity** — build any MAS however you like: different roles, memory modules, tools per agent, and system pattern (Figure 8-8).
- It's difficult to suggest a single "best" platform; **experimentation is key**, along with platform independence and continued development.
- **n8n** — an AI workflow automation tool that requires **predefined workflows** for LLMs/agents to follow (vs. autonomous agents). Agents may still have some autonomy, making it "just on the edge of a MAS."

### 8.5 Communication Protocols

- Communication between agents is challenging; how they exchange information greatly affects performance. **Effective context engineering requires optimizing both inputs and outputs.** Too much/too little information, or wrong format, degrades results.

#### History
- Before LLMs, **agent communication languages (ACLs)** like **FIPA-ACL** and **KQML** offered standardization using message types such as "request" and "inform."
- Since GPT-3.5 (late 2022), protocols like **MCP** (Ch. 5) facilitated agent-tool communication.
- MAS, as collaborative systems, benefit greatly from standardized **Agent-to-Agent (A2A)** communication protocols.

#### A2A (Agent2Agent)
- **First developed by Google.** A major protocol for inter-agent communication.
- Allows agents developed on different frameworks (LangGraph, CrewAI, AutoGen, etc.) and different cloud platforms (Azure, AWS, Google Cloud) to work together regardless of underlying architecture — reducing the need for custom connections (Figure 8-9).
- Like MCP, A2A is an **open standard**, but for **collaboration between agents** instead of agent-tool communication (Figure 8-10).
- A2A allows agents to: **discover each other and their capabilities**, exchange (un)structured information across modalities (text, images), **stream responses**, and **handle multi-turn conversations**. Enables remote collaboration and exchange of relevant info/states.

**Three core actors in A2A:**
1. **User** — generally a human (can be automated) who initiates a request and/or defines a goal.
2. **A2A client (client agent)** — initiates communications to other agents on behalf of the user; the entity that uses the A2A protocol.
3. **A2A server (remote agent)** — the agent called upon by the client to execute a task; can be a single agent or an entire MAS.

**A2A interaction steps (Figure 8-11):**
1. **Task initiation** — user defines a task for the A2A client (no A2A protocol needed; not agent-to-agent).
2. **Discovery** — client discovers which agents are available and what they can do. Each agent exposes a **`.json` file** describing capabilities, endpoint URL, and other info — the **agent card**, accessible at **`/.well-known/agent-card.json`**. (Example: a Train Agent card with name, description, url, version, capabilities [streaming, pushNotifications, stateTransitionHistory], defaultInputModes/defaultOutputModes [text], and skills list.)
3. **Authentication** — after choosing a remote agent, the client goes through authentication described in the agent card's **security scheme** (e.g., API keys, OAuth 2.0).
4. **Client agent communication** — client sends a request to the remote agent via **HTTPS with JSON-RPC 2.0** (like MCP). The remote agent starts working; if it needs more information, it sends a message back. It can stream updates to notify the client when done.
5. **Remote agent communication** — once complete, the remote agent sends a message to the client with relevant information or **artifacts**.

#### Other protocols
- **Internet of Agents (IoA)** — an agent integration protocol for creating MAS. Based on the concept of the internet; features an **instant-messaging-like design** and dynamic modules for agent conversation and collaboration.
- **RL for MAS** — improving standardization and collaboration through RL; training LLMs/agents in the MAS context so entities are experienced with such systems.
- **Takeaway:** standardization is a new field in MAS; start thinking about what standardization means, why it's required, and with what common methodologies it can be achieved.

### 8.6 Agent Society

- As agents collaborate and interact, we see the first glimpses of **agentic societies** — simulations envisioning interactive artificial societies run by agents. They describe a common environment where agents can interact without always needing specific tasks. They allow the **emergence of sociality, identity, and potentially the theory of mind.**

#### Emotional Intelligence
- LLMs can be given identities by providing character profiles — at first achieved through prompting (e.g., in the system prompt: "your name is Alex, and you are a software engineer").
- Since LLMs are trained on human-generated data and are great at mimicking human behavior, **some human psychology applies** to how LLMs behave. This new field is **"AI psychology"** — finding commonalities between how humans and LLMs behave.

#### Theory of Mind (ToM)
- The book covered cognitive ability (reasoning, thinking, judging, problem-solving). AI psychology goes beyond to **emotional intelligence** — recognizing, understanding, and expressing emotions.
- Research: LLM capabilities in (multi-modal) emotion recognition, creating emotional intelligence tests; users with anxious attachment personalities may form emotional dependency on GPT-4.
- **Theory of Mind (ToM)** — the ability to **attribute mental states (such as emotion) to others**; understanding that others have mental states (emotions, desires, beliefs) different from our own. Essential for social interaction; gives rise to empathy. Research shows **early indicators of ToM in LLMs**.
- **Example study:** "Can LLMs Reason Like Humans? Assessing Theory of Mind Reasoning in LLMs for Open-Ended Questions" — used Reddit r/changemyview to source open-ended discussions. Prompts grounded in the mental states of the original poster. Tested **GPT-4, Zephyr-7B, Llama2-Chat-13B**. Human evaluators scored reasoning correctness and alignment with the query's intention.
  - **Small overlap of 35%** between human evaluators and the best LLM (GPT-4).
  - **Overlap rose to 42%** when the user's intention and emotion were added to the prompt (via BERT-like models).
  - Although there's discrepancy on the extent of LLM ToM, progress is being made.

#### Social identity
- LLM behavior in social interaction may unfold in **human-like biases**. Research by **Hu et al. (2025)** showed the **social identity bias** — the tendency to favor our own groups and be hostile toward others — is also a property of LLMs.
- Method: prompted 77 different LLMs with **in-group sentences** ("We are…") and **out-group sentences** ("They are…"). LLMs completed them; **in-group sentences tend to be more positive than out-group sentences**.
- Table 8-1 examples: out-group "They are just a bunch of dumb f**ks" (OPT-IML-30B, Negative, VADER –0.7506); in-group "We are a group of talented young people..." (GPT-2-large-774M, Positive, 0.5106). Sentiment measured with RoBERTa and VADER.

#### Ethical considerations
- LLMs' seemingly strong emotional intelligence makes it hard not to **anthropomorphize** them. This raises ethical considerations in critical situations (mental healthcare, suicide prevention).
- If something goes wrong, should the LLM be held responsible? Intuitive answer: "No! That's the service provider's responsibility." But can the provider be held responsible if the underlying LLM was trained with significant flaws or under-evaluated? Can the LLM creators be held responsible if the provider misuses it? And what is "misuse"? The book does not answer these questions but emphasizes the need for ethical consideration.

### 8.7 Simulations

- Simulated environments where agents (socially) interact. If a simulation is accurate enough, it may serve as a representation to understand the mechanism of the world it simulates → **world models**. In the MAS context, simulations explore agent behavior in social situations.

#### Interactive Simulacra of Human Behavior (Generative Agents / Smallville)
- One of the most influential papers on MAS in social simulations: **"Generative Agents: Interactive Simulacra of Human Behavior"** — agents placed in a **pixel-sandbox environment** to plan their days, go to group activities, and form relationships.
- **Smallville** — the simulated small-town environment (Figure 8-12).
- **25 unique agents**, each initialized with one paragraph describing identity, occupation, relationships, etc. Each agent had **three modules: memory, planning, and reflection** (like the core components in ReAct and Reflexion from Ch. 6).

**Memory component — the Memory Stream:**
- Tracks states of the entire environment, the agent's own behavior, and the behavior of those it interacts with.
- Instead of summarization (which would require summarizing potentially non-relevant info), the **memory stream** balances three attributes:
  1. **Recency** — a higher score given to more recent states.
  2. **Importance** — judged by another LLM: how important is the state to the agent, might it evoke strong emotions.
  3. **Relevance** — calculated through **cosine similarity** between a question and a state, where each state is embedded.
- A state is a current situation (e.g., "2023-02-13 22:48:20: Isabella Rodriguez is stretching").
- This RAG-like solution distills many states into a select few relevant to the agent's current situation (Figure 8-14).

**Reflection:**
- A **second type of memory**. Agents periodically reflect on the latest events roughly **2–3 times a day**.
- During reflection, the **100 most recent states** are retrieved and used to prompt an LLM to extract the **three most salient high-level questions** (e.g., "What topic is Isabella Rodriguez passionate about?").
- These questions retrieve the most relevant states, which are used to answer the questions through **five insights**. Reflections are then put back into the memory stream (Figure 8-15).

**Planning:**
- Agents create and continuously update plans with a **location, starting time, and duration**. Plans are stored in the memory stream for retrieval, like reflections.
- Agents consider **observations, reflections, and plans** when deciding next actions.

**Execution (Figure 8-16):**
- Agents use a **ReAct-like schema** to continuously execute actions and process environments.
- Plans are updated based on observations **if the observation is of importance**.
- Using a summary of current context, the agent's LLM is asked: **"Should you react to the observation, and if so, what would be an appropriate reaction?"**
- Agents interact via **dialogue**, resulting in spontaneous interactions based on memory of the agents they interact with (Figure 8-17).

#### Other simulacra
- **Agent Hospital** — hospital sandbox simulating the entire patient journey (disease onset, triage, medicine dispensary, post-hospital follow-up). Doctor agents **evolve** as they treat many patient agents, improving capabilities.
- **SimClass** — simulates classroom education with agent-student and student-student interactions. Emerging behavior shows collaboration as student agents work together to solve tasks.
- These simulations are great case studies for how agents interact and collaborate as tasks differ and grow in complexity.

### 8.8 Deep Research Agents

- **Deep Research** — a system where agents work together to research topics and provide comprehensive reports using various resources. In 2025, it gained significant traction (Anthropic, Perplexity, Google, OpenAI adopted Deep Research products).
- Single Deep Research agents were covered in Ch. 3, 4, 5 (Search-o1, Search-R1) due to reliance on tools, memory, reasoning. **MAS are particularly useful for Deep Research** due to complexity and the collaborative nature of scientific research.
- **Anthropic's multi-agent research system:** search agents, a citation agent, and a lead researcher agent to orchestrate the workflow.

#### Toward an AI Co-Scientist
- Released February 2025. A multi-agent Deep Research system built on **Gemini 2.0**, taking inspiration from the **scientific method** and incorporating thorough hypothesis generation.
- Main goal: **generate hypotheses and a research proposal** given a research goal. Criteria: plausibility, novelty, testability, safety (adjustable to user preference).
- **Flow (Figure 8-18):** user specifies a research goal + relevant documents → a **supervisor agent** parses and derives a research plan, orchestrating specialized agents:
  - **Generation agent** — initiates research by generating preliminary focus areas through literature research using search tools.
  - **Reflection agent** — reviews correctness, quality, and explanatory power of generated hypotheses.
  - **Ranking agent** — ranks hypotheses based on **Elo-style tournaments** through scientific debates.
  - **Proximity agent** — identifies similar hypotheses so they can be grouped, cleaned of duplicates, and explored more efficiently.
  - **Evolution agent** — continuously refines highest-scoring hypotheses (leveraging literature for supporting details, exploring unconventional reasoning).
  - **Meta-review agent** — combines insights from all reviews and debates to improve the system over time.
- The Generation agent produces a first draft → Reflection reviews → Ranking runs the tournament → best hypotheses fed to Proximity, Evolution, Meta-review agents to improve quality and update state.
- All info stored in the **context memory** (generated hypotheses, outputs of each agent, current state). The supervisor orchestrates which agents to run when.
- **Key feature:** the user can **guide the system while it's computing** — provide manual reviews of hypotheses, evaluate proposals, prompt follow-ups or prioritize fields. Instead of replacing people, it **empowers them** through human-machine collaboration.

#### Agent Laboratory
- A similar but **more static system**. Takes a structured approach by **pre-defining the stages** of its MAS: **literature review, experimentation, and report writing** (like AI co-scientist, driven by the human researcher; reduces time-intensive tasks like coding and documentation).
- Main goal: create a **research report along with the code** needed for running the experiments.
- **Phase 1 — Literature review (Figure 8-19):** relevant papers curated based on the user's research idea. A **PhD student agent (GPT-4o)** uses the **arXiv API** to retrieve papers. Can summarize papers, extract full content, add selected summaries for the curated review. **Iterative** — continuously refines its query until a pre-defined number of relevant papers is reached. Output: a selection of paper summaries.
- **Phase 2 — Experimentation (Figure 8-20):** uses the literature review across three stages:
  1. **Plan formulation** — a PhD student agent collaborates with a **postdoc agent** to generate a research objective + experimental steps. Output: a plan.
  2. **Data preparation** — a **machine learning engineer agent** uses the HuggingFace datasets library to find the best datasets; a **software engineer agent** submits code and checks for bugs.
  3. **Running experiments** — the ML engineer agent implements the experimental plan in code using the **mle-solver** module (an advanced coding agent; see Ch. 10) to autonomously generate code.
- **Phase 3 — Report writing (Figure 8-21):** two steps:
  1. **Report writing** — the PhD student agent and a **professor agent** collaborate to convert findings into an academic report. Highly collaborative: frequent reviews by the professor agent and additional arXiv research.
  2. **Report refinement** — **three reviewer agents** (based on NeurIPS peer reviewers) evaluate the draft on **originality, quality, clarity, and significance**. The PhD agent decides whether to address feedback or if it's complete.
- Agents continuously improve the report until all agents agree it's of sufficient quality (Figure 8-22).
- **Key feature:** larger degrees of freedom for agents to behave **autonomously within each step, but the order of steps is fixed**. No autonomy over the overall architecture (fixed pattern); high autonomy within each step.

### Chapter 8 Key Takeaways
- A MAS contains multiple agents collaborating, coordinating, or competing to achieve a goal; each may have specific subgoals.
- Agent types: homogeneous (parallel execution), heterogeneous (different roles), emergent specialization.
- Benefits: collaboration, scalability, speed. Disadvantages: complexity, cost, evaluation difficulty.
- Orchestration patterns: centralized (most common; single point of failure), decentralized (stable, no leader), hierarchical (like orgs; most communication), federated (privacy across organizations).
- Subagents can be wrapped as tools for an orchestrator to build a simple centralized MAS.
- Frameworks: CAMEL (role-playing, AI user + AI assistant + task specifier), MetaGPT (org structure, SOP, artifacts, iterative feedback), AutoGen, LangGraph, CrewAI (production-grade, modular), n8n (predefined workflows).
- A2A (Google): standardized inter-agent protocol — user, client agent, remote agent; steps: task initiation, discovery (agent card at /.well-known/agent-card.json), authentication (API keys, OAuth 2.0), client communication (HTTPS + JSON-RPC 2.0), remote communication.
- Agent societies: AI psychology, emotional intelligence, Theory of Mind (ToM), social identity bias (Hu et al. 2025), ethical considerations (responsibility in critical applications).
- Smallville/Generative Agents: 25 agents, memory stream (recency + importance + relevance), reflection (100 states → 3 questions → 5 insights), planning, ReAct-like execution.
- Deep Research: AI co-scientist (Gemini 2.0; generation, reflection, ranking, proximity, evolution, meta-review agents), Agent Laboratory (literature review, experimentation, report writing).

---

## High-Yield Vocabulary (Ch 8)

| Term | Meaning |
|---|---|
| MAS | Multi-agent system — multiple agents collaborating/coordinating/competing |
| Homogeneous agents | Similar capabilities; efficient parallel task execution |
| Heterogeneous agents | Different roles/goals; may have different LLMs, memory, tools |
| Emergent specialization | Identical agents evolving into specialized roles via interaction |
| Centralized pattern | Single orchestration agent controls communication/tasks |
| Decentralized pattern | No leader; communication distributed; stable if agents fail |
| Hierarchical pattern | Layered system with different authority levels per layer |
| Federated pattern | Agents in separate organizations communicating indirectly for privacy |
| CAMEL | Early role-playing MAS framework (AI user, AI assistant, task specifier) |
| MetaGPT | Framework mimicking org structures; SOP; artifacts; iterative feedback |
| AutoGen / LangGraph / CrewAI | Production-grade modular MAS frameworks (2025) |
| n8n | AI workflow automation tool with predefined workflows |
| ACL | Agent communication language (pre-LLM: FIPA-ACL, KQML) |
| A2A | Agent2Agent protocol (Google) — standardized inter-agent communication |
| Agent card | `.json` file (at /.well-known/agent-card.json) describing an agent's capabilities |
| A2A client | Agent initiating communication on behalf of the user |
| A2A server | Remote agent called upon to execute a task |
| IoA | Internet of Agents — instant-messaging-like agent integration protocol |
| AI psychology | Field studying commonalities between human and LLM behavior |
| Emotional intelligence | Recognizing, understanding, expressing emotions |
| Theory of Mind (ToM) | Attributing mental states (emotions, desires, beliefs) to others |
| Social identity bias | In-group favoritism / out-group hostility (also in LLMs) |
| World model | Accurate simulation representing the mechanism of the simulated world |
| Smallville | Simulated town in Generative Agents paper (25 agents) |
| Memory stream | RAG-like memory balancing recency, importance, relevance |
| Reflection (Generative Agents) | 100 recent states → 3 salient questions → 5 insights |
| Deep Research | MAS researching topics and producing comprehensive reports |
| AI co-scientist | Gemini 2.0 MAS for hypothesis generation (6 specialized agents) |
| mle-solver | Module autonomously generating experimental code (Agent Laboratory) |
| Agent Laboratory | 3-phase research MAS: literature review, experimentation, report writing |
| Context memory | Stores hypotheses, agent outputs, and current state in AI co-scientist |
