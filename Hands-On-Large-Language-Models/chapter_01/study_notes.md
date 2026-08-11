# 📘 Chapter 1 Study Bundle: An Introduction to Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 1

---

## §1. Study Notes

### Core Theme
Large language models are the flagship of Language AI — a field that builds systems to understand, process, and generate human language — and this chapter maps the history (bag-of-words → word2vec → attention → Transformers → BERT/GPT), the two model families (representation vs generative), the two-step training paradigm (pretraining + fine-tuning), common applications, responsible-use concerns, hardware realities, and how to generate your first text.

### Key Definitions
- **Language AI**: A subfield of AI focused on developing technologies capable of understanding, processing, and generating human language. Often used interchangeably with natural language processing (NLP). The term deliberately encompasses non-LLM technologies too, like retrieval systems that give LLMs superpowers.
- **Artificial intelligence (AI)**: Computer systems dedicated to performing tasks close to human intelligence (McCarthy: "the science and engineering of making intelligent machines, especially intelligent computer programs"). AI does not have to confine itself to biologically observable methods.
- **Bag-of-words**: A technique for representing unstructured text by splitting it into tokens and counting how often each unique vocabulary word appears. A classic "representation model" method (first mentioned ~1950s, popular ~2000s).
- **Tokenization**: The process of splitting text into individual words or subwords (tokens). Most common: split on whitespace — which fails for languages like Mandarin that lack spaces.
- **Vocabulary**: The set of all unique words retained across the corpus; used to represent each sentence as a count vector.
- **Vector / vector representation**: A numerical representation of text. Bag-of-words produces a vector of word counts.
- **Embedding**: A vector representation of data that attempts to capture its meaning.
- **word2vec**: A 2013 method that learns semantic word representations by training on vast text (e.g., entire Wikipedia) with neural networks, looking at which words tend to appear next to each other.
- **Parameters**: The weights on the connections between nodes in a neural network — numerical values representing the model's understanding of language.
- **Neural network**: Interconnected layers of nodes that process information; each connection has a weight.
- **RNN (recurrent neural network)**: A neural network variant that models sequences; used for encoding and decoding. Autoregressive: consuming all previously generated words to generate the next.
- **Attention**: A 2014 mechanism allowing a model to "attend to" relevant parts of the input sequence and amplify their signal. Lets the decoder focus on the most important input words.
- **Self-attention**: Attention to different positions within a single sequence; lets the model look forward and back in one sequence in one go.
- **Transformer**: The 2017 "Attention Is All You Need" architecture based solely on attention; stacked encoder/decoder blocks; trainable in parallel (vs RNNs' sequential limitation).
- **Encoder block**: In the Transformer, self-attention followed by a feedforward neural network.
- **Decoder block**: Same as encoder plus an extra attention layer over the encoder output; the self-attention layer masks future positions.
- **BERT (Bidirectional Encoder Representations from Transformers)**: 2018 encoder-only architecture focusing on representing language; trained with masked language modeling; the [CLS] token represents the whole input.
- **GPT (Generative Pre-trained Transformer)**: 2018 decoder-only architecture for generative tasks; GPT-1 (117M params), GPT-2 (1.5B), GPT-3 (175B).
- **Masked language modeling**: Training technique that masks part of the input for the model to predict.
- **Transfer learning**: Pretraining on a general task (language modeling), then fine-tuning for a specific task.
- **Representation model**: Encoder-only model focused on representing language (e.g., embeddings); typically does not generate text.
- **Generative model**: Decoder-only model focused on generating text; typically not trained to produce embeddings.
- **LLM (large language model)**: Primarily generative decoder-only models, but the book uses a broad definition that also includes representation models and models under 1B parameters that don't generate text.
- **Foundation model / base model**: An open source base model resulting from pretraining; can be fine-tuned for specific tasks.
- **Completion model**: A generative model that takes in input text and attempts to autocomplete it; instruct/chat models answer questions rather than merely complete text.
- **Context length / context window**: The maximum number of tokens the model can process. Grows as new tokens are generated (autoregressive).
- **Prompt**: The user query fed to an instruct/chat model.
- **Pretraining**: The first (costly) training step on vast internet text to learn grammar, context, language patterns — predicting the next word. Produces a foundation/base model that generally doesn't follow instructions.
- **Fine-tuning / post-training**: The second step — further training on a narrower task (classification, instruction following). Much cheaper than pretraining.
- **Quantization**: A type of compression (details in Chapters 7 & 12).
- **API (application programming interface)**: The interface used to communicate with closed-source LLMs.
- **Proprietary (closed source) model**: Weights/architecture not shared (e.g., GPT-4, Claude).
- **Open model**: Weights and architecture shared (e.g., Cohere Command R, Mistral, Microsoft Phi, Meta Llama). License terms vary.
- **Backend packages**: No-GUI packages for loading/running LLMs (llama.cpp, LangChain, Hugging Face Transformers).
- **Token**: The atom of input; the tokenizer splits text into tokens before the model processes them.
- **Tokenizer**: The component in charge of splitting input text into tokens; loaded alongside the generative model.
- **VRAM (video random-access memory)**: GPU memory; more is generally better; some models cannot run without sufficient VRAM.
- **GPU-poor**: Term for those without a powerful GPU, who must rely on limited compute.
- **Hugging Face Hub**: Main source for finding/downloading LLMs; 800,000+ models at time of writing.

### Core Concepts & Frameworks
- **Chapter's framing questions**: What is Language AI? What are LLMs? What are common use cases/applications? How can we use LLMs ourselves?
- **History of Language AI (Figure 1-1)**: Text is unstructured and loses meaning as zeros/ones, so the field has focused on structured representation.
- **Bag-of-words pipeline**: tokenization (split on whitespace) → build vocabulary (unique words) → count word frequencies → vector representation. Classic but not obsolete (used in Ch 5 as a complement).
- **word2vec**: neural-network-based; learns from word co-occurrence/neighbors; words with similar neighbors get closer embeddings. Embedding properties are often obscure and not humanly identifiable, but together they translate language into computer language.
- **Embedding similarity**: distance metrics measure semantic closeness; compressing to 2D shows similar words cluster.
- **Types of embeddings**: word, sentence, document-level abstractions (bag-of-words = document-level; word2vec = word-level).
- **RNN era**: encoder-decoder translation; context embedding represented the whole input (bad for long sentences); autoregressive generation; sequential processing prevented parallelization.
- **Attention (2014)**: decoder attends to relevant input words (e.g., "llamas"→"lama's"); hidden states of all input words passed instead of a single context embedding.
- **Transformer (2017)**: removes recurrence; parallelizable; stacked encoders/decoders; encoder = self-attention + feedforward; decoder = + attention over encoder output, masked self-attention; autoregressive.
- **BERT (2018)**: encoder-only, 12 encoders, [CLS] token, masked language modeling, transfer learning (pretrain on Wikipedia → fine-tune for classification). Also a feature-extraction machine.
- **GPT family**: decoder-only; GPT-1 117M params (7,000 books + Common Crawl); GPT-2 1.5B; GPT-3 175B. Bigger → more capable.
- **The Year of Generative AI (2023)**: ChatGPT (product) powered by GPT-3.5 (model); open source + proprietary foundation models proliferated.
- **Alternative architectures**: Mamba and RWKV attempt Transformer-level performance with advantages like larger context windows or faster inference.
- **Book's "moving" LLM definition**: "Large" is arbitrary; the book includes models under 1B params and non-generative models; a model's name doesn't change its behavior.
- **Two-step training paradigm**: Traditional ML = one-step (train for specific task). LLM = pretraining (language modeling, expensive) + fine-tuning (adapt to task/behavior). Any model that went through pretraining is a "pretrained model" (including fine-tuned ones).
- **Common applications**: (1) sentiment classification (supervised; encoder- or decoder-only; Ch 4/11); (2) topic finding in tickets (unsupervised; encoder-only classify + decoder-only label topics; Ch 5); (3) retrieval/inspection of documents via semantic search (Ch 8) + custom embedding models (Ch 12); (4) LLM chatbot with external resources (prompt engineering Ch 6, RAG Ch 8, fine-tuning Ch 12); (5) multimodal recipe-from-fridge-photo (image input; Ch 9).
- **Responsible LLM development**: bias/fairness (data biases reproduced/amplified), transparency/accountability (human-vs-LLM ambiguity, medical devices), harmful content (confident but incorrect, fake news), intellectual property (who owns output?), regulation (EU AI Act).
- **Compute reality**: VRAM is the key GPU resource; Llama 2 trained on A100-80GB; at $1.50/hr → costs exceeding $5,000,000; no single rule for VRAM needs. The book is "for the GPU-poor": Google Colab free tier gives T4 GPU with 16GB VRAM (the suggested minimum).
- **Proprietary vs open models**: Proprietary (API-based) — no GPU needed, more performant, but paid, no self fine-tuning, data shared with provider. Open — full control, transparency, communities (Hugging Face), but needs powerful hardware + knowledge.
- **Open source frameworks**: focus on backend packages (llama.cpp, LangChain, Hugging Face Transformers); GUI local UIs: text-generation-webui, KoboldCpp, LM Studio.
- **First text generation**: load two components (model + tokenizer); Hugging Face Hub (800k+ models); Phi-3-mini = 3.8B params, MIT license, runs <8GB VRAM (quantized <6GB). Model path "microsoft/Phi-3-mini-4k-instruct". Use `transformers.pipeline` with params: `return_full_text=False` (only output), `max_new_tokens=500` (limit output length), `do_sample=False` (greedy: most probable token).

### Important Numbers / Stats / Tokens
- GPT-2: first software able to write human-indistinguishable articles; ChatGPT reached 1M users in 5 days, 100M in 2 months (p.1).
- Bag-of-words: first mentioned ~1950s, popular ~2000s (p.4).
- word2vec released 2013, trained on vast data like entire Wikipedia (p.6).
- Attention introduced 2014 (Bahdanau et al., "Neural machine translation by jointly learning to align and translate") (p.11).
- Transformer: "Attention is all you need", 2017 (Vaswani et al.) (p.13).
- BERT: 2018 (Devlin et al.) encoder-only, 12 encoders in base (p.16); GPT-1 2018 decoder-only (p.18).
- GPT-1: 7,000 books + Common Crawl corpus; 117M parameters (p.18).
- GPT-2: 1.5 billion parameters (2019) (p.18).
- GPT-3: 175 billion parameters (2020, few-shot learners) (p.18).
- 2023 = "The Year of Generative AI"; ChatGPT powered by GPT-3.5; GPT-4 variants followed (p.21).
- Mamba (2023), RWKV (2023) — alternative architectures (p.22).
- Llama 2: 2 trillion tokens training dataset (p.24).
- Llama 2 training: 3,311,616 GPU hours; at $1.50/hr → >$5,000,000 (p.27).
- Google Colab free tier: T4 GPU, 16 GB VRAM (suggested minimum) (p.27).
- Hugging Face Hub: 800,000+ models (p.30).
- Phi-3-mini: 3.8 billion parameters; runs on <8GB VRAM; quantized <6GB; MIT license (p.32).

### Algorithms & Formulæ
- **Bag-of-words algorithm** (step-by-step):
  1. Tokenize each sentence (split on whitespace).
  2. Build vocabulary = all unique tokens across sentences.
  3. For each sentence, count occurrences of each vocabulary word.
  4. Output = vector of counts (vector representation).
- **word2vec training** (step-by-step):
  1. Assign every vocabulary word a random vector embedding (e.g., 50 values).
  2. In each training step, take pairs of words from training data.
  3. Model predicts whether the pair are likely neighbors in a sentence.
  4. Update embeddings toward the ground truth; words with similar neighbors end up closer.
- **Transformer encoder-decoder flow** (step-by-step):
  1. Input flows through stacked encoder blocks (self-attention → feedforward).
  2. Decoder self-attention layer masks future positions (prevents look-ahead).
  3. Decoder cross-attention layer attends to encoder output.
  4. Decoder generates tokens autoregressively (each output consumes all prior).
- **Two-step LLM training paradigm** (vs traditional ML):
  - Traditional ML: [train on task] (one step).
  - LLM: [pretrain on vast corpus → foundation/base model] → [fine-tune on task → task-specialized model].
- **First-text pipeline (code flow)**:
  1. `AutoModelForCausalLM.from_pretrained("microsoft/Phi-3-mini-4k-instruct", device_map="cuda", torch_dtype="auto", trust_remote_code=True)` — load model.
  2. `AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")` — load tokenizer.
  3. `pipeline("text-generation", model=model, tokenizer=tokenizer, return_full_text=False, max_new_tokens=500, do_sample=False)` — create generator.
  4. `messages = [{"role": "user", "content": "Create a funny joke about chickens."}]` — format prompt as chat messages.
  5. `output = generator(messages); print(output[0]["generated_text"])` — generate.

### Diagrams / Visuals
- **Figure 1-1** — A peek into the history of Language AI (timeline of methods/models).
- **Figure 1-2** — Language AI capable of many tasks by processing textual input (translation, generation, summarization, etc.).
- **Figure 1-3** — Each sentence split into words (tokens) by splitting on whitespace.
- **Figure 1-4** — Vocabulary created by retaining all unique words across both sentences.
- **Figure 1-5** — Bag-of-words created by counting individual words → vector representations.
- **Figure 1-6** — Neural network = interconnected layers of nodes; each connection a linear equation (weights/parameters).
- **Figure 1-7** — Neural network trained to predict if two words are neighbors; embeddings updated to ground truth.
- **Figure 1-8** — Embedding values represent properties; dimensions as (oversimplified) concepts.
- **Figure 1-9** — Similar words are close in dimensional space (embedding similarity).
- **Figure 1-10** — Embeddings can be created for different input types (word, sentence, document).
- **Figure 1-11** — Two RNNs (decoder + encoder) translate English→Dutch ("I love llamas" → "Ik hou van lama's").
- **Figure 1-12** — Each previous output token used as input for the next (autoregressive).
- **Figure 1-13** — word2vec embeddings → context embedding representing the whole sequence.
- **Figure 1-14** — Attention lets a model attend to parts of sequences with more/less relevance.
- **Figure 1-15** — Decoder attention focuses on "llamas" before generating "lama's".
- **Figure 1-16** — Transformer = stacked encoder and decoder blocks; input flows through each.
- **Figure 1-17** — Encoder block = self-attention to generate intermediate representations.
- **Figure 1-18** — Self-attention attends to all parts of the input sequence (looks forward and back).
- **Figure 1-19** — Decoder has an additional attention layer over the encoder output.
- **Figure 1-20** — Decoder self-attention masks future positions to prevent "looking into the future".
- **Figure 1-21** — BERT base architecture with 12 encoders.
- **Figure 1-22** — BERT trained with masked language modeling.
- **Figure 1-23** — After pretraining, fine-tune BERT for specific tasks.
- **Figure 1-24** — GPT-1 decoder-only architecture (removes encoder-attention block).
- **Figure 1-25** — GPT models quickly grew in size (117M → 1.5B → 175B).
- **Figure 1-26** — Generative LLMs complete input; instruct models answer questions.
- **Figure 1-27** — Context length = maximum context an LLM can handle.
- **Figure 1-28** — A comprehensive view of the Year of Generative AI (many models, both open and proprietary).
- **Figure 1-29** — Traditional ML = single-step training for a target task.
- **Figure 1-30** — LLM training = multistep approach (pretraining + fine-tuning).
- **Figure 1-31** — Closed-source LLMs accessed via API; details not shared.
- **Figure 1-32** — Open-source LLMs used directly by the user; details shared.

### Common Exam Traps
- **ChatGPT = product, not model**: ChatGPT is the product; originally powered by GPT-3.5. Do not equate the two.
- **LLM ≠ only generative**: The book explicitly counts representation models (encoder-only, e.g., BERT) and models under 1B params as "large language models". Encoder-only vs decoder-only is about architecture focus (representation vs generation), and each model can do tasks typical of the other family.
- **"Large" is arbitrary**: A definition that changes with new releases; excluding capable models based on size is the book's stated objection.
- **Bag-of-words ignores semantics**: It treats text as a literal bag of words — no meaning; word2vec fixes this.
- **Attention vs self-attention**: Attention (2014) was for RNN decoder attending to input; self-attention (Transformer) attends within a single sequence.
- **The Transformer removed recurrence**: It's solely attention-based; RNNs' sequential processing prevented parallelization; the Transformer trains in parallel.
- **Context length grows autoregressively**: The current context length increases as new tokens are generated.
- **Autoregressive = consume all previous output**: Each generated word is fed back as input.
- **[CLS] token**: BERT's special classification token used as the whole-input representation for fine-tuning.
- **Pretrained model includes fine-tuned**: Any model that went through pretraining is a pretrained model, fine-tuned models included.
- **Tokenization isn't always whitespace**: Mandarin has no spaces between words — a key limitation of naive splitting.
- **VRAM, not "GPU" alone**: VRAM is the specific constraint; more is generally better; some models can't run without enough.
- **do_sample=False = greedy**: The model always picks the most probable next token (no creativity).
- **Phi-3-mini license**: MIT — commercial use allowed without constraints.
- **Open model ≠ necessarily "open source"**: Permissive-commercial licenses may restrict commercial use; training data and code are seldom shared.
- **GPU cost arithmetic**: Llama 2 training exceeding $5M assumes $1.50/hr × 3,311,616 GPU hours.

### Chapter Summary
Chapter 1 positions LLMs as the centerpiece of Language AI, the field of building systems that understand, process, and generate human language. The history runs from bag-of-words (counts, no meaning), to word2vec (neural embeddings capturing meaning), to RNNs with attention (context-aware but sequential), to the 2017 Transformer (pure attention, parallelizable), and finally to the two modern families: encoder-only representation models like BERT (masked language modeling, [CLS] token, transfer learning) and decoder-only generative models like the GPT series (autoregressive completion, scaling from 117M to 175B parameters). The book then adopts an explicit, broad definition of LLM that includes under-1B and non-generative models, and frames LLM training as a two-step paradigm: costly pretraining on vast internet text followed by cheap task-specific fine-tuning.

The second half covers the practical landscape: applications (classification, topic finding, semantic search, chatbots with tools, multimodal reasoning), responsible development (bias, transparency, harmful content, IP, EU AI Act regulation), the compute/VRAM reality (the "GPU-poor"; Colab T4 with 16GB VRAM), proprietary vs open models and their trade-offs, and open-source backend frameworks. The chapter ends by generating the book's first text with Phi-3-mini via Hugging Face Transformers: loading model + tokenizer, building a text-generation pipeline with `return_full_text=False`, `max_new_tokens=500`, and `do_sample=False`, formatting a chat-style prompt, and printing the model's joke about chickens.

### Confidence Check
- **Sure**: Bag-of-words pipeline (tokenize → vocabulary → count → vector); BERT = encoder-only w/ [CLS] + masked LM; GPT decoder-only scaling 117M/1.5B/175B; two-step training (pretrain + fine-tune); Phi-3-mini specs & code params; attention vs self-attention distinction.
- **Uncertain**: The exact page-level figure numbers for Figures 1-26 to 1-32 (text extraction gives figures without precise page anchors); whether "autoregressive" terminology also applies to the decoder-only generation loop vs. only RNN step; minor — the precise scope of what "Chapter 8" (retrieval/RAG) vs "Chapter 12" (fine-tuning) covers in the source edition's TOC.

---

## §2. Code & Pseudocode Breakdown
*(No separate code file — this chapter's code is analyzed within the bundle below.)*

### Code Block 1: Loading the model and tokenizer
```python
from transformers import AutoModelForCausalLM, AutoTokenizer

# Load model and tokenizer
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
```
- **Explanation**: Two components are loaded — the generative model itself and its tokenizer. `AutoModelForCausalLM` loads a causal (decoder-only) language model; `AutoTokenizer` loads the tokenizer used to split input into tokens. `device_map="cuda"` places the model on the NVIDIA GPU; `torch_dtype="auto"` selects the appropriate precision; `trust_remote_code=True` allows custom model code. Downloading can take a few minutes.
- **Fits the architecture**: The tokenizer converts raw text → token IDs; the model converts token IDs → next-token predictions. Both components are essential for text generation.

### Code Block 2: Creating a text-generation pipeline
```python
from transformers import pipeline

# Create a pipeline
generator = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=False,
    max_new_tokens=500,
    do_sample=False
)
```
- **Explanation**: `transformers.pipeline` wraps model + tokenizer + generation into a single callable. `return_full_text=False` returns only the generated output, not the prompt. `max_new_tokens=500` caps output length (prevents runaway generation to the context window). `do_sample=False` selects the most probable token each step (greedy decoding).
- **Fits the architecture**: This is the autoregressive completion step — take input, predict next token, feed it back, repeat up to `max_new_tokens`.

### Code Block 3: The prompt and first generation
```python
# The prompt (user input / query)
messages = [
    {"role": "user", "content": "Create a funny joke about chickens."}
]

# Generate output
output = generator(messages)
print(output[0]["generated_text"])
```
- **Explanation**: The prompt is formatted as a list of chat-message dictionaries, each with a `role` (here `user`) and `content`. The generator returns a list of outputs; we print the generated text. Result: *"Why don't chickens like to go to the gym? Because they can't crack the egg-sistence of it!"*
- **Fits the architecture**: Chat-format prompting is how instruct/chat models are driven — the model completes the conversation by generating the assistant turn.

*(If no further code blocks exist in this chapter, the above three cover all code in Chapter 1.)*

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*
