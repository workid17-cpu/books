# 📘 Practice Exam — Chapter 3: Looking Inside Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 3
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which statement best describes how a Transformer LLM generates text?
   a) It generates the entire response in a single operation
   b) It generates one token per forward pass, appending each output token to the prompt
   c) It generates tokens in reverse order to improve coherence
   d) It generates text by copying from the training dataset

2. Each token generation step is one ____ through the model.
   a) backward pass
   b) tokenizer call
   c) forward pass
   d) caching step

3. Models that consume their earlier predictions to make later predictions are called:
   a) recurrent models
   b) discriminative models
   c) encoder-only models
   d) autoregressive models

4. Which model is NOT autoregressive, per the chapter?
   a) A text generation LLM
   b) GPT-style generators
   c) BERT
   d) Llama 2

5. The three major components of a Transformer LLM are:
   a) embeddings, attention, and feedforward
   b) tokenizer, stack of Transformer blocks, and language modeling head
   c) encoder, decoder, and LM head
   d) vocabulary, embeddings matrix, and softmax

6. What does the LM head do?
   a) It tokenizes the prompt into token IDs
   b) It caches keys and values for faster generation
   c) It rotates positional embeddings
   d) It translates the output of the block stack into probability scores for the next token

7. Phi-3-mini's `embed_tokens` matrix has dimensions:
   a) 32064 × 3072
   b) 3072 × 32064
   c) 32064 × 32064
   d) 3072 × 9216

8. How many `Phi3DecoderLayer` blocks does Phi-3-mini contain?
   a) 6
   b) 50
   c) 32
   d) 100

9. Each Transformer block contains which two components?
   a) a tokenizer and an LM head
   b) a decoder and an encoder
   c) an attention layer and a feedforward neural network (MLP)
   d) a KV cache and a softmax layer

10. The output of the LM head is:
    a) a single token ID
    b) a probability score for every token in the vocabulary
    c) the decoded text of the next token
    d) a tensor of shape [1, 6, 3072]

11. The method of choosing a single token from the probability distribution is called the:
    a) sampling rate
    b) temperature schedule
    c) tokenization strategy
    d) decoding strategy

12. Always picking the highest-probability token is called:
    a) temperature sampling
    b) beam search
    c) greedy decoding
    d) nucleus sampling

13. Greedy decoding is equivalent to setting which parameter to zero?
    a) temperature
    b) top-k
    c) max_new_tokens
    d) do_sample

14. In the sampling example, if the token "Dear" has a 40% probability, it will be picked:
    a) 100% of the time because it is the highest
    b) 40% of the time, with other tokens picked according to their scores
    c) never, because sampling avoids the top token
    d) only when the temperature is zero

15. For the prompt "The capital of France is", the decoded top token was:
    a) London
    b) France
    c) Berlin
    d) Paris

16. The shape of `model_output[0]` in the manual forward pass was:
    a) torch.Size([1, 6, 32064])
    b) torch.Size([1, 32, 3072])
    c) torch.Size([1, 6, 3072])
    d) torch.Size([6, 1, 3072])

17. The shape of `lm_head_output` was:
    a) torch.Size([1, 6, 32064])
    b) torch.Size([1, 6, 3072])
    c) torch.Size([32064, 6])
    d) torch.Size([6, 32064])

18. The limit on how many tokens a model can process at once is its:
    a) hidden size
    b) context length
    c) vocabulary size
    d) batch size

19. A model with a 4K context length has how many processing streams?
    a) 4,096
    b) 1,024
    c) 32064
    d) 32

20. For text generation, which stream's output feeds the LM head?
    a) the first stream
    b) the middle stream
    c) every stream's average
    d) the last stream

21. Why must the previous token streams still be computed even though only the last stream's output is used?
    a) the attention mechanism in each Transformer block uses their earlier outputs
    b) the KV cache requires them for storage
    c) the tokenizer needs them to decode
    d) the LM head needs all outputs to normalize scores

22. The optimization that caches keys and values to speed up generation is called:
    a) the context cache
    b) the KV cache
    c) the head cache
    d) the token cache

