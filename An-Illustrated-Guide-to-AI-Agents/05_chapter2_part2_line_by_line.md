# Chapter 2 — Line-by-Line Detailed Explanation (Part 2)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 2 "Large Language Models" — Part 2 (deep dive: Transformer internals, self-attention, efficient attention, MoE).
**Note:** Each numbered item quotes a paragraph from the book, then gives (1) a plain-English explanation, (2) word meanings, (3) technical terms explained.

---

## 2.6 The Transformer Architecture

> "LLMs have predominantly been neural networks built in the Transformer decoder architecture since around 2020."

**Word meanings:**
- **predominantly** = mostly.

**Technical terms:**
- **neural network** = a computing system modeled loosely on the brain, made of many interconnected "neurons" (nodes) that learn from data.
- **Transformer** = the architecture introduced in the 2017 paper "Attention Is All You Need" that powers modern LLMs.
- **decoder** = in the Transformer, the part that generates text by predicting the next token. GPT-style LLMs are *decoder-only* (no separate encoder). Contrast: the original Transformer had both an encoder (reads input) and decoder (writes output).

### The tokenizer, Transformer blocks, and language modeling head

> "A Transformer decoder has three major components: a tokenizer, a stack of Transformer blocks, and a language modeling head (LM head)."

**Technical terms (three components — memorize):**
1. **Tokenizer** = software that splits input text into tokens.
2. **Stack of Transformer blocks** = many identical processing layers applied in sequence.
3. **Language modeling head (LM head)** = the final layer converting representations into a probability distribution over the vocabulary.

> "The tokenizer is the piece of software responsible for breaking the text input into tokens. It is carefully optimized earlier in the training process of the LLM to help bring out the capabilities we need in the trained LLM. The vast majority of agent developers will work with a ready-made tokenizer prepared by the model provider."

> "For an LLM intended to power agents, careful tuning for a tokenizer includes support for generating software code, the various languages intended for use, and adding special tokens (like <|start|>) that are used in the specified data format."

**Word meanings:**
- **tuning** = adjusting/optimizing.

**Technical terms:**
- **special tokens** = reserved tokens not representing words, used for formatting structure (e.g., `<|start|>`, `<|end|>`, role markers).

> "A tokenizer has a set number of tokens in its vocabulary, say 50,000. We can also see in the figure that the model has an equal number of vector embeddings– one per token in our vocabulary. These embeddings vectors are numeric representations of each token and they are what the model uses to calculate language and how it processes its inputs."

**Technical terms:**
- **vocabulary** = the fixed set of tokens a tokenizer knows (e.g., 50,000 entries).
- **embeddings** = numeric vectors (lists of numbers) representing each token; the model's "initial view" of a token.
- **vector** = an ordered list of numbers, often representing a point in high-dimensional space.

> "Almost the entirety of the processing done inside a language model is conducted inside the stack of Transformer blocks. But what results from that process is a single vector containing information on what the next token should be."

> "That vector is passed to the LM head to interpret. It makes a simple calculation, which results in a probability score for each token in its vocabulary. That score is informed by everything the model learned in its training phases, and tokens with high probabilities are the ones most likely to appear as a completion in response to the input tokens."

**Technical terms:**
- **probability score** = how likely each vocabulary token is to be next. The LM head maps the final vector to these scores (logits → softmax).

> "The next natural step would be to actually pick the output token as informed by these probabilities. We may choose the highest probability token, but there are often good reasons for picking other tokens. The method to pick tokens from the probability distribution is called a decoding strategy."

**Technical terms:**
- **decoding strategy** = the rule for choosing a token from the probability distribution (e.g., greedy vs sampling).

> "If you've played with the temperature setting of an LLM, then you have interacted with these probabilities. A temperature value of zero leads to always choosing the single token with the highest probability. Increasing that temperature allows sampling, which means choosing from the distribution in a way where higher probability tokens have a higher chance of being picked."

**Technical terms:**
- **temperature** = a knob controlling randomness:
  - **0** → greedy: always pick the top-probability token (deterministic).
  - **higher** → sampling: lower-probability tokens get a chance.
