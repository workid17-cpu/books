# 📘 Chapter 3 Flashcards: Looking Inside Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 3

---

## Part 1: Terms → Definitions

**Q:** What is a forward pass?
**A:** Inputs entering the neural network and flowing through its computations to produce an output on the other end of the computation graph.

**Q:** What is an autoregressive model?
**A:** A model that consumes its earlier predictions to make later predictions (e.g., the first generated token is used to generate the second).

**Q:** Why are text generation LLMs called autoregressive models?
**A:** Because they generate one token at a time, feeding each earlier prediction into the next.

**Q:** What differentiates text generation models from text representation models like BERT?
**A:** Text generation models are autoregressive; representation models like BERT are not (BERT is bidirectional).

**Q:** What is the language modeling head (LM head)?
**A:** A simple neural network layer that translates the output of the Transformer-block stack into probability scores for the most likely next token.

**Q:** What is a token embedding?
**A:** A vector representation associated with a token in the tokenizer's vocabulary.

**Q:** What are the three major components of a Transformer LLM?
**A:** The tokenizer, a stack of Transformer blocks, and the language modeling head.

**Q:** What is a Transformer block?
**A:** A repeated processing unit made up of an attention layer and a feedforward neural network.

**Q:** What is the feedforward neural network (MLP) responsible for?
**A:** Storing information and making predictions and interpolations from data it was trained on — memorization and generalization.

**Q:** What is the attention layer responsible for?
**A:** Incorporating relevant context information from other input tokens and positions.

**Q:** What is self-attention?
**A:** Attention that operates on the input vector at a position, pulling in relevant information from previous elements of the sequence.

**Q:** What is an attention head?
**A:** One of several parallel applications of attention within an attention layer, each with its own projection matrices.

**Q:** What is the query projection matrix used for?
**A:** Projecting inputs into queries — the space used to ask "how relevant is each previous token to me?"

**Q:** What is the key projection matrix used for?
**A:** Projecting inputs into keys — the space representing what each previous token holds.

**Q:** What is the value projection matrix used for?
**A:** Projecting inputs into values — the space holding the information to be combined.

**Q:** What are the two major steps of attention?
**A:** (1) Relevance scoring — scoring how relevant each previous token is; (2) combining information — combining value vectors weighted by those scores.

**Q:** What is relevance scoring?
**A:** Multiplying the query vector of the current position by the keys matrix to score each previous token's relevance, then normalizing with softmax.

**Q:** How does attention combine information?
**A:** Multiplying each token's value vector by its relevance score and summing the resulting vectors.

**Q:** What does softmax do in attention?
**A:** Normalizes the relevance scores so they sum up to 1.

**Q:** What is the model's context length?
**A:** The limit on how many tokens the model can process at once; equal to the number of processing streams (e.g., 4K context = 4K streams).

**Q:** What is the model dimension?
**A:** The size of the vector each token stream takes as input and produces as output (e.g., 3,072 for Phi-3-mini).

**Q:** What is the KV cache?
**A:** A cache of previous computation results (keys and values vectors in the attention mechanism) so each new token only needs the last stream computed.

**Q:** Is the KV cache enabled by default in Hugging Face Transformers?
**A:** Yes — it is enabled by default; you disable it with `use_cache=False`.

**Q:** What is a decoding strategy?
**A:** The method of choosing a single token from the probability distribution over the vocabulary.

**Q:** What is greedy decoding?
**A:** Always picking the highest-probability token; equivalent to setting the temperature to zero.

**Q:** Why is greedy decoding not always the best strategy?
**A:** Adding randomness and sampling from the distribution (e.g., a 40% token picked 40% of the time) tends to produce better outputs for most use cases.

**Q:** What is sparse attention?
**A:** Limiting the context of previous tokens the model can attend to, improving computational efficiency.

**Q:** What is sliding window attention?
**A:** A sparse-attention variant (e.g., Longformer) where attention is limited to a window of nearby previous tokens.

**Q:** How did GPT-3 use sparse attention?
**A:** It interweaved full-attention and efficient-attention blocks (alternating, e.g., blocks 1&3 full, 2&4 sparse) — not sparse everywhere, which would degrade quality.

