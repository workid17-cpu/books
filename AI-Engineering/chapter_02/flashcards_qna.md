# Chapter 2 Flashcards: Understanding Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 2

---

## Part 1Terms → Definitions

**Q:** What is Common Crawl?
**A:** A dataset created by a nonprofit organization that sporadically crawls websites on the internet (about 2–3 billion web pages per month in 2022–2023); a common training-data source (GPT-3, Gemini) whose quality is questionable.

**Q:** What is C4?
**A:** The Colossal Clean Crawled Corpus — Google's "clean subset" of Common Crawl.

**Q:** What is a low-resource language?
**A:** A language with limited availability as training data — typically one not among the languages with ≥1% share in Common Crawl.

**Q:** What does the "world:Common Crawl ratio" measure?
**A:** The ratio between a language's share of world population and its share of Common Crawl; ideally 1, higher = more under-represented (Punjabi 231.56; English 0.40).

**Q:** What is MMLU?
**A:** Massive Multitask Language Understanding — a benchmark of 14,000 multiple-choice problems across 57 subjects; GPT-4 performs much better in English than in under-represented languages.

**Q:** What is the transformer architecture?
**A:** The dominant architecture for language-based foundation models (Vaswani et al., 2017), based on the attention mechanism; it dispenses with RNNs so input tokens are processed in parallel.

**Q:** What is seq2seq (sequence-to-sequence)?
**A:** The 2014 architecture with an RNN-based encoder (processes inputs) and decoder (generates outputs); the transformer's predecessor, credited with big translation/summarization gains.

**Q:** What is the attention mechanism?
**A:** A mechanism that weighs the importance of different input tokens when generating each output token, using query (Q), key (K), and value (V) vectors.

**Q:** What is the query vector (Q)?
**A:** Represents the current state of the decoder at each decoding step — like a person looking for information to create a summary.

**Q:** What is the key vector (K)?
**A:** Represents a previous token — like a page number in a book; previous tokens include both input tokens and previously generated tokens.

**Q:** What is the value vector (V)?
**A:** Represents the actual value of a previous token as learned by the model — like a page's content.

**Q:** How is attention weight computed?
**A:** By a dot product between the query vector and a token's key vector; a high score means more of that token's value vector is used.

**Q:** What is multi-headed attention?
**A:** Splitting Q, K, V into smaller vectors per attention head so the model attends to different groups of previous tokens simultaneously (Llama 2-7B: 32 heads × 128-dim = 4096).

**Q:** What is prefill?
**A:** The first inference step: the model processes input tokens in parallel and creates the intermediate state (key and value vectors for all input tokens) needed to generate the first output token.

**Q:** What is decode?
**A:** The second inference step: the model generates one output token at a time.

**Q:** What is a transformer block (layer)?
**A:** A building block containing an attention module (query, key, value, and output projection matrices) plus an MLP module (linear layers separated by nonlinear activation functions); the number of blocks = the model's number of layers.

**Q:** What are the embedding and output layers in a transformer?
**A:** The embedding module (embedding matrix + positional embedding matrix) comes before the blocks; the output layer / unembedding layer / model head (one matrix) maps output vectors into token probabilities after the blocks.

**Q:** What is an activation function and why simple ones win?
**A:** A nonlinear function (ReLU, GELU) that lets linear layers learn nonlinear patterns; fancier functions don't work better and cost more compute, so simple ones are preferred.

**Q:** What is a sparse model?
**A:** A model with a large percentage of zero-value parameters; a 7B model that is 90% sparse has only 700M non-zero parameters and can need less compute than a small dense model.

**Q:** What is a mixture-of-experts (MoE) model?
**A:** A sparse model (Shazeer et al., 2017) divided into groups of parameters (experts); only a subset of experts is active for each token (e.g., Mixtral 8x7B: 2 of 8 experts active per token).