- **sampling** = randomly choosing a token weighted by its probability.

---

### Processing through the Transformer blocks

> "A neural network processes inputs and produces an output in a forward pass. This means that the calculation flows sequentially through the various layers. This is exactly what happens with the Transformer blocks. First the first block starts processing, then it passes its results of processing to the next block, and so on. The final block passes its result to the LM head, and we proceed to decoding as we've seen in the previous section."

**Technical terms:**
- **forward pass** = running input through the network once, layer by layer, to get output.

> "It also shows how each token can be seen flowing through its own track. The number of tracks that a model supports is commonly known as its context size. So, a model that has a context length of 100,000 can have only that number of tokens flowing through it simultaneously, which generally limits its input and output capacity to text of that size."

**Word meanings:**
- **simultaneously** = at the same time.

**Technical terms:**
- **track** = a token's own parallel lane through the Transformer blocks.
- **context size / context length** = the max number of tokens that fit in the model at once. Example: 100,000-token context = 100,000 token tracks.
- **context window** = the same idea; the model can only "see" this much input.

> "For text generation, the processing flow of the last token is what's used to generate the next token. That calculation is informed, however, by all the processing conducted on the previous tokens as we'll see next when looking closer at the insides of a Transformer block."

**Explanation:** To predict the next token, only the *final* token's output vector is sent to the LM head — but that vector has been enriched with information from all earlier tokens via self-attention.

> "For agent developers, context length is a key property for selecting the best model to build an agent around. It sets a limit on how much information we can pack in the input of the model. This limitation gives rise to a whole discipline of context engineering that we'll revisit repeatedly in this book. This is the discipline of choosing the most relevant information for the model to fit within this architectural limitation of the Transformer model."

**Word meanings:**
- **gives rise to** = causes/creates.
- **discipline** = here, a field/practice.

**Technical terms:**
- **context engineering** = deliberately choosing what information to include in the prompt so it fits the context window and stays relevant.

> "Another key concept for agent developers is to realize that after that first pass where all the input tokens are processed, it's common to cache the results so that in the next forward pass, we process the information along only the one track associated with the new token we're generating. This is referred to as prompt-caching, prefix-caching, or more technically, kv-caching. In Figure 2-20, we can see how only one track is active and information from previous tracks is cached and used for that one active calculation. This leads to dramatically increased speed and reduces the amount of processing required."

**Technical terms:**
- **kv-caching** = caching previously computed Keys and Values (details in section 2.7) so each generation step only computes the new token's track.
- **prompt-caching / prefix-caching** = other names for the same optimization.

> "Optimizing for the kv-cache is a key responsibility for agent developers to increase the speed and reduce the cost of the agents they build. In Chapter 10, we cover some strategies used by developers of software engineering agents to optimize for this cache, reducing the latency and improving the economics of their agents."

**Word meanings:**
- **latency** = delay before a response.
- **economics** = here, cost-effectiveness.

---

### Inside the Transformer block

> "There are two major components inside a Transformer block: a self-attention layer and a feed-forward neural network layer."

**Technical terms (memorize):**
- **self-attention layer** = lets each token gather context from other tokens in the sequence.
- **feed-forward neural network (FFNN) layer** = processes each token's representation independently.

> "We've seen how the input text is broken down into tokens. And we've also seen that each token has an associated static embedding vector in the model. The Transformer operates on these embedding vectors. The first Transformer block is presented with the embedding vectors associated with the tokens in the input text. As we can see in Figure 2-21, the first block does a bit of processing on these input vectors and hands off the results of its processing (as the same number and size of vectors) to the next Transformer block. This goes on block by block until the last block in the stack."

**Explanation:** Blocks transform vectors but keep their count and size constant — only the *values* change as representations are refined.

---

### The Transformer block: the feed-forward neural network

> "Let's first talk about the feed-forward neural network because it's simpler, even though in the architecture it comes after self-attention. This layer does the heavy lifting in predicting the next token because the training process shapes its ability to predict the patterns encoded in the training dataset. If we train a feed-forward neural network on vast amounts of web data, when we give it the words 'The Shawshank', it would be able to predict that the next word would be 'Redemption', because the 1994 film with that name is the most common occurrence of this sequence of tokens."