23. On a Colab T4 GPU, generating 100 tokens took approximately how long with the KV cache enabled?
    a) 21.8 seconds
    b) 1.5 seconds
    c) 4.5 seconds
    d) 0.45 seconds

24. How long did the same 100-token generation take with the cache disabled?
    a) 4.5 seconds
    b) 10.0 seconds
    c) 40 seconds
    d) 21.8 seconds

25. In Hugging Face Transformers, the KV cache is:
    a) enabled by default
    b) disabled by default
    c) only available on CUDA
    d) an experimental feature

26. The feedforward neural network in the Transformer blocks is primarily responsible for:
    a) incorporating context from other tokens
    b) storing information and making predictions/interpolations from training data
    c) normalizing the hidden states
    d) generating the positional embeddings

27. Passing "The Shawshank" to the model was expected to generate which most probable next word?
    a) Redemption
    b) Movie
    c) 1994
    d) Stephen

28. Why does a modern chat LLM like GPT-4 answer "The Shawshank" with a full descriptive response instead of just the raw next-token completion?
    a) It has a larger vocabulary
    b) It uses a KV cache
    c) It is an encoder model
    d) It is trained on instruction-tuning and human preference/feedback fine-tuning

29. In the example "The dog chased the squirrel because it", the attention mechanism's job is to determine:
    a) whether "it" refers to the dog or the squirrel
    b) the next verb after "it"
    c) the tense of "chased"
    d) the subject of the sentence

30. The two major steps of attention are:
    a) encoding and decoding
    b) scoring relevance and combining information
    c) querying and updating
    d) tokenizing and embedding

31. Relevance scoring is done by multiplying the current position's query vector by the:
    a) value matrix
    b) softmax output
    c) keys matrix
    d) embeddings matrix

32. The scores in the relevance scoring step are normalized by a softmax so they:
    a) become integers
    b) become equal to each other
    c) range from 0 to 100
    d) sum up to 1

33. After relevance scoring, attention combines information by:
    a) multiplying each value vector by its relevance score and summing the results
    b) averaging all key vectors
    c) multiplying the query by the value matrix
    d) concatenating all previous output vectors

34. Multiple parallel applications of attention within a layer are called:
    a) layers
    b) attention heads
    c) streams
    d) blocks

35. Which attention variant shares the keys and values matrices across ALL heads, leaving only queries per head?
    a) multi-head attention
    b) grouped-query attention
    c) multi-query attention
    d) sparse attention

36. Which attention variant is used by models like Llama 2 and 3?
    a) multi-head attention
    b) multi-query attention
    c) sparse attention
    d) grouped-query attention

37. How did GPT-3 use sparse/efficient attention?
    a) it interweaved full-attention and efficient-attention blocks
    b) it used sparse attention in all blocks
    c) it replaced attention with feedforward layers
    d) it did not use attention at all

38. Flash Attention speeds up the attention calculation by optimizing:
    a) the number of attention heads
    b) what values are loaded and moved between a GPU's SRAM and HBM
    c) the vocabulary size
    d) the softmax function

39. Which of the following is a tweak in a 2024-era Transformer block like Llama 3?
    a) absolute positional embeddings only
    b) ReLU activation
    c) pre-normalization with RMSNorm
    d) LayerNorm after attention

40. Rotary positional embeddings (RoPE) are applied:
    a) at the start of the forward pass like absolute embeddings
    b) only to the value matrix
    c) in the LM head
    d) in the attention step, mixed into the queries and keys just before relevance scoring

---

## Section B: True/False (1 point each)