**Q:** What is FLOP?
**A:** Floating point operation — a standardized unit of compute requirement; GPT-3-175B = 3.14 × 10²³ FLOPs, PaLM-2 largest = 10²² FLOPs.

**Q:** What is the difference between FLOPs and FLOP/s?
**A:** FLOPs = total operations for a task; FLOP/s = operations per second (machine peak performance, e.g., H100 NVL = 60 TeraFLOP/s).

**Q:** What is inverse scaling?
**A:** Phenomena where bigger models perform worse; e.g., Anthropic (2022) found more alignment training led to models that align less (Perez et al., 2022).

**Q:** What is the Chinchilla scaling law?
**A:** DeepMind 2022: for compute-optimal training, training tokens should be ~20× the model size, with model size and tokens scaled equally (3B model → ~60B tokens).

**Q:** What is scaling extrapolation (hyperparameter transfer)?
**A:** Studying hyperparameters on small models and extrapolating to much larger models (Microsoft & OpenAI transferred from 40M to 6.7B params, 2022).

**Q:** What is post-training?
**A:** Training after pre-training to fix two issues: (1) the model is optimized for completion, not conversation, and (2) outputs can be toxic/wrong. Two steps: supervised finetuning + preference finetuning.

**Q:** What is supervised finetuning (SFT)?
**A:** Finetuning the pre-trained model on high-quality (prompt, response) demonstration data to optimize for conversations instead of completion; also called behavior cloning.

**Q:** What is demonstration data?
**A:** (Prompt, response) pairs used for SFT; showing examples of appropriate responses so the model "clones" that behavior.

**Q:** What is preference finetuning?
**A:** Further finetuning so the model outputs responses aligned with human preference, typically with reinforcement learning; techniques include RLHF, DPO, RLAIF.

**Q:** What is RLHF?
**A:** Reinforcement learning from human feedback — the earliest popular preference-finetuning algorithm: (1) train a reward model scoring the model's outputs, (2) optimize the model to maximize reward-model scores.

**Q:** What is a reward model?
**A:** Given (prompt, response), outputs a score for how good the response is; trained on comparison data (prompt, winning_response, losing_response).

**Q:** What is comparison data?
**A:** Labeled data of the form (prompt, winning_response, losing_response); ranking three responses (A > B > C) yields three ranked pairs: (A>B), (A>C), (B>C).

**Q:** What is pointwise evaluation?
**A:** Scoring each response independently (e.g., 5/10 vs 7/10) — harder than comparison because labeler scores vary widely.

**Q:** What is DPO (Direct Preference Optimization)?
**A:** A simpler preference-finetuning alternative to RLHF (Rafailov et al., 2023); Meta switched from RLHF (Llama 2) to DPO (Llama 3) to reduce complexity.

**Q:** What is PPO?
**A:** Proximal policy optimization — the reinforcement learning algorithm (OpenAI, 2017) often used to finetune a model against the reward model.

**Q:** What is the "best of N" strategy?
**A:** Generate N outputs and pick the best one (highest probability/score, or a heuristic); Stitch Fix and Grab use reward-model-scored best of N without RL.

**Q:** What is sampling?
**A:** The process by which a model chooses an output from all possible options; it makes AI outputs probabilistic.

**Q:** What is a logit?
**A:** A raw pre-softmax score for each possible token; logits don't sum to 1 and can be negative.

**Q:** What is the softmax function?
**A:** Converts logits to probabilities: pᵢ = e^(xᵢ) / Σⱼ e^(xⱼ); requires two passes over all values.

**Q:** What is temperature (T)?
**A:** A constant that divides logits before softmax; higher T = more creative/chaotic, lower T = more consistent/boring; providers typically cap T in [0, 2]; 0.7 recommended for creative use.

**Q:** What is logprob?
**A:** Log probability — probability in log scale, which reduces the underflow problem with large vocabularies; the sequence logprob is the sum of its token logprobs.

**Q:** What is top-k sampling?
**A:** Performing softmax over only the top-k logits (k commonly 50–500); reduces computation; smaller k → more predictable but less interesting text.