**Word meanings:**
- **heavy lifting** = the main hard work.

**Technical terms:**
- **factual associations** = the feed-forward layers are thought to store learned facts/patterns (e.g., "The Shawshank" → "Redemption").

> "In the original Transformer as well as in the majority of LLMs at the time of writing, the feed-forward neural network layer is one big neural network. The training process prunes and adapts its connections in a way that encodes the patterns in language. Recall that by now, we're no longer talking about a specific human language, we're talking about a model that supports multiple human languages, multiple programming languages, as well as the patterns we need to define tool calling, multi-turn conversations, and, as we'll see in Chapter 3, reasoning patterns."

**Word meanings:**
- **prunes** = trims away (connections).

**Technical terms:**
- **dense model** = one big feed-forward network where all neurons are active for every token.

> "In a dense model, the feed-forward network expands the token representation to a larger hidden dimension (here, 512 inputs expanded to 2048) before compressing back down, with all neurons active for every token. Research suggests these layers play a significant role in storing factual associations learned during training, though the full picture of how knowledge is distributed across a model remains an open question."

**Technical terms:**
- **hidden dimension** = the internal width of the network (e.g., 512 → 2048 → 512).

> "In recent years, there has been a trend away from having a single large network, and replacing it with a large number of smaller, more specialized networks. This architecture is called a mixture-of-experts (MoE) architecture, and we talk about it more in the second part of this chapter. We mention this here because as you select a model to power your agent, you may come across a choice of a dense model and an MoE model, especially if you're looking at open source models."

**Word meanings:**
- **trend** = general direction.

**Technical terms:**
- **mixture-of-experts (MoE)** = replacing one big feed-forward net with many smaller "expert" nets plus a router.

> "In Figure 2-25, we see a basic MoE feed-forward layer that contains four sublayers; each is called an expert. These are preceded by a router that looks at the input token and decides which expert (or set of experts) is best suited to process this particular token."

**Technical terms:**
- **expert** = a smaller, specialized feed-forward network.
- **router** = the network that picks which expert handles each token.

---

### The Transformer block: an overview of self-attention

> "Language encodes a lot of information in the order of words in a sequence and the context that a word is used in. A word such as bank can mean a financial institution or can mean a riverbank. We would only know which is meant by looking at the context where the word is used. The self-attention layer allows the Transformer to make these distinctions."

**Technical terms:**
- **disambiguation** (implied) = resolving which meaning of a word is intended. Example: "bank" (money) vs "bank" (river).

> "As we can see in Figure 2-26, a model is presented with an input sentence that is, 'The dog chased the llama because it'. When processing the word it in its own processing track, the model needs to know whether it refers to the dog or the llama. Self-attention is tuned to resolve this kind of problem."

> "The high-level intuition is that it attends to the most relevant previous tokens in the sequence. It learns that relevance from the training process. Self-attention enriches the information encoded in the input vector with information from the most relevant previous tokens in the sequence."

**Word meanings:**
- **attends to** = focuses on / pays attention to.
- **enriches** = adds to / enhances.

> "Self-attention does this by taking two steps: first, it scores the relevance of the previous tokens and then proceeds to a step of combining the relevant information into the token we're processing."

**Technical terms (two steps — memorize):**
1. **relevance scoring** = how much attention to pay to each previous position.
2. **combining information** = blending the relevant positions' representations into the current token's output vector, proportional to their scores.

> "Self-attention is one of the most resource-intensive operations in the Transformer. That's why it's commonly one of the areas most targeted for improvement."

**Word meanings:**
- **resource-intensive** = uses a lot of memory/compute.

---

## 2.7 How Self-Attention Works (the math)

> "A trained model is able to attend properly by utilizing three projection matrices that resulted from the training process. We multiply the input vectors by each of these projection matrices, resulting in matrices we call the Queries, Keys, and Values matrices."

**Technical terms (memorize):**
- **projection matrices** = learned weight matrices that transform input vectors into new spaces.
- **Queries (Q)** = "what am I looking for?" — the current token's search query.
- **Keys (K)** = "what do I contain?" — labels of other tokens describing their content.
- **Values (V)** = "what do I actually contribute?" — the actual information content of each token.

