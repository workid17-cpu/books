# 📘 Chapter 1 Flashcards: An Introduction to Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 1

---

## Part 1: Terms → Definitions

**Q:** What is Language AI?
**A:** A subfield of AI focused on developing technologies capable of understanding, processing, and generating human language; often used interchangeably with natural language processing (NLP).

**Q:** What is the book's inclusive definition of "large language model"?
**A:** Primarily generative decoder-only models, but the book also counts representation (encoder-only) models and models under 1 billion parameters that don't generate text and run on consumer hardware.

**Q:** Why does the book avoid a strict size-based definition of "LLM"?
**A:** Because "large" is arbitrary and evolves; excluding capable models based on size is misguided — the name doesn't change how a model behaves.

**Q:** What is bag-of-words?
**A:** A technique for representing unstructured text by tokenizing, building a vocabulary of unique words, and counting word frequencies to produce a vector representation.

**Q:** What is the main flaw of bag-of-words?
**A:** It ignores the semantic meaning of text — it treats language as a literal bag of words.

**Q:** What is tokenization?
**A:** The process of splitting text into individual words or subwords (tokens); the most common method splits on whitespace.

**Q:** Why is whitespace tokenization insufficient for all languages?
**A:** Some languages, like Mandarin, don't have whitespaces around individual words.

**Q:** What is an embedding?
**A:** A vector representation of data that attempts to capture its meaning.

**Q:** What is word2vec?
**A:** A 2013 method that learns semantic word representations (embeddings) using neural networks trained on vast text like Wikipedia, based on which words appear near each other.

**Q:** How does word2vec decide that two words are similar?
**A:** If two words tend to have the same neighbors in sentences, their embeddings are placed closer together.

**Q:** Are the "properties" (dimensions) of embeddings humanly interpretable?
**A:** No — they are often obscure and seldom relate to a single humanly identifiable concept, but together they let computers work with language.

**Q:** What is a vector / vector representation?
**A:** A numerical representation of text (e.g., bag-of-words produces a count vector).

**Q:** What are model parameters?
**A:** The weights on connections between nodes in a neural network — numerical values that represent the model's understanding of language.

**Q:** What is a neural network?
**A:** Interconnected layers of nodes that process information; each connection carries a weight (parameter).

**Q:** What is a recurrent neural network (RNN)?
**A:** A neural network variant that models sequences as an additional input; used for encoding input and decoding output.

**Q:** What does "autoregressive" mean?
**A:** When generating the next word, the model consumes all previously generated words as input.

**Q:** What is attention?
**A:** A 2014 mechanism that lets a model focus on (attend to) the parts of the input sequence relevant to one another and amplify their signal.

**Q:** What problem did attention solve in RNN translation?
**A:** A single context embedding poorly represented long sentences; attention passes the hidden states of all input words to the decoder instead.

**Q:** What is self-attention?
**A:** Attention to different positions within a single sequence, letting the model look forward and back in one sequence in one go.

**Q:** What is the Transformer?
**A:** The 2017 "Attention is All You Need" architecture based solely on attention, with stacked encoder/decoder blocks, trainable in parallel.

**Q:** Why could the Transformer be trained in parallel while RNNs couldn't?
**A:** RNNs process sequences sequentially (precluding parallelization); the Transformer's attention-only design allows parallel training.

**Q:** What does an encoder block contain?
**A:** Self-attention followed by a feedforward neural network.

**Q:** What does the decoder block add beyond the encoder block?
**A:** An additional attention layer over the encoder output; its self-attention layer masks future positions.

**Q:** Why does the decoder's self-attention mask future positions?
**A:** To prevent "looking into the future" / leaking information when generating output.

**Q:** What is BERT?
**A:** Bidirectional Encoder Representations from Transformers (2018) — an encoder-only architecture focused on representing language.

**Q:** What is the [CLS] token in BERT?
**A:** A classification token added to the input, used as the representation for the entire input, typically for fine-tuning.

**Q:** What is masked language modeling?
**A:** A training technique that masks part of the input for the model to predict.

**Q:** What is transfer learning in the LLM context?
**A:** Pretraining a model on a general task (language modeling), then fine-tuning it for a specific task.

**Q:** Why is fine-tuning cheaper than pretraining?
**A:** Most training is already done by pretraining; fine-tuning requires less data and compute.

**Q:** What is a representation model?
**A:** An encoder-only model that focuses on representing language (e.g., embeddings) and typically does not generate text.

**Q:** What is a generative model?
**A:** A decoder-only model that focuses on generating text and typically isn't trained to produce embeddings.

