# 📘 Chapter 2 Flashcards: Tokens and Embeddings
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 2

---

## Part 1: Terms → Definitions

**Q:** What is a token?
**A:** A small chunk of text — a character, word, or part of a word — that an LLM processes.

**Q:** What are the two roles of a tokenizer?
**A:** (1) Process input text into token IDs for the model; (2) decode the model's output token IDs back into text.

**Q:** What is a token ID?
**A:** A unique integer ID referencing a specific token in the tokenizer's vocabulary table.

**Q:** What is the vocabulary of a tokenizer?
**A:** The table of all the tokens the tokenizer knows.

**Q:** What is a special token?
**A:** A unique token with a role other than representing text (e.g., beginning/end of text, padding, unknown, [CLS], [SEP], [MASK], chat role, or FIM tokens).

**Q:** What is BPE (byte pair encoding)?
**A:** A tokenization method used by GPT models that optimizes an efficient set of tokens to represent a text dataset.

**Q:** What is WordPiece?
**A:** The tokenization method used by BERT.

**Q:** What is SentencePiece?
**A:** A tokenizer implementation used by Flan-T5 that supports BPE and the unigram language model.

**Q:** What are word tokens?
**A:** Tokens that are whole words; common with earlier methods like word2vec.

**Q:** What is a key challenge with word tokenization?
**A:** It can't handle new words entering the dataset, and it creates large vocabularies with near-duplicate tokens (apology/apologize/apologetic/apologist).

**Q:** What are subword tokens?
**A:** Tokens that are full or partial words (e.g., "apolog", "izing"); the most common tokenization scheme.

**Q:** Why is subword tokenization more expressive than word tokenization?
**A:** It shares roots and common suffixes (e.g., -y, -ize, -etic, -ist), representing more words with fewer tokens.

**Q:** What are character tokens?
**A:** Tokens that are individual characters; they handle new words but make modeling harder.

**Q:** What are byte tokens?
**A:** Tokens that are individual bytes used to represent unicode characters; "tokenization-free encoding" (CANINE, ByT5).

**Q:** Why are byte tokens competitive in multilingual scenarios?
**A:** Because they don't rely on language-specific word boundaries; examples include CANINE and ByT5.

**Q:** Are GPT-2/RoBERTa tokenizers byte-level because they include bytes as fallback?
**A:** No — they include bytes only as a fallback for unrepresentable characters; they're still subword tokenizers.

**Q:** What is an [UNK] token?
**A:** The unknown token used when the tokenizer has no encoding for a character.

**Q:** What is the [SEP] token used for?
**A:** Separator — enables tasks requiring two texts (cross-encoder), e.g., reranking (Ch 8).

**Q:** What is the [PAD] token used for?
**A:** Padding — fills unused positions since the model expects input of a certain length.

**Q:** What is the [CLS] token used for?
**A:** Classification — the special token used as the whole-input representation for classification tasks (Ch 4).

**Q:** What is the [MASK] token used for?
**A:** Masking — hides tokens during training (masked language modeling).

**Q:** What is the `<s>` token?
**A:** A special token indicating the beginning of the text (e.g., the first token ID 1 in the Phi-3 example).

**Q:** What is the <|endoftext|> token?
**A:** The end-of-text token, used to signal the model has completed generation.

**Q:** What are FIM (fill-in-the-middle) tokens?
**A:** `<|fim_prefix|>`, `<|fim_middle|>`, `<|fim_suffix|>` — enable the LLM to generate a completion given text both before and after it.

**Q:** What are chat role tokens?
**A:** Tokens indicating conversation turns/speakers: `<|user|>`, `<|assistant|>`, `<|system|>`.

**Q:** What is an embedding?
**A:** A vector representation of data that attempts to capture its meaning.

**Q:** What is the embeddings matrix?
**A:** The matrix holding an embedding vector for each token in the tokenizer's vocabulary; a portion of a downloaded pretrained model.

**Q:** What are contextualized word embeddings?
**A:** Word representations that differ based on context, produced by language models (vs static embeddings).

**Q:** What are text embeddings?
**A:** A single vector that represents a whole sentence, paragraph, or document.

**Q:** What are static embeddings?
**A:** Fixed, context-free word embeddings (e.g., word2vec, GloVe).

**Q:** What is word2vec?
**A:** A 2013 algorithm producing word embeddings via sliding windows, skip-gram, and negative sampling.

**Q:** What is skip-gram?
**A:** The word2vec method of selecting neighboring words as training examples.

**Q:** What is negative sampling?
**A:** Adding negative examples — random words that are not neighbors — by random sampling from the dataset.

**Q:** Why are negative examples needed in word2vec?
**A:** Without them, a model can "cheat" by always predicting 1; negatives teach it to distinguish neighbors from non-neighbors.

**Q:** What is noise-contrastive estimation?
**A:** The important idea that random negative examples suffice to train useful models.