> "The first step of self-attention, relevance scoring, involves multiplying the query for the token we're currently processing by the key vectors associated with all the input tokens. This results in a relevance score for each vector. Here, 'dog' receives the highest score (40%), indicating it is the most relevant preceding token to the current position."

**Explanation:** Query × Key = a score measuring how related the current token is to each other token. "Dog" wins in the book's example.

> "After deciding the relevance values, self-attention then proceeds to combine information from these tokens, weighted by how relevant they are, and merge them into the vector of the current token we're processing. We see that weighted summation calculation. This vector becomes the output of the self-attention layer."

**Explanation:** Multiply each Value by its relevance weight and add them up — that weighted sum is the enriched output vector.

> "Figure 2-33 presents this in another way, closer to the mathematical formula you'll often see in LLM literature. The current and previous tokens are projected into Queries, Keys, and Values; the query-key product (scaled by √d_k and passed through softmax) produces the relevance scores, which are then multiplied by the Values to produce the attention output for the current position."

**The famous formula (memorize):**
```
Attention(Q, K, V) = softmax(Q · Kᵀ / √d_k) · V
```
**Word meanings:**
- **literature** = published research papers.

**Technical terms:**
- **Q · Kᵀ** = the dot product of Queries with transposed Keys (measures similarity).
- **√d_k** = square root of the key dimension — a scaling factor to prevent scores from getting too large (which would make softmax too "peaked"/extreme).
- **softmax** = a function that turns raw scores into a probability distribution (all positive, summing to 1).
- **d_k** = the dimension (size) of the key vectors.

---

## 2.8 KV-Caching Revisited

> "What we've described so far are major components of self-attention as described in the Transformer paper, which is often called 'vanilla attention.' This early form of self-attention, however, does a lot of redundant calculations of the K and V weights for each generated attention score if we apply it naively to text generation. Specifically, at each step, attention is recalculated for the entire sequence despite having calculated the K and V vectors of the previously seen tokens before."

**Word meanings:**
- **vanilla** = basic/original version (from vanilla = plain flavor).
- **redundant** = unnecessary because already done.
- **naively** = in the simplest, unoptimized way.

**Explanation of the problem:** When generating token-by-token, the model re-runs attention over the whole sequence every step, recomputing K and V for earlier tokens that never changed. That's wasted work.

> "This is where the KV cache comes in. Instead of having to recalculate those vectors, we can simply cache and reuse them for subsequent decoding steps. This makes inference much faster by reducing the redundant computation."

**Word meanings:**
- **cache** = store for later reuse.

> "Also note that the Query (Q) vectors do not need to be cached because they become unnecessary in subsequent iterations. Specifically, we need only the query vector of the latest token to compute the self-attention."

**Key point (memorize):** Cache **K and V**, not **Q** — only the newest token's query is ever needed.

> "Although such a KV cache can make inference much faster, it does require significantly more memory if all KV values are cached. For that, there are many different forms of attention created that attempt to reduce the calculations needed, which should therefore also reduce the KV cache that needs to be maintained."

**Explanation:** The KV cache speeds up time but costs memory → motivates the efficient-attention variants next.

---

## 2.9 More Efficient Self-Attention

> "Variants of attention mechanisms have since been developed to alleviate the memory issue of the KV cache and optimize the attention calculations. Popular techniques include Grouped-Query Attention and Flash Attention. More recently, however, DeepSeek introduced two attention mechanisms that showed tremendous improvements in the efficiency of attention calculations, namely Multi-head Latent Attention (MLA) and DeepSeek Sparse Attention (DSA)."

**Word meanings:**
- **alleviate** = reduce (a problem).

**Technical terms (overview table):**
- **Flash Attention** = an IO-aware algorithm that makes attention fast and memory-efficient at the GPU level (by avoiding writing the full attention matrix to memory). (Book: Dao et al., 2022.)
- **Grouped-Query Attention (GQA)** = shares K and V across groups of query heads.
- **Multi-Query Attention (MQA)** = shares a single K and V across all query heads.
- **Multi-head Latent Attention (MLA)** = compresses K and V into a small latent vector for caching (DeepSeek-V2).
- **DeepSeek Sparse Attention (DSA)** = selects only the most relevant tokens to attend to (DeepSeek-V3.2).

