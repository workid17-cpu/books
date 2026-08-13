# Chapter 3 Flashcards: Evaluation Methodology
**Source:** *AI Engineering* (Chip Huyen), Chapter 3

---

## Part 1Terms → Definitions

**Q:** What is evaluation in the AI engineering context?
**A:** The process of assessing a model's/system's outputs to mitigate risks and uncover opportunities; for some applications it can take up the majority of the development effort.

**Q:** What is exact evaluation?
**A:** Evaluation that produces judgment without ambiguity (e.g., multiple-choice: answer is A, you pick B → wrong). Approaches: functional correctness and similarity measurements against reference data.

**Q:** What is subjective evaluation?
**A:** Evaluation whose result depends on who/what grades the output and how it's prompted (e.g., essay grading, AI as a judge); scores change with the judge model and prompt.

**Q:** What is a "vibe check"?
**A:** Eyeballing evaluation results informally (word of mouth); a 2023 a16z study found 6 of 70 decision makers evaluated models this way — risky and slow for iteration.

**Q:** What is benchmark saturation?
**A:** A benchmark becomes saturated once a model achieves a perfect score; with foundation models this happens fast (GLUE 2018 saturated in a year → SuperGLUE 2019; MMLU 2020 → MMLU-Pro 2024).

**Q:** What is entropy?
**A:** A metric measuring how much information, on average, a token carries; higher entropy = more bits needed per token and harder to predict what comes next. A 2-token language has entropy 1; a 4-token language has entropy 2.

**Q:** What is cross entropy?
**A:** A metric measuring how difficult it is for a language model to predict what comes next in a dataset; for true distribution P and learned distribution Q: H(P,Q) = H(P) + DKL(P‖Q). Not symmetric.

**Q:** What is KL divergence?
**A:** The divergence of the model's learned distribution Q with respect to the true distribution P (DKL(P‖Q)); zero when the model learns the training distribution perfectly.

**Q:** What is a bit?
**A:** A unit of entropy/cross entropy using base 2; each bit represents 2 unique values. Perplexity with bits = 2^(cross entropy).

**Q:** What is a nat?
**A:** A unit of entropy/cross entropy using the natural log (base e), used by TensorFlow and PyTorch; perplexity with nats = e^(cross entropy).

**Q:** What is bits-per-character (BPC)?
**A:** The number of bits per character; makes token-level metrics comparable across models with different tokenizers (e.g., 6 bits/token ÷ 2 chars/token = BPC 3).

**Q:** What is bits-per-byte (BPB)?
**A:** The number of bits to represent one byte of the original training data; more standardized than BPC because it avoids character-encoding differences (ASCII 7 bits vs UTF-8 8–32 bits).

**Q:** What is perplexity (PPL)?
**A:** The exponential of entropy/cross entropy (2^H with bits, e^H with nats); measures a model's uncertainty in predicting the next token. A 2-bit cross entropy → perplexity 4.

**Q:** What is data contamination in the evaluation context?
**A:** When a benchmark's data appears in a model's training data, detectable via low perplexity on that data; it makes the model's benchmark score less trustworthy.

**Q:** What is functional correctness?
**A:** Evaluating a system based on whether it performs its intended functionality (e.g., generated website meets requirements?); the ultimate metric for any application, but not always easy to automate.

**Q:** What is execution accuracy?
**A:** Functional correctness for code: run the generated code (e.g., in a Python interpreter) and check whether it produces correct outputs.

**Q:** What is pass@k?
**A:** For each problem, generate k code samples; a model solves a problem if any of the k samples passes all test cases; the score is the fraction of solved problems (5/10 with k=3 → pass@3 = 50%).

**Q:** What is reference data?
**A:** Data in the format (input, reference responses); reference responses are also called ground truths or canonical responses. An input can have multiple reference responses.

**Q:** What are reference-based vs reference-free metrics?
**A:** Reference-based metrics require reference responses to score (e.g., BLEU); reference-free metrics don't (e.g., most AI-as-a-judge setups).

**Q:** What is exact match?
**A:** A similarity measure that checks whether the generated response matches a reference response exactly; works for short, exact responses but fails for open-ended tasks with many valid answers.

**Q:** What is lexical similarity?
**A:** A similarity measure based on how much two texts overlap (tokens, edit distance, or n-grams); metrics include BLEU, ROUGE, METEOR++, TER, CIDEr.

