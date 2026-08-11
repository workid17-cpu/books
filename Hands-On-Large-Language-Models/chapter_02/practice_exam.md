# AI Agents — Practice Exam (Chapter 2)
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 2 "Tokens and Embeddings"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What are the two roles of a tokenizer?
   a) Preparing input text into token IDs and decoding output token IDs back into text
   b) Training the model and generating embeddings
   c) Tokenizing both the GPU and the CPU
   d) Only splitting text on whitespace

2. In the Phi-3 example, the model receives as input:
   a) The raw prompt string
   b) A tensor of integer token IDs (input_ids)
   c) The generated output text
   d) The vocabulary table

3. The first token ID in the example (ID 1, `<s>`) indicates:
   a) An unknown word
   b) The end of the text
   c) The beginning of the text
   d) A padding position

4. How is the space character represented in tokenization?
   a) As its own dedicated token
   b) As a special [SPACE] token
   c) As multiple tokens
   d) By a special hidden character on partial tokens indicating connection to the preceding token

5. Which are the four notable tokenization schemes?
   a) BPE, WordPiece, SentencePiece, unigram
   b) Cased, uncased, padded, masked
   c) Word, subword, character, byte
   d) Input, output, hidden, special

6. Why is subword tokenization more expressive than word tokenization?
   a) It has a larger vocabulary
   b) It shares roots and common suffixes (e.g., -y, -ize, -etic, -ist)
   c) It always uses whole words
   d) It uses fewer special tokens

7. With a context length of 1,024, subword tokenization fits about how much more text than character tokens?
   a) The same amount
   b) Half as much
   c) Three times as much
   d) Ten times as much

8. Which method is used by BERT?
   a) BPE
   b) SentencePiece
   c) Unigram
   d) WordPiece

9. Which tokenizer implementation does Flan-T5 use?
   a) SentencePiece
   b) WordPiece
   c) CANINE
   d) ByT5

10. The uncased BERT tokenizer:
    a) Keeps capitalization
    b) Lowercases all text and drops newline breaks
    c) Preserves emojis as tokens
    d) Uses 50,257 tokens

11. How did the cased BERT tokenizer represent "CAPITALIZATION"?
    a) As one token
    b) As two tokens
    c) As eight tokens (`CA ##PI ##TA ##L ##I ##Z ##AT ##ION`)
    d) As [UNK]

12. The GPT-2 tokenizer encoded the 🎵 emoji as:
    a) A single token
    b) The [UNK] token
    c) Two tokens
    d) Three tokens (IDs 8582, 236, 113)

13. Which tokenizer is unique in assigning a single token to a two-tab string (`\t\t`)?
    a) StarCoder2
    b) GPT-4
    c) Galactica
    d) Flan-T5

14. What do the FIM tokens `<|fim_prefix|>`, `<|fim_middle|>`, `<|fim_suffix|>` enable?
    a) Sentence classification
    b) Generating a completion given both text before and after it
    c) Masked language modeling
    d) Cross-encoder reranking

15. Which model adds chat role tokens `<|user|>`, `<|assistant|>`, `<|system|>` to a reused tokenizer?
    a) GPT-4
    b) StarCoder2
    c) Phi-3
    d) Galactica

16. Why did StarCoder2 assign each digit its own token?
    a) To reduce vocabulary size
    b) To speed up training
    c) To improve code autocompletion only
    d) To better represent numbers and mathematics

17. What are the three major factors dictating how a tokenizer breaks down text?
    a) Method, parameters/special tokens, and training dataset
    b) GPU, CPU, and VRAM
    c) Context length, batch size, and temperature
    d) Model, tokenizer, and pipeline

18. Why is a pretrained language model unable to use a different tokenizer without training?
    a) Tokenizers are proprietary
    b) The model holds an embedding vector for each token in its own tokenizer's vocabulary
    c) Hugging Face disallows it
    d) Tokenizers only work on the output side

19. How are embedding vectors initialized before training?
    a) With zeros
    b) With pretrained GloVe values
    c) Randomly, like the rest of the model's weights
    d) With one-hot encodings

20. Contextualized word embeddings differ from static embeddings because they:
    a) Are always 768-dimensional
    b) Are cheaper to compute
    c) Come from word2vec
    d) Represent a word differently based on its context