**Q:** What is multi-query attention (MQA)?
**A:** Sharing the keys and values matrices across ALL attention heads; only the queries are unique per head.

**Q:** What is grouped-query attention (GQA)?
**A:** Sharing keys/values matrices within groups of heads — more than one shared set but fewer than one per head; used by Llama 2 and 3.

**Q:** What is the trade-off of grouped-query attention vs multi-query attention?
**A:** GQA sacrifices a little of MQA's efficiency for a large improvement in quality.

**Q:** What is Flash Attention?
**A:** A method/implementation giving significant speedups for Transformer training and inference on GPUs by optimizing what values are loaded and moved between SRAM and HBM.

**Q:** What are residual connections?
**A:** Skip connections in a Transformer block that let information and gradients flow around a sub-layer.

**Q:** What is LayerNorm?
**A:** Layer normalization, the normalization operation used in the original Transformer block.

**Q:** What is RMSNorm?
**A:** Root mean square layer normalization — simpler and more efficient than LayerNorm.

**Q:** What is SwiGLU?
**A:** A newer activation function (from "GLU Variants Improve Transformer") more common than the original ReLU in modern blocks.

**Q:** What is pre-normalization?
**A:** Performing normalization before the attention and feedforward layers; reported to reduce required training time.

**Q:** What are absolute positional embeddings?
**A:** Positional encodings that mark the first token as position 1, the second as position 2, etc.; can be static (geometric) or learned.

**Q:** What are rotary positional embeddings (RoPE)?
**A:** A method encoding positional information that captures absolute AND relative token position by rotating vectors in embedding space; applied in the attention step.

**Q:** Where are rotary embeddings applied in the forward pass?
**A:** In the attention step — mixed into the queries and keys matrices just before relevance scoring (not at the start of the forward pass).

**Q:** What is packing?
**A:** Efficiently organizing short training documents into the context — grouping multiple documents in one context while minimizing padding.

**Q:** Why do absolute positional embeddings break with packing?
**A:** If Document 50 starts at position 50 and is labeled token 50, the model wrongly assumes previous context belonging to an unrelated document.

**Q:** What can a decoder Transformer block attend to?
**A:** Only previous tokens (autoregressive); BERT can attend to both sides (bidirectional).

**Q:** What are other kinds of Transformer heads besides the LM head?
**A:** Sequence classification heads and token classification heads.

**Q:** Why do LLM APIs stream output tokens?
**A:** Because even ~4 seconds is too long to wait for a full response; streaming shows tokens as generated.

**Q:** Why did the email example stop abruptly?
**A:** It reached the token limit set by `max_new_tokens=50`.

**Q:** What is a "head" in a Transformer?
**A:** A task-specific neural network layer attached to a stack of Transformer blocks (e.g., LM head, sequence classification head, token classification head).

**Q:** What did the manual forward pass on "The capital of France is" produce?
**A:** The most probable next token decoded to "Paris".

**Q:** What shapes did the manual forward pass produce?
**A:** `model_output[0].shape` = `torch.Size([1, 6, 3072])`; `lm_head_output.shape` = `torch.Size([1, 6, 32064])`.

---

## Part 2: Short Answer

**Q:** Describe how a Transformer LLM generates text step by step.
**A:** (1) Tokenize the prompt into input IDs; (2) run a forward pass through the Transformer-block stack then the LM head → probability distribution; (3) pick a token via a decoding strategy; (4) append the token to the prompt; (5) repeat until completion or the token limit.

**Q:** Why must previous token streams still be computed if only the last stream feeds the LM head?
**A:** Because the attention mechanism inside each Transformer block uses the earlier streams' intermediate outputs to compute the final stream.

**Q:** What does `lm_head_output[0,-1]` give you, and how do you get the generated token text?
**A:** It gives the probability scores for the last token in the sequence (index −1 of batch 0); `.argmax(-1)` gives the top token ID, and `tokenizer.decode` renders it as text.

**Q:** Explain the KV cache timing results in the chapter.
**A:** Generating 100 tokens on a Colab T4 GPU took ~4.5 seconds with cache vs ~21.8 seconds without — a dramatic speedup from not recomputing previous streams.

