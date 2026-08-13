# Practice Exam — Chapter 3: Evaluation Methodology
**Source:** *AI Engineering* (Chip Huyen), Chapter 3
**Instructions:** Allow ~45–55 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which of the following is cited as a real catastrophic failure motivating evaluation?
   a) A chatbot encouraged a man who later died by suicide
   b) A model refused to answer customer support tickets
   c) An image generator produced low-resolution photos
   d) A translation model mixed up two languages

2. According to the chapter, what did Greg Brockman (OpenAI cofounder) tweet in December 2023?
   a) "Evaluation is the last step of AI engineering"
   b) "Evals are surprisingly often all you need"
   c) "Benchmarks are more important than models"
   d) "Human evaluation always beats automation"

3. Why is evaluating foundation models harder than evaluating traditional ML models?
   a) Foundation models have fewer parameters to inspect
   b) They are less intelligent than classification models
   c) They cannot be run on standard hardware
   d) Their open-endedness defeats comparison against a curated list of ground truths

4. The benchmark GLUE (2018) became saturated and was replaced by:
   a) MMLU
   b) NaturalInstructions
   c) SuperGLUE
   d) Chatbot Arena

5. Which of the following replaced MMLU (2020) for many early foundation models?
   a) MMLU-Pro
   b) GLUE
   c) SuperGLUE
   d) MTEB

6. What is entropy, in the context of language modeling?
   a) The number of tokens in a model's vocabulary
   b) The probability of generating the most likely token
   c) The total compute required to train a model
   d) How much information, on average, a token carries

7. A language with four equally likely position tokens has an entropy of:
   a) 1 bit
   b) 4 bits
   c) 2 bits
   d) 8 bits

8. Cross entropy H(P, Q) is defined as:
   a) H(P) × D_KL(Q‖P)
   b) H(Q) − D_KL(P‖Q)
   c) H(Q) ÷ D_KL(P‖Q)
   d) H(P) + D_KL(P‖Q)

9. Which of the following is true about cross entropy?
   a) It is symmetric: H(P,Q) = H(Q,P)
   b) It is not symmetric
   c) It is always zero for a trained model
   d) It equals the training data's entropy minus the KL divergence

10. If a model has cross entropy of 6 bits and each token averages 2 characters, its BPC is:
    a) 3
    b) 12
    c) 6
    d) 0.33

11. With BPC = 3 and ASCII characters (7 bits each), the BPB is approximately:
    a) 1.5
    b) 2.33
    c) 7
    d) 3.43

12. Perplexity is defined as:
    a) The square root of cross entropy
    b) The logarithm of cross entropy
    c) The negative of cross entropy
    d) The exponential of entropy and cross entropy

13. A language model with 2-bit cross entropy has a perplexity of:
    a) 2
    b) 8
    c) 16
    d) 4

14. Which of the following tends to give a HIGHER expected perplexity?
    a) More structured data like HTML
    b) Data the model has memorized
    c) A longer context
    d) A larger vocabulary

15. According to the chapter, what typically happens to a model's perplexity after post-training (SFT/RLHF)?
    a) It stays the same
    b) It decreases
    c) It increases
    d) It becomes undefined

16. A low perplexity on a benchmark's data likely indicates:
    a) The benchmark is very easy
    b) Data contamination — the benchmark data was in the model's training data
    c) The model is badly calibrated
    d) The benchmark is too small

17. What is functional correctness?
    a) Whether a system performs its intended functionality
    b) Whether outputs match references lexically
    c) Whether the model can classify inputs correctly
    d) Whether perplexity is below a threshold

18. In pass@k evaluation, a model solves a problem if:
    a) Its most likely output passes all test cases
    b) The average of k samples passes the tests
    c) All k samples pass at least one test case
    d) Any of the k generated samples passes all test cases

19. If 5 of 10 problems are solved with k = 3, the pass@3 score is:
    a) 30%
    b) 15%
    c) 50%
    d) 100%

20. Which of the following is a text-to-SQL benchmark relying on functional correctness?
    a) HumanEval
    b) MBPP
    c) Spider
    d) MTEB

21. Reference responses are also called:
    a) Ground truths or canonical responses
    b) Logprobs or logits
    c) Embeddings or vectors
    d) Rubrics or guidelines

22. Which exact-match variation can wrongly accept a factually incorrect answer?
    a) Requiring exact string equality
    b) Ignoring whitespace and capitalization
    c) Comparing only the first token
    d) Accepting any output that contains the reference response