21. Which application is NOT powered by contextualized token embeddings per the chapter?
    a) Named-entity recognition
    b) Extractive text summarization
    c) Spell checking
    d) AI image generation systems

22. The DeBERTa v3 "Hello world" output shape `torch.Size([1, 4, 384])` means:
    a) 1 batch, 4 tokens, 384 embedding values per token
    b) 4 batches, 1 token, 384 values
    c) 384 batches, 1 token, 4 values
    d) 1 batch, 384 tokens, 4 values

23. What is a text embedding?
    a) An embedding per token
    b) A single vector representing a whole sentence or document
    c) A static word vector
    d) A token ID

24. The all-mpnet-base-v2 model encodes "Best movie ever!" into a vector of shape:
    a) (1,)
    b) (384,)
    c) (4, 384)
    d) (768,)

25. What are the two main ideas of the word2vec algorithm?
    a) Skip-gram and negative sampling
    b) BPE and WordPiece
    c) CLS and SEP
    d) Encoding and decoding

26. Why are negative examples necessary in word2vec training?
    a) To increase the vocabulary
    b) So the model can't cheat by always predicting 1
    c) To speed up the sliding window
    d) To reduce the embedding dimension

27. Negative example selection is inspired by:
    a) Noise-contrastive estimation
    b) Masked language modeling
    c) Cross-encoder training
    d) Tokenization-free encoding

28. In the playlist recommender, what acts as "words" and "sentences"?
    a) Users and ratings
    b) Artists and albums
    c) Genres and radio stations
    d) Songs and playlists

29. What parameters were used to train the song Word2Vec model?
    a) vector_size=32, window=20, negative=50, min_count=1, workers=4
    b) vector_size=50, window=5, negative=5, min_count=5
    c) vector_size=768, window=2, negative=10
    d) vector_size=100, window=10, negative=100

30. The playlist dataset was collected by:
    a) OpenAI
    b) Shuo Chen from Cornell University
    c) Jay Alammar
    d) Spotify

31. What was recommended for "Billie Jean" (ID 3822)?
    a) Metallica and Van Halen songs
    b) Only other Michael Jackson songs
    c) Kiss, Madonna, and other Michael Jackson songs
    d) Classical music

32. The contrastive idea of a model predicting if two vectors have a certain relation is central to:
    a) Chapter 9 (multimodal) only
    b) Chapter 10 (sentence embeddings/retrieval) only
    c) Neither chapter
    d) Both Chapter 10 and Chapter 9

33. Why did RAG (retrieval-augmented generation) rise?
    a) Generative models alone aren't reliable search engines
    b) Tokenizers couldn't handle code
    c) Embeddings were too large
    d) Fine-tuning was too expensive

34. The Gensim load `glove-wiki-gigaword-50` produces embeddings of dimension:
    a) 768
    b) 50
    c) 384
    d) 32

35. What were the nearest neighbors of "king" in the GloVe example?
    a) queen, prince, emperor, throne, kingdom, ruler, etc.
    b) monarch, crown, castle
    c) llama, alpaca, camel
    d) king only

36. What makes the GPT-4 tokenizer well suited to code (per the chapter)?
    a) It uses character tokens
    b) It has a single token for whitespace sequences (up to 83) and a token for `elif`
    c) It drops all punctuation
    d) It uses the unigram method

37. The byte-tokenization papers CANINE and ByT5 are especially competitive in which scenario?
    a) Multilingual scenarios
    b) Code generation only
    c) Sentence classification
    d) Music recommendation

38. How does the GPT-2 tokenizer handle newlines?
    a) It drops them (model is blind to them)
    b) It replaces them with [UNK]
    c) It merges them into the previous token
    d) It represents them in the tokenizer

39. The `<|endoftext|>` token is used to:
    a) Start a new conversation
    b) Mask tokens during training
    c) Signal the model has completed generation
    d) Separate two sentences

40. The book's main generative model Phi-3-mini reuses which tokenizer?
    a) GPT-4's
    b) BERT's
    c) StarCoder2's
    d) Llama 2's

---

## Section B: True/False (1 point each)