**Q:** Why does the feedforward network make "The Shawshank" → "Redemption" work?
**A:** Training on a massive text archive (containing many mentions of "The Shawshank Redemption") let the model memorize that association; feedforward layers store this information and can also interpolate/generalize.

**Q:** Why does GPT-4 respond to "The Shawshank" with a descriptive answer rather than just "Redemption"?
**A:** Because chat LLMs are trained on instruction-tuning and human preference/feedback fine-tuning; raw language models like GPT-3 are difficult to use directly.

**Q:** Explain the "dog chased the squirrel because it" example.
**A:** To predict what follows "it," the model must resolve what "it" refers to; attention incorporates context (and learned patterns from training data) into the representation of "it" to make that determination.

**Q:** Walk through the full attention calculation for one head.
**A:** (1) Multiply layer inputs by the query/key/value projection matrices to produce Q, K, V matrices; (2) relevance scoring: multiply the current position's query vector by the keys matrix and softmax-normalize; (3) combine: multiply each value vector by its relevance score and sum the results.

**Q:** How does multi-head attention improve on single-head attention?
**A:** Running several attention operations in parallel (each its own head) increases the model's capacity to attend to different patterns at once.

**Q:** Compare multi-head, multi-query, and grouped-query attention.
**A:** Multi-head: every head has its own Q/K/V. Multi-query: all heads share one K/V set (only queries differ). Grouped-query: heads are grouped, each group sharing one K/V set — a middle ground (Llama 2/3).

**Q:** Why is the attention layer the most computationally expensive part, and how is it optimized?
**A:** It's the most expensive component; optimizations include sparse/sliding-window attention, K/V sharing (MQA, GQA), and Flash Attention (SRAM↔HBM movement optimization).

**Q:** What does Flash Attention optimize, exactly?
**A:** What values are loaded and moved between a GPU's shared memory (SRAM) and high bandwidth memory (HBM) — it computes exact attention faster via IO-awareness.

**Q:** What tweaks does a 2024-era Transformer block (e.g., Llama 3) make vs the original?
**A:** Pre-normalization (norm before attention/feedforward), RMSNorm instead of LayerNorm, SwiGLU instead of ReLU, grouped-query attention, and rotary embeddings (RoPE).

**Q:** Explain the problem with absolute positional embeddings under packing.
**A:** With documents packed into one context, telling the model a packed document's first token is "position 50" misinforms it into assuming prior context that actually belongs to an unrelated document.

**Q:** Where exactly are rotary embeddings applied?
**A:** During the attention step, mixed into the queries and keys matrices just before they are multiplied for relevance scoring — not added at the start of the forward pass.

**Q:** What does the attention figure color coding mean?
**A:** The dark blue cell is the token being processed; the light blue cells are the previous tokens the attention mechanism allows it to attend to.

**Q:** What domains beyond language are Transformers being adapted to?
**A:** Computer vision, robotics, and time series (e.g., vision transformers, Open X-Embodiment/RT-X, time-series surveys).

**Q:** Why is packing needed during training?
**A:** Many training documents are much shorter than the context (e.g., a 10-word sentence vs a 4K context); packing groups multiple documents per context while minimizing padding, for training efficiency.

**Q:** What is the difference between greedy decoding and sampling?
**A:** Greedy always picks the highest-probability token (temperature 0); sampling picks tokens with probability equal to their scores (a 40% token is picked 40% of the time).

**Q:** What does `print(model)` reveal for Phi-3-mini?
**A:** The nested architecture: `embed_tokens` (Embedding 32,064×3,072), 32 `Phi3DecoderLayer` blocks (each with self-attention, MLP, RMSNorm layers), and `lm_head` (Linear 3,072→32,064).

---

## Part 3: Fill-in-the-Blank

**Q:** A Transformer LLM generates ______ at a time.
**A:** One token.

**Q:** Each token generation step is one ______ through the model.
**A:** Forward pass.

**Q:** Models that consume their earlier predictions to make later predictions are called ______ models.
**A:** Autoregressive.

