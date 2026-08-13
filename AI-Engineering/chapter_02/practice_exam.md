# Practice Exam — Chapter 2: Understanding Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 2
**Instructions:** Allow ~45–55 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What is the most dominant architecture for language-based foundation models as of the book's writing?
   a) Transformer architecture (Vaswani et al., 2017)
   b) Seq2seq with RNNs
   c) GANs
   d) State space models

2. Which organization created the Common Crawl dataset used in most disclosed training data?
   a) Google
   b) A nonprofit organization that crawls websites
   c) OpenAI
   d) Meta

3. English accounts for roughly what percentage of Common Crawl data?
   a) 5.97%
   b) 75%
   c) 45.88%
   d) 18.15%

4. The MMLU benchmark consists of:
   a) 1,000 multiple-choice problems across 10 subjects
   b) 14,000 multiple-choice problems across 57 subjects
   c) 57 multiple-choice problems across 14,000 subjects
   d) 52 translated short texts

5. On Project Euler math problems, Yennie Jun found that GPT-4 solved English problems compared to Armenian or Farsi:
   a) More than three times as often
   b) Equally often
   c) Less often
   d) Only when translated to Chinese

6. On the MASSIVE dataset, what is the median token length for Burmese text conveying the same meaning as 7 English tokens?
   a) 32
   b) 14
   c) 20
   d) 72

7. Which of the following is a famous domain-specific model mentioned in the chapter?
   a) ChatGPT
   b) Gemini
   c) DeepMind's AlphaFold
   d) Stable Diffusion

8. What two problems with seq2seq did the transformer architecture address?
   a) Vanishing gradients and exploding gradients
   b) Slow training and high memory
   c) Only using the final hidden state of the input, and sequential (slow) RNN processing
   d) Vocabulary size and tokenization

9. When was the attention mechanism introduced relative to the transformer paper?
   a) Three years before the transformer paper
   b) The same year as the transformer paper
   c) Three years after the transformer paper
   d) It was invented with the transformer

10. In attention, the query vector (Q) represents:
    a) A previous token's content
    b) The current state of the decoder at each decoding step
    c) The final hidden state of the encoder
    d) The vocabulary size

11. What is the hidden dimension size of Llama 2-7B?
    a) 128
    b) 5120
    c) 4096
    d) 8192

12. During the prefill step of transformer inference:
    a) The model generates one output token at a time
    b) The model searches the vocabulary greedily
    c) The model applies softmax twice
    d) The model processes input tokens in parallel and builds key/value state

13. A transformer block contains which two modules?
    a) An attention module and an MLP module
    b) An embedding module and an output layer
    c) A prefill module and a decode module
    d) A query module and a value module

14. What is the mathematical definition of ReLU?
    a) ReLU(x) = x²
    b) ReLU(x) = max(0, x)
    c) ReLU(x) = sigmoid(x)
    d) ReLU(x) = tanh(x)

15. How many parameters are active per token for Mixtral 8x7B?
    a) 56 billion
    b) 46.7 billion
    c) 12.9 billion
    d) 7 billion

16. A model with 7 billion parameters stored in 2 bytes per parameter needs at least how much GPU memory for inference?
    a) 7 GB
    b) 32 GB
    c) 128 GB
    d) 14 GB

17. According to the Chinchilla scaling law, a compute-optimal 3B-parameter model needs approximately how many training tokens?
    a) 60 billion
    b) 3 billion
    c) 300 billion
    d) 1.4 trillion

18. The Chinchilla scaling law states that compute-optimal training needs roughly how many training tokens per parameter?
    a) 2
    b) 20
    c) 200
    d) 0.2

19. GPT-3-175B was trained using approximately how many FLOPs?
    a) 10²²
    b) 6 × 10¹³
    c) 3.14 × 10²³
    d) 5.2 × 10¹⁸

20. One FLOP/s-day is equal to:
    a) 60 FLOPs
    b) 24 FLOPs
    c) 1000 FLOPs
    d) 86,400 FLOPs

