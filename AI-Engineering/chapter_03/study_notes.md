# Chapter 3 Study Bundle: Evaluation Methodology
**Source:** *AI Engineering* (Chip Huyen), Chapter 3

---

## §1. Study Notes

### Core Theme
Chapter 3 explains **how to evaluate open-ended foundation models** — the biggest hurdle teams face when bringing AI applications to reality. The chapter is the first of two on evaluation (this one covers *methods*; Chapter 4 covers *model selection and building an evaluation pipeline*). Because evaluation must be considered in the context of a whole system, teams must first identify where their system is likely to fail and design evaluation around those failures — no amount of metrics can make a system robust if you don't know where it breaks. The chapter covers: (1) **why evaluating foundation models is harder than evaluating traditional ML models** — more intelligent outputs are harder to judge, open-endedness defeats ground-truth comparison, models are black boxes, benchmarks saturate fast, and the scope of evaluation has expanded to discovering new capabilities; (2) **language modeling metrics** — entropy, cross entropy, bits-per-character (BPC), bits-per-byte (BPB), and perplexity, which guide LM training and correlate with downstream performance; (3) **exact evaluation** — functional correctness (e.g., pass@k on code) and similarity measurements against reference data (exact match, lexical similarity like BLEU/ROUGE/edit distance, semantic similarity with embeddings); (4) **AI as a judge** — using AI models to evaluate AI responses (fast, cheap, no reference data needed, but with inconsistency, criteria ambiguity, cost/latency, and biases like self-bias, position bias, and verbosity bias); and (5) **ranking models with comparative evaluation** — pointwise vs pairwise (win-rate) ranking, rating algorithms (Elo, Bradley–Terry, TrueSkill), and the challenges of scalability, standardization/quality control, and converting relative performance to absolute performance. The chapter's throughline: systematic evaluation is required to mitigate risks and uncover opportunities, and subjective methods (AI judges) must be **supplemented** with exact evaluation and/or human evaluation.