**Q:** What is edit distance (fuzzy matching)?
**A:** The number of edits to convert one text to another; operations include deletion, insertion, and substitution (some matchers also count transposition as one edit, others as two).

**Q:** What is an n-gram?
**A:** A sequence of n tokens; 1-gram = unigram (one token), 2-gram = bigram (two tokens). "My cats scare the mice" has four bigrams.

**Q:** What is semantic similarity?
**A:** How close two texts are in meaning, computed by embedding texts and measuring vector similarity (e.g., cosine similarity); also called embedding similarity; metrics include BERTScore and MoverScore.

**Q:** What is an embedding?
**A:** A numerical representation (vector) that captures the meaning of the original data; typical size 100–10,000 elements; produced by models like BERT, CLIP, and Sentence Transformers.

**Q:** What is cosine similarity?
**A:** cos(A,B) = (A·B)/(‖A‖‖B‖) for embeddings A and B; identical embeddings → 1, opposite embeddings → –1.

**Q:** What is MTEB?
**A:** Massive Text Embedding Benchmark (Muennighoff et al., 2023) — measures embedding quality across multiple tasks.

**Q:** What is a joint (multimodal) embedding space?
**A:** A space where embeddings of different modalities (text, images, etc.) can be compared and combined; enables text-based image search. Examples: CLIP, ULIP, ImageBind.

**Q:** What is AI as a judge (LLM as a judge)?
**A:** Using AI to evaluate AI responses; the evaluating model is the AI judge. Subjective — the score depends on the judge model and the prompt.

**Q:** What is MT-Bench?
**A:** Zheng et al.'s (2023) evaluation benchmark where GPT-4–human agreement reached 85%, higher than human–human agreement (81%).

**Q:** What is AlpacaEval?
**A:** An AI-judge benchmark (Dubois et al., 2023) whose judges have a near-perfect 0.98 correlation with LMSYS's human-evaluated Chatbot Arena leaderboard.

**Q:** What is self-bias?
**A:** AI judges favoring their own responses (Zheng et al., 2023: GPT-4 favors itself +10% win rate; Claude-v1 +25%).

**Q:** What is first-position bias?
**A:** AI judges favoring the first answer in a pairwise comparison or list of options; opposite of human recency bias (favoring the last-seen answer); mitigated by repeating tests with different orderings.

**Q:** What is verbosity bias?
**A:** AI judges favoring lengthier answers regardless of quality (Wu and Aji 2023: ~100-word responses with errors beat ~50-word correct ones; Saito et al. 2023: ~2× longer almost always preferred).

**Q:** What is self-evaluation / self-critique?
**A:** A model judging its own response (e.g., "What's 10+3?" → "30" → self-critique → "13"); great for sanity checks and nudging the model to revise; sometimes called self-ask.

**Q:** What is a reward model?
**A:** A specialized judge that scores a (prompt, response) pair; used in RLHF for years; e.g., Cappy (Google, 2023), 360M params, score 0–1.

**Q:** What is a reference-based judge?
**A:** A specialized judge that evaluates a generated response against reference responses, outputting a similarity or quality score; e.g., BLEURT (similarity, range ~–2.5 to 1.0) and Prometheus (quality 1–5, reference = 5).

**Q:** What is a preference model?
**A:** A specialized judge that takes (prompt, response 1, response 2) and predicts which response users prefer; e.g., PandaLM (Wang et al., 2023), JudgeLM (Zhu et al., 2023).

**Q:** What is pointwise evaluation?
**A:** Evaluating each model independently (e.g., Likert scale) and ranking by their scores.

**Q:** What is comparative evaluation?
**A:** Evaluating models against each other side-by-side and computing a ranking from the comparison results; each comparison is a match; the probability A beats B is A's win rate over B.

