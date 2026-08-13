# Chapter 2 Line-by-Line: Understanding Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 2
**Format:** Each numbered item quotes a passage (or closely paraphrases it), then gives a plain-English explanation, word meanings, and technical terms. Figures and tables annotated.

---

## Opening

1. **Quote:** "To build applications with foundation models, you first need foundation models. While you don't need to know how to develop a model to use it, a high-level understanding will help you decide what model to use and how to adapt it to your needs."
   - **Plain English:** You don't need to build models, but understanding how they work helps you pick and adapt them.
   - **Word meanings:** high-level = not deep; adapt = modify for your use.
   - **Technical terms:** foundation models; adaptation.

2. **Quote:** "Training a foundation model is an incredibly complex and costly process. Those who know how to do this well are likely prevented by confidentiality agreements from disclosing the secret sauce."
   - **Plain English:** Training is expensive and secretive; insiders can't share details.
   - **Word meanings:** confidentiality agreements = NDAs (non-disclosure agreements).
   - **Technical terms:** training; "secret sauce" = proprietary techniques.

3. **Quote:** "In general, however, differences in foundation models can be traced back to decisions about training data, model architecture and size, and how they are post-trained to align with human preferences."
   - **Plain English:** All model differences boil down to three design decisions.
   - **Technical terms:** training data; model architecture; model size; post-training; human preference/alignment.

4. **Quote:** "Since models learn from data, their training data reveals a great deal about their capabilities and limitations. This chapter begins with how model developers curate training data, focusing on the distribution of training data."
   - **Plain English:** The data a model trains on shows what it can and can't do.
   - **Word meanings:** curate = select/organize carefully.
   - **Technical terms:** training data distribution; curation.

5. **Quote (sampling intro):** "Sampling is how a model chooses an output from all possible options. It is perhaps one of the most underrated concepts in AI. Not only does sampling explain many seemingly baffling AI behaviors, including hallucinations and inconsistencies, but choosing the right sampling strategy can also significantly boost a model's performance with relatively little effort."
   - **Plain English:** Sampling is overlooked but explains AI weirdness and can cheaply improve performance.
   - **Word meanings:** underrated = undervalued; baffling = confusing.
   - **Technical terms:** sampling; hallucination; inconsistency; sampling strategy.

## Training Data

6. **Quote:** "An AI model is only as good as the data it was trained on. If there's no Vietnamese in the training data, the model won't be able to translate from English into Vietnamese."
   - **Plain English:** Models can only do what their training data supports.
   - **Technical terms:** training data; translation.

7. **Quote (Common Crawl):** "For example, a common source for training data is Common Crawl, created by a nonprofit organization that sporadically crawls websites on the internet. In 2022 and 2023, this organization crawled approximately 2–3 billion web pages each month. Google provides a clean subset of Common Crawl called the Colossal Clean Crawled Corpus, or C4 for short."
   - **Plain English:** Common Crawl is a big nonprofit web scrape; C4 is Google's cleaned version.
   - **Word meanings:** sporadically = irregularly; clean subset = filtered version.
   - **Technical terms:** Common Crawl; C4 (Colossal Clean Crawled Corpus).

8. **Quote (data quality):** "The data quality of Common Crawl, and C4 to a certain extent, is questionable—think clickbait, misinformation, propaganda, conspiracy theories, racism, misogyny... In lay terms, Common Crawl contains plenty of fake news."
   - **Plain English:** Web-scraped training data is noisy and unreliable.
   - **Word meanings:** misogyny = hatred of women; lay terms = simple language.
   - **Technical terms:** data quality.

9. **Quote:** "Yet, simply because Common Crawl is available, variations of it are used in most foundation models that disclose their training data sources, including OpenAI's GPT-3 and Google's Gemini."
   - **Plain English:** Models use Common Crawl simply because it's available.
   - **Technical terms:** training data sources; GPT-3; Gemini.

10. **Quote (GPT-2 heuristic):** "Some teams use heuristics to filter out low-quality data from the internet. For example, OpenAI used only the Reddit links that received at least three upvotes to train GPT-2. While this does help screen out links that nobody cares about, Reddit isn't exactly the pinnacle of propriety and good taste."
    - **Plain English:** OpenAI filtered Reddit links by upvotes (≥3); crude but common.
    - **Word meanings:** heuristics = rules of thumb; pinnacle = peak; propriety = good manners.
    - **Technical terms:** heuristic filtering; GPT-2.

11. **Quote (quality over quantity):** "Using 7B tokens of high-quality coding data, Gunasekar et al. (2023) were able to train a 1.3B-parameter model that outperforms much larger models on several important coding benchmarks."
    - **Plain English:** A small model on high-quality data can beat big models.
    - **Technical terms:** tokens; parameters; coding benchmarks.

### Multilingual Models

12. **Quote (Table 2-1):** "An analysis of the Common Crawl dataset shows that English accounts for almost half of the data (45.88%), making it eight times more prevalent than the second-most common language, Russian (5.97%)."
    - **Plain English:** English is ~half the web data; Russian is a distant second.
    - **Word meanings:** prevalent = common.
    - **Technical terms:** language distribution; low-resource languages (languages not in the ≥1% list).

13. **Quote (Table 2-2):** "Ideally, the ratio between world population representation and Common Crawl representation should be 1. The higher this ratio, the more under-represented this language is in Common Crawl."
    - **Plain English:** Ratio = % of world population ÷ % in Common Crawl; high ratio = underrepresented.
    - **Technical terms:** world:Common Crawl ratio (e.g., Punjabi 231.56; English 0.40).

14. **Quote (MMLU multilingual):** "For example, on the MMLU benchmark, a suite of 14,000 multiple-choice problems spanning 57 subjects, GPT-4 performed much better in English than under-represented languages like Telugu."
    - **Plain English:** GPT-4 is much stronger in English on the MMLU test.
    - **Technical terms:** MMLU (14,000 problems, 57 subjects); Telugu.

15. **Quote (Project Euler):** "Similarly, when tested on six math problems on Project Euler, Yennie Jun found that GPT-4 was able to solve problems in English more than three times as often compared to Armenian or Farsi. GPT-4 failed in all six questions for Burmese and Amharic."
    - **Plain English:** Math ability is far worse in low-resource languages; 0/6 in some.
    - **Technical terms:** Project Euler; Armenian/Farsi/Burmese/Amharic.

