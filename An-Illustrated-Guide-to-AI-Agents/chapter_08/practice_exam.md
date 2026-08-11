# AI Agents — Practice Exam (Chapter 8)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 8 "Multi-Agent Systems"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. Which best defines a multi-agent system?
   a) A single agent with many tools and memory modules
   b) Multiple LLMs running in parallel on separate servers
   c) A team of human evaluators testing one agent
   d) Multiple agents collaborating, coordinating, or competing to achieve a goal

2. Which is NOT one of the three MAS environment types?
   a) Physical
   b) Economic
   c) Virtual
   d) Text

3. Homogeneous agents are typically used for:
   a) Handling distinct roles with different tools
   b) Safeguarding data privacy across organizations
   c) Efficient parallel task execution
   d) Evolving into specialized roles

4. In Hu et al. (2025), LLMs tended to complete in-group ("We are...") sentences with ______ sentiment than out-group sentences.
   a) More positive
   b) More negative
   c) Identical
   d) Unrelated

5. Which orchestration pattern risks system collapse if the orchestration agent fails?
   a) Decentralized
   b) Hierarchical
   c) Federated
   d) Centralized

6. Which orchestration pattern requires the most communication?
   a) Centralized
   b) Decentralized
   c) Hierarchical
   d) Federated

7. In A2A, the agent that executes a task is the:
   a) User
   b) A2A client
   c) Agent card
   d) A2A server (remote agent)

8. The agent card is accessible at:
   a) `/.well-known/agent-card.json`
   b) `/.agents/card.json`
   c) `/.well-known/capabilities.json`
   d) `/.agent-card/card.json`

9. CAMEL's task specifier agent:
   a) Executes the final answer
   b) Ranks hypotheses by debate
   c) Rewrites the user's query to align with the assigned roles
   d) Extracts entities for product titles

10. MetaGPT improves communication efficiency by sharing:
    a) Only free-form dialogue
    b) Artifacts like documents and diagrams
    c) Binary success signals
    d) Hidden weight updates

11. Which of the following is NOT a benefit of a MAS?
    a) Collaboration
    b) Scalability
    c) Faster task solving
    d) Lower compute cost than a single LLM

12. The three attributes balanced by the memory stream are:
    a) Recency, importance, relevance
    b) Speed, latency, bandwidth
    c) Precision, recall, F1
    d) Temperature, top-p, seed

13. During Smallville reflection, the ______ most recent states are retrieved.
    a) 10
    b) 50
    c) 100
    d) 1000

14. Reflection questions are answered through ______ insights.
    a) 3
    b) 5
    c) 10
    d) 100

15. In the orchestrator example, saving €4/day until 2030 yields:
    a) €6,764
    b) €1,691
    c) €4,000
    d) €10,455

16. Which A2A actor initiates communication on behalf of the user?
    a) A2A server
    b) Remote agent
    c) Agent card
    d) A2A client (client agent)

17. The AI co-scientist's Ranking agent ranks hypotheses using:
    a) Exact string match
    b) Elo-style tournaments via scientific debates
    c) K-fold cross-validation
    d) Human pairwise voting

18. Which AI co-scientist agent groups similar hypotheses to remove duplicates?
    a) Generation
    b) Reflection
    c) Proximity
    d) Meta-review

19. MCP is used for ______, while A2A is used for ______.
    a) agent↔tool; agent↔agent
    b) agent↔agent; agent↔tool
    c) agent↔human; agent↔agent
    d) tool↔tool; agent↔agent

20. Which MAS framework mimics entire organizational structures with an SOP and artifacts?
    a) CAMEL
    b) n8n
    c) AutoGen
    d) MetaGPT

21. A2A client communication uses:
    a) Plain HTTP with XML
    b) HTTPS with JSON-RPC 2.0
    c) gRPC with protobuf
    d) WebSockets only

22. Which of the following is a benefit of the decentralized pattern?
    a) Single point of control
    b) Minimal communication overhead
    c) System continues even if some agents fail
    d) Requires the most communication

23. The AI co-scientist was built on:
    a) Gemini 2.0
    b) GPT-4o
    c) Claude
    d) Llama 3

24. In Agent Laboratory's Phase 1, the ______ agent uses the arXiv API to retrieve papers.
    a) Professor
    b) Software engineer
    c) ML engineer
    d) PhD student