### Multi-head Latent Attention (MLA)

> "MLA is a variant of Multi-head Attention, which maintains separate Q, K, and V projection matrices for each attention head, producing a different Q, K, and V matrix per head. MLA uses low-rank joint compression of the keys and values to reduce the KV cache during inference. At its core, it compresses the keys and values into a smaller latent representation that is cached in place of the full K and V. In practice, this compressed cache is often combined with quantization (reducing the numerical precision of stored values) for additional memory savings."

**Word meanings:**
- **joint** = together (both keys and values).

**Technical terms:**
- **Multi-head Attention (MHA)** = standard attention with several parallel "heads," each with its own Q, K, V.
- **attention head** = one independent attention computation, run in parallel with others.
- **low-rank** = a smaller, compressed representation that captures most of the important information.
- **latent representation** = a compressed/hidden, lower-dimensional representation.
- **quantization** = storing numbers at lower precision (fewer bits) to save memory.

> "MLA first compresses the input embeddings into lower-dimensional representations called the Latent Q and Latent KV. These representations are significantly smaller than the full Q and KV, which allows us to cache the Latent KV instead of the full K and V. Positional information via Rotary Position Embedding (RoPE) is applied to a decoupled component of the Latent Q, since the Latent Q is recomputed at every step. RoPE is not applied to the Latent K itself, because the cached K would then need to be recomputed at every step, breaking the benefit of caching. Therefore, positional information is carried by a separate small key instead. Note that at this step, the Q, K, and V representations are split across multiple attention heads, much like standard Multi-head Attention. Finally, the content and positional components are concatenated and passed through standard Multi-head Attention."

**Technical terms (memorize the MLA details):**
- **Latent Q** = compressed query; recomputed every step.
- **Latent KV** = compressed keys+values; this is what gets cached (instead of full K, V).
- **RoPE (Rotary Position Embedding)** = a method for injecting positional information (token order) into Q and K. In MLA it is applied to a **decoupled component of Latent Q** (because Latent Q is recomputed each step) — **NOT to Latent K** (which would force recomputation of the cache, destroying the benefit). Position is carried by a **separate small key**.
- **decoupled** = separated out from the main pathway.
- **concatenated** = joined together (content + positional parts) before standard multi-head attention.

> "As such, MLA is essentially Multi-head Attention but with a compressed KV cache containing the previously mentioned Latent KV representation. This compression reduces the KV cache quite a bit and allows for much faster inference. It's also more efficient than previous methods, such as Grouped-Query Attention."

> "Note that Grouped-Query Attention and Multi-Query Attention share K and V across query heads to reduce the memory necessary for the KV cache but tend to be less accurate."

**Summary of the four attention approaches (memorize):**
| Approach | How it saves memory | Trade-off |
|---|---|---|
| MHA | None (full K and V per head) | Most memory/accurate |
| MQA | Shares one K, V across all heads | Less memory, less accuracy |
| GQA | Shares K, V across groups of heads | Middle ground |
| MLA | Caches small compressed Latent KV, projects back up | Very efficient, more accurate than GQA |

---

### DeepSeek Sparse Attention (DSA)

> "The next step in making MLA more efficient was first introduced in DeepSeek-V3.2. This new attention mechanism, called DeepSeek Sparse Attention (DSA), is instantiated under MLA and is an additional module to more efficiently select the tokens to attend to. It has two main components, a lightning indexer and a Top-K Selector."

**Word meanings:**
- **instantiated** = implemented/created as a specific case.
- **sparse** = not dense; only a subset used.

**Technical terms:**
- **sparse attention** = attention computed over only a subset of tokens instead of the full sequence.

> "The lightning indexer determines how relevant each preceding token is to the current query. For that, it uses the Q/K values that both have RoPE applied to them. Likewise, it takes in a scalar weight parameter w that helps the lightning indexer make better decisions about which tokens to select for the full attention mechanism. The output of the lightning indexer is scores fed to the Top-K Selector, which in turn retrieves only the KV entries that correspond to the Top-K index scores. As a result, the attention output is computed through the query token and a subset of KV entries."

