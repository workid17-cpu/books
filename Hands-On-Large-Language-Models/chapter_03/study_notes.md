# 📘 Chapter 3 Study Bundle: Looking Inside Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 3

---

## §1. Study Notes

### Core Theme
This chapter opens the hood on Transformer-based LLMs, focusing on text generation models. It covers the high-level picture (one-token-at-a-time, autoregressive generation), the three main components (tokenizer, stack of Transformer blocks, LM head), the forward pass and decoding strategies, parallel token processing and context size, the KV cache for speeding up generation, the two components inside each Transformer block (feedforward network and attention layer), a deep dive into how attention is calculated (queries/keys/values, relevance scoring, combining information), and the recent architectural improvements: sparse/local attention, multi-query and grouped-query attention, Flash Attention, pre-normalization + RMSNorm + SwiGLU, and rotary positional embeddings (RoPE).

### Key Definitions
- **Forward pass**: Machine-learning speak for inputs entering the neural network and flowing through the computations to produce an output on the other end of the computation graph.
- **Autoregressive model**: A model that consumes its earlier predictions to make later predictions (e.g., the first generated token is used to generate the second). Text generation LLMs are autoregressive; representation models like BERT are not.
- **Language modeling head (LM head)**: A simple neural network layer that translates the output of the Transformer-block stack into probability scores for what the most likely next token is.
- **Token embedding**: A vector representation associated with each token in the tokenizer's vocabulary.
- **Transformer block**: A repeated processing unit in the model made up of an attention layer and a feedforward neural network; models have from ~6 (original Transformer paper) to over a hundred (many large LLMs) blocks.
- **Feedforward neural network (MLP)**: A Transformer-block component that houses the majority of the model's processing capacity — memorization and interpolation from training data.
- **Attention layer**: A Transformer-block component mainly concerned with incorporating relevant information from other input tokens and positions (context).
- **Self-attention**: Attention that operates on the input vector at a position, pulling in relevant information from previous elements of the sequence.
- **Attention head**: One of several parallel applications of attention executed within an attention layer; each head has its own projection matrices.
- **Query projection matrix**: A training-produced matrix that projects inputs into queries (the space used to ask "how relevant is each previous token to me?").
- **Key projection matrix**: A training-produced matrix that projects inputs into keys (the space used to represent what each previous token holds).
- **Value projection matrix**: A training-produced matrix that projects inputs into values (the space holding the information to be combined).
- **Relevance scoring**: The first of attention's two major steps — multiplying the query vector of the current position with the keys matrix to score how relevant each previous token is, then normalizing with softmax.
- **Combining information**: The second major step — multiplying each token's value vector by its relevance score and summing the results to produce the attention output.
- **Softmax**: An operation that normalizes scores so they sum up to 1.
- **Context length**: The limit on how many tokens a model can process at once; a model with 4K context length can only process 4K tokens (4K processing streams).
- **Model dimension (hidden size)**: The size of the vector each token stream takes as input and produces as output; each stream's output vector has the same size as its input.
- **KV cache (keys and values cache)**: An optimization that caches previous computation results (specific vectors in the attention mechanism) so generating each new token only computes the last stream, giving a significant speedup.
- **Decoding strategy**: The method of choosing a single token from the probability distribution over the vocabulary.
- **Greedy decoding**: Choosing the highest-probability token every time; equivalent to setting temperature to zero (temperature covered in Chapter 6).
- **Sparse attention**: Limiting the context of previous tokens the model can attend to, improving efficiency (e.g., "Generating long sequences with sparse transformers").
- **Sliding window attention**: A sparse-attention variant (e.g., Longformer) where attention is limited to a window of nearby previous tokens.
- **Multi-query attention**: Sharing the keys and values matrices across all attention heads so only the queries differ per head.
- **Grouped-query attention (GQA)**: Sharing keys/values matrices within groups of heads — more than one shared set, but fewer than one per head; a middle ground between multi-head and multi-query attention. Used by Llama 2 and 3.
- **Flash Attention**: A popular method/implementation providing significant speedups for both training and inference on GPUs by optimizing what values are loaded and moved between a GPU's shared memory (SRAM) and high bandwidth memory (HBM).
- **Residual connections**: Skip connections in a Transformer block that let gradients and information flow around a sub-layer.
- **Layer normalization (LayerNorm)**: The normalization operation used in the original Transformer block.
- **RMSNorm**: Root mean square layer normalization — a simpler, more efficient normalization than LayerNorm used in newer variants.
- **SwiGLU**: A newer activation function (from "GLU Variants Improve Transformer") that has become more common than the original ReLU.
- **Pre-normalization**: Performing normalization before the attention and feedforward layers (reported to reduce required training time).
- **Absolute positional embeddings**: Positional encodings that mark the first token as position 1, the second as position 2, etc.; can be static (geometric functions) or learned.
- **Rotary positional embeddings (RoPE)**: A method that encodes positional information capturing absolute and relative token position by rotating vectors in their embedding space; applied in the attention step rather than at the start of the forward pass.
- **Packing**: Efficiently organizing short training documents into the context — grouping multiple documents in a single context while minimizing padding at the end.
- **Autoregressive decoding constraint**: Decoder Transformer blocks can only attend to previous tokens; BERT can attend to both sides (the "B" in BERT = bidirectional).