**Q:** How does the book visually distinguish the two model families?
**A:** Representation models are teal with a vector icon; generative models are pink with a chat icon.

**Q:** What is GPT?
**A:** Generative Pre-trained Transformer (2018), a decoder-only architecture for generative tasks; GPT-1 → GPT-2 → GPT-3.

**Q:** What was GPT-1 trained on and how big was it?
**A:** A corpus of 7,000 books and Common Crawl; 117 million parameters.

**Q:** What is Common Crawl?
**A:** A large dataset of web pages used as training data.

**Q:** What were the parameter counts of GPT-1, GPT-2, and GPT-3?
**A:** GPT-1: 117M; GPT-2: 1.5B; GPT-3: 175B.

**Q:** What is a completion model?
**A:** A generative model that takes in text and attempts to autocomplete it; instruct/chat models answer questions instead of just completing.

**Q:** What is the context length / context window?
**A:** The maximum number of tokens the model can process; it grows as new tokens are generated due to autoregressive generation.

**Q:** What is a foundation model / base model?
**A:** An open source base model produced by pretraining that can be fine-tuned for specific tasks like following instructions.

**Q:** Why is 2023 called "The Year of Generative AI"?
**A:** Because of the release, adoption, and media coverage of ChatGPT (GPT-3.5) and the fast proliferation of open and proprietary LLMs.

**Q:** What is the distinction between ChatGPT and GPT-3.5/GPT-4?
**A:** ChatGPT is the product; GPT-3.5/GPT-4 are the underlying models (ChatGPT was first powered by GPT-3.5).

**Q:** What are Mamba and RWKV?
**A:** Alternative (non-Transformer) architectures aiming for Transformer-level performance with advantages like larger context windows or faster inference.

**Q:** What is pretraining?
**A:** The first, compute-heavy training step on a vast corpus of internet text, learning grammar, context, and language patterns; produces a base/foundation model.

**Q:** What is fine-tuning / post-training?
**A:** The second step — further training a pretrained model on a narrower task to adapt it (e.g., classification or instruction following).

**Q:** How many tokens was Llama 2 trained on?
**A:** 2 trillion tokens.

**Q:** What is a "pretrained model" per the book's definition?
**A:** Any model that went through the pretraining step — including fine-tuned models.

**Q:** What is semantic search?
**A:** A retrieval method using embeddings to find relevant documents for an LLM to use (Chapter 8).

**Q:** What is retrieval-augmented generation (RAG)?
**A:** Combining an LLM with external retrieved documents/resources to ground its answers (Chapter 8).

**Q:** What is a multimodal LLM task (per this chapter)?
**A:** The LLM takes non-text input like an image and reasons about it (e.g., writing recipes from a fridge photo; Chapter 9).

**Q:** What is a proprietary (closed source) LLM?
**A:** A model whose weights and architecture are not shared with the public (e.g., GPT-4, Claude); accessed via API.

**Q:** What is an open LLM?
**A:** A model whose weights and architecture are shared publicly (e.g., Command R, Mistral, Phi, Llama), with varying license terms.

**Q:** Why can't all "open" models be considered open source?
**A:** Some have permissive commercial licenses restricting commercial use; training data and source code are seldom shared.

**Q:** What is an API?
**A:** Application programming interface — the interface used to communicate with an LLM without directly accessing it.

**Q:** What is VRAM?
**A:** Video random-access memory — GPU memory; more is generally better and some models can't run without enough.

**Q:** What is the "GPU-poor"?
**A:** The term for people without a powerful GPU who must work with limited compute.