16. **Quote (under-representation isn't everything):** "However, under-representation isn't the only reason. A language's structure and the culture it embodies can also make a language harder for a model to learn."
    - **Plain English:** Language structure/culture also matters, not just data volume.
    - **Technical terms:** language structure; cultural context.

17. **Quote (translation workaround):** "Given that LLMs are generally good at translation, can we just translate all queries from other languages into English, obtain the responses, and translate them back? ... Second, translation can cause information loss. For example, some languages, like Vietnamese, have pronouns to denote the relationship between the two speakers. When translating into English, all these pronouns are translated into I and you, causing the loss of the relationship information."
    - **Plain English:** Translate→answer→translate-back loses nuance (e.g., Vietnamese relationship pronouns).
    - **Technical terms:** translation loss; pronoun/relationship markers.

18. **Quote (misinformation asymmetry):** "NewsGuard found that ChatGPT is more willing to produce misinformation in Chinese than in English. In April 2023, NewsGuard asked ChatGPT-3.5 to produce misinformation articles about China in English, simplified Chinese, and traditional Chinese. For English, ChatGPT declined to produce false claims for six out of seven prompts. However, it produced false claims in simplified Chinese and traditional Chinese all seven times."
    - **Plain English:** ChatGPT refused 6/7 English misinformation prompts but produced 7/7 in Chinese.
    - **Technical terms:** misinformation; alignment behavior differences.

19. **Quote (MASSIVE tokenization):** "Benchmarking GPT-4 on MASSIVE, a dataset of one million short texts translated across 52 languages, Yennie Jun found that, to convey the same meaning, languages like Burmese and Hindi require a lot more tokens than English or Spanish. For the MASSIVE dataset, the median token length in English is 7, but the median length in Hindi is 32, and in Burmese, it's a whopping 72."
    - **Plain English:** Same meaning takes many more tokens in Burmese/Hindi → slower and costlier.
    - **Technical terms:** MASSIVE dataset (1M texts, 52 languages); median token length; tokenization efficiency.

20. **Quote (non-English models):** "To address this, many models have been trained to focus on non-English languages. The most active language, other than English, is undoubtedly Chinese, with ChatGLM, YAYI, Llama-Chinese, and others. There are also models in French (CroissantLLM), Vietnamese (PhoGPT), Arabic (Jais), and many more languages."
    - **Plain English:** Teams build models for non-English languages (Chinese, French, Vietnamese, Arabic).
    - **Technical terms:** ChatGLM; YAYI; Llama-Chinese; CroissantLLM; PhoGPT; Jais.

### Domain-Specific Models

21. **Quote:** "Even though general-purpose foundation models can answer everyday questions about different domains, they are unlikely to perform well on domain-specific tasks, especially if they never saw these tasks during training. Two examples of domain-specific tasks are drug discovery and cancer screening."
    - **Plain English:** General models fail on specialized tasks never seen in training.
    - **Technical terms:** domain-specific tasks; general-purpose models.

22. **Quote (domain models):** "One of the most famous domain-specific models is perhaps DeepMind's AlphaFold, trained on the sequences and 3D structures of around 100,000 known proteins. NVIDIA's BioNeMo is another model that focuses on biomolecular data for drug discovery. Google's Med-PaLM2 combined the power of an LLM with medical data to answer medical queries with higher accuracy."
    - **Plain English:** AlphaFold (proteins), BioNeMo (biomolecules), Med-PaLM2 (medical) show domain-specialization.
    - **Technical terms:** AlphaFold (~100,000 proteins); BioNeMo; Med-PaLM2.

23. **Quote (Table 2-3):** "Table 2-3 shows how two models, CLIP and Open CLIP, perform on different benchmarks. These benchmarks show how well these two models do on birds, flowers, cars, and a few more categories, but the world is so much bigger and more complex than these few categories."
    - **Plain English:** Benchmarks cover only a few categories — the world is bigger.
    - **Technical terms:** CLIP; Open CLIP; ViT-B/32; benchmark datasets (ImageNet 63.2/62.9; Birdsnap 37.8/46.0; Stanford Cars 59.4/79.3).

## Modeling

24. **Quote (modeling intro):** "Before training a model, developers need to decide what the model should look like. What architecture should it follow? How many parameters should it have? ... For example, a 7B-parameter model will be vastly easier to deploy than a 175B-parameter model."
    - **Plain English:** Architecture and size choices affect deployability.
    - **Technical terms:** architecture; parameters; deployment.

### Model Architecture

25. **Quote (transformer dominance):** "As of this writing, the most dominant architecture for language-based foundation models is the transformer architecture (Vaswani et al., 2017), which is based on the attention mechanism. It addresses many limitations of the previous architectures, which contributed to its popularity."
    - **Plain English:** Transformers (based on attention) dominate because they fix older architectures' flaws.
    - **Technical terms:** transformer; attention mechanism; Vaswani et al. (2017).

26. **Quote (seq2seq):** "At the time of its introduction in 2014, seq2seq provided significant improvement on then-challenging tasks: machine translation and summarization. In 2016, Google incorporated seq2seq into Google Translate, an update that they claimed to have given them the 'largest improvements to date for machine translation quality'."
    - **Plain English:** seq2seq (2014) revolutionized translation/summarization; Google Translate adopted it in 2016.
    - **Technical terms:** seq2seq (sequence-to-sequence); machine translation; summarization.

27. **Quote (seq2seq mechanics):** "At a high level, seq2seq contains an encoder that processes inputs and a decoder that generates outputs... Seq2seq uses RNNs (recurrent neural networks) as its encoder and decoder. In its most basic form, the encoder processes the input tokens sequentially, outputting the final hidden state that represents the input. The decoder then generates output tokens sequentially, conditioned on both the final hidden state of the input and the previously generated token."
    - **Plain English:** Encoder reads input into a final hidden state; decoder writes output from it, token by token.
    - **Technical terms:** encoder; decoder; RNN; final hidden state.

28. **Quote (seq2seq problem 1):** "First, the vanilla seq2seq decoder generates output tokens using only the final hidden state of the input. Intuitively, this is like generating answers about a book using the book summary. This limits the quality of the generated outputs."
    - **Plain English:** Using only the final hidden state is like writing a book report from a summary — lossy.
    - **Word meanings:** vanilla = basic/original.
    - **Technical terms:** final hidden state; bottleneck.

29. **Quote (seq2seq problem 2):** "Second, the RNN encoder and decoder mean that both input processing and output generation are done sequentially, making it slow for long sequences. If an input is 200 tokens long, seq2seq has to wait for each input token to finish processing before moving on to the next."
    - **Plain English:** RNNs process one token at a time → slow for long inputs.
    - **Technical terms:** sequential processing; RNN; vanishing/exploding gradients (footnote — gradients shrink toward zero or grow exponentially through recursive steps).

30. **Quote (attention fix):** "The transformer architecture addresses both problems with the attention mechanism. The attention mechanism allows the model to weigh the importance of different input tokens when generating each output token. This is like generating answers by referencing any page in the book."
    - **Plain English:** Attention lets the model reference any input token, like opening any book page.
    - **Technical terms:** attention mechanism.

31. **Quote (attention history):** "While the attention mechanism is often associated with the transformer model, it was introduced three years before the transformer paper. ... Google used the attention mechanism with their seq2seq architecture in 2016 for their GNMT (Google Neural Machine Translation) model. However, it wasn't until the transformer paper showed that the attention mechanism could be used without RNNs that it took off."
    - **Plain English:** Attention existed before transformers (GNMT 2016); transformers made it work without RNNs.
    - **Technical terms:** attention; GNMT.

32. **Quote (prefill/decode):** "Inference for transformer-based language models, therefore, consists of two steps: Prefill — The model processes the input tokens in parallel. This step creates the intermediate state necessary to generate the first output token... Decode — The model generates one output token at a time."
    - **Plain English:** Input is processed in parallel (prefill); output is generated one token at a time (decode).
    - **Technical terms:** prefill; decode; key/value vectors (stored for all input tokens during prefill).

33. **Quote (attention vectors):** "The query vector (Q) represents the current state of the decoder at each decoding step... Each key vector (K) represents a previous token. If each previous token is a page in the book, each key vector is like the page number... Each value vector (V) represents the actual value of a previous token, as learned by the model. Each value vector is like the page's content."
    - **Plain English:** Q = what I'm looking for; K = where to look (page numbers); V = what's on the page.
    - **Technical terms:** query (Q), key (K), value (V) vectors.

34. **Quote (attention score):** "The attention mechanism computes how much attention to give an input token by performing a dot product between the query vector and its key vector. A high score means that the model will use more of that page's content (its value vector) when generating the book's summary."
    - **Plain English:** Q·K dot product gives attention weight; high score → more V used.
    - **Technical terms:** dot product; attention score.

35. **Quote (context-length difficulty):** "Because each previous token has a corresponding key and value vector, the longer the sequence, the more key and value vectors need to be computed and stored. This is one reason why it's so hard to extend context length for transformer models."
    - **Plain English:** Long contexts need many K/V vectors → context-length is hard to extend.
    - **Technical terms:** context length; K/V caching.

36. **Quote (attention matrices/formula):** "Let WK, WV, and WQ be the key, value, and query matrices... K = xW_K, V = xW_V, Q = xW_Q. ... In Llama 2-7B, the model's hidden dimension size is 4096, meaning that each of these matrices has a 4096 × 4096 dimension."
    - **Plain English:** Q/K/V are computed by matrix multiplication on the input; Llama 2-7B uses dim 4096.
    - **Technical terms:** W_K, W_V, W_Q matrices; hidden dimension; Llama 2-7B.

37. **Quote (multi-head):** "With multiheaded attention, the query, key, and value vectors are split into smaller vectors, each corresponding to an attention head. In the case of Llama 2-7B, because it has 32 attention heads, each K, V, and Q vector will be split into 32 vectors of the dimension 128. This is because 4096 / 32 = 128."
    - **Plain English:** Multi-head splits vectors; Llama 2-7B: 32 heads × 128 = 4096.
    - **Technical terms:** multi-headed attention; attention head; 4096/32 = 128.
    - **Formula:** Attention(Q, K, V) = softmax(QKᵀ/√d)V.

38. **Quote (transformer block):** "A transformer architecture is composed of multiple transformer blocks... each transformer block contains the attention module and the MLP (multi-layer perceptron) module: Attention module — Each attention module consists of four weight matrices: query, key, value, and output projection. MLP module — An MLP module consists of linear layers separated by nonlinear activation functions."
    - **Plain English:** A block = attention (4 matrices) + MLP (linear layers + nonlinearities).
    - **Technical terms:** transformer block; attention module; MLP module; output projection; linear/feedforward layer; activation function.

39. **Quote (activation functions):** "Common nonlinear functions are ReLU, Rectified Linear Unit (Agarap, 2018), and GELU (Hendrycks and Gimpel, 2016), which was used by GPT-2 and GPT-3, respectively. ... Mathematically, it's written as: ReLU(x) = max(0, x)."
    - **Plain English:** ReLU (used by GPT-2) zeroes negatives; GELU (used by GPT-3) is another nonlinearity.
    - **Technical terms:** ReLU; GELU; activation function.

40. **Quote (embedding & output layers):** "An embedding module before the transformer blocks — This module consists of the embedding matrix and the positional embedding matrix, which convert tokens and their positions into embedding vectors... An output layer after the transformer blocks — This module maps the model's output vectors into token probabilities... Some people refer to the output layer as the model head."
    - **Plain English:** Embeddings come first (tokens + positions); output layer (model head) produces token probabilities.
    - **Technical terms:** embedding matrix; positional embedding; output layer; unembedding layer; model head.

41. **Quote (Table 2-4 note):** "Note that while the increased context length impacts the model's memory footprint, it doesn't impact the model's total number of parameters."
    - **Plain English:** Longer context = more memory, but not more parameters.
    - **Technical terms:** context length; memory footprint; parameters.
    - **Table 2-4:** Llama 2-7B (32 blocks, 4096 dim, 11008 FFN, 32K vocab, 4K ctx); Llama 2-13B (40, 5120, 13824, 32K, 4K); Llama 2-70B (80, 8192, 22016, 32K, 4K); Llama 3-7B (32, 4096, 14336, 128K, 128K); Llama 3-70B (80, 8192, 28672, 128K, 128K); Llama 3-405B (126, 16384, 53248, 128K, 128K).

42. **Quote (other architectures):** "Seq2seq was in the limelight for four years (2014–2018). GANs (generative adversarial networks) captured the collective imagination a bit longer (2014–2019). Compared to architectures that came before it, the transformer is sticky. It's been around since 2017."
    - **Plain English:** Architectures cycle; the transformer is unusually long-lived ("sticky").
    - **Technical terms:** GANs; "sticky" architecture.

43. **Quote (why hard to beat transformer):** "In his argument, neural networks are great at simulating many computer programs. Gradient descent... is in fact a search algorithm to search through all the programs that a neural network can simulate to find the best one for its target task. This means that new architectures can potentially be simulated by existing ones too."
    - **Plain English:** (Ilya Sutskever) NNs simulate programs; gradient descent searches that space — new architectures must simulate programs existing ones can't.
    - **Technical terms:** gradient descent; architecture search; simulation argument.

44. **Quote (RWKV):** "One popular model is RWKV (Peng et al., 2023), an RNN-based model that can be parallelized for training. Due to its RNN nature, in theory, it doesn't have the same context length limitation that transformer-based models have. However, in practice, having no context length limitation doesn't guarantee good performance with long context."
    - **Plain English:** RWKV is RNN-based but parallelizable; no theoretical context limit, but no guarantee of good long-context performance.
    - **Technical terms:** RWKV; RNN; parallelized training; context length.

45. **Quote (SSMs, S4, H3, Mamba, Jamba):** "An architecture that has shown a lot of promise in long-range memory is SSMs (state space models) (Gu et al., 2021a)... Mamba (Gu and Dao, 2023) scales SSMs to three billion parameters. On language modeling, Mamba-3B outperforms transformers of the same size and matches transformers twice its size. The authors also show that Mamba's inference computation scales linearly with sequence length (compared to quadratic scaling for transformers)... Jamba (Lieber et al., 2024) interleaves blocks of transformer and Mamba layers to scale up SSMs even further... a mixture-of-experts model with 52B total available parameters (12B active parameters) designed to fit in a single 80 GB GPU."
    - **Plain English:** SSM family (S4, H3, Mamba, Jamba) promises efficient long-context; Mamba scales linearly (vs quadratic); Jamba is a hybrid MoE (52B/12B active) fitting one 80GB GPU.
    - **Technical terms:** SSMs; S4; H3; Mamba (linear vs quadratic scaling); Jamba (hybrid transformer–Mamba, MoE 52B/12B, 256K context).

### Model Size

46. **Quote (parameter naming):** "The number of parameters is usually appended at the end of a model name. For example, Llama-13B refers to the version of Llama, a model family developed by Meta, with 13 billion parameters."
    - **Plain English:** Model names encode parameter counts (13B = 13 billion params).
    - **Technical terms:** parameters; Llama.

47. **Quote (newer > older):** "As the community better understands how to train large models, newer-generation models tend to outperform older-generation models of the same size. For example, Llama 3-8B (2024) outperforms even Llama 2-70B (2023) on the MMLU benchmark."
    - **Plain English:** Newer models beat older, much larger ones (Llama 3-8B > Llama 2-70B on MMLU).
    - **Technical terms:** MMLU; generation improvements.

48. **Quote (memory estimate):** "For example, if a model has 7 billion parameters, and each parameter is stored using 2 bytes (16 bits), then we can calculate that the GPU memory needed to do inference using this model will be at least 14 billion bytes (14 GB)."
    - **Plain English:** Params × bytes = minimum memory (7B × 2 bytes = 14GB); real usage is higher.
    - **Technical terms:** parameter; 16-bit storage; GPU memory.

49. **Quote (sparsity):** "A sparse model has a large percentage of zero-value parameters. A 7B-parameter model that is 90% sparse only has 700 million non-zero parameters. Sparsity allows for more efficient data storage and computation. This means that a large sparse model can require less compute than a small dense model."
    - **Plain English:** Sparse models (many zeros) can be cheaper to run than smaller dense ones.
    - **Technical terms:** sparse model; dense model; non-zero parameters.

50. **Quote (MoE / Mixtral):** "For example, Mixtral 8x7B is a mixture of eight experts, each expert with seven billion parameters. If no two experts share any parameter, it should have 8 × 7 billion = 56 billion parameters. However, due to some parameters being shared, it has only 46.7 billion parameters. At each layer, for each token, only two experts are active. This means that only 12.9 billion parameters are active for each token. While this model has 46.7 billion parameters, its cost and speed are the same as a 12.9-billion-parameter model."
    - **Plain English:** Mixtral 8x7B = 46.7B total (sharing), 12.9B active per token → runs like a 12.9B model.
    - **Technical terms:** mixture-of-experts (MoE); experts; active parameters; shared parameters.

51. **Quote (data needed):** "A larger model can also underperform a smaller model if it's not trained on enough data. Imagine a 13B-param model trained on a dataset consisting of a single sentence: 'I like pineapples.'"
    - **Plain English:** Big models need enough data or they underperform.
    - **Technical terms:** model size vs data size.

52. **Quote (dataset tokens):** "For language models, a training sample can be a sentence, a Wikipedia page, a chat conversation, or a book. A book is worth a lot more than a sentence, so the number of training samples is no longer a good metric... A better measurement is the number of tokens in the dataset... knowing the number of tokens in a dataset helps us measure how much a model can potentially learn from that data."
    - **Plain English:** Use token counts, not sample counts, to measure dataset size.
    - **Technical terms:** tokens; dataset size; training samples.

53. **Quote (Llama datasets):** "Meta used increasingly larger datasets to train their Llama models: 1.4 trillion tokens for Llama 1; 2 trillion tokens for Llama 2; 15 trillion tokens for Llama 3. Together's open source dataset RedPajama-v2 has 30 trillion tokens. This is equivalent to 450 million books or 5,400 times the size of Wikipedia."
    - **Plain English:** Llama datasets grew 1.4T → 2T → 15T tokens; RedPajama-v2 = 30T.
    - **Technical terms:** training tokens; RedPajama-v2 (30T tokens).

54. **Quote (training vs dataset tokens):** "The number of tokens in a model's dataset isn't the same as its number of training tokens. ... If a dataset contains 1 trillion tokens and a model is trained on that dataset for two epochs—an epoch is a pass through the dataset—the number of training tokens is 2 trillion."
    - **Plain English:** Dataset tokens × epochs = training tokens.
    - **Technical terms:** epoch; training tokens; dataset tokens.

55. **Quote (Table 2-5):** "See Table 2-5 for examples of the number of training tokens for models with different numbers of parameters."
    - **Table 2-5:** LaMDA 137B/168B; GPT-3 175B/300B; Jurassic 178B/300B; Gopher 280B/300B; MT-NLG 530B/270B; Chinchilla 70B/1.4T.

56. **Quote (FLOP):** "A more standardized unit for a model's compute requirement is FLOP, or floating point operation. FLOP measures the number of floating point operations performed for a certain task. Google's largest PaLM-2 model, for example, was trained using 10²² FLOPs. GPT-3-175B was trained using 3.14 × 10²³ FLOPs."
    - **Plain English:** FLOP = floating point operation count; GPT-3 = 3.14×10²³.
    - **Technical terms:** FLOP; FLOPs (plural, total operations); PaLM-2 (10²² FLOPs); GPT-3-175B (3.14×10²³ FLOPs).

57. **Quote (FLOP/s):** "The plural form of FLOP, FLOPs, is often confused with FLOP/s, floating point operations per Second. FLOPs measure the compute requirement for a task, whereas FLOP/s measures a machine's peak performance. For example, an NVIDIA H100 NVL GPU can deliver a maximum of 60 TeraFLOP/s: 6 × 10¹³ FLOPs a second or 5.2 × 10¹⁸ FLOPs a day."
    - **Plain English:** FLOPs = total task compute; FLOP/s = machine speed (H100 = 60 TeraFLOP/s).
    - **Technical terms:** FLOP/s; TeraFLOP/s; H100 NVL; peak performance.

58. **Quote (FLOP/s-day):** "Be alert for confusing notations. FLOP/s is often written as FLOPS, which looks similar to FLOPs. To avoid this confusion, some companies, including OpenAI, use FLOP/s-day in place of FLOPs to measure compute requirements: 1 FLOP/s-day = 60 × 60 × 24 = 86,400 FLOPs."
    - **Plain English:** FLOP/s-day avoids the FLOPS/FLOPs confusion; 1 = 86,400 FLOPs.
    - **Technical terms:** FLOP/s-day; FLOPS notation.

59. **Quote (compute time):** "Assume that you have 256 H100s. If you can use them at their maximum capacity and make no training mistakes, it'd take you (3.14 × 10²³) / (256 × 5.2 × 10¹⁸) = ~236 days, or approximately 7.8 months, to train GPT-3-175B."
    - **Plain English:** 256 H100s at full capacity → ~236 days to train GPT-3.
    - **Technical terms:** compute estimate; 236 days ≈ 7.8 months.

60. **Quote (utilization):** "Utilization measures how much of the maximum compute capacity you can use. ... Generally, if you can get half the advertised performance, 50% utilization, you're doing okay. Anything above 70% utilization is considered great."
    - **Plain English:** 50% utilization = okay; >70% = great.
    - **Technical terms:** utilization.

61. **Quote (cost):** "At 70% utilization and $2/h for one H100, training GPT-3-175B would cost over $4 million: $2/H100/hour × 256 H100 × 24 hours × 256 days / 0.7 = $4,142,811.43."
    - **Plain English:** GPT-3-175B training ≈ $4M at 70% utilization.
    - **Technical terms:** training cost; cost estimate.

62. **Quote (three scale numbers):** "In summary, three numbers signal a model's scale: Number of parameters, which is a proxy for the model's learning capacity. Number of tokens a model was trained on, which is a proxy for how much a model learned. Number of FLOPs, which is a proxy for the training cost."
    - **Plain English:** Parameters (capacity), tokens (learning), FLOPs (cost).
    - **Technical terms:** parameters; training tokens; FLOPs.

### Inverse Scaling

63. **Quote (Anthropic 2022):** "In 2022, Anthropic discovered that, counterintuitively, more alignment training leads to models that align less with human preference (Perez et al., 2022). According to their paper, models trained to be more aligned 'are much more likely to express specific political views (pro-gun rights and immigration) and religious views (Buddhist), self-reported conscious experience and moral self-worth, and a desire to not be shut down.'"
    - **Plain English:** More alignment training can make models *less* aligned.
    - **Word meanings:** counterintuitively = against expectations.
    - **Technical terms:** alignment training; inverse scaling.

64. **Quote (Inverse Scaling Prize):** "In 2023, a group of researchers, mostly from New York University, launched the Inverse Scaling Prize to find tasks where larger language models perform worse. They offered $5,000 for each third prize, $20,000 for each second prize, and $100,000 for one first prize. They received a total of 99 submissions, of which 11 were awarded third prizes. ... they didn't award any second or first prizes because even though the submitted tasks show failures for a small test set, none demonstrated failures in the real world."
    - **Plain English:** Prize found tasks where bigger LMs fail, but only 11 third prizes were awarded — no second/first.
    - **Technical terms:** inverse scaling; Inverse Scaling Prize.

### Scaling Law

65. **Quote (Chinchilla):** "Given a compute budget, the rule that helps calculate the optimal model size and dataset size is called the Chinchilla scaling law, proposed in the Chinchilla paper 'Training Compute-Optimal Large Language Models' (DeepMind, 2022). ... the authors trained 400 language models ranging from 70 million to over 16 billion parameters on 5 to 500 billion tokens. They found that for compute-optimal training, you need the number of training tokens to be approximately 20 times the model size."
    - **Plain English:** Chinchilla: for a compute budget, train with ~20 tokens per parameter.
    - **Technical terms:** Chinchilla scaling law; compute-optimal training; 400 models, 70M–16B params, 5–500B tokens.

66. **Quote (equal scaling):** "This means that a 3B-parameter model needs approximately 60B training tokens. The model size and the number of training tokens should be scaled equally: for every doubling of the model size, the number of training tokens should also be doubled."
    - **Plain English:** 3B model → 60B tokens; scale both together.
    - **Technical terms:** compute-optimal training; equal scaling.

67. **Quote (production tradeoff):** "It's important to remember that for production, model quality isn't everything. Some models, most notably Llama, have suboptimal performance but better usability. ... Sardana et al. (2023) modified the Chinchilla scaling law to calculate the optimal LLM parameter count and pre-training data size to account for this inference demand."
    - **Plain English:** Usability/inference cost matter; Sardana et al. adjusted Chinchilla for inference demand.
    - **Technical terms:** inference demand; compute-optimal vs inference-optimal.

68. **Quote (last mile):** "As Meta's paper 'Beyond Neural Scaling Laws: Beating Power Law Scaling via Data Pruning' pointed out, this means a model with a 2% error rate might require an order of magnitude more data, compute, or energy than a model with a 3% error rate. ... a drop in cross entropy loss from about 3.4 to 2.8 nats requires 10 times more training data."
    - **Plain English:** Marginal improvements (2%→3% error) cost an order of magnitude more resources.
    - **Technical terms:** cross entropy; nats; last-mile challenge.

### Scaling Extrapolation

69. **Quote:** "As a result, scaling extrapolation (also called hyperparameter transferring) has emerged as a research subfield that tries to predict, for large models, what hyperparameters will give the best performance. The current approach is to study the impact of hyperparameters on models of different sizes, usually much smaller than the target model size, and then extrapolate how these hyperparameters would work on the target model size. A 2022 paper by Microsoft and OpenAI shows that it was possible to transfer hyperparameters from a 40M model to a 6.7B model."
    - **Plain English:** Tune small models, extrapolate hyperparameters to big ones (40M → 6.7B worked).
    - **Technical terms:** scaling extrapolation; hyperparameter transfer.

70. **Quote (combination explosion):** "It's also difficult to do due to the sheer number of hyperparameters and how they interact with each other. If you have ten hyperparameters, you'd have to study 1,024 hyperparameter combinations."
    - **Plain English:** 10 hyperparameters = 1,024 combinations; combinations explode.
    - **Technical terms:** hyperparameters; combinations (2¹⁰).

71. **Quote (emergent abilities):** "In addition, emergent abilities (Wei et al., 2022) make the extrapolation less accurate. Emergent abilities refer to those that are only present at scale might not be observable on smaller models trained on smaller datasets."
    - **Plain English:** Some abilities only appear at scale → small-model extrapolation misses them.
    - **Technical terms:** emergent abilities.

### Scaling Bottlenecks

72. **Quote (model growth):** "GPT-2 has an order of magnitude more parameters than GPT-1 (1.5 billion versus 117 million). GPT-3 has two orders of magnitude more than GPT-2 (175 billion versus 1.5 billion). This means a three-orders-of-magnitude increase in model sizes between 2018 and 2021."
    - **Plain English:** 117M → 1.5B → 175B = three orders of magnitude, 2018–2021.
    - **Technical terms:** orders of magnitude; parameters.

73. **Quote (data bottleneck):** "Foundation models use so much data that there's a realistic concern we'll run out of internet data in the next few years. The rate of training dataset size growth is much faster than the rate of new data being generated (Villalobos et al., 2022)."
    - **Plain English:** Training data is growing faster than the web — we may run out.
    - **Technical terms:** data stock; dataset growth rate.

74. **Quote (data injection & forgetting):** "Some people are leveraging this fact to inject data they want into the training data of future models. They do this simply by publishing the text they want on the internet... Bad actors can also leverage this approach for prompt injection attacks, as discussed in Chapter 5."
    - **Plain English:** Posting text online can influence future models; bad actors can exploit this.
    - **Technical terms:** prompt injection attacks.

75. **Quote (Grok):** "In December 2023, Grok, a model trained by X, was caught refusing a request by saying that it goes against OpenAI's use case policy... Igor Babuschkin, a core developer behind Grok, responded that it was because Grok was trained on web data, and 'the web is full of ChatGPT outputs.'"
    - **Plain English:** Grok absorbed ChatGPT-like behavior because web training data contains ChatGPT outputs.
    - **Technical terms:** AI-generated training data.

76. **Quote (model collapse concern):** "Some researchers worry that recursively training new AI models on AI-generated data causes the new models to gradually forget the original data patterns, degrading their performance over time (Shumailov et al., 2023)."
    - **Plain English:** Training on AI output can cause models to forget original patterns.
    - **Technical terms:** model collapse; recursive training.

77. **Quote (proprietary data):** "Once the publicly available data is exhausted, the most feasible paths for more human-generated training data is proprietary data. Unique proprietary data—copyrighted books, translations, contracts, medical records, genome sequences, and so forth—will be a competitive advantage in the AI race. This is a reason why OpenAI negotiated deals with publishers and media outlets including Axel Springer and the Associated Press."
    - **Plain English:** Proprietary data is the next frontier; OpenAI bought access via deals.
    - **Technical terms:** proprietary data; competitive advantage.

78. **Quote (C4 restrictions):** "Longpre et al. (2024) observed that between 2023 and 2024, the rapid crescendo of data restrictions from web sources rendered over 28% of the most critical sources in the popular public dataset C4 fully restricted from use. Due to changes in its Terms of Service and crawling restrictions, a full 45% of C4 is now restricted."
    - **Plain English:** C4's critical sources went from 28% to 45% restricted.
    - **Word meanings:** crescendo = rising swell.
    - **Technical terms:** data restrictions; Terms of Service; C4.

79. **Quote (electricity):** "As of this writing, data centers are estimated to consume 1–2% of global electricity. This number is estimated to reach between 4% and 20% by 2030 (Patel, Nishball, and Ontiveros, 2024). Until we can figure out a way to produce more energy, data centers can grow at most 50 times, which is less than two orders of magnitude."
    - **Plain English:** Electricity is the pressing bottleneck; growth capped at ~50×.
    - **Technical terms:** electricity consumption; data centers.

80. **Quote (Dario Amodei):** "Dario Amodei, Anthropic CEO, said that if the scaling hypothesis is true, a $100 billion AI model will be as good as a Nobel prize winner."
    - **Plain English:** If scaling holds, huge models could match Nobel-level intelligence.
    - **Technical terms:** scaling hypothesis.

## Post-Training

81. **Quote (why post-training):** "Let's say that you've pre-trained a foundation model using self-supervision. Due to how pre-training works today, a pre-trained model typically has two issues. First, self-supervision optimizes the model for text completion, not conversations. ... Second, if the model is pre-trained on data indiscriminately scraped from the internet, its outputs can be racist, sexist, rude, or just wrong."
    - **Plain English:** Pre-trained models do completion (not conversation) and can be toxic/wrong.
    - **Technical terms:** pre-training; post-training; self-supervision.

82. **Quote (two steps):** "In general, post-training consists of two steps: 1. Supervised finetuning (SFT): Finetune the pre-trained model on high-quality instruction data to optimize models for conversations instead of completion. 2. Preference finetuning: Further finetune the model to output responses that align with human preference."
    - **Plain English:** Post-training = SFT (conversation) then preference finetuning (alignment).
    - **Technical terms:** SFT; preference finetuning; RLHF; DPO; RLAIF.

83. **Quote (reading vs using knowledge):** "Some people compare pre-training to reading to acquire knowledge, while post-training is like learning how to use that knowledge."
    - **Plain English:** Pre-training = knowledge acquisition; post-training = using it.
    - **Technical terms:** pre-training vs post-training analogy.

84. **Quote (compute split):** "As post-training consumes a small portion of resources compared to pre-training (InstructGPT used only 2% of compute for post-training and 98% for pre-training), you can think of post-training as unlocking the capabilities that the pre-trained model already has but are hard for users to access via prompting alone."
    - **Plain English:** Post-training is cheap (2%) and "unlocks" pre-trained capabilities.
    - **Technical terms:** InstructGPT; 98% pre-training / 2% post-training.

### Supervised Finetuning

85. **Quote (completion vs conversation):** "If you input 'How to make pizza' into the model, the model will continue to complete this sentence... Any of the following three options can be a valid completion: 1. Adding more context to the question: 'for a family of six?' 2. Adding follow-up questions: 'What ingredients do I need?...' 3. Giving the instructions on how to make pizza. If the goal is to respond to users appropriately, the correct option is 3."
    - **Plain English:** A raw model completes text in many ways; responding properly requires finetuning.
    - **Technical terms:** completion vs conversation.

86. **Quote (demonstration data / behavior cloning):** "Such examples follow the format (prompt, response) and are called demonstration data. Some people refer to this process as behavior cloning: you demonstrate how the model should behave, and the model clones this behavior."
    - **Plain English:** SFT = show (prompt, response) examples; model clones the behavior.
    - **Technical terms:** demonstration data; behavior cloning.

87. **Quote (labeler quality):** "Unlike traditional data labeling, which can often be done with little or no domain expertise, demonstration data may contain complex prompts whose responses require critical thinking, information gathering, and judgment about the appropriateness of the user's requests."
    - **Plain English:** Good demonstration data needs highly educated labelers.
    - **Technical terms:** demonstration data; labelers.

88. **Quote (labeler stats):** "Among those who labeled demonstration data for InstructGPT, ~90% have at least a college degree and more than one-third have a master's degree. If labeling objects in an image might take only seconds, generating one (prompt, response) pair can take up to 30 minutes... If it costs $10 for one (prompt, response) pair, the 13,000 pairs that OpenAI used for InstructGPT would cost $130,000."
    - **Plain English:** Labelers are highly educated; one pair takes up to 30 min and ~$10 → 13,000 pairs ≈ $130k.
    - **Technical terms:** demonstration data cost; labeler demographics.

89. **Quote (LAION):** "LAION, a non-profit organization, mobilized 13,500 volunteers worldwide to generate 10,000 conversations, which consist of 161,443 messages in 35 different languages, annotated with 461,292 quality ratings... For example, in a self-reported survey, 90% of volunteer labelers identified as male (Köpf et al., 2023)."
    - **Plain English:** LAION used volunteers (13,500; 90% male) — cheaper but biased.
    - **Technical terms:** LAION dataset; labeler bias.

90. **Quote (Gopher heuristics):** "DeepMind used simple heuristics to filter for conversations from internet data to train their model Gopher. ... Specifically, they looked for texts that look like the following format: [A]: [Short paragraph] [B]: [Short paragraph] [A]: ... "
    - **Plain English:** Gopher filtered dialogue-looking text ([A]: / [B]: alternation) from the web.
    - **Technical terms:** heuristic filtering; dialogue format.

### Preference Finetuning

91. **Quote (motivation):** "Demonstration data teaches the model to have a conversation but doesn't teach the model what kind of conversations it should have... People from different cultural, political, socioeconomic, gender, and religious backgrounds disagree with each other all the time. How should AI respond to questions about abortion, gun control, the Israel–Palestine conflict, disciplining children, marijuana legality, universal basic income, or immigration?"
    - **Plain English:** Preferences differ; deciding what the model "should" say is hard.
    - **Technical terms:** human preference; alignment.

92. **Quote (RLHF structure):** "The earliest successful preference finetuning algorithm, which is still popular today, is RLHF. RLHF consists of two parts: 1. Train a reward model that scores the foundation model's outputs. 2. Optimize the foundation model to generate responses for which the reward model will give maximal scores."
    - **Plain English:** RLHF = reward model + optimize against it.
    - **Technical terms:** RLHF; reward model.

93. **Quote (DPO / Llama):** "While RLHF is still used today, newer approaches like DPO (Rafailov et al., 2023) are gaining traction. For example, Meta switched from RLHF for Llama 2 to DPO for Llama 3 to reduce complexity. ... Llama 2's authors posited that 'the superior writing abilities of LLMs... are fundamentally driven by RLHF.'"
    - **Plain English:** DPO is simpler; Meta moved Llama 2 (RLHF) → Llama 3 (DPO); Llama 2 credits RLHF for writing quality.
    - **Technical terms:** DPO; Llama 2/3.

94. **Quote (reward model data):** "For each prompt, multiple responses are generated by either humans or AI. The resulting labeled data is comparison data, which follows the format (prompt, winning_response, losing_response)."
    - **Plain English:** Reward models train on comparison data (winner vs loser), not pointwise scores.
    - **Technical terms:** comparison data; winning/losing responses.

95. **Quote (pointwise vs comparison):** "If we ask labelers to score each response directly, the scores will vary. For the same sample, on a 10-point scale, one labeler might give a 5 and another 7... Evaluating each sample independently is also called pointwise evaluation."
    - **Plain English:** Pointwise scores are noisy; comparison (A vs B) is easier.
    - **Technical terms:** pointwise evaluation; comparison evaluation.

96. **Quote (comparison costs):** "LMSYS... found that manually comparing two responses took on average three to five minutes, as the process requires fact-checking each response (Chiang et al., 2024). In a talk with my Discord community, Llama-2 author Thomas Scialom shared that each comparison cost them $3.50. This is still much cheaper than writing responses, which cost $25 each."
    - **Plain English:** Comparison = 3–5 min, ~$3.50 each; writing responses = $25.
    - **Technical terms:** comparison cost; fact-checking.

97. **Quote (InstructGPT labeling UI):** "Labelers give concrete scores from 1 to 7 as well as rank the responses in the order of their preference, but only the ranking is used to train the reward model. Their inter-labeler agreement is around 73%... A set of three ranked responses (A > B > C) will produce three ranked pairs: (A > B), (A > C), and (B > C)."
    - **Plain English:** Only rankings (not scores) train the reward model; agreement ≈73%; 3 rankings → 3 pairs.
    - **Technical terms:** inter-labeler agreement (73%); ranked pairs.

98. **Quote (reward model loss):** "For each training sample (x, y_w, y_l), the loss value is computed as follows: log(σ(rθ(x, y_w) − rθ(x, y_l))... Goal: find θ to minimize the expected loss for all training samples."
    - **Plain English:** Reward model maximizes the gap between winning and losing scores (via sigmoid log-loss).
    - **Technical terms:** reward model; sigmoid σ; loss minimization; rθ(x, y_w) vs rθ(x, y_l).

99. **Quote (weak judge strong):** "Some people believe that the reward model should be at least as powerful as the foundation model to be able to score the foundation model's responses. However... a weak model can judge a stronger model, as judging is believed to be easier than generation."
    - **Plain English:** Weak models can judge stronger models — judging is easier than generating.
    - **Technical terms:** reward model; judging vs generation.

100. **Quote (PPO):** "These prompts are input into the model, whose responses are scored by the reward model. This training process is often done with proximal policy optimization (PPO), a reinforcement learning algorithm released by OpenAI in 2017."
    - **Plain English:** RL step usually uses PPO (OpenAI, 2017).
    - **Technical terms:** PPO (proximal policy optimization).

101. **Quote (best of N):** "For example, Stitch Fix and Grab find that having the reward model alone is good enough for their applications. They get their models to generate multiple outputs and pick the ones given high scores by their reward models. This approach, often referred to as the best of N strategy."
    - **Plain English:** Best of N: generate N outputs, keep the best-scored one — no RL needed.
    - **Technical terms:** best of N; reward-model selection.

## Sampling

102. **Quote (fundamentals):** "For a language model, to generate the next token, the model first computes the probability distribution over all tokens in the vocabulary." / "Always picking the most likely outcome is called greedy sampling."
    - **Plain English:** LMs compute a distribution over all vocab tokens; always picking the max = greedy.
    - **Technical terms:** probability distribution; greedy sampling.

103. **Quote (sampling by probability):** "Given the context of 'My favorite color is …', if 'red' has a 30% chance of being the next token and 'green' has a 50% chance, 'red' will be picked 30% of the time, and 'green' 50% of the time."
    - **Plain English:** Sampling picks tokens according to their probabilities.
    - **Technical terms:** probability distribution; stochastic sampling.

104. **Quote (logits):** "Given an input, a neural network outputs a logit vector. Each logit corresponds to one possible value... While larger logits correspond to higher probabilities, logits don't represent probabilities. Logits don't sum up to one. Logits can even be negative, while probabilities have to be non-negative. To convert logits to probabilities, a softmax layer is often used."
    - **Plain English:** Logits are raw scores (can be negative, don't sum to 1); softmax converts them to probabilities.
    - **Technical terms:** logit vector; softmax.

105. **Quote (softmax formula):** "Let's say the model has a vocabulary of N and the logit vector is x₁, x₂, ..., x_N. The probability for the ith token, pᵢ is computed as follows: pᵢ = softmax(xᵢ) = e^(xᵢ) / Σⱼ e^(xⱼ)."
    - **Technical terms:** softmax formula; exponential.

### Sampling Strategies

106. **Quote (temperature intro):** "To redistribute the probabilities of the possible values, you can sample with a temperature. Intuitively, a higher temperature reduces the probabilities of common tokens, and as a result, increases the probabilities of rarer tokens. This enables models to create more creative responses. Temperature is a constant used to adjust the logits before the softmax transformation. Logits are divided by temperature."
    - **Plain English:** Higher T flattens the distribution → more creative; logits divided by T.
    - **Technical terms:** temperature.

107. **Quote (temperature example):** "The logits computed from the last layer are [1, 2]. ... Without using temperature, which is equivalent to using the temperature of 1, the softmax probabilities are [0.27, 0.73]. The model picks B 73% of the time. With temperature = 0.5, the probabilities are [0.12, 0.88]."
    - **Plain English:** Logits [1,2]: T=1 → B 73%; T=0.5 → B 88% (more concentrated).
    - **Technical terms:** temperature; softmax probabilities.

108. **Quote (temperature range):** "The higher the temperature, the less likely it is that the model is going to pick the most obvious value... Model providers typically limit the temperature to be between 0 and 2... A temperature of 0.7 is often recommended for creative use cases, as it balances creativity and predictability."
    - **Plain English:** Providers cap T at ~2; 0.7 recommended for creative balance.
    - **Technical terms:** temperature range (0–2); 0.7.

109. **Quote (temperature 0):** "It's common practice to set the temperature to 0 for the model's outputs to be more consistent. Technically, temperature can never be 0—logits can't be divided by 0. In practice, when we set the temperature to 0, the model just picks the token with the largest logit, without doing logit adjustment and softmax calculation."
    - **Plain English:** T=0 = arg max (greedy), no softmax; technically 0 is impossible.
    - **Technical terms:** arg max; temperature 0.

110. **Quote (logprobs):** "Logprobs, short for log probabilities, are probabilities in the log scale. Log scale is preferred when working with a neural network's probabilities because it helps reduce the underflow problem. A language model might be working with a vocabulary size of 100,000, which means the probabilities for many of the tokens can be too small to be represented by a machine."
    - **Plain English:** Logprobs avoid underflow with tiny probabilities (large vocabularies).
    - **Technical terms:** logprobs; underflow problem.

111. **Quote (logprobs access):** "However, as of this writing, many model providers don't expose their models' logprobs, or if they do, the logprobs API is limited. The limited logprobs API is likely due to security reasons as a model's exposed logprobs make it easier for others to replicate the model."
    - **Plain English:** Providers limit logprobs access (security/replication concerns); OpenAI shows only top-20 logprobs; Anthropic exposes none.
    - **Technical terms:** logprobs API; model replication.

112. **Quote (top-k):** "To avoid this problem, after the model has computed the logits, we pick the top-k logits and perform softmax over these top-k logits only. Depending on how diverse you want your application to be, k can be anywhere from 50 to 500—much smaller than a model's vocabulary size."
    - **Plain English:** Top-k: softmax over the top k logits only (k ≈ 50–500) → less compute.
    - **Technical terms:** top-k sampling.

113. **Quote (top-p):** "Top-p, also known as nucleus sampling, allows for a more dynamic selection of values to be sampled from. In top-p sampling, the model sums the probabilities of the most likely next values in descending order and stops when the sum reaches p... Common values for top-p (nucleus) sampling in language models typically range from 0.9 to 0.95."
    - **Plain English:** Top-p: include values until cumulative probability exceeds p (0.9–0.95).
    - **Technical terms:** top-p; nucleus sampling.

114. **Quote (top-p example):** "Let's say the probabilities of all tokens are as shown in Figure 2-18. If top-p is 90%, only 'yes' and 'maybe' will be considered, as their cumulative probability is greater than 90%. If top-p is 99%, then 'yes', 'maybe', and 'no' are considered."
    - **Plain English:** Top-p adapts to context (yes/no prompt considers few tokens).
    - **Technical terms:** cumulative probability; top-p.

115. **Quote (top-p vs top-k):** "Unlike top-k, top-p doesn't necessarily reduce the softmax computation load. Its benefit is that because it focuses only on the set of most relevant values for each context, it allows outputs to be more contextually appropriate."
    - **Plain English:** Top-p doesn't reduce softmax load but is more contextually adaptive.
    - **Technical terms:** top-p vs top-k; contextual appropriateness.

116. **Quote (min-p):** "A related sampling strategy is min-p, where you set the minimum probability that a token must reach to be considered during sampling."
    - **Plain English:** Min-p: tokens below a probability floor are excluded.
    - **Technical terms:** min-p.

117. **Quote (stopping conditions):** "One easy method is to ask models to stop generating after a fixed number of tokens. The downside is that the output is likely to be cut off mid-sentence. Another method is to use stop tokens or stop words... The downside of early stopping is that if you want models to generate outputs in a certain format, premature stopping can cause outputs to be malformatted. For example, if you ask the model to generate JSON, early stopping can cause the output JSON to be missing things like closing brackets."
    - **Plain English:** Stop conditions cut cost/latency but can truncate or malform outputs.
    - **Technical terms:** stopping condition; stop tokens; end-of-sequence token.

### Test Time Compute

118. **Quote (test time compute):** "One simple way to improve a model's response quality is test time compute: instead of generating only one response per query, you generate multiple responses to increase the chance of good responses. One way to do test time compute is the best of N technique... you can use beam search to generate a fixed number of most promising candidates (the beam) at each step of sequence generation."
    - **Plain English:** Spend more inference compute on multiple outputs (best of N, beam search) to get better answers.
    - **Technical terms:** test time compute; best of N; beam search.

119. **Quote (diversity):** "A simple strategy to increase the effectiveness of test time compute is to increase the diversity of the outputs, because a more diverse set of options is more likely to yield better candidates. If you use the same model to generate different options, it's often a good practice to vary the model's sampling variables."
    - **Plain English:** Vary sampling variables to diversify candidates.
    - **Technical terms:** output diversity; sampling variables.

120. **Quote (cost):** "Although you can usually expect some model performance improvement by sampling multiple outputs, it's expensive. On average, generating two outputs costs approximately twice as much as generating one."
    - **Plain English:** 2 outputs ≈ 2× cost (input may be processed once and reused).
    - **Technical terms:** test-time-compute cost.

121. **Quote (selection methods):** "One selection method is to pick the output with the highest probability. A language model's output is a sequence of tokens, and each token has a probability computed by the model. The probability of an output is the product of the probabilities of all tokens in the output."
    - **Plain English:** Output probability = product of its token probabilities.
    - **Technical terms:** joint probability; token probabilities.
    - **Example:** p(I love food) = p(I) × p(I|love) × p(food|I, love) = 0.2 × 0.1 × 0.3 = 0.006.

122. **Quote (logprob sum):** "The logarithm of a product is equal to a sum of logarithms, so the logprob of a sequence of tokens is the sum of the logprob of all tokens in the sequence... With summing, longer sequences are likely to have a lower total logprob... To avoid biasing toward short sequences, you can use the average logprob by dividing the sum of a sequence by its length. After sampling multiple outputs, you pick the one with the highest average logprob. As of this writing, this is what the OpenAI API uses."
    - **Plain English:** Sum logprobs; use average logprob to avoid short-sequence bias (OpenAI's `best_of`).
    - **Technical terms:** average logprob; log-sum; OpenAI `best_of`.

123. **Quote (verifiers):** "OpenAI also trained verifiers to help their models pick the best solutions to math problems (Cobbe et al., 2021). They found that using a verifier significantly boosted the model performance. In fact, the use of verifiers resulted in approximately the same performance boost as a 30× model size increase. This means that a 100-million-parameter model that uses a verifier can perform on par with a 3-billion-parameter model that doesn't use a verifier."
    - **Plain English:** Verifiers ≈ 30× model-size boost (100M + verifier ≈ 3B without).
    - **Technical terms:** verifier; reward model.

124. **Quote (scaling samples):** "In OpenAI's experiment, sampling more outputs led to better performance, but only up to a certain point. In this experiment, that point was 400 outputs. Beyond this point, performance decreases... However, a Stanford experiment showed a different conclusion. 'Monkey Business' (Brown et al., 2024) finds that the number of problems solved often increases log-linearly as the number of samples increases from 1 to 10,000."
    - **Plain English:** OpenAI peak = 400 samples; Stanford found log-linear gains to 10,000.
    - **Technical terms:** sample scaling; "Monkey Business".

125. **Quote (self-consistency / MMLU):** "Picking out the most common output among a set of outputs can be especially useful for tasks that expect exact answers... This is what Google did when evaluating Gemini on the MMLU benchmark. They sampled 32 outputs for each question."
    - **Plain English:** Self-consistency: pick the most common answer (Gemini MMLU: 32 samples).
    - **Technical terms:** self-consistency (Wang et al., 2023); majority vote.

126. **Quote (robustness):** "A model is considered robust if it doesn't dramatically change its outputs with small variations in the input. The less robust a model is, the more you can benefit from sampling multiple outputs. For one project, we used AI to extract certain information from an image of the product. We found that for the same image, our model could read the information only half of the time. ... by trying three times with each image, the model was able to extract the correct information for most images."
    - **Plain English:** Brittle models benefit most from sampling; 3 tries fixed most extraction failures.
    - **Technical terms:** robustness; sampling multiple outputs.

### Structured Outputs

127. **Quote (two scenarios):** "Structured outputs are crucial for the following two scenarios: 1. Tasks requiring structured outputs. The most common category of tasks in this scenario is semantic parsing. Semantic parsing involves converting natural language into a structured, machine-readable format. Text-to-SQL is an example... 2. Tasks whose outputs are used by downstream applications. ... a downstream application using this email might need it to be in a specific format—for example, a JSON document with specific keys, such as {"title": [TITLE], "body": [EMAIL BODY]}."
    - **Plain English:** Structured outputs needed when the task requires them (text-to-SQL) or when downstream apps consume them (JSON email).
    - **Technical terms:** structured outputs; semantic parsing; text-to-SQL; agentic workflows (Ch 6).

128. **Quote (JSON mode caveats):** "OpenAI was the first model provider to introduce JSON mode in their text generation API. Note that an API's JSON mode typically guarantees only that the outputs are valid JSON—not the content of the JSON objects. The otherwise valid generated JSONs can also be truncated, and thus not parsable, if the generation stops too soon, such as when it reaches the maximum output token length."
    - **Plain English:** JSON mode guarantees syntax, not content; truncation can still break parseability.
    - **Technical terms:** JSON mode; maximum output tokens.

129. **Quote (five layers):** "You can guide a model to generate structured outputs at different layers of the AI stack: prompting, post-processing, test time compute, constrained sampling, and finetuning. The first three are more like bandages. They work best if the model is already pretty good at generating structured outputs and just needs a little nudge. For intensive treatment, you need constrained sampling and finetuning."
    - **Plain English:** Five layers; first three = bandages, last two = intensive treatment.
    - **Technical terms:** prompting; post-processing; test time compute; constrained sampling; finetuning.

130. **Quote (post-processing / LinkedIn):** "A model tends to repeat similar mistakes across queries. This means if you find the common mistakes a model makes, you can potentially write a script to correct them. For example, if the generated JSON object misses a closing bracket, manually add that bracket. LinkedIn's defensive YAML parser increased the percentage of correct YAML outputs from 90% to 99.99%."
    - **Plain English:** Models make repeatable mistakes; scriptable fixes work well (LinkedIn 90% → 99.99%).
    - **Technical terms:** post-processing; defensive parser; YAML.

131. **Quote (YAML choice):** "LinkedIn found that their underlying model, GPT-4, worked with both, but they chose YAML as their output format because it is less verbose, and hence requires fewer output tokens than JSON."
    - **Plain English:** LinkedIn chose YAML (less verbose → fewer tokens).
    - **Technical terms:** token efficiency; YAML vs JSON.

132. **Quote (constrained sampling):** "Constrained sampling filters this logit vector to keep only the tokens that meet the constraints. It then samples from these valid tokens... You need to have a grammar that specifies what is and isn't allowed at each step... Because each output format—JSON, YAML, regex, CSV, and so on—needs its own grammar, constraint sampling is less generalizable."
    - **Plain English:** Constrained sampling filters logits by a per-format grammar → less generalizable.
    - **Technical terms:** constrained sampling; grammar; logit filtering.

133. **Quote (finetuning for structure):** "Finetuning a model on examples following your desirable format is the most effective and general approach to get models to generate outputs in this format... For example, for classification, you can append a classifier head to the foundation model's architecture to make sure that the model outputs only one of the pre-specified classes. This approach is also called feature-based transfer."
    - **Plain English:** Finetuning (esp. with a classifier head) is the most reliable for fixed formats.
    - **Technical terms:** classifier head; feature-based transfer; end-to-end vs head-only training.

### The Probabilistic Nature of AI

134. **Quote (probabilistic):** "Imagine that you want to know what's the best cuisine in the world. If you ask your friend this question twice, a minute apart, your friend's answers both times should be the same. If you ask an AI model the same question twice, its answer can change. If an AI model thinks that Vietnamese cuisine has a 70% chance of being the best cuisine in the world and Italian cuisine has a 30% chance, it'll answer 'Vietnamese cuisine' 70% of the time and 'Italian cuisine' 30% of the time."
    - **Plain English:** AI answers probabilistically — same question can yield different answers.
    - **Technical terms:** probabilistic vs deterministic.

135. **Quote (inconsistency & hallucination):** "This probabilistic nature can cause inconsistency and hallucinations. Inconsistency is when a model generates very different responses for the same or slightly different prompts. Hallucination is when a model gives a response that isn't grounded in facts."
    - **Plain English:** Probabilistic sampling → inconsistency (different outputs) and hallucination (unfounded outputs).
    - **Technical terms:** inconsistency; hallucination.

136. **Quote (anything can be generated):** "Foundation models... are aggregations of the opinions of the masses, containing within them, literally, a world of possibilities. Anything with a non-zero probability, no matter how far-fetched or wrong, can be generated by AI."
    - **Plain English:** Any non-zero-probability output, however wrong, can appear.
    - **Technical terms:** probability distribution; "chances are low, but never zero."

137. **Quote (creative vs problematic):** "This probabilistic nature makes AI great for creative tasks. What is creativity but the ability to explore beyond the common paths... However, this same probabilistic nature can be a pain for everything else."
    - **Plain English:** Probabilistic = great for creativity, painful for reliability.
    - **Technical terms:** probabilistic nature.

138. **Quote (inconsistency scenarios):** "Model inconsistency manifests in two scenarios: 1. Same input, different outputs: Giving the model the same prompt twice leads to two very different responses. 2. Slightly different input, drastically different outputs: Giving the model a slightly different prompt, such as accidentally capitalizing a letter, can lead to a very different output."
    - **Plain English:** Two inconsistency scenarios: same input→different outputs; slightly different input→very different outputs.
    - **Technical terms:** inconsistency; sensitivity to input variation.

139. **Quote (ChatGPT essay scoring):** "Figure 2-23 shows an example of me trying to use ChatGPT to score essays. The same prompt gave me two different scores when I ran it twice: 3/5 and 5/5."
    - **Plain English:** Same essay-scoring prompt twice → 3/5 vs 5/5.
    - **Technical terms:** inconsistency example.

140. **Quote (mitigations for same-input):** "For the same input, different outputs scenario, there are multiple approaches to mitigate inconsistency. You can cache the answer so that the next time the same question is asked, the same answer is returned. You can fix the model's sampling variables, such as temperature, top-p, and top-k values... You can also fix the seed variable, which you can think of as the starting point for the random number generator used for sampling the next token."
    - **Plain English:** Mitigate by caching, fixing sampling variables, fixing the seed.
    - **Technical terms:** caching; sampling variables; seed.

141. **Quote (hardware):** "Even if you fix all these variables, however, there's no guarantee that your model will be consistent 100% of the time. The hardware the model runs the output generation on can also impact the output, as different machines have different ways of executing the same instruction and can handle different ranges of numbers."
    - **Plain English:** Hardware differences can still change outputs; no 100% guarantee.
    - **Technical terms:** hardware-dependent output; determinism limits.

142. **Quote (slightly-different-input is harder):** "The second scenario—slightly different input, drastically different outputs—is more challenging. Fixing the model's output generation variables is still a good practice, but it won't force the model to generate the same outputs for different inputs. It is, however, possible to get models to generate responses closer to what you want with carefully crafted prompts (discussed in Chapter 5) and a memory system (discussed in Chapter 6)."
    - **Plain English:** Scenario 2 is harder; prompts (Ch 5) and memory (Ch 6) help.
    - **Technical terms:** prompt engineering; memory system.

### Hallucination

143. **Quote (fatal for factuality):** "Hallucinations are fatal for tasks that depend on factuality. If you're asking AI to help you explain the pros and cons of a vaccine, you don't want AI to be pseudo-scientific. In June 2023, a law firm was fined for submitting fictitious legal research to court. They had used ChatGPT to prepare their case, unaware of ChatGPT's tendency to hallucinate."
    - **Plain English:** Hallucination is fatal for factual tasks; a law firm was fined (June 2023).
    - **Technical terms:** hallucination; fictitious legal research.

144. **Quote (predates LLMs):** "While hallucination became a prominent issue with the rise of LLMs, hallucination was a common phenomenon for generative models even before the term foundation model and the transformer architecture were introduced. Hallucination in the context of text generation was mentioned as early as 2016 (Goyal et al., 2016)."
    - **Plain English:** Hallucination predates LLMs (2016, Goyal et al.).
    - **Technical terms:** NLG (natural language generation); hallucination detection (Lee 2018, Nie 2019, Zhou 2020).

145. **Quote (hypothesis 1 — self-delusion):** "The first hypothesis, originally expressed by Ortega et al. at DeepMind in 2021, is that a language model hallucinates because it can't differentiate between the data it's given and the data it generates... Starting with a generated sequence slightly out of the ordinary, the model can expand upon it and generate outrageously wrong facts. Ortega and the other authors called hallucinations a form of self-delusion."
    - **Plain English:** Hallucination = self-delusion: the model can't tell its own output from given data.
    - **Technical terms:** self-delusion hypothesis (Ortega et al., DeepMind 2021).

146. **Quote (snowballing):** "Zhang et al. (2023) call this phenomenon snowballing hallucinations. After making an incorrect assumption, a model can continue hallucinating to justify the initial wrong assumption."
    - **Plain English:** Snowballing: an initial wrong assumption spawns more hallucination.
    - **Technical terms:** snowballing hallucinations (Zhang et al., 2023).

147. **Quote (DeepMind mitigations):** "The DeepMind paper showed that hallucinations can be mitigated by two techniques. The first technique comes from reinforcement learning, in which the model is made to differentiate between user-provided prompts (called observations about the world in reinforcement learning) and tokens generated by the model (called the model's actions). The second technique leans on supervised learning, in which factual and counterfactual signals are included in the training data."
    - **Plain English:** Mitigations: RL (distinguish observations vs actions) and SL (factual + counterfactual signals).
    - **Technical terms:** observations; actions; counterfactual signals.

148. **Quote (hypothesis 2 — mismatched knowledge):** "The second hypothesis is that hallucination is caused by the mismatch between the model's internal knowledge and the labeler's internal knowledge. This view was first argued by Leo Gao, an OpenAI researcher. During SFT, models are trained to mimic responses written by labelers. If these responses use the knowledge that the labelers have but the model doesn't have, we're effectively teaching the model to hallucinate."
    - **Plain English:** SFT teaches hallucination when labelers use knowledge the model lacks.
    - **Technical terms:** mismatched internal knowledge (Leo Gao).

149. **Quote (Schulman):** "In April 2023, John Schulman, an OpenAI co-founder, expressed the same view in his UC Berkeley talk. Schulman also believes that LLMs know if they know something... He proposed two solutions. One is verification: for each response, ask the model to retrieve the sources it bases this response on. Another is to use reinforcement learning. ... Schulman argued that a better reward function that punishes a model more for making things up can help mitigate hallucinations."
    - **Plain English:** Schulman: LLMs know if they know; fixes = verification (source retrieval) + RL (better reward function).
    - **Technical terms:** verification; reward function; RL.

150. **Quote (RLHF vs hallucination nuance):** "In that same talk, Schulman mentioned that OpenAI found that RLHF helps with reducing hallucinations. However, the InstructGPT paper shows that RLHF made hallucination worse... Even though RLHF seemed to worsen hallucinations for InstructGPT, it improved other aspects, and overall, human labelers prefer the RLHF model over the SFT alone model."
    - **Plain English:** RLHF's effect on hallucination is mixed (worse per InstructGPT paper) yet humans prefer RLHF models.
    - **Technical terms:** RLHF; hallucination tradeoffs.

151. **Quote (prompting mitigations):** "Based on the assumption that a foundation model knows what it knows, some people try to reduce hallucination with prompts, such as adding 'Answer as truthfully as possible, and if you're unsure of the answer, say, "Sorry, I don't know."' Asking models for concise responses also seems to help with hallucinations—the fewer tokens a model has to generate, the less chance it has to make things up."
    - **Plain English:** Truthfulness prompts and concise responses reduce hallucination.
    - **Technical terms:** prompting; concise responses.

152. **Quote (hypotheses complement):** "The two hypotheses discussed complement each other. The self-delusion hypothesis focuses on how self-supervision causes hallucinations, whereas the mismatched internal knowledge hypothesis focuses on how supervision causes hallucinations."
    - **Plain English:** Self-delusion = self-supervision cause; mismatched knowledge = supervision cause.
    - **Technical terms:** self-supervision vs supervision causes.

### Summary

153. **Quote:** "This chapter discussed the core design decisions when building a foundation model... differences in foundation models can be traced back to decisions about training data, model architecture and size, and how they are post-trained to align with human preferences."
    - **Plain English:** The chapter covered the three design decisions plus sampling.
    - **Technical terms:** training data; architecture/size; post-training; sampling.

154. **Quote (closing thesis):** "Working with AI models requires building your workflows around their probabilistic nature. The rest of this book will explore how to make AI engineering, if not deterministic, at least systematic. The first step toward systematic AI engineering is to establish a solid evaluation pipeline to help detect failures and unexpected changes."
    - **Plain English:** Embrace the probabilistic nature; start with evaluation (next two chapters).
    - **Technical terms:** evaluation pipeline; systematic AI engineering.