41. Tokens are only the output of a model, never its input. (T/F)
42. Punctuation characters are their own token. (T/F)
43. Word tokenization handles new words introduced after training well. (T/F)
44. The `##` characters in WordPiece indicate a partial token connected to the preceding token. (T/F)
45. The uncased BERT tokenizer preserves newline breaks. (T/F)
46. The Flan-T5 tokenizer represents emoji and Chinese characters with the <unk> token. (T/F)
47. GPT-2 and RoBERTa tokenizers are byte-level tokenization-free encoders. (T/F)
48. Language is, in one sense, a sequence of tokens. (T/F)
49. High-quality text embedding models are usually trained specifically for text-embedding tasks rather than just averaging token embeddings. (T/F)
50. Averaging token embeddings is the only way to produce a text embedding. (T/F)
51. Word2vec embeddings are contextualized — the embedding of "bank" changes with its context. (T/F)
52. A sliding window generates training examples for word2vec. (T/F)
53. Negative examples in word2vec are words that are typically neighbors. (T/F)
54. The playlist recommender produced genre-consistent recommendations (e.g., Metallica → metal/hard rock). (T/F)
55. The word2vec training matrix has dimensions vocab_size × embedding_dimensions. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Describe the three factors that determine how a tokenizer breaks down text.
57. Explain the four tokenization schemes (word, subword, character, byte) and one advantage of each.
58. Why is the space character not a separate token, and how are spaces implied?
59. Compare the GPT-2 and GPT-4 tokenizers on: vocabulary size, handling of whitespace, representation of "CAPITALIZATION", and FIM tokens.
60. What special tokens does StarCoder2 add, and why?
61. Explain why a pretrained model is tied to its tokenizer.
62. What is the difference between token embeddings, contextualized word embeddings, and text embeddings?
63. Describe how word2vec generates training examples and why negative sampling matters.
64. Walk through the song-recommendation pipeline (data, model, recommendations).
65. Why did RAG emerge, and what does it combine?

---

## Section D: Essay / Applied (5 points each)

66. **Tokenizer design.** Discuss the three tokenizer design choices (method, parameters, dataset) and how the guided tour (BERT uncased/cased, GPT-2, Flan-T5, GPT-4, StarCoder2, Galactica, Phi-3) demonstrates each. Use specific examples (whitespace, digits, emoji, capitalization, special tokens).
67. **From tokens to embeddings.** Explain how tokens become embeddings inside a language model, why embeddings are randomly initialized then trained, and how contextualized word embeddings improve on static ones. Include the DeBERTa example (`[1,4,384]`) and the applications they power.
68. **word2vec and contrastive training.** Describe the word2vec algorithm: sliding window, skip-gram, negative sampling, noise-contrastive estimation, and the training matrix. Explain why the "two vectors → relation prediction" idea is powerful and where else it appears (Ch 9, Ch 10).
69. **Embeddings for recommendations.** Describe how the music recommender is built from playlists: treating songs as words and playlists as sentences, the Word2Vec parameters, and `most_similar`. Explain why the recommendations (Billie Jean; Fade to Black) make sense.
70. **Whitespace, code, and specialization.** Explain the significance of whitespace tokens for code models, why digit-per-token helps math representation, and how specialized tokenizers (Galactica's citations/reasoning, StarCoder2's filename/reponame tokens) optimize for their domains.

---

## ANSWER KEY

### Section A: Multiple Choice
1. a
2. b
3. c
4. d
5. c
6. b
7. c
8. d
9. a
10. b
11. c
12. d
13. c
14. b
15. c
16. d
17. a
18. b
19. c
20. d
21. c
22. a
23. b
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
35. a
36. b
37. a
38. d
39. c
40. d

### Section B: True/False
41. **F** — Tokens are both input (tokenizer output) and output (generated).
42. **T** — Punctuation is its own token.
43. **F** — Word tokenization struggles with new words (a key reason subword exists).
44. **T** — `##` marks a partial token connected to the preceding one.
45. **F** — The uncased BERT tokenizer drops newline breaks.
46. **T** — Flan-T5 replaces emoji/Chinese with <unk>.
47. **F** — They include bytes as fallback but remain subword tokenizers.
48. **T** — Language is a sequence of tokens.
49. **T** — Dedicated training produces higher quality text embeddings.
50. **F** — Averaging is one common way; dedicated training is the other main approach.
51. **F** — word2vec embeddings are static/context-free.
52. **T** — A sliding window generates word2vec training examples.
53. **F** — Negative examples are words that are NOT usually neighbors.
54. **T** — Recommendations stayed in genre (metal/hard rock).
55. **T** — vocab_size × embedding_dimensions.

