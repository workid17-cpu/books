# AI Agents — Practice Exam (Chapter 1)
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 1 "An Introduction to Large Language Models"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which system was the first able to write articles indistinguishable from those written by humans?
   a) GPT-1
   b) ChatGPT
   c) BERT
   d) GPT-2

2. How long did it take ChatGPT to reach one hundred million active users?
   a) Two months
   b) Five days
   c) One year
   d) Two weeks

3. Language AI is best defined as:
   a) Any software that uses if-else logic
   b) A subfield of AI focused on understanding, processing, and generating human language
   c) Only the study of chatbots
   d) The hardware used to run neural networks

4. Why does the book's definition of Language AI deliberately include non-LLM technologies?
   a) Because LLMs are too expensive to discuss
   b) Because NLP excludes language tasks
   c) Because technologies like retrieval systems can give LLMs "superpowers"
   d) Because all Language AI must be multimodal

5. The first step of bag-of-words is:
   a) Tokenization (splitting text into words/subwords)
   b) Counting word frequencies
   c) Building a vocabulary
   d) Computing distance metrics

6. Why is whitespace-based tokenization problematic for Mandarin?
   a) Mandarin uses characters instead of letters
   b) Mandarin has no vowels
   c) Mandarin tokens are too long
   d) Mandarin does not have whitespaces around individual words

7. Bag-of-words creates embeddings at which level?
   a) Word level
   b) Document level
   c) Sentence level
   d) Character level

8. word2vec generates embeddings by learning from:
   a) Hand-labeled word meanings
   b) Grammar rules
   c) Which words tend to appear next to each other in sentences
   d) Image captions

9. In a neural network, the connection weights are referred to as the model's:
   a) Parameters
   b) Layers
   c) Nodes
   d) Tokens

10. In word2vec training, the model predicts whether two words are likely to be:
    a) Synonyms
    b) From the same language
    c) Of the same length
    d) Neighbors in a sentence

11. What did attention (2014) add to the RNN translation architecture?
    a) The ability to parallelize training
    b) The ability to focus on relevant parts of the input sequence and amplify their signal
    c) The removal of the decoder
    d) Static word embeddings

12. Why could the Transformer be trained in parallel while RNNs could not?
    a) It had fewer parameters
    b) It used word2vec embeddings
    c) It removed recurrence and relied solely on attention
    d) It processed one token at a time

13. An encoder block in the Transformer consists of:
    a) Self-attention followed by a feedforward neural network
    b) A recurrent layer and attention
    c) A decoder and a mask
    d) Only masked language modeling

14. The decoder's self-attention layer masks future positions in order to:
    a) Speed up training
    b) Reduce the number of parameters
    c) Improve translation quality
    d) Prevent leaking information when generating output

15. BERT is an ______ architecture focused on ______:
    a) Decoder-only; generating text
    b) Encoder-only; representing language
    c) Encoder-decoder; translating
    d) Recurrent; sequencing

16. Which token in BERT represents the entire input for fine-tuning?
    a) [SEP]
    b) [MASK]
    c) [CLS]
    d) [PAD]

17. BERT is trained using:
    a) Masked language modeling
    b) Next-word prediction only
    c) Reinforcement learning
    d) Unsupervised clustering

18. GPT-1 was trained on which data?
    a) Wikipedia only
    b) 2 trillion web tokens
    c) Image datasets
    d) 7,000 books and Common Crawl

19. The parameter counts of GPT-1, GPT-2, and GPT-3 were respectively:
    a) 1.5B, 175B, 117M
    b) 117M, 1.5B, 175B
    c) 117M, 175B, 1.5B
    d) 175B, 1.5B, 117M

20. Generative decoder-only models are also often called:
    a) Representation models
    b) Feature extractors
    c) Completion models
    d) Encoder stacks

21. The context length represents:
    a) The maximum number of tokens the model can process
    b) The number of training epochs
    c) The number of layers in the model
    d) The size of the vocabulary

22. "ChatGPT" refers to:
    a) The GPT-3.5 model architecture
    b) An open source foundation model
    c) A fine-tuning technique
    d) The product, not the underlying model