25. The Proximity agent's main job is to:
    a) Rank hypotheses by Elo
    b) Review hypothesis quality
    c) Identify similar hypotheses for grouping/deduplication
    d) Generate the first draft

26. The three NeurIPS-style reviewer agents in Agent Laboratory evaluate the report on:
    a) Length, format, citations
    b) Originality, quality, clarity, significance
    c) Readability, font, color
    d) Speed, cost, latency

27. Emergent specialization means:
    a) Identical agents evolve into specialized roles through interaction
    b) Agents are pre-assigned different roles by the designer
    c) Agents share one environment
    d) Agents are grouped into hierarchies

28. Which protocol is designed like instant messaging for agents?
    a) A2A
    b) MCP
    c) FIPA-ACL
    d) Internet of Agents (IoA)

29. In the memory stream, "importance" is judged by:
    a) Cosine similarity
    b) A recency timestamp
    c) Another LLM
    d) A rule-based heuristic

30. Deep Research refers to:
    a) Systems where agents research topics and produce comprehensive reports
    b) Agents that read books deeply
    c) Deep learning on research papers
    d) A benchmark for reading comprehension

31. In Agent Laboratory, which module autonomously generates experimental code?
    a) arXiv API
    b) mle-solver
    c) SOP module
    d) Elo tournament

32. The federated pattern primarily addresses:
    a) Speed of execution
    b) Cost of compute
    c) Evaluation difficulty
    d) Privacy of data across organizations

33. In Smallville, agents decide whether to react to an observation using a:
    a) ReAct-like schema
    b) Decision tree
    c) Rule-based system
    d) Reward function

34. The "€4 per day" example first calls the ______ agent, then the ______ agent.
    a) math; math
    b) date; date
    c) date; math
    d) math; date

35. Table 8-1 sentiment was measured using:
    a) TF-IDF and BM25
    b) RoBERTa and VADER
    c) BERT and LLaMA
    d) Word2Vec and GloVe

36. Which framework requires predefined workflows rather than autonomous agents?
    a) AutoGen
    b) LangGraph
    c) CrewAI
    d) n8n

37. The book's caveat about single-agent systems is that they:
    a) Are always faster
    b) Never make mistakes
    c) Have issues scaling to large, complex use cases
    d) Require no orchestration

38. In A2A, authentication is described in the agent card's:
    a) Security scheme
    b) Skills list
    c) Default output modes
    d) Version field

39. Agent Laboratory's three pre-defined stages are:
    a) Generation, Reflection, Ranking
    b) Search, Cite, Summarize
    c) Plan, Prepare, Deploy
    d) Literature review, experimentation, report writing

40. What does the book say about choosing a MAS framework?
    a) AutoGen is always best
    b) Experimentation is key; there is no single best
    c) CrewAI is always best
    d) n8n is the recommended default

---

## Section B: True/False (1 point each)

41. A MAS may feature one or more environments. (T/F)
42. Heterogeneous agents are typically used for efficient parallel task execution. (T/F)
43. The centralized pattern has no single point of failure. (T/F)
44. The federated pattern allows direct communication of raw data between organizations. (T/F)
45. CAMEL was one of the first MAS frameworks based on role-playing. (T/F)
46. MetaGPT's engineer agent runs and debugs its own code. (T/F)
47. A2A is a proprietary protocol used only by Google. (T/F)
48. The agent card is a JSON file describing an agent's capabilities. (T/F)
49. LLMs show social identity bias favoring out-groups over in-groups. (T/F)
50. The memory stream works by summarizing all stored states. (T/F)
51. Smallville agents reflect on recent events roughly 2–3 times a day. (T/F)
52. The AI co-scientist can be guided by the user while it is computing. (T/F)
53. Agent Laboratory gives agents autonomy over the overall architecture. (T/F)
54. Deep Research products gained significant traction in 2025. (T/F)
55. Research shows LLMs have fully mature theory of mind. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Define a multi-agent system and name the three agent types (with their typical use).
57. Contrast the centralized and decentralized orchestration patterns, giving one advantage and one disadvantage of each.
58. Explain the "subagents as tools" trick and why it helps when an orchestrator has many tools.
59. Describe CAMEL's flow (idea → answer) and name the roles involved.
60. List and briefly describe the five A2A interaction steps.
61. What is an agent card, where is it found, and what does it contain?
62. Explain the three attributes the memory stream balances in the Generative Agents paper.
63. Describe the six specialized agents in the AI co-scientist system.
64. Contrast the AI co-scientist and Agent Laboratory in terms of autonomy and structure.
65. What ethical considerations does the book raise about anthropomorphizing LLMs?