**Q:** What is top-p (nucleus) sampling?
**A:** Including the smallest set of most-likely values whose cumulative probability exceeds p (common 0.9–0.95); more contextually dynamic than top-k; doesn't reduce softmax load.

**Q:** What is test time compute?
**A:** Allocating more compute at inference to improve quality — generating multiple outputs (best of N), beam search, verifiers/reward models to select the best.

**Q:** What is a verifier?
**A:** A model (often a reward model) that scores candidate outputs; OpenAI found verifiers gave roughly the same boost as a 30× model-size increase (Cobbe et al., 2021).

**Q:** What is self-consistency?
**A:** Picking the most common output among a set of outputs (Wang et al., 2023) — useful for exact-answer tasks; Google sampled 32 outputs per MMLU question for Gemini.

**Q:** What is semantic parsing?
**A:** Converting natural language into a structured, machine-readable format (e.g., text-to-SQL, text-to-regex).

**Q:** What is constrained sampling?
**A:** Filtering the logit vector to keep only tokens that meet grammar constraints, then sampling among valid tokens; less generalizable (each format needs its own grammar) and adds latency.

**Q:** What is feature-based transfer?
**A:** Modifying a model's architecture before finetuning — e.g., appending a classifier head so a foundation model outputs only pre-specified classes (discussed in Chapter 7).

**Q:** What is inconsistency?
**A:** A model generating very different responses for the same or slightly different prompts; two scenarios: same input→different outputs, and slightly different input→drastically different outputs.

**Q:** What is hallucination?
**A:** A model giving a response not grounded in facts; a concern for generative models even before LLMs (mentioned as early as 2016, Goyal et al.).

**Q:** What is the self-delusion hypothesis of hallucination?
**A:** (Ortega et al., DeepMind 2021) A model hallucinates because it can't differentiate between the data it's given and the data it generates; also called snowballing hallucinations (Zhang et al., 2023).

**Q:** What is the mismatched-internal-knowledge hypothesis of hallucination?
**A:** (Leo Gao; echoed by John Schulman) Hallucination is caused by the mismatch between the model's internal knowledge and the labeler's knowledge — SFT teaches hallucination when labelers use knowledge the model lacks.

## Part 2Short Answer

**Q:** What are the three design decisions that explain most differences between foundation models?
**A:** Training data; model architecture and size; and how the model is post-trained to align with human preferences.