23. In the lexical-similarity example, response A ("My cats eat the mice") scores how high relative to the reference "My cats scare the mice"?
    a) 60%
    b) 80%
    c) 40%
    d) 100%

24. The three edit-distance operations are:
    a) Deletion, insertion, substitution
    b) Addition, subtraction, multiplication
    c) Tokenize, embed, normalize
    d) Split, merge, transpose

25. Which metrics are common for lexical similarity?
    a) BERTScore and MoverScore
    b) BLEU, ROUGE, METEOR++, TER, CIDEr
    c) pass@k and exact match
    d) MTEB and MMLU

26. Which of the following is a drawback of lexical similarity mentioned in the chapter?
    a) It requires expensive GPU compute
    b) Higher lexical similarity scores don't always mean better responses (e.g., on HumanEval)
    c) It can't handle English text
    d) It always requires human judges

27. Why can "What's up?" and "How are you?" be scored as similar?
    a) They share many characters
    b) They have identical token overlap
    c) They are semantically close despite lexical differences
    d) They have the same edit distance

28. Cosine similarity between identical embeddings equals:
    a) 1
    b) 0
    c) –1
    d) Infinity

29. Embedding vector sizes are typically between:
    a) 2 and 16
    b) 10 and 100
    c) 1 million and 10 million
    d) 100 and 10,000

30. Which model maps text and images into a joint embedding space?
    a) BERT
    b) CLIP
    c) GPT-4
    d) MTEB

31. What is an AI judge?
    a) An AI model used to evaluate other AI models
    b) A human who audits AI outputs
    c) A benchmark that ranks AI models
    d) A reward model that trains AI

32. On MT-Bench, the agreement between GPT-4 and humans reached:
    a) 65%
    b) 81%
    c) 85%
    d) 98%

33. Which of the following is a limitation of AI as a judge?
    a) It requires reference data
    b) Criteria ambiguity — e.g., "faithfulness" scoring differs across tools
    c) It can never explain its decisions
    d) It is always slower than human evaluators

34. Which bias causes AI judges to favor the first answer in a pairwise comparison?
    a) Recency bias
    b) Verbosity bias
    c) First-position bias
    d) Self-bias

35. Zheng et al. (2023) found that GPT-4 favors itself with what higher win rate?
    a) 10%
    b) 25%
    c) 50%
    d) 81%

36. What is the input/output of a preference model?
    a) (prompt, response) → a 0–1 score
    b) (prompt, response 1, response 2) → which response users prefer
    c) (candidate, reference) → a similarity score
    d) (prompt) → a quality score between 1 and 5

37. Which company first used comparative evaluation in AI in 2021?
    a) OpenAI
    b) Google DeepMind
    c) Anthropic
    d) Meta

38. Why did LMSYS's Chatbot Arena switch from Elo to the Bradley–Terry algorithm?
    a) Elo was too slow to compute
    b) Elo was sensitive to the order of evaluators and prompts
    c) Bradley–Terry scores are easier to interpret
    d) Elo couldn't handle ties

39. In January 2024, LMSYS evaluated 57 models using 244,000 comparisons — how many model pairs does that correspond to?
    a) 57
    b) 244,000
    c) 1,596
    d) 153

40. Which of the following is a benefit of comparative evaluation mentioned in the chapter?
    a) It never gets saturated as long as newer, stronger models are introduced
    b) It provides absolute performance scores
    c) It can be gamed by training on reference data
    d) It requires no human or AI evaluators

## Section B: True/False (1 point each)

41. **T / F** — Evaluation should be considered in the context of a whole system, not in isolation.

42. **T / F** — Public evaluation benchmarks like GLUE and MMLU never become saturated because they keep adding questions.

43. **T / F** — A higher perplexity means the model is more certain about the next token.

44. **T / F** — Cross entropy is symmetric: H(P, Q) always equals H(Q, P).

45. **T / F** — Bits-per-byte (BPB) is more standardized than BPC because it avoids character-encoding differences (ASCII vs UTF-8).

46. **T / F** — Perplexity can be used to detect data contamination: low perplexity on a benchmark's data suggests the data was in training.

47. **T / F** — In pass@k, generating more code samples per problem raises the expected score.

48. **T / F** — Lexical similarity and semantic similarity always agree on which response is better.

