# Chapter 10 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 10 "Code Agents and Code LLMs"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings are paraphrased/annotated; every substantive paragraph is covered.

---

## Section 1: Introduction

1. **"Coding agents are some of the most potent types of AI agent in terms of the size of their likely impact. Code generation became one of the earliest applications of LLMs once they hit the scale of GPT-3 in 2020. As we saw in Chapter 3, code generation was also a major component for reasoning problems that reasoning LLMs tackle. Code is a key investment area for LLMs for two reasons. First, an enormous range of knowledge work, from software engineering to research, data analysis, and visualization, can be expressed as code. Second, and more useful for training, code can often be verified automatically: you can run it, or test it, and get a clear signal of whether it worked. That verifiability is what makes coding more tractable than open-ended tasks where there is no objective check."**
   - **Plain-English:** Coding agents matter hugely; code is a key LLM investment area because (1) much knowledge work can be expressed as code and (2) code can be verified by running it.
   - **Word meanings:** *potent* = powerful; *tractable* = solvable/manageable; *verifiable* = can be checked objectively.
   - **Technical terms:** GPT-3 (2020) = the scale where code generation took off; verifiability = running tests gives a clear pass/fail signal.

2. **"This chapter walks through that world. We'll meet the people who use and build code agents, see what makes software engineering agents distinct, build one ourselves, and finish with how the underlying code LLMs are trained."**
   - **Plain-English:** Roadmap: users/builders, software-engineering agents, build one, then training.
   - **Technical terms:** code LLM = the model underneath a code agent.

## Section 2: Users and Builders

3. **Figure 10-1: "The people who use and build code agents, and the audience for this chapter. Users range from those who want a non-code output, to vibe coders, to software engineers, depending on how closely they engage with the code. Builders divide into those who build the agents and those who train the LLMs underneath them."**
   - **Plain-English:** Three user groups (non-code output, vibe coders, software engineers) and two builder groups (agent builders, LLM trainers).
   - **Word meanings:** *vibe coder* = someone building software through natural language.
   - **Technical terms:** non-code output = a chart/analysis the user never codes for.

4. **"Users of coding agents fall into three groups that differ in how closely they work with the code itself: those who just want a non-code output like a chart or an analysis and may never see the code; vibe coders who want working software but build it through natural language and let the agent handle the details; and software engineers who read, review, and direct the agent inside a codebase."**
   - **Plain-English:** Users range from no-code (output only) to vibe coders to expert reviewers/directors.
   - **Word meanings:** *codebase* = a repository of code.

5. **"Builders are split in two: those who build the agents, assembling the tools, context, and loop around a model, which we will do ourselves later in this chapter, and those who train the coding LLMs that everything else depends on."**
   - **Plain-English:** Two kinds of builders: assemble agents (tools/context/loop) or train the underlying models.
   - **Technical terms:** loop = the plan-act-observe cycle around the model.

6. **"People often ask LLMs to generate code. Coding agents take this to a whole new level by having the ability to execute that code, debug any errors it produces, and iterate over multiple steps to solve bigger and bigger problems."**
   - **Plain-English:** Code agents go beyond generation: they execute, debug, and iterate.
   - **Technical terms:** iterate = repeat steps until the task is solved.

7. **Figure 10-2 examples: "(1) asking LLMs, mostly within a playground UI, to write a specific piece of code. (2) the LLM has generated the code, executed it in an execution environment, and then presented the results to the user. Flows like this open up coding agents to a massive audience of users. (3) the agent solves larger tasks that can include tens or even hundreds of steps before that final result is produced. Here it also has to rely on a richer execution environment."**
   - **Plain-English:** Evolution: (1) code in a playground, (2) generate + execute + present results (opens to non-developers), (3) software-engineering-scale multi-step tasks.
   - **Word meanings:** *playground UI* = a simple chat interface for experimenting.
   - **Technical terms:** execution environment = sandbox/VM where code runs.

## Section 3: Building Code Agents

### Code Agents to Serve Non-Coders

8. **"Code agents open the door for non-coders to wield powerful code tools they did not have access to before. Data analysis is a great example of this."**
   - **Plain-English:** Non-coders gain powerful analysis via agents.
   - **Word meanings:** *wield* = use.

9. **"When we ask the agent to plot a figure, the main behaviors we expect from it are the following: 1. Data retrieval — The agent will need to decide what tool to use to retrieve the data we need to plot. On the simpler side, this can be a web search tool. But this can also require the agent to generate code to retrieve that data."**
   - **Plain-English:** First behavior: get the data, possibly by writing retrieval code.
   - **Technical terms:** data retrieval = fetching the data to plot.

10. **"A) You hand the agent the file (for example, a spreadsheet) — A simple tool to use here is a Python sandbox environment that we pass the exact file to. Here, the agent will need to write code to read and manipulate the data in data manipulation libraries like pandas or polars."**
    - **Plain-English:** Case A: file provided → Python sandbox + pandas/polars.
    - **Technical terms:** pandas/polars = Python data-manipulation libraries.

11. **"B) The agent has to find the files itself — A more advanced scenario is if the tool is a command line and the agent needs to write the code to find and retrieve the file (or files) first. Tools like this would operate on a virtual machine with access to a filesystem. The code generated here would use command-line tools to search and navigate the filesystem."**
    - **Plain-English:** Case B: agent searches the filesystem with command-line tools on a VM.
    - **Technical terms:** virtual machine (VM) = full environment with a filesystem.

12. **"C) The data is in a database — In a scenario like this, the agent can be connected to a SQL tool giving it access to one or multiple databases. The agent would need to write the SQL code and send it to the tool to execute to retrieve and transform the required data."**
    - **Plain-English:** Case C: SQL tool queries the database.
    - **Technical terms:** SQL = Structured Query Language.

13. **"2. Creating the plot — Once the agent has the data it needs, it will need to write the code to plot it, and return the resulting plot. 3. Troubleshooting — We cannot assume that everything will go smoothly... if we get an error from the execution environment and send it back to the LLM, it can recover on the next step. This is the ReAct loop from Chapter 6 doing its job: the error is just another observation, and the model reasons about it and tries a corrected action. However, we shouldn't rely on that alone. The more we plan for specific failure cases, the better the end user experience will be."**
    - **Plain-English:** Then plot the data; when errors occur, feed them back to the LLM as observations (ReAct) and also plan for failure cases up front.
    - **Technical terms:** ReAct loop = reasoning + acting with observations; observation = error output fed back.