### Section C: Short Answer (model answers)
56. **Three factors.** (1) Tokenization method (BPE, WordPiece, SentencePiece); (2) parameters/design choices (vocab size, special tokens, capitalization); (3) training dataset domain (English vs code vs multilingual).
57. **Four schemes.** Word (whole words; word2vec era; poor for new words); subword (full+partial words; most common; expressive, handles new words); character (raw letters; handles new words but harder modeling); byte (unicode bytes; tokenization-free; multilingual-friendly; CANINE/ByT5).
58. **Spaces.** The space character has no own token; partial tokens carry a hidden connection marker (e.g., `##`); tokens without it are assumed to have a space before them.
59. **GPT-2 vs GPT-4.** GPT-2: vocab 50,257; newlines/tabs/space tokens (tab=197, spaces=220); "CAPITALIZATION" = 4 tokens; no FIM tokens. GPT-4: vocab ~100K+; single token for whitespace runs up to 83; "CAPITALIZATION" = 2 tokens; FIM tokens `<|fim_prefix|>/<|fim_middle|>/<|fim_suffix|>`.
60. **StarCoder2 tokens.** `<|endoftext|>`, FIM tokens incl `<fim_pad>`, plus `<filename>`, `<reponame>`, `<gh_stars>` to track code across files/repos.
61. **Tied tokenizer.** The model holds an embedding vector per token in its tokenizer's vocabulary; a different tokenizer would have mismatched vocabulary → needs retraining.
62. **Embedding types.** Token embedding = per-token vector; contextualized = per-token but context-dependent (from LMs); text embedding = one vector per sentence/document.
63. **word2vec training.** Sliding window creates center+neighbor examples; negative sampling adds random non-neighbors; two-vector classifier predicts neighbor/not; embeddings updated; matrix vocab_size×embedding_dims.
64. **Song pipeline.** Load playlists (songs=words) + metadata; train `Word2Vec(vector_size=32, window=20, negative=50, min_count=1, workers=4)`; `model.wv.most_similar(positive=str(song_id))`; map IDs→titles/artists. Results: Billie Jean → Prince/Madonna/MJ; Fade to Black → metal/hard rock.
65. **RAG.** Generative models alone aren't reliable search engines (2023 "Google killers" hype); RAG combines search + LLMs (Ch 8).

### Section D: Essay (grading notes)
66. **Expect** three choices with concrete examples from the tour: BPE/WordPiece/SentencePiece methods; vocab sizes (30,522/28,996/50,257/32,100/100K+/49,152/50,000/32,000); special tokens ([CLS]/[SEP]/[PAD]/[MASK]/[UNK], FIM, chat roles, repo/filename, citations); capitalization (cased vs uncased); dataset domain (code-focused whitespace/digits vs science tokens).
67. **Expect** token IDs → embeddings lookup (first step inside model); random init → trained values; embeddings matrix; contextualized embeddings differ by context; DeBERTa `[1,4,384]` ([CLS] Hello world [SEP]); applications (NER, extractive summarization, classification, image generation DALL·E/Midjourney/Stable Diffusion).
68. **Expect** sliding window example from Dune; positive (center+neighbor) and negative (random) examples; why negatives needed; skip-gram; negative sampling; noise-contrastive estimation; matrix shape; why two-vector relation prediction is powerful and reused in Ch 10 (sentence embeddings/retrieval) and Ch 9 (image+caption matching).
69. **Expect** dataset (Shuo Chen/Cornell, US radio playlists); songs as words, playlists as sentences; Word2Vec params; `most_similar`; mapping via songs_df; genre-consistent outputs.
70. **Expect** whitespace significance for code/Python indentation; single-token whitespace runs (GPT-4 up to 83, StarCoder2/Galactica); digit-per-token math benefits (600→6 0 0; GPT-2 870 vs 871 inconsistency); specialized tokens (Galactica citations/reasoning; StarCoder2 filename/reponame/gh_stars).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely tokenizer comparisons or word2vec).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
