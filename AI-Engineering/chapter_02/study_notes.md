# Chapter 2 Study Bundle: Understanding Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 2

---

## §1. Study Notes

### Core Theme
Chapter 2 explains the **design decisions** that go into building a foundation model — decisions about **training data**, **model architecture and size**, and **post-training** (alignment with human preference) — plus the **sampling** process by which a model chooses outputs. The chapter is structured for users of foundation models, not builders: a high-level understanding helps you decide *which* model to use and *how* to adapt it. The chapter covers: (1) **training data** — most models rely on whatever data is available (e.g., Common Crawl), which is why language- and domain-specific data curation matters; (2) **modeling** — the transformer architecture (attention, prefill/decode), alternatives (RWKV, SSMs, Mamba, Jamba), model sizing (parameters, tokens, FLOPs), the Chinchilla scaling law, and scaling bottlenecks (data and electricity); (3) **post-training** — supervised finetuning (SFT) and preference finetuning (RLHF, DPO) to make models conversational and aligned; and (4) **sampling** — temperature, top-k, top-p, test time compute, structured outputs, and the probabilistic nature of AI (inconsistency and hallucination). The chapter's recurring message: working with AI requires **building workflows around the model's probabilistic nature** — the first step toward *systematic* AI engineering is a solid evaluation pipeline (the subject of the next two chapters).