21. What level of compute utilization is considered "great"?
    a) Above 70%
    b) Above 90%
    c) Above 50%
    d) Exactly 100%

22. What was awarded as the first prize in the 2023 Inverse Scaling Prize?
    a) $100,000 to one team
    b) No first prize was awarded
    c) $20,000 to one team
    d) Eleven first prizes

23. According to Longpre et al. (2024), what portion of C4's most critical sources is now restricted?
    a) 8%
    b) 28%
    c) 45%
    d) 90%

24. For InstructGPT, what fraction of compute went to post-training versus pre-training?
    a) 50% / 50%
    b) 10% / 90%
    c) 33% / 67%
    d) 2% / 98%

25. Supervised finetuning is also referred to as:
    a) Behavior cloning
    b) Preference finetuning
    c) Test time compute
    d) Reinforcement learning from human feedback

26. A reward model is trained on which kind of data?
    a) Pointwise scores
    b) Comparison data (prompt, winning_response, losing_response)
    c) Unlabeled text
    d) Demonstration data only

27. The inter-labeler agreement among InstructGPT reward-model labelers was approximately:
    a) 50%
    b) 90%
    c) 73%
    d) 99%

28. The InstructGPT reward-model loss for a sample (x, y_w, y_l) is:
    a) −log(σ(rθ(x, y_l) − rθ(x, y_w)))
    b) log(σ(rθ(x, y_w)))
    c) rθ(x, y_w) + rθ(x, y_l)
    d) −log(σ(rθ(x, y_w) − rθ(x, y_l)))

29. Which preference finetuning approach did Meta switch to for Llama 3?
    a) DPO (Direct Preference Optimization)
    b) RLHF
    c) RLAIF
    d) PPO

30. Which companies use a reward model with the best of N strategy, skipping RL?
    a) OpenAI and Google
    b) Stitch Fix and Grab
    c) Meta and Anthropic
    d) Microsoft and NVIDIA

31. With logits [1, 2] and temperature 0.5, the softmax probabilities are:
    a) [0.27, 0.73]
    b) [0.50, 0.50]
    c) [0.12, 0.88]
    d) [0.73, 0.27]

32. In practice, setting temperature to 0 makes the model:
    a) Divide logits by zero
    b) Sample uniformly
    c) Double the logits
    d) Pick the token with the largest logit (arg max)

33. In top-k sampling, k is commonly set to a value in which range?
    a) 50 to 500
    b) 1 to 10
    c) 1000 to 5000
    d) 0.9 to 0.95

34. Common values for top-p (nucleus) sampling range from:
    a) 0.1 to 0.3
    b) 0.9 to 0.95
    c) 1 to 2
    d) 50 to 500

35. OpenAI found that using verifiers produced roughly the same performance boost as what model-size increase?
    a) 3×
    b) 10×
    c) 30×
    d) 300×

36. In OpenAI's experiment on scaling the number of sampled outputs, performance peaked at:
    a) 10 outputs
    b) 100 outputs
    c) 10,000 outputs
    d) 400 outputs

37. How many outputs per question did Google sample when evaluating Gemini on MMLU?
    a) 32
    b) 5
    c) 400
    d) 3

38. LinkedIn's defensive YAML parser increased the percentage of correct YAML outputs from 90% to:
    a) 95%
    b) 99.99%
    c) 99%
    d) 100%

39. What does an API's JSON mode typically guarantee?
   a) That the JSON content is factually correct
   b) That the JSON objects contain required keys
   c) That outputs can never be truncated
   d) That the outputs are valid JSON (syntax), not the content

40. The "self-delusion" hypothesis of hallucination was originally expressed by:
    a) Leo Gao at OpenAI
    b) John Schulman at OpenAI
    c) Zhang et al. at NYU
    d) Ortega et al. at DeepMind

## Section B: True/False (1 point each)