23. 2023 was dubbed "The Year of":
    a) Attention
    b) Generative AI
    c) Transformers
    d) Fine-tuning

24. Mamba and RWKV are examples of:
    a) Encoder-only BERT variants
    b) Bag-of-words techniques
    c) Alternative architectures challenging the Transformer
    d) Proprietary API models

25. Why does the book reject a strict size-based definition of "large language model"?
    a) Because smaller models are always better
    b) Because all models generate text equally well
    c) Because "large" is arbitrary and changes over time
    d) Because consumer hardware can't run large models

26. Traditional machine learning is a ______ step process, while LLM training typically uses ______ steps:
    a) One; two
    b) Two; one
    c) Zero; one
    d) Three; four

27. The first step of LLM training, ______, produces a foundation/base model:
    a) Fine-tuning
    b) Quantization
    c) Pretraining
    d) Tokenization

28. How many tokens was Llama 2 trained on?
    a) 175 billion
    b) 2 trillion
    c) 7,000
    d) 3.8 billion

29. The EU AI Act is an example of:
    a) Regulation of foundation models including LLMs
    b) A proprietary model release
    c) An open source license
    d) A GPU pricing model

30. The key GPU resource for running models is:
    a) Clock speed
    b) Core count
    c) Cache size
    d) VRAM (video random-access memory)

31. How many GPU hours went into training the Llama 2 family, and what estimated cost resulted?
    a) 16 hours; $1.50
    b) 100,000 hours; $150,000
    c) 3,311,616 hours; over $5,000,000
    d) 1 billion hours; $1 billion

32. A free Google Colab instance provides which GPU with how much VRAM?
    a) A100 with 80 GB
    b) T4 with 16 GB
    c) V100 with 32 GB
    d) RTX 3090 with 24 GB

33. Proprietary models like GPT-4 and Claude are accessed through:
    a) An API
    b) A local GPU
    c) Open weights
    d) Quantization

34. Which is NOT an example of an open model given in the chapter?
    a) Cohere's Command R
    b) Microsoft's Phi
    c) Meta's Llama
    d) Anthropic's Claude

35. The book's main generative model, Phi-3-mini, has how many parameters and under which license?
    a) 175 billion; Apache 2.0
    b) 7 billion; MIT
    c) 1.5 billion; GPL
    d) 3.8 billion; MIT

36. When you load an LLM, the two components loaded are:
    a) The model and its optimizer
    b) The model and its tokenizer
    c) The GPU and the backend
    d) The vocabulary and the embeddings

37. The Hugging Face path used for the book's model is:
    a) "meta-llama/Llama-2-7b"
    b) "openai/gpt-3.5"
    c) "bert-base-uncased"
    d) "microsoft/Phi-3-mini-4k-instruct"

38. Setting `return_full_text=False` in the pipeline means:
    a) Only the generated output is returned, not the prompt
    b) The model returns the full conversation
    c) No text is returned at all
    d) The prompt is doubled

39. With `do_sample=False`:
    a) The model picks a random token
    b) The model never finishes generating
    c) The model always selects the most probable next token
    d) The model uses a temperature of 2

40. Which is a GUI-based framework for a ChatGPT-like local interface mentioned in the chapter?
    a) llama.cpp
    b) LM Studio
    c) LangChain
    d) Hugging Face Transformers

---

## Section B: True/False (1 point each)

