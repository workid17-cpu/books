# 📘 Chapter 2 Study Bundle: Tokens and Embeddings
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 2

---

## §1. Study Notes

### Core Theme
Tokens are how LLMs see and produce text, and embeddings are the numeric representations that let models compute over those tokens — this chapter covers tokenization methods (word/subword/character/byte), a guided tour of real tokenizers (BERT, GPT-2, GPT-4, StarCoder2, Galactica, Phi-3), the three tokenizer design choices, static vs contextualized embeddings, text/sentence embeddings, and how word2vec (skip-gram + negative sampling) extends beyond LLMs into recommendation systems.

### Key Definitions
- **Token**: A small chunk of text — a character, word, or part of a word — that an LLM processes.
- **Tokenization**: The process of breaking text into tokens.
- **Tokenizer**: The component that splits input text into tokens (and translates output token IDs back into text). Two roles: prepare model input, decode model output.
- **Token ID**: A unique integer ID referencing a token in the tokenizer's vocabulary table.
- **Vocabulary**: The table of all tokens a tokenizer knows.
- **Special token**: A unique token with a role other than representing text (e.g., beginning-of-text, end-of-text, padding, unknown, [CLS], [SEP], [MASK], chat role tokens, FIM tokens).
- **BPE (byte pair encoding)**: Tokenization method used by GPT models; aims to optimize an efficient set of tokens to represent a dataset.
- **WordPiece**: Tokenization method used by BERT.
- **SentencePiece**: Tokenizer implementation used by Flan-T5; supports BPE and unigram language model.
- **Word tokens**: Tokens that are whole words (common in word2vec-era methods).
- **Subword tokens**: Tokens that are full or partial words (the most common scheme; e.g., BPE).
- **Character tokens**: Tokens that are individual characters.
- **Byte tokens**: Tokens that are individual bytes used to represent unicode characters; "tokenization-free encoding" (e.g., CANINE, ByT5).
- **Unk token**: `[UNK]`/`<unk>` — the unknown token used when the tokenizer has no encoding for a character.
- **Embedding**: A vector representation of data that attempts to capture its meaning.
- **Embeddings matrix**: The matrix holding an embedding vector for each token in the tokenizer's vocabulary; a portion of a downloaded pretrained model.
- **Contextualized word embeddings**: Word representations that differ based on context, produced by language models (vs static embeddings).
- **Text embeddings**: A single vector representing a sentence, paragraph, or document.
- **Static embeddings**: Fixed, context-free word embeddings (e.g., word2vec, GloVe).
- **word2vec**: 2013 algorithm producing word embeddings via a sliding window, skip-gram, and negative sampling.
- **GloVe**: Pretrained word embeddings (e.g., glove-wiki-gigaword-50, 50-dimensional, trained on Wikipedia).
- **Skip-gram**: The word2vec method of selecting neighboring words as training examples.
- **Negative sampling**: Adding negative examples (random words that are not neighbors) by random sampling from the dataset.
- **Contrastive training**: A model takes two vectors and predicts if they have a certain relation; a powerful idea used by word2vec, and central to Chapter 10 (sentence embeddings/retrieval) and Chapter 9 (multimodal).
- **Cross-encoder**: A model given two texts at once (used with [SEP]); e.g., for reranking (Ch 8).
- **Noise-contrastive estimation**: The important idea inspiring random negative example selection.
- **RAG (retrieval-augmented generation)**: Combines search and LLMs because generative models alone aren't reliable search engines (Ch 8).
- **NER (named-entity recognition)**: A task powered by contextualized token embeddings.
- **Extractive text summarization**: Summarizes a long text by highlighting the most important parts (vs generating new text).
- **FIM (fill-in-the-middle) tokens**: `<|fim_prefix|>`, `<|fim_middle|>`, `<|fim_suffix|>` — enable an LLM to generate a completion given both text before and after.
- **Hugging Face**: Organization behind Transformers; hosts models; `AutoTokenizer`/`AutoModel`/`pipeline`.
- **DeBERTa v3**: Small, efficient model good for token embeddings (used in the contextualized-embedding example).
- **all-mpnet-base-v2**: Embedding model from sentence-transformers producing 768-dim text embeddings.
- **Gensim**: Library for downloading pretrained word embeddings (word2vec, GloVe) and training Word2Vec models.