41. **T / F** — Seq2seq architecture used RNNs as both encoder and decoder.

42. **T / F** — The transformer was the first architecture to use the attention mechanism.

43. **T / F** — Prefill processes input tokens in parallel; decode generates one output token at a time.

44. **T / F** — Increasing a model's context length increases its total number of parameters.

45. **T / F** — Mixtral 8x7B has 56 billion total parameters.

46. **T / F** — A 90%-sparse 7B-parameter model has 700 million non-zero parameters.

47. **T / F** — The number of training tokens in a dataset is always the same as the number of tokens in the dataset.

48. **T / F** — The Chinchilla scaling law says model size and training tokens should be scaled equally.

49. **T / F** — Post-training typically consumes more compute than pre-training for InstructGPT.

50. **T / F** — In RLHF, pointwise scores (not rankings) are used to train the reward model.

51. **T / F** — A weak model can judge a stronger model because judging is easier than generation.

52. **T / F** — Higher temperature makes model outputs more creative but potentially less coherent.

53. **T / F** — Top-p (nucleus) sampling reduces the softmax computation load compared to top-k.

54. **T / F** — The logprob of a sequence of tokens is the sum of the logprobs of its tokens.

55. **T / F** — The InstructGPT paper showed that RLHF made hallucination worse, yet human labelers still preferred the RLHF model.

## Section C: Short Answer (model answers)

56. Why do model developers often have to rely on available data (e.g., Common Crawl) instead of data tailored to their needs, and what consequence does this have?

57. Explain why translation (to English and back) is not an ideal workaround for non-English language support.

58. What are the two problems with seq2seq that the transformer architecture fixed, and how did the attention mechanism fix them?

59. What are the three numbers that signal a model's scale, and what is each a proxy for?

60. State the Chinchilla scaling law and its compute-optimal rule for model size vs training tokens.

61. What are the two steps of post-training, and what two pre-training problems do they address?

62. Why is comparison data used instead of pointwise scores to train reward models?

63. Contrast top-k and top-p sampling, including which one reduces softmax computation load.

64. What selection methods can be used to pick the best output in test time compute?

65. State the two hypotheses for why language models hallucinate, and the two proposed mitigation approaches for each.

## Section D: Essay (grading notes)

66. Explain the role of training data in determining a model's capabilities: the dominance of English, low-resource languages, the consequences for non-English performance, tokenization cost differences, and how domain-specific models address gaps.

67. Describe the transformer architecture: the problems it solved, the attention mechanism (Q, K, V), prefill vs decode, multi-headed attention, and the building blocks of a transformer model.

68. Discuss model scale: the three numbers of scale, dataset vs training tokens, FLOPs/FLOP-s/FLOP/s-day distinctions, the Chinchilla scaling law, and the scaling bottlenecks (data and electricity).

69. Explain post-training: SFT (demonstration data, labelers), preference finetuning (reward model, comparison data, RLHF/DPO), best of N, and the key numbers (InstructGPT compute split, labeling costs, inter-labeler agreement).

70. Discuss sampling and the probabilistic nature of AI: temperature, top-k, top-p, logprobs, test time compute (best of N, verifiers, self-consistency), structured outputs, inconsistency, and hallucination — and why building workflows around probabilistic behavior is the chapter's takeaway.

---

## Answer Key

### Section A: Multiple Choice
1. a
2. b
3. c
4. b
5. a
6. d
7. c
8. c
9. a
10. b
11. c
12. d
13. a
14. b
15. c
16. d
17. a
18. b
19. c
20. d
21. a
22. b
23. c
24. d
25. a
26. b
27. c
28. d
29. a
30. b
31. c
32. d
33. a
34. b
35. c
36. d
37. a
38. b
39. d
40. d