### Key Definitions
- **Evaluation:** The process of assessing a model's/system's outputs to mitigate risks and uncover opportunities; for many applications, figuring out evaluation can take up the majority of the development effort.
- **Exact evaluation:** Produces judgment without ambiguity (e.g., "the MCQ answer is A, you picked B → wrong"). Approaches in this chapter: **functional correctness** and **similarity measurements against reference data**.
- **Subjective evaluation:** Depends on who/what grades the output and how it's prompted (e.g., essay grading, AI as a judge); the score depends on the judge model and prompt.
- **Word of mouth / vibe check:** Evaluating models informally (a16z 2023: 6 of 70 decision makers evaluated models this way); ad hoc and unreliable, creates risk and slows iteration.
- **Benchmark saturation:** A benchmark becomes saturated once a model achieves a perfect score; with foundation models, benchmarks saturate fast (GLUE 2018 → saturated in a year → SuperGLUE 2019; NaturalInstructions 2021 → Super-NaturalInstructions 2022; MMLU 2020 → largely replaced by MMLU-Pro 2024).
- **Language modeling metrics:** Cross entropy, perplexity, bits-per-character (BPC), and bits-per-byte (BPB) — closely related; if you know one you can compute the others given the necessary information. All can be used for any model generating token sequences (including non-text tokens).
- **Entropy (H):** Measures how much information, on average, a token carries; the higher the entropy, the more bits needed to represent each token, and the harder it is to predict what comes next. Introduced by Claude Shannon in 1951 ("Prediction and Entropy of Printed English").
- **Cross entropy (H(P,Q)):** Measures how difficult it is for a language model (learned distribution Q) to predict what comes next in a dataset (true distribution P); `H(P,Q) = H(P) + DKL(P‖Q)`. Trained LMs minimize it; it's not symmetric.
- **KL divergence (DKL(P‖Q)):** The divergence of the model's learned distribution Q with respect to the true distribution P; zero if the model learns the training distribution perfectly.
- **Bit / nat:** Units of entropy/cross entropy. Bit = base 2; nat = natural log, base e (used by TensorFlow and PyTorch). Perplexity = 2^(cross entropy in bits) or e^(cross entropy in nats).
- **Bits-per-character (BPC):** Number of bits per character; makes token-level metrics comparable across models with different tokenizers. Complicated by encoding schemes (ASCII = 7 bits/char; UTF-8 = 8–32 bits/char).
- **Bits-per-byte (BPB):** Number of bits to represent one byte of the original training data; a more standardized metric than BPC. Cross entropy tells you how efficiently a model can compress text (BPB 3.43 → compresses each original 8-bit byte to 3.43 bits → less than half original size).
- **Perplexity (PPL):** The exponential of entropy/cross entropy; measures the model's uncertainty in predicting the next token. Higher uncertainty = more possible options for the next token.
- **Functional correctness:** Evaluating a system based on whether it performs the intended functionality (e.g., does the generated website meet requirements?); the ultimate metric for any application; not always easy to measure or automate (code generation is a task where it can be).
- **pass@k:** For each problem, generate k code samples; a model solves a problem if any of the k samples passes all test cases; the score is the fraction of solved problems. In expectation pass@1 ≤ pass@3 ≤ pass@10.
- **Reference data / ground truths / canonical responses:** Reference responses paired with inputs in the format (input, reference responses); an input can have multiple reference responses. Metrics that require references are **reference-based**; those that don't are **reference-free**.
- **Exact match:** The generated response matches one of the reference responses exactly; works for short, exact responses (math, common knowledge, trivia, account balances); a looser variation accepts any output containing the reference response (which can wrongly accept "September 12, 1929" as correct for "What year was Anne Frank born?").
- **Lexical similarity:** Measures how much two texts overlap (by tokens, edit distance, or n-grams); metrics include BLEU, ROUGE, METEOR++, TER, CIDEr. Drawback: requires a comprehensive reference set and high lexical similarity ≠ better responses.
- **Edit distance (fuzzy matching / approximate string matching):** The number of edits (deletion, insertion, substitution; some add transposition) to convert one text to another; "bad" is 1 edit from "bard" and 3 from "cash".
- **n-gram:** A sequence of n tokens; 1-gram = unigram (a token), 2-gram = bigram. "My cats scare the mice" has four bigrams.
- **Semantic similarity (embedding similarity):** How close two texts are in meaning, computed by transforming texts into embeddings and measuring similarity (e.g., cosine similarity). Metrics: BERTScore, MoverScore.
- **Embedding:** A numerical representation (vector) that captures the meaning of the original data; size typically 100–10,000. Specialized models: BERT, CLIP, Sentence Transformers. Evaluated by MTEB (Massive Text Embedding Benchmark).
- **Cosine similarity:** `cos(A,B) = (A·B) / (‖A‖‖B‖)`; identical embeddings → 1, opposite embeddings → –1. A·B = dot product; ‖A‖ = Euclidean (L2) norm.
- **Joint/multimodal embedding space:** A space where data of different modalities (e.g., text and images) can be compared and combined; enables text-based image search. CLIP (text+images), ULIP (text, images, 3D point clouds), ImageBind (six modalities).
- **AI as a judge (LLM as a judge):** Using AI to evaluate AI responses; the AI model used is the **AI judge**. The judgment is subjective — it depends on the judge model and prompt. An AI judge is a *system*: model + prompt + sampling parameters.
- **Self-bias:** AI judges favor their own responses (GPT-4 favored itself with a 10% higher win rate; Claude-v1 25%, per Zheng et al., 2023).
- **First-position bias:** AI judges favor the first answer in a pairwise comparison (opposite of humans' **recency bias**, favoring the last-seen answer); mitigated by repeating tests with different orderings.
- **Verbosity bias:** Favoring lengthier answers regardless of quality; Wu and Aji (2023) found GPT-4 and Claude-1 prefer ~100-word responses with factual errors over ~50-word correct responses; Saito et al. (2023) found when one response is ~2× longer, the judge almost always prefers it.
- **Self-evaluation / self-critique:** A model judging its own response; sounds like cheating (self-bias), but is great for sanity checks and nudges the model to revise/improve (e.g., "What's 10+3?" → "30" → self-critique → "13").
- **Specialized judges:** Small, specialized models trained to make specific judgments. Three types: **reward model** (scores (prompt, response); e.g., Cappy, 360M params, score 0–1), **reference-based judge** (evaluates against reference responses; e.g., BLEURT, Prometheus), and **preference model** (predicts which of two responses users prefer; e.g., PandaLM, JudgeLM).
- **Pointwise evaluation:** Evaluating each model independently (e.g., Likert scale) then ranking by scores.
- **Comparative evaluation:** Evaluating models against each other (side-by-side) and computing a ranking from comparison results; each comparison is a **match**; the probability A is preferred over B is A's **win rate** over B.
- **Rating algorithm:** Computes a per-model score from comparative signals and ranks models by those scores; e.g., Elo, Bradley–Terry, TrueSkill (borrowed from sports/video games).
- **Transitivity assumption:** If A ranks higher than B and B higher than C, infer A ranks higher than C without needing a direct A-vs-C comparison; unclear whether it holds for AI models (human preference isn't necessarily transitive; different pairs may be evaluated by different evaluators on different prompts).
- **Preference model:** A specialized AI judge that predicts which response users prefer; motivated because preference signals are expensive to collect and are needed both for evaluation and post-training alignment.

### Core Concepts & Frameworks
- **Why evaluation matters (chapter motivation):**
  - Real failures: a man committed suicide after being encouraged by a chatbot; lawyers submitted hallucinated false evidence; Air Canada was ordered to pay damages when its AI chatbot gave a passenger false information. Without quality control of AI outputs, the risk of AI may outweigh its benefits.
  - Greg Brockman (OpenAI cofounder, December 2023): "evals are surprisingly often all you need."
  - Evaluation should be considered in the context of a whole system, not in isolation: first identify likely failure points and design evaluation around them (may require redesigning the system for visibility into failures).
- **Why foundation models are harder to evaluate (four reasons):**
  1. **More intelligent → harder to evaluate:** Most people can spot a first grader's math error; few can judge PhD-level math. Evaluating sophisticated outputs requires fact-checking, reasoning, and domain expertise (corollary: evaluation becomes much more time-consuming). Footnote: when GPT-o1 came out (Sept 2024), Fields medalist Terence Tao compared it to "a mediocre, but not completely incompetent, graduate student"; the joke became: if we need the brightest humans to evaluate AI, no one will be qualified to evaluate future models.
  2. **Open-endedness defeats ground truths:** Traditional ML is close-ended (classification outputs only expected categories → compare against expected outputs). Open-ended tasks have so many possible correct responses that a comprehensive list of correct outputs is impossible to curate.
  3. **Black boxes + fast-saturating benchmarks:** Model providers often don't expose architecture/training data/training process, so models can be evaluated only by observing outputs. Public benchmarks capture too little and saturate fast: GLUE (2018) saturated in a year → SuperGLUE (2019); NaturalInstructions (2021) → Super-NaturalInstructions (2022); MMLU (2020) → MMLU-Pro (2024).
  4. **Expanded scope of evaluation:** General-purpose models aren't evaluated only on known tasks; evaluation must also *discover new tasks* the model can do — including tasks beyond human capabilities. Evaluation now explores the potential and limitations of AI.
- **Investment in evaluation lags behind the rest of AI engineering:**
  - LLM evaluation papers grew exponentially in the first half of 2023: from 2 papers/month to ~35/month (Figure 3-1, Chang et al., 2023).
  - In Chip's analysis of the top 1,000 AI repositories on GitHub (by stars), >50 repositories were dedicated to evaluation (as of May 2024); the growth curve is exponential (Figure 3-2).
  - Balduzzi et al. (DeepMind): "developing evaluations has received little systematic attention compared to developing algorithms"; experiment results are almost exclusively used to improve algorithms, rarely to improve evaluation.
  - Anthropic called on policymakers to increase government funding/grants for developing evaluation methodologies and analyzing the robustness of existing evaluations.
  - The number of evaluation tools is small compared to modeling/training and AI-orchestration tools (Figure 3-3).
  - Result: many teams eyeball results or use a small set of ad hoc go-to prompts based on the curator's personal experience rather than the application's needs. This works for getting a project off the ground but not for iteration → the book pushes a *systematic* approach.
- **Language modeling metrics framework:**
  - A language model encodes statistical information about languages (given "I like drinking __", "tea" is more likely than "charcoal"). The more statistical info captured, the better the next-token prediction and the lower the training cross entropy. Closer your data is to the model's training data → better performance.
  - Entropy example (Figure 3-4): a language with 2 position tokens (upper/lower) needs 1 bit → entropy 1; with 4 tokens (upper-left/upper-right/lower-left/lower-right) needs 2 bits → entropy 2. Lower entropy = more predictable.
  - Cross entropy decomposition: with P = true distribution, Q = learned distribution: H(P) = training data's entropy; DKL(P‖Q) = divergence; H(P,Q) = H(P) + DKL(P‖Q). Cross entropy is not symmetric (H(P,Q) ≠ H(Q,P)). If a model learns perfectly, its cross entropy equals the training data's entropy and KL divergence = 0.
  - Units: cross entropy 6 bits → the model needs 6 bits per token. Because tokenizers differ across models, compare BPC/BPB instead: e.g., 6 bits/token ÷ 2 chars/token = BPC 3. ASCII encodes each char in 7 bits; with BPC 3 and 7 bits/char (⅞ byte): BPB = 3 ÷ ⅞ = 3.43. A BPB of 3.43 means the model compresses each original byte (8 bits) to 3.43 bits → less than half the original size.
  - Perplexity = 2^(entropy/cross entropy in bits), or e^(·) if measured in nats (TensorFlow/PyTorch use nats). 2 bits → perplexity 4 (choose among 2² = 4 options, e.g., the 4 position tokens). Confusion between bit and nat leads many to report perplexity rather than cross entropy.
  - Perplexity interpretation rules:
    - **More structured data → lower expected perplexity** (HTML more predictable than everyday text).
    - **Bigger vocabulary → higher perplexity** (children's book < War and Peace; character-based < word-based perplexity).
    - **Longer context → lower perplexity** (Shannon conditioned on up to 10 previous tokens; today models typically use 500–10,000 previous tokens, upper-bounded by max context length).
    - Perplexity as low as 3 is not uncommon → a 1-in-3 chance of predicting the next token correctly on a vocabulary in the 10,000s–100,000s — incredible odds.
    - Perplexity is a proxy for model capability (GPT-2 report: larger models consistently give lower perplexity, Table 3-1), but many companies stopped reporting it.
    - **Post-training caveat:** perplexity typically *increases* after SFT/RLHF ("post-training collapses entropy" — as a model gets better at tasks it may get worse at next-token prediction); quantization can change perplexity in unexpected ways.
    - **Contamination detection:** perplexity is lowest for texts the model saw/memorized during training → low perplexity on a benchmark's data suggests the benchmark leaked into training data. Also used for **deduplication** (add new data only if its perplexity is high) and **detecting abnormal texts** (highest perplexity = unpredictable/gibberish texts like "my dog teaches quantum physics in his free time").
  - Computing perplexity requires the model's probabilities/logprobs; not all commercial models expose logprobs (Chapter 2).
- **Functional correctness:**
  - Measures whether the system performs its intended functionality (generated website meets requirements? reservation succeeds?). The ultimate metric; measurement isn't always straightforward or automatable.
  - **Code generation = automation-friendly:** execution accuracy — run generated code (e.g., `gcd(num1, num2)`) in a Python interpreter and check correct outputs (e.g., gcd(15, 20) must return 5). Unit-test validation is standard practice in software engineering (LeetCode, HackerRank).
  - Benchmarks: HumanEval (OpenAI) and MBPP (Mostly Basic Python Problems Dataset, Google) use functional correctness; text-to-SQL benchmarks Spider (Yu et al., 2018), BIRD-SQL (Li et al., 2023), WikiSQL (Zhong et al., 2017) also rely on it.
  - **pass@k:** each benchmark problem comes with test cases (assert statements); generate k samples per problem; problem solved if any sample passes all test cases; score = fraction solved (10 problems, 5 solved with k=3 → pass@3 = 50%). More samples → greater score in expectation.
  - Other automatable functional-correctness tasks: game bots (Tetris score), tasks with measurable objectives (energy optimization → energy saved). Caveat: many complex tasks have measurable objectives but AI can't do them end-to-end; evaluating part of a solution is sometimes harder than the end outcome (evaluating one chess move vs win/lose/draw).
- **Similarity measurements against reference data:**
  - Used when functional correctness can't be automated (e.g., translation). Reference data = (input, reference responses); generated responses more similar to references are considered better.
  - Bottleneck: reference data generation is slow/expensive (humans, increasingly AI). Human-generated references treat human performance as the gold standard; AI-generated references need human review but much less labor than from-scratch generation.
  - Four ways to measure similarity between two open-ended texts: (1) evaluator judgment whether texts are the same; (2) exact match (binary); (3) lexical similarity (sliding scale, e.g., 0–1 or –1 to 1); (4) semantic similarity (sliding scale). AI evaluators increasingly common (next section).
  - Similarity measurements apply beyond evaluation: retrieval/search, ranking, clustering, anomaly detection, data deduplication.
- **Exact match details:**
  - Works for short, exact responses ("What's 2+3?", "Who was the first woman to win a Nobel Prize?", "What's my current account balance?", analogies).
  - Variation: accept any output *containing* the reference ("The answer is 5" contains "5") — but this can wrongly accept wrong answers ("September 12, 1929" contains "1929" for Anne Frank's birth year, but the full date is wrong).
  - Fails for complex tasks: "Comment ça va?" has many valid translations ("How are you?", "How is everything?", "How are you doing?") — "How is it going?" would be marked wrong; the longer/complex the text, the more possible translations → exhaustive reference sets are impossible.
- **Lexical similarity details:**
  - Simple form: count common tokens. Reference "My cats scare the mice" (5 words): response A "My cats eat the mice" = 4/5 = 80%; response B "Cats and mice fight all the time" = 3/5 = 60% → A more similar.
  - **Edit distance:** deletion ("brad"→"bad"), insertion ("bad"→"bard"), substitution ("bad"→"bed"); some matchers treat transposition ("mats"→"mast") as one edit, others as two (deletion + insertion). "bad" is 1 edit from "bard" and 3 from "cash".
  - **n-gram similarity:** percentage of reference n-grams also present in the generated response (may preprocess so "cats"/"cat" or "will not"/"won't" are/aren't distinct tokens).
  - Metrics: BLEU, ROUGE, METEOR++, TER, CIDEr — differ in how overlap is calculated; used by WMT, COCO Captions, GEMv2. Fewer benchmarks use lexical similarity since the rise of foundation models.
  - Drawbacks: (1) requires a comprehensive reference set — a good response gets a low score if the reference set lacks similar responses (Adept's Fuyu scored poorly on image-captioning despite correct captions because correct answers were missing from references; Figure 3-5); (2) references can be wrong (WMT 2023 Metrics shared task found many bad reference translations; low-quality references help explain why reference-free metrics rival reference-based metrics in correlation with human judgment, Freitag et al., 2023); (3) higher lexical similarity ≠ better responses (on HumanEval, OpenAI found BLEU scores similar for correct and incorrect solutions → optimizing BLEU ≠ optimizing functional correctness, Chen et al., 2021).
- **Semantic similarity details:**
  - Lexical ≠ semantic: "What's up?" vs "How are you?" are lexically different but semantically close; "Let's eat, grandma" vs "Let's eat grandma" look similar but mean different things.
  - Requires transforming text into an **embedding** (numerical representation, e.g., "the cat sits on a mat" → [0.11, 0.02, 0.54]); similarity computed with cosine similarity (identical → 1; opposite → –1). Applicable to any modality (text = **semantic textual similarity**).
  - Semantic similarity sits in the exact-evaluation category but can be considered subjective because different embedding algorithms produce different embeddings; given two embeddings, the similarity score is computed exactly.
  - Cosine formula: cos(A,B) = (A·B) / (‖A‖·‖B‖); A·B = dot product; ‖A‖ = Euclidean (L2) norm. Metrics: BERTScore (BERT embeddings), MoverScore (mixture of algorithms).
  - Drawbacks: reliability depends on embedding quality (same-meaning texts can get low scores with bad embeddings); the embedding algorithm may need nontrivial compute/time.
- **Introduction to embedding:**
  - Embeddings are vectors capturing meaning; typical size 100–10,000 elements. Models: BERT, CLIP (Contrastive Language–Image Pre-training), Sentence Transformers; proprietary embedding APIs. Table 3-2 sizes: BERT base 768 / large 1024; CLIP image 512 / text 512; OpenAI text-embedding-3-small 1536 / text-embedding-3-large 3072; Cohere embed-english-v3.0 1024 / light 384.
  - GPTs and Llamas also generate embeddings (embedding layer in the transformer); intermediate-layer embeddings can be extracted but quality may be worse than specialized embedding models.
  - An embedding is a *lower-dimensional* representation of complex data (a 10,000-element vector is far lower-dimensional than raw data).
  - Word-embedding models (word2vec, GloVe) vs document embeddings.
  - A good embedding algorithm places more-similar texts closer (cosine similarity or related metrics): "the cat sits on a mat" closer to "the dog plays on the grass" than to "AI research is super fun". Embedding quality also evaluated by utility on tasks (classification, topic modeling, recommender systems, RAG); MTEB (Massive Text Embedding Benchmark, Muennighoff et al., 2023) measures embedding quality across multiple tasks.
  - Non-text embeddings: Criteo/Coveo product embeddings, Pinterest embeddings for images/graphs/queries/users.
  - Joint/multimodal embeddings: CLIP (text + images, first major such model, Radford et al., 2021), ULIP (language, images, 3D point clouds, Xue et al., 2022), ImageBind (six modalities including text, images, audio, Girdhar et al., 2023). CLIP training: for each (image, text) pair, a text encoder and an image encoder project both into a joint embedding space; training goal is to bring matching image/text embeddings close together (Figure 3-6). Enables text-based image search (image of a man fishing closer to "a fisherman" than "fashion show").
- **AI as a judge:**
  - History: idea of automating evaluation with AI has existed for a long time (Chip's 2017 NeurIPS MEWR workshop on reference-free MT evaluation), but became practical around 2020 with GPT-3. Now one of the most common methods for evaluating AI models in production (most AI-evaluation-startup demos in 2023–2024; LangChain State of AI 2023: 58% of evaluations on their platform used AI judges).
  - **Why AI judges:** fast, easy, relatively cheap vs humans; work without reference data (production-friendly); judge on any criteria (correctness, repetitiveness, toxicity, wholesomeness, hallucinations); each AI model is an aggregation of the masses → judgments can represent the masses; can *explain* decisions (useful for auditing, Figure 3-7).
  - **Correlation with humans:** Zheng et al. (2023) on MT-Bench: GPT-4–human agreement 85% > human–human agreement 81%; AlpacaEval authors (Dubois et al., 2023): near-perfect 0.98 correlation with LMSYS's Chatbot Arena leaderboard (human-evaluated).
  - **Three usage modes (with example prompts):** (1) evaluate a response by itself given the question (score 1–5); (2) compare a generated response to a reference (True/False) — alternative to human-designed similarity; (3) compare two generated responses (output A or B) — generates preference data for post-training alignment (Ch 2), test-time compute (Ch 2), and comparative evaluation. A general-purpose judge can evaluate any criterion ("Would Gandalf say this?", "rate product-photo trustworthiness 1–5").
  - Built-in criteria examples (Table 3-3, Sept 2024): Azure AI Studio (groundedness, relevance, coherence, fluency, similarity); MLflow.metrics (faithfulness, relevance); LangChain Criteria Evaluation (conciseness, relevance, correctness, coherence, harmfulness, maliciousness, helpfulness, controversiality, misogyny, insensitivity, criminality); Ragas (faithfulness, answer relevance). **Criteria aren't standardized** — Azure's "relevance" may differ from MLflow's.
  - **Prompting an AI judge** should clearly explain: (1) the task; (2) the criteria (more detailed = better); (3) the scoring system — classification (good/bad; relevant/irrelevant/neutral), discrete numerical (1–5; a special case of classification with numerical interpretation), or continuous numerical (0–1). LMs are better with text than numbers: classification > numerical; discrete > continuous; wider discrete ranges → worse. Include examples of each score. Azure AI Studio's relevance prompt illustrates task + criteria + scoring + low-score example + justification.
  - An AI judge is **not just a model** — it's a system of model + prompt + sampling parameters; changing any of them yields a different judge.
- **Limitations of AI as a judge:**
  - **Inconsistency:** probabilistic; same judge + same input can give different scores across runs; hard to reproduce/trust. Mitigate with sampling variables (Ch 2); Zheng et al. (2023): adding evaluation examples raised GPT-4 consistency 65% → 77.5%, but high consistency ≠ high accuracy (consistent mistakes are possible), and longer prompts quadrupled GPT-4 spending.
  - **Criteria ambiguity:** metrics aren't standardized; MLflow, Ragas, and LlamaIndex all have a "faithfulness" criterion but with different instructions and scoring (Table 3-4): MLflow 1–5, Ragas 0/1, LlamaIndex YES/NO — scores aren't comparable. Application evaluation should ideally stay fixed so metrics can monitor changes, but AI judges are AI applications and change over time (a judge-team change vs app-team change can be misattributed). **Rule of thumb: do not trust any AI judge if you can't see the model and the prompt used.**
  - **Costs and latency:** GPT-4 generating + evaluating = ~2× API calls; three criteria → ~4× calls (evaluation can take up the majority of the budget). Reduce costs: weaker judge models; **spot-checking** (= sampling — evaluate only a subset; larger sample → more confidence but higher cost; finding the balance takes trial and error, Ch 4). Production guardrail use adds latency (evaluate before returning responses → trade-off reduced risk vs increased latency; possibly a nonstarter for strict-latency apps). AI judges are still much cheaper than human evaluators.
  - **Biases:** self-bias (GPT-4 +10% win rate for itself; Claude-v1 +25%); first-position bias (AI favors the first answer — opposite of human recency bias; mitigate with multiple orderings/careful prompts); verbosity bias (favor longer answers; Wu and Aji 2023: prefer ~100-word responses with factual errors over ~50-word correct ones; Saito et al. 2023: ~2× longer → almost always preferred; GPT-4 less prone than GPT-3.5 — may fade as models strengthen; humans also favor longer responses but far less). Plus all-AI limitations: privacy/IP (sending data to a proprietary judge; unknown training data → commercial-safety uncertainty).
  - Despite limitations, adoption will grow; AI judges **should be supplemented** with exact evaluation and/or human evaluation.
- **What models can act as judges?**
  - Judge can be **stronger**, **weaker**, or **same** as the model judged. A stronger judge makes better judgments and can improve weaker models; you still use a weaker model to *generate* because of cost/latency (e.g., cheap in-house model generates, GPT-4 evaluates 1% of responses; or fast model generates while slow strong model evaluates in the background, with remedy actions like replacing bad responses). The opposite pattern (strong generates, weak evaluates) is also common.
  - Two challenges of stronger-as-judge: (1) the strongest model has no eligible judge; (2) you need an alternative evaluation method to determine which model is strongest.
  - **Self-evaluation/self-critique:** model judging itself — sounds like cheating (self-bias) but great for sanity checks; can nudge the model to revise/improve (Press et al., 2022; Gou et al., 2023; Valmeekam et al., 2023). Example: "What's 10+3?" → "30" → "Is this answer correct?" → "No, it's 13." Sometimes called self-ask.
  - **Weaker judge:** judging is easier than generating (anyone can have an opinion about a song). Zheng et al. (2023): stronger models correlate better with human preference → teams opt for the strongest affordable judges, but that study was limited to general-purpose judges. Promising direction: **small, specialized judges** — more reliable than larger general-purpose judges for specific judgments.
  - **Three specialized judges:**
    - **Reward model:** scores (prompt, response); used in RLHF for years. Cappy (Google, 2023): 360M params, score 0–1.
    - **Reference-based judge:** evaluates generated response against reference responses → similarity or quality score. BLEURT (Sellam et al., 2020): (candidate, reference) → similarity (range confusingly ~–2.5 to 1.0 — an example of criteria ambiguity). Prometheus (Kim et al., 2023): (prompt, generated, reference, scoring rubric) → quality 1–5, assuming reference = 5.
    - **Preference model:** (prompt, response 1, response 2) → which is preferred. Exciting direction: predicting human preference makes evaluation easier and models safer; preference data is expensive to collect (Ch 2). Examples: PandaLM (Wang et al., 2023), JudgeLM (Zhu et al., 2023); PandaLM also explains its rationale (Figure 3-9).
- **Comparative evaluation (ranking models):**
  - You often want a *ranking*, not raw scores. **Pointwise:** evaluate each model independently, rank by scores (dancing contest: score each dancer, pick highest). **Comparative:** evaluate models against each other and compute a ranking from comparisons (dancing side-by-side, judges pick the best).
  - For subjective quality, comparative is typically easier than pointwise (easier to say which of two songs is better than to score each). First used in AI by Anthropic (2021); powers LMSYS Chatbot Arena (community pairwise comparisons); many providers use it in production (ChatGPT asks users to compare outputs side-by-side, Figure 3-10). Two or more models respond per request; an evaluator (human or AI) picks the winner; ties allowed to avoid random winners.
  - **Preference vs correctness:** not all questions should be answered by preference — many by correctness (cell-phone-radiation-tumors Yes/No example); preference voting can produce wrong signals that misalign models. Asking users to pick can frustrate them (if you knew the answer you wouldn't ask). Preference-based voting only works if voters are knowledgeable — works when AI is an intern/assistant speeding up tasks users can already do, not when users ask AI to do things they can't.
  - Comparative evaluation **≠ A/B testing**: A/B shows one candidate per user at a time; comparative shows multiple outputs simultaneously.
  - **Win rate:** probability A is preferred over B, computed from all matches between A and B (Table 3-5 shows a match history). With only two models ranking is easy; with more models it gets hard (Table 3-6 shows five models with pairwise win rates where the ranking isn't obvious).
  - **Rating algorithms:** compute a score per model from comparative signals, then rank. Adapted from sports/video games (almost a century old): Elo, Bradley–Terry, TrueSkill. LMSYS Chatbot Arena originally used Elo, switched to Bradley–Terry because Elo is sensitive to the order of evaluators and prompts (they kept scaling Bradley–Terry scores to look like Elo: ×400 + 1000, rescaled so Llama-13b = 800).
  - **Ranking correctness = predictive quality:** a ranking is correct if, for any pair, the higher-ranked model is more likely to win. Model ranking is a predictive problem: compute ranking from historical matches, use it to predict future outcomes; there's no ground truth, and ranking quality = how well it predicts future matches. Chip's analysis: Chatbot Arena's ranking is good, at least for model pairs with sufficient matches.
- **Challenges of comparative evaluation:**
  1. **Scalability bottlenecks:** model pairs grow quadratically with the number of models. LMSYS (Jan 2024): 57 models, 244,000 comparisons ≈ 1,596 pairs → only ~153 comparisons per pair — small for the range of tasks. **Transitivity** helps (A>B, B>C → A>C) but it's unclear if it holds for AI models (papers citing this limitation: Boubdir et al., Balduzzi et al., Munos et al.; human preference isn't necessarily transitive; different pairs are evaluated by different evaluators/prompts). New models must be evaluated against existing ones (can reshuffle existing rankings); private models are hard to evaluate comparatively (must collect own signals / build own leaderboard / pay for private evaluation). Mitigation: better matching algorithms — not all pairs need equal comparison; stop matching confident pairs; sample matches that reduce the most uncertainty.
  2. **Lack of standardization and quality control:** crowdsourcing (Chatbot Arena style): anyone submits a prompt, votes on two anonymous models' responses, names revealed after voting. Pros: wide signal range, relatively hard to game (though gaming attempts grow as it becomes popular). Cons: no standard for what a better response is; volunteers may not fact-check (may prefer sounding-good-but-wrong responses); preference in the wild isn't appropriate for all use cases (a refused inappropriate joke gets downvoted, but a developer may prefer refusal); malicious voters can pick toxic responses; users evaluate outside their working environments (prompts may not reflect real use; unlikely to use sophisticated prompting). Among 33,000 LMSYS prompts (2023): 180 were "hello"/"hi" (0.55%, excluding variations like "hello!"); brainteasers repeated ("X has 3 sisters..." asked 44 times); simple prompts make models hard to differentiate and can pollute rankings. Leaderboards without sophisticated context construction (RAG augmentation) won't reflect real RAG performance — generating well ≠ retrieving well.
     - Standardization options: (a) limit users to predetermined prompts (hurts diversity; LMSYS instead filters to *hard* prompts using an internal model and ranks on those); (b) use only trusted, trained evaluators (Scale's private comparative leaderboard approach — expensive, reduces comparison volume); (c) embed comparative evaluation into products (e.g., two code snippets in an editor; chat apps) — but users may not be experts, may randomly click (noise), though signals from the small % who vote correctly can suffice; (d) some teams prefer AI to human evaluators (AI more reliable than random internet users, even if worse than trained experts).
  3. **From comparative to absolute performance:** comparative tells *which* model is better, not *how good* it is or whether it's good enough. "B better than A" is compatible with B-good/A-bad, both-bad, or both-good. Example: A resolves 70% of support tickets; B wins against A 51% of the time — unclear how that converts to resolved requests (a 1% win-rate change can induce a huge performance boost in some apps and minimal in others). Cost–benefit analysis needs absolute performance: if B costs 2× A, comparative evaluation can't tell whether the boost is worth it.
- **Future of comparative evaluation:**
  - Easier to compare two outputs than score each one; as models surpass human performance, humans may be unable to give concrete scores but may still detect differences → comparative may remain the only option (Llama 2 paper: humans still give valuable feedback when comparing two answers beyond the best human annotators' ability, Touvron et al., 2023).
  - Captures the quality we care about: human preference; reduces pressure to constantly create benchmarks; **never saturates** as long as newer, stronger models are introduced (unlike benchmarks that become useless at perfect scores).
  - Relatively hard to game (no reference data to train on) → many trust public comparative leaderboards more than any other public leaderboard.
  - Provides discriminating signals not obtainable otherwise; offline: a great addition to evaluation benchmarks; online: complementary to A/B testing.
- **Summary thesis:** the stronger AI becomes, the higher the catastrophic-failure potential → evaluation is more important and harder. Human evaluation remains necessary in many cases (humans in the loop for sanity checks), but this chapter focuses on **automatic evaluation**: language modeling metrics (perplexity/cross entropy) for the LM component, exact evaluation (functional correctness, similarity) for open-ended responses, subjective AI-as-a-judge for flexibility, and comparative evaluation for ranking. Subjective metrics are highly dependent on the judge; scores need context and aren't comparable across judges; AI judges change over time so they're unreliable as long-running benchmarks — supplement them with exact evaluation, human evaluation, or both. Comparative evaluation and post-training alignment both need expensive preference signals → motivates **preference models** (specialized AI judges predicting user preference). Building a reliable evaluation pipeline is Chapter 4's topic.

### Important Numbers / Stats / Tokens
- Greg Brockman (Dec 2023): "evals are surprisingly often all you need."
- a16z 2023: 6 out of 70 decision makers evaluated models by word of mouth.
- LLM evaluation papers: 2/month → ~35/month (first half 2023); >50 evaluation repos in top-1,000 GitHub AI repos (May 2024).
- Benchmark saturation timeline: GLUE 2018 → SuperGLUE 2019; NaturalInstructions 2021 → Super-NaturalInstructions 2022; MMLU 2020 → MMLU-Pro 2024.
- Terence Tao on GPT-o1 (Sept 2024): "a mediocre, but not completely incompetent, graduate student."
- Shannon's entropy: introduced 1951, "Prediction and Entropy of Printed English"; entropy of a language = average number of binary digits per letter in the most efficient encoding.
- Entropy example: 2 tokens → 1 bit; 4 tokens → 2 bits.
- Cross entropy: H(P,Q) = H(P) + DKL(P‖Q); not symmetric.
- BPC example: 6 bits/token ÷ 2 chars/token = 3; BPB example: BPC 3, char = 7 bits (⅞ byte) → 3.43.
- Perplexity: PPL = 2^H (bits) or e^H (nats); 2-bit cross entropy → PPL 4; PPL 3 → 1-in-3 chance of correct next token; vocabularies are in the 10,000s–100,000s.
- Context length for perplexity: Shannon up to 10 tokens; today 500–10,000 tokens (upper-bounded by max context).
- Table 3-1 GPT-2 perplexity (LAMBADA PPL): SOTA 99.8; 117M 35.13; 345M 15.60; 762M 10.87; 1542M 8.63 (larger → lower PPL across datasets).
- Post-training typically *increases* perplexity ("collapses entropy"); quantization changes it unexpectedly.
- pass@k: 10 problems, 5 solved with k=3 → pass@3 = 50%; pass@1 < pass@3 < pass@10 in expectation.
- Code benchmarks: HumanEval (OpenAI), MBPP (Google); text-to-SQL: Spider, BIRD-SQL, WikiSQL.
- Lexical similarity example: "My cats scare the mice" → A "My cats eat the mice" 4/5 = 80%; B "Cats and mice fight all the time" 3/5 = 60%.
- Edit distance ops: deletion, insertion, substitution (some add transposition); "bad" 1 edit from "bard", 3 from "cash".
- n-gram example: "My cats scare the mice" = 4 bigrams.
- Lexical metrics: BLEU, ROUGE, METEOR++, TER, CIDEr; benchmark users: WMT, COCO Captions, GEMv2.
- Semantic: cosine identical = 1, opposite = –1; BERTScore, MoverScore.
- Embedding sizes (Table 3-2): BERT base 768 / large 1024; CLIP image 512 / text 512; text-embedding-3-small 1536 / large 3072; Cohere embed-english-v3.0 1024 / light 384.
- MTEB = Massive Text Embedding Benchmark (Muennighoff et al., 2023).
- Joint embedding models: CLIP (2021, text+images), ULIP (text, images, 3D point clouds), ImageBind (6 modalities, 2023).
- AI as a judge became practical ~2020 (GPT-3); LangChain 2023: 58% of evaluations via AI judges.
- MT-Bench: GPT-4–human agreement 85% vs human–human 81% (Zheng et al., 2023).
- AlpacaEval: 0.98 correlation with Chatbot Arena (Dubois et al., 2023).
- Consistency: examples raised GPT-4 consistency 65% → 77.5%; spending quadrupled.
- Costs: generate + evaluate with GPT-4 ≈ 2× calls; 3 criteria ≈ 4× calls.
- Biases: self-bias GPT-4 +10% / Claude-v1 +25% win rate; verbosity: ~100-word-with-errors beats ~50-word-correct; ~2× longer almost always wins (Saito et al., 2023); GPT-4 less prone than GPT-3.5.
- Cappy (reward model): 360M params, score 0–1 (Google 2023).
- BLEURT score range: approximately –2.5 to 1.0.
- Comparative evaluation in AI: first used by Anthropic (2021); LMSYS Chatbot Arena.
- Chatbot Arena: originally Elo → Bradley–Terry; scaled scores ×400 + 1000, Llama-13b rescaled to 800.
- Scalability: 57 models, 244,000 comparisons, 1,596 model pairs, ~153 comparisons/pair (Jan 2024).
- LMSYS prompts (2023, 33,000): 180 "hello"/"hi" = 0.55% (plus variations); "X has 3 sisters..." asked 44 times.
- Llama 2 paper (Touvron et al., 2023): humans still provide valuable feedback comparing two answers beyond best annotator ability.
- Rating algorithms: Elo, Bradley–Terry, TrueSkill.

### Algorithms & Formulæ
- **Cross entropy:** `H(P, Q) = H(P) + DKL(P‖Q)` — model's cross entropy on training data = data entropy + KL divergence of learned distribution Q from true distribution P. Not symmetric: H(P,Q) ≠ H(Q,P).
- **Bits-per-character:** `BPC = bits_per_token / characters_per_token` (e.g., 6/2 = 3).
- **Bits-per-byte:** `BPB = BPC / (bits_per_char / 8)` (e.g., 3 ÷ ⅞ = 3.43 for 7-bit ASCII chars).
- **Perplexity:** `PPL(P) = 2^(H(P))`; `PPL(P,Q) = 2^(H(P,Q))` (bits) or `e^(H(P,Q))` (nats).
- **Sequence perplexity:** `PPL(x1..xn) = P(x1..xn)^(−1/n) = (∏_{i=1..n} P(xi | x1..x(i−1)))^(−1/n)` — the inverse of the geometric mean of the per-token probabilities; requires the model's probabilities/logprobs.
- **Cosine similarity:** `cos(A, B) = (A·B) / (‖A‖ · ‖B‖)` where A·B = dot product and ‖A‖ = Euclidean (L2) norm; identical → 1, opposite → –1.
- **pass@k:** `pass@k = (# problems solved by ≥1 of k samples) / (# problems)`; solved = all test cases pass.
- **Win rate:** `win_rate(A over B) = (# matches A wins vs B) / (# matches A vs B)`.
- **Rating algorithms (Elo / Bradley–Terry / TrueSkill):** compute a per-model score from comparative signals; rank by score. Elo sensitive to order of evaluators/prompts (why Chatbot Arena switched to Bradley–Terry).
- **Evaluation economy:** costs grow with number of judge calls (2× for generate+evaluate; 4× for generation + 3 criteria); spot-checking (sampling) reduces cost but lowers confidence.

### Diagrams / Visuals
- **Figure 3-1:** LLM evaluation papers over time — exponential growth from 2 to ~35/month in the first half of 2023 (Chang et al., 2023).
- **Figure 3-2:** Number of open source evaluation repositories among the 1,000 most popular AI repos on GitHub — exponential growth curve (Chip's analysis, as of May 2024).
- **Figure 3-3:** Evaluation tool count is small compared to modeling/training and AI-orchestration tools (from the same 1,000-repo list) — evaluation infrastructure lags.
- **Figure 3-4:** Two languages describing positions within a square: (a) 2 tokens (upper/lower) → 1 bit; (b) 4 tokens (upper-left/upper-right/lower-left/lower-right) → 2 bits; more information per token but more bits needed.
- **Table 3-1:** GPT-2 model sizes (117M→1542M) vs metrics (LAMBADA PPL/ACC, CBT-CN/NE ACC, WikiText2/PTB PPL, enwiki8 BPB, text8 BPC, WikiText103 PPL, IBW PPL) — larger models consistently lower perplexity (OpenAI, 2018).
- **Figure 3-5:** Fuyu (Adept) generated a correct image caption but was given a low score because the correct answer was missing from reference captions.
- **Figure 3-6:** CLIP's architecture — (image, text) pairs → text encoder + image encoder → joint embedding space; training pulls matching image/text embeddings together (Radford et al., 2021).
- **Figure 3-7:** An AI judge (GPT-4) explaining its judgment — judges can both score and explain, useful for auditing.
- **Table 3-3:** Built-in AI-as-a-judge criteria (Sept 2024): Azure AI Studio, MLflow.metrics, LangChain Criteria Evaluation, Ragas.
- **Figure 3-8:** Example of an AI judge evaluating the quality of an answer given a question.
- **Table 3-4:** Different tools' default prompts/scoring for the same criterion "faithfulness": MLflow 1–5, Ragas 0/1, LlamaIndex YES/NO — scores not comparable.
- **Figure 3-9:** PandaLM example output — predicts which response is better AND explains its rationale (Wang et al., 2023).
- **Figure 3-10:** ChatGPT asking users to compare two outputs side-by-side (comparative evaluation in production).
- **Table 3-5:** History of pairwise model comparisons (matches) — Model A, Model B, Winner per match.
- **Table 3-6:** Win rates of five models across all 10 model pairs (1000 matches each) — ranking isn't obvious from pairwise win rates alone.

### Common Exam Traps
- **Evaluation chapters split:** this chapter covers evaluation *methods*; Chapter 4 covers *selecting models* and *building an evaluation pipeline*. Don't confuse them.
- **Exact vs subjective:** functional correctness and similarity are exact; AI as a judge is subjective (depends on judge model + prompt). Semantic similarity is *mostly* exact but embedding-choice makes it arguably subjective — given two embeddings, the score is computed exactly.
- **Evaluation must be designed around failure points** — metrics/tools can't make a system robust if you don't know where it fails (system-level thinking).
- **Perplexity vs cross entropy:** perplexity = 2^(cross entropy) (bits) or e^(·) (nats). TensorFlow/PyTorch report cross entropy in *nats*. Higher perplexity = more uncertainty = worse predictive accuracy.
- **Lower perplexity is better** — a common inversion trap. But: more structured data → lower PPL; bigger vocab → higher PPL; longer context → lower PPL.
- **Perplexity ≠ downstream quality after post-training:** SFT/RLHF typically *raise* perplexity ("post-training collapses entropy") — a post-trained model can get better at tasks while getting worse at next-token prediction; quantization changes perplexity unpredictably.
- **Perplexity double-use:** low perplexity on benchmark data = possible data contamination (the benchmark leaked into training → scores less trustworthy); high perplexity on new data = worth adding (deduplication); high perplexity on a text = abnormal text detection.
- **BPC vs BPB:** BPC = bits per *character*; BPB = bits per *byte* (avoids character-encoding differences; ASCII 7 bits vs UTF-8 8–32 bits). Don't swap them.
- **KL divergence is not symmetric; cross entropy is not symmetric** (H(P,Q) ≠ H(Q,P)).
- **Entropy example numbers:** 2 tokens → 1 bit; 4 tokens → 2 bits (log₂ of the number of options).
- **pass@k definition:** solved if *any* of k samples passes *all* test cases; score = fraction of problems solved. Expectation: pass@1 < pass@3 < pass@10.
- **Exact-match "contains" variation trap:** accepting any output containing the reference can accept factually wrong answers ("September 12, 1929" contains "1929").
- **Lexical ≠ semantic:** "What's up?" vs "How are you?" (semantically close, lexically different); "Let's eat, grandma" vs "Let's eat grandma" (lexically close, semantically different).
- **Lexical similarity drawbacks:** needs comprehensive references (Fuyu), references can be wrong (WMT 2023), and higher lexical similarity ≠ better responses (HumanEval BLEU).
- **Edit-distance ops:** deletion, insertion, substitution (transposition sometimes counts as one edit, sometimes as two).
- **Semantic similarity relies on embedding quality:** bad embeddings → same-meaning texts can score low.
- **Cosine similarity range:** identical → 1, opposite → –1.
- **Embedding size numbers:** BERT base 768, CLIP 512, text-embedding-3-small 1536/large 3072, Cohere 1024/384. Don't mix them up.
- **CLIP is text+image** (first major joint embedding model); ULIP adds 3D point clouds; ImageBind covers six modalities.
- **AI as a judge is a system** (model + prompt + sampling parameters), not just a model.
- **AI judge strengths:** 58% of LangChain evals; GPT-4–human agreement 85% > human–human 81%; AlpacaEval 0.98 correlation.
- **Consistency vs accuracy:** adding examples raised GPT-4 consistency 65%→77.5%, but high consistency ≠ high accuracy (consistent mistakes possible); examples quadrupled spending.
- **Criteria ambiguity:** "faithfulness" exists in MLflow (1–5), Ragas (0/1), LlamaIndex (YES/NO) with different prompts → scores not comparable. "Do not trust any AI judge if you can't see the model and the prompt used."
- **Bias numbers:** self-bias GPT-4 +10% / Claude-v1 +25% win rate; first-position bias (vs human recency bias); verbosity bias (longer with errors beats shorter correct; ~2× longer almost always wins; GPT-4 less biased than GPT-3.5).
- **Specialized judges:** reward model (Cappy, 360M params, 0–1) ≠ reference-based judge (BLEURT range ~–2.5 to 1.0; Prometheus 1–5 with reference = 5) ≠ preference model (PandaLM, JudgeLM; picks between two responses + rationale).
- **Self-evaluation is for sanity checks/improvement, not a replacement** for external judging.
- **Pointwise vs comparative:** pointwise = independent scores (Likert); comparative = pairwise matches/win rates. Comparative is easier for subjective quality but gives relative, not absolute, performance.
- **Comparative ≠ A/B testing:** A/B shows one candidate at a time; comparative shows multiple simultaneously.
- **Not all questions should be preference-based:** correctness questions (cell phone radiation) need correctness-based evaluation; preference voting only works for knowledgeable voters (AI-as-intern use cases).
- **Win rate:** probability A beats B; a 51% win rate doesn't convert cleanly to absolute performance (A resolves 70% of tickets; unclear how much B would resolve).
- **Transitivity assumption** (A>B, B>C → A>C) is questionable for AI (human preference not necessarily transitive; different evaluators/prompts).
- **Rating algorithm trivia:** Elo sensitive to evaluator/prompt order → Chatbot Arena switched to Bradley–Terry but kept scaling scores to look like Elo (×400 + 1000, Llama-13b = 800).
- **Scalability numbers:** 57 models → 1,596 pairs; 244,000 comparisons → ~153 per pair (Jan 2024). Model pairs grow *quadratically*.
- **Crowdsourcing cons:** no standard, no fact-checking, misuse (refused jokes downvoted), malicious votes, prompts don't reflect real use (180 "hello"/"hi" of 33,000 = 0.55%; "X has 3 sisters" asked 44 times), simple prompts pollute rankings.
- **Comparative never saturates** (as long as stronger models keep appearing) — unlike benchmarks that become useless at perfect scores; relatively hard to game (no reference data to cheat with).
- **Elo/Bradley–Terry/TrueSkill** come from sports/video games — a near-century of precedent.

### Chapter Summary
Chapter 3 explains **how to evaluate open-ended foundation models**, framed as the largest hurdle to shipping AI applications. The chapter opens with the motivation (real-world catastrophic failures; Brockman's "evals are surprisingly often all you need") and four reasons foundation models are harder to evaluate than traditional ML models: they're too intelligent to judge casually, too open-ended for ground-truth comparison, often black boxes whose public benchmarks saturate fast (GLUE→SuperGLUE, MMLU→MMLU-Pro), and they expand the scope of evaluation to discovering new capabilities. Despite a boom in evaluation papers/repos, investment still lags behind the rest of the AI engineering pipeline. The chapter then covers **language modeling metrics** — entropy (bits of information per token), cross entropy (H(P,Q) = H(P) + DKL(P‖Q)), BPC/BPB, and perplexity (2^cross entropy) — plus their interpretation rules (structure, vocabulary, context length), their proxy relationship to capability, their post-training caveat (perplexity rises after SFT/RLHF), and their use in contamination detection, deduplication, and abnormal-text detection. Next, **exact evaluation**: functional correctness (execution accuracy, pass@k on HumanEval/MBPP, text-to-SQL benchmarks) and similarity against reference data — exact match (works only for short exact answers; "contains" variant has false-accept traps), lexical similarity (token overlap, edit distance, n-grams; BLEU/ROUGE/METEOR++/TER/CIDEr; drawbacks include missing references, wrong references, and BLEU≠correctness), and semantic similarity (embeddings + cosine similarity; BERTScore/MoverScore; embedding intro — sizes, models, MTEB, joint/multimodal embeddings like CLIP/ULIP/ImageBind). The rising star is **AI as a judge** — fast, cheap, reference-free, explainable, and highly correlated with humans (85% vs 81%), but limited by inconsistency (mitigated with examples: 65%→77.5%), criteria ambiguity (faithfulness scoring differs across tools), cost/latency (~4× for three criteria), and biases (self-bias, first-position bias, verbosity bias). Judges may be stronger, weaker, or the same as the judged model; specialized judges (reward models like Cappy, reference-based like BLEURT/Prometheus, preference models like PandaLM/JudgeLM) are promising. Finally, **comparative evaluation** ranks models via pairwise matches and win rates using rating algorithms (Elo, Bradley–Terry, TrueSkill), but faces scalability bottlenecks (quadratic pairs; transitivity is questionable), standardization/quality-control issues (crowdsourcing noise), and the comparative-to-absolute gap (win rates don't reveal absolute quality or cost-benefit). The chapter's takeaway: build **systematic** evaluation, and always **supplement subjective AI judgments with exact evaluation and/or human evaluation**; preference models are a promising bridge because both comparative evaluation and post-training alignment need expensive preference signals. Chapter 4 builds the evaluation pipeline.

### Confidence Check
- **Sure:** the four challenges of evaluating foundation models (intelligence, open-endedness, black-box/benchmark saturation, expanded scope); benchmark saturation timeline (GLUE 2018→SuperGLUE 2019; NaturalInstructions 2021→Super-NaturalInstructions 2022; MMLU 2020→MMLU-Pro 2024); entropy/cross-entropy definitions and formulas (H(P,Q) = H(P) + DKL(P‖Q); asymmetry); bit/nat units and perplexity as exponential (2^H vs e^H); entropy example (2 tokens→1 bit; 4 tokens→2 bits); BPC/BPB definitions and worked examples (6/2=3; 3÷⅞≈3.43); perplexity interpretation rules (structure, vocab size, context length); perplexity uses (capability proxy, contamination, deduplication, abnormal-text detection) and the post-training caveat (perplexity rises; quantization); functional correctness concept and pass@k mechanics; code/text-to-SQL benchmarks (HumanEval, MBPP, Spider, BIRD-SQL, WikiSQL); exact-match strengths/limitations ("contains" trap); lexical similarity (token overlap 80%/60% example; edit-distance ops; n-grams; BLEU/ROUGE/METEOR++/TER/CIDEr; Fuyu/wrong-references/HumanEval-BLEU drawbacks); semantic similarity (embedding + cosine, range 1 to –1; BERTScore/MoverScore; embedding sizes Table 3-2; MTEB; CLIP/ULIP/ImageBind joint embeddings); AI-as-a-judge strengths and numbers (58% LangChain; 85% vs 81%; 0.98 AlpacaEval); prompting structure (task/criteria/scoring system) and the "classification>numerical, discrete>continuous" guidance; limitations (inconsistency 65%→77.5% with quadrupled cost; criteria ambiguity Table 3-4; cost/latency 2×/4×; self-bias 10%/25%, position bias, verbosity bias); judge selection (stronger/weaker/self; specialized judges: Cappy, BLEURT, Prometheus, PandaLM, JudgeLM); pointwise vs comparative; win rate; rating algorithms (Elo→Bradley–Terry, scaling ×400+1000, Llama-13b=800); comparative challenges (57 models/244K comparisons/~153 per pair; transitivity; new/private models; crowdsourcing pros/cons including 180 "hello"/"hi"; comparative-to-absolute gap; A-resolves-70% example); future benefits (comparative never saturates, hard to game, survives human performance limits).
- **Uncertain:** precise values in Figure 3-1/3-2 growth curves (graphical, not enumerated in text); full Table 3-1 dataset values beyond LAMBADA PPL (some OCR artifacts in table cells — verified LAMBADA PPL series 35.13/15.60/10.87/8.63); exact wording of the three naive judge prompts (reproduced as printed, may contain OCR artifacts); some footnote citation page numbers; exact LMSYS "hello"/"hi" percentages beyond 0.55% and 44-count brainteaser (as printed).

---

## §2. Code & Pseudocode Breakdown

This chapter is largely conceptual but includes math formulas, tables, example prompts, and a code-generation example. Below are the key "algorithmic" breakdowns.

### Language modeling metrics
```
# Entropy (bits): more information per token = more bits needed
2 tokens (upper/lower)          -> 1 bit  (log2(2) = 1)
4 tokens (upper-left, ...)      -> 2 bits (log2(4) = 2)

# Cross entropy decomposition
P = true distribution;  Q = model-learned distribution
H(P)     = training data's entropy
D_KL(P||Q) = KL divergence of Q from P
H(P, Q)  = H(P) + D_KL(P||Q)        # NOT symmetric: H(P,Q) != H(Q,P)
# perfect model => KL divergence = 0, cross entropy = data entropy

# Units
6 bits/token; 2 chars/token       -> BPC = 6/2 = 3
BPC 3; ASCII char = 7 bits = 7/8 byte
                                 -> BPB = 3 / (7/8) = 3.43
# BPB 3.43 means each original 8-bit byte is represented with 3.43 bits
# (i.e., the model compresses the text to less than half its size)

# Perplexity (PPL)
PPL(P)   = 2^(H(P))        # base 2 (bits)
PPL(P,Q) = 2^(H(P,Q))      # or e^(H(P,Q)) if using nats (TF/PyTorch)
# 2-bit cross entropy => PPL = 2^2 = 4 (must choose among 4 options)
```

### Sequence perplexity (requires logprobs)
```
PPL(x1..xn) = P(x1..xn)^(-1/n)
            = ( ∏_{i=1..n} P(xi | x1..x(i-1)) )^(-1/n)
# The inverse of the geometric mean of the per-token probabilities.
# Needs the model's probabilities/logprobs for each next token;
# not all commercial models expose logprobs (see Chapter 2).
```

### Exact match
```
def exact_match(generated, references):
    return any(generated == ref for ref in references)

# Looser variant: match if the reference is CONTAINED in the output
def contains_match(generated, references):
    return any(ref in generated for ref in references)
# Trap: "September 12, 1929" contains "1929", but the answer is wrong.
```

### Lexical similarity
```
# Token overlap
reference = "My cats scare the mice"                 # 5 words
A = "My cats eat the mice"        -> 4/5 = 80% overlap
B = "Cats and mice fight all the time" -> 3/5 = 60% overlap
# A is more similar to the reference.

# Edit distance (fuzzy matching): number of edits to convert one text to another
# operations: deletion  ("brad" -> "bad")
#             insertion ("bad"  -> "bard")
#             substitution ("bad" -> "bed")
#             transposition sometimes 1 edit, sometimes 2 (del + ins)
# "bad" is 1 edit from "bard", 3 edits from "cash"

# n-gram overlap
# "My cats scare the mice" = four bigrams: "my cats","cats scare",
#   "scare the","the mice"
# score = % of reference n-grams also in the generated response
# common metrics: BLEU, ROUGE, METEOR++, TER, CIDEr
```

### Semantic similarity (embedding-based)
```
A = embedding(generated);  B = embedding(reference)
cos(A,B) = (A . B) / (||A|| * ||B|)
# . = dot product; ||·|| = Euclidean (L2) norm
# identical embeddings -> 1; opposite embeddings -> -1
# metrics: BERTScore (BERT embeddings), MoverScore (mixture)
```

### pass@k (functional correctness for code)
```
for each problem in benchmark:               # e.g., HumanEval, MBPP
    for i in 1..k:
        sample_i = model.generate(problem)   # k code samples
    solved = any(passes_all_test_cases(sample_i) for i in 1..k)
pass@k = (# solved problems) / (# problems)  # e.g., 5/10 with k=3 => 50%
# expectation: pass@1 <= pass@3 <= pass@10
```

### AI as a judge — three naive prompt templates
```
# 1. Evaluate a response by itself
"Given the following question and answer, evaluate how good the answer is
 for the question. Use the score from 1 to 5.
 - 1 means very bad. - 5 means very good.
 Question: [QUESTION]  Answer: [ANSWER]  Score:"

# 2. Compare a generated response to a reference response
"Given the following question, reference answer, and generated answer,
 evaluate whether this generated answer is the same as the reference answer.
 Output True or False.
 Question: [QUESTION]  Reference answer: [REFERENCE]
 Generated answer: [GENERATED]"

# 3. Compare two generated responses (preference signal)
"Given the following question and two answers, evaluate which answer is
 better. Output A or B.
 Question: [QUESTION]  A: [FIRST]  B: [SECOND]  The better answer is:"
```

### Prompting an AI judge (structure)
```
1. Task:      e.g., "score the relevance between a generated answer and the
               question based on the ground truth answer"
2. Criteria:  detailed instruction, e.g., "Your primary focus should be on
               whether the generated answer contains sufficient information
               to address the given question according to the ground truth"
3. Scoring:   classification (good/bad) | discrete numeric (1-5) |
              continuous numeric (0-1)
# + include examples of each score with justifications (best practice)
# guidelines: classification > numerical; discrete > continuous;
#             wider discrete ranges -> worse
```

### Win rate and comparative ranking
```
win_rate(A over B) = (# matches where A beats B) / (# matches A vs B)
# with >2 models, a rating algorithm turns pairwise signals into a ranking:
# Elo, Bradley-Terry, TrueSkill
# Chatbot Arena: Elo -> Bradley-Terry (Elo sensitive to order of
#   evaluators/prompts); Bradley-Terry scores scaled ×400 + 1000,
#   rescaled so Llama-13b = 800
# transitivity assumption: A>B, B>C => A>C (questionable for AI models)
```

### Spot-checking (cost reduction for AI judges)
```
evaluate only a fraction p of generated responses (p = sample %)
# larger p -> more confidence, higher cost; smaller p -> cheaper, may miss
# failures. Balance is application-specific (trial and error; see Ch 4).
# cost scaling: generate+evaluate = 2x calls; 3 criteria => 4x calls
```