### Core Concepts & Frameworks
- **Tokens are both input and output**: models generate one token at a time; a prompt is first broken into tokens; the model receives `input_ids`.
- **Tokenizer's dual role**: (1) process input text → token IDs for the model; (2) process model output → translate token IDs back into text (via `decode`).
- **Token spaces**: punctuation is its own token; the space character does not have its own token — partial tokens (e.g., "izing") carry a special hidden character (like `##` in WordPiece) indicating connection to the preceding token; tokens without it are assumed to have a space before them.
- **Three factors dictating tokenization**:
  1. **Method** chosen at model design time (BPE, WordPiece, SentencePiece).
  2. **Parameters/design choices** — vocabulary size, special tokens, capitalization handling.
  3. **Training dataset** — same method+params but different dataset (English vs code vs multilingual) yields different tokenizers.
- **Four tokenization schemes (Figure 2-6)**:
  - **Word tokens**: common with word2vec; challenges: can't handle new words; large vocabulary with minimal differences (apology/apologize/apologetic/apologist). Subword fixes this with shared roots + suffixes (-y, -ize, -etic, -ist).
  - **Subword tokens**: full + partial words; expressive; can represent new words by breaking them into smaller known characters.
  - **Character tokens**: handles new words via raw letters; but modeling is harder (must spell "p-l-a-y"); subword fits ~3x more text in a context (subword avg ~3 chars/token).
  - **Byte tokens**: break tokens into unicode bytes; "tokenization-free encoding" (CANINE, ByT5); competitive especially in multilingual scenarios. Some subword tokenizers (GPT-2, RoBERTa) include bytes as fallback tokens — not fully byte-level.
- **Guided tour findings (each tokenizer)**:
  - **BERT uncased (WordPiece, vocab 30,522)**: [UNK] for emoji/Chinese; lowercase; no newlines; `capital##ization`; [CLS]...[SEP] wrapping.
  - **BERT cased (vocab 28,996)**: keeps uppercase; "CAPITALIZATION" = 8 tokens (`CA ##PI ##TA ##L ##I ##Z ##AT ##ION`).
  - **GPT-2 (BPE, vocab 50,257)**: newlines preserved; capitalization preserved ("CAPITALIZATION" = 4 tokens); emoji 🎵 = tokens 8582, 236, 113; tabs = token 197; four spaces = three tokens (220) with final space part of quote token.
  - **Flan-T5 (SentencePiece, vocab 32,100)**: no newline/whitespace tokens (bad for code); emoji/Chinese → `<unk>`.
  - **GPT-4 (BPE, vocab ~100,000+)**: four spaces = single token (specific token for each whitespace sequence up to 83); `elif` has its own token; fewer tokens for most words (CAPITALIZATION=2, tokens=1); FIM tokens `<|fim_prefix|>`, `<|fim_middle|>`, `<|fim_suffix|>`.
  - **StarCoder2 (BPE, vocab 49,152, 15B params)**: whitespace lists = single token; each digit its own token (600→6 0 0, better for math); repo/filename tokens `<filename>`, `<reponame>`, `<gh_stars>`; FIM tokens incl `<fim_pad>`.
  - **Galactica (BPE, vocab 50,000, science-focused)**: single token for whitespace sequences AND tabs (`\t\t`); special tokens for citations (`[START_REF]`/`[END_REF]`), reasoning (`<work>` chain-of-thought), math, amino acid and DNA sequences.
  - **Phi-3 / Llama 2 (BPE, vocab 32,000)**: Phi-3 reuses Llama 2's tokenizer + chat role tokens `<|user|>`, `<|assistant|>`, `<|system|>`; `<|endoftext|>`.