### Section B: True/False
41. **T** — Seq2seq used RNNs for both encoder and decoder.
42. **F** — Attention was introduced three years before the transformer; the transformer showed it could be used without RNNs.
43. **T** — Prefill is parallel input processing; decode is sequential token generation.
44. **F** — Longer context increases memory footprint but not the number of parameters.
45. **F** — Raw 8 × 7B = 56B, but shared parameters make it 46.7B total (12.9B active per token).
46. **T** — 7B × 10% = 700M non-zero parameters.
47. **F** — Dataset tokens × epochs = training tokens (1T-token dataset over 2 epochs = 2T training tokens).
48. **T** — For every doubling of model size, double the training tokens.
49. **F** — InstructGPT: 2% post-training vs 98% pre-training.
50. **F** — Only the rankings (not the 1–7 scores) trained the reward model.
51. **T** — Judging is easier than generation.
52. **T** — Higher T → more creative, potentially less coherent.
53. **F** — Top-p does NOT reduce softmax computation load; top-k does.
54. **T** — Log of a product = sum of logs.
55. **T** — RLHF worsened hallucinations for InstructGPT, but humans preferred the RLHF model overall.

### Section C: Short Answer (model answers)
56. **Available data.** Collecting sufficient data to train a large model is expensive and hard, so developers use whatever is available (Common Crawl, C4), even if it doesn't exactly match their needs. Consequence: models perform well on tasks present in the training data but not necessarily on the tasks you care about — hence the need for data curation (languages, domains).

57. **Translation workaround.** (1) It requires a model that sufficiently understands the under-represented source language to translate it; (2) translation causes information loss — e.g., Vietnamese relationship-denoting pronouns all collapse to "I"/"you" in English, losing relational context.

58. **Seq2seq problems.** (1) The decoder used only the final hidden state of the input (like writing a book report from a summary) → poor output quality; (2) RNN encoder/decoder processed tokens sequentially → slow for long sequences. Attention lets the model weigh every input token when generating each output token (like referencing any page in the book) and allows parallel input processing.

59. **Three numbers of scale.** Parameters (proxy for learning capacity); training tokens (proxy for how much was learned); FLOPs (proxy for training cost).

60. **Chinchilla law.** Given a compute budget, compute-optimal training needs training tokens ≈ 20 × model size (3B model → ~60B tokens), and model size and tokens should be scaled equally (doubling size → double tokens).

61. **Post-training steps.** SFT (finetune on high-quality instruction/demonstration data to optimize for conversations) and preference finetuning (RLHF/DPO to align outputs with human preference). They address (1) models optimized for completion, not conversation, and (2) outputs that can be toxic, rude, or wrong.

62. **Comparison vs pointwise.** Pointwise scores vary wildly (5 vs 7 for the same sample; even the same labeler twice); comparing two responses is easier and more reliable. InstructGPT's UI collected 1–7 scores, but only rankings trained the reward model (inter-labeler agreement ≈73%).

63. **Top-k vs top-p.** Top-k fixes the count (k = 50–500) and reduces softmax computation by computing it over only top-k logits. Top-p (nucleus) includes the smallest set of most-likely values whose cumulative probability exceeds p (0.9–0.95), is contextually dynamic, but doesn't reduce softmax load.