---

## Section D: Essay / Applied (5 points each)

66. **Orchestration patterns.** Compare the four MAS orchestration patterns — centralized, decentralized, hierarchical, and federated — covering how communication is managed, one strength and one weakness of each, and a scenario in which each is the best fit.
67. **The A2A protocol.** Explain what A2A is, how it differs from MCP, the three core actors, the agent card, and the five-step interaction flow. Why is standardization important for MAS?
68. **Generative Agents (Smallville).** Describe the three modules (memory stream, reflection, planning) and the execution schema of the 25 agents. Explain how memory, reflection, and plans interact during execution, and why the memory stream is a RAG-like design.
69. **Deep Research systems.** Compare the AI co-scientist and Agent Laboratory as multi-agent Deep Research systems: their goals, agent roles, pipelines, and how much autonomy the user/agents have in each.
70. **Design a MAS.** Design a multi-agent travel-booking system (hotels, flights, scheduling). Choose an orchestration pattern, name the agents and their tools, describe how they would communicate (including A2A if applicable), and justify your choices with reference to the benefits and drawbacks of MAS.

---

## ANSWER KEY

### Section A: Multiple Choice
1. d
2. b
3. c
4. a
5. d
6. c
7. d
8. a
9. c
10. b
11. d
12. a
13. c
14. b
15. a
16. d
17. b
18. c
19. a
20. d
21. b
22. c
23. a
24. d
25. c
26. b
27. a
28. d
29. c
30. a
31. b
32. d
33. a
34. c
35. b
36. d
37. c
38. a
39. d
40. b

### Section B: True/False
41. **T** — MAS may have physical, virtual, or text environments, one or more.
42. **F** — That's homogeneous agents; heterogeneous agents differ in roles/goals.
43. **F** — Centralized has a single orchestration agent; its failure can collapse the system (single point of failure).
44. **F** — Federated agents only communicate indirectly (results) to safeguard data privacy.
45. **T** — CAMEL was one of the first frameworks, revolving around role-playing.
46. **T** — Iterative programming with executable feedback: the engineer runs and debugs code.
47. **F** — A2A is an open standard (first developed by Google), usable across frameworks and clouds.
48. **T** — JSON file at /.well-known/agent-card.json describing capabilities, URL, etc.
49. **F** — LLMs favor in-groups (in-group sentences more positive), per Hu et al. (2025).
50. **F** — It balances recency, importance, and relevance rather than summarizing everything.
51. **T** — Agents reflect roughly 2–3 times a day.
52. **T** — Users can guide the system while it computes (manual reviews, prioritization).
53. **F** — The stage order is fixed; autonomy is only within each step.
54. **T** — Anthropic, Perplexity, Google, and OpenAI adopted Deep Research products in 2025.
55. **F** — Only early indicators; GPT-4 overlapped with humans only 35–42%.