**Q:** What GPU does a free Google Colab instance provide at the time of writing?
**A:** A T4 GPU with 16 GB VRAM (the book's suggested minimum).

**Q:** How much did training the Llama 2 family cost in the book's estimate?
**A:** Over $5,000,000 (3,311,616 GPU hours × ~$1.50/hr).

**Q:** What is quantization?
**A:** A type of model compression (covered in Chapters 7 and 12) that reduces VRAM requirements.

**Q:** What are backend packages?
**A:** No-GUI packages for loading and running LLMs efficiently, e.g., llama.cpp, LangChain, Hugging Face Transformers.

**Q:** What are text-generation-webui, KoboldCpp, and LM Studio?
**A:** Frameworks providing a ChatGPT-like GUI interface for local LLMs.

**Q:** What is the Hugging Face Hub?
**A:** The main source for finding and downloading LLMs; 800,000+ models at the time of writing.

**Q:** What is Phi-3-mini?
**A:** The book's main generative model: 3.8 billion parameters, MIT license, runs on <8 GB VRAM (<6 GB when quantized).

**Q:** Which two components are loaded when using an LLM?
**A:** The generative model itself and its underlying tokenizer.

**Q:** What does the tokenizer do?
**A:** Splits input text into tokens before feeding them to the generative model.

**Q:** What is the Hugging Face path used for the book's model?
**A:** "microsoft/Phi-3-mini-4k-instruct".

**Q:** What does `return_full_text=False` do in the pipeline?
**A:** Returns only the model's generated output, not the prompt.

**Q:** What does `max_new_tokens=500` do?
**A:** Caps the maximum number of generated tokens to prevent long, unwieldy output.

**Q:** What does `do_sample=False` do?
**A:** Disables sampling — the model always selects the most probable next token (greedy decoding).

**Q:** What is the European AI Act?
**A:** A regulation governing the development and deployment of foundation models including LLMs.

---

## Part 2: Short Answer

**Q:** Name the four questions the chapter sets out to answer.
**A:** (1) What is Language AI? (2) What are large language models? (3) What are the common use cases and applications of LLMs? (4) How can we use LLMs ourselves?

**Q:** Why is text hard for computers, per the chapter?
**A:** Text is unstructured and loses its meaning when represented as individual characters (zeros and ones), so the field has focused on structured representation.

**Q:** Describe the bag-of-words pipeline step by step.
**A:** Tokenize each sentence (split on whitespace) → build a vocabulary of all unique words → count how often each vocabulary word appears in each sentence → produce a count vector (vector representation).

**Q:** Why did the field move from bag-of-words to word2vec?
**A:** Bag-of-words ignores meaning; word2vec captures semantic meaning in embeddings learned from word co-occurrence.

**Q:** Explain how attention improved RNN translation.
**A:** Instead of passing a single context embedding to the decoder, attention passes the hidden states of all input words, letting the decoder focus on the most relevant input words (e.g., "llamas" when generating "lama's").

**Q:** Why is the Transformer considered a breakthrough for training?
**A:** It removed recurrence, relying solely on attention, so it could be trained in parallel — vastly speeding up training.

**Q:** Contrast BERT and GPT architectures.
**A:** BERT is encoder-only (representation), uses masked language modeling and the [CLS] token; GPT is decoder-only (generation), autoregressive, and trained on text completion.

**Q:** What does "the current context length will increase as new tokens are generated" mean?
**A:** Because generation is autoregressive, each newly generated token is added to the input, so the context in use grows during a session.

**Q:** How does the book define a "pretrained model"?
**A:** Any model that has gone through the pretraining step — including fine-tuned models.

**Q:** Give three examples of LLM applications from the chapter.
**A:** (1) Sentiment classification of customer reviews (supervised); (2) finding common topics in ticket issues (unsupervised); (3) document retrieval/inspection via semantic search. (Also: chatbots with tools/docs; multimodal recipe generation from a fridge photo.)

**Q:** Why is supervised vs unsupervised classification relevant in the applications section?
**A:** Sentiment detection has predefined labels (supervised); finding topics in tickets has no predefined labels (unsupervised), so encoder-only models cluster while decoder-only models label the topics.

**Q:** Name the six responsible-LLM concerns listed.
**A:** Bias and fairness; transparency and accountability; generating harmful content; intellectual property; regulation (EU AI Act). (Plus the general call for ethical use.)

**Q:** Why is "your data is shared with the provider" a problem for proprietary models?
**A:** It's not desirable for sensitive use cases, such as sharing patient data.

**Q:** Why do the authors prefer open source models?
**A:** The freedom to experiment, explore inner workings, and run locally provides more benefits than proprietary LLMs.

**Q:** Why does VRAM matter so much for running models?
**A:** Some models simply cannot be used at all without sufficient VRAM; more VRAM is generally better.

**Q:** What determines how much VRAM a model needs?
**A:** Architecture and size, compression technique (e.g., quantization), context size, and the backend used to run it.

**Q:** What is the benefit of `transformers.pipeline`?
**A:** It encapsulates the model, tokenizer, and text-generation process into a single callable function.

**Q:** How is a chat prompt formatted in the book's example?
**A:** As a list of dictionaries, each with a `role` (e.g., "user") and a `content` key defining the prompt.

**Q:** Why might a model "continue generating output until they reach their context window"?
**A:** Some models keep generating text indefinitely; `max_new_tokens` caps the output length.

**Q:** What happens with `do_sample=False` vs sampling?
**A:** With `do_sample=False` the model always picks the most probable token (greedy); sampling strategies (Chapter 6) invoke creativity.

---

## Part 3: Fill-in-the-Blank

**Q:** The first software able to write articles indistinguishable from human writing was ______.
**A:** GPT-2.

**Q:** ChatGPT reached one million users in ______ and one hundred million in ______.
**A:** Five days; two months.

**Q:** ______ is the process of splitting sentences into individual words or subwords.
**A:** Tokenization.

**Q:** Bag-of-words was first mentioned around the ______ but became popular around the ______.
**A:** 1950s; 2000s.

**Q:** word2vec was released in ______ and trains on data like the entirety of ______.
**A:** 2013; Wikipedia.

**Q:** The connections between nodes in a neural network have ______, referred to as the model's ______.
**A:** Weights; parameters.

**Q:** Embeddings allow us to measure the ______ between two words using ______ metrics.
**A:** Semantic similarity; distance.

**Q:** The word "bank" illustrates that word2vec embeddings are ______ regardless of context.
**A:** Static (fixed / context-free).

**Q:** RNNs are used for two tasks: ______ (representing an input sentence) and ______ (generating an output sentence).
**A:** Encoding; decoding.

**Q:** Attention was introduced in ______ and allows a model to ______ parts of the input.
**A:** 2014; focus on (attend to).

**Q:** The Transformer paper is titled "______".
**A:** Attention Is All You Need.

**Q:** The encoder block consists of ______ followed by a ______ neural network.
**A:** Self-attention; feedforward.

**Q:** The decoder's self-attention layer ______ future positions to prevent leaking information.
**A:** Masks.

**Q:** BERT was introduced in ______ and is an ______ architecture.
**A:** 2018; encoder-only.

**Q:** BERT uses the ______ token as the representation for the entire input.
**A:** [CLS].

**Q:** BERT is trained using ______ language modeling.
**A:** Masked.

**Q:** GPT-1 was trained on ______ books and ______, and had ______ million parameters.
**A:** 7,000; Common Crawl; 117.

**Q:** GPT-2 had ______ billion parameters; GPT-3 used ______ billion.
**A:** 1.5; 175.

**Q:** The context length is the maximum number of ______ the model can process.
**A:** Tokens.

**Q:** 2023 is called "The Year of ______".
**A:** Generative AI.

**Q:** When first released, ChatGPT was powered by ______.
**A:** GPT-3.5.

**Q:** The book counts models with fewer than ______ parameters and models that do not generate text as LLMs.
**A:** 1 billion.

**Q:** Traditional machine learning is a ______ step process; LLM training is a ______ step approach.
**A:** One; two (or multi).

**Q:** Llama 2 was trained on a dataset containing ______ trillion tokens.
**A:** 2.

**Q:** The resulting model of pretraining is often called a ______ model or ______ model.
**A:** Foundation; base.

**Q:** The two main categories of models in the book are ______ models (encoder-only) and ______ models (decoder-only).
**A:** Representation; generative.

**Q:** Meta used ______ GPUs to create the Llama 2 family.
**A:** A100-80 GB.

**Q:** At $1.50/hr, creating the Llama 2 models would cost over $______.
**A:** 5,000,000.

**Q:** A free Google Colab instance provides a ______ GPU with ______ GB VRAM.
**A:** T4; 16.

**Q:** Examples of proprietary models include OpenAI's ______ and Anthropic's ______.
**A:** GPT-4; Claude.

**Q:** Examples of open models include Cohere's ______, the ______ models, Microsoft's ______, and Meta's ______ models.
**A:** Command R; Mistral; Phi; Llama.

**Q:** Backend packages without a GUI include ______, ______, and Hugging Face ______.
**A:** llama.cpp; LangChain; Transformers.

**Q:** The book's main generative model, ______, has ______ billion parameters and is licensed under ______.
**A:** Phi-3-mini; 3.8; the MIT license.

**Q:** When you use an LLM, two models are loaded: the ______ itself and its underlying ______.
**A:** Generative model; tokenizer.

**Q:** The Hugging Face path to the book's model is "______".
**A:** microsoft/Phi-3-mini-4k-instruct.

**Q:** In the pipeline, `return_full_text=False` returns only the ______.
**A:** Output (generated text), not the prompt.

**Q:** `max_new_tokens=500` prevents ______ output.
**A:** Long and unwieldy.

**Q:** With `do_sample=False`, the model selects the next most ______ token.
**A:** Probable.

**Q:** The model's first generated joke was about ______.
**A:** Chickens.

**Q:** An example of a government regulation on LLMs is the ______.
**A:** European AI Act.