41. A Transformer LLM generates the entire response text all at once in a single operation. (T/F)
42. Text representation models like BERT are autoregressive. (T/F)
43. The tokenizer is one of the three major components of a Transformer LLM. (T/F)
44. The LM head outputs a probability score for each token in the vocabulary. (T/F)
45. Only the last token stream's output is used to predict the next token. (T/F)
46. The number of processing streams equals the model's context length. (T/F)
47. Greedy decoding is equivalent to setting the temperature to zero. (T/F)
48. The KV cache is disabled by default in Hugging Face Transformers. (T/F)
49. Using the KV cache changes the output of generation. (T/F)
50. The feedforward neural network makes the model work as a simple lookup database. (T/F)
51. Sparse attention limits the context of previous tokens that the model can attend to. (T/F)
52. In multi-query attention, every head has its own distinct keys and values matrices. (T/F)
53. Grouped-query attention sacrifices a little efficiency for a large improvement in quality. (T/F)
54. Rotary embeddings are added at the beginning of the forward pass. (T/F)
55. With document packing, labeling a packed document's first token as "position 50" can mislead the model. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Describe the autoregressive generation loop of a Transformer LLM, including what happens to the prompt between steps.
57. Walk through the manual forward-pass code (tokenize → `model.model` → `lm_head`) and explain the shapes `[1, 6, 3072]` and `[1, 6, 32064]`.
58. What is the KV cache, why does it speed up generation, and what numbers does the chapter report?
59. Explain the two steps of attention (relevance scoring and combining information) and which matrices/operations are involved in each.
60. Compare multi-head, multi-query, and grouped-query attention. Which is used by Llama 2/3 and why?
61. What is Flash Attention, and what hardware resource does it optimize?
62. What are the differences between the original Transformer block and a 2024-era block (pre-normalization, RMSNorm, SwiGLU)?
63. Explain why absolute positional embeddings are problematic under document packing, and how rotary embeddings (RoPE) solve it.
64. Why is the feedforward network said to do "memorization and interpolation", and how does that relate to generalization?
65. In the attention example "Sarah fed the cat because it", what should "it" represent, and how does attention achieve this?

---

## Section D: Essay / Applied (5 points each)

66. **The Transformer LLM as a system.** Explain the full picture of a Transformer LLM: the three components (tokenizer, block stack, LM head), how text is generated one token at a time in an autoregressive loop, what the LM head produces, and how decoding strategies (greedy vs sampling) choose the actual token. Use the Phi-3-mini example (shapes, "Paris") to illustrate.
67. **Parallelism, context, and the KV cache.** Describe how Transformers process tokens in parallel (streams equal to context length), why only the last stream's output is used yet all streams must be computed, and how the KV cache avoids recomputation. Include the timing experiment results and why LLM APIs stream their output.
68. **Attention from the ground up.** Explain why attention matters for language (the "dog chased the squirrel" example), the projection matrices (query/key/value), the two attention steps (relevance scoring with softmax; combining information by weighted sum), and how multi-head attention increases capacity.
69. **The evolution of efficient attention.** Trace the improvements to the attention layer: sparse/sliding-window attention (and how GPT-3 interleaves it with full attention), multi-query attention, grouped-query attention, and Flash Attention. Explain the trade-offs at each step (quality vs efficiency; memory movement).
70. **The modern Transformer block and positional information.** Describe the tweaks in a 2024-era block (pre-normalization, RMSNorm, SwiGLU, GQA, RoPE). Then explain positional embeddings in depth: absolute vs rotary, the document-packing problem, and where RoPE is applied in the forward pass.

---

## ANSWER KEY

### Section A: Multiple Choice
1. b
2. c
3. d
4. c
5. b
6. d
7. a
8. c
9. c
10. b
11. d
12. c
13. a
14. b
15. d
16. c
17. a
18. b
19. a
20. d
21. a
22. b
23. c
24. d
25. a
26. b
27. a
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
39. c
40. d

### Section B: True/False
41. **F** — It generates one token at a time, one forward pass per token.
42. **F** — BERT is NOT autoregressive; generation LLMs are.
43. **T** — Tokenizer, stack of Transformer blocks, LM head.
44. **T** — The LM head outputs a probability score for every token in the vocabulary.
45. **T** — Only the last stream's output feeds the LM head.
46. **T** — Streams = context length (e.g., 4K context = 4K streams).
47. **T** — Greedy decoding = always highest score = temperature 0.
48. **F** — It is enabled by default; you disable with `use_cache=False`.
49. **F** — It only skips recomputation of previous streams; results are unchanged.
50. **F** — It is not simply a database; it also interpolates and generalizes.
51. **T** — Sparse attention limits attended context (local/window attention).
52. **F** — Multi-query attention SHARES K/V across all heads; only queries differ per head.
53. **T** — GQA trades a little MQA efficiency for a large quality gain.
54. **F** — Rotary embeddings are applied in the attention step, not at the start of the forward pass.
55. **T** — The model would wrongly assume previous context from an unrelated document.