41. Word2vec embeddings are static — "bank" always has the same embedding regardless of context. (T/F)
42. Bag-of-words captures the semantic meaning of text. (T/F)
43. RNNs can be trained in parallel because they process sequences independently. (T/F)
44. The [CLS] token in BERT is used as the representation for the entire input. (T/F)
45. BERT-like models can be used purely as feature extraction machines without fine-tuning. (T/F)
46. The book counts representation (encoder-only) models as large language models. (T/F)
47. The context length decreases as new tokens are generated. (T/F)
48. ChatGPT is the underlying model that powers the GPT-3.5 product. (T/F)
49. Fine-tuning is generally less compute-intensive and requires less data than pretraining. (T/F)
50. Any model that goes through pretraining — including fine-tuned ones — is considered a pretrained model. (T/F)
51. All "open" models with shared weights are true open source with no usage restrictions. (T/F)
52. The GPU-poor refers to people without a powerful GPU. (T/F)
53. The more VRAM you have, the better, because some models cannot run without sufficient VRAM. (T/F)
54. `max_new_tokens=500` prevents the model from generating long, unwieldy output. (T/F)
55. LLMs cannot be used for multimodal tasks like reasoning about images. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Define Language AI and explain why the term can be used interchangeably with NLP.
57. Walk through the bag-of-words pipeline and name its key limitation.
58. Explain how word2vec learns embeddings and why words with similar neighbors end up close together.
59. What problem did attention solve in the RNN encoder-decoder architecture, and how?
60. Describe the difference between the encoder block and the decoder block in the Transformer.
61. Contrast representation models (BERT) and generative models (GPT) in terms of architecture and focus.
62. Describe the two-step LLM training paradigm and why fine-tuning saves massive resources.
63. Give three example LLM applications from the chapter and name the chapter(s) that cover them.
64. List the six responsible-LLM concerns and give one concrete example of each.
65. Compare proprietary and open LLMs: give one advantage and one disadvantage of each.

---

## Section D: Essay / Applied (5 points each)

66. **History of Language AI.** Trace the evolution from bag-of-words to word2vec to RNNs with attention to the Transformer, then to BERT and GPT. For each stage, state what problem it solved and what limitation it left for the next stage.
67. **The Transformer.** Explain the "Attention Is All You Need" architecture: encoder and decoder blocks, self-attention, masked self-attention, cross-attention over the encoder, and why the design allows parallel training. Why is this the foundation of most models in the book?
68. **Interfacing with LLMs.** Compare proprietary models (via API), open models (run locally), and the frameworks used to interact with each. Include the role of Hugging Face, backend packages, GUI tools, and the trade-offs of data control, cost, hardware, and performance.
69. **Generating your first text.** Describe the code walkthrough: loading the model and tokenizer with Hugging Face IDs, building a text-generation pipeline, and the meaning of `return_full_text`, `max_new_tokens`, and `do_sample`. Why is the tokenizer loaded alongside the model?
70. **Responsible LLM development.** Discuss bias and fairness, transparency and accountability, harmful content, intellectual property, and regulation. Why does the chapter argue these matter more as LLMs are more widely adopted?

---

## ANSWER KEY

### Section A: Multiple Choice
1. d
2. a
3. b
4. c
5. a
6. d
7. b
8. c
9. a
10. d
11. b
12. c
13. a
14. d
15. b
16. c
17. a
18. d
19. b
20. c
21. a
22. d
23. b
24. c
25. c
26. a
27. c
28. b
29. a
30. d
31. c
32. b
33. a
34. d
35. d
36. b
37. d
38. a
39. c
40. b

### Section B: True/False
41. **T** — word2vec produces static, context-free embeddings.
42. **F** — Bag-of-words ignores semantics; it treats text as a literal bag of words.
43. **F** — RNNs process sequences sequentially, which precludes parallelization.
44. **T** — The [CLS] token represents the entire input for fine-tuning.
45. **T** — BERT generates embeddings at almost every step, making it a feature extraction machine.
46. **T** — The book explicitly counts representation (encoder-only) models as LLMs.
47. **F** — The context length increases as new tokens are generated (autoregressive).
48. **F** — ChatGPT is the product; GPT-3.5 was the underlying model that powered it.
49. **T** — Fine-tuning is less compute-intensive and requires less data.
50. **T** — Any model that went through pretraining is a pretrained model, including fine-tuned ones.
51. **F** — Some open-weight models restrict commercial use; training data and code are seldom shared.
52. **T** — The GPU-poor are those without a powerful GPU.
53. **T** — More VRAM is generally better; some models can't run without sufficient VRAM.
54. **T** — `max_new_tokens` caps output length.
55. **F** — LLMs can be multimodal (e.g., reasoning about a fridge photo; Chapter 9).