14. **"Notice that in this example the user doesn't necessarily see or interact with any of the code. This is one of the major levers of power for systems like this that dramatically increase the number of people it can be useful for."**
    - **Plain-English:** Hiding the code broadens the audience dramatically.
    - **Word meanings:** *levers of power* = key advantages.

### Code Tools

15. **"Code tools are a pivotal component in how we tie an LLM to a software system. The model decides what it wants to do, and the tool carries that action out against a real environment: running code in a sandbox, executing a shell command, or querying a database."**
    - **Plain-English:** Tools are how the model acts on real systems.
    - **Technical terms:** code tool = the bridge from model decision to environment action.

16. **File manipulation tools: "Some of the first tasks a code agent needs to handle are reading, searching, and editing files. These can be implemented as distinct tools, each with its own permissions and checks. This enables the agent to find relevant files, edit them, create new files, and navigate through filesystems. These tools are often, but not always, wrappers around command-line tools."**
    - **Plain-English:** File tools let the agent read/search/edit files; often wrappers around CLI commands.
    - **Technical terms:** wrapper = function exposing a CLI tool to the model.

17. **"Common file manipulation tools to give code agents include: Read entire file (e.g., cat). Read a section of a file (to better manage model context when dealing with long files) (e.g., sed, head, tail). List files in a directory (e.g., ls). Pattern-match file names that follow a certain pattern (e.g., glob). Search the contents of files (e.g., grep)."**
    - **Plain-English:** Standard file tools: cat, sed/head/tail, ls, glob, grep.
    - **Technical terms:** context management = reading only relevant file sections to save context.

18. **Code interpreter sandbox: "A code interpreter or sandbox is a code execution environment that is a little more restricted than a full virtual machine. These restrictions are important from a security point of view because LLM-generated code is not guaranteed to be safe. An agent could delete important files, corrupt a system, or share private data with external third parties. This can happen either through intended malicious attacks (possibly utilizing prompt injection), or as well-meaning or naive agent actions."**
    - **Plain-English:** Sandboxes restrict LLM-generated code because it can be harmful (deletion, corruption, data leaks) — via attacks or naive mistakes.
    - **Technical terms:** sandbox = restricted execution environment; prompt injection = planted malicious instructions.

19. **"These sandboxes have to be defined with certain limited resources (e.g., in memory, processor, or disk space). They often are ephemeral so that: 1) A fresh environment is created for each user or session. This makes the initial state of the sandbox predictable (e.g., in terms of what packages are available, for example). This also helps ensure the data of user A is separated from the agents of user B. 2) Data is deleted at the end of the session and there's no expectation of data persistence."**
    - **Plain-English:** Sandboxes are resource-limited and ephemeral: fresh per user/session, data deleted afterward.
    - **Word meanings:** *ephemeral* = short-lived/temporary.
    - **Technical terms:** persistence = surviving beyond the session.

20. **"For security reasons, these sandboxes are often restricted in terms of which commands can run, and you can decide whether the code running inside has internet access. Every such restriction trades convenience for security: it's tempting to grant broad access so the agent never gets stuck, but each capability you add widens what a mistake or an attack can do."**
    - **Plain-English:** Every capability granted widens the blast radius of mistakes/attacks.
    - **Word meanings:** *blast radius* = extent of damage.
    - **Technical terms:** trade-off = convenience vs security.

21. **"Code sandboxes have become a part of commercial services from LLM providers and agent providers... Gemini, for example, provides a dedicated code execution tool that can be accessed via its API."**
    - **Plain-English:** Sandboxes are commercialized; Gemini offers a code execution tool via API.
    - **Technical terms:** code execution tool = sandbox exposed through an API.

22. **Code (Gemini code execution):**
    ```python
    ## pip install google-genai
    ## requires a Gemini API key from Google AI Studio
    from google import genai
    from google.genai import types

    client = genai.Client()

    system_message = """
    You are a personal math tutor. When asked a math question,
    write and run code using the code execution tool to answer the question.
    """

    resp = client.models.generate_content(
        model="gemini-2.5-flash",
        contents=prompt,
        config=types.GenerateContentConfig(
            system_instruction=system_message,
            tools=[types.Tool(code_execution=types.ToolCodeExecution())],
        ),
    )
    ```
    - **Plain-English:** A Gemini client with a system message instructing it to use the code execution tool.
    - **Technical terms:** code_execution tool = the sandbox; generate_content = API call.

23. **"Inspecting the response object shows the model has written the code... This leads this execution result to be returned to the model... so it then produced the response... We can see the two steps of executing such a command in Figure 10-5. It starts with the model writing the code that calculates the answer and sending it to the tool for execution. The results of that execution are returned to the model, which can now proceed to the following step: presenting the final output to the user."**
    - **Plain-English:** Two-step flow: model writes code → tool executes → results return → model presents final answer.
    - **Technical terms:** trajectory = the write-execute-answer sequence.

24. **Code (profit computation written by the model):**
    ```python
    original_units = 5000
    original_price = 29
    cost_per_unit = 14
    original_revenue = original_units * original_price
    original_cost_of_goods = original_units * cost_per_unit
    original_profit = original_revenue - original_cost_of_goods
    print(f'{original_profit=}')
    # New Scenario: price cut 10%, volume lift 10%
    new_price = original_price * (1 - 0.10)
    new_units = original_units * (1 + 0.10)
    new_revenue = new_units * new_price
    new_cost_of_goods = new_units * cost_per_unit
    new_profit = new_revenue - new_cost_of_goods
    print(f'{new_profit=}')
    change_in_profit = new_profit - original_profit
    print(f'{change_in_profit=}')
    # Output: original_profit=75000, new_profit=66550.0, change_in_profit=-8450.0
    ```
    - **Plain-English:** The model computed profits and the change: a 10% price cut + 10% volume lift decreases monthly profit by $8,450.
    - **Technical terms:** code execution = running the generated script.

25. **"What happens in the background here is that OpenAI creates a container to run this code and charges us for it. So executing this trajectory carries LLM inference cost (metered in tokens) and a single container (charged per session)."**
    - **Plain-English:** Costs: token-based inference + per-session container.
    - **Word meanings:** *metered* = billed by usage.
    - **Technical terms:** container = isolated runtime for the sandbox.