**Technical terms:**
- **lightning indexer** = a lightweight scorer that rates how relevant each preceding token is to the current query (uses RoPE-applied Q/K and a scalar weight `w`).
- **Top-K Selector** = keeps only the K highest-scoring KV entries.
- **Top-K** = selecting the top K items from a ranked list.

> "Together, the Lightning Indexer and Top-K Selector reduce the number of tokens to attend to K. In the paper referenced, the authors selected 2048 tokens to attend to, which drastically reduces the computation necessary for the attention computations."

**Memorize:** DSA attends to only K tokens (e.g., 2048) rather than the entire sequence.

> "Note that since MLA is used, there is already a significant memory benefit due to the compression of the KV-cache. In their construction of MLA, instead of using MHA as the core attention mechanism, MQA was used. The authors mentioned optimizing for computational efficiency, which is true for MQA, considering it uses fewer keys and values than MHA."

**Key point:** DeepSeek builds DSA on top of MLA, and uses **MQA** (not MHA) as the core attention for maximum computational efficiency (fewer K and V).

---

## 2.10 Mixture of Experts (MoE)

> "A technique that is becoming more mainstream to create more efficient LLMs is MoE. MoE uses several submodels or 'experts' to improve the quality and efficiency of LLMs. There are two main components of an MoE: **Experts** — Each feed-forward neural network in an LLM is replaced by a set of 'experts'; **Router or gate network** — Determines which tokens are sent to which experts."

**Word meanings:**
- **mainstream** = widely adopted.
- **submodels** = smaller component models.

**Technical terms (two components — memorize):**
1. **Experts** = the set of smaller feed-forward networks replacing the single one.
2. **Router (gate network)** = decides which expert handles each token.

> "Remember that a typical Transformer-based LLM uses Self-Attention followed by a feed-forward neural network. We call this a dense model because everything is activated. In a dense Transformer block, the feed-forward neural network is a single network applied to every token. Here, a 512-dimensional input is expanded to 2048 and compressed back down, with every connection and neuron active for every token that passes through."

**Technical terms:**
- **dense model** = all parameters/neurons active for every token.

> "A sparse model, in contrast, may deploy several feed-forward neural networks instead. These are typically smaller than a regular network, but together they tend to be bigger. Each feed-forward neural network in a sparse model is typically referred to as an 'expert' because, during training, each 'expert' learns different information and may specialize in the processing of certain tokens. For instance, one expert might be used for processing numbers, whereas another processes verbs. It's still a bit unclear what these experts actually learn, but there has been some research suggesting that they specialize in fine-grained information such as verbs versus numbers rather than each learning an entirely different domain."

**Word meanings:**
- **fine-grained** = very specific/detailed.

**Technical terms:**
- **sparse model** = multiple experts, only some active per token.
- **specialization** = each expert learns to handle certain kinds of tokens (e.g., numbers vs verbs).

> "To choose a subset of experts during inference, we make use of the router (also called a gate network). This is a small feed-forward neural network that is trained to choose an expert for a given token. The router, together with the experts, makes up the MoE layer."

> "The router is arguably the most important component because the experts are nothing more than just small feed-forward neural networks. So, how exactly does the router then choose which expert to use for each token? The router, as a neural network, will have its own weight matrix, which is used to multiply the input token embeddings. Applying a softmax on the output will result in a probability distribution per expert. This probability distribution provides the likelihood that an expert will be chosen given an input token. The highest-probability expert (FFNN 1 at 0.45) is selected to process the token, and its output is scaled by the router's probability before being passed forward."

**Word meanings:**
- **scaled by** = multiplied by.

**Technical terms (memorize the router mechanics):**
- The router multiplies the token embedding by its own weight matrix, applies **softmax** → a probability over experts.
- The highest-probability expert is selected.
- **Crucially, the selected expert's output is scaled by the router's probability** before being passed forward.

