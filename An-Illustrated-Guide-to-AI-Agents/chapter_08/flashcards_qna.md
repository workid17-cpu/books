# Flashcards & Q&A — Chapter 8
**Source:** *An Illustrated Guide to AI Agents*, Chapter 8 "Multi-Agent Systems"
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.

## Part 1: Term → Definition

1. **MAS** → Multi-agent system — multiple agents collaborating, coordinating, or at times competing to achieve a goal.
2. **Single-agent system** → One agent solely responsible for its goal, interacting only with humans, not other intelligent systems.
3. **What three things may differ between agents in a MAS?** → Tools, (sub)goals, and even personalities.
4. **Three MAS environment types** → Physical (real-world robots), virtual (game-like), text (coding IDEs, chat interfaces).
5. **Homogeneous agents** → Similar capabilities; typically used for efficient parallel task execution.
6. **Heterogeneous agents** → Differ in roles, goals, and tasks; may be initialized with different LLMs, memory systems, and tools.
7. **Emergent specialization** → Identical agents evolving into specialized roles through interaction with each other and their environments.
8. **Three benefits of MAS** → Collaboration, scalability, and being faster.
9. **Three disadvantages of MAS** → Complexity (orchestration/management), cost (LLM compute), and harder evaluation.
10. **Centralized pattern** → A single orchestration agent controls communication and allocates responsibilities/tasks to other agents.
11. **Why is centralized the most common pattern?** → Ease of implementation and management.
12. **Downside of the centralized pattern** → The entire system may collapse if the orchestration agent fails (single point of failure).
13. **Decentralized pattern** → Each agent shares similar responsibilities; communication is distributed with no clear "leader."
14. **Benefit of the decentralized pattern** → More stable — even if some agents fail, the system continues.
15. **Downside of the decentralized pattern** → Requires many capable agents and significant communication overhead → extensive resources.
16. **Hierarchical pattern** → Agents structured into a layered system; each level has a different degree of authority and distinct roles.
17. **Downside of the hierarchical pattern** → Still complex and requires the most communication.
18. **Federated pattern** → Agents distributed among parallel environments (organizations) that only communicate indirectly to safeguard data privacy.
19. **Why use the federated pattern?** → When agents need to be distributed among organizations; each adheres to different regulations but can share results.
20. **Drawback of the federated pattern** → Relies on shared standards that might not always be in place.
21. **The "subagents as tools" trick** → Wrapping specialized subagents in Python functions and registering them as tools of an orchestrator agent.
22. **Result of the orchestrator example "€4 per day until 2030"** → €6,764 (1691 days × €4), using date agent then math agent.
23. **Why does dedicating tool subsets to subagents help?** → With dozens/hundreds of tools an orchestrator makes mistakes; subsets reduce its load.
24. **CAMEL** → One of the first MAS frameworks; agents roleplay (AI user + AI assistant) to create specialized collaborating entities.
25. **CAMEL's task specifier agent** → Rewrites the user's query into something more aligned with the task, knowing the assigned roles.
26. **MetaGPT** → Framework mimicking entire organizational structures with specialized roles, powered by ReAct.
27. **MetaGPT's three components** → Specialization of roles (with SOP), a communication protocol (artifacts like docs/diagrams), and iterative programming with executable feedback.
28. **SOP in MetaGPT** → Standard operating procedure for software development where all agents work sequentially.
29. **MetaGPT's self-correction mechanism** → Iterative programming with executable feedback — e.g., an engineer runs and debugs its own code.
30. **The three most common production-grade MAS frameworks (end of 2025)** → Microsoft's AutoGen, LangGraph, and CrewAI.
31. **What do AutoGen, LangGraph, and CrewAI focus on?** → Modularity — build any MAS with different roles, memory modules, tools per agent, and system patterns.
32. **n8n** → AI workflow automation tool requiring predefined workflows; "just on the edge of a MAS."
33. **ACL (agent communication language)** → Pre-LLM standardization like FIPA-ACL and KQML, using message types such as "request" and "inform."
34. **A2A (Agent2Agent)** → Open standard protocol (first developed by Google) for collaboration between agents, reducing custom connections.
35. **MCP vs A2A** → MCP = agent↔tool communication; A2A = agent↔agent collaboration.
36. **What A2A allows agents to do** → Discover each other and their capabilities, exchange (un)structured information across modalities, stream responses, and handle multi-turn conversations.
37. **The three core actors in A2A** → User (initiates), A2A client/client agent (communicates on user's behalf), A2A server/remote agent (executes the task).
38. **Agent card** → A `.json` file describing an agent's capabilities, endpoint URL, and info, accessible at `/.well-known/agent-card.json`.
39. **The five A2A interaction steps** → Task initiation, discovery, authentication, client agent communication, remote agent communication.
40. **A2A authentication schemes** → Described in the agent card's security scheme — e.g., API keys, OAuth 2.0.
41. **A2A client communication transport** → HTTPS with JSON-RPC 2.0 (like MCP).
42. **Internet of Agents (IoA)** → Agent integration protocol based on the internet concept; instant-messaging-like design and dynamic modules for conversation/collaboration.
43. **RL for MAS** → Training LLMs/agents in the MAS context so entities are experienced with such systems and collaboration improves.
44. **Agentic society** → Simulation of an interactive artificial society run by agents, allowing emergence of sociality, identity, and potentially theory of mind.
45. **AI psychology** → Field finding commonalities between how humans and LLMs behave.
46. **Emotional intelligence** → Recognizing, understanding, and expressing emotions.
47. **Theory of Mind (ToM)** → Ability to attribute mental states (emotions, desires, beliefs) to others, different from our own; gives rise to empathy.
48. **Social identity bias** → Tendency to favor our own groups (in-group) and be hostile toward others (out-group) — also a property of LLMs.
49. **Hu et al. (2025) finding** → Prompting 77 LLMs to complete in-group ("We are…") and out-group ("They are…") sentences: in-group completions are more positive.
50. **RoBERTa and VADER** → Sentiment models used to measure in-group/out-group sentence sentiment in Table 8-1.
51. **Why is anthropomorphization ethically tricky?** → Strong apparent emotional intelligence in critical contexts (mental healthcare, suicide prevention) raises unresolved responsibility questions.
52. **World model** → An accurate simulation that serves as a representation to understand the mechanism of the world it simulates.
53. **Generative Agents / Smallville** → Influential paper simulating 25 agents in a small town, each with memory, planning, and reflection modules.
54. **Memory stream** → RAG-like memory that scores states by recency, importance, and relevance instead of summarization.
55. **The three attributes balanced by the memory stream** → Recency (higher for recent states), importance (LLM-judged), relevance (cosine similarity between question and embedded state).
56. **What is a "state" in the memory stream?** → A current situation record, e.g., "2023-02-13 22:48:20: Isabella Rodriguez is stretching."
57. **Reflection (Generative Agents)** → Agents retrieve the 100 most recent states, extract the 3 most salient high-level questions, answer them via 5 insights, and store reflections back.
58. **How often do agents reflect?** → Roughly 2–3 times a day.
59. **Planning (Generative Agents)** → Agents create and update plans with a location, starting time, and duration, stored in the memory stream.
60. **Execution (Generative Agents)** → ReAct-like schema; the LLM is asked "Should you react to the observation, and if so, what would be an appropriate reaction?"
61. **Agent Hospital** → Hospital sandbox simulating the full patient journey; doctor agents evolve as they treat patient agents.
62. **SimClass** → Simulates classroom education with agent-student and student-student interactions; emergent collaboration behavior.
63. **Deep Research** → System where agents work together to research topics and provide comprehensive reports using various resources.
64. **Anthropic's multi-agent research system** → Search agents, a citation agent, and a lead researcher agent to orchestrate the workflow.
65. **AI co-scientist** → Google's Feb 2025 multi-agent Deep Research system on Gemini 2.0 generating hypotheses and a research proposal.
66. **AI co-scientist evaluation criteria** → Plausibility, novelty, testability, and safety (adjustable to user preference).
67. **The six specialized agents in AI co-scientist** → Generation, Reflection, Ranking, Proximity, Evolution, and Meta-review.
68. **Ranking agent** → Ranks hypotheses based on Elo-style tournaments through scientific debates.
69. **Proximity agent** → Identifies similar hypotheses so they can be grouped, deduplicated, and explored efficiently.
70. **Evolution agent** → Continuously refines the highest-scoring hypotheses, leveraging literature and unconventional reasoning.
71. **Meta-review agent** → Combines insights from all reviews and debates to improve the system over time.
72. **Context memory (AI co-scientist)** → Stores generated hypotheses, outputs of each agent, and current state; supervisor uses it to decide which agents to run.
73. **AI co-scientist's key feature** → The user can guide the system while it computes — manual reviews, evaluating proposals, follow-ups, prioritizing fields.
74. **Agent Laboratory** → More static research MAS pre-defining three stages: literature review, experimentation, report writing.
75. **Agent Laboratory's main goal** → Create a research report along with the code needed for running the experiments.
76. **Phase 1 of Agent Laboratory (literature review)** → A PhD student agent (GPT-4o) uses the arXiv API to iteratively retrieve papers until enough relevant ones are found.
77. **Phase 2 of Agent Laboratory (experimentation)** → Plan formulation (PhD + postdoc agents), data preparation (ML engineer + software engineer agents), running experiments (mle-solver).
78. **mle-solver** → Module (advanced coding agent, see Ch. 10) that autonomously generates experimental code.
79. **Phase 3 of Agent Laboratory (report writing)** → PhD + professor agents write the report; three NeurIPS-style reviewer agents evaluate originality, quality, clarity, and significance.
80. **Agent Laboratory's key feature** → High autonomy within each step, but fixed order of steps (no autonomy over overall architecture).

## Part 2: Short Answer

81. **What makes MAS well suited to intricate tasks?** → They balance multiple complex dependencies by deploying multiple specialized agents that collaborate, each handling a different task.
82. **Explain emergent specialization with a game example.** → Identical agents in a game-like simulation interact and evolve distinct roles — some learn resource gathering, others defensive roles — without being programmed with those roles.
83. **Contrast centralized vs decentralized orchestration on failure resilience.** → Centralized collapses if the orchestration agent fails (single point of failure); decentralized continues because agents share similar responsibilities/capabilities.
84. **Walk through the "€4 per day until 2030" orchestrator example.** → Orchestrator calls ask_date_agent ("days between now and 2030") → 1691 days; calls ask_math_agent (€4 × 1691) → answers €6,764.
85. **What does inspecting each subagent's trajectory reveal?** → How optimized each subagent was for its task (e.g., date agent: today → days_between), vs the orchestrator trajectory giving only an abstract overview.
86. **Describe CAMEL's flow from idea to answer.** → User proposes an idea → AI user + AI assistant created with roles → task specifier rewrites the query → multi-turn conversation between AI user and AI assistant → AI user ends and returns the completed answer.
87. **Why does MetaGPT share artifacts instead of only dialogue?** → It enhances communication efficiency and provides structured interfaces — documents and diagrams instead of free-form chat.
88. **How does A2A reduce the need for custom connections?** → It's an open standard allowing agents built on different frameworks (LangGraph, CrewAI, AutoGen) and cloud platforms (Azure, AWS, GCP) to interoperate.
89. **What information does an agent card contain?** → Name, description, url, version, capabilities (e.g., streaming, pushNotifications, stateTransitionHistory), default input/output modes, and skills list.
90. **Explain the ToM open-ended study results.** → Using Reddit r/changemyview, GPT-4 had 35% overlap with human evaluators; adding the user's intention and emotion (via BERT-like models) raised overlap to 42%.
91. **Why do LLMs show human-like psychological traits?** → They're trained on human-generated data and are great at mimicking human behavior, so some human psychology applies.
92. **How does the memory stream decide what to retrieve?** → It scores every state by recency (recent = higher), importance (LLM-judged emotional significance), and relevance (cosine similarity of embedded question and state), then selects the few most relevant.
93. **Walk through one reflection cycle in Smallville.** → Retrieve the 100 most recent states → LLM extracts the 3 most salient high-level questions → those questions retrieve the most relevant states → answer via 5 insights → store reflections back into the memory stream.
94. **How do Smallville agents decide whether to react?** → Using a summary of current context, the LLM is asked "Should you react to the observation, and if so, what would be an appropriate reaction?" Plans update when observations are important.
95. **Summarize AI co-scientist's pipeline.** → User gives research goal + documents → supervisor derives a plan → Generation produces a first draft → Reflection reviews → Ranking runs Elo-style tournaments → best hypotheses go to Proximity (dedupe), Evolution (refine), Meta-review (improve system) → all state in context memory.
96. **Contrast AI co-scientist and Agent Laboratory autonomy.** → AI co-scientist lets the user steer live and orchestrates agents dynamically; Agent Laboratory fixes the stage order (literature → experimentation → report) with high autonomy only inside each step.
97. **Why are MAS particularly useful for Deep Research?** → Research is complex and collaborative (search, citations, writing); MAS provides the specialization and orchestration to handle it.
98. **What unresolved ethical question does the book raise about LLMs?** → If something goes wrong in a critical context, who is responsible — the LLM, the provider, or the creators? (The book does not answer; it calls for ethical consideration.)
99. **Why might a hierarchical pattern feel intuitive?** → It mirrors how traditional organizations operate, with layers of authority and specialized roles.
100. **How does federated pattern protect privacy?** → Organizations' agents only communicate indirectly (results), each adhering to different regulations, so raw data stays inside each environment.

## Part 3: Fill-in-the-Blank

101. "A multi-agent system consists of multiple agents ______, ______, or at times ______ to achieve a goal." → collaborating; coordinating; competing
102. "MAS environments can be ______ (robots), ______ (games), or ______ (IDEs/chat)." → physical; virtual; text
103. "______ agents are used for efficient parallel task execution; ______ agents differ in roles." → Homogeneous; heterogeneous
104. "In the centralized pattern the system might ______ if the orchestration agent fails." → collapse
105. "The ______ pattern has no clear leader; the ______ pattern has levels of authority; the ______ pattern safeguards privacy across organizations." → decentralized; hierarchical; federated
106. "Subagents can be wrapped in Python ______ and registered as ______ of an orchestrator." → functions; tools
107. "CAMEL's two collaborating roles are ______ and ______, plus a ______ agent that refines the query." → AI user; AI assistant; task specifier
108. "MetaGPT agents follow a standard operating procedure and work ______." → sequentially
109. "The three production-grade modular frameworks are ______, ______, and ______." → AutoGen; LangGraph; CrewAI
110. "A2A was first developed by ______ and is an ______ standard." → Google; open
111. "An agent's capabilities are described in its ______, found at ______." → agent card; /.well-known/agent-card.json
112. "A2A client communication uses HTTPS with ______." → JSON-RPC 2.0
113. "______ and ______ were pre-LLM agent communication languages." → FIPA-ACL; KQML
114. "The ability to attribute mental states to others is ______." → theory of mind (ToM)
115. "Hu et al. (2025) showed LLMs exhibit ______ bias — favoring in-group over out-group." → social identity
116. "The memory stream balances ______, ______, and ______." → recency; importance; relevance
117. "During reflection, the ______ most recent states yield the ______ most salient questions, answered through ______ insights." → 100; 3; five
118. "Smallville agents plan with a ______, ______, and ______." → location; starting time; duration
119. "AI co-scientist's Ranking agent uses ______-style tournaments." → Elo
120. "In Agent Laboratory, the ______ agent uses the arXiv API for literature review, and ______ autonomously generates experimental code." → PhD student; mle-solver