### Key Definitions
- **Training data distribution:** The mix of languages, domains, and topics in the data a model learns from; since models learn from data, the distribution reveals a lot about their capabilities and limitations.
- **Low-resource language:** A language with limited availability as training data — typically those not among the languages with ≥1% share in Common Crawl.
- **Transformer architecture (Vaswani et al., 2017):** The dominant architecture for language-based foundation models, based on the **attention mechanism**; it dispenses with RNNs so input tokens can be processed in parallel.
- **Seq2seq (sequence-to-sequence) architecture:** Introduced 2014; an encoder that processes inputs plus a decoder that generates outputs, both RNN-based; the transformer's predecessor.
- **Attention mechanism:** Lets the model weigh the importance of different input tokens when generating each output token; uses query (Q), key (K), and value (V) vectors. Introduced three years *before* the transformer (GNMT, 2016), but took off only when the transformer showed it could be used without RNNs.
- **Query vector (Q):** Represents the current state of the decoder at each decoding step (like a person looking for information).
- **Key vector (K):** Represents a previous token (like a page number); previous tokens include both input tokens and previously generated tokens.
- **Value vector (V):** Represents the actual value of a previous token as learned by the model (like a page's content).
- **Multi-headed attention:** Splits Q, K, V into smaller vectors per attention head so the model can attend to different groups of previous tokens simultaneously. Llama 2-7B: 32 heads → each K/V/Q vector of dimension 128 (4096/32).
- **Prefill:** The first of two inference steps; the model processes input tokens in parallel and creates the intermediate state (key and value vectors for all input tokens) needed to generate the first output token.
- **Decode:** The second inference step; the model generates one output token at a time.
- **Transformer block (layer):** One building block of a transformer, containing an attention module (Q, K, V, and output projection matrices) and an MLP module (linear layers separated by nonlinear activation functions). The number of blocks = the model's number of layers.
- **Embedding module:** Before the transformer blocks; converts tokens and their positions into embedding vectors (embedding matrix + positional embedding matrix).
- **Output layer / unembedding layer / model head:** After the transformer blocks; maps output vectors into token probabilities; typically one matrix.
- **Activation function:** A nonlinear function (e.g., ReLU `max(0,x)`, GELU) that lets linear layers learn nonlinear patterns; simple functions work best — fancier ones don't perform better and cost more compute.
- **Sparse model:** A model with a large percentage of zero-value parameters; a 7B model that is 90% sparse has only 700M non-zero parameters.
- **Mixture-of-experts (MoE) (Shazeer et al., 2017):** A type of sparse model divided into groups of parameters (experts); only a subset of experts is active for each token. Mixtral 8x7B: 8 experts × 7B params, 46.7B total (shared params), 12.9B active per token.
- **FLOP (floating point operation):** A standardized unit for compute requirement. GPT-3-175B: 3.14 × 10²³ FLOPs; PaLM-2 largest: 10²² FLOPs.
- **FLOP/s (FLOPs per second):** A machine's peak performance; H100 NVL: 60 TeraFLOP/s = 6 × 10¹³ FLOPs/s = 5.2 × 10¹⁸ FLOPs/day. **1 FLOP/s-day = 86,400 FLOPs** (OpenAI's unit).
- **Utilization:** How much of a machine's maximum compute capacity you actually use; ~50% is okay, >70% is great.
- **Inverse scaling:** Phenomena where bigger models perform *worse*; e.g., Anthropic 2022 found more alignment training led to models that align less (Perez et al., 2022). The Inverse Scaling Prize (2023, NYU-led) rewarded tasks where larger LMs do worse.
- **Scaling law (Chinchilla, DeepMind 2022):** Given a compute budget, compute-optimal training needs roughly **20 training tokens per parameter**; model size and training tokens should be scaled equally (doubling model size → double tokens).
- **Scaling extrapolation (hyperparameter transfer):** Studying how hyperparameters affect small models, then extrapolating to much larger target models (Microsoft & OpenAI transferred hyperparameters from a 40M to a 6.7B model).
- **Emergent abilities (Wei et al., 2022):** Abilities that appear only at scale and may not be observable on smaller models — they make scaling extrapolation less accurate.
- **Post-training:** Training after pre-training to address (1) models optimized for text completion rather than conversation and (2) outputs that can be racist, sexist, rude, or wrong. Two steps: SFT + preference finetuning.
- **Supervised finetuning (SFT):** Finetuning the pre-trained model on high-quality (prompt, response) **demonstration data** to optimize for conversations instead of completion; also called behavior cloning.
- **Preference finetuning:** Further finetuning to output responses aligned with human preference, typically with reinforcement learning. Techniques: RLHF (GPT-3.5, Llama 2), DPO (Llama 3), RLAIF (potentially Claude).
- **RLHF (reinforcement learning from human feedback):** Two parts: (1) train a **reward model** that scores the foundation model's outputs; (2) optimize the foundation model to generate responses with maximal reward-model scores (often with PPO).
- **Reward model:** Given (prompt, response), outputs a score of how good the response is; trained on **comparison data** `(prompt, winning_response, losing_response)` rather than pointwise scores.
- **DPO (Direct Preference Optimization) (Rafailov et al., 2023):** A simpler preference finetuning alternative to RLHF; used by Meta for Llama 3 to reduce complexity.
- **Comparison data:** Labeled data of the form (prompt, winning_response, losing_response); ranking 3 responses (A > B > C) yields 3 ranked pairs: (A>B), (A>C), (B>C).
- **Pointwise evaluation:** Scoring each response independently (e.g., 5/10 vs 7/10) — a harder labeling task than comparison because scores vary.
- **Sampling:** The process by which a model chooses an output from all possible options; it makes AI outputs probabilistic.
- **Greedy sampling:** Always picking the most likely next token; works for classification but creates boring LM outputs.
- **Logit:** A raw pre-softmax score for each possible token; logits don't sum to 1 and can be negative.
- **Softmax:** Converts logits to probabilities: `p_i = e^(x_i) / Σ_j e^(x_j)`; requires two passes over all values (expensive for large vocabularies).
- **Temperature (T):** A constant that divides logits before softmax; higher T → more creative/chaotic outputs, lower T → more consistent/boring. Typical provider range 0–2; 0.7 often recommended for creative use; T=0 (arg max) picks the largest logit without softmax.
- **Logprob (log probability):** Probability in log scale; helps avoid the underflow problem with tiny probabilities (e.g., vocab of 100,000). The logprob of a sequence = sum of its tokens' logprobs; **average logprob** (dividing by length) avoids biasing against long sequences. OpenAI API exposes logprobs of only the top 20 tokens; Anthropic doesn't expose them.
- **Top-k sampling:** Perform softmax only over the top-k logits (k commonly 50–500); reduces softmax computation; smaller k → more predictable but less interesting text.
- **Top-p (nucleus) sampling:** Sum the probabilities of the most likely next values in descending order until the sum reaches p; only those values are considered. Common values 0.9–0.95; more dynamic than top-k (a "yes/no" prompt should consider ~2 values, "meaning of life" many). Doesn't reduce softmax computation load.
- **Min-p sampling:** Set the minimum probability a token must reach to be considered.
- **Test time compute:** Allocating more compute at inference to improve quality — e.g., generating multiple outputs (best of N), beam search, verifiers/reward models to select the best.
- **Best of N:** Generate N outputs and pick the one that works best (highest probability, best score, or a heuristic); used by Stitch Fix and Grab with reward models.
- **Beam search:** Generating a fixed number of most promising candidates (the beam) at each step, instead of independent outputs.
- **Self-consistency (Wang et al., 2023):** Picking the most common output among a set of outputs; useful for exact-answer tasks (e.g., math, multiple choice — Google sampled 32 outputs for Gemini's MMLU evaluation).
- **Verifier:** A model (often a reward model) that scores candidate outputs; OpenAI found verifiers gave roughly the same boost as a **30× model size increase** (Cobbe et al., 2021).
- **Semantic parsing:** Converting natural language into a structured, machine-readable format (e.g., text-to-SQL, text-to-regex).
- **Structured outputs:** Outputs that follow certain formats (JSON, YAML, regex, SQL); crucial when (1) the task requires structured outputs or (2) outputs are consumed by downstream applications/agents.
- **Constrained sampling:** Filtering the logit vector to keep only tokens meeting constraints (based on a grammar), then sampling among valid tokens; less generalizable (each format needs its own grammar) and adds latency.
- **Inconsistency:** A model generating very different responses for the same or slightly different prompts. Two scenarios: (1) same input, different outputs; (2) slightly different input, drastically different outputs.
- **Hallucination:** A model giving a response not grounded in facts; a concern for generative models even before LLMs (mentioned as early as 2016, Goyal et al., 2016).
- **Self-delusion (hallucination hypothesis 1, Ortega et al., DeepMind 2021):** A model hallucinates because it can't differentiate between the data it's given and the data it generates; also called **snowballing hallucinations** (Zhang et al., 2023) — after an incorrect assumption, the model keeps hallucinating to justify it.
- **Mismatched internal knowledge (hallucination hypothesis 2, Leo Gao):** Hallucination caused by the mismatch between the model's internal knowledge and the labeler's internal knowledge — if labeler responses use knowledge the model lacks, SFT teaches the model to hallucinate.
- **RLAIF (reinforcement learning from AI feedback):** Preference finetuning using AI-generated feedback instead of human feedback.

### Core Concepts & Frameworks
- **The three foundational design decisions (the chapter's spine):** Differences between foundation models trace back to decisions about (1) **training data**, (2) **model architecture and size**, and (3) **post-training to align with human preferences**.
- **"Use what we have, not what we want":** Model developers often rely on available data (Common Crawl, C4) even if it doesn't exactly meet their needs; this leads to models that perform well on tasks present in the training data but not necessarily on your tasks — hence data curation matters.
- **Training data sources:**
  - **Common Crawl:** Nonprofit; crawled ~2–3 billion web pages per month in 2022–2023; quality is questionable (fake news, conspiracy theories, etc.); variations of it are used in most foundation models that disclose training data (GPT-3, Gemini).
  - **C4 (Colossal Clean Crawled Corpus):** Google's clean subset of Common Crawl.
  - **GPT-2 heuristic:** OpenAI used only Reddit links that received at least three upvotes.
  - **Quality over quantity:** Gunasekar et al. (2023) trained a 1.3B model on 7B tokens of high-quality coding data that outperforms much larger models on coding benchmarks.
- **Multilingual models:**
  - English dominates: **45.88%** of Common Crawl; Russian second at **5.97%** (8× less). Table 2-1 lists languages ≥1% (English, Russian, German, Chinese, Japanese, French, Spanish, Italian, Dutch, Polish, Portuguese, Vietnamese).
  - Under-represented languages (Table 2-2): Punjabi (world:Common Crawl ratio **231.56**), Swahili (115.26), Urdu (105.38), Kannada (65.57), Telugu (64.89), Gujarati (61.51), Marathi (58.10), Bengali (36.56); English ratio = 0.40. Ideal ratio = 1.
  - MMLU benchmark = 14,000 multiple-choice problems across 57 subjects; GPT-4 performs much better in English than in under-represented languages (Telugu, Marathi, Punjabi are worst). OpenAI translated MMLU questions with Azure AI Translator.
  - Yennie Jun: on six Project Euler math problems, GPT-4 solved English problems more than **3×** as often as Armenian or Farsi; failed all six in Burmese and Amharic.
  - Translation-as-workaround is not ideal: needs a model that understands the source language, and translation causes information loss (e.g., Vietnamese relationship pronouns collapse to "I"/"you").
  - Unexpected risks: NewsGuard (April 2023) found ChatGPT-3.5 refused false-claim requests in English 6/7 times but produced false claims in simplified *and* traditional Chinese all 7 times.
  - Cost/latency: tokenization is less efficient for some languages — on MASSIVE (1M short texts, 52 languages) median token length: English 7, Hindi 32, Burmese 72 (10× English) → ~10× longer generation time and 10× cost for Burmese.
  - Non-English models: Chinese (ChatGLM, YAYI, Llama-Chinese), French (CroissantLLM), Vietnamese (PhoGPT), Arabic (Jais).
- **Domain-specific models:**
  - General-purpose models underperform on domain-specific tasks the model never saw (drug discovery, cancer screening).
  - Examples: **AlphaFold** (DeepMind, ~100,000 known proteins), **BioNeMo** (NVIDIA, biomolecular data for drug discovery), **Med-PaLM2** (Google, LLM + medical data).
  - Table 2-3 (ViT-B/32): CLIP vs Open CLIP on ImageNet (63.2 vs 62.9), Birdsnap (37.8 vs 46.0), Stanford Cars (59.4 vs 79.3), etc.
- **Transformer architecture — the problem it solved:**
  - seq2seq (2014) used RNNs and delivered big gains on translation/summarization (Google Translate 2016 = "largest improvements to date"); two problems: (1) decoder generates output using *only the final hidden state* of the input (like writing a book summary from the book's summary), and (2) RNNs process sequentially → slow for long sequences (RNNs also prone to vanishing/exploding gradients).
  - Transformer (Vaswani et al., 2017) fixes both with **attention**: weigh each input token when generating each output token (like referencing any page of the book); input tokens processed in parallel.
  - Inference = **prefill** (parallel, builds K/V state) + **decode** (one token at a time). Sequential output bottleneck remains for autoregressive models → motivates Chapter 9 inference optimization.
  - Attention math: K = xW_K, V = xW_V, Q = xW_Q; `Attention(Q,K,V) = softmax(QKᵀ/√d)V`. Long sequences → more K/V vectors to compute/store → why extending context length is hard.
  - Model size is determined by: model dimension (sizes of Q/K/V/output-projection matrices), number of transformer blocks, feedforward dimension, vocabulary size.
  - Table 2-4 (Llama dims): Llama 2-7B (32 blocks, 4096 dim, 11008 FFN, 32K vocab, 4K ctx); Llama 2-13B (40, 5120, 13824); Llama 2-70B (80, 8192, 22016); Llama 3-7B (32, 4096, 14336, 128K vocab, 128K ctx); Llama 3-70B (80, 8192, 28672); Llama 3-405B (126, 16384, 53248). Context length affects memory footprint but not parameter count.
  - Architecture history: seq2seq in limelight 2014–2018 (4 yrs), GANs 2014–2019, transformer "sticky" since 2017; Ilya Sutskever is first author of seq2seq and second author of AlexNet; his argument on why beating the transformer is hard: neural nets simulate computer programs, gradient descent searches that program space, so a new architecture must simulate programs existing ones can't.
  - Transformer was designed to run fast on TPUs, only later optimized on GPUs.
- **Alternative architectures:**
  - **RWKV (Peng et al., 2023):** RNN-based but parallelizable for training; in theory no context-length limitation, in practice no guarantee of good long-context performance.
  - **SSMs (state space models) (Gu et al., 2021a):** promising for long-range memory.
  - **S4 (Gu et al., 2021b):** made SSMs more efficient.
  - **H3 (Fu et al., 2022):** added a mechanism to recall early tokens and compare across sequences (attention-like but more efficient).
  - **Mamba (Gu and Dao, 2023):** scales SSMs to 3B params; Mamba-3B beats transformers of the same size and matches twice its size; inference computation scales **linearly** with sequence length vs transformers' quadratic.
  - **Jamba (Lieber et al., 2024):** hybrid transformer–Mamba; MoE with 52B total (12B active) params, fits in a single 80GB GPU; strong up to 256K context; small memory footprint.
- **Model size — three numbers of scale:**
  1. **Number of parameters** — proxy for learning capacity.
  2. **Number of training tokens** — proxy for how much the model learned.
  3. **Number of FLOPs** — proxy for training cost.
  - Newer-generation models beat older same-size models (Llama 3-8B 2024 beats Llama 2-70B 2023 on MMLU).
  - Memory: 7B params × 2 bytes = 14 GB minimum for inference.
  - Sparsity: 90%-sparse 7B model = 700M non-zero params; large sparse models can need less compute than small dense models.
  - Dataset size: measured in **tokens** (a book is worth more than a sentence, so sample count is a bad metric; tokens are the unit the model operates on). Llama datasets: 1.4T (Llama 1), 2T (Llama 2), 15T (Llama 3) tokens; RedPajama-v2: 30T tokens ≈ 450M books ≈ 5,400× Wikipedia.
  - Training tokens vs dataset tokens: 1T-token dataset × 2 epochs = 2T training tokens; large models typically pre-trained on only one epoch.
  - Table 2-5 (params → training tokens): LaMDA 137B→168B; GPT-3 175B→300B; Jurassic 178B→300B; Gopher 280B→300B; MT-NLG 530B→270B; Chinchilla 70B→1.4T (the compute-optimal outlier).
  - Compute math: 256 H100s at max capacity → train GPT-3-175B in (3.14×10²³)/(256×5.2×10¹⁸) ≈ **236 days ≈ 7.8 months**; at 70% utilization and $2/h → **~$4M** ($4,142,811.43).
  - Quality, quantity, and diversity are the three golden goals for training data (Ch 8).
- **Inverse scaling:** 2022 Anthropic (Perez et al.) — more alignment training → models more likely to express specific political/religious views and less aligned. Inverse Scaling Prize (2023): $5,000 third / $20,000 second / $100,000 first prizes; 99 submissions, 11 third prizes; **no second or first prizes** because no submitted task showed real-world failures. Larger LMs are sometimes (only sometimes) worse on memorization tasks and tasks with strong priors.
- **The Chinchilla scaling law:**
  - Trained 400 models (70M–16B params) on 5–500B tokens to study params/tokens/FLOPs/loss.
  - Compute-optimal training: **training tokens ≈ 20 × model size**; a 3B model needs ~60B tokens; scale size and tokens equally (double size → double tokens).
  - Predicts optimal params + tokens per FLOP budget AND the expected training loss.
  - Assumes data cost << compute cost; Sardana et al. (2023) modified it for inference demand (Llama chose smaller, more usable models).
  - Last-mile economics: improving error rate from 3% to 2% may need an order of magnitude more data/compute/energy; cross entropy drop from ~3.4 to 2.8 nats requires **10× more training data**.
- **Scaling extrapolation:** With only one shot at hyperparameters for a large model, study small models and extrapolate (40M → 6.7B worked, Microsoft & OpenAI 2022). Nontrivial: 10 hyperparameters = 1,024 combinations to study individually and jointly; emergent abilities reduce accuracy (Luke Metz blog, 2022).
- **Scaling bottlenecks (data & electricity):**
  - Data: dataset growth faster than new data generation (Villalobos et al., 2022); risk of running out of internet data; people (and bad actors — prompt injection, Ch 5) inject text into training data via the public web.
  - AI-generated web data: Grok (Dec 2023) refused a request citing OpenAI's policy — likely because it was trained on web data "full of ChatGPT outputs" (Igor Babuschkin); recursively training on AI data can cause models to forget original patterns (Shumailov et al., 2023; nuance in Ch 8).
  - Proprietary data (copyrighted books, translations, contracts, medical records, genomes) will be a competitive advantage — OpenAI deals with Axel Springer and the Associated Press; Reddit and Stack Overflow changed terms to block scraping; C4 restrictions: **28%** of critical sources restricted 2023–2024 → **45%** now restricted (Longpre et al., 2024).
  - Electricity: data centers use ~1–2% of global electricity, projected 4–20% by 2030 (Patel et al., 2024); growth capped at ~50× → power shortage concerns.
  - Dario Amodei: if the scaling hypothesis holds, a $100B model could be as good as a Nobel prize winner.
- **Post-training workflow (Figure 2-10):** pre-training → SFT → preference finetuning (e.g., RLHF). Shoggoth-with-a-smiley-face analogy (Figure 2-11): pre-training = untamed monster; SFT = makes it socially acceptable; preference finetuning = smiley face. Post-training unlocks capabilities pre-training already provides but that are hard to access via prompting alone; InstructGPT used only **2%** of compute for post-training vs 98% pre-training.
- **SFT details:**
  - A pre-trained model completes text ("How to make pizza" → valid completions include follow-up questions, more context, or instructions — the *correct* conversational response is instructions).
  - Demonstration data = (prompt, response) pairs; behavior cloning. Data should span the range of requests (QA, summarization, translation).
  - InstructGPT demonstration data (Figure 2-12 distribution): no multimodal tasks (text-only model).
  - Labelers: ~**90%** college degree, >1/3 master's; one pair can take up to 30 min; $10/pair → 13,000 pairs ≈ $130,000 (excluding design, recruiting, QC).
  - LAION: 13,500 volunteers, 10,000 conversations, 161,443 messages in 35 languages, 461,292 quality ratings; ~90% of volunteer labelers identified as male (Köpf et al., 2023).
  - DeepMind Gopher used heuristics: filter for `[A]: [short paragraph] [B]: ...` dialogue format from internet data.
  - Can train from scratch on demonstration data (skipping pre-training), but pre-training + finetuning usually gives superior results.
- **Preference finetuning details:**
  - Motivation: demonstration data teaches *how* to converse, not *what* conversations to have; controversial topics (abortion, gun control, immigration) upset someone regardless; over-censoring makes models boring.
  - RLHF (earliest popular preference algorithm): reward model + optimization; Meta used RLHF (Llama 2) → DPO (Llama 3) for complexity reduction; Llama 2 authors: "superior writing abilities ... fundamentally driven by RLHF."
  - Reward model training data: comparison data (prompt, winning, losing). Pointwise scores vary (5 vs 7 by different labelers; same labeler twice). LMSYS: comparing two responses takes 3–5 minutes (fact-checking); Thomas Scialom (Llama-2 author): $3.50/comparison vs $25 to write a response.
  - InstructGPT labeling UI (Figure 2-13): scores 1–7 but **only ranking is used**; inter-labeler agreement ~**73%** (7 of 10 people rank the same pair the same way); 3 ranked responses → 3 ranked pairs.
  - Reward model objective: `loss = -log(σ(rθ(x, y_w) − rθ(x, y_l)))`; minimize expected loss (σ = sigmoid). Reward model can be trained from scratch or finetuned on the strongest foundation model; a **weak model can judge a stronger one** (judging is easier than generation).
  - Finetuning with RM: sample prompts, score responses, train to maximize scores — often with **PPO** (OpenAI, 2017). RLHF and DPO both improve over SFT alone, though why they work is debated.
  - Skip RL entirely: **best of N** with reward model alone (Stitch Fix, Grab) — generate multiple outputs, pick highest-scored.
- **Sampling fundamentals:**
  - Classification: model outputs class probabilities (e.g., spam 90% / not 10%); a >50% threshold marks spam.
  - LM next token: probability distribution over all tokens in the vocabulary (Figure 2-14); greedy sampling = always pick most likely.
  - Logits → softmax (`p_i = e^(x_i)/Σe^(x_j)`); softmax needs two passes → expensive at large vocab → top-k motivation.
- **Sampling strategies:**
  - **Temperature:** logits divided by T before softmax. Example logits [1, 2]: T=1 → [0.27, 0.73]; T=0.5 → [0.12, 0.88]. Higher T → more creative, less coherent; lower T → more consistent, boring. Providers limit T to 0–2; 0.7 often recommended for creative tasks; T=0 is effectively arg max (logits can't be divided by 0).
  - **Logprobs:** log-scale probabilities avoid underflow; sequence logprob = sum of token logprobs; average logprob avoids short-sequence bias (what OpenAI API's `best_of` uses).
  - **Top-k:** softmax over top-k logits only (k ~50–500); less compute; smaller k → more predictable, less interesting.
  - **Top-p (nucleus):** include the smallest set of values whose cumulative probability exceeds p (common 0.9–0.95); dynamic per context; doesn't reduce softmax load; works well in practice.
  - **Min-p:** minimum probability threshold for a token to be considered.
  - **Stopping conditions:** fixed token count (may cut mid-sentence); stop tokens/stop words (e.g., EOS); keep latency/cost down; premature stopping can break formats (e.g., missing JSON closing brackets).
- **Test time compute:**
  - Generate multiple responses to raise the chance of a good one; best of N; beam search keeps a fixed beam of promising candidates; diversify by varying sampling variables.
  - Cost: 2 outputs ≈ 2× cost (input can be processed once and reused).
  - Selecting best: (1) highest probability (product of token probs) / highest **average logprob** (OpenAI `best_of`); (2) reward model / verifier (Nextdoor: reward model was the key factor; OpenAI verifiers ≈ **30× model-size boost** — a 100M-param model with verifier ≈ 3B without); (3) application-specific heuristics (shortest response, generate until valid SQL).
  - Scaling: OpenAI found performance improved with more samples but **peaked at 400** (adversarial outputs can fool the verifier); Stanford "Monkey Business" (Brown et al., 2024) found solved problems grow **log-linearly** from 1 to 10,000 samples. DeepMind (Snell et al., 2024): scaling test-time compute can be more efficient than scaling model parameters.
  - Latency use case: TIFIN generates multiple responses in parallel, shows the first valid completed response.
  - Self-consistency: pick the most common output (Google sampled 32 outputs per MMLU question for Gemini).
  - Robustness: a model is robust if small input variations don't dramatically change outputs; brittle models benefit most from sampling multiple outputs (e.g., image text extraction succeeded only half the time per try, but 3 tries worked for most images).
- **Structured outputs:**
  - Two scenarios: (1) task requires structured outputs (semantic parsing: text-to-SQL, text-to-regex, classification with valid classes); (2) outputs consumed by downstream applications (e.g., email as JSON `{"title": ..., "body": ...}`; agentic workflows pass outputs into tools — Ch 6).
  - GPT-4o text-to-regex example outputs (email, dates).
  - Frameworks: guidance, outlines, instructor, llama.cpp. OpenAI first introduced JSON mode; JSON mode guarantees valid JSON *syntax*, not content — and truncation at max tokens can still break parseability.
  - Five layers to guide structured outputs: **prompting → post-processing → test time compute → constrained sampling → finetuning**. First three are "bandages" (work if the model is already good); constrained sampling and finetuning are "intensive treatment."
  - Prompting: no guarantee the model follows; AI-as-a-judge validation = 2 queries/output (cost/latency); model providers report 0% to high-90s% valid JSON.
  - Post-processing: models repeat similar mistakes; script to fix them; LinkedIn's defensive YAML parser raised correct YAML from 90% → **99.99%** (Bottaro and Ramgopal, 2020); LinkedIn chose YAML over JSON because it's less verbose → fewer output tokens.
  - Constrained sampling: filter logits to tokens meeting a grammar; needs a grammar per format (JSON, YAML, regex, CSV); less generalizable; grammar verification adds latency.
  - Finetuning: most effective/general; can modify architecture (classifier head for classification — **feature-based transfer**, Ch 7); retrain whole model (better but more resources) or just the head.
  - Future: as models follow instructions better, these techniques become less important.
- **Probabilistic nature of AI:**
  - Example: best-cuisine question — a friend answers the same way twice; an AI that believes Vietnamese 70% / Italian 30% answers accordingly each time. Opposite of deterministic.
  - Causes **inconsistency** and **hallucination**. Models aggregate "the opinions of the masses"; anything with non-zero probability can be generated. One-fifth of customer-support questions for an AI company were about inconsistency; hallucination is the biggest blocker for enterprise use cases (with Drew Houston and Harrison Chase, July 2023).
  - Inconsistency mitigation (same input): cache answers; fix sampling variables (temperature, top-p, top-k); fix the seed (starting point of the random number generator). Hardware differences can also change outputs; no guarantee of 100% consistency. Slightly-different-input scenario is harder — fix sampling variables, craft prompts (Ch 5), add memory (Ch 6).
  - Hallucination: fatal for factuality-dependent tasks (law firm fined June 2023 for ChatGPT-fabricated legal research). Hallucination predates LLMs (2016, Goyal et al.); detecting/measuring it is a long-standing NLG staple.
  - Hypothesis 1 — **self-delusion**: model can't distinguish data it's given from data it generates; a slightly off generated sentence ("Chip Huyen is an architect.") becomes conditioning context → snowballing hallucinations (Zhang et al., 2023); can cause wrong answers to questions the model would otherwise get right (Figure 2-25: 9677 divisible by 13). Mitigations: RL — make the model distinguish user prompts (observations) from its own tokens (actions); SL — include factual and counterfactual signals in training data.
  - Hypothesis 2 — **mismatched knowledge**: labeler responses use knowledge the model lacks → SFT teaches hallucination; theoretically fixable by including labeler knowledge with each response (impossible in practice). John Schulman (April 2023): LLMs know if they know something; fixes: verification (retrieve sources) and RL (better reward function punishing made-up content). OpenAI found RLHF helps reduce hallucinations, but the InstructGPT paper shows RLHF made hallucinations *worse* (Figure 2-26) — yet humans preferred the RLHF model overall.
  - Prompting mitigations: "Answer as truthfully as possible, and if you're unsure, say, 'Sorry, I don't know'"; concise responses help (fewer tokens = fewer chances to make things up). Self-delusion hypothesis concerns **self-supervision**; mismatched-knowledge hypothesis concerns **supervision**.
- **Summary thesis:** build workflows around the probabilistic nature; the first step to *systematic* AI engineering is a solid evaluation pipeline (next two chapters).

### Important Numbers / Stats / Tokens
- Common Crawl: ~2–3 billion web pages/month (2022–2023); English = **45.88%**, Russian = **5.97%**.
- Table 2-1 (≥1% languages): en 45.88%, ru 5.97%, de 5.88%, zh 4.87%, ja 4.79%, fr 4.73%, es 4.47%, it 2.57%, nl 2.06%, pl 1.66%, pt 1.15%, vi 1.03%.
- Table 2-2 world:Common Crawl ratios: Punjabi 231.56, Swahili 115.26, Urdu 105.38, Kannada 65.57, Telugu 64.89, Gujarati 61.51, Marathi 58.10, Bengali 36.56, English 0.40. World population used: 8 billion.
- MMLU: **14,000** multiple-choice problems, **57** subjects.
- GPT-4 Project Euler math: >3× better in English than Armenian/Farsi; 0/6 in Burmese and Amharic.
- MASSIVE (1M short texts, 52 languages): median token length English 7, Hindi 32, Burmese 72 (10× English) → ~10× slower and ~10× more expensive for Burmese.
- NewsGuard (April 2023): ChatGPT-3.5 declined 6/7 English misinformation prompts; produced false claims 7/7 in simplified and traditional Chinese.
- GPT-2 data heuristic: Reddit links with ≥3 upvotes.
- Gunasekar et al. (2023): 1.3B model on 7B high-quality coding tokens beats much larger models.
- AlphaFold: ~100,000 known proteins.
- Llama 2-7B: hidden dim 4096, 32 layers/blocks, 32 heads × 128-dim vectors, feedforward 11,008, vocab 32K, context 4K.
- Table 2-4 (blocks/dim/FFN/vocab/ctx): 2-7B (32/4096/11008/32K/4K); 2-13B (40/5120/13824/32K/4K); 2-70B (80/8192/22016/32K/4K); 3-7B (32/4096/14336/128K/128K); 3-70B (80/8192/28672/128K/128K); 3-405B (126/16384/53248/128K/128K).
- Mixtral 8x7B: 8 experts, 56B raw (8×7B) but **46.7B** total (shared params); **12.9B active** per token (2 experts per layer); cost/speed like a 12.9B model.
- Mamba-3B: beats same-size transformers, matches twice its size; linear inference scaling vs quadratic.
- Jamba: 52B total / 12B active; fits one 80GB GPU; up to 256K context.
- Memory estimate: 7B params × 2 bytes = 14GB min for inference.
- Llama dataset tokens: Llama 1 = 1.4T, Llama 2 = 2T, Llama 3 = 15T. RedPajama-v2 = 30T ≈ 450M books (≈67K tokens/book) ≈ 5,400× Wikipedia.
- Table 2-5: LaMDA 137B/168B tokens; GPT-3 175B/300B; Jurassic 178B/300B; Gopher 280B/300B; MT-NLG 530B/270B; Chinchilla 70B/1.4T.
- FLOPs: PaLM-2 largest = 10²²; GPT-3-175B = 3.14 × 10²³. H100 NVL = 60 TeraFLOP/s = 6×10¹³/s = 5.2×10¹⁸/day. 1 FLOP/s-day = 86,400 FLOPs.
- GPT-3-175B on 256 H100s: ~236 days ≈ 7.8 months; at 70% utilization, $2/h → **$4,142,811.43** (~$4M).
- Utilization: ~50% okay; >70% great.
- Chinchilla: 400 models, 70M–16B params, 5–500B tokens; compute-optimal ≈ 20 tokens/param (3B model → 60B tokens).
- Cross-entropy 3.4 → 2.8 nats = 10× more data.
- Scaling extrapolation: 40M → 6.7B transfer (2022); 10 hyperparameters = 1,024 combinations.
- Inverse Scaling Prize: 99 submissions, 11 third prizes ($5,000); no second ($20,000) or first ($100,000) prizes.
- Model-size history: GPT-1 117M → GPT-2 1.5B (×10) → GPT-3 175B (×100), 2018–2021.
- C4 data restrictions: 28% of critical sources restricted (2023–2024) → 45% now restricted.
- Electricity: data centers 1–2% of global electricity now → 4–20% by 2030; data-center growth cap ~50×.
- InstructGPT: 98% pre-training / 2% post-training compute. Labelers: ~90% college degree, >1/3 master's; up to 30 min/pair; $10/pair → 13,000 pairs ≈ $130,000.
- LAION: 13,500 volunteers; 10,000 conversations; 161,443 messages; 35 languages; 461,292 quality ratings; ~90% male labelers.
- Reward model labeling: comparison = 3–5 min (LMSYS); $3.50/comparison vs $25/written response (Scialom); InstructGPT inter-labeler agreement ≈ 73%.
- Temperature example: logits [1, 2] → T=1 [0.27, 0.73]; T=0.5 [0.12, 0.88]; providers cap T at 2; 0.7 common for creative.
- Top-k: k ~50–500. Top-p: 0.9–0.95 common.
- Verifiers ≈ 30× model-size boost (Cobbe et al., 2021).
- OpenAI sample-scaling peak: 400 outputs; Stanford: log-linear growth 1→10,000 samples.
- Google Gemini MMLU: 32 sampled outputs per question.
- LinkedIn YAML: 90% → 99.99% with defensive parser.
- Law firm fined (June 2023) for fictitious ChatGPT legal research.
- InstructGPT RLHF made hallucination worse (Figure 2-26) yet humans preferred it.
- Customer-support review: ~1/5 of questions about AI inconsistency; hallucination = biggest enterprise blocker (Drew Houston, Harrison Chase panel, July 2023).

### Algorithms & Formulæ
- **Softmax:** `p_i = e^(x_i) / Σ_j e^(x_j)` — converts logits to a probability distribution over the vocabulary.
- **Attention:** `K = xW_K; V = xW_V; Q = xW_Q`; `Attention(Q, K, V) = softmax(QKᵀ/√d)V` — d = dimension (Llama 2-7B: 4096; per head 128 = 4096/32).
- **ReLU:** `ReLU(x) = max(0, x)` — converts negative values to 0 (GELU used by GPT-3, ReLU by GPT-2).
- **Reward model objective (InstructGPT):** for sample (x, y_w, y_l): `loss = -log(σ(rθ(x, y_w) − rθ(x, y_l)))`; minimize expected loss over all samples (σ = sigmoid). Training data: prompt x, winning response y_w, losing response y_l; s_w = r(x,y_w), s_l = r(x,y_l).
- **Temperature-adjusted softmax:** `p_i = softmax(x_i / T)`.
- **Sequence probability:** `p(I love food) = p(I) × p(I|love) × p(food|I, love)` (product of conditional token probs). Logprob of sequence = sum of token logprobs; average logprob = sum / length.
- **Compute time estimate:** training time = FLOPs / (machines × FLOP/s per machine); GPT-3 example: (3.14×10²³)/(256 × 5.2×10¹⁸) ≈ 236 days. Cost = machines × $/hour × hours / utilization.
- **Compute-optimal scaling (Chinchilla):** tokens ≈ 20 × params; double params → double tokens.
- **Best of N selection:** pick max average logprob, max reward-model score, most frequent output (self-consistency), or heuristic (shortest / valid-format).
- **Constrained sampling:** keep only logits for tokens satisfying grammar constraints; sample from valid tokens only.

### Diagrams / Visuals
- **Figure 2-1:** MMLU performance by language — GPT-4 better in English than under-represented languages (Telugu, Marathi, Punjabi worst); questions translated with Azure AI Translator.
- **Figure 2-2:** GPT-4 math (Project Euler) — far better in English than Armenian/Farsi; 0/6 in Burmese and Amharic.
- **Figure 2-3:** Distribution of domains in C4 (Washington Post 2023); caveat: only shows included categories, not missing ones.
- **Figure 2-4:** Seq2seq (top) vs transformer (bottom) — arrows show which tokens the decoder attends to when generating each output token.
- **Figure 2-5:** Attention mechanism example (query seeking info from How/are/you/?/¿) next to the "Attention Is All You Need" visualization.
- **Figure 2-6:** Weight composition of a transformer model (embedding + transformer blocks + output layer).
- **Figure 2-7:** Transformer vs Mamba vs Jamba layer blocks (from the Jamba paper, Lieber et al., 2024).
- **Figure 2-8:** Scaling-law graphs — training loss vs parameters, FLOPs, training tokens (Chinchilla).
- **Figure 2-9:** Training-dataset-size growth vs available data stock projection (Villalobos et al., 2024).
- **Figure 2-10:** Full training workflow: pre-training → SFT → RLHF.
- **Figure 2-11:** Shoggoth with a smiley face (anthrupad) — the "monster made customer-appropriate" meme.
- **Figure 2-12:** Distribution of prompts used to finetune InstructGPT (no multimodal tasks).
- **Figure 2-13:** The labeler UI for creating comparison data for InstructGPT's reward model (scores 1–7 + ranking).
- **Figure 2-14:** LM computing the probability distribution over all vocabulary tokens to generate the next token ("My favorite color is …" → red 30%, green 50%).
- **Figure 2-15:** The logit vector for each input — each logit corresponds to a vocabulary token.
- **Figure 2-16:** Softmax probabilities for tokens A and B at different temperatures (logits [1, 2]); B = 73% at T=1.
- **Figure 2-17:** Workflow: logits → probabilities → logprobs.
- **Figure 2-18:** Example token probabilities for top-p: with p=90% only "yes"+"maybe" considered; p=99% adds "no."
- **Figure 2-19:** OpenAI (2021) — sampling more outputs helps, but performance peaks at 400 outputs.
- **Figure 2-20:** Using guidance to generate outputs constrained to a set of options and to a regex.
- **Figure 2-21:** Constrained sampling — filter logits that don't meet constraints, sample among valid outputs.
- **Figure 2-22:** Adding a classifier head to a base model to turn it into a 3-class classifier (feature-based transfer).
- **Figure 2-23:** ChatGPT essay scoring — same prompt twice gave 3/5 and 5/5 (inconsistency).
- **Figure 2-24:** LLaVA-v1.5-7B self-delusion — convinces itself a shampoo bottle is milk, then lists milk in ingredients.
- **Figure 2-25:** Initial incorrect assumption makes the model claim 9677 is divisible by 13 even though it knows otherwise.
- **Figure 2-26:** InstructGPT hallucination worse with RLHF+SFT than SFT alone (Ouyang et al., 2022).

### Common Exam Traps
- **FLOPs vs FLOP/s vs FLOPS:** FLOPs (plural) = total operations for a task (GPT-3 = 3.14×10²³); FLOP/s = operations per second (machine peak, H100 = 60 TeraFLOP/s); FLOP/s-day = 86,400 FLOPs (OpenAI's unit). Don't confuse them.
- **Training tokens ≠ dataset tokens:** 1T-token dataset trained for 2 epochs = 2T *training* tokens; epoch = one pass through the dataset. Table 2-5's Chinchilla 70B/1.4T is the compute-optimal outlier.
- **Prefill vs decode:** prefill processes *input* in parallel (builds K/V state); decode generates *output* one token at a time. The output bottleneck remains sequential.
- **Attention was introduced before the transformer** (2014, used in GNMT 2016); the transformer's contribution was using attention *without RNNs*.
- **Q, K, V roles:** Q = current decoder state (the seeker); K = token identity (page number); V = token content (page content). Dot product of Q and K gives attention weight.
- **Activation functions:** GPT-2 used ReLU, GPT-3 used GELU — don't swap them. ReLU(x) = max(0,x).
- **Context length vs parameter count:** longer context affects memory footprint but not the number of parameters.
- **Mixtral 8x7B arithmetic:** 8×7B = 56B raw, but shared params make it **46.7B** total, with **12.9B active** per token (2 experts). Cost/speed = 12.9B model, not 46.7B.
- **Sparsity:** a 90%-sparse 7B model has 700M non-zero parameters; large sparse models can be cheaper to run than small dense ones.
- **Memory minimum:** 7B params × 2 bytes = 14GB — this is a *minimum*; actual memory is higher (Ch 7).
- **Chinchilla 20× rule:** training tokens ≈ 20 × parameters (3B → 60B tokens); scale both equally. Applies to dense models on human data; adapting to MoE/synthetic data is open research.
- **Compute cost formula:** cost = machines × price/h × hours / utilization. GPT-3-175B ≈ 236 days / ~$4M (256 H100s, $2/h, 70% util).
- **Utilization benchmarks:** ~50% is okay, >70% is great — not 90%.
- **Inverse Scaling Prize:** 11 third prizes awarded; **no second or first prizes** — a common trap (people remember $100,000 first prize but forget none was awarded).
- **Llama 3-8B beats Llama 2-70B on MMLU:** newer generation outperforms older same-family larger models — size alone isn't everything.
- **Scaled equally:** compute-optimal training scales model size AND tokens together; don't grow only one.
- **Emergent abilities make scaling extrapolation less accurate** — they don't appear in small models.
- **SFT vs preference finetuning:** SFT = demonstration (prompt, response) data / behavior cloning; preference finetuning = reward/RL (RLHF, DPO, RLAIF). "Instruction finetuning" is ambiguous — the book avoids it.
- **Pre-training vs post-training compute:** InstructGPT = 98% pre-training, 2% post-training. Post-training *unlocks* capabilities, doesn't train from scratch.
- **Pointwise vs comparison labels:** pointwise scores vary (5 vs 7); comparison (A>B) is easier and what reward models use. InstructGPT UI collected 1–7 scores but **only ranking trained the reward model**.
- **Best of N ≠ RLHF:** Stitch Fix/Grab skip RL and just generate multiple outputs and keep the highest reward-model-scored one.
- **Weak model can judge stronger model:** judging is easier than generation.
- **Temperature 0:** technically can't be 0 (division by zero); in practice = arg max (greedy). Higher T = more creative/chaotic, lower T = more consistent/boring. Providers cap at ~2.
- **Logprob sign:** logprob values are negative (log of values in [0,1]); longer sequences → lower total logprob → use *average* logprob to avoid short-sequence bias.
- **Top-k vs top-p:** top-k fixes the count (k=50–500); top-p fixes cumulative probability (0.9–0.95), dynamic per context. Top-p does NOT reduce softmax computation; top-k does.
- **Sampling-scaling numbers:** OpenAI peak = 400 outputs (then decreases); Stanford = log-linear to 10,000. Two different results — don't mix them.
- **Verifier boost:** verifiers ≈ 30× model-size increase (100M + verifier ≈ 3B without).
- **Gemini MMLU:** 32 sampled outputs per question (CoT@32) — sampling multiple outputs, not a special prompt.
- **LinkedIn YAML:** defensive parser lifted correctness 90% → 99.99%; they chose YAML over JSON because YAML is less verbose (fewer tokens).
- **JSON mode guarantee:** guarantees valid JSON syntax, not content; truncation at max tokens can still break parseability.
- **Structured-output layers:** prompting/post-processing/test-time-compute are "bandages"; constrained sampling and finetuning are "intensive treatment." Only finetuning (with architecture modification) can *guarantee* formats like a classifier head.
- **Inconsistency has two scenarios:** same input→different outputs; slightly different input→drastically different outputs. Fixing sampling variables helps the first, not the second.
- **Seed:** starting point of the random number generator — fixing it (plus temperature/top-p/top-k) reduces but never eliminates inconsistency (hardware can differ too).
- **Two hallucination hypotheses:** self-delusion (Ortega/DeepMind; model can't distinguish given vs generated data — about *self-supervision*; = snowballing hallucinations per Zhang et al.) vs mismatched internal knowledge (Gao/Schulman; labeler knows more than model — about *supervision*). They complement each other.
- **RLHF and hallucination:** OpenAI (Schulman) says RLHF helps; InstructGPT paper shows RLHF *worsened* hallucinations — yet humans preferred the RLHF model. Both can be true.
- **Burmese tokenization:** median token length 72 vs English 7 → ~10× slower and ~10× costlier, not "10× more accurate."
- **Common Crawl ≠ C4:** C4 is Google's *clean subset* of Common Crawl; Common Crawl is the raw nonprofit crawl.

### Chapter Summary
Chapter 2 surveys the core design decisions behind foundation models so you can choose and adapt them wisely. **Training data** — models inherit the distribution of whatever data is available (Common Crawl, C4), which is English-dominated and noisy; curating data for under-represented languages and specific domains (multilingual models, AlphaFold/BioNeMo/Med-PaLM2) is often necessary because general-purpose models underperform where their training data under-represents the task. **Modeling** — the **transformer** dominates because attention fixes seq2seq's two flaws (final-hidden-state-only decoding and sequential RNN processing); inference splits into parallel prefill and sequential decode. Model **size** is captured by three numbers — parameters (capacity), training tokens (learning), FLOPs (training cost); the **Chinchilla scaling law** says compute-optimal training needs ~20 tokens per parameter and equal scaling of both; scaling bottlenecks are **data** (running out of internet text; restrictions; AI-generated data) and **electricity**. **Post-training** fixes the two pre-training problems (completion not conversation; toxic outputs) via **SFT** (demonstration data, behavior cloning) and **preference finetuning** (RLHF reward model trained on comparison data; DPO; best of N). **Sampling** — the model picks outputs probabilistically via logits→softmax, controlled by **temperature** (creativity), **top-k** (fixed count), **top-p** (cumulative probability), and stopping conditions; **test time compute** (best of N, verifiers ≈ 30× size boost, self-consistency) and **structured outputs** (prompting → post-processing → test-time compute → constrained sampling → finetuning) shape outputs in production. Finally, the probabilistic nature of sampling causes **inconsistency** (same/slightly-different inputs → different outputs) and **hallucination** (self-delusion and mismatched-knowledge hypotheses) — so AI engineering must build workflows around this probabilistic nature, starting with a rigorous evaluation pipeline (Chapters 3–4).

### Confidence Check
- **Sure:** training data distribution effects (Common Crawl/C4, English dominance 45.88%, low-resource languages, Table 2-1/2-2 ratios); MMLU (14,000 Qs, 57 subjects); MASSIVE token-length numbers (7/32/72) and cost implications; domain-specific model examples; the seq2seq problems and transformer/attention solution; Q/K/V roles and attention formula; prefill/decode; multi-head math (Llama 2-7B 4096/32=128); transformer block composition; Table 2-4 Llama dims; alternative architectures (RWKV, SSM/S4/H3/Mamba/Jamba) with key claims; the three scale numbers; sparse/MoE (Mixtral 46.7B/12.9B active); token-based dataset sizing (Llama 1.4T/2T/15T; RedPajama 30T); Table 2-5; FLOP/FLOP/s/FLOP/s-day distinctions; the 236-day/~$4M GPT-3-175B computation; utilization thresholds; Chinchilla 20× rule; inverse scaling & prize results (11 third, no first/second); scaling bottlenecks (data, electricity, C4 28%→45%, 1–2%→4–20% electricity); post-training two-step (SFT + preference); demonstration/comparison data definitions and costs ($10/pair→$130k; $3.50 vs $25); reward-model loss formula; PPO; best of N; softmax; temperature (logits [1,2] example, 0.7 recommendation, T=0=arg max); logprobs & underflow; top-k/top-p/min-p; stopping conditions; test-time compute techniques and numbers (verifier ≈30×, 400-sample peak, log-linear to 10,000, Gemini 32 samples, best_of average logprob); structured outputs scenarios, frameworks, five layers, LinkedIn 90%→99.99%, JSON mode caveats; inconsistency scenarios and mitigations (cache, sampling vars, seed, hardware); hallucination hypotheses and mitigations; RLHF-vs-hallucination nuance.
- **Uncertain:** exact percentages in some figures (Figure 2-3 domain distribution values not fully enumerated in the text; Figure 2-14 token probabilities beyond red/green); precise GPT-4o regex strings (email/date regexes reproduced as printed — may contain OCR artifacts); exact ordering of some Llama model dimensions in Table 2-4 (verified against the printed values); some footnote/citation page numbers; the precise pairing of some Table 2-3 benchmark values (ImageNet v2 shows "–" for CLIP).

---

## §2. Code & Pseudocode Breakdown

This chapter is conceptual but includes formulæ, tables, pseudocode, and example prompts. Below are the key "algorithmic" breakdowns.

### Softmax (logits → probabilities)
Given a vocabulary of N and logit vector x₁, x₂, …, x_N:
```
p_i = softmax(x_i) = e^(x_i) / Σ_j e^(x_j)
```
- Logits don't sum to 1 and can be negative; softmax produces non-negative probabilities summing to 1.
- Softmax requires two passes over all values (exponential sum, then per-value division) → computationally expensive for large vocabularies (motivates top-k).

### Temperature-adjusted sampling
```
adjusted_logit_i = x_i / T
p_i = softmax(adjusted_logit_i)
```
- Example, logits [1, 2]: T=1 → [0.27, 0.73]; T=0.5 → [0.12, 0.88]; as T→0, P(B)→1; providers cap T in [0, 2]; T=0 means arg max (greedy, no softmax).

### Attention
```
K = x W_K;  V = x W_V;  Q = x W_Q
Attention(Q, K, V) = softmax( Q Kᵀ / √d ) V
```
- Llama 2-7B: hidden dim 4096 (each Q/K/V/projection matrix 4096×4096); 32 heads → per-head dim 128 = 4096/32.
- Attention weight = dot product of Q with K; higher score → more of that token's V is used. Multi-head outputs are concatenated then transformed by the output projection matrix.

### Transformer inference: prefill + decode
```
# Prefill: process input in parallel
process all input tokens in parallel  -> store K/V for all input tokens

# Decode: generate one token at a time
for each output position:
    attend over all previous tokens (input + generated)
    sample/select the next token from the softmax distribution
```

### Reward model training (InstructGPT-style RLHF)
```
# Data: (x, y_w, y_l)  prompt, winning response, losing response
s_w = rθ(x, y_w)        # reward-model score for winning response
s_l = rθ(x, y_l)        # reward-model score for losing response
loss = -log( σ( s_w - s_l ) )    # σ = sigmoid
# Goal: find θ that minimizes the expected loss over all training samples
```
- Then PPO: sample prompts, generate responses, maximize reward-model scores.
- Best of N alternative: generate N outputs, keep the one with the highest reward-model score (no RL).

### Sampling strategies (single next token)
```
logits = model(context)                 # logit vector, one per vocab token
# Greedy:              token = argmax(logits)
# Temperature:         probs = softmax(logits / T);  token ~ probs
# Top-k:               keep top-k logits; probs = softmax(top-k);  token ~ probs
# Top-p (nucleus):     sort probs desc; keep smallest set with cumulative prob > p;  token ~ those
# Min-p:               keep tokens with prob >= p_min
# Constrained:         keep only logits allowed by grammar; sample among valid tokens
```

### Sequence probability and logprob
```
p("I love food") = p("I") × p("love" | "I") × p("food" | "I", "love")
                 = 0.2 × 0.1 × 0.3 = 0.006
logprob("I love food") = logprob("I") + logprob("love") + logprob("food")   # sum, not product
average_logprob = sum_of_logprobs / sequence_length                          # avoids short-sequence bias
```
- Selection: pick the output with the highest average logprob (OpenAI API `best_of`), the highest reward-model/verifier score, the most frequent answer (self-consistency), or a task heuristic.

### Test time compute cost scaling
```
2 outputs ≈ 2× the cost of 1 output      # input may be processed once and reused
OpenAI (2021): quality improves with more samples but peaks at 400, then decreases
Brown et al. (2024, "Monkey Business"): solved problems grow log-linearly from 1 to 10,000 samples
```

### Example prompts (structured outputs, GPT-4o)
```
System: Given an item, create a regex that represents all the ways the item can be written.
        Return only the regex.
        Example: US phone number -> \+?1?\s?(\()?(\d{3})(?(1)\))[-.\s]?(\d{3})[-.\s]?(\d{4})
User:   Email address ->
GPT-4o: [a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
User:   Dates ->
GPT-4o: (?:\d{1,2}[\/\-\.])(?:\d{1,2}[\/\-\.])?\d{2,4}
```

### Reinforcement-learning hallucination mitigation (Ortega et al., DeepMind)
- Make the model distinguish **observations about the world** (user-provided prompts) from the **model's actions** (tokens the model generates).
- Supervised alternative: include **factual and counterfactual signals** in training data.

### Compute/cost estimation for training
```
# GPT-3-175B = 3.14 × 10^23 FLOPs; H100 NVL = 5.2 × 10^18 FLOPs/day
days = 3.14e23 / (256 machines × 5.2e18) ≈ 236 days ≈ 7.8 months
cost = 256 × $2/h × 24h × 256 days / 0.70 ≈ $4,142,811.43
```