> "Note that any number of experts can be selected, but generally a fixed number are selected for training and inference. When selecting multiple experts, there is a need to balance how much each expert is trained. If the same set of experts is always chosen during training and inference, then all other experts are undertrained."

**Word meanings:**
- **undertrained** = not trained enough (because rarely used).

**Technical terms:**
- **fixed number of experts** = e.g., always activate 2 out of 8 (or 8 out of 256) per token.

### Load balancing

> "To balance the distribution of training among experts, the router will have to dynamically balance which expert to choose and when. This is referred to as load balancing. To prevent one expert from dominating the training time, there are two main techniques that are often employed in one way or another, namely expert capacity and auxiliary loss."

**Word meanings:**
- **dominating** = taking over / being used too much.

**Technical terms (two load-balancing techniques — memorize):**
1. **Expert capacity** — a limit on how many tokens each expert can process per batch. When an expert is full, overflow tokens go to the next-highest-scoring expert.
2. **Auxiliary loss** — an extra loss term added to the router that rewards equal distribution of tokens across experts (or punishes repeatedly choosing the same expert).

> "Expert capacity gives each expert in the MoE layer a limit to how many tokens it can process. Instead of having a single expert do all the work, the tokens are somewhat more equally distributed. For instance, by the time an expert has reached capacity, each subsequent token routed to it will be sent to the next-highest scoring expert."

> "In contrast, instead of limiting the experts, the router can also be adjusted to account for this probability imbalance. A straightforward technique is to add Gaussian noise just before the router produces its probabilities. By introducing noise, the distributions will slightly change, and by (slight) chance, sometimes choose different experts to use. A more advanced technique to balance how the router selects experts is called auxiliary loss. These are loss functions that can be added to the router to reward it for equally distributing the experts during training or punish it when the same expert is chosen."

**Technical terms:**
- **Gaussian noise** = random values added to the router input (a simple balancing trick).
- **auxiliary loss** = a penalty/reward term steering the router toward balance.

### Sparse vs active parameters

> "The main benefit of MoE is its computational requirements. Although using MoE does not make the resulting model smaller, it runs much faster because only a few experts are activated at a given time. All parameters that a MoE model has need to be loaded into memory and are called sparse parameters. The active parameters, in contrast, are those that are activated only during inference."

**Technical terms (memorize):**
- **sparse parameters** = ALL parameters, which must all be loaded into memory (e.g., 30B).
- **active parameters** = the subset actually used during inference for a given token (e.g., 3B).
- MoE models aren't smaller — they just use less compute per token because only a few experts are active.

> "Most recent models tend to use MoE layers, such as OpenAI's GPT-OSS and NVIDIA's Nemotron 3. Often, you'll see models like Qwen3-30B-A3B that put the number of sparse parameters (30 billion) and active parameters (3 billion) in their name. As such, even though 30 billion parameters need to be loaded in memory, only 3 billion are used, which makes it much faster for inference."

**Example (memorize):** **Qwen3-30B-A3B** = 30B sparse (loaded into memory) + 3B active (used per token). Naming convention: `ModelName-SparseB-ActiveB`.

> "Another example of MoE is the previously discussed DeepSeek-R1. As shown in Figure 2-45, DeepSeek-R1 has 256 experts, of which 8 are always chosen. Note that there is also a shared expert bypassing the router. This expert is always chosen, which often helps the model divert all general knowledge to that expert and more specialized knowledge to all others."

**Technical terms (memorize):**
- **shared expert** = an expert that bypasses the router and is *always* active; it tends to absorb general knowledge while the routed experts specialize.
- **DeepSeek-R1 config**: 256 experts, 8 activated per token, plus 1 shared expert.

> "In practice, there are many different choices for the number of experts chosen for a given LLM and the sizes of each expert compared to the overall size of the LLM."