26. **"There are other options for code execution even if we want to continue using the same model. We can use hosted sandbox services from other commercial providers. Examples include Modal, Daytona, E2B, and Together Code Sandbox. These services provide an SDK or API that receives the code our agent wants to execute, and they return the code execution result while hiding the complexity of environment management. It's also possible to roll our own code interpreter: by using open source tools such as Open Interpreter or the e2b repo, we can configure Python sandboxes that execute the agent's code in an isolated container we control, which is the right level of effort when we have specific security or networking requirements that the hosted services don't meet."**
    - **Plain-English:** Alternatives: hosted sandbox services (Modal, Daytona, E2B, Together Code Sandbox) or self-hosted interpreters (Open Interpreter, e2b) for specific security needs.
    - **Technical terms:** SDK = software development kit; hosted vs self-hosted sandboxes.

27. **Command-line tools: "While code interpreters are restricted for security reasons, the next step up is giving the model more power to access the command line and utilize the powerful tools it has, granting it more control of a computer or dedicated virtual machine. Models need this level of access when they have to inspect or change the environment, for example, to install Python packages that aren't part of the code interpreter image. Software engineering agents such as Cursor or Claude Code also tend to require some command-line access to do their work: configuring environments, running Python scripts and unit tests, and much more."**
    - **Plain-English:** Command-line access gives agents more control (installing packages, configuring environments) — needed by Cursor/Claude Code.
    - **Technical terms:** command-line tool = shell access; environment configuration.

28. **"Needless to say that granting an LLM unfettered access to the command line can become a security nightmare quickly. Recent research highlights just how critical it is to treat the execution environment of coding agents as an adversarial surface rather than a benign and trusted backend. For example, 'RedCode: Risky Code Execution and Generation Benchmark for Code Agents' (2024) presents a benchmark of code prompts that probe agents for dangerous behaviors like file deletion, privilege escalation, or network misuse."**
    - **Plain-English:** Command-line access is dangerous; treat the execution environment as adversarial. RedCode probes dangerous behaviors.
    - **Word meanings:** *unfettered* = unrestricted; *benign* = harmless.
    - **Technical terms:** adversarial surface = area where attacks can happen; privilege escalation = gaining more rights.

29. **"Another work, 'SandboxEval: Towards Securing Test Environment for Untrusted Code' (2025), outlines security-relevant properties such as 1) exposing system, directory, and metadata information, 2) manipulating structures, contents, and privileges of filesystems, and 3) initiating external communication and dangerous operations."**
    - **Plain-English:** SandboxEval lists three threat categories: info exposure, filesystem manipulation, external communication.
    - **Technical terms:** metadata = data about data.

30. **"More recent work moves from the sandbox to the personal-agent systems became popular in 2026. 'Your Agent, Their Asset: A Real-World Safety Analysis of OpenClaw' (2026) runs the first real-world safety evaluation of such an agent and finds that poisoning any single dimension of its persistent state, its capabilities, identity, or knowledge, raises attack success rates from roughly 25% to 64% to 74%, concluding the exposure is inherent in the architecture instead of a fixable bug."**
    - **Plain-English:** The OpenClaw study found poisoning persistent state (capabilities, identity, knowledge) raises attack success from ~25% to 64–74%; exposure is architectural, not fixable.
    - **Technical terms:** persistent state = long-lived agent memory/identity.

31. **"These threats are one reason software agents in IDEs ask the user to approve each command before running it. The developer is the ultimate decision-maker here, bearing the responsibility of permitting actions case by case. That model works when the person approving has the expertise to judge each command, as a developer in an IDE usually does, but it offers little real protection to the non-technical users we met earlier in the chapter, who can't evaluate what a given command will do."**
    - **Plain-English:** IDE agents require per-command approval; good for expert developers but weak protection for non-experts.
    - **Technical terms:** approval flow = human-in-the-loop permission.

32. **"Threats could come from a malicious user or from well-intentioned but naive model actions that cause data loss. The safeguard is therefore mostly the agent builder's responsibility, applied at design time: follow the principle of least privilege by granting the agent only the permissions and data access its task requires, and scope the tasks and workflows you assign it so that a wrong action stays contained. This is a choice the builder makes up front, not something the agent decides for itself."**
    - **Plain-English:** Builders must apply least privilege and scope tasks at design time.
    - **Technical terms:** least privilege = minimal permissions needed.

33. **Computer use tools: "Computer use tools are analogous to command-line tools that operate on a virtual machine, but they can interact with the UI of the operating system and feed screenshots of the UI to the vision-language model operating the computer as a key source of information. This can be handy for cases when we want the agent to operate applications like Microsoft Excel, for example, or for cases when the agent is building UIs and needs to verify how they are actually rendered in a specific browser at a specific window size."**
    - **Plain-English:** Computer use tools operate the OS UI, feeding screenshots to a vision-language model (Excel, UI verification).
    - **Technical terms:** vision-language model = multimodal model reading screenshots.

34. **Code search: "Advanced software engineering agents can benefit from more powerful code retrieval capabilities. One example here is a tool like ast-grep, which can do more useful pattern matching on code than a tool like grep alone can. Because it operates on the code abstract syntax tree (AST), rather than raw text, it operates on code structure instead of surface representation, allowing it to find patterns while ignoring formatting, variable names, and whitespace."**
    - **Plain-English:** ast-grep matches on code structure (AST), ignoring formatting/names/whitespace.
    - **Technical terms:** AST = abstract syntax tree.

35. **"Code semantic search is another way to give a code agent more powerful retrieval capabilities. By searching using dense embeddings generated by a code-embedding model, a code agent can ask a query such as 'Find me a code snippet that iterates over a list' and get back the most relevant candidates. One benchmark of note that measures code retrieval is CoIR (Code Information Retrieval Benchmark), which also maintains a leaderboard comparing the capabilities of various models."**
    - **Plain-English:** Semantic search uses embeddings to find relevant code; CoIR benchmarks it.
    - **Technical terms:** dense embeddings; CoIR leaderboard.

36. **SQL tool: "An organization's most important data is often stored within a database, and one of the most useful actions an agent can do is to retrieve the relevant data for a specific task or scenario... databases are queried via SQL code that code LLMs can write. Whether the data resides in a single database or across multiple, a coding agent can leverage the context it has to write exceedingly complex SQL statements to retrieve data even if it requires more than a single query."**
    - **Plain-English:** SQL tools let agents pull data, even via complex multi-query sequences.
    - **Technical terms:** multi-query flow = multiple SQL statements building intermediate results.