- **Whitespace significance**: a model with a single token for four consecutive spaces is more tuned to Python; tokenization choices improve task performance.
- **Language as token sequence + embeddings**: if training data has lots of English → English capability; factual data (Wikipedia) → some factual generation. Embeddings are the numeric representation space for capturing meaning/patterns.
- **RAG motivation ("Oops" box)**: around early 2023 some models were "Google killers"; generative models alone aren't reliable search engines → RAG (search + LLM), Ch 8.
- **Model holds embeddings for tokenizer vocab**: a pretrained model is linked with its tokenizer and can't use a different one without training. Embeddings are randomly initialized then trained into useful values.
- **Contextualized word embeddings**: LMs create token embeddings that differ by context (better than static); power NER, extractive summarization, text classification; also power DALL·E, Midjourney, Stable Diffusion.
- **Text embeddings**: sentence-transformers package; `SentenceTransformer("sentence-transformers/all-mpnet-base-v2")` → 768-dim vector for "Best movie ever!"; used for categorization, semantic search, RAG (Part II).
- **word2vec beyond LLMs**: embedding objects (songs, etc.) useful for recommender engines and robotics. Gensim loads `glove-wiki-gigaword-50` (66MB, Wikipedia-trained, 50-dim); `most_similar` finds nearest neighbors ("king" → prince, queen, emperor, ...).
- **word2vec training**: sliding window (e.g., size 2) generates training examples from running text; classification task: two words → 1 if neighbors, 0 if not; negative examples (random words) needed to prevent cheating (predict 1 always); skip-gram = selecting neighbors; negative sampling = random non-neighbors.
- **Recommendation systems via word2vec**: treat each song as a word/token and each playlist as a sentence; train Word2Vec on playlists (vector_size=32, window=20, negative=50, min_count=1, workers=4); nearest-neighbor songs are recommendations (e.g., "Billie Jean" → Kiss, Madonna, other MJ; "Fade to Black" → Van Halen, Dio, Guns N' Roses). Dataset by Shuo Chen (Cornell) — playlists from hundreds of US radio stations.
- **Contrastive training's reach**: two-vector relation prediction — core of Chapter 10 (sentence embeddings/retrieval) and Chapter 9 (multimodal: image + caption → describes or not).

### Important Numbers / Stats / Tokens
- GPT-4 tokenizer vocabulary size: a little over 100,000 (p.14).
- GPT-2 vocab: 50,257 (p.13); BERT uncased 30,522 (p.11); BERT cased 28,996 (p.12); Flan-T5 32,100 (p.14); StarCoder2 49,152 (p.15); Galactica 50,000 (p.16); Phi-3/Llama 2 32,000 (p.17).
- GPT-2 tab = token 197; four spaces = tokens (number 220), final space part of quote token (p.13).
- Emoji 🎵 = GPT-2 tokens 8582, 236, 113; `decode([8582,236,113])` → 🎵 (p.13).
- Subword tokens average ~3 characters; subword fits ~3x more text than character tokens in a context of 1,024 (p.9).
- GPT-4 whitespace tokens: every sequence of whitespaces up to 83 (p.15).
- StarCoder2: 15B params (p.15); each digit its own token (p.16); Galactica tabs `\t\t` single token (p.17).
- Example context length 1,024 (p.9).
- Vocabulary sizes often 30K/50K, increasingly 100K (p.19).
- `input_ids` example: `<s>`=1; `tokenizer.decode(3323)`='Sub', `decode(622)`='ject', `decode([3323,622])`='Subject', `decode(29901)`=':' (p.6).
- DeBERTa v3 output shape: `torch.Size([1, 4, 384])` — batch 1, 4 tokens ([CLS] Hello world [SEP]), 384 embedding dims (p.24).
- all-mpnet-base-v2: `vector.shape` = `(768,)` (p.26).
- Gensim glove-wiki-gigaword-50: 66MB, Wikipedia-trained, vector size 50 (p.27).
- Word2Vec playlist model params: vector_size=32, window=20, negative=50, min_count=1, workers=4 (p.33).
- Phi-3 chat tokens: `<|user|>`, `<|assistant|>`, `<|system|>` (p.17).
- GPT-2 special token: `<|endoftext|>` (p.13).
- "king" nearest neighbors: prince 0.82, queen 0.78, ii 0.77, emperor 0.77, son 0.77, uncle 0.76, kingdom 0.75, throne 0.75, brother 0.75, ruler 0.74 (p.27).
- DeBERTa v3 described in "DeBERTaV3: Improving DeBERTa using ELECTRA-style pre-training gradient-disentangled embedding sharing" (p.23).

### Algorithms & Formulæ
- **LLM text generation pipeline**:
  1. Prompt → tokenizer → `input_ids` (tensor of token IDs).
  2. `model.generate(input_ids, max_new_tokens=20)` → generates token IDs.
  3. `tokenizer.decode(generation_output[0])` → human-readable text.
- **word2vec training** (step-by-step):
  1. Slide a window (size N) over running text to generate training examples.
  2. Each example: central word + a neighbor (positive example, label 1).
  3. Add negative examples (random words, label 0) via negative sampling.
  4. Create embedding vector per token, randomly initialized (matrix vocab_size × embedding_dimensions).
  5. Train a neural network: take two embedding vectors, predict related or not.
  6. Update embeddings based on correctness; final trained embeddings emerge.
- **Song recommendation pipeline**:
  1. Load playlist dataset (each playlist = list of song IDs); load song metadata (id/title/artist).
  2. Train `Word2Vec(playlists, vector_size=32, window=20, negative=50, min_count=1, workers=4)`.
  3. `model.wv.most_similar(positive=str(song_id))` → nearest-neighbor songs.
  4. Map IDs to titles/artists via the metadata DataFrame.

### Diagrams / Visuals
- **Figure 2-1** — LLMs deal with text in chunks called tokens; tokens → numeric embeddings.
- **Figure 2-2** — High-level view: language model + input prompt.
- **Figure 2-3** — Tokenizer breaks text into words/parts of words (color-coded) per method + training.
- **Figure 2-4** — Tokenizer processes prompt → list of token IDs (input to model).
- **Figure 2-5** — Tokenizer converts output token IDs back into words.
- **Figure 2-6** — Four tokenization methods: words, subwords, characters, bytes.
- **Figure 2-7** — A language model holds an embedding vector per token in its tokenizer.
- **Figure 2-8** — Language models produce contextualized token embeddings (improve on static).
- **Figure 2-9** — Model operates on raw static embeddings → produces contextual text embeddings (key visual for Ch 3).
- **Figure 2-10** — Text embedding model extracts features → converts input text to embeddings (single vector).
- **Figure 2-11** — Sliding window generates training examples for word2vec.
- **Figure 2-12** — Each training example shows a pair of neighboring words.
- **Figure 2-13** — Negative examples: words not usually neighbors.
- **Figure 2-14** — Skip-gram (selecting neighboring words) and negative sampling (random negative examples).
- **Figure 2-15** — Vocabulary of words with random, uninitialized embedding vectors.
- **Figure 2-16** — Neural network trained to predict if two words are neighbors; embeddings updated.
- **Figure 2-17** — Song-playlist dataset (playlists each containing lists of songs).

### Common Exam Traps
- **Tokenizers have two jobs**: both input (text→IDs) and output (IDs→text). Don't forget the decode side.
- **The space character has no own token**: spaces are implied by a special hidden character on partial tokens (or `##` in WordPiece). Not a separate token.
- **Token IDs are not embeddings**: IDs are integers referencing the vocabulary; the model then looks up embeddings (randomly initialized, trained).
- **word2vec embeddings are static**: "bank" always the same, regardless of context — the motivation for contextualized embeddings.
- **Subword vs character context fit**: subword fits ~3x more text than character tokens in the same context window (not the reverse).
- **Byte-level ≠ includes bytes as fallback**: GPT-2/RoBERTa include bytes as fallback tokens but aren't byte-level tokenizers.
- **BERT uncased vs cased**: uncased lowercases and drops newlines; cased keeps uppercase and expands words into more tokens.
- **GPT-4's code focus**: whitespace-run tokens, `elif` own token, FIM tokens — all stem from code + natural language focus.
- **StarCoder2 digit-per-token**: 600 → "6 0 0" to better represent numbers/math (vs GPT-2 where 870 = one token but 871 = "8"+"71").
- **Galactica's science tokens**: citations `[START_REF]`, reasoning `<work>`; also assigns single token to two tabs.
- **Phi-3 reuses Llama 2's tokenizer**: plus chat tokens `<|user|>`, `<|assistant|>`, `<|system|>`.
- **ChatGPT = product; not a tokenizer/model family in this chapter's sense** — recall Ch 1's distinction.
- **Random negative examples work**: noise-contrastive estimation inspired; you don't need curated negatives.
- **A pretrained model can't swap tokenizers without retraining**: model embeddings are tied to its tokenizer's vocabulary.
- **word2vec → contrastive training**: two-vector relation prediction is the same idea behind sentence embeddings (Ch 10) and multimodal image-caption matching (Ch 9).
- **text embeddings ≠ token embeddings**: token = per-token; text = single vector for the whole sentence/document.
- **`min_count=1` in Word2Vec**: keeps all songs even singletons in the playlist dataset context.

### Chapter Summary
Chapter 2 explains the two foundational concepts of working with LLMs. Tokens are the units models process — a prompt is tokenized into token IDs (via the tokenizer), and the model's output IDs are decoded back to text. Tokenization schemes span word, subword (the dominant approach), character, and byte levels, each with trade-offs in vocabulary expressivity, handling of unseen words, and context-window utilization. The three design choices — method (BPE, WordPiece, SentencePiece), parameters (vocab size, special tokens, capitalization), and training dataset — shape every tokenizer; a guided tour of BERT, GPT-2, Flan-T5, GPT-4, StarCoder2, Galactica, and Phi-3 shows how newer, code-focused tokenizers handle whitespace, digits, FIM tokens, and domain-specific tokens.

The second half covers embeddings. Language models convert token IDs into embeddings (randomly initialized, then trained), and can produce contextualized word embeddings that improve on static vectors, powering NER, extractive summarization, text classification, and image-generation systems. Text embedding models (e.g., all-mpnet-base-v2 via sentence-transformers) produce a single 768-dim vector per sentence/document for semantic search and RAG. Finally, word2vec — built on skip-gram and negative sampling via contrastive training — extends beyond language to recommendation systems, demonstrated by embedding songs from playlists to recommend similar music (e.g., "Billie Jean" → Prince/Madonna/MJ; "Fade to Black" → Van Halen/Dio/Guns N' Roses). This prepares readers for Chapter 3 (how the Transformer processes tokens) and Chapter 10 (contrastive training for embeddings/retrieval).

### Confidence Check
- **Sure**: tokenizer input/output dual role; four tokenization schemes; BERT uncased/cased differences; GPT-2 vocab 50,257 + emoji tokens; GPT-4 whitespace/elif/FIM; StarCoder2 digit-per-token; Galactica science tokens; Phi-3/Llama-2 shared tokenizer + chat tokens; word2vec skip-gram + negative sampling; song recommendation pipeline; DeBERTa `[1,4,384]`; all-mpnet-base-v2 `(768,)`.
- **Uncertain**: Exact figure numbers for Figures 2-7 through 2-17 in the printed page flow (page anchors from PDF text are approximate); the precise maximum in GPT-4's whitespace token list ("a list of 83 whitespaces" quoted from text); minor — whether the "three times" context claim uses the exact 1,024 figure in all editions.

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading the model and tokenizer (Ch 1 recap)
```python
from transformers import AutoModelForCausalLM, AutoTokenizer
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
```
- **Explanation**: Same loading pattern as Chapter 1. `AutoModelForCausalLM` loads the decoder-only generator; `AutoTokenizer` loads its tokenizer.
- **Fits the architecture**: The tokenizer will convert the prompt to `input_ids`; the model generates new token IDs; the tokenizer decodes them.

### Code Block 2: Tokenizing the prompt and generating
```python
prompt = "Write an email apologizing to Sarah for the tragic gardening mishap. Explain how it happened.<|assistant|>"
input_ids = tokenizer(prompt, return_tensors="pt").input_ids.to("cuda")
generation_output = model.generate(
  input_ids=input_ids,
  max_new_tokens=20
)
print(tokenizer.decode(generation_output[0]))
```
- **Explanation**: The prompt (with `<|assistant|>` chat token) is tokenized into `input_ids`; `model.generate` creates 20 new tokens; `tokenizer.decode` reconstructs the text. The bold portion is the 20 generated tokens ("Subject: My Sincere Apologies...").
- **Fits the architecture**: The model never sees raw text — only the integer token IDs.

### Code Block 3: Inspecting input token IDs
```python
for id in input_ids[0]:
   print(tokenizer.decode(id))
```
- **Explanation**: Decodes each token ID separately, showing `<s>`, "Write", "an", "email", "apolog", "izing", ..., "happened", ".", `<|assistant|>`. Demonstrates word-level, subword-level, and punctuation tokens.
- **Fits the architecture**: `<s>` (ID 1) is the beginning-of-text special token; partial tokens carry a hidden connection marker.

### Code Block 4: Decoding output tokens
```python
print(tokenizer.decode(3323))   # Sub
print(tokenizer.decode(622))    # ject
print(tokenizer.decode([3323, 622]))  # Subject
print(tokenizer.decode(29901))  # :
```
- **Explanation**: Individual and combined token decoding. `3323`+`622` = "Subject", `29901` = ":".
- **Fits the architecture**: The tokenizer translates output IDs back to text, just as it tokenizes input.

### Code Block 5: The token-visualization helper
```python
colors_list = [
    '102;194;165', '252;141;98', '141;160;203',
    '231;138;195', '166;216;84', '255;217;47'
]
def show_tokens(sentence, tokenizer_name):
    tokenizer = AutoTokenizer.from_pretrained(tokenizer_name)
    token_ids = tokenizer(sentence).input_ids
    for idx, t in enumerate(token_ids):
        print(
            f'\x1b[0;30;48;2;{colors_list[idx % len(colors_list)]}m' +
            tokenizer.decode(t) +
            '\x1b[0m',
            end=' '
        )
```
- **Explanation**: Loads any tokenizer by name and prints each decoded token with a rotating ANSI background color.
- **Fits the architecture**: Color-codes tokens to make tokenization boundaries visible across models (the guided tour).

### Code Block 6: The comparison text
```python
text = """
English and CAPITALIZATION
🎵鸟
show_tokens False None elif == >= else: two tabs:" " Three tabs: "   "
12.0*50=600
"""
```
- **Explanation**: A deliberately varied probe covering capitalization, non-English script, emoji, code keywords, whitespace/indentation, and numbers.
- **Fits the architecture**: Highlights how each tokenizer treats these token categories.

### Code Block 7: Contextualized embeddings with DeBERTa
```python
from transformers import AutoModel, AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("microsoft/deberta-base")
model = AutoModel.from_pretrained("microsoft/deberta-v3-xsmall")
tokens = tokenizer('Hello world', return_tensors='pt')
output = model(**tokens)[0]
```
- **Explanation**: Loads a tokenizer + model (DeBERTa v3 xsmall), tokenizes "Hello world", and runs the model. `output.shape` = `torch.Size([1, 4, 384])`: batch 1, four tokens ([CLS], Hello, world, [SEP]), 384 dims each.
- **Fits the architecture**: Token IDs → static embeddings (first step inside the model) → contextualized text embeddings.

### Code Block 8: Text embeddings with sentence-transformers
```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")
vector = model.encode("Best movie ever!")
```
- **Explanation**: Loads a text-embedding model and encodes a whole sentence into one vector. `vector.shape` = `(768,)`.
- **Fits the architecture**: A single vector represents the entire sentence — the basis for semantic search, classification, clustering, RAG.

### Code Block 9: Pretrained word embeddings with Gensim
```python
import gensim.downloader as api
model = api.load("glove-wiki-gigaword-50")
model.most_similar([model['king']], topn=11)
```
- **Explanation**: Downloads 50-dim GloVe embeddings trained on Wikipedia (66MB), then finds nearest neighbors of "king" (prince, queen, emperor, ...).
- **Fits the architecture**: Static word embeddings; similarity measured in the vector space.

### Code Block 10: Song recommendation data loading
```python
import pandas as pd
from urllib import request
data = request.urlopen('https://storage.googleapis.com/maps-premium/dataset/yes_complete/train.txt')
lines = data.read().decode("utf-8").split('\n')[2:]
playlists = [s.rstrip().split() for s in lines if len(s.split()) > 1]
songs_file = request.urlopen('https://storage.googleapis.com/maps-premium/dataset/yes_complete/song_hash.txt')
songs_file = songs_file.read().decode("utf-8").split('\n')
songs = [s.rstrip().split('\t') for s in songs_file]
songs_df = pd.DataFrame(data=songs, columns = ['id', 'title', 'artist'])
songs_df = songs_df.set_index('id')
```
- **Explanation**: Downloads playlist data (skips 2 metadata lines, keeps playlists with >1 song), loads song metadata into a DataFrame indexed by song ID.
- **Fits the architecture**: Songs act as "words" and playlists as "sentences" for word2vec.

### Code Block 11: Training the song embedding model
```python
from gensim.models import Word2Vec
model = Word2Vec(
    playlists, vector_size=32, window=20, negative=50, min_count=1, workers=4
)
```
- **Explanation**: Trains word2vec on playlists (32-dim embeddings, window 20, 50 negative samples). Takes a minute or two.
- **Fits the architecture**: Same skip-gram + negative sampling as text word2vec, applied to songs.

### Code Block 12: Recommendations
```python
song_id = 2172
model.wv.most_similar(positive=str(song_id))
```
```python
import numpy as np
def print_recommendations(song_id):
    similar_songs = np.array(
        model.wv.most_similar(positive=str(song_id),topn=5)
    )[:,0]
    return  songs_df.iloc[similar_songs]
print_recommendations(2172)
```
- **Explanation**: `most_similar` returns nearest-neighbor song IDs; `print_recommendations` maps them to titles/artists via `songs_df`. Song 2172 ("Fade To Black", Metallica) → Van Halen, Dio, Guns N' Roses, Judas Priest.
- **Fits the architecture**: Embedding proximity = recommendation similarity.

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*