49. **T / F** — Semantic similarity is computed by comparing embeddings, for example with cosine similarity.

50. **T / F** — An AI judge is just a model; the prompt does not affect its judgments.

51. **T / F** — Including evaluation examples in a judge's prompt increased GPT-4's consistency from 65% to 77.5% in Zheng et al. (2023).

52. **T / F** — Verbosity bias means AI judges favor lengthier answers regardless of quality.

53. **T / F** — Comparative evaluation is the same as A/B testing.

54. **T / F** — With comparative evaluation, adding a new model can change the ranking of existing models.

55. **T / F** — Comparative evaluation tells you how good a model is in absolute terms, so cost–benefit decisions need no other evaluation.

## Section C: Short Answer (model answers)

56. List the four reasons foundation models are harder to evaluate than traditional ML models.

57. Define entropy, cross entropy, and perplexity, and state the relationship among them (with formulas).

58. Give the three general rules for interpreting perplexity values.

59. What is pass@k, and why does a larger k raise the score in expectation?

60. What are the three drawbacks of lexical similarity highlighted in the chapter?

61. Explain the difference between exact, lexical, and semantic similarity, with one example each.

62. Why are AI judges attractive, and what evidence supports their quality?

63. List the biases of AI judges discussed in the chapter and how the first-position bias can be mitigated.

64. Contrast pointwise and comparative evaluation, and give one reason comparative is easier for subjective quality.

65. List the three challenges of comparative evaluation and one mitigation/response for each.

## Section D: Essay (grading notes)

66. Discuss why foundation models are harder to evaluate than traditional ML models, including benchmark saturation, black-box models, and the expanded scope of evaluation, plus evidence that evaluation investment lags behind the rest of AI engineering.

67. Explain the language modeling metrics: entropy, cross entropy (H(P,Q) = H(P) + D_KL(P‖Q)), BPC/BPB, and perplexity — with the worked examples from the chapter (2/4-token languages, 6-bit/2-char BPC, 3 ÷ 7/8 BPB, 2-bit → perplexity 4) — and the three interpretation rules plus the post-training/quantization caveats.

68. Describe the two exact evaluation approaches: functional correctness (with pass@k and examples of code/text-to-SQL benchmarks) and similarity measurements against reference data (exact match and its "contains" trap, lexical similarity and its drawbacks, semantic similarity and embeddings), including what makes a good embedding and joint/multimodal embeddings.

69. Explain AI as a judge: why it's attractive (speed, cost, reference-free, explainability, human correlation), the three usage modes, how to prompt a judge (task/criteria/scoring system), and its limitations (inconsistency, criteria ambiguity, cost/latency, biases), plus judge selection (stronger/weaker/self) and the three specialized judges.

70. Explain comparative evaluation: pointwise vs comparative, win rates, rating algorithms (Elo, Bradley–Terry, TrueSkill; why Chatbot Arena switched), the three challenges (scalability/transitivity, standardization and quality control, comparative-to-absolute gap), and why comparative evaluation has a strong future despite its limitations.

---

## Answer Key

### Section A: Multiple Choice
1. a
2. b
3. d
4. c
5. a
6. d
7. c
8. d
9. b
10. a
11. d
12. d
13. d
14. d
15. c
16. b
17. a
18. d
19. c
20. c
21. a
22. d
23. b
24. a
25. b
26. b
27. c
28. a
29. d
30. b
31. a
32. c
33. b
34. c
35. a
36. b
37. c
38. b
39. c
40. a