37. **"Managing the context of a SQL tool is its own area of research that taps into established works of schema linking in database research. And while simpler SQL agents can work by providing a general database schema describing the tables and columns in the database, more advanced agents can be empowered by a more advanced semantic layer that identifies the relevant columns but might also be able to index some of the relevant data inside the tables and the actual language that users tend to use, which might be different from what's actually stored inside the database."**
    - **Plain-English:** Schema linking and semantic layers help SQL agents map user language to database structure.
    - **Technical terms:** schema linking; semantic layer.

38. **"An example of this is if a user asks of an SQL agent of a fashion ecommerce shop: show me all outerwear products. In the database the categories listed include jackets, blazers, and coats, but not explicit 'outerwear.'"**
    - **Plain-English:** The semantic layer bridges user terms ("outerwear") to DB values (jackets, blazers, coats).
    - **Technical terms:** terminology mapping.

39. **"More recently, 'Arming Data Agents with Tribal Knowledge' (2026) augments a SQL agent with the institutional knowledge of what columns really mean and how users phrase requests, accumulated from the agent's own query mistakes instead of from a hand-built semantic layer."**
    - **Plain-English:** Learn institutional knowledge from the agent's own query mistakes.
    - **Word meanings:** *tribal knowledge* = unwritten institutional know-how.
    - **Technical terms:** self-learned semantic layer.

### Context Management

40. **"As with other agents, when environments grow, we need to get around the constraints of the LLM context window on the one hand, and the latency and cost of utilizing very long contexts even when they're available."**
    - **Plain-English:** Manage context window limits and long-context cost/latency.
    - **Technical terms:** context window = max input tokens.

41. **"Even though the user asks the agent for just one thing, as agent builders we add a great deal more to the context. Alongside the usual system and developer prompts, agent persona, and tool descriptions, a software engineering agent's context often includes a section on desired security behaviors and a description of the repository it's working in... Much of this context is static and repeats on every call as the agent works through a task over many steps."**
    - **Plain-English:** Builders add security behaviors + repo description; much context is static and repeated.
    - **Word meanings:** *static* = unchanging.

42. **"That repetition is exactly what makes caching pay off. If we organize the context carefully, we can get dramatic savings in latency and cost, something you'll see called kv-caching, prompt-caching, or prefix-caching. The idea... was introduced in Chapter 2. Caching lets the model reuse the calculations it has already run over the input prefix instead of recomputing them at each step."**
    - **Plain-English:** Repetitive static context makes caching pay off (kv/prompt/prefix-caching).
    - **Technical terms:** prefix-caching = reusing prefix computations.

43. **"As Figure 10-9 shows, even the first call to a software engineering agent can run to tens of thousands of tokens, and parts of it repeat in every subsequent call as the agent works across multiple, sometimes dozens, of steps. The context assembled for a software engineering agent's call. The agent persona, security policy, repository summary, and task description are packed together and sent to the coding LLM, which responds with a thought, plan, and tool call. The tool's response then feeds back in for the next step."**
    - **Plain-English:** Calls can be tens of thousands of tokens; persona/security/repo/task pack the prompt; tool responses feed back.
    - **Technical terms:** agent loop = LLM call → tool call → observation.

44. **"Note that for the cache to work, two things need to happen: 1) the cached input needs to remain identical – we can't even change a single token in existing methods, and 2) all the new input needs to be appended to the end of the cached input."**
    - **Plain-English:** Cache requires an identical prefix; new input must append at the end.
    - **Technical terms:** cache key = exact token prefix.

45. **"By the third LLM call (step 5), the entire context accumulated over the earlier steps is served from cache as a single block, and only the new output is computed. Letting the cache grow to cover the whole prior trajectory is what maximizes the cache hit rate. If you guessed the entire input of step 2, then you're correct."**
    - **Plain-English:** Best practice: let the cache grow with the trajectory to maximize hit rate.
    - **Technical terms:** cache hit rate = fraction of tokens served from cache.

46. **The repository map: "Representing the structure of a repository is essential context for plenty of software engineering agents. Key components of a repository map are file and directory structure at some level of resolution and some additional information about all or a selection of relevant files. One popular tool to generate a repository map is Gitingest, which can take the URL of a GitHub repo and generate a repo map that includes the directory list as well as the contents of short files."**
    - **Plain-English:** Repo maps = structure + file summaries; Gitingest generates them from a GitHub URL.
    - **Technical terms:** repository map = structured summary of a repo.

47. **"In a blog post, the creators of Aider, one of the early command-line code agents and a clear pre-cursor to tools like Claude Code, describe a more thoughtful approach to generating file summaries, which relies on the abstract syntax tree to identify important parts like class and function signatures, and using these as summaries of the files. They additionally employ a follow-up step of ranking and filtering for only the most relevant definitions that are used the most in other parts of the repository."**
    - **Plain-English:** Aider summarizes files via AST signatures, then ranks/filters the most relevant definitions.
    - **Technical terms:** Aider = early CLI code agent; signature = function/class declaration.

48. **"One final repo description method that's relevant to know, especially when we can expend additional LLM calls for a certain task, is to use an LLM to summarize key or relevant files... 'CodeMonkeys: Scaling Test-Time Compute for Software Engineering.' Their efficiency comes from amortization: because the system already samples many candidate solutions per issue, it can afford to have an LLM read every file in the repository to find the relevant ones, spreading that one-time cost across all the downstream samples so it accounts for only about 15% of the total budget. The identified files are then ranked and trimmed to fit the context used in later steps."**
    - **Plain-English:** CodeMonkeys uses an LLM to read all files, amortizing the cost across many samples (~15% of budget), then ranks/trims.
    - **Technical terms:** amortization = spreading a one-time cost; test-time compute.

49. **Context compaction: "In Chapter 4, we saw how summarizing the context of a long conversation is one form of short-term memory that allows us to shrink the context to continue a conversation. In different specialized agents, the information that is required in that summary often needs to be optimized for that domain. For code agents, that may include information on the environment, code style preferences, preferences for when and how to run tests and when to commit the repository to source control."**
    - **Plain-English:** Compaction = summarizing context; for code agents include environment, code style, test/commit preferences.
    - **Technical terms:** compaction = context summarization.

50. **"Viewed together with what we've covered in cache optimization, we may find a pattern where we can keep different sections of the trajectory more static to optimize the cache hit rate. In Figure 10-13, we can see one example breakdown of different sections that change at different frequencies. The static section stays put, the stable section shifts now and then, and only the dynamic section turns over step to step. After compaction, the dynamic section shrinks while everything above it can keep getting served from cache."**
    - **Plain-English:** Organize trajectory into static/stable/dynamic sections; compaction shrinks the dynamic section.
    - **Technical terms:** static/stable/dynamic sections.