### Section C: Short Answer (model answers)
56. **MAS definition + types.** A MAS consists of multiple agents collaborating, coordinating, or at times competing to achieve a goal; each may have specific subgoals. Types: (1) homogeneous — similar capabilities, for efficient parallel execution; (2) heterogeneous — different roles/goals, possibly different LLMs, memory, tools; (3) emergent specialization — identical agents evolving into specialized roles through interaction.
57. **Centralized vs decentralized.** Centralized: one orchestration agent controls communication and allocates tasks — advantage: easy to implement/manage (most common); disadvantage: single point of failure. Decentralized: no clear leader, communication distributed — advantage: stable, continues if agents fail, scales by adding agents; disadvantage: requires many capable agents and significant communication overhead.
58. **Subagents as tools.** Wrap each specialized subagent in a Python function (e.g., ask_math_agent, ask_date_agent) and register them as tools of an orchestrator. With dozens/hundreds of tools an orchestrator makes mistakes sifting through them; dedicating subsets to subagents reduces the load, and subagent trajectories show how optimized each was for its task.
59. **CAMEL flow.** User proposes an idea → AI user + AI assistant created with roles (e.g., blogger/programmer) → task specifier agent rewrites the query to align with the roles → AI user and AI assistant hold multi-turn conversations → AI user ends and returns the completed answer.
60. **Five A2A steps.** (1) Task initiation — user defines the task (not agent-to-agent); (2) Discovery — client finds available agents via agent cards; (3) Authentication — via the card's security scheme (API keys, OAuth 2.0); (4) Client agent communication — HTTPS + JSON-RPC 2.0, remote agent can request more info and stream updates; (5) Remote agent communication — returns information or artifacts.
61. **Agent card.** A `.json` file at `/.well-known/agent-card.json` describing an agent: name, description, url, version, capabilities (streaming, pushNotifications, stateTransitionHistory), default input/output modes, and a skills list.
62. **Memory stream attributes.** (1) Recency — higher score for more recent states; (2) Importance — LLM-judged significance (might evoke strong emotions); (3) Relevance — cosine similarity between an embedded question and each embedded state. It distills many states into the few relevant to the agent's current situation (RAG-like).
63. **AI co-scientist agents.** Generation (initiates research, literature via search tools); Reflection (reviews correctness/quality/explanatory power); Ranking (Elo-style tournaments via scientific debates); Proximity (groups/deduplicates similar hypotheses); Evolution (refines highest-scoring hypotheses using literature + unconventional reasoning); Meta-review (combines all insights to improve the system).
64. **AI co-scientist vs Agent Laboratory.** AI co-scientist: supervisor dynamically orchestrates agents, user can guide it live, context memory shared. Agent Laboratory: fixed three-stage order (literature review → experimentation → report writing) with high autonomy inside each step but no autonomy over the overall architecture.
65. **Ethics.** Strong apparent emotional intelligence invites anthropomorphization; in critical situations (mental health, suicide prevention) responsibility is unresolved — LLM, provider, or creators? What counts as misuse? The book doesn't answer but stresses ethical consideration.

### Section D: Essay (grading notes)
66. **Expect** four patterns defined: centralized (single orchestration agent; easy but single point of failure; fit: small teams, simple delegation); decentralized (no leader; stable/resilient but heavy communication; fit: fault-tolerant peer systems); hierarchical (layered authority, like organizations; intuitive but most communication; fit: org-style workflows); federated (indirect cross-org communication for privacy; needs shared standards; fit: multi-organization data-sharing).
67. **Expect** A2A = open standard (Google) for agent↔agent collaboration vs MCP's agent↔tool; actors = user, A2A client (initiates), A2A server (executes); agent card JSON at /.well-known/agent-card.json; five steps: task initiation, discovery, authentication, client communication (HTTPS + JSON-RPC 2.0), remote communication. Standardization reduces custom connections across frameworks/clouds.
68. **Expect** memory stream (recency + importance + relevance, RAG-like, no full summarization); reflection (2–3×/day: 100 most recent states → 3 salient questions → 5 insights, stored back); planning (location + start time + duration, stored in memory); execution via ReAct-like schema asking "should you react...?"; observations update plans when important; dialogue drives spontaneous interactions.
69. **Expect** AI co-scientist: Gemini 2.0, goal = hypotheses + research proposal, six agents, supervisor + context memory, live user steering. Agent Laboratory: goal = report + code, three fixed phases, PhD/postdoc/professor/ML-engineer/software-engineer agents, mle-solver, NeurIPS-style reviewers. Autonomy: co-scientist dynamic/user-steered; Lab fixed order, autonomy within steps.
70. **Expect** a coherent design: e.g., centralized orchestrator (booking supervisor) with specialized agents — flight agent (flight-search tool), hotel agent (hotel API), schedule agent (calendar/dates tool), possibly a math agent for costs; justify choice (easy management, single failure point mitigated); communication via A2A agent cards + JSON-RPC if cross-platform, or function-tool wrappers in a single framework (AutoGen/LangGraph/CrewAI); note benefits (parallelism, specialization, scalability) and drawbacks (cost, complexity, evaluation).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely orchestration patterns or the A2A flow).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