**Q:** What is contrastive training (as introduced here)?
**A:** A model takes two vectors and predicts if they have a certain relation (e.g., are they neighbors?); central to word2vec, Ch 10 (sentence embeddings/retrieval), and Ch 9 (multimodal).

**Q:** What is GloVe?
**A:** A source of pretrained word embeddings (e.g., glove-wiki-gigaword-50, 50-dimensional, Wikipedia-trained).

**Q:** What is a cross-encoder?
**A:** A model that takes two texts at once (with [SEP]); used for reranking (Ch 8).

**Q:** What is RAG (retrieval-augmented generation)?
**A:** Combining search with LLMs, because generative models alone aren't reliable search engines (Ch 8).

**Q:** What is NER (named-entity recognition)?
**A:** A task powered by contextualized token embeddings that identifies named entities in text.

**Q:** What is extractive text summarization?
**A:** Summarizing a long text by highlighting its most important parts, instead of generating new text.

**Q:** What is DeBERTa v3?
**A:** A small, efficient model described as one of the best for token embeddings (used in the "Hello world" example).

**Q:** What is sentence-transformers?
**A:** A package for leveraging pretrained text-embedding models (e.g., all-mpnet-base-v2).

**Q:** What is Gensim?
**A:** A library for downloading pretrained word embeddings and training Word2Vec models.

**Q:** What does `model.wv.most_similar` return in Gensim?
**A:** A list of the most similar items (by embedding distance) to the given word/song.

---

## Part 2: Short Answer

**Q:** What are the three factors that dictate how a tokenizer breaks down text?
**A:** (1) The tokenization method (BPE, WordPiece, SentencePiece, etc.); (2) tokenizer design parameters (vocabulary size, special tokens, capitalization); (3) the dataset the tokenizer is trained on.

**Q:** How does the space character appear in tokenization?
**A:** It doesn't have its own token — partial tokens carry a special hidden character (e.g., `##` in WordPiece) indicating connection to the preceding token; tokens without it are assumed to have a space before them.

**Q:** Why does subword tokenization fit more text into a context window than character tokens?
**A:** Subword tokens average ~3 characters each, so with a context of 1,024 you can fit roughly three times more text than with character tokens.

**Q:** What did the uncased BERT tokenizer do with the probe text?
**A:** Lowercased everything, dropped newlines, encoded "capitalization" as `capital ##ization`, and replaced emoji/Chinese with [UNK]; wrapped in [CLS]...[SEP].

**Q:** How did the cased BERT tokenizer differ?
**A:** It kept uppercase tokens, expanding "CAPITALIZATION" into 8 tokens (`CA ##PI ##TA ##L ##I ##Z ##AT ##ION`); vocab 28,996.

**Q:** How did GPT-2 handle the emoji 🎵?
**A:** It encoded it as multiple tokens (IDs 8582, 236, 113); `decode([8582,236,113])` reconstructs 🎵.

**Q:** Why is the digit-per-token choice in StarCoder2 helpful?
**A:** Each digit gets its own token (600 → "6 0 0"), giving better number/math representation (vs GPT-2 where 870 = one token but 871 = "8"+"71").

**Q:** What domain-specific special tokens does Galactica use?
**A:** Citations ([START_REF]/[END_REF]), chain-of-thought reasoning (<work>), math, amino acid sequences, and DNA sequences.

**Q:** What makes Galactica unique among the tokenizers in the tour?
**A:** It's the only one assigning a single token to a two-tab string (`\t\t`).

**Q:** What did Phi-3 reuse from Llama 2?
**A:** Its BPE tokenizer (vocab 32,000), with added chat role tokens `<|user|>`, `<|assistant|>`, `<|system|>` and `<|endoftext|>`.

**Q:** Why are whitespace tokens significant for code models?
**A:** A single token for four spaces is more tuned to Python; representing indentation as separate tokens makes modeling harder and hurts performance.

**Q:** Why is a pretrained model linked to its tokenizer?
**A:** The model holds an embedding vector for each token in its tokenizer's vocabulary, so it can't use a different tokenizer without retraining.

**Q:** How are embedding vectors initialized and refined?
**A:** Randomly initialized like other weights, then assigned useful values during training.

**Q:** What does `torch.Size([1, 4, 384])` mean for the DeBERTa example?
**A:** Batch of 1, four tokens ([CLS], Hello, world, [SEP]), each embedded in a 384-value vector.

**Q:** How do text embedding models work?
**A:** They take a piece of text and produce a single vector; commonly by averaging token embeddings, but high-quality models are trained specifically for the task.

**Q:** What is the output shape of all-mpnet-base-v2 for "Best movie ever!"?
**A:** `(768,)` — a single 768-dimensional vector.

**Q:** What are the two main concepts of word2vec?
**A:** Skip-gram (selecting neighboring words) and negative sampling (adding random non-neighbor examples).