**Q:** Why is English-dominated training data a problem, and what do teams do about it?
**A:** English is ~45.88% of Common Crawl, so general-purpose models work much better in English (GPT-4's MMLU scores drop for Telugu/Marathi/Punjabi; math fails 0/6 in Burmese/Amharic). Teams build language-specific models (ChatGLM, YAYI, PhoGPT, Jais) and curate data for under-represented languages.

**Q:** Why isn't "translate to English, answer, translate back" a good workaround?
**A:** It requires a model that sufficiently understands the source language, and translation causes information loss — e.g., Vietnamese relationship pronouns all collapse to "I"/"you."

**Q:** What were the two problems with seq2seq that the transformer fixed?
**A:** (1) The decoder used only the final hidden state of the input (like writing a book report from a summary) — limits output quality; (2) RNNs process sequentially, so both input processing and output generation are slow for long sequences.

**Q:** When was the attention mechanism introduced relative to the transformer paper?
**A:** Attention was introduced three years before the transformer paper (2014; used in GNMT 2016); it only took off when the transformer showed it could be used without RNNs.

**Q:** Why is extending context length hard for transformers?
**A:** Each previous token needs a key and value vector; longer sequences mean more K/V vectors to compute and store, so attention memory grows with context.

**Q:** How does MoE make Mixtral 8x7B cheaper to run than its parameter count suggests?
**A:** 8 experts × 7B = 56B raw, but shared parameters make it 46.7B total; only 2 experts are active per token → 12.9B active parameters, so cost/speed is like a 12.9B model.

**Q:** Why is the number of tokens the preferred dataset-size metric for LMs?
**A:** Training samples vary hugely in value (a book vs a sentence), and the token is the unit the model operates on — token count measures how much the model can potentially learn.

**Q:** What are the three numbers of scale and what is each a proxy for?
**A:** Number of parameters (learning capacity); number of training tokens (how much learned); number of FLOPs (training cost).

**Q:** What is the difference between dataset tokens and training tokens?
**A:** Training tokens = dataset tokens × epochs. A 1T-token dataset trained for 2 epochs = 2T training tokens. Large models are typically pre-trained on only one epoch.

**Q:** What did the Inverse Scaling Prize find?
**A:** It found tasks where larger LMs perform worse; of 99 submissions it awarded 11 third prizes ($5,000 each) but no second or first prizes, because no submission demonstrated failures in the real world.

**Q:** State the Chinchilla compute-optimal rule with an example.
**A:** Training tokens ≈ 20 × model size; a 3B model needs ~60B training tokens, and scaling size and tokens together (doubling model size → double tokens) stays compute-optimal.

**Q:** What are the two scaling bottlenecks and their key facts?
**A:** Data — dataset growth outpaces new web data (Villalobos et al., 2022), C4 critical sources went from 28% to 45% restricted (Longpre et al., 2024), and AI-generated data risks model collapse. Electricity — data centers use 1–2% of global electricity now, projected 4–20% by 2030, capping growth at ~50×.

**Q:** What are the two post-training steps, and what problems do they fix?
**A:** SFT (finetune on high-quality instruction/demonstration data → conversational responses) and preference finetuning (RLHF/DPO → responses aligned with human preference). Together they fix the "completion not conversation" and "toxic/wrong outputs" problems.

**Q:** Why does preference finetuning use comparison data instead of direct scores?
**A:** Pointwise scores vary wildly (5 vs 7 for the same sample); comparing two responses is easier and more reliable. InstructGPT's UI collected 1–7 scores, but only the rankings trained the reward model (inter-labeler agreement ≈73%).

**Q:** Write the InstructGPT reward-model loss in words.
**A:** For each sample (prompt x, winning y_w, losing y_l), the loss is −log(σ(rθ(x, y_w) − rθ(x, y_l))): maximize the sigmoid of the winning-minus-losing score gap; minimize the expected loss over all samples.

**Q:** Why can a weak model judge a stronger model?
**A:** Judging is believed to be easier than generation — so reward models need not be more powerful than the foundation model they score.

**Q:** Explain the temperature example with logits [1, 2].
**A:** T=1 → softmax probabilities [0.27, 0.73] (B picked 73% of the time); T=0.5 → [0.12, 0.88] (B 88%). Higher T flattens the distribution → more creative; T→0 concentrates probability on B.

**Q:** What is temperature 0 in practice?
**A:** Technically impossible (can't divide logits by 0); in practice the model just picks the token with the largest logit (arg max), skipping the softmax.

**Q:** Why are logprobs preferred over raw probabilities, and how is sequence quality scored?
**A:** Log scale avoids underflow (vocab of 100,000 → many tiny probabilities); the sequence logprob = sum of token logprobs, and the average logprob (sum ÷ length) avoids biasing against long sequences — what the OpenAI API's best_of uses.

**Q:** Contrast top-k and top-p sampling.
**A:** Top-k fixes the count (k = 50–500) and reduces softmax computation; top-p fixes the cumulative probability (0.9–0.95) and adapts to context but doesn't reduce softmax load.

**Q:** What selection methods are available for test time compute?
**A:** Highest output probability / highest average logprob; reward-model or verifier scoring; most common output (self-consistency); application heuristics (shortest response, keep generating until valid SQL).

**Q:** What are the two scenarios requiring structured outputs?
**A:** (1) The task itself requires structured output (semantic parsing: text-to-SQL, text-to-regex, classification into valid classes); (2) downstream applications consume the output (e.g., email as JSON with specific keys; agentic tool inputs).

**Q:** Name the five layers for guiding structured outputs and their strength.
**A:** Prompting, post-processing, test time compute (the three "bandages"), then constrained sampling and finetuning (the "intensive treatment"). Only constrained sampling or finetuning reliably guarantee formats.

**Q:** What caveats apply to OpenAI's JSON mode?
**A:** It guarantees only that outputs are valid JSON syntax, not content; outputs truncated at the max-token limit can still be unparsable, and setting max tokens too long makes responses slow and expensive.

**Q:** What are the two inconsistency scenarios and how is each mitigated?
**A:** (1) Same input → different outputs: cache answers, fix sampling variables (temperature/top-p/top-k), fix the seed. (2) Slightly different input → drastically different outputs: harder — crafting prompts (Ch 5) and a memory system (Ch 6) help; hardware differences can break consistency regardless.

**Q:** State the two hallucination hypotheses and how they complement each other.
**A:** Self-delusion (Ortega/DeepMind) — the model can't distinguish given vs generated data; caused by self-supervision. Mismatched internal knowledge (Gao/Schulman) — labelers use knowledge the model lacks during SFT; caused by supervision.

**Q:** What mitigations did DeepMind propose for hallucination?
**A:** Reinforcement learning (make the model distinguish user prompts/observations from its own tokens/actions) and supervised learning (include factual and counterfactual signals in training data).

**Q:** What did Schulman propose, and what does the InstructGPT paper show about RLHF?
**A:** Schulman: LLMs know if they know something — use verification (retrieve sources) and RL with a reward function that punishes fabrication. InstructGPT paper: RLHF made hallucinations worse, yet human labelers still preferred the RLHF model over SFT alone.

**Q:** How can prompting reduce hallucination?
**A:** Add "Answer as truthfully as possible, and if you're unsure, say, 'Sorry, I don't know,'" and ask for concise responses — fewer tokens means fewer chances to fabricate.

## Part 3Fill-in-the-Blank

**Q:** The nonprofit dataset that crawls ~2–3 billion web pages per month is called ______.
**A:** Common Crawl.

**Q:** Google's clean subset of Common Crawl is called the Colossal Clean Crawled Corpus, or ______ for short.
**A:** C4.

**Q:** In Common Crawl, English accounts for ______% of the data; Russian, the second-most common language, accounts for ______%.
**A:** 45.88; 5.97.

**Q:** The ideal world:Common Crawl ratio for a language is ______; Punjabi's ratio of ______ makes it the most under-represented language in Table 2-2.
**A:** 1; 231.56.

**Q:** The MMLU benchmark contains ______ multiple-choice problems across ______ subjects.
**A:** 14,000; 57.

**Q:** On the MASSIVE dataset, the median token length is 7 for English, 32 for Hindi, and ______ for Burmese.
**A:** 72.

**Q:** DeepMind's AlphaFold was trained on the sequences and 3D structures of around ______ known proteins.
**A:** 100,000.

**Q:** The transformer architecture is based on the ______ mechanism and was proposed in the 2017 paper ______.
**A:** attention; "Attention Is All You Need" (Vaswani et al., 2017).

**Q:** The seq2seq architecture was introduced in ______ and was in the limelight from ______ to ______.
**A:** 2014; 2014; 2018.

**Q:** In transformer inference, the two steps are ______ (input processed in parallel, K/V state built) and ______ (one output token at a time).
**A:** prefill; decode.

**Q:** Llama 2-7B has a hidden dimension of ______ and ______ attention heads, giving each head a dimension of ______.
**A:** 4096; 32; 128.

**Q:** The attention formula is Attention(Q, K, V) = softmax(______)V.
**A:** QKᵀ/√d.

**Q:** ReLU is defined as ReLU(x) = ______; it was used by GPT-______, while GELU was used by GPT-______.
**A:** max(0, x); 2; 3.

**Q:** Mixtral 8x7B has ______ total parameters (shared parameters bring it below 8 × 7B) and ______ active parameters per token.
**A:** 46.7 billion; 12.9 billion.

**Q:** A 7B-parameter model that is 90% sparse has only ______ non-zero parameters.
**A:** 700 million.

**Q:** A 7-billion-parameter model stored in 2 bytes per parameter needs at least ______ GB of GPU memory for inference.
**A:** 14.

**Q:** Meta trained Llama 1, Llama 2, and Llama 3 on ______, ______, and ______ trillion tokens, respectively.
**A:** 1.4; 2; 15.

**Q:** RedPajama-v2 contains ______ trillion tokens, equivalent to about 450 million books or 5,400× the size of Wikipedia.
**A:** 30.

**Q:** GPT-3-175B was trained using ______ × 10²³ FLOPs; an H100 NVL can deliver ______ TeraFLOP/s.
**A:** 3.14; 60.

**Q:** 1 FLOP/s-day equals ______ FLOPs.
**A:** 86,400.

**Q:** At 70% utilization and $2/hour per H100, training GPT-3-175B on 256 H100s would cost about $______.
**A:** 4,142,811.43 (~4 million).

**Q:** Generally, ______% utilization is "okay" and anything above ______% is considered great.
**A:** 50; 70.

**Q:** The Chinchilla scaling law says compute-optimal training needs roughly ______ training tokens per parameter (so a 3B model needs ~______B tokens).
**A:** 20; 60.

**Q:** In 2023, the Inverse Scaling Prize awarded ______ third prizes and ______ second or first prizes.
**A:** 11; no.

**Q:** A drop in cross-entropy loss from ~3.4 to ~2.8 nats requires ______ times more training data.
**A:** 10.

**Q:** For InstructGPT, pre-training used ______% of compute and post-training used ______%.
**A:** 98; 2.

**Q:** Among InstructGPT demonstration-data labelers, ~______% had at least a college degree and more than one-third had a master's degree.
**A:** 90.

**Q:** The 13,000 (prompt, response) pairs used for InstructGPT would cost about $______ at $10 per pair.
**A:** 130,000.

**Q:** LAION mobilized ______ volunteers to generate 10,000 conversations with ______ messages in 35 languages.
**A:** 13,500; 161,443.

**Q:** Comparing two responses took labelers an average of three to ______ minutes (LMSYS); Llama-2 author Thomas Scialom said each comparison cost $______.
**A:** five; 3.50.

**Q:** InstructGPT labelers' inter-labeler agreement was around ______%; a set of three ranked responses (A > B > C) yields ______ ranked pairs.
**A:** 73; three.

**Q:** The reward model objective is −log(σ(rθ(x, y_w) − rθ(x, ______))).
**A:** y_l.

**Q:** For logits [1, 2], temperature 1 gives probabilities [0.27, 0.73]; temperature ______ gives [0.12, 0.88].
**A:** 0.5.

**Q:** A temperature of ______ is often recommended for creative use cases; model providers typically limit temperature to between 0 and ______.
**A:** 0.7; 2.

**Q:** Common top-p (nucleus) values range from ______ to ______.
**A:** 0.9; 0.95.

**Q:** OpenAI found verifiers produced roughly the same performance boost as a ______ × model-size increase (100M-parameter model + verifier ≈ 3B model without).
**A:** 30.

**Q:** In OpenAI's experiment, sampling more outputs helped only up to ______ outputs, after which performance decreased; Stanford found log-linear gains from 1 to ______ samples.
**A:** 400; 10,000.

**Q:** Google sampled ______ outputs per question when evaluating Gemini on MMLU.
**A:** 32.

**Q:** LinkedIn's defensive YAML parser raised correct YAML outputs from 90% to ______%.
**A:** 99.99.

**Q:** In June ______, a law firm was fined for submitting fictitious legal research generated by ChatGPT.
**A:** 2023.