**Q:** Text representation models like BERT are ______ autoregressive.
**A:** Not.

**Q:** The three major components of a Transformer LLM are the tokenizer, a stack of Transformer blocks, and the ______.
**A:** Language modeling head (LM head).

**Q:** Phi-3-mini's embeddings matrix is Embedding(32064, ______).
**A:** 3072.

**Q:** Phi-3-mini has ______ Transformer decoder layers.
**A:** 32.

**Q:** The LM head outputs a vector with ______ scores (one per vocabulary token).
**A:** 32,064.

**Q:** The output of the model is a ______ for each token in the vocabulary.
**A:** Probability score.

**Q:** Picking the highest-scoring token every time is called ______ decoding.
**A:** Greedy.

**Q:** Greedy decoding is what happens if you set the ______ parameter to zero.
**A:** Temperature (covered in Chapter 6).

**Q:** For "The capital of France is", the model's top token decoded to ______.
**A:** Paris.

**Q:** `model_output[0].shape` was torch.Size([1, 6, ______]).
**A:** 3072.

**Q:** `lm_head_output.shape` was torch.Size([1, 6, ______]).
**A:** 32064.

**Q:** The limit on how many tokens a model can process at once is its ______ length.
**A:** Context.

**Q:** A model with 4K context length has ______ processing streams.
**A:** 4K (4,096).

**Q:** Only the output of the ______ stream is used to predict the next token.
**A:** Last.

**Q:** The optimization that caches keys and values to speed up generation is called the ______ cache.
**A:** KV (keys and values).

**Q:** Generating 100 tokens on a T4 GPU took ~______ seconds with cache and ~______ seconds without.
**A:** 4.5; 21.8.

**Q:** In Hugging Face Transformers, cache is enabled by ______.
**A:** Default.

**Q:** To disable the cache you set ______ to False.
**A:** use_cache.

**Q:** A Transformer block is made up of an attention layer and a ______ neural network.
**A:** Feedforward.

**Q:** The feedforward network houses the majority of the model's ______ capacity.
**A:** Processing.

**Q:** "The Shawshank" → most probable next word ______ (1994 film).
**A:** Redemption.

**Q:** Attention has two major steps: scoring ______ and combining ______.
**A:** Relevance; information.

**Q:** Each parallel application of attention within a layer is called an attention ______.
**A:** Head.

**Q:** The three projection matrices are the query, ______, and value matrices.
**A:** Key.

**Q:** Relevance scores are normalized by a ______ operation so they sum to 1.
**A:** Softmax.

**Q:** The "it" in "Sarah fed the cat because it" should represent the ______.
**A:** Cat.

**Q:** GPT-3 ______ full-attention and efficient-attention blocks.
**A:** Interweaved (alternated).

**Q:** ______ attention shares keys and values matrices between all heads.
**A:** Multi-query.

**Q:** ______ attention is used by models like Llama 2 and 3.
**A:** Grouped-query.

**Q:** Flash Attention optimizes movement between a GPU's ______ (SRAM) and ______ (HBM).
**A:** Shared memory; high bandwidth memory.

**Q:** ______ is a simpler, more efficient normalization than LayerNorm.
**A:** RMSNorm.

**Q:** Modern blocks use the ______ activation instead of ReLU.
**A:** SwiGLU.

**Q:** Normalization happening prior to attention/feedforward is called ______-normalization.
**A:** Pre.

**Q:** The original Transformer's absolute embeddings marked the first token as position ______.
**A:** 1.

**Q:** Rotary embeddings are applied in the ______ step, not the start of the forward pass.
**A:** Attention.

**Q:** RoPE captures ______ and ______ token position information.
**A:** Absolute; relative.

**Q:** The process of grouping multiple short documents in one context while minimizing padding is called ______.
**A:** Packing.

**Q:** BERT's B stands for ______.
**A:** Bidirectional.

**Q:** Besides the LM head, other Transformer heads include sequence classification heads and ______ heads.
**A:** Token classification.

**Q:** The next chapter (Chapter 4) starts with ______, a common task in Language AI.
**A:** Text classification.