64. **Selection methods.** Highest output probability / highest average logprob (OpenAI's best_of); reward-model or verifier scoring; most common output (self-consistency, Wang et al., 2023); application-specific heuristics (shortest response; keep generating until valid SQL).

65. **Hallucination hypotheses.** (1) Self-delusion (Ortega et al., DeepMind): the model can't differentiate given data from its own generated data; mitigations = RL (distinguish user prompts/observations from model tokens/actions) and SL (factual + counterfactual signals). (2) Mismatched internal knowledge (Leo Gao; echoed by Schulman): labeler responses use knowledge the model lacks during SFT; mitigations = verification (retrieve sources) and RL with a reward function that punishes fabrication.

### Section D: Essay (grading notes)
66. **Expect** English = 45.88% of Common Crawl (Russian 5.97%); low-resource languages (Punjabi ratio 231.56); MMLU (14,000 Qs, 57 subjects) — GPT-4 much better in English than Telugu/Marathi/Punjabi; Project Euler — English >3× Armenian/Farsi, 0/6 Burmese/Amharic; MASSIVE median token length (English 7, Hindi 32, Burmese 72) → ~10× slower/costlier for Burmese; NewsGuard — more misinformation in Chinese; translation workaround loses info (Vietnamese pronouns); language- and domain-specific models (ChatGLM, CroissantLLM, PhoGPT, Jais; AlphaFold ~100k proteins, BioNeMo, Med-PaLM2).

67. **Expect** seq2seq (2014, RNN encoder/decoder) problems (final-hidden-state-only decoding; sequential processing); attention (Q = current decoder state, K = token identity, V = token content; dot product Q·K = weight; formula Attention(Q,K,V) = softmax(QKᵀ/√d)V); attention introduced 3 years before transformer (GNMT 2016), took off when usable without RNNs; prefill (parallel, builds K/V) vs decode (sequential); multi-head (Llama 2-7B: 4096 dim, 32 heads, 128 per head); transformer block = attention module (Q/K/V/output-projection matrices) + MLP module (linear + activation); embedding/positional embedding before, output layer/unembedding/model head after; Table 2-4 dimensions; alternatives (RWKV, SSMs/S4/H3, Mamba, Jamba).

68. **Expect** parameters (capacity), training tokens (learning), FLOPs (cost); dataset tokens vs training tokens (epochs; Llama 1.4T/2T/15T; RedPajama-v2 30T); FLOP vs FLOP/s vs FLOP/s-day (86,400); H100 NVL 60 TeraFLOP/s; GPT-3-175B = 3.14×10²³ FLOPs; 256 H100s ≈ 236 days ≈ 7.8 months; $4M at 70% utilization/$2 per hour; Chinchilla ~20 tokens/param (3B→60B); bottlenecks: data (dataset growth > new data, C4 28%→45% restricted, AI-generated data/Grok, model collapse) and electricity (1–2% → 4–20% by 2030; growth cap ~50×).

69. **Expect** SFT = demonstration data (prompt, response), behavior cloning; labelers ~90% college degree, >1/3 master's, up to 30 min/pair, $10/pair → 13,000 pairs ≈ $130k; LAION volunteers (13,500, 90% male); Gopher dialogue heuristics. Preference finetuning = reward model (comparison data; 3–5 min; $3.50 vs $25; 73% agreement; loss −log(σ(rθ(x,y_w) − rθ(x,y_l))); weak model can judge strong) + RL (PPO); DPO for Llama 3; best of N (Stitch Fix, Grab); InstructGPT 98% pre-training/2% post-training.

70. **Expect** sampling fundamentals (logits→softmax; greedy); temperature (logits [1,2]: T=1 [0.27,0.73], T=0.5 [0.12,0.88]; 0.7 recommended; providers cap 0–2; T=0 = arg max); logprobs (underflow; sum = sequence logprob; average logprob); top-k (50–500) vs top-p (0.9–0.95) vs min-p; stopping conditions; test time compute (best of N, beam search, diversity, cost ~2×; selection: avg logprob, verifiers ≈30×, self-consistency, heuristics; OpenAI peak 400, Stanford log-linear to 10,000; Gemini 32 samples; robustness); structured outputs (semantic parsing; downstream JSON; five layers prompting→post-processing→test-time compute→constrained sampling→finetuning; LinkedIn YAML 90→99.99%; JSON mode caveats); inconsistency (two scenarios; cache/sampling variables/seed/hardware); hallucination (self-delusion vs mismatched knowledge; RLHF nuance); closing thesis — build workflows around probabilistic nature, start with evaluation.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely FLOPs/FLOP-s/FLOP-s-day, temperature math, top-k vs top-p, reward-model loss, or hallucination hypotheses).
- **Below 70%**: Re-read the chapter study notes and retake.