### Core Concepts & Frameworks
- **Text-in, text-out, one token at a time**: A trained Transformer LLM is best understood as software that takes text and generates text — but it does not generate the whole response at once; each token generation step is one forward pass, and after each step the output token is appended to the prompt, which is presented to the model again for the next forward pass.
- **Autoregressive loop**: Software around the network runs the model in a loop to sequentially expand the generated text until completion; the model is simply predicting the next token based on an input prompt.
- **Three components of a Transformer LLM**: (1) tokenizer (breaks text into token IDs; holds the vocabulary), (2) a stack of Transformer blocks (does all the processing; model has an embedding vector per vocabulary token), (3) the LM head (outputs probability scores for the most likely next token).
- **The forward pass flow**: For each generated token, computation flows once through each Transformer block in order, then to the LM head, which outputs the probability distribution for the next token. Phi-3-mini example: `model.model(input_ids)` → `[1, 6, 3072]`; `model.lm_head(...)` → `[1, 6, 32064]`; `lm_head_output[0,-1].argmax(-1)` → token ID → decode → "Paris".
- **Other Transformer heads**: The LM head is one of multiple possible "heads" attached to a stack of blocks; other kinds include sequence classification heads and token classification heads.
- **Sampling over greedy**: Greedy decoding (always highest probability) doesn't tend to lead to the best outputs; sampling from the probability distribution (e.g., "Dear" at 40% has a 40% chance of being picked) gives other tokens a chance. Greedy = temperature 0 (see Ch 6).
- **Parallel token processing**: Transformers lend themselves better to parallel computing than previous architectures. Each input token flows through its own computation path/stream; the number of streams equals the context length. Only the last stream's output feeds the LM head, but the earlier streams' computations are still required because the attention mechanism in each Transformer block uses earlier outputs.
- **Context length limits streams**: A 4K-context model has 4K streams and processes up to 4K tokens at once.
- **KV cache mechanics**: When generating the second token, appending the output token and re-passing would repeat all previous computations; caching previous results (especially keys/values vectors) means only the last stream needs computing. Cache is on by default in Hugging Face Transformers; disable via `use_cache=False`. On a T4 GPU, generating 100 tokens took 4.5 seconds with cache vs 21.8 seconds without. LLM APIs stream output tokens rather than making users wait for full generation.
- **Feedforward = memorization + interpolation**: The feedforward networks (collectively across layers) store the information and behaviors learned from training data (e.g., "The Shawshank" → "Redemption"), but the model is not simply a database — it also interpolates between data points and complex patterns to generalize to unseen inputs.
- **Why chat models differ from raw LMs**: Raw LMs (like GPT-3) are difficult for people to use; they are trained on instruction-tuning and human preference/feedback fine-tuning to match expectations.
- **Attention = incorporating context**: Attention helps the model incorporate context when processing a specific token. Example: "The dog chased the squirrel because it" — attention determines whether "it" refers to the dog or the squirrel, adding context information into the representation of the "it" token based on patterns learned from training data. (Pre-neural n-gram approaches struggled here — see Jurafsky & Martin, Ch 3.)
- **Two steps of attention**: (1) score relevance of each previous token (query of current position × keys matrix, normalized by softmax); (2) combine information (multiply each value vector by its relevance score and sum).
- **Multi-head attention**: Attention is duplicated and executed multiple times in parallel; each parallel application is an attention head, increasing the model's capacity to attend to different types of patterns. Inputs are split into heads and head outputs are later combined.
- **Attention computation setup**: The attention layer processes a single position; inputs are the vector of the current token plus vectors of previous tokens; training produces query, key, and value projection matrices; multiplying inputs by these projections yields the queries, keys, and values matrices.
- **Efficient attention evolution**: (1) Local/sparse attention (GPT-3 interweaves full- and efficient-attention blocks — alternating, e.g., blocks 1&3 full, 2&4 sparse — because using sparse everywhere would vastly degrade quality); (2) multi-query attention (share K/V across all heads; only queries unique per head); (3) grouped-query attention (share K/V within groups — fewer than one per head but more than one total; sacrifices a little efficiency vs MQA for a large quality gain; used by Llama 2/3); (4) Flash Attention (IO-aware: optimizes SRAM↔HBM movement; papers: "FlashAttention: Fast and memory-efficient exact attention with IO-awareness" and "FlashAttention-2").
- **Modern Transformer block tweaks (2024-era, e.g., Llama 3)**: pre-normalization, RMSNorm (simpler/faster than LayerNorm), SwiGLU activation (vs ReLU), grouped-query attention, rotary embeddings.
- **Rotary positional embeddings (RoPE)**: Positional embeddings track token order, indispensable for language. Absolute embeddings (mark first token = position 1, etc.; static geometric or learned) cause challenges when scaling (e.g., document packing — telling the model the first token of Document 50 is "number 50" misleads it into assuming previous context that belongs to an unrelated document). RoPE encodes positional info capturing absolute AND relative position by rotating vectors in embedding space; applied in the attention step, mixed into the queries and keys matrices just before relevance scoring.
- **Document packing**: Short documents are packed together into each context during training (e.g., a 4K context shouldn't be wasted on a 10-word sentence); minimizes padding. (Refs: "Efficient sequence packing without cross-contamination"; "Introducing packed BERT for 2X training speed-up".)
- **Other Transformer directions**: Architecture research extends to computer vision ("Transformers in vision: A survey", "A survey on vision transformer"), robotics ("Open X-Embodiment"), and time series ("Transformers in time series: A survey").

### Important Numbers / Stats / Tokens
- Phi-3-mini-4k-instruct pipeline: `return_full_text=False`, `max_new_tokens=50`, `do_sample=False` (p.1).
- Phi-3 model structure: `embed_tokens`: Embedding(32064, 3072, padding_idx=32000); 32 decoder layers (`32 x Phi3DecoderLayer`); `lm_head`: Linear(3072 → 32064, no bias); qkv_proj 3072→9216; gate_up_proj 3072→16384; down_proj 8192→3072 (p.6).
- Vocabulary of the example model: 32,064 tokens; embedding vector size 3,072 (p.6-7).
- `lm_head_output` shape `[1, 6, 32064]`; `model_output[0].shape` = `torch.Size([1, 6, 3072])` (p.8, 11).
- "The capital of France is" → top token → "Paris" (p.9).
- Example context length: 4K (4,096) → 4K streams (p.9).
- KV cache timing on Colab T4: 100 tokens = 4.5 s with cache; 21.8 s without (p.13).
- Transformer blocks: ~6 in the original Transformer paper to over a hundred in many large LLMs (p.13).
- "The Shawshank" → most probable next word "Redemption" (1994 film) (p.15).
- "The dog chased the squirrel because it" attention example (p.16); "Sarah fed the cat because it" → "it" represents the cat (p.19).
- GPT-3 interweaves full-attention and efficient-attention blocks (alternating) (p.24).
- GQA used by Llama 2 and 3 (p.26).
- Packing: allocating a whole 4K context to a short 10-word sentence is inefficient (p.30-31).
- "Dear" 40% probability example: 40% chance of being picked under sampling (p.7).

### Algorithms & Formulæ
- **Text generation loop**:
  1. Tokenize prompt → `input_ids`.
  2. Run a forward pass through the model (stack of Transformer blocks, then LM head) → probability distribution over the vocabulary.
  3. Choose one token via a decoding strategy (sampling vs greedy).
  4. Append the output token to the prompt.
  5. Repeat until completion (or token limit).
- **Manual forward-pass inspection (code)**:
  1. `input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to("cuda")`.
  2. `model_output = model.model(input_ids)` → `[batch, seq_len, hidden]`.
  3. `lm_head_output = model.lm_head(model_output[0])` → `[batch, seq_len, vocab]`.
  4. `token_id = lm_head_output[0,-1].argmax(-1)`; `tokenizer.decode(token_id)`.
- **Attention calculation (single head)**:
  1. Multiply layer inputs by projection matrices → queries, keys, values matrices.
  2. Relevance scoring: multiply the query vector of the current position by the keys matrix → relevance score per previous token; pass through softmax to normalize (scores sum to 1).
  3. Combining: multiply each token's value vector by its relevance score; sum the resulting vectors → attention output for this position.
- **KV cache**:
  - Forward pass for token 2 with cache: only the last stream is computed; previous results (keys/values) are reused from cache.
  - Enabled by default; `use_cache=False` disables; measured 4.5 s vs 21.8 s for 100 tokens on a T4.
- **Sampling (decoding)**: choose tokens according to their probability scores (40% token picked 40% of the time) vs greedy decoding (always the highest score; temperature = 0).
- **Grouped-query attention groups**: heads are grouped; each group shares one set of keys and values matrices (fewer shared sets than heads, more than one).

### Diagrams / Visuals
- **Figure 3-1** — High-level abstraction: Transformer LLMs take a text prompt and output generated text.
- **Figure 3-2** — Four steps of token generation; each step is one forward pass (one token generated at a time).
- **Figure 3-3** — An output token is appended to the prompt; the new text is presented to the model again for the next forward pass.
- **Figure 3-4** — A Transformer LLM = tokenizer + stack of Transformer blocks + LM head.
- **Figure 3-5** — Tokenizer vocabulary of 50,000 tokens with associated token embeddings.
- **Figure 3-6** — At the end of the forward pass, the model predicts a probability score for each token in the vocabulary.
- **Figure 3-7** — Decoding strategy chooses a token by sampling from the probability distribution.
- **Figure 3-8** — Each token is processed through its own stream of computation (with interaction between streams in attention steps).
- **Figure 3-9** — Each processing stream takes a vector as input and produces a final result vector of the same size (model dimension).
- **Figure 3-10** — Caching computation results of previous tokens avoids repeating the same calculation for each new token.
- **Figure 3-11** — The bulk of processing happens inside a series of Transformer blocks, each handing results to the next.
- **Figure 3-12** — A Transformer block = a self-attention layer + a feedforward neural network.
- **Figure 3-13** — The feedforward network likely does the majority of the model's memorization and interpolation.
- **Figure 3-14** — Self-attention incorporates relevant information from previous positions to process the current token.
- **Figure 3-15** — Simplified framing of attention: input sequence + current position being processed; input vector → output vector incorporating previous elements.
- **Figure 3-16** — Attention's two major steps: relevance scoring for each position, then combining information based on the scores.
- **Figure 3-17** — Multiple attention heads run in parallel, with a preceding splitting step and a later combining step.
- **Figure 3-18** — Before self-attention: layer inputs + query/key/value projection matrices.
- **Figure 3-19** — Attention carried out by interaction of queries, keys, values matrices (bottom row = current position).
- **Figure 3-20** — Relevance scoring: query of current position × keys matrix → relevance scores.
- **Figure 3-21** — Combining information: multiply relevance scores by respective value vectors, then sum.
- **Figure 3-22** — Local attention boosts performance by attending only to a small number of previous positions.
- **Figure 3-23** — Full attention vs sparse attention (source: "Generating long sequences with sparse transformers").
- **Figure 3-24** — Attention figures legend: which token is being processed (dark blue) and which previous tokens it can attend to (light blue).
- **Figure 3-25** — Comparison of multi-head, grouped-query, and multi-query attention (source: "Fast transformer decoding: One write-head is all you need").
- **Figure 3-26** — Multi-head attention: each head has a distinct query, key, and value matrix.
- **Figure 3-27** — Multi-query attention shares keys and values across all heads (only queries differ).
- **Figure 3-28** — Grouped-query attention: groups of attention heads share key/value matrices.
- **Figure 3-29** — The Transformer block from the original Transformer paper (with residual connections and layer normalization).
- **Figure 3-30** — A 2024-era Transformer block (Llama 3): pre-normalization, grouped-query attention, rotary embeddings.
- **Figure 3-31** — Packing: grouping multiple short documents in a single context while minimizing padding.
- **Figure 3-32** — Rotary embeddings are applied in the attention step, not at the start of the forward pass.
- **Figure 3-33** — Rotary positional embeddings are added to token representations just before relevance scoring in self-attention.

### Common Exam Traps
- **One token per forward pass**: LLMs generate one token at a time, not the entire response in one operation; each step appends the output token to the prompt.
- **Autoregressive ≠ representation**: Text generation LLMs are autoregressive (consume their own earlier predictions); BERT and similar representation models are not.
- **LM head output is probabilities, not text**: The LM head outputs a probability score for every token in the vocabulary; the decoding strategy selects the actual token.
- **The model does not receive raw text**: It receives token IDs; the tokenizer sits on both sides (input and output).
- **Only the last stream's output feeds the LM head**: But previous streams' computations are still needed — attention uses earlier outputs inside each block.
- **Context length = number of streams**: 4K context = 4K token streams, max 4K tokens processed at once.
- **KV cache is enabled by default**: In Hugging Face Transformers you must set `use_cache=False` to disable it; the measured speedup was ~4.5 s vs ~21.8 s on a T4.
- **KV cache ≠ a trick that changes results**: It skips recomputation of previous streams — the output is identical, just faster.
- **Feedforward = memorization + interpolation**: Not just a lookup database; it generalizes to unseen inputs.
- **Attention is two steps**: (1) relevance scoring (query × keys, softmax-normalized), then (2) combining information (values × scores, summed). Don't confuse which matrices are used where.
- **Queries/keys/values are projections**: Produced by multiplying layer inputs by the projection matrices; each head has its own.
- **Multi-query vs grouped-query vs multi-head**: MQA shares one set of K/V across ALL heads; GQA shares K/V within GROUPS (more than one shared set); multi-head gives every head its own. GQA trades a little MQA efficiency for quality (Llama 2/3).
- **Sparse attention everywhere would degrade quality**: GPT-3 interleaves full- and efficient-attention blocks; it doesn't use sparse attention for all blocks.
- **Flash Attention optimizes GPU memory movement**: SRAM ↔ HBM, not the math itself (exact attention, IO-aware).
- **RoPE is applied in the attention step**: Rotary embeddings are mixed into queries and keys just before relevance scoring — NOT added at the start of the forward pass like absolute embeddings.
- **RoPE captures absolute AND relative position**: Better than absolute-only schemes, which break with document packing.
- **Packing vs context length**: Packing fills a context with multiple short documents (minimizing padding); a packed document starting at position 50 must not be labeled position 50, or the model assumes unrelated previous context.
- **Greedy = temperature 0**: Covered more in Chapter 6; greedy is the deterministic maximum-probability choice.
- **LLM APIs stream tokens**: Because even ~4 s is too long to wait for a full response.

### Chapter Summary
Chapter 3 explains how Transformer LLMs work internally, with a focus on text generation. The high-level picture: a Transformer LLM is text-in/text-out software that generates one token per forward pass in an autoregressive loop, appending each output token to the prompt. The system has three components — the tokenizer (which holds the vocabulary), a stack of Transformer blocks (the bulk of processing), and the LM head (which outputs probability scores for every token in the vocabulary). A decoding strategy (sampling, or greedy — temperature 0) selects the single output token. Transformers process all input tokens in parallel through individual streams equal to the model's context length, though only the last stream's output is used for prediction; a KV cache skips recomputation of previous streams and speeds generation dramatically (4.5 s vs 21.8 s for 100 tokens on a T4).

Each Transformer block contains two components: a feedforward neural network (memorization and interpolation) and an attention layer (incorporating context). Attention is calculated by projecting inputs into queries, keys, and values; the query of the current position is multiplied by the keys matrix for relevance scoring (softmax-normalized), and the values are combined weighted by those scores. Multiple attention heads run in parallel for greater capacity. Recent improvements include sparse/sliding-window attention, multi-query and grouped-query attention (Llama 2/3), Flash Attention (GPU memory-optimized), pre-normalization with RMSNorm and SwiGLU activations, and rotary positional embeddings (RoPE) applied within the attention step — enabling the efficient, high-quality Transformers behind modern LLMs. Chapter 4 moves to practical applications, starting with text classification.

### Confidence Check
- **Sure**: autoregressive one-token-at-a-time generation; three components (tokenizer/blocks/LM head); Phi-3 shapes `[1,6,3072]` → `[1,6,32064]` and "Paris" decoding; context length = number of streams; KV cache timing numbers and default-on behavior; feedforward = memorization + interpolation; "The Shawshank"→"Redemption"; attention = relevance scoring + combining; queries/keys/values projections; multi-head → multi-query → grouped-query progression (Llama 2/3); GPT-3 interleaving full/sparse blocks; Flash Attention SRAM↔HBM; RMSNorm/SwiGLU/pre-norm; RoPE applied in attention step and why (packing problem).
- **Uncertain**: Exact figure numbers in the printed page flow for some figures (page anchors from PDF text are approximate); the precise wording of a few quoted passages where the PDF extraction broke lines mid-sentence (numbered-list markers duplicated by extraction); minor — the exact temperature definition details deferred to Chapter 6.

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading the model and declaring a pipeline
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline
# Load model and tokenizer
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
# Create a pipeline
generator = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=False,
    max_new_tokens=50,
    do_sample=False,
)
```
- **Explanation**: Loads the Phi-3-mini-4k-instruct model and tokenizer, then wraps them in a text-generation pipeline that generates up to 50 new tokens (`max_new_tokens=50`) using greedy decoding (`do_sample=False`) and returns only the newly generated text (`return_full_text=False`).
- **Fits the architecture**: The pipeline runs the autoregressive loop under the hood — forward pass, decode, append token, repeat.

### Code Block 2: Generating an email with the pipeline
```python
prompt = "Write an email apologizing to Sarah for the tragic gardening mishap. Explain how it happened."
output = generator(prompt)
print(output[0]['generated_text'])
```
```
Solution 1:
Subject: My Sincere Apologies for the Gardening Mishap
Dear Sarah,
I hope this message finds you well. I am writing to express my deep
```
- **Explanation**: The model begins the email ("Subject: ...") and stops abruptly because it hit the 50-token limit set by `max_new_tokens`; raising the limit would let it continue until concluding the email.
- **Fits the architecture**: Demonstrates token-by-token generation; the 50-token cutoff is visible in the truncated output.

### Code Block 3: Printing the model structure
```python
print(model)
```
```
Phi3ForCausalLM(
  (model): Phi3Model(
    (embed_tokens): Embedding(32064, 3072, padding_idx=32000)
    (embed_dropout): Dropout(p=0.0, inplace=False)
    (layers): ModuleList(
      (0-31): 32 x Phi3DecoderLayer(
        (self_attn): Phi3Attention(
          (o_proj): Linear(in_features=3072, out_features=3072, bias=False)
          (qkv_proj): Linear(in_features=3072, out_features=9216, bias=False)
          (rotary_emb): Phi3RotaryEmbedding()
        )
        (mlp): Phi3MLP(
          (gate_up_proj): Linear(in_features=3072, out_features=16384, bias=False)
          (down_proj): Linear(in_features=8192, out_features=3072, bias=False)
          (activation_fn): SiLU()
        )
        (input_layernorm): Phi3RMSNorm()
        (resid_attn_dropout): Dropout(p=0.0, inplace=False)
        (resid_mlp_dropout): Dropout(p=0.0, inplace=False)
        (post_attention_layernorm): Phi3RMSNorm()
      )
    )
    (norm): Phi3RMSNorm()
  )
  (lm_head): Linear(in_features=3072, out_features=32064, bias=False)
)
```
- **Explanation**: Shows the nested layers: the bulk is `model` (Phi3Model) plus `lm_head`. Inside Phi3Model, `embed_tokens` is the embeddings matrix (32,064 tokens × 3,072-dim vectors); then a stack of 32 `Phi3DecoderLayer` blocks, each with a self-attention layer and an MLP (feedforward); finally `lm_head` maps a 3,072-dim vector to 32,064 scores (one per vocabulary token).
- **Fits the architecture**: The structure directly mirrors the three-component model (tokenizer + blocks + LM head); each block contains the attention layer and feedforward network described in the chapter.

### Code Block 4: Manually running the model and LM head
```python
prompt = "The capital of France is"
# Tokenize the input prompt
input_ids = tokenizer(prompt, return_tensors="pt").input_ids
input_ids = input_ids.to("cuda")
# Get the output of the model before the lm_head
model_output = model.model(input_ids)
# Get the output of the lm_head
lm_head_output = model.lm_head(model_output[0])
```
- **Explanation**: Tokenizes the prompt, runs the Transformer-block stack (`model.model`), then applies the LM head. The prompt "The capital of France is" produces six tokens, so `model_output[0].shape` = `torch.Size([1, 6, 3072])` and `lm_head_output.shape` = `torch.Size([1, 6, 32064])`.
- **Fits the architecture**: `[1, 6, 3072]` = batch of 1, six tokens, each a 3,072-dim vector (model dimension); `[1, 6, 32064]` = probability scores for all 32,064 vocabulary tokens at each position.

### Code Block 5: Choosing and decoding the token
```python
token_id = lm_head_output[0,-1].argmax(-1)
tokenizer.decode(token_id)
```
```
Paris
```
- **Explanation**: `lm_head_output[0,-1]` selects the last token's score list across the batch (index 0) and sequence (index −1); `.argmax(-1)` returns the highest-scoring token ID; `tokenizer.decode` converts it to text: "Paris".
- **Fits the architecture**: This is greedy decoding — picking the most probable token (equivalent to temperature 0).

### Code Block 6: Timing generation with and without KV cache
```python
prompt = "Write a very long email apologizing to Sarah for the tragic gardening mishap. Explain how it happened."
input_ids = tokenizer(prompt, return_tensors="pt").input_ids
input_ids = input_ids.to("cuda")
```
```python
%%timeit -n 1
# Generate the text
generation_output = model.generate(
  input_ids=input_ids,
  max_new_tokens=100,
  use_cache=True
)
```
```python
%%timeit -n 1
# Generate the text
generation_output = model.generate(
  input_ids=input_ids,
  max_new_tokens=100,
  use_cache=False
)
```
- **Explanation**: `%%timeit` measures average runtime (runs the cell several times). Generating 100 tokens with `use_cache=True` took ~4.5 seconds on a Colab T4 GPU; with `use_cache=False` it took ~21.8 seconds.
- **Fits the architecture**: The cache reuses previous streams' keys/values so each new token only needs the last stream computed — a dramatic generation speedup. Cache is on by default.

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*

## §4. Practice Exam
*(Separate file: `practice_exam.md`)*
