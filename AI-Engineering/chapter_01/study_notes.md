# Chapter 1 Study Bundle: Introduction to Building AI Applications with Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 1

---

## §1. Study Notes

### Core Theme
Chapter 1 explains **why AI engineering emerged** and **what it is**: the discipline of building applications on top of **foundation models** (models like GPT-4, Claude, Gemini that are trained by someone else and reused across many applications). The chapter traces a four-step evolution: (1) **language models** (statistical next-word predictors) → (2) **large language models** (LLMs, made trainable at massive scale by **self-supervision**, which infers labels from the input itself and removes the labeling bottleneck) → (3) **foundation models** (LLMs extended to other modalities — images, audio, video — and capable of many tasks out of the box) → (4) **AI engineering** (the fast-growing practice of adapting and evaluating these pretrained models for specific applications). The chapter then surveys **use cases**, the **planning** of AI applications (evaluate, build-or-buy, human role, defensibility, metrics, milestones, maintenance), and the **AI engineering stack** (application development, model development, infrastructure), contrasting AI engineering with ML engineering and full-stack engineering. The key message: with foundation models, the work shifts from *training models* to *adapting and evaluating models*.

### Key Definitions
- **AI engineering:** Building applications on top of foundation models. Also the fastest-growing discipline in computer science (the book's claim).
- **Language model:** A model that encodes statistical information about a language — how likely a word is to appear in a given context. Example: "My favorite color is ___" → "blue" is more likely than "car."
- **Token:** The basic unit of a language model's input/output: a character, a word, or a part of a word. GPT-4 breaks "I can't wait to build AI applications" into **nine tokens** ("can't" → "can" + "'t").
- **Tokenization:** The process of breaking text into tokens. On average a GPT-4 token is about ¾ the length of a word, so ~100 tokens ≈ 75 words.
- **Vocabulary:** The set of all tokens a model recognizes. Mixtral 8x7B has a 32,000-token vocabulary; GPT-4 has 100,256.
- **Masked language model:** Predicts missing tokens using context from both before and after (fill-in-the-blank); e.g., BERT (Devlin et al., 2018). Used for non-generative tasks like sentiment analysis and classification.
- **Autoregressive (causal) language model:** Predicts the next token using only preceding tokens; generates one token after another; the model of choice for text generation; the default meaning of "language model" in this book.
- **Completion:** What an autoregressive model outputs — a prediction of the most probable next tokens (not a guaranteed-correct answer). "To be or not to be" → ", that is the question."
- **Supervised learning:** Training using labeled data (input → output pairs); expensive and slow.
- **Self-supervision:** Training where the model infers labels from the input data itself (e.g., next-token prediction), removing the data-labeling bottleneck; the key to LLM scaling.
- **Unsupervised learning:** Learning without labels at all; distinct from self-supervision (which *does* derive labels, just from the input).
- **`<BOS>` / `<EOS>`:** Special tokens marking the beginning and end of a sequence; `<EOS>` is important for teaching a model to end its responses.
- **Parameter:** The weights (and biases) of a model that are learned during training; model size is measured by parameter count. GPT-1 (June 2018): 117M parameters; GPT-2 (Feb 2019): 1.5B; as of writing, ~100B counts as large.
- **Foundation model:** A large model that serves as the base for many applications; the book's umbrella term covering both LLMs (text) and LMMs (large multimodal models, text + images/video/audio, e.g., GPT-4V, Claude 3).
- **Embedding:** A vector capturing meaning; CLIP embeds texts and images into a shared vector space.
- **Prompt engineering:** Getting a model to express desirable behavior by adjusting the input only (instructions, context) without changing model weights.
- **RAG (retrieval-augmented generation):** Connecting a model to external data (e.g., a database of customer reviews) to provide context at inference time.
- **Finetuning:** Further training a pretrained model on a (usually smaller, domain-specific) dataset; changes model weights.
- **Agent:** An AI application that can plan and use external tools.
- **MLOps:** A portmanteau of machine learning and operations; the practice of building ML applications.
- **Model as a service:** Providing access to trained models via APIs (e.g., OpenAI API), one reason the barrier to AI application building dropped.
- **Ground truth:** The correct answer used to evaluate a model on close-ended tasks.
- **Pre-training:** Training from scratch with randomly initialized weights, on text completion; most resource-intensive; can use up to 98% of a project's total compute/data (InstructGPT).
- **Post-training:** Any training that happens after pre-training; conceptually the same as finetuning (the terms are used interchangeably); post-training is done by model developers (e.g., OpenAI post-trains models to follow instructions), finetuning by application developers.
- **Inference optimization:** Making models faster and cheaper to run; a core model-development responsibility.
- **Dataset engineering:** Curating, generating, and annotating training data.
- **AI interface:** The interface through which end users interact with a model (chat UI, browser extension, plugin, API, etc.).
- **TTFT / TPOT:** Time to first token / time per output token — latency metrics for measuring how fast a model responds.
- **MMLU:** Massive Multitask Language Understanding (Hendrycks et al., 2020), a benchmark used to compare models — central to the Gemini-vs-ChatGPT evaluation controversy in this chapter.

### Core Concepts & Frameworks
- **Four-step evolution (the chapter's spine):**
  1. **Language models** — statistical models of word probabilities (Sherlock Holmes's "Adventure of the Dancing Men," 1905; Claude Shannon's WWII cryptography and 1951 paper "Prediction and Entropy of Printed English").
  2. **Large language models** — made possible by **self-supervision** + **web-scale text data**.
  3. **Foundation models** — LLMs extended to other modalities (GPT-4V, Gemini, Claude 3); general-purpose rather than task-specific.
  4. **AI engineering** — the practice of building applications on these models.
- **Why tokens instead of words or characters:** (1) sub-word tokens break words into meaningful components ("cooking" → "cook"+"ing"); (2) fewer unique tokens than words → smaller vocabulary → more efficient; (3) handles unknown words ("chatgpting" → "chatgpt"+"ing"). A balance between the fewer units of words and the more meaning per unit of characters.
- **Self-supervision pipeline (Figure 1-2):** "I love street food." → 6 training samples (Table 1-1): with `<BOS>`/`<EOS>`, each position gives (context → next token) pairs:
  - `<BOS>` → I | `<BOS> I` → love | `<BOS> I love` → street | `<BOS> I love street` → food | `<BOS> I love street food` → `.` | `... food .` → `<EOS>`
  - Self-supervision differs from unsupervised learning: self-supervision *infers* labels from input; unsupervised learning needs *no* labels at all.
- **Why larger models need more data (counterintuitive):** greater capacity must be matched by more data to maximize performance; training a large model on a small dataset wastes compute.
- **Foundation models vs task-specific models:** Foundation models are general-purpose — good for many tasks out of the box (Super-NaturalInstructions benchmark, Wang et al., 2022). To make them great for a specific task, adapt with the **three common techniques**: prompt engineering (detailed instructions), RAG (connect to external data), finetuning (further train on high-quality data). Example: a retailer wants product descriptions in the brand voice — the out-of-the-box model may not capture it.
- **Adapting is cheaper than building:** ~10 examples + 1 weekend (adapt) vs ~1M examples + 6 months (build from scratch); task-specific models are smaller, faster, cheaper.
- **Three factors for AI engineering's explosive growth:**
  1. **General-purpose AI capabilities** — AI can now write as well as humans → can automate any task involving communication; synthesize training data; write code.
  2. **Increased AI investments** — Matt Ross (Scribd): AI cost dropped two orders of magnitude Apr 2022 → Apr 2023; Goldman Sachs: ~$100B US / $200B global by 2025; FactSet: 1 in 3 S&P 500 companies mentioned AI in Q2 2023 earnings calls (3× the year before); WallStreetZen: AI-mentioning stocks +4.6% vs +2.4%.
  3. **Low entrance barrier** — model-as-a-service APIs; AI writes your code; plain-English workflows.
- **The term "AI engineering":** Not ML engineering (though a superset relationship exists); not MLOps/AIOps/LLMOps (which emphasize operations). A survey of 20 practitioners building on foundation models favored "AI engineering."
- **Use cases (205 open-source AI apps with ≥500 GitHub stars, Table 1-3):** 8 groups — Coding; Image and video production; Writing; Education; Conversational bots; Information aggregation; Data organization; Workflow automation. Enterprises group uses into: customer experience, employee productivity, process optimization.
- **Eloundou et al., 2023 ("GPTs are GPTs"):** a task is "exposed" if AI reduces the time to do it by ≥50%; an occupation is 80% exposed if 80% of its tasks are exposed. High exposure: interpreters/translators, tax preparers, web designers, writers. No exposure: cooks, stonemasons, athletes. Human-annotated results (Table 1-2): interpreters/translators 76.5% (Human α), survey researchers 84.4% (Human β), mathematicians 100.0% (Human ζ); humans labeled 15 occupations "fully exposed."
- **Which tasks AI is good at:** programming (hands-down most popular — GitHub Copilot ARR crossed $100M two years after launch), image/video production (Midjourney $200M ARR at 1.5 years old), writing (MIT study: ChatGPT cut writing time 40%, raised quality 18% — Noy and Zhang, 2023), education (NYC Schools + LA Unified banned then un-banned ChatGPT; Chegg stock $28 → $2), conversational bots (companions, therapists, customer support, NPCs in games), information aggregation (Salesforce 2023: 74% of generative AI users summarize/distill), data organization (IDP market $12.81B by 2030, +32.9%/yr), workflow automation (booking, refunds, invoicing → agents).
- **AI product defensibility:** low entry barrier is both blessing and curse — a layer on top of a foundation model can be subsumed by the model or a competitor. Three competitive advantages: **technology, data, distribution**. Data is nuanced: first-mover + usage data creates a moat. Cautionary tales: Calendly (vs Google Calendar), Mailchimp (vs Gmail), Photoroom (vs Google Photos) — "features of a bigger product."
- **Planning an AI application (the checklist):**
  - **Use case evaluation** — existential threat (competitors with AI make you obsolete; 7% of 2,500 execs in Gartner 2023 cited business continuity) → build in-house; else buy.
  - **Role of AI and humans:** critical vs complementary (Face ID critical vs Gmail Smart Compose complementary — critical needs higher accuracy); reactive vs proactive (chatbot reactive, Google Maps traffic alerts proactive — proactive needs higher quality bar); dynamic vs static (ChatGPT memory dynamic, object detection in Google Photos static); human role = support, decisions, or both (Microsoft's Crawl-Walk-Run: crawl = human mandatory, walk = AI works internally, run = AI talks to external users directly).
  - **Setting expectations** — measure business metrics (chatbot: % messages automated, how many more processed, response speed, labor saved, customer satisfaction); technical metrics: quality, latency (TTFT, TPOT, total latency), cost per inference, other (interpretability, fairness). Median human response time ≈ 1 hour → anything faster is good enough.
  - **Milestone planning** — evaluate existing models early; goals change after evaluation; "the last mile is the hardest": UltraChat (Ding et al., 2023) — 0→60 easy, 60→100 exceedingly challenging; LinkedIn (2024): 1 month to 80%, 4 more months to beat 95%.
  - **Maintenance** — the field moves fast ("a bullet train"): context lengths grow, outputs improve, inference gets cheaper (Figure 1-11); today's best option is tomorrow's worst; prices halve; providers go out of business; regulations evolve (GDPR, US Oct 2023 Executive Order on compute).
- **The AI engineering stack (three layers, Figure 1-13):**
  - **Application development:** good prompts + necessary context (prompt engineering, RAG/agents) + rigorous evaluation + good interfaces. Most action in the last two years.
  - **Model development:** modeling/training, dataset engineering, inference optimization.
  - **Infrastructure:** model serving, data & compute management, monitoring.
- **AI engineering vs ML engineering — three major differences:**
  1. **Use pretrained models** — less modeling/training, more model **adaptation**.
  2. **Bigger, compute-heavy models** — more pressure for efficient training/inference optimization; GPU clusters (Fortune 500 head: teams know 10 GPUs, not 1,000).
  3. **Open-ended outputs** — harder to evaluate → **evaluation is a much bigger problem**.
  - In short: "less about model development, more about adapting and evaluating models."
- **Model adaptation taxonomy (by whether weights change):**
  - **Prompt-based (prompt engineering):** no weight updates; easier; less data; try many models cheaply; may not suffice for complex tasks or strict performance targets.
  - **Finetuning:** updates weights; more data & complexity; better quality, latency, cost; enables new tasks not exposed during training.
- **Model development responsibilities:** modeling & training (TensorFlow, PyTorch, Hugging Face Transformers; ML knowledge still valuable but no longer a must-have), dataset engineering (curation, generation, annotation — harder with open-ended outputs; data is the main differentiator since models are commodities; training from scratch > finetuning > prompt engineering in data needs), inference optimization (autoregressive generation is sequential: 10ms/token → 100 tokens = 1 second; quantization, distillation, parallelism — Ch 7–9).
- **Application development responsibilities:** evaluation (select models, benchmark progress, decide deployment readiness, detect issues), prompt engineering & context construction (Gemini's MMLU jumped from 83.7% to 90.04% with a prompt technique), AI interface (standalone apps, browser extensions, chatbots in Slack/Discord/WeChat/WhatsApp, APIs for plugins, voice, AR/VR; chat is the most common).
- **The Gemini evaluation controversy (Dec 2023):** Google claimed Gemini Ultra beat ChatGPT on MMLU using **CoT@32** (32 chain-of-thought examples) while GPT-4/ChatGPT were evaluated with **5 examples**; with equal settings (5-shot), ChatGPT performed better. Lesson: **evaluation settings must be comparable** — this is why evaluation is a first-class concern.
- **AI engineering vs full-stack engineering:** interfaces matter more → AI tooling is shifting toward JavaScript (LangChain.js, Transformers.js, OpenAI's Node library, Vercel's AI SDK); more AI engineers come from web/full-stack backgrounds; full-stack advantage = turn ideas into demos fast, get feedback, iterate. Traditional ML: start with data + training, product last. AI engineering: **build the product first, invest in data/models once the product shows promise** (Figure 1-16).

### Important Numbers / Stats / Tokens
- GPT-4 breaks "I can't wait to build AI applications" into **nine tokens**; "can't" → "can" + "'t".
- Average GPT-4 token ≈ ¾ of a word; **100 tokens ≈ 75 words**.
- Mixtral 8x7B vocabulary: **32,000** tokens; GPT-4 vocabulary: **100,256**.
- Sherlock Holmes "Adventure of the Dancing Men" (1905); Claude Shannon 1951 paper "Prediction and Entropy of Printed English."
- AlexNet (Krizhevsky et al., 2012): supervised; 1M ImageNet images, 1,000 categories.
- Labeling costs: 5 cents/image → $50,000 per 1M images; two labelers → double; 1M categories → ~$50M. Amazon SageMaker Ground Truth: 8 cents/image (<50k images), 2 cents/image (>1M images) (as of Sept 2024).
- Table 1-1: "I love street food." → 6 training samples (with `<BOS>`/`<EOS>`).
- GPT-1 (June 2018): 117M params; GPT-2 (Feb 2019): 1.5B params; as of writing ~100B = "large."
- CLIP (OpenAI): 400M (image, text) pairs from the web = 400× ImageNet, no manual labeling; first model to generalize across image classification tasks without additional training; embedding model (not generative).
- Goldman Sachs: ~$100B US, ~$200B global AI investment by 2025.
- FactSet: 1 in 3 S&P 500 companies mentioned AI in Q2 2023 earnings calls (3× the prior year).
- WallStreetZen: AI-mentioning companies' stocks +4.6% vs +2.4% average.
- Matt Ross (Scribd): AI cost down **two orders of magnitude** (Apr 2022 → Apr 2023).
- 4 open-source AI engineering tools (AutoGPT, Stable Diffusion web UI, LangChain, Ollama) amassed more GitHub stars in 2 years than Bitcoin; on track to surpass React and Vue.
- LinkedIn (Aug 2023): "Generative AI," "ChatGPT," "Prompt Engineering," "Prompt Crafting" additions up ~75%/month on average.
- theresanaiforthat.com (Sept 16, 2024): 16,814 AIs for 14,688 tasks and 4,803 jobs.
- AWS enterprise generative AI use cases: 3 buckets (customer experience, employee productivity, process optimization).
- O'Reilly 2024 survey: 8 categories (programming, data analysis, customer support, marketing copy, other copy, research, web design, art).
- Eloundou et al. (2023): task exposed if AI cuts time ≥50%; 80% exposed occupation = 80% of tasks exposed. Table 1-2: interpreters/translators 76.5% (α); survey researchers 84.4% (β); mathematicians 100.0% (ζ); 15 occupations "fully exposed"; no exposure: cooks, stonemasons, athletes.
- 205 open-source AI apps with ≥500 GitHub stars analyzed; 8 groups (Table 1-3).
- GitHub Copilot ARR crossed **$100M** two years after launch.
- Magic raised **$320M**; Anysphere **$60M** (both Aug 2024).
- gpt-engineer and screenshot-to-code reached **50,000** GitHub stars within a year.
- McKinsey: AI makes developers 2× as productive for documentation; 25–50% more productive for code generation/refactoring; minimal gains on highly complex tasks; AI better at frontend than backend.
- Midjourney: **$200M ARR at 1.5 years old**. Top 10 free Graphics & Design apps on Apple App Store (Dec 2023): half have AI in their names.
- Marketing = **11%** of company budget.
- MIT study (Noy and Zhang, 2023): 453 college-educated professionals; ChatGPT: time −40%, quality +18%; closed gaps between workers; 2× as likely to use it 2 weeks later, 1.6× after 2 months.
- Amazon flooded with AI-generated travel guidebooks (NYT, 2023); NewsGuard (June 2023): ~400 ads from 141 brands on junk AI websites; one site produced 1,200 articles/day.
- NYC Public Schools and LA Unified banned then reversed ChatGPT bans.
- Duolingo (Pajak & Bicknell, 2022): 4 stages of course creation; lesson personalization benefits most from AI.
- Chegg stock: $28 (Nov 2022) → $2 (Sept 2024).
- Salesforce 2023 Generative AI Snapshot: **74%** of generative AI users use it to distill/summarize complex ideas.
- IDP (intelligent data processing) market: **$12.81B by 2030**, growing **32.9%/year**.
- Gartner 2023: **7%** of 2,500 executives cited business continuity as AI motivation.
- Table 1-5 MMLU scores: Gemini Ultra **90.04%** CoT@32; Gemini Pro **79.13%** CoT@8; GPT-4 **87.29%** CoT@32 (via API); GPT-3.5 **70%** 5-shot; PaLM 2-L **78.4%** 5-shot; Claude 2 **78.5%** 5-shot (79.6% CoT); Inflection-2 **73.0%**; Grok 1 **68.0%**; Llama-2 **86.4%** (reported, 5-shot); Gemini Ultra 83.7% CoT / 71.8% 5-shot when reported by Google.
- InstructGPT: pre-training = up to **98%** of overall compute/data.
- Autoregressive latency example: 10ms/token → 100 tokens ≈ 1 second; ~100ms latency typical for internet apps = big challenge.
- Figure 1-11 (Katrina Nguyen, 2024): inference cost fell and MMLU rose 2022→2024.

### Algorithms & Formulæ
- **Next-token prediction (training):** for each sequence position, P(next token | all preceding tokens); the model is trained to maximize the probability of the actual next token. This single algorithm provides both the "inputs" and "labels" for self-supervision.
- **Entropy (Shannon):** a measure of how much information is contained in a message / how unpredictable a sequence is; underpins the statistical view of language.
- **Supervised vs self-supervised:** supervised uses (input, label) pairs from human annotation; self-supervised derives labels from the input (e.g., next token in a sentence); unsupervised requires no labels.
- **Training-sample expansion:** a sequence of N tokens with `<BOS>`/`<EOS>` produces N+1 training pairs (each prefix predicts the next token) — the basis of Table 1-1.
- **Model-size ↔ data relationship:** to maximize performance, a larger model (more parameters) requires more training data; capacity without data is wasted compute.
- **CLIP contrastive training:** learn a shared embedding space from (image, text) pairs co-occurring online; enables zero-shot image classification (compare image embedding to class-description embeddings).
- **Evaluation fairness (MMLU):** scores must be compared under identical settings (same number of examples, same prompt technique). Gemini's CoT@32 vs ChatGPT 5-shot was an apples-to-oranges comparison.
- **Adaptation decision rule:** start with prompt engineering (cheap, no weights); if performance/quality insufficient, escalate to finetuning (weights, data, cost). RAG adds external context without weights.

### Diagrams / Visuals
- **Figure 1-1:** An LLM as a completion machine — input text → model → completion of the most probable next tokens ("To be or not to be" → ", that is the question.").
- **Figure 1-2:** Self-supervised training — each sentence provides multiple (context → next token) training pairs; no human labels.
- **Figure 1-3:** A multimodal (LMM) model generating the next token conditioned on both text and image tokens.
- **Figure 1-4:** Foundation models as general-purpose: one model serves many tasks vs traditional task-specific models.
- **Figure 1-5:** Adapting an existing model (10 examples, 1 weekend) vs building from scratch (1M examples, 6 months) — reduction in time-to-market.
- **Figure 1-6:** Three factors behind AI engineering's growth: general-purpose capabilities, investment, low entrance barrier.
- **Figure 1-7:** Open-source AI engineering tools' GitHub stars soaring past Bitcoin and toward React/Vue.
- **Figure 1-8:** Companies favor internal-facing apps first (a16z 2024 Growth report) to build expertise while minimizing data-privacy/compliance/risk.
- **Figure 1-9:** Reactive vs proactive AI (chatbot responds on demand; Google Maps traffic alerts appear proactively).
- **Figure 1-10:** Microsoft Crawl-Walk-Run: crawl = human-in-the-loop mandatory; walk = AI interacts internally; run = AI interacts directly with external users.
- **Figure 1-11:** Inference cost and MMLU performance evolution 2022–2024 (cost down, quality up).
- **Figure 1-12:** AI engineering and ML engineering as overlapping disciplines (some companies treat them as the same role).
- **Figure 1-13:** The three-layer AI stack: application development → model development → infrastructure.
- **Figure 1-14:** Breakdown of model development (modeling/training, dataset engineering, inference optimization) and application development (evaluation, prompt engineering, AI interface).
- **Figure 1-15:** Traditional ML pipeline vs foundation-model AI pipeline (adaptation, evaluation, interfaces).
- **Figure 1-16:** Traditional ML builds product after data/model; AI engineering builds the product first, then invests in data/models once the product shows promise (after Shawn Wang, "The Rise of the AI Engineer," 2023).

### Common Exam Traps
- **Masked vs autoregressive LM:** masked predicts *missing* tokens from *both sides* (fill-in-blank, BERT, non-generative); autoregressive predicts the *next* token from *previous* tokens only (generative, default in the book). Do not mix them up.
- **Completion ≠ conversation:** completions are probability-based predictions; a raw model will *continue* text rather than "answer." Making a model *respond appropriately* comes from **post-training** (covered on p.78), not from the base language model.
- **Self-supervision ≠ unsupervised learning:** self-supervision *infers labels from the input*; unsupervised learning uses *no labels at all*. This distinction is often tested.
- **"Training" misuse:** feeding journal entries into ChatGPT is **prompt engineering**, not training/finetuning — training always changes weights.
- **Training vs weight changes:** not all weight changes are training — **quantization** changes weight values but is not training.
- **Pre-training vs post-training vs finetuning:** pre-training = from scratch (most expensive, up to 98% of compute); post-training = after pre-training (interchangeable with finetuning; done by model developers); finetuning = done by application developers. Pre- and post-training form a spectrum.
- **Model size is measured in parameters, not bytes or layers**; "large" is a moving target (117M was large in 2018).
- **Larger models need MORE data** (counterintuitive but tested): capacity without data wastes compute.
- **Tokens ≈ ¾ of a word:** ~100 tokens ≈ 75 words; don't confuse tokens with words.
- **Foundation model ≠ LLM:** foundation models include multimodal models (LMMs); LLMs are text-only. GPT-4V, Gemini, Claude 3 are foundation models, better characterized as such than as "just" LLMs.
- **Prompt engineering/RAG don't change weights;** finetuning does.
- **Eloundou exposure definition:** a task is exposed if AI reduces time by ≥**50%**; an occupation is 80% exposed if **80% of its tasks** are exposed. "Fully exposed" = every task.
- **Gemini MMLU controversy:** CoT@**32** vs ChatGPT's **5** examples — the settings differed, so the comparison was unfair; MMLU scores are only comparable under identical evaluation settings.
- **Table 1-5 traps:** Claude 2 was 78.5% 5-shot; Inflection-2 79.6% (this is *CoT*, not 5-shot, in some listings — check which number matches which technique); GPT-4 87.29% via API; GPT-3.5 70% 5-shot; Llama-2 68.0% 5-shot; PaLM 2-L 78.4% 5-shot; Grok 1 73.0%.
- **Crawl-Walk-Run:** crawl = human mandatory; walk = AI-internal; run = AI-external. Human-in-the-loop is not the same as "run."
- **Defensibility:** three moats = technology, data, distribution; **data** is the nuanced one (usage-data flywheel), but first-mover data advantage can evaporate if a bigger platform subsumes you (Calendly/Mailchimp/Photoroom).
- **Reactive vs proactive:** proactive features (Google Maps traffic) have a *higher quality bar* because they interrupt the user.
- **AI engineering vs ML engineering differences (the 3):** (1) pretrained models → adaptation over training; (2) bigger/compute-heavy → inference optimization + GPU scale; (3) open-ended outputs → harder evaluation. "Less model development, more adapting and evaluating."
- **Evaluation is the biggest new problem** — open-ended outputs mean no exhaustive ground-truth lists; this is why the chapter emphasizes evaluation repeatedly.
- **AI engineering vs full-stack:** interfaces rising → JS tooling (LangChain.js, Transformers.js, Vercel AI SDK); build-product-first philosophy (opposite of traditional ML's data-first).
- **Quantization:** changes weight values but is *not* training — a weight change without training.

### Chapter Summary
Chapter 1 introduces **AI engineering** — building applications on foundation models — and explains why it emerged. Language models (statistical next-token predictors, from Sherlock Holmes to Claude Shannon) scaled into **LLMs** because **self-supervision** (labels inferred from the input itself) removed the data-labeling bottleneck, letting models train on the entire web. LLMs expanded into **foundation models** spanning modalities (LLMs + LMMs like GPT-4V, Gemini, Claude 3), which are general-purpose and adaptable via **prompt engineering, RAG, and finetuning** — far cheaper than building models from scratch. **Three factors** — general-purpose capabilities, surging investment, and a low entrance barrier (APIs, AI writing code) — made AI engineering the fastest-growing discipline. The chapter surveys **use cases** (coding, image/video, writing, education, conversational bots, information aggregation, data organization, workflow automation) and a **planning checklist** (use-case evaluation, build-or-buy, critical-vs-complementary role of AI, reactive-vs-proactive, dynamic-vs-static, human-in-the-loop crawl-walk-run, defensibility via technology/data/distribution, measurable expectations, milestone realism, and continuous maintenance). Finally, it maps the **AI engineering stack** (application development, model development, infrastructure), the **three differences vs ML engineering** (adapting pretrained models, inference optimization at GPU scale, and much harder evaluation), and the growing overlap with **full-stack engineering** (build the product first; JS-centric tooling). The recurring thesis: with foundation models, success comes from **adapting and evaluating** existing models, not training your own.

### Confidence Check
- **Sure:** definitions of tokens/vocabulary/tokenization and the 9-token GPT-4 example; masked vs autoregressive LMs; self-supervision vs supervised vs unsupervised and the 6-sample Table 1-1 expansion; BOS/EOS; parameter-count sizing history (117M → 1.5B → ~100B); why larger models need more data; foundation models vs LLMs vs LMMs; the three adaptation techniques (prompt engineering, RAG, finetuning) and which change weights; the three growth factors and their stats; the term "AI engineering" and the 20-practitioner survey; the 8 use-case groups; Eloundou's exposure definitions and Table 1-2 numbers; planning checklist (build-or-buy, critical/complementary, reactive/proactive, dynamic/static, crawl-walk-run, defensibility, metrics, last-mile, maintenance); the 3-layer stack; the 3 differences vs ML engineering; the Gemini MMLU CoT@32 controversy and Table 1-5 scores; pre-training/post-training/finetuning definitions; quantization ≠ training; build-product-first philosophy.
- **Uncertain:** exact wording of a few quotes (PDF extraction split some lines mid-sentence); precise figure caption text for Figures 1-4, 1-5, 1-12–1-16 (paraphrased from extraction); some exact page numbers for named citations; the precise Inflection-2/Claude 2 MMLU technique-vs-score pairing in Table 1-5 (verified the widely-cited 5-shot numbers; CoT variants flagged as reported).

---

## §2. Code & Pseudocode Breakdown

This chapter is conceptual (no runnable code listings); it contains tables, figures, and pseudocode-style descriptions. Below are the key "algorithmic" breakdowns presented as tables/pseudocode.

### Table 1-1: Self-supervised training samples for "I love street food."
Input sequence with `<BOS>` and `<EOS>`: `<BOS> I love street food . <EOS>`

| # | Context (input) | Label (next token) |
|---|---|---|
| 1 | `<BOS>` | I |
| 2 | `<BOS> I` | love |
| 3 | `<BOS> I love` | street |
| 4 | `<BOS> I love street` | food |
| 5 | `<BOS> I love street food` | . |
| 6 | `<BOS> I love street food .` | `<EOS>` |

- Six training samples generated from one sentence with no human labeling.
- **Terms:** `<BOS>` = beginning-of-sequence marker; `<EOS>` = end-of-sequence marker (teaches the model when to stop); context = the tokens seen so far; label = the next token to predict.
- **Takeaway:** this is *self-supervision* — both the input (context) and the label (next token) come from the raw text itself.

### Tokenization walkthrough (GPT-4, "I can't wait to build AI applications")
- Raw text → 9 tokens: `I`, `can`, `'t`, ` wait`, ` to`, ` build`, ` AI`, ` app`, `lications` (approximate token split per the book).
- "can't" → `can` + `'t` (meaningful sub-word split).
- Vocabulary: Mixtral 8x7B = 32,000; GPT-4 = 100,256.
- **Terms:** token = basic unit; vocabulary = all known tokens; tokenization = the breaking process.
- **Why sub-word tokens:** meaningful components, smaller vocabulary than full words, handles out-of-vocabulary words ("chatgpting" → "chatgpt"+"ing").

### Self-supervised training loop (conceptual)
```
for each sequence in corpus:
    prepend <BOS>, append <EOS>
    for each position i in sequence:
        context  = tokens[0:i]
        label    = tokens[i]
        loss += -log P(label | context)      # next-token prediction
```
- The model learns P(next token | all previous tokens) — the basis of both LLM scaling and generative text.

### The three-layer AI engineering stack (Figure 1-13)
```
┌───────────────────────────────────────────────┐
│  APPLICATION DEVELOPMENT                     │
│  prompt engineering · context (RAG/agents)    │
│  evaluation · interfaces                      │
├───────────────────────────────────────────────┤
│  MODEL DEVELOPMENT                            │
│  modeling & training · dataset engineering    │
│  inference optimization                       │
├───────────────────────────────────────────────┤
│  INFRASTRUCTURE                               │
│  model serving · data & compute · monitoring  │
└───────────────────────────────────────────────┘
```
- Start at the top (application development) and move down only as needed.
- **Terms:** serving = running models in production; monitoring = tracking quality/cost/latency.

### Model adaptation decision flow (no weights → weights)
```
1. Prompt engineering   (no weight updates; cheap; needs few examples)
   ├─ sufficient? ────── YES → ship
   └─ NO
2. RAG                  (add external context at inference; no weight updates)
   ├─ sufficient? ────── YES → ship
   └─ NO
3. Finetuning           (updates weights; needs data, compute, evaluation)
   → ship with evaluation harness
```
- **Terms:** weights = learned parameters; RAG = retrieval-augmented generation; finetuning = continued training on task data.
- **Takeaway:** escalate complexity only when simpler techniques are insufficient.

### MMLU evaluation fairness check (the Gemini lesson)
```
Model A: Gemini Ultra  → MMLU 90.04% using CoT@32 (32 examples)
Model B: GPT-4/ChatGPT → MMLU 87.29% using 5 examples (GPT-4, via API)
"Fair" comparison: same prompt technique + same number of examples
→ with 5-shot settings, ChatGPT performed better than Gemini.
```
- **Terms:** MMLU = Massive Multitask Language Understanding benchmark; CoT@N = chain-of-thought prompting with N examples.
- **Takeaway:** always compare models under identical evaluation settings.

### Crawl-Walk-Run (human-in-the-loop escalation, Figure 1-10)
```
CRAWL → human involvement mandatory (AI assists human agents)
WALK  → AI can interact with internal employees
RUN   → increased automation; AI interacts with external users directly
```
- Escalate as confidence/accuracy grows; e.g., a 95% verbatim-acceptance rate may justify letting customers interact directly.

### Traditional ML vs AI engineering development order (Figure 1-16)
```
Traditional ML:  data → model training → evaluation → product (last)
AI engineering:  product/demo first → feedback → invest in data & models
                 once the product shows promise
```
- **Takeaway:** AI engineers are much more involved in building the product; full-stack skills are an advantage.