**From the book's Table 2-1 (selected examples):**
| Model | Sparse params | Active params | Experts activated | Shared experts |
|---|---|---|---|---|
| Mistral 8x7B | 46.7B | 12.8B | 4 | 0 |
| DeepSeek-R1 | 671B | 37B | 8 | 0 (well, plus shared) |
| Llama 4 Maverick | 400B | 17B | 8 | 0 |
| Qwen 3 235B-A22B | 235B | 22B | 8 | 0 |
| Kimi-K2 | 1000B | 32B | 4 | 2 |
| GPT-OSS 120B | 120B | 5.1B | 4 | 0 |
| GPT-OSS 20B | 20B | 3.6B | 8 | 0 |
| GLM 4.5 | 335B | 12B | 10 | 0 |
| Mistral 3 Large | 671B | 41B | 6 | 2 |

*(Note: the book's table numbers are illustrative of the sparse/active convention; the key takeaway is the sparse-vs-active distinction and that different models pick different expert counts.)*

---

## 2.11 Summary (Chapter 2)

> "This chapter covered the engine that powers every agent in this book: the LLM. Part 1 looked at LLMs from the perspective of an agent developer. Language models consume and produce tokens, and the formats built on top of those tokens–system prompts, multi-turn conversations, and tool calls–are what let a language model serve as the reasoning core of an agent. We then walked through how LLMs are created across two training phases. Pre-training via next-token prediction produces a base model. Post-training via SFT and RL shapes the base model into something that follows instructions and generates responses aligned with human preferences. We looked at how RLVR and the GRPO algorithm use verifiable signals such as format and correctness to push models toward reliable behavior. We also opened up the Transformer itself, tracing how tokens flow through a stack of blocks, each containing a self-attention layer and a feed-forward neural network, before the LM head converts the final representation into a next-token probability distribution. For agent developers, the two architectural properties worth carrying into later chapters are context length, which limits how much information we can pack into a model's input, and the KV cache, which shapes the economics and latency of every agent we build."

**Word meanings:**
- **aligned with** = matched to.

**Memorize these two key architectural properties:**
1. **Context length** — the input size limit (drives context engineering).
2. **KV cache** — drives speed/cost of every agent.

> "Part 2 went deeper into the internals. We saw how self-attention is actually computed through the Queries, Keys, and Values produced by three projection matrices and how relevance scoring and information combining make up the two steps of the attention operation. We then looked at how self-attention has evolved to address its memory and compute costs. The KV cache itself avoids redundant recomputation. Grouped-Query and Multi-query Attention shrink the cache by sharing K and V across heads. DeepSeek's Multi-head Latent Attention takes a different approach, caching a low-rank compressed representation that gets projected back up when needed. DeepSeek Sparse Attention adds a Lightning Indexer and Top-K Selector to attend to only the most relevant tokens instead of the entire sequence. We closed with MoE architectures, which replaced the dense feed-forward layer with a router and a set of smaller expert networks. This produces models where the total and active parameter counts can differ by an order of magnitude, a distinction that matters when selecting a model to deploy."

**Word meanings:**
- **order of magnitude** = roughly 10× (a factor of ten).

---

## Quick-Reference Glossary (Chapter 2)
- **LLM** = Large Language Model; predicts/generates tokens.
- **Token** = word/part-word/number/punctuation unit of I/O.
- **Autoregressive** = generates by feeding its own output back in.
- **Base model** = after pre-training (next-token prediction).
- **SFT** = supervised fine-tuning / instruction tuning (prompt excluded from loss).
- **RLHF** = RL from human/reward-model preferences.
- **RLVR** = RL with automated verifiable rewards.
- **GRPO** = group-relative policy optimization (groups + relative rewards).
- **Tokenizer** = text → token IDs.
- **Embeddings** = numeric vectors per token.
- **Transformer blocks** = self-attention + feed-forward layers.
- **LM head** = final layer → probability distribution over vocabulary.
- **Decoding** = picking a token (greedy at temperature 0; sampling above).
- **Context length** = max simultaneous tokens.
- **KV cache** = cached K and V for fast generation (Q not cached).
- **Self-attention** = Q·Kᵀ/√d_k → softmax → ·V.
- **MHA / MQA / GQA / MLA / DSA** = attention variants (see table).
- **RoPE** = Rotary Position Embedding (position info).
- **MoE** = router + experts; sparse vs active parameters.
- **Shared expert** = always-active expert bypassing the router.
- **Load balancing** = expert capacity / auxiliary loss.