**Q:** What is a win rate?
**A:** The probability that model A is preferred over model B, computed from all matches between A and B (A's wins ÷ A-vs-B matches).

**Q:** What is a rating algorithm?
**A:** An algorithm that computes a per-model score from comparative signals and ranks models by those scores; e.g., Elo, Bradley–Terry, TrueSkill (borrowed from sports/video games).

**Q:** What is the transitivity assumption in ranking?
**A:** The assumption that A>B and B>C implies A>C, letting you infer rankings without direct comparisons; questionable for AI models because human preference isn't necessarily transitive.

**Q:** What is spot-checking?
**A:** Evaluating only a subset of responses (= sampling) to reduce cost; larger samples give more confidence but cost more.

**Q:** What is a reward model's input/output (in RLHF)?
**A:** Input: (prompt, response) pair; output: a score of how good the response is given the prompt.

**Q:** What is semantic textual similarity (STS)?
**A:** Semantic similarity applied to text; computed via embeddings rather than lexical overlap.

**Q:** What is the L2 (Euclidean) norm?
**A:** The magnitude of a vector used in cosine similarity's denominator: ‖A‖ = sqrt(sum of squared components).

**Q:** What is text-to-SQL?
**A:** Generating SQL queries from natural language; evaluated via functional correctness (running the SQL), e.g., benchmarks Spider, BIRD-SQL, WikiSQL.

**Q:** What is the "contains-match" variation of exact matching?
**A:** Accepting any output that contains the reference response (e.g., "The answer is 5" contains "5"); can falsely accept wrong answers ("September 12, 1929" contains "1929").

**Q:** What is benchmark GLUE?
**A:** General Language Understanding Evaluation, released 2018; saturated in a year, replaced by SuperGLUE (2019) — an example of fast benchmark saturation.

**Q:** What is MMLU?
**A:** A strong 2020 benchmark many early foundation models relied on; largely replaced by MMLU-Pro (2024) as models saturated it.

**Q:** What is product-embedded comparative evaluation?
**A:** Incorporating side-by-side comparisons into products (e.g., two code snippets in an editor; chat apps); users evaluate during workflows, though many clicks may be random noise.

**Q:** What is the "comparative-to-absolute gap"?
**A:** Comparative evaluation tells which model is better but not how good either is — "B beats A" is compatible with both-good, both-bad, or B-good/A-bad, so cost-benefit decisions need absolute evaluation too.

## Part 2Short Answer

**Q:** Why is evaluation described as the biggest hurdle to bringing AI applications to reality?
**A:** Because without quality control of AI outputs, the risk of AI may outweigh its benefits (real examples: chatbot-encouraged suicide, hallucinated legal evidence, Air Canada damages). For some applications, figuring out evaluation takes up the majority of development effort, and Greg Brockman tweeted "evals are surprisingly often all you need."

**Q:** State the four reasons foundation models are harder to evaluate than traditional ML models.
**A:** (1) More intelligent outputs are harder to judge (PhD-level math vs first-grade); (2) open-endedness defeats ground-truth comparison; (3) models are black boxes and public benchmarks saturate fast; (4) the scope of evaluation has expanded to discovering new capabilities.

**Q:** How does benchmark saturation work, and which benchmarks show it?
**A:** A benchmark saturates when a model achieves a perfect score; foundation models saturate fast. GLUE (2018) → SuperGLUE (2019); NaturalInstructions (2021) → Super-NaturalInstructions (2022); MMLU (2020) → MMLU-Pro (2024).

**Q:** What evidence shows evaluation investment lags behind the rest of AI engineering?
**A:** Evaluation papers grew exponentially (2 → ~35/month, early 2023) and >50 of the top 1,000 GitHub AI repos are evaluation-focused, yet DeepMind's Balduzzi et al. note "developing evaluations has received little systematic attention"; evaluation tools are scarce vs modeling/orchestration tools; Anthropic asked policymakers for more evaluation funding.

**Q:** Define entropy, cross entropy, and perplexity and their relationship.
**A:** Entropy (H) = average information per token. Cross entropy H(P,Q) = H(P) + DKL(P‖Q) = how hard the model finds the data. Perplexity = 2^cross entropy (bits) or e^cross entropy (nats). Higher values = more uncertainty.

**Q:** Why is cross entropy not symmetric?
**A:** H(P,Q) ≠ H(Q,P) because KL divergence is not symmetric — the divergence of Q from P differs from the divergence of P from Q. Order matters.

**Q:** Walk through the BPC and BPB example in the chapter.
**A:** 6 bits per token with 2 chars per token → BPC = 3. With ASCII (7 bits/char = 7/8 byte): BPB = 3 ÷ (7/8) = 3.43, meaning each original 8-bit byte is represented with 3.43 bits → the text compresses to less than half its size.

**Q:** What are the three general rules for interpreting perplexity?
**A:** (1) More structured data → lower expected perplexity (HTML < everyday text); (2) bigger vocabulary → higher perplexity (word-based > character-based); (3) longer context → lower perplexity (Shannon used ≤10 tokens; today 500–10,000).

**Q:** Why might perplexity be misleading after post-training or quantization?
**A:** Post-training (SFT/RLHF) makes models better at tasks but can worsen next-token prediction — perplexity typically increases ("post-training collapses entropy"). Quantization can change perplexity in unexpected ways too.

**Q:** List three uses of perplexity beyond guiding LM training.
**A:** (1) Detecting data contamination (low PPL on benchmark data → likely in training data); (2) deduplicating training data (add new data only if its PPL is high); (3) detecting abnormal/unpredictable texts (high PPL).

**Q:** How does pass@k work, and why does a bigger k raise the score in expectation?
**A:** Generate k code samples per problem; solved if any sample passes all test cases; score = fraction solved. More samples raise the chance at least one passes → in expectation pass@1 < pass@3 < pass@10.

**Q:** Which code and text-to-SQL benchmarks use functional correctness?
**A:** Code: HumanEval (OpenAI) and MBPP (Google). Text-to-SQL: Spider, BIRD-SQL, WikiSQL. Evaluation = running the code/SQL and checking outputs (e.g., gcd(15,20) must return 5).

**Q:** What are the four ways to measure similarity between two open-ended texts?
**A:** (1) Evaluator judgment whether texts are the same; (2) exact match (binary); (3) lexical similarity (token/edit/n-gram overlap, sliding scale); (4) semantic similarity (embeddings + cosine, sliding scale).

**Q:** Why does exact match fail for tasks like translation?
**A:** An input can have many valid outputs ("Comment ça va?" → "How are you?", "How is everything?", "How are you doing?"); "How is it going?" would be marked wrong, and exhaustive reference sets are impossible for complex texts.

**Q:** Give an example showing the "contains-match" variation can accept wrong answers.
**A:** "What year was Anne Frank born?" Reference = 1929. Output "September 12, 1929" contains "1929" → accepted, but the date is factually wrong.

**Q:** Give the chapter's lexical-similarity example with the 80%/60% numbers.
**A:** Reference: "My cats scare the mice" (5 words). Response A "My cats eat the mice" → 4/5 = 80%. Response B "Cats and mice fight all the time" → 3/5 = 60%. A is more similar.

**Q:** What are the three drawbacks of lexical similarity?
**A:** (1) Needs a comprehensive reference set — good responses score low when references are missing (Adept's Fuyu); (2) references can be wrong (WMT 2023 found bad reference translations); (3) higher lexical similarity ≠ better responses (HumanEval: BLEU similar for correct and incorrect code).

**Q:** Why can semantic similarity be considered both exact and subjective?
**A:** Given two embeddings, the similarity score is computed exactly (cosine), but different embedding algorithms produce different embeddings — so the result can be considered subjective to the embedding choice.

**Q:** What makes an embedding algorithm good, and how is embedding quality measured?
**A:** Good = more-similar texts have closer embeddings ("cat sits on a mat" closer to "dog plays on the grass" than "AI research is super fun"). Measured by utility on tasks (classification, topic modeling, recommenders, RAG) — e.g., via MTEB.

**Q:** Describe CLIP and why joint embeddings are useful.
**A:** CLIP (Radford et al., 2021) trains text and image encoders to project both into a joint embedding space, pulling matching (image, text) pairs together. Joint spaces enable comparing/combining modalities, e.g., text-based image search. ULIP adds 3D point clouds; ImageBind covers six modalities.

**Q:** Why are AI judges attractive, and what evidence supports their quality?
**A:** Fast, cheap, reference-free, judge on any criterion, explainable, and representative of the masses. Evidence: 58% of LangChain evals used AI judges (2023); GPT-4–human agreement 85% > human–human 81% (MT-Bench); AlpacaEval 0.98 correlation with Chatbot Arena.

**Q:** What should an AI-judge prompt clearly explain?
**A:** (1) The task to perform; (2) the evaluation criteria (the more detailed, the better); (3) the scoring system — classification (good/bad), discrete numerical (1–5), or continuous numerical (0–1). Include examples of each score. Classification > numerical; discrete > continuous; wide ranges hurt.

**Q:** List the limitations of AI as a judge.
**A:** Inconsistency (probabilistic; examples raised GPT-4 consistency 65%→77.5% but quadrupled cost); criteria ambiguity (same "faithfulness" = MLflow 1–5, Ragas 0/1, LlamaIndex YES/NO); costs/latency (~2× calls for generate+evaluate, ~4× for 3 criteria); biases (self-bias, first-position bias, verbosity bias); privacy/IP of proprietary judges.

**Q:** Why is an AI judge a "system" rather than just a model?
**A:** Because a judge is defined by its model + prompt + sampling parameters; changing any of them results in a different judge, and you shouldn't trust a judge if you can't see the model and prompt used.

**Q:** Contrast stronger, weaker, and self judges.
**A:** Stronger judges make better judgments and can coach weaker models, but the strongest model has no eligible judge. Weaker judges work because judging is easier than generating. Self-judging (self-evaluation) is for sanity checks and self-revision, not final verdicts (self-bias).

**Q:** Describe the three types of specialized AI judges with examples.
**A:** Reward model — scores (prompt, response), e.g., Cappy (360M params, 0–1). Reference-based judge — scores against references, e.g., BLEURT (similarity, ~–2.5 to 1.0), Prometheus (1–5, reference = 5). Preference model — predicts which of two responses users prefer, e.g., PandaLM, JudgeLM.

**Q:** Contrast pointwise and comparative evaluation, with a dancing-contest analogy.
**A:** Pointwise: score each dancer independently, rank by scores. Comparative: dancers perform side-by-side, judges pick the best; ranking derived from pairwise decisions. Comparative is easier for subjective quality but yields relative, not absolute, performance.

**Q:** Why isn't comparative evaluation the same as A/B testing, and why shouldn't all questions be decided by preference?
**A:** A/B shows one candidate per user at a time; comparative shows multiple simultaneously. Preference voting only works when voters are knowledgeable (AI-as-intern tasks); factual questions (e.g., cell-phone-radiation link) need correctness-based evaluation, and forcing users to choose frustrates them.

**Q:** List the three challenges of comparative evaluation.
**A:** (1) Scalability — model pairs grow quadratically (57 models → 1,596 pairs, only ~153 comparisons each); transitivity is questionable; new/private models complicate it. (2) Standardization/quality control — crowdsourcing produces noisy prompts (180 "hello"/"hi" of 33,000), unchecked votes, and no standard. (3) Comparative-to-absolute gap — win rates don't reveal absolute quality or cost-benefit (A resolves 70% of tickets; B beats A 51% — unclear how that converts).

**Q:** Why might comparative evaluation remain valuable as models surpass human performance?
**A:** Humans may be unable to give concrete scores but can still detect differences between two outputs (Llama 2 paper: humans still provide valuable feedback when comparing two answers beyond best-annotator ability). Comparative evaluation also never saturates while stronger models keep appearing and is relatively hard to game.

## Part 3Fill-in-the-Blank

**Q:** In December 2023, OpenAI cofounder Greg Brockman tweeted that "______ are surprisingly often all you need."
**A:** evals.

**Q:** A 2023 a16z study found that ______ out of 70 decision makers evaluated models by word of mouth.
**A:** 6.

**Q:** The benchmark GLUE came out in ______ and became saturated in just a year, leading to SuperGLUE in ______.
**A:** 2018; 2019.

**Q:** MMLU (2020) was largely replaced by ______ (2024).
**A:** MMLU-Pro.

**Q:** The number of published LLM-evaluation papers grew from 2 per month to almost ______ per month in the first half of 2023.
**A:** 35.

**Q:** Claude Shannon introduced entropy in his ______ paper "Prediction and Entropy of Printed English."
**A:** 1951.

**Q:** A language with only two position tokens has entropy of ______; a language with four tokens has entropy of ______.
**A:** 1; 2.

**Q:** Cross entropy is computed as H(P,Q) = H(P) + ______.
**A:** DKL(P‖Q) (KL divergence).

**Q:** If cross entropy is 6 bits and each token has 2 characters on average, the BPC is ______.
**A:** 3.

**Q:** With BPC = 3 and ASCII characters of 7 bits each, the BPB is ______.
**A:** 3.43 (3 ÷ 7/8).

**Q:** Perplexity is the ______ of entropy and cross entropy.
**A:** exponential.

**Q:** A language model with 2-bit cross entropy has perplexity ______ (choose among 2² = 4 options).
**A:** 4.

**Q:** TensorFlow and PyTorch use ______ (natural log) as the unit for entropy and cross entropy.
**A:** nat.

**Q:** Claude Shannon evaluated his model's cross entropy conditioned on up to ______ previous tokens; today models use between 500 and ______.
**A:** 10; 10,000.

**Q:** A perplexity of 3 means the model has a ______ in 3 chance of predicting the next token correctly (if all tokens are equally likely).
**A:** 1.

**Q:** Post-training (SFT/RLHF) typically ______ a model's perplexity — "post-training collapses entropy."
**A:** increases.

**Q:** If a model's perplexity on a benchmark's data is low, the benchmark was likely ______ in the model's training data (data contamination).
**A:** included.

**Q:** pass@k = the fraction of ______ out of all problems, where a problem is solved if any of k code samples passes all its test cases.
**A:** solved problems.

**Q:** For HumanEval, a problem is accompanied by a set of test cases implemented as ______ statements.
**A:** assert.

**Q:** In the pass@k example, 5 solved problems out of 10 with k = 3 gives pass@3 = ______.
**A:** 50%.

**Q:** Common lexical-similarity metrics include BLEU, ROUGE, METEOR++, TER, and ______.
**A:** CIDEr.

**Q:** In the cosine-similarity formula cos(A,B) = (A·B)/(‖A‖‖B‖), ‖A‖ is the ______ norm (L2 norm).
**A:** Euclidean.

**Q:** Embedding vector sizes are typically between ______ and ______ elements.
**A:** 100; 10,000.

**Q:** OpenAI's text-embedding-3-small has embedding size ______; text-embedding-3-large has ______.
**A:** 1536; 3072.

**Q:** BERT base embedding size is ______; CLIP image and text embeddings are both ______.
**A:** 768; 512.

**Q:** LangChain's State of AI report (2023) found that ______% of evaluations on their platform were done by AI judges.
**A:** 58.

**Q:** On MT-Bench, GPT-4–human agreement reached ______%, higher than human–human agreement of ______%.
**A:** 85; 81.

**Q:** AlpacaEval's AI judges have a ______ correlation with LMSYS's Chatbot Arena leaderboard.
**A:** 0.98.

**Q:** Including evaluation examples in the prompt raised GPT-4's consistency from 65% to ______%.
**A:** 77.5.

**Q:** Zheng et al. (2023) found GPT-4 favors itself with a ______% higher win rate; Claude-v1 favors itself with ______%.
**A:** 10; 25.

**Q:** In Zheng et al.'s experiment, including more examples caused GPT-4 spending to ______.
**A:** quadruple (increase 4×).

**Q:** Google's Cappy reward model has ______ million parameters and produces a score between 0 and 1.
**A:** 360.

**Q:** The self-evaluation example: "What's 10+3?" → "30" → self-critique → "The correct answer is ______."
**A:** 13.

**Q:** In AI, comparative evaluation was first used in 2021 by ______ to rank different models.
**A:** Anthropic.

**Q:** In January 2024, LMSYS evaluated 57 models using ______ comparisons across ______ model pairs.
**A:** 244,000; 1,596.

**Q:** LMSYS's Chatbot Arena switched from ______ to the Bradley–Terry algorithm because Elo was sensitive to the order of evaluators and prompts.
**A:** Elo.

**Q:** Bradley–Terry scores were scaled by ×400 and +1000, then rescaled so that the model ______ has a score of 800.
**A:** Llama-13b.

**Q:** Among 33,000 LMSYS prompts published in 2023, 180 "hello"/"hi" prompts account for ______% of the data.
**A:** 0.55.

**Q:** The brainteaser "X has 3 sisters, each has a brother. How many brothers does X have?" was asked ______ times.
**A:** 44.

**Q:** In the customer-support example, model A resolves ______% of tickets; model B wins against A 51% of the time, but it's unclear how that win rate converts.
**A:** 70.