### Section B: True/False
41. **T** — Evaluation must be designed around the system's failure points; it isn't a standalone step.
42. **F** — Benchmarks saturate fast (GLUE 2018 → SuperGLUE 2019; MMLU → MMLU-Pro); that's why new ones keep being introduced.
43. **F** — Higher perplexity = MORE uncertainty about the next token, not more certainty.
44. **F** — Cross entropy is not symmetric (KL divergence isn't symmetric).
45. **T** — BPB avoids character-encoding differences (ASCII 7 bits vs UTF-8 8–32 bits).
46. **T** — The model finds memorized data easiest to predict (lowest perplexity).
47. **T** — More samples raise the chance at least one passes → pass@1 < pass@3 < pass@10 in expectation.
48. **F** — They measure different things: lexical overlap vs meaning ("What's up?" vs "How are you?").
49. **T** — Semantic similarity = embedding similarity (e.g., cosine similarity).
50. **F** — An AI judge is a system of model + prompt + sampling parameters; changing the prompt changes the judge.
51. **T** — Consistency rose from 65% to 77.5% (though spending quadrupled and consistency ≠ accuracy).
52. **T** — Longer-with-errors can beat shorter-correct (Wu and Aji 2023; Saito et al. 2023).
53. **F** — A/B shows one candidate at a time; comparative shows multiple outputs simultaneously.
54. **T** — The new model is evaluated against existing ones, which can reshuffle rankings.
55. **F** — Comparative gives relative rankings only; absolute quality/cost-benefit needs other evaluation (e.g., "B beats A" could mean both are bad).

### Section C: Short Answer (model answers)
56. **(1)** More intelligent outputs are harder to judge (PhD-level math vs first-grade; need fact-checking, reasoning, domain expertise). **(2)** Open-endedness defeats ground-truth comparison — impossible to curate a comprehensive list of correct outputs. **(3)** Models are black boxes (providers hide architecture/training data) and public benchmarks saturate fast (GLUE→SuperGLUE, MMLU→MMLU-Pro). **(4)** Scope of evaluation has expanded: general-purpose models must also be evaluated for newly discoverable tasks, including ones beyond human capabilities.

57. **Entropy** H(P): average information per token (2 tokens → 1 bit; 4 tokens → 2 bits). **Cross entropy** H(P,Q) = H(P) + D_KL(P‖Q): how hard the model finds predicting the dataset; not symmetric. **Perplexity** = exponential of cross entropy: PPL = 2^H(P,Q) (bits) or e^H(P,Q) (nats). Higher values = more uncertainty.

58. **(1)** More structured data → lower expected perplexity (HTML < everyday text). **(2)** Bigger vocabulary → higher perplexity (word-based > character-based; children's book < War and Peace). **(3)** Longer context → lower perplexity (Shannon: ≤10 tokens; today 500–10,000, bounded by max context).

59. Generate k code samples per problem; a problem is solved if ANY of the k samples passes ALL its test cases; pass@k = fraction of solved problems (5/10 with k=3 → 50%). Larger k → more chances at least one sample passes → higher expected score (pass@1 < pass@3 < pass@10).

60. **(1)** Requires a comprehensive reference set — a good response scores low if references lack similar responses (Adept's Fuyu). **(2)** References can be wrong (WMT 2023 found bad reference translations). **(3)** Higher lexical similarity ≠ better responses (HumanEval: BLEU scores similar for correct and incorrect code).

61. **Exact match:** binary, generated response equals a reference exactly (e.g., "5" for "What's 2+3?"); a "contains" variant can accept wrong answers ("September 12, 1929" contains "1929"). **Lexical similarity:** token/word overlap (e.g., "My cats eat the mice" = 80% vs "My cats scare the mice"; "Cats and mice fight all the time" = 60%). **Semantic similarity:** meaning via embeddings + cosine (e.g., "What's up?" ≈ "How are you?" despite zero lexical overlap).

62. Fast, easy, relatively cheap, work without reference data, judge on any criterion, explain decisions, and represent "the masses." Evidence: 58% of LangChain evals used AI judges (2023); GPT-4–human agreement 85% > human–human 81% (MT-Bench, Zheng et al. 2023); AlpacaEval judges have a 0.98 correlation with LMSYS Chatbot Arena.

63. **Self-bias** (model favors its own outputs; GPT-4 +10%, Claude-v1 +25% win rate). **First-position bias** (favors the first answer — opposite of human recency bias); mitigated by repeating tests with different orderings or crafting prompts carefully. **Verbosity bias** (favors longer answers regardless of quality). Plus general AI limitations (privacy/IP of proprietary judges).

64. **Pointwise:** evaluate each model independently (e.g., Likert scale) and rank by scores. **Comparative:** compare models side-by-side and derive ranking from matches/win rates. Comparative is easier for subjective quality because it's easier to say which of two songs is better than to give each a concrete score.

65. **(1) Scalability:** model pairs grow quadratically (57 models → 1,596 pairs, ~153 comparisons each); transitivity assumption is questionable; new/private models complicate it. Mitigation: smarter matching algorithms that sample matches reducing the most uncertainty. **(2) Standardization/quality control:** crowdsourced votes are noisy (180 "hello"/"hi" of 33,000 prompts; unchecked, sometimes malicious voters). Mitigations: hard-prompt filtering (LMSYS), trained/trusted evaluators (Scale), product-embedded evaluation, or AI evaluators. **(3) Comparative-to-absolute gap:** win rates don't reveal absolute quality or cost-benefit. Mitigation: combine with absolute/pointwise or task-specific evaluation.

### Section D: Essay (grading notes)
66. **Expect** the four reasons (intelligence gap — Terence Tao on GPT-o1; open-endedness vs ground truths; black boxes + saturation — GLUE 2018→SuperGLUE 2019, NaturalInstructions→Super-NaturalInstructions, MMLU→MMLU-Pro; expanded scope — discovering new capabilities). Evidence of lagging investment: evaluation papers 2→~35/month (early 2023), >50 evaluation repos in top-1,000 GitHub repos, DeepMind's Balduzzi et al. ("little systematic attention"), Anthropic's call for government funding, scarce evaluation tools vs modeling/orchestration tools.

67. **Expect** entropy (2 tokens → 1 bit; 4 → 2 bits; Shannon 1951); cross entropy H(P,Q) = H(P) + D_KL(P‖Q), not symmetric; units bit/nat (TF/PyTorch use nats); BPC (6 bits/2 chars = 3) and BPB (3 ÷ 7/8 = 3.43; compression < half size); perplexity = 2^H or e^H (2-bit → PPL 4; PPL 3 → 1-in-3 next-token odds); three rules (structure, vocab, context length — Shannon ≤10 vs today 500–10,000); uses (capability proxy, contamination, deduplication, abnormal-text detection); caveats (post-training raises PPL — "entropy collapse"; quantization changes it unexpectedly).

68. **Expect** functional correctness (website/reservation; code execution accuracy — gcd example; unit tests; HumanEval/MBPP; text-to-SQL Spider/BIRD-SQL/WikiSQL; pass@k mechanics with 5/10 = 50%; game bots, measurable objectives); similarity against references (reference-based/free; human-gold-standard; four similarity types); exact match (short answers; contains-match trap); lexical (token overlap 80/60 example; edit distance ops; n-grams; BLEU/ROUGE/METEOR++/TER/CIDEr; three drawbacks); semantic (embeddings + cosine 1/–1; BERTScore/MoverScore; exact-but-subjective nuance); embeddings (sizes 100–10,000, Table 3-2 numbers, MTEB, CLIP/ULIP/ImageBind joint embeddings).

69. **Expect** why AI judges (fast/cheap/reference-free/any criterion/masses/explainable; 58% LangChain; 85% vs 81% MT-Bench; 0.98 AlpacaEval); three modes (self-score, vs reference, vs other response → preference data); prompting (task, criteria, scoring system — classification/discrete/continuous; classification > numerical; discrete > continuous; include examples); limitations (inconsistency — examples raised consistency 65%→77.5% but quadrupled cost, consistency ≠ accuracy; criteria ambiguity — faithfulness = MLflow 1–5 vs Ragas 0/1 vs LlamaIndex YES/NO, "don't trust a judge you can't see"; cost/latency — 2× calls generate+evaluate, 4× for three criteria, spot-checking; biases — self-bias 10%/25%, first-position, verbosity, privacy/IP); judge selection (stronger/weaker/self-evaluation + sanity checks; specialized judges — reward model Cappy 360M, reference-based BLEURT/Prometheus, preference models PandaLM/JudgeLM).

70. **Expect** pointwise vs comparative (dancing contest analogy; easier for subjective quality); history (Anthropic 2021; Chatbot Arena; matches/win rates; ties); rating algorithms (Elo/Bradley–Terry/TrueSkill from sports; Elo sensitive to order → Arena switched; ×400 + 1000 scaling, Llama-13b = 800; ranking = predictive problem); challenges (scalability — 57 models/244K comparisons/1,596 pairs/~153 per pair; transitivity questionable; new/private models; matching algorithms; standardization/quality — crowdsourcing pros/cons, 180 hellos, gaming, hard-prompt filtering, Scale trained evaluators, product-embedded, AI vs random users; comparative-to-absolute gap — B-beats-A three scenarios, 70%-ticket example, 1% win-rate variance, cost-benefit); future (human limits — Llama 2 paper; never saturates; hard to game; discriminating signals; offline/online roles; summary: supplement subjective with exact/human evaluation; preference models bridge evaluation and alignment).