**Q:** How does word2vec generate training examples from running text?
**A:** A sliding window (e.g., size 2) picks the central word and its neighbors; the center word + each neighbor forms a positive example; random words form negatives.

**Q:** What is the word2vec training matrix shape?
**A:** vocab_size × embedding_dimensions.

**Q:** How is a song recommender built with word2vec?
**A:** Treat songs as words and playlists as sentences; train `Word2Vec(playlists, vector_size=32, window=20, negative=50, min_count=1, workers=4)`; then `most_similar` finds nearest-neighbor songs.

**Q:** What did the recommender return for "Billie Jean" (3822)?
**A:** Kiss (Prince), Wanna Be Startin' Somethin' (MJ), The Way You Make Me Feel (MJ), Holiday (Madonna), Don't Stop 'Til You Get Enough (MJ).

**Q:** What did the recommender return for Metallica's "Fade to Black" (2172)?
**A:** Van Halen, Dio, Guns N' Roses, Judas Priest — same heavy metal/hard rock genre.

**Q:** Where is the playlist dataset from, and who collected it?
**A:** Collected by Shuo Chen from Cornell University; playlists from hundreds of US radio stations.

**Q:** How are contextualized embeddings used beyond text?
**A:** They power AI image generation systems like DALL·E, Midjourney, and Stable Diffusion.

---

## Part 3: Fill-in-the-Blank

**Q:** LLMs generate their output ______ at a time.
**A:** One token.

**Q:** The model receives ______, not raw text, as its input.
**A:** Token IDs (input_ids).

**Q:** The first token in the Phi-3 example is ID 1, which is the special token ______.
**A:** `<s>` (beginning of text).

**Q:** In WordPiece, the characters ______ indicate a token is a partial token connected to the preceding one.
**A:** `##`.

**Q:** GPT-2's tokenizer method is ______; BERT's is ______.
**A:** BPE; WordPiece.

**Q:** Flan-T5 uses the ______ tokenizer implementation.
**A:** SentencePiece.

**Q:** GPT-2's vocabulary size is ______.
**A:** 50,257.

**Q:** BERT uncased vocabulary size is ______; cased is ______.
**A:** 30,522; 28,996.

**Q:** GPT-4's vocabulary size is a little over ______.
**A:** 100,000.

**Q:** StarCoder2 has ______ billion parameters and a vocabulary of ______.
**A:** 15; 49,152.

**Q:** GPT-4 has a specific token for every whitespace sequence up to ______ whitespaces.
**A:** 83.

**Q:** The word "CAPITALIZATION" is ______ tokens in GPT-2 and ______ tokens in GPT-4.
**A:** Four; two.

**Q:** In GPT-2, the tab is token number ______ and the space sequence uses token number ______.
**A:** 197; 220.

**Q:** Galactica's vocabulary size is ______ and it's focused on ______ knowledge.
**A:** 50,000; scientific.

**Q:** Phi-3/Llama 2 vocabulary size is ______.
**A:** 32,000.

**Q:** Phi-3 adds chat role tokens ______, ______, and ______.
**A:** `<|user|>`; `<|assistant|>`; `<|system|>`.

**Q:** A model with a context of 1,024 fits about ______ as much text with subword vs character tokens.
**A:** Three times.

**Q:** Subword tokens average about ______ characters per token.
**A:** Three.

**Q:** Token IDs → embeddings is the ______ step that occurs inside a language model.
**A:** First.

**Q:** DeBERTa processed "Hello world" into ______ tokens, each in a ______-value vector.
**A:** Four; 384.

**Q:** The all-mpnet-base-v2 embedding vector has ______ dimensions.
**A:** 768.

**Q:** The Gensim download `glove-wiki-gigaword-50` is ______ MB, Wikipedia-trained, with vector size ______.
**A:** 66; 50.

**Q:** The nearest neighbors of "king" included ______, ______, and ______.
**A:** prince; queen; emperor (also ii, son, uncle, kingdom, throne, brother, ruler).

**Q:** word2vec's two main ideas are ______ and ______.
**A:** Skip-gram; negative sampling.

**Q:** The word2vec embedding matrix has dimensions ______ × ______.
**A:** vocab_size × embedding_dimensions.

**Q:** The song playlist Word2Vec model used vector_size ______, window ______, negative ______.
**A:** 32; 20; 50.

**Q:** "Fade To Black" is by ______; its recommendations were in the ______ genre.
**A:** Metallica; heavy metal / hard rock.

**Q:** Negative example selection is inspired by ______ estimation.
**A:** Noise-contrastive.

**Q:** The two-vector relation-prediction idea is central to Chapter ______ (embeddings/retrieval) and Chapter ______ (multimodal).
**A:** 10; 9.

**Q:** RAG stands for ______.
**A:** Retrieval-augmented generation.