### Code Agents for Software Engineering

51. **"When the end user is a software engineer, code agents take on a completely different nature. For most developers now, their first encounter with a code agent is inside their IDE or code editor. These tools started by surfacing LLM code generation features such as autocompletion, which can be handy in writing documentation or writing a first draft of a function if we write the function signature first. Eventually, agents started offering the capability to solve more complex problems that would require editing code in multiple places to solve more challenging problems than what autocomplete can do on its own."**
    - **Plain-English:** Devs meet agents in IDEs; from autocompletion to multi-file editing.
    - **Technical terms:** autocompletion; multi-file edits.

52. **"The first generations of agents needed to be guided closely because this task took on a different nature that's not as well represented in their training data. Early systems compensated with heavy external scaffolding. The original SWE-agent, for instance, introduced a purpose-built Agent-Computer Interface, a constrained set of commands designed to make a repository navigable for a model that couldn't reliably do it on its own. Newer agents rely far less on this kind of handholding because the models powering them are now trained directly on tool use, multi-step trajectories, and software engineering tasks."**
    - **Plain-English:** Early agents needed scaffolding (SWE-agent's Agent-Computer Interface); newer models are trained for tool use directly.
    - **Technical terms:** scaffolding = external structure helping the agent; Agent-Computer Interface (ACI).

53. **"Going from writing function-level code to creating or making changes to entire repositories requires a different set of skills, even if it's all about writing code in the end. A code agent needs software engineering skills, which include skills like environment management, unit testing, debugging and troubleshooting, and the ability to reproduce bugs and investigate what causes them, which entails reading enough of the right places of the repository to understand the intention and flow of the program."**
    - **Plain-English:** Real SE agents need environment management, testing, debugging, and reproducing bugs.
    - **Technical terms:** unit testing; reproduce bugs.

54. **SWE-bench: "The SWE-bench benchmark was the first widely adopted code agent benchmark. It is a collection of hundreds of software engineering issues collected from open source repositories and actual issues reported by users. In each example, a single GitHub issue is presented to the code agent alongside a snapshot of the repository at a specific point in time. The model then has to resolve that issue in a way that passes a set of test suites that verify the correctness of the fix."**
    - **Plain-English:** SWE-bench = hundreds of real GitHub issues + repo snapshots; fix must pass hidden test suites.
    - **Technical terms:** SWE-bench; test suite.

55. **"SWE-bench caught on because it measures something real: the issues come from actual open source projects, and each one ships with the repository's own test suite, so a fix can be graded objectively by running the tests rather than judged subjectively. That verifiability, the same property that makes code a tractable target for LLMs in the first place, together with the fact that early systems solved only a small fraction of the issues and so left plenty of headroom, is what turned it into the number labs compete on."**
    - **Plain-English:** SWE-bench measures something real and objectively gradable; early low scores left headroom.
    - **Word meanings:** *headroom* = room for improvement.

56. **Planning for software engineering agents: "Because agents tackle larger scale problems that often require multiple steps, the planning step emerged as a key component of these systems. A coding agent often has to not only write a detailed plan with concrete steps but also keep returning to the plan and updating its status or even the entire plan as it learns more about the task. The UIs for code agents started showing plans and to-do-list items as distinct elements so the user keeps track of them."**
    - **Plain-English:** Planning is key; plans get updated; UIs surface plans as to-do lists.
    - **Technical terms:** plan/todo surfacing = separate UI element.

57. **"Writing a plan and then executing each part of it seems like two different tasks that can be handled by two different prompts, or even different agents. So some agentic code systems would assign the planning step to a dedicated planning agent and then some or all steps to dedicated code agents to tackle the appropriate steps. But even in a single agent system, the plan is one of the most important components of resolving the task. With that importance, it's often a good idea to assign it to the best available model and to have it think on it for a reasonable amount of time for the task."**
    - **Plain-English:** Planning can be a dedicated agent; even in single agents, give planning the best model and time.
    - **Technical terms:** dedicated planning agent.

58. **"When designing a planning prompt or agent, we have to bear in mind that some tasks require a bit of investigation to know exactly how to solve the task. This is especially the case for software tasks, which often require exploration and research phases or asking the user for clarification or more information. So, it's important to not assume that the approach to solving a problem is crystal clear at planning time. The plan likely needs to include exploration and investigation steps and have some flexibility based on the findings from these steps."**
    - **Plain-English:** Plans must include exploration/investigation and flexibility; don't assume clarity up front.
    - **Technical terms:** exploration phase = investigating before solving.

59. **Task-Specific Workflows: "There are often scenarios in which a problem is simpler to tackle with a static workflow than with an agent. A static workflow runs through a fixed set of steps you lay out in advance, rather than letting the model decide what to do at each turn. Plenty of routine tasks are simple enough that reaching for an agent is overkill. A proficient agent developer keeps other tools within reach and picks the one that fits the problem, rather than shoehorning an agent into every job."**
    - **Plain-English:** Use static workflows for routine tasks; don't force an agent into everything.
    - **Word meanings:** *overkill* = more than needed; *shoehorning* = forcing inappropriately.

60. **Agentless: "For software engineering tasks, the Agentless approach stands out for its simplicity and ability to compete with agentic approaches via a simple three-phase process... localize the relevant code, generate several candidate patches, then pick the best using generated unit tests. In a way, the Agentless approach works much like a Retrieval-Augmented Generation (RAG) system. It starts with a search step, finding all the code snippets in the repository that are relevant to the issue we want to fix. This is localization. It then hands those snippets and the issue to a coding LLM that generates a candidate solution. This is repair."**
    - **Plain-English:** Agentless = three fixed phases: localize, repair, pick best via generated tests — RAG-like.
    - **Technical terms:** Agentless; RAG; localization; repair.

61. **"The method improves the chances of resolving the bug by sampling multiple solutions from the LLM, so it has multiple attempts to solve the issue. But then how do we select the best solution candidate? Another powerful idea here is that coding LLMs can also be great at generating unit tests. So, we can present the issue to the LLM and ask it to generate a suite of unit tests to validate solutions for this bug. These generated unit tests can then be used to rank and choose the best solutions. Because the unit tests are themselves generated, and we're not fully confident in their quality, the selected solution does not need to pass all of them. It just needs to pass more than the other candidates do."**
    - **Plain-English:** Sample multiple patches; generate unit tests to rank them; best needs to pass more tests than rivals, not all.
    - **Technical terms:** sampling; generated unit tests as ranking signal.

62. **"We do want to re-highlight these two ideas because they share an advanced concept around using LLMs: we can use the probabilistic nature of LLMs to our advantage. One way is by sampling multiple possible solutions (at temperature values that allow some reasonable variety), and the second is by using generated unit tests as a ranking signal and not a pass/fail signal. These are powerful ideas that belong to the agent builder's toolkit."**
    - **Plain-English:** Two toolkit ideas: sample multiple solutions (with temperature variety) and use generated tests as ranking, not pass/fail.
    - **Technical terms:** sampling; temperature; ranking signal.

### TinyAgent with Code Tools

63. **"A core part of the coding agent is the tools it uses, and these are what tie an LLM to a software system. We covered several categories, but arguably the most common ones are: File manipulation tools — This lets the agent read, list, and write files. Code interpreter — This lets the agent execute code it writes."**
    - **Plain-English:** TinyAgent coding capabilities = file tools + code interpreter.
    - **Technical terms:** file tools; code interpreter.

64. **"As we saw in Chapter 5, these tools all return a string whether they succeed or not. This output can then be used in the messages structure as an observation to indicate whether the tool call succeeded. Some of the tools, such as execute_python and write_file, are not safe by default. If we were to give your TinyAgent complete freedom, it might corrupt files or take dangerous actions. Instead, we'll use the requires_approval parameter that we explored in Chapter 5. Before execution, you'll be asked to reply with 'Y' (yes) or 'N' (no), so you can inspect whether you believe the action is safe."**
    - **Plain-English:** Tools return string observations; write_file/execute_python need human approval (Y/N).
    - **Technical terms:** requires_approval = human-in-the-loop gate.

65. **Code (coding tools):**
    ```python
    import subprocess
    import sys
    from pathlib import Path


    def read_file(path: str) -> str:
        """Read a file's contents."""
        target = Path(path)
        if not target.exists():
            return f"Error: '{path}' not found."
        return target.read_text(encoding="utf-8")


    def list_files(directory: str = ".") -> str:
        """List files in a directory."""
        target = Path(directory)
        if not target.is_dir():
            return f"Error: '{directory}' is not a directory."
        entries = sorted(target.iterdir())
        return "\n".join(p.name + ("/" if p.is_dir() else "") for p in entries) or "(empty)"


    def write_file(path: str, content: str) -> str:
        """Write content to a file."""
        target = Path(path)
        target.parent.mkdir(parents=True, exist_ok=True)
        target.write_text(content, encoding="utf-8")
        return f"Written to '{path}'."


    def execute_python(code: str) -> str:
        """Execute Python code and return output."""
        try:
            result = subprocess.run(
                [sys.executable, "-c", code],
                capture_output=True,
                text=True,
                timeout=30,
            )
        except subprocess.TimeoutExpired:
            return "Error: Code execution timed out (30s limit)."
        if result.returncode != 0:
            return (
                f"Exit code {result.returncode}\n"
                f"STDOUT:\n{result.stdout}\n"
                f"STDERR:\n{result.stderr}"
            ).strip()
        return result.stdout.strip() or "(no output)"
    ```
    - **Plain-English:** Four tools: read a file, list a directory, write a file, and execute Python in a subprocess (30s timeout).
    - **Technical terms:** subprocess = running code in a separate process.

66. **Code (registering tools with approval):**
    ```python
    tools = NativeTools(requires_approval=["write_file", "execute_python"])
    tools.add_tool("read_file", read_file)
    tools.add_tool("list_files", list_files)
    tools.add_tool("write_file", write_file)
    tools.add_tool("execute_python", execute_python)
    ```
    - **Plain-English:** Register the four tools; require approval for write_file and execute_python.
    - **Technical terms:** tool registry; approval-gated tools.

67. **"With capabilities to write and execute code, this is already sufficient to turn your TinyAgent into a basic coding agent! It can now be run with agent.run as we did before to execute certain tasks. This, however, is not an ideal interface. As we discussed previously, more advanced interfaces like Cursor and agents that live in the terminal are common among coding solutions. As such, let's implement an interface ourselves such that your TinyAgent can be used anywhere in the terminal."**
    - **Plain-English:** The agent can now code, but a terminal interface is nicer.
    - **Technical terms:** terminal interface = CLI.

68. **"The first thing we'll have to do is create a Display class. This class will be used in your TinyAgent to display what the agent is doing at any given moment. We separate the steps in the ReAct loop as we have done before (with the color coding you're familiar with): THOUGHT — The reasoning of the agent; ACTION — The tool the agent wants to use; OBSERVATION — The output of the tool use; ANSWER — The final answer of the agent. The Display class uses ANSI escape codes (e.g., '\033[35m') to color the output. Assigning a distinct color to each phase of the ReAct loop makes the agent's reasoning, tool executions, and final answers easily readable and scannable at a glance."**
    - **Plain-English:** A Display class color-codes THOUGHT/ACTION/OBSERVATION/ANSWER via ANSI escape codes.
    - **Technical terms:** ANSI escape codes = terminal color codes.

69. **Code (Display class):**
    ```python
    BOLD = "\033[1m"
    RESET = "\033[0m"
    GREEN = "\033[32m"   # THOUGHT
    RED = "\033[31m"     # ACTION
    YELLOW = "\033[33m"  # OBSERVATION
    PURPLE = "\033[35m"  # ANSWER


    class Display:
        """Chat interface with styling."""

        def __call__(self, event: str, data: str | Response = None) -> None:
            if event == "thinking":
                print(f"  Thinking...\n{RESET}")
            elif event == "response":
                print(f"{BOLD}{GREEN}{'▒▒ THOUGHT ▒▒':<13}{RESET}")
                print(f"{data.reasoning}{RESET}\n")
                if data.content:
                    print(f"{BOLD}{PURPLE}{'▒▒ ANSWER ▒▒':<13}{RESET}")
                    print(f"{data.content}{RESET}\n")
            elif event == "tool_call" and data:
                tool = data.tool_call["tool"]
                kwargs = data.tool_call["kwargs"]
                print(f"{BOLD}{RED}{'▒▒ ACTION ▒▒':<13}{RESET}")
                print(f"{tool}({kwargs}){RESET}\n")
            elif event == "observation":
                print(f"{BOLD}{YELLOW}{'▒▒ OBSERVATION ▒▒':<13}{RESET}")
                print(f"{data}{RESET}\n")
                print(f"{'─' * 40}STEP{'─' * 40}{RESET}\n")
    ```
    - **Plain-English:** The Display prints styled sections per ReAct event type.
    - **Technical terms:** event-driven display.

70. **"The Display now needs to be integrated into your TinyAgent. This is, fortunately, a straightforward process since we need to add only self.display(...) where we want to track a certain behavior."**
    - **Plain-English:** Add self.display(...) calls at tracked points.
    - **Technical terms:** instrumentation = adding display hooks.

71. **Code (TinyAgent with Display):**
    ```python
    class TinyAgent:
        """A minimal, modular, and educational agent framework."""

        def __init__(self, llm, memory, tools, planner, display):
            self.llm = llm
            self.memory = memory
            self.tools = tools
            self.planner = planner
            self.display = display
            self.trajectory = Trajectory()
            system_prompt = "You are a helpful assistant.\n\n"
            system_prompt += self.planner.prompt
            system_prompt += self.tools.prompt
            self.memory.add("system", system_prompt)

        def run(self, task: str, image_data: str = None) -> str:
            self.memory.add("user", task, image_data=image_data)
            self.trajectory.initialize(task)
            for step in range(self.planner.max_steps):
                result = self._step()
                if result is not None:
                    return result
            return "Max steps reached without completion."

        def _step(self) -> str | None:
            self.display("thinking")
            response = self.llm.generate(
                self.memory.get_messages(), tools=self.tools.schemas
            )
            self.memory.add("assistant", response.content, tool_call=response.tool_call)
            self.display("response", response)
            response = self.planner.parse(response)
            response = self.tools.parse(response)
            if self.tools.is_done(response):
                self.trajectory.add(response)
                return response.content
            return self._execute_action(response)

        def _execute_action(self, response: Response) -> None:
            self.display("tool_call", response)
            result = self.tools.execute(response)
            role, observation = self.tools.observation(result)
            self.memory.add(role, observation)
            self.trajectory.add(response, observation)
            self.display("observation", observation)
            return None
    ```
    - **Plain-English:** TinyAgent now takes a display and emits events for each ReAct phase.
    - **Technical terms:** autonomy loop; max_steps.

72. **"All code we have created thus far lives in the illustrated-agents package, and because it's pure Python, you can run your coding agent like so... Running this will give you a nice interface to work with, as shown in Figure 10-17. As you can see, when you give it a task, the separation between THOUGHT/ACTION/OBSERVATION will be shown. This will give you an intuitive understanding of what's under the hood."**
    - **Plain-English:** Run the coding agent from the terminal; it shows THOUGHT/ACTION/OBSERVATION per step.
    - **Technical terms:** CLI entry point.

73. **"Likewise, the LLM makes a large difference. If the small 4 billion-parameter model doesn't suit your needs, try a larger one, or a model trained specifically for this kind of work. North Mini Code, for example, is an open mixture-of-experts model from Cohere built for agentic coding: 30 billion parameters total but only 3 billion active per token, released under Apache 2.0. It asks more of your hardware than Gemma, but it's purpose-built for the code-focused, multi-step, tool-using work your TinyAgent is now doing."**
    - **Plain-English:** The model matters; North Mini Code (Cohere) = 30B total/3B active MoE, Apache 2.0, built for agentic coding.
    - **Technical terms:** mixture-of-experts (MoE); active parameters per token.

74. **"Congratulations! You have created a coding agent in the CLI entirely from scratch and in pure Python! As you experiment, you'll find the biggest lever on your agent's behavior is the model behind it. That's where we turn next: what it takes to train an LLM capable of powering a coding agent."**
    - **Plain-English:** A CLI coding agent built from scratch; next: training the models.
    - **Technical terms:** model choice = the biggest lever.

### Building Code LLMs

75. **"The quality of a coding agent is limited, in large part, by the quality of the model powering it. Not only must the model exhibit high performance on writing code, but it should also be skilled at tool use and agentic software engineering tasks for it to be capable of powering a coding agent."**
    - **Plain-English:** The model must be good at code AND tool use/agentic SE tasks.
    - **Technical terms:** agentic tasks = tool-using multi-step work.

76. **Figure 10-18: "The stages that turn an untrained model into one capable of powering a coding agent. Language modeling during pre-training produces a base model, supervised fine-tuning makes it instruction-tuned, and reinforcement learning gives the trained model the behaviors that agentic work depends on. The same recipe applies whether the end goal is a generalist LLM or a code-focused one."**
    - **Plain-English:** Recipe: pre-training → SFT → RL; same for generalist or code LLMs.
    - **Technical terms:** pre-training; SFT; RL.

77. **Data composition: "These models have to see a lot more code data in the various training stages. Qwen3 Coder, for example, is pre-trained on 7.5 trillion tokens, 70% of which is code."**
    - **Plain-English:** Code models need heavy code data; Qwen3 Coder = 7.5T tokens, 70% code.
    - **Technical terms:** token budget; data composition.

78. **Coding task data: "To better serve as engines that power coding agents, coding LLMs need to be trained on data that looks like what the model will see when it's in use, which includes: Tool-use data — Trajectories where the model calls a tool and uses the result, like running grep to locate a function and then acting on what it finds; Multi-step data — Tasks that play out over several turns, like reproducing a bug, editing a file, running the tests, reading the failure, and patching again; Unit-test data — Code paired with the tests that verify it, like a function alongside the pytest cases that exercise it; Software engineering tasks — Repository-level issues and the changes that resolve them, like a GitHub issue paired with the diff that closes it."**
    - **Plain-English:** Training data must match real use: tool-use, multi-step, unit-test, and SE-task data.
    - **Technical terms:** tool-use data; multi-step data; unit-test data; diff.

79. **Reinforcement learning: "Now a major component of the training flow. Where in the past Reinforcement Learning from Human Feedback (RLHF) had been useful to polish the behavior of a model, coding LLMs are trained with large-scale Reinforcement Learning with Verifiable Rewards (RLVR) as we covered in Chapter 2."**
    - **Plain-English:** Coding LLMs use RLVR (verifiable rewards) rather than RLHF.
    - **Technical terms:** RLHF; RLVR = verifiable reward signal.

80. **"While coding data is important, other capabilities are just as important for an LLM to successfully empower a coding agent. These include reasoning, multi-step tool calling, long-context, and planning, as well as trajectories that demonstrate proper software engineering practices (how to reproduce issues, how to use coding tools and common libraries…etc.)."**
    - **Plain-English:** Also need reasoning, multi-step tool calling, long-context, planning, and SE practices.
    - **Technical terms:** long-context capability.

81. **"For a sense of how much data each stage demands, open source model GLM-5's pipeline is a useful current example (Figure 10-20): pre-training on the order of 28 trillion tokens, mid-training that extends context out to 200K with long code and agent data, SFT, and then RL split into reasoning, agentic, and general stages before a final distillation step."**
    - **Plain-English:** GLM-5: ~28T pre-training tokens, 200K context mid-training, SFT, RL (reasoning/agentic/general), distillation.
    - **Technical terms:** mid-training; context extension; distillation.

82. **Bootstrapping Intelligence: The Synthetic Data Feedback Loop: "A key inflection point that many outside the research industry may not know is that around 2024, LLMs became a major source of the training data for the next generation of LLMs. You start to see that more clearly with a lot of the Llama 3.1 post-training data actually having been generated with Llama 3. This repeats itself with math and code; Qwen2.5-Math and Qwen2.5-Coder were both essential in creating training data for Qwen3 and Qwen3-Coder. And as we saw in Chapter 3 with DeepSeek-R1, a lot of long chains of priceless reasoning traces were generated by DeepSeek-R1-Zero."**
    - **Plain-English:** Since ~2024, LLMs generate training data for the next generation (Llama 3 → 3.1, Qwen2.5 → Qwen3, DeepSeek-R1-Zero → R1).
    - **Word meanings:** *inflection point* = turning point.
    - **Technical terms:** synthetic data; feedback loop.

83. **"If you want a closer look at synthetic data generation, several open source projects share code and details on these pipelines. This includes Nvidia's Nemotron 3 Nano and Nemotron 2 Nano, AI21 'Scaling Text-Rich Image Understanding via Code-Guided Synthetic Multimodal Data Generation' (GitHub), and 'OmniSQL - Synthesizing High-quality Text-to-SQL Data at Scale' as some examples with scripts."**
    - **Plain-English:** Open-source synthetic-data projects: Nemotron 3/2 Nano, AI21 code-guided multimodal, OmniSQL.
    - **Technical terms:** synthetic data pipelines.

## Summary

84. **"In this chapter, we examined how code agents are among the most impactful categories of AI agents. We saw how they extend basic LLM code generation into multi-step problem solving by writing code, executing it, debugging failures, and iterating until a larger task is completed. We stressed how general users, not just software developers, stand to benefit, starting with a data analysis example in which a non-coder never has to see the code."**
    - **Plain-English:** Summary: code agents extend generation to multi-step problem solving; non-coders benefit.
    - **Technical terms:** iterative debugging.

85. **"We then broke down the core tool and environment design space that lets a code agent act on the world. We saw building blocks such as file manipulation tools, sandboxed code interpreters, and command-line access to dedicated virtual machines, along with the security trade-offs each one carries and the principle of least privilege that helps manage them. We also highlighted computer-use tools that let a model interact with GUIs, code search methods that go beyond text matching, and SQL tools that connect agents to organizational databases."**
    - **Plain-English:** Summary: tools (files, sandbox, CLI, computer-use, code search, SQL) + security trade-offs + least privilege.
    - **Technical terms:** tool/environment design space.

86. **"From there we turned to how software engineering agents manage their context, using prompt caching, repository maps, and context compaction to keep long trajectories affordable. We looked at what changes when the user is a software engineer instead of a non-coder, how benchmarks such as SWE-bench frame the task as resolving a real GitHub issue until its tests pass, and how planning, memory, and task-specific workflows like Agentless tackle that task, sometimes without a full agent at all."**
    - **Plain-English:** Summary: context management (caching, repo maps, compaction), SWE-bench, planning, Agentless.
    - **Technical terms:** context management; Agentless.

87. **"We then pulled these threads together and built a coding agent ourselves, extending the TinyAgent from earlier chapters with file and code-execution tools and a terminal interface so you could watch its thought, action, and observation loop run in your own environment. Finally, we went underneath the agent to the model that powers it. We outlined how a coding LLM is built across pre-training, SFT, and RL, why these models need far more code, tool-use, and software engineering data than a generalist, and how reinforcement learning with verifiable rewards turns the chapter's opening observation, that code can be checked by running it, into a training signal. We closed on the synthetic data feedback loop, where each generation of models increasingly trains the next."**
    - **Plain-English:** Summary: built a TinyAgent coding agent; covered LLM training (pre-training/SFT/RL, RLVR) and the synthetic data feedback loop.
    - **Technical terms:** verifiable rewards; synthetic data loop.

88. **Afterword: "You bought this book. That was the last transaction in it. Everything else inside arrived as a gift... A transaction is even: you pay, you receive, the books close, and you walk away owing no one. A gift is not even, and it never closes. It asks one thing of whomever receives it: that it keeps moving. Held still, hoarded or buried, it stops being a gift and begins to rot. This is not an exchange we are closing. It's a gift we're handing on..."**
    - **Plain-English:** The book's closing: knowledge is a gift, not a transaction — keep it moving.
    - **Word meanings:** *hoarded* = kept selfishly.

## Chapter 10 Key Takeaways
- Code agents extend LLM code generation into multi-step execution, debugging, and iteration.
- Users: non-code output seekers, vibe coders, software engineers; builders: agent builders and coding-LLM trainers.
- Tools: file manipulation (cat, sed/head/tail, ls, glob, grep), sandboxed interpreters (ephemeral, security-restricted; Gemini code execution; Modal/Daytona/E2B/Together; Open Interpreter), command-line tools (adversarial surface; RedCode, SandboxEval, OpenClaw; least privilege), computer-use tools (screenshots to VLM), code search (ast-grep/AST, semantic search, CoIR), SQL tools (schema linking, semantic layer, tribal knowledge).
- Context management: prefix caching (identical prefix + appended new input), repository maps (Gitingest, Aider AST, CodeMonkeys amortization), compaction (static/stable/dynamic sections).
- SE agents: IDE autocompletion → multi-file work; SWE-agent's Agent-Computer Interface; SWE-bench; planning (dedicated planner, exploration); Agentless (localize → repair by sampling → rank with generated tests); static workflows vs agents.
- TinyAgent coding agent: read_file/list_files/write_file/execute_python, requires_approval, Display (ANSI colors), CLI; North Mini Code (30B/3B MoE, Apache 2.0).
- Code LLM training: pre-training (Qwen3 Coder 7.5T/70% code) → SFT (tool-use/multi-step/unit-test/SE data) → RL (RLVR); GLM-5 (28T tokens, 200K context, distillation); synthetic data feedback loop (Llama 3.1, Qwen2.5, DeepSeek-R1-Zero).