### Section C: Short Answer (model answers)
56. **Language AI / NLP.** Language AI is a subfield of AI focused on technologies that understand, process, and generate human language; it's used interchangeably with NLP, and it intentionally includes non-LLM technologies like retrieval systems that empower LLMs.
57. **Bag-of-words.** Tokenize (split on whitespace) → build vocabulary of unique words → count word frequencies per sentence → produce a count vector. Limitation: it ignores the semantic meaning of text.
58. **word2vec.** Assign random embeddings; in each training step take word pairs and predict whether they are neighbors; update embeddings toward the ground truth. Words with the same neighbors end up with closer embeddings.
59. **Attention (2014).** A single context embedding poorly represented long sentences. Attention passes hidden states of all input words to the decoder and lets it focus on the most relevant input words (e.g., "llamas" when generating "lama's").
60. **Encoder vs decoder block.** Encoder = self-attention + feedforward. Decoder = same plus an additional attention layer over the encoder output; its self-attention masks future positions to prevent look-ahead.
61. **Representation vs generative.** BERT is encoder-only, focuses on representing language (embeddings), uses [CLS] + masked language modeling. GPT is decoder-only, autoregressive, focuses on generating/completing text. The distinction is about primary focus, not hard capability.
62. **Two-step paradigm.** (1) Pretraining on vast internet text → foundation/base model (costly); (2) fine-tuning/post-training on a narrower task → task-adapted model (cheap, less data). Any pretrained model is still a "pretrained model" even after fine-tuning.
63. **Applications.** (1) Sentiment classification of reviews (supervised; Ch 4/11); (2) finding topics in tickets (unsupervised; encoder-only clusters, decoder-only labels; Ch 5); (3) document retrieval via semantic search (Ch 8); also chatbots with tools/docs (Ch 6/8/12) and multimodal recipe-from-fridge-photo (Ch 9).
64. **Responsible concerns.** Bias/fairness (reproducing/amplifying training bias); transparency/accountability (human-vs-LLM ambiguity; medical devices); harmful content (confident incorrect text, fake news); intellectual property (who owns output / training-data phrases); regulation (EU AI Act).
65. **Proprietary vs open.** Proprietary: no GPU needed + more performant (advantage); but paid, no self fine-tuning, data shared with provider (disadvantage). Open: full control, transparency, communities (advantage); but needs powerful hardware and specific knowledge (disadvantage).

### Section D: Essay (grading notes)
66. **Expect** bag-of-words (counts, no meaning) → word2vec (semantic embeddings, static/context-free) → RNNs+attention (context-aware, but sequential/no parallelization) → Transformer (pure attention, parallel training) → BERT (encoder-only, [CLS], masked LM) and GPT (decoder-only, autoregressive completion, scaling to 175B).
67. **Expect** stacked encoder/decoder blocks; encoder = self-attention + feedforward; decoder = + cross-attention over encoder output, masked self-attention (no look-ahead); autoregressive generation; parallelizable training; foundation of BERT, GPT, and most models in the book.
68. **Expect** proprietary via API (GPT-4, Claude) — no GPU, more performant, paid, no fine-tuning, data shared; open models (Command R, Mistral, Phi, Llama) — control, transparency, communities, but hardware + knowledge needed; backend packages (llama.cpp, LangChain, Transformers) vs GUI tools (text-generation-webui, KoboldCpp, LM Studio); Hugging Face Hub as the main source.
69. **Expect** load model + tokenizer by HF ID ("microsoft/Phi-3-mini-4k-instruct"); `pipeline("text-generation", ...)`; `return_full_text=False` (output only), `max_new_tokens=500` (cap length), `do_sample=False` (greedy, most probable token); tokenizer splits input into tokens before the model.
70. **Expect** discussion of bias/fairness (amplification, opaque training data), transparency/accountability (human-in-the-loop, medical devices), harmful content (confident wrongness, fake news), IP (ownership of output and training-data phrases), regulation (EU AI Act); importance grows with adoption.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely the history/architecture or interfacing sections).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