### Section C: Short Answer (model answers)
56. **Autoregressive loop.** Tokenize the prompt → forward pass through the block stack and LM head → probability distribution → decoding strategy picks one token → append that token to the prompt → run the updated prompt through the model again → repeat until completion or the token limit (e.g., `max_new_tokens=50`).
57. **Manual forward pass.** `model.model(input_ids)` runs the block stack → `[1, 6, 3072]` (batch 1, six tokens, 3,072-dim vectors); `model.lm_head(model_output[0])` → `[1, 6, 32064]` (probability scores for all 32,064 vocabulary tokens per position); `lm_head_output[0,-1].argmax(-1)` + decode → "Paris".
58. **KV cache.** Caches previous streams' keys and values so each new token only computes the last stream; on a T4, 100 tokens took ~4.5 s with cache vs ~21.8 s without. On by default; `use_cache=False` disables it.
59. **Attention steps.** (1) Relevance scoring: current position's query × keys matrix → score per previous token → softmax so scores sum to 1. (2) Combining: each value vector × its score, summed → attention output.
60. **Attention variants.** Multi-head: each head own Q/K/V. Multi-query: one shared K/V set across all heads. Grouped-query: heads grouped, each group sharing K/V (fewer than heads, more than one). Llama 2/3 use GQA — a little less efficient than MQA but much higher quality.
61. **Flash Attention.** A method/implementation that speeds up attention on GPUs by optimizing what values are loaded/moved between SRAM and HBM (IO-aware exact attention).
62. **Block tweaks.** Original: LayerNorm (after sublayers), ReLU. Modern: pre-normalization (norm before attention/feedforward), RMSNorm (simpler/faster), SwiGLU activation, grouped-query attention, rotary embeddings.
63. **RoPE vs absolute.** Absolute embeddings mark positions 1,2,3... and are added at the start of the forward pass; under packing, labeling a packed document's first token as "position 50" misleads the model into assuming unrelated previous context. RoPE encodes absolute AND relative position by rotating vectors in embedding space and is applied in the attention step to the queries and keys before relevance scoring.
64. **Memorization + interpolation.** Feedforward layers store learned facts/associations from the training archive (e.g., "The Shawshank" → "Redemption") but also interpolate between data points and complex patterns, enabling generalization to inputs not in the training set.
65. **"it" example.** "it" should represent the cat — attention bakes "cat information" into the representation of "it" based on patterns learned during training.

### Section D: Essay (grading notes)
66. **Expect** the three components; token-by-token generation; prompt-updating loop; LM head produces a full vocabulary probability distribution; greedy vs sampling (temperature 0 vs 40%-probability example); the "The capital of France is" → "Paris" illustration with shapes.
67. **Expect** per-token streams; context length = stream count; why previous streams must still be computed (attention uses earlier outputs); KV cache skipping recomputation; 4.5 s vs 21.8 s; streaming motivation.
68. **Expect** "The dog chased the squirrel because it" pronoun resolution; Q/K/V projection matrices produced by training; two steps (query×keys softmax scoring; values weighted-sum); multi-head parallel capacity (splitting then combining).
69. **Expect** sparse/sliding-window (Longformer); GPT-3 alternating full and sparse blocks to preserve quality; MQA sharing one K/V; GQA grouping (Llama 2/3) balancing efficiency vs quality; Flash Attention SRAM↔HBM IO-optimization.
70. **Expect** pre-normalization (training time), RMSNorm vs LayerNorm, SwiGLU vs ReLU, GQA, RoPE; absolute vs rotary positional embeddings; the packing problem (4K context, short documents, "position 50" trap); RoPE applied to queries/keys just before relevance scoring.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely attention mechanics or positional embeddings).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
