# 📘 Chapter 2 Line-by-Line: Tokens and Embeddings
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 2
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "Tokens and embeddings are two of the central concepts of using large language models (LLMs). As we've seen in the first chapter, they're not only important to understanding the history of Language AI, but we cannot have a clear sense of how LLMs work, how they're built, and where they will go in the future without a good sense of tokens and embeddings."
   - **Plain English:** Tokens and embeddings are essential to understanding LLMs.
   - **Word meanings:** central = core/fundamental.
   - **Technical terms:** tokens = text chunks; embeddings = numeric representations.

2. **Quote:** "In this chapter, we look more closely at what tokens are and the tokenization methods used to power LLMs. We will then dive into the famous word2vec embedding method that preceded modern-day LLMs and see how it's extending the concept of token embeddings to build commercial recommendation systems that power a lot of the apps you use. Finally, we go from token embeddings into sentence or text embeddings, where a whole sentence or document can have one vector that represents it—enabling applications like semantic search and topic modeling."
   - **Plain English:** Chapter roadmap: tokenization, word2vec, recommendations, text embeddings.
   - **Word meanings:** preceded = came before.
   - **Technical terms:** semantic search; topic modeling; word2vec.

---

## LLM Tokenization

3. **Quote:** "The way the majority of people interact with language models, at the time of this writing, is through a web playground that presents a chat interface between the user and a language model. You may notice that a model does not produce its output response all at once; it actually generates one token at a time."
   - **Plain English:** LLMs generate output token-by-token.
   - **Word meanings:** playground = web demo interface.
   - **Technical terms:** token-by-token generation.

4. **Quote:** "But tokens aren't only the output of a model, they're also the way in which the model sees its inputs. A text prompt sent to the model is first broken down into tokens."
   - **Plain English:** Tokens are both input and output format.
   - **Technical terms:** input tokens; output tokens.

### How Tokenizers Prepare the Inputs to the Language Model

5. **Quote:** "Viewed from the outside, generative LLMs take an input prompt and generate a response... Before the prompt is presented to the language model, however, it first has to go through a tokenizer that breaks it into pieces."
   - **Plain English:** A tokenizer sits between the prompt and the model.
   - **Technical terms:** tokenizer.

6. **Quote:** "You can find an example showing the tokenizer of GPT-4 on the OpenAI Platform. If we feed it the input text, it shows the output in Figure 2-3, where each token is shown in a different color."
   - **Plain English:** The GPT-4 tokenizer can be visualized color-coded.
   - **Technical terms:** GPT-4 tokenizer.

### Downloading and Running an LLM

7. **Quote:** "Let's start by loading our model and its tokenizer as we've done in Chapter 1... We can then proceed to the actual generation. We first declare our prompt, then tokenize it, then pass those tokens to the model, which generates its output. In this case, we're asking the model to only generate 20 new tokens."
   - **Plain English:** Load model+tokenizer, tokenize prompt, generate (20 tokens).
   - **Technical terms:** `input_ids`; `model.generate`.

8. **Quote:** "Looking at the code, we can see that the model does not in fact receive the text prompt. Instead, the tokenizers processed the input prompt, and returned the information the model needed in the variable input_ids, which the model used as its input."
   - **Plain English:** The model receives token IDs, not raw text.
   - **Technical terms:** `input_ids`.

9. **Quote:** "This reveals the inputs that LLMs respond to, a series of integers... Each one is the unique ID for a specific token (character, word, or part of a word). These IDs reference a table inside the tokenizer containing all the tokens it knows."
   - **Plain English:** Token IDs are integers referencing a vocabulary table.
   - **Technical terms:** token ID; vocabulary table.

10. **Quote:** "If we want to inspect those IDs, we can use the tokenizer's decode method to translate the IDs back into text that we can read."
    - **Plain English:** `decode` converts IDs back to readable text.
    - **Technical terms:** decode.

11. **Quote:** "This is how the tokenizer broke down our input prompt. Notice the following: The first token is ID 1 (<s>), a special token indicating the beginning of the text. Some tokens are complete words (e.g., Write, an, email). Some tokens are parts of words (e.g., apolog, izing, trag, ic). Punctuation characters are their own token."
    - **Plain English:** Tokenizer output mixes special, whole-word, subword, and punctuation tokens.
    - **Technical terms:** special token; subword token.

12. **Quote:** "Notice how the space character does not have its own token. Instead, partial tokens (like 'izing' and 'ic') have a special hidden character at their beginning that indicates that they're connected with the token that precedes them in the text. Tokens without that special character are assumed to have a space before them."
    - **Plain English:** Spaces are implied by hidden markers on partial tokens.
    - **Technical terms:** hidden character / `##` (WordPiece) markers.

13. **Quote:** "On the output side, we can also inspect the tokens generated by the model by printing the generation_output variable. This shows the input tokens as well as the output tokens."
    - **Plain English:** `generation_output` contains input + generated IDs.
    - **Technical terms:** generation output tensor.

14. **Quote:** "This shows us the model generated the token 3323, 'Sub', followed by token 622, 'ject'. Together they formed the word 'Subject'. They were then followed by token 29901, which is the colon ':'... Just like on the input side, we need the tokenizer on the output side to translate the token ID into the actual text."
    - **Plain English:** Output IDs are also decoded via the tokenizer (e.g., 3323+622 = "Subject").
    - **Technical terms:** token decoding.

### How Does the Tokenizer Break Down Text?

15. **Quote:** "There are three major factors that dictate how a tokenizer breaks down an input prompt."
    - **Plain English:** Three factors determine tokenization.
    - **Technical terms:** method; parameters; dataset.

16. **Quote:** "First, at model design time, the creator of the model chooses a tokenization method. Popular methods include byte pair encoding (BPE) (widely used by GPT models) and WordPiece (used by BERT). These methods are similar in that they aim to optimize an efficient set of tokens to represent a text dataset, but they arrive at it in different ways."
    - **Plain English:** Factor 1 = method (BPE, WordPiece, etc.).
    - **Technical terms:** BPE; WordPiece.

17. **Quote:** "Second, after choosing the method, we need to make a number of tokenizer design choices like vocabulary size and what special tokens to use."
    - **Plain English:** Factor 2 = design parameters (vocab size, special tokens).
    - **Technical terms:** vocabulary size; special tokens.

18. **Quote:** "Third, the tokenizer needs to be trained on a specific dataset to establish the best vocabulary it can use to represent that dataset. Even if we set the same methods and parameters, a tokenizer trained on an English text dataset will be different from another trained on a code dataset or a multilingual text dataset."
    - **Plain English:** Factor 3 = training dataset domain.
    - **Technical terms:** trained tokenizer.

19. **Quote:** "In addition to being used to process the input text into a language model, tokenizers are used on the output of the language model to turn the resulting token ID into the output word or token associated with it."
    - **Plain English:** Tokenizers handle both input and output.
    - **Technical terms:** dual role of tokenizer.

### Word Versus Subword Versus Character Versus Byte Tokens

20. **Quote:** "The tokenization scheme we just discussed is called subword tokenization. It's the most commonly used tokenization scheme but not the only one. The four notable ways to tokenize are... Word tokens, Subword tokens, Character tokens, Byte tokens."
    - **Plain English:** Four tokenization schemes.
    - **Technical terms:** word, subword, character, byte tokens.

21. **Quote:** "Word tokens — This approach was common with earlier methods like word2vec but is being used less and less in NLP... One challenge with word tokenization is that the tokenizer may be unable to deal with new words that enter the dataset after the tokenizer was trained. This also results in a vocabulary that has a lot of tokens with minimal differences between them (e.g., apology, apologize, apologetic, apologist)."
    - **Plain English:** Word tokens can't handle new words and create near-duplicate vocab entries.
    - **Technical terms:** out-of-vocabulary (OOV) words.

22. **Quote:** "This latter challenge is resolved by subword tokenization as it has a token for apolog, and then suffix tokens (e.g., -y, -ize, -etic, -ist) that are common with many other tokens, resulting in a more expressive vocabulary."
    - **Plain English:** Subword shares roots and suffixes, boosting expressivity.
    - **Word meanings:** expressive = able to represent more.
    - **Technical terms:** subword tokens; suffixes.

23. **Quote:** "Subword tokens — This method contains full and partial words. In addition to the vocabulary expressivity mentioned earlier, another benefit of the approach is its ability to represent new words by breaking down the new token into smaller characters, which tend to be a part of the vocabulary."
    - **Plain English:** Subword handles new words by decomposing into known pieces.
    - **Technical terms:** decomposition into subwords.

24. **Quote:** "Character tokens — This is another method that can deal successfully with new words because it has the raw letters to fall back on. While that makes the representation easier to tokenize, it makes the modeling more difficult. Where a model with subword tokenization can represent 'play' as one token, a model using character-level tokens needs to model the information to spell out 'p-l-a-y' in addition to modeling the rest of the sequence."
    - **Plain English:** Character tokens handle new words but make modeling harder.
    - **Technical terms:** character-level modeling.

25. **Quote:** "Subword tokens present an advantage over character tokens in the ability to fit more text within the limited context length of a Transformer model. So with a model with a context length of 1,024, you may be able to fit about three times as much text using subword tokenization than using character tokens (subword tokens often average three characters per token)."
    - **Plain English:** Subword packs ~3x more text into the context window than character tokens.
    - **Technical terms:** context length; compression ratio.

26. **Quote:** "Byte tokens — One additional tokenization method breaks down tokens into the individual bytes that are used to represent unicode characters. Papers like 'CANINE...' outline methods like this, which are also called 'tokenization-free encoding.' Other works like 'ByT5...' show that this can be a competitive method, especially in multilingual scenarios."
    - **Plain English:** Byte tokens work on unicode bytes; useful for multilingual.
    - **Technical terms:** byte-level tokenization; CANINE; ByT5.

27. **Quote:** "One distinction to highlight here: some subword tokenizers also include bytes as tokens in their vocabulary as the final building block to fall back to when they encounter characters they can't otherwise represent. The GPT-2 and RoBERTa tokenizers do this, for example. This doesn't make them tokenization-free byte-level tokenizers, because they don't use these bytes to represent everything, only a subset."
    - **Plain English:** Byte-fallback in subword tokenizers ≠ byte-level tokenization.
    - **Technical terms:** byte fallback; byte-level tokenizers.

### Comparing Trained LLM Tokenizers

28. **Quote:** "We've pointed out earlier three major factors that dictate the tokens that appear within a tokenizer: the tokenization method, the parameters and special tokens we use to initialize the tokenizer, and the dataset the tokenizer is trained on. Let's compare and contrast a number of actual, trained tokenizers... This comparison will show us that newer tokenizers have changed their behavior to improve model performance, and we'll also see how specialized models (like code generation models, for example) often need specialized tokenizers."
    - **Plain English:** Comparing real tokenizers reveals how choices affect behavior and performance.
    - **Technical terms:** specialized tokenizers.

29. **Quote:** "We'll use a number of tokenizers to encode the following text... This will allow us to see how each tokenizer deals with a number of different kinds of tokens: Capitalization. Languages other than English. Emojis. Programming code with keywords and whitespaces often used for indentation... Numbers and digits. Special tokens."
    - **Plain English:** A probe text tests capitalization, languages, emoji, code, numbers, special tokens.
    - **Technical terms:** probe/example text.

30. **Quote:** "Special tokens. These are unique tokens that have a role other than representing text. They include tokens that indicate the beginning of the text, or the end of the text (which is the way the model signals to the system that it has completed this generation), or other functions."
    - **Plain English:** Special tokens have structural roles (start/end of text, etc.).
    - **Technical terms:** special tokens; end-of-text token.

#### BERT base (uncased) (2018)

31. **Quote:** "Tokenization method: WordPiece... Vocabulary size: 30,522. Special tokens: unk_token [UNK], sep_token [SEP], pad_token [PAD], cls_token [CLS], mask_token [MASK]."
    - **Plain English:** BERT uses WordPiece with a fixed set of special tokens.
    - **Technical terms:** [UNK], [SEP], [PAD], [CLS], [MASK].

32. **Quote:** "[UNK] — An unknown token that the tokenizer has no specific encoding for."
    - **Plain English:** Unknown tokens fall back to [UNK].
    - **Technical terms:** unknown token.

33. **Quote:** "[SEP] — A separator that enables certain tasks that require giving the model two texts (in these cases, the model is called a cross-encoder). One example is reranking, as we'll see in Chapter 8."
    - **Plain English:** [SEP] separates two texts for cross-encoder tasks.
    - **Technical terms:** cross-encoder; reranking.

34. **Quote:** "[PAD] — A padding token used to pad unused positions in the model's input (as the model expects a certain length of input, its context-size)."
    - **Plain English:** [PAD] fills unused input positions.
    - **Technical terms:** padding.

35. **Quote:** "[CLS] — A special classification token for classification tasks, as we'll see in Chapter 4."
    - **Plain English:** [CLS] is for classification.
    - **Technical terms:** classification token.

36. **Quote:** "[MASK] — A masking token used to hide tokens during the training process."
    - **Plain English:** [MASK] hides tokens for masked language modeling.
    - **Technical terms:** masking.

37. **Quote:** "BERT was released in two major flavors: cased (where the capitalization is kept) and uncased (where all capital letters are first turned into small cap letters). With the uncased (and more popular) version of the BERT tokenizer, we notice the following: The newline breaks are gone... All the text is in lowercase... The word 'capitalization' is encoded as two subtokens: capital ##ization. The ## characters are used to indicate this token is a partial token connected to the token that precedes it... The emoji and Chinese characters are gone and replaced with the [UNK] special token."
    - **Plain English:** Uncased BERT lowercases, drops newlines, uses `##` for subwords, and maps emoji/Chinese to [UNK].
    - **Technical terms:** uncased; ## subword marker.

#### BERT base (cased) (2018)

38. **Quote:** "Vocabulary size: 28,996... The cased version of the BERT tokenizer differs mainly in including uppercase tokens... Notice how 'CAPITALIZATION' is now represented as eight tokens: CA ##PI ##TA ##L ##I ##Z ##AT ##ION. Both BERT tokenizers wrap the input within a starting [CLS] token and a closing [SEP] token."
    - **Plain English:** Cased BERT keeps case and expands words into more subword tokens; wraps input with [CLS]...[SEP].
    - **Technical terms:** cased tokenizer.

39. **Quote:** "[CLS] stands for classification as it's a token used at times for sentence classification. [SEP] stands for separator, as it's used to separate sentences in some applications that require passing two sentences to a model (For example, in Chapter 8, we will use a [SEP] token to separate the text of the query and a candidate result.)"
    - **Plain English:** [CLS] for classification; [SEP] for separating sentences/queries.
    - **Technical terms:** [CLS]; [SEP].

#### GPT-2 (2019)

40. **Quote:** "Tokenization method: Byte pair encoding (BPE)... Vocabulary size: 50,257. Special tokens: <|endoftext|>."
    - **Plain English:** GPT-2 uses BPE, 50,257 vocab, one special token.
    - **Technical terms:** BPE; <|endoftext|>.

41. **Quote:** "With the GPT-2 tokenizer, we notice the following: The newline breaks are represented in the tokenizer. Capitalization is preserved, and the word 'CAPITALIZATION' is represented in four tokens. The 🎵鸟 characters are now represented by multiple tokens each... For example, the 🎵 emoji is broken down into the tokens with token IDs 8582, 236, and 113. The tokenizer is successful in reconstructing the original character from these tokens... The two tabs are represented as two tokens (token number 197 in that vocabulary) and the four spaces are represented as three tokens (number 220) with the final space being a part of the token for the closing quote character."
    - **Plain English:** GPT-2 preserves newlines/case; encodes emoji as multi-token sequences; whitespace handled specially.
    - **Technical terms:** multi-token emoji encoding.

42. **Quote:** "What is the significance of whitespace characters? These are important for models to understand or generate code. A model that uses a single token to represent four consecutive whitespace characters is more tuned to a Python code dataset. While a model can live with representing it as four different tokens, it does make the modeling more difficult as the model needs to keep track of the indentation level, which often leads to worse performance."
    - **Plain English:** Whitespace tokens matter for code; single-token indentation helps.
    - **Technical terms:** whitespace tokens; indentation modeling.

#### Flan-T5 (2022)

43. **Quote:** "Flan-T5 uses a tokenizer implementation called SentencePiece, introduced in 'SentencePiece: A simple and language independent subword tokenizer...' which supports BPE and the unigram language model... Vocabulary size: 32,100... No newline or whitespace tokens; this would make it challenging for the model to work with code. The emoji and Chinese characters are both replaced by the <unk> token, making the model completely blind to them."
    - **Plain English:** SentencePiece; no whitespace tokens; emoji/Chinese → <unk>.
    - **Technical terms:** SentencePiece; unigram LM.

#### GPT-4 (2023)

44. **Quote:** "Tokenization method: BPE. Vocabulary size: A little over 100,000. Special tokens: <|endoftext|>; Fill in the middle tokens. These three tokens enable the LLM to generate a completion given not only the text before it but also considering the text after it... <|fim_prefix|>, <|fim_middle|>, <|fim_suffix|>."
    - **Plain English:** GPT-4 uses BPE (~100K vocab) with FIM tokens for fill-in-the-middle.
    - **Technical terms:** FIM (fill-in-the-middle) tokens.

45. **Quote:** "The GPT-4 tokenizer represents the four spaces as a single token. In fact, it has a specific token for every sequence of whitespaces up to a list of 83 whitespaces. The Python keyword elif has its own token in GPT-4. Both this and the previous point stem from the model's focus on code in addition to natural language. The GPT-4 tokenizer uses fewer tokens to represent most words. Examples here include 'CAPITALIZATION' (two tokens versus four) and 'tokens' (one token versus three)."
    - **Plain English:** GPT-4's code focus: whitespace-run tokens, `elif` token, more compact words.
    - **Technical terms:** code-focused tokenization.

#### StarCoder2 (2024)

46. **Quote:** "StarCoder2 is a 15-billion parameter model focused on generating code... Tokenization method: Byte pair encoding (BPE). Vocabulary size: 49,152. Special tokens: <|endoftext|>; Fill in the middle tokens: <fim_prefix>, <fim_middle>, <fim_suffix>, <fim_pad>; When representing code, managing the context is important... StarCoder2 uses special tokens for the name of the repository and the filename: <filename>, <reponame>, <gh_stars>."
    - **Plain English:** StarCoder2 adds FIM + repo/filename tokens for code context.
    - **Technical terms:** <filename>, <reponame>, <gh_stars>.

47. **Quote:** "This is an encoder that focuses on code generation: Similar to GPT-4, it encodes the list of whitespaces as a single token. A major difference here to everything we've seen so far is that each digit is assigned its own token (so 600 becomes 6 0 0). The hypothesis here is that this would lead to better representation of numbers and mathematics. In GPT-2, for example, the number 870 is represented as a single token. But 871 is represented as two tokens (8 and 71). You can intuitively see how that might be confusing to the model and how it represents numbers."
    - **Plain English:** Digit-per-token helps number/math representation.
    - **Technical terms:** digit-per-token.

#### Galactica

48. **Quote:** "The Galactica model... is focused on scientific knowledge and is trained on many scientific papers, reference materials, and knowledge bases. It pays extra attention to tokenization that makes it more sensitive to the nuances of the dataset... For example, it includes special tokens for citations, reasoning, mathematics, amino acid sequences, and DNA sequences. Tokenization method: Byte pair encoding (BPE). Vocabulary size: 50,000."
    - **Plain English:** Galactica's science-focused tokenizer with domain special tokens.
    - **Technical terms:** citation/reasoning/math/bio tokens.

49. **Quote:** "References: Citations are wrapped within the two special tokens [START_REF] and [END_REF]... Step-by-step reasoning: <work> is an interesting token that the model uses for chain-of-thought reasoning."
    - **Plain English:** [START_REF]/[END_REF] wrap citations; `<work>` drives chain-of-thought.
    - **Technical terms:** chain-of-thought.

50. **Quote:** "The Galactica tokenizer behaves similar to StarCoder2 in that it has code in mind. It also encodes whitespaces in the same way: assigning a single token to sequences of whitespace of different lengths. It differs in that it also does that for tabs, though. So from all the tokenizers we've seen so far, it's the only one that assigns a single token to the string made up of two tabs ('\t\t')."
    - **Plain English:** Galactica also single-tokens tabs.
    - **Technical terms:** tab tokens.

#### Phi-3 (and Llama 2)

51. **Quote:** "The Phi-3 model we look at in this book reuses the tokenizer of Llama 2 yet adds a number of special tokens. Tokenization method: Byte pair encoding (BPE). Vocabulary size: 32,000. Special tokens: <|endoftext|>; Chat tokens: As chat LLMs rose to popularity in 2023, the conversational nature of LLMs started to be a leading use case. Tokenizers have been adapted to this direction by the addition of tokens that indicate the turns in a conversation and the roles of each speaker. These special tokens include: <|user|>, <|assistant|>, <|system|>."
    - **Plain English:** Phi-3 reuses Llama 2's BPE tokenizer + chat role tokens.
    - **Technical terms:** chat role tokens.

### Tokenizer Properties

52. **Quote:** "The preceding guided tour of trained tokenizers showed a number of ways in which actual tokenizers differ from each other. But what determines their tokenization behavior? There are three major groups of design choices that determine how the tokenizer will break down text: the tokenization method, the initialization parameters, and the domain of the data the tokenizer targets."
    - **Plain English:** Recap: method, parameters, and data domain determine behavior.
    - **Technical terms:** tokenization method; parameters; data domain.

53. **Quote:** "Tokenization methods — As we've seen, there are a number of tokenization methods with byte pair encoding (BPE) being the more popular one. Each of these methods outlines an algorithm for how to choose an appropriate set of tokens to represent a dataset."
    - **Plain English:** Methods are algorithms choosing token sets.
    - **Technical terms:** BPE popularity.

54. **Quote:** "Tokenizer parameters — After choosing a tokenization method, an LLM designer needs to make some decisions about the parameters of the tokenizer. These include: Vocabulary size — How many tokens to keep in the tokenizer's vocabulary? (30K and 50K are often used as vocabulary size values, but more and more we're seeing larger sizes like 100K.) Special tokens... Capitalization — In languages such as English, how do we want to deal with capitalization? Should we convert everything to lowercase?"
    - **Plain English:** Parameters: vocab size, special tokens, capitalization handling.
    - **Technical terms:** vocabulary size; special tokens; cased/uncased.

55. **Quote:** "The domain of the data — Even if we select the same method and parameters, tokenizer behavior will be different based on the dataset it was trained on (before we even start model training)... For code, for example, we've seen that a text-focused tokenizer may tokenize the indentation spaces like this... This may be suboptimal for a code-focused model. Code-focused models are often improved by making different tokenization choices... These tokenization choices make the model's job easier and thus its performance has a higher probability of improving."
    - **Plain English:** Dataset domain shapes the tokenizer; code-focused choices improve code performance.
    - **Technical terms:** domain-specific tokenization.

---

## Token Embeddings

56. **Quote:** "Now that we understand tokenization, we have solved one part of the problem of representing language to a language model. In this sense, language is a sequence of tokens. And if we train a good-enough model on a large-enough set of tokens, it starts to capture the complex patterns that appear in its training dataset: If the training data contains a lot of English text, that pattern reveals itself as a model capable of representing and generating the English language. If the training data contains factual information (Wikipedia, for example), the model would have the ability to generate some factual information."
    - **Plain English:** Language = token sequence; patterns from training data become capabilities.
    - **Technical terms:** training data patterns.

57. **Quote:** "The next piece of the puzzle is finding the best numerical representation for these tokens that the model can use to calculate and properly model the patterns in the text... As we've seen in Chapter 1, that is what embeddings are. They are the numeric representation space utilized to capture the meanings and patterns in language."
    - **Plain English:** Embeddings are the numeric space for modeling text patterns.
    - **Technical terms:** embeddings.

58. **Quote:** "Oops: Achieving a good threshold of language coherence and better-than-average factual generation, however, starts to present a new problem. Some users start to trust the model's fact generation ability (e.g., at the beginning of 2023 some language models were being dubbed 'Google killers'). It didn't take long for advanced users to recognize that generation models alone aren't reliable search engines. This led to the rise of retrieval-augmented generation (RAG), which combines search and LLMs."
    - **Plain English:** Generative models aren't reliable search engines → RAG.
    - **Word meanings:** dubbed = called.
    - **Technical terms:** RAG (retrieval-augmented generation).

### A Language Model Holds Embeddings for the Vocabulary of Its Tokenizer

59. **Quote:** "After a tokenizer is initialized and trained, it is then used in the training process of its associated language model. This is why a pretrained language model is linked with its tokenizer and can't use a different tokenizer without training."
    - **Plain English:** Model and tokenizer are tied together.
    - **Technical terms:** linked tokenizer.

60. **Quote:** "The language model holds an embedding vector for each token in the tokenizer's vocabulary... When we download a pretrained language model, a portion of the model is this embeddings matrix holding all of these vectors."
    - **Plain English:** The embeddings matrix is part of the downloaded model.
    - **Technical terms:** embeddings matrix.

61. **Quote:** "Before the beginning of the training process, these vectors are randomly initialized like the rest of the model's weights, but the training process assigns them the values that enable the useful behavior they're trained to perform."
    - **Plain English:** Embeddings start random and are trained into useful values.
    - **Technical terms:** random initialization; trained embeddings.

### Creating Contextualized Word Embeddings with Language Models

62. **Quote:** "Now that we've covered token embeddings as the input to a language model, let's look at how language models can create better token embeddings. This is one of the primary ways to use language models for text representation. This empowers applications like named-entity recognition or extractive text summarization (which summarizes a long text by highlighting the most important parts of it, instead of generating new text as a summary)."
    - **Plain English:** LMs produce better token embeddings for NER and extractive summarization.
    - **Technical terms:** NER; extractive text summarization.

63. **Quote:** "Instead of representing each token or word with a static vector, language models create contextualized word embeddings that represent a word with a different token based on its context. These vectors can then be used by other systems for a variety of tasks. In addition to the text applications... these contextualized vectors, for example, are what powers AI image generation systems like DALL·E, Midjourney, and Stable Diffusion."
    - **Plain English:** Contextualized embeddings differ by context; they power text apps and image generation.
    - **Technical terms:** contextualized word embeddings.

64. **Quote:** "The model we're using here is called DeBERTa v3, which at the time of writing is one of the best-performing language models for token embeddings while being small and highly efficient. It is described in the paper 'DeBERTaV3: Improving DeBERTa using ELECTRA-style pre-training gradient-disentangled embedding sharing'."
    - **Plain English:** DeBERTa v3: small, efficient, good token embeddings.
    - **Technical terms:** DeBERTa v3.

65. **Quote:** "This code downloads a pretrained tokenizer and model, then uses them to process the string 'Hello world'... output.shape — This prints out: torch.Size([1, 4, 384]). Skipping the first dimension, we can read this as four tokens, each one embedded in a vector of 384 values. The first dimension is the batch dimension used in cases (like training) when we want to send multiple input sentences to the model at the same time."
    - **Plain English:** Output shape [1,4,384]: batch 1, 4 tokens, 384-d embeddings.
    - **Technical terms:** batch dimension.

66. **Quote:** "We can use what we've learned about tokenizers to inspect them... This prints out: [CLS] Hello world [SEP]. This particular tokenizer and model operate by adding the [CLS] and [SEP] tokens to the beginning and end of a string."
    - **Plain English:** The model adds [CLS] and [SEP], making four tokens.
    - **Technical terms:** [CLS]/[SEP] wrapping.

67. **Quote:** "This is the raw output of a language model. The applications of large language models build on top of outputs like this."
    - **Plain English:** The tensor is the raw LM output that applications use.
    - **Technical terms:** raw output.

68. **Quote:** "Technically, the switch from token IDs into raw embeddings is the first step that occurs inside a language model."
    - **Plain English:** The first internal step is token IDs → embeddings.
    - **Technical terms:** embedding lookup.

69. **Quote:** "A visual like this is essential for the next chapter when we start to look at how Transformer-based LLMs work."
    - **Plain English:** The input/output embedding picture sets up Chapter 3.
    - **Technical terms:** Transformer internals.

---

## Text Embeddings (for Sentences and Whole Documents)

70. **Quote:** "While token embeddings are key to how LLMs operate, a number of LLM applications require operating on entire sentences, paragraphs, or even text documents. This has led to special language models that produce text embeddings—a single vector that represents a piece of text longer than just one token."
    - **Plain English:** Text embeddings = one vector per sentence/document.
    - **Technical terms:** text embeddings.

71. **Quote:** "There are multiple ways of producing a text embedding vector. One of the most common ways is to average the values of all the token embeddings produced by the model. Yet high-quality text embedding models tend to be trained specifically for text embedding tasks."
    - **Plain English:** Averaging token embeddings is common; dedicated training is better.
    - **Technical terms:** mean pooling; dedicated embedding models.

72. **Quote:** "We can produce text embeddings with sentence-transformers, a popular package for leveraging pretrained embedding models... we use the all-mpnet-base-v2 model."
    - **Plain English:** sentence-transformers + all-mpnet-base-v2.
    - **Technical terms:** sentence-transformers.

73. **Quote:** "The number of values, or the dimensions, of the embedding vector depends on the underlying embedding model... vector.shape → (768,)."
    - **Plain English:** all-mpnet-base-v2 produces 768-dim vectors.
    - **Technical terms:** embedding dimension.

74. **Quote:** "In Part II of this book, once we start looking at applications, we'll start to see the immense usefulness of these text embeddings vectors in powering everything from categorization to semantic search to RAG."
    - **Plain English:** Text embeddings power categorization, semantic search, RAG.
    - **Technical terms:** categorization; semantic search; RAG.

---

## Word Embeddings Beyond LLMs

75. **Quote:** "Embeddings are useful even outside of text and language generation. Embeddings, or assigning meaningful vector representations to objects, turns out to be useful in many domains, including recommender engines and robotics."
    - **Plain English:** Embeddings work beyond text (recommenders, robotics).
    - **Technical terms:** object embeddings.

76. **Quote:** "Seeing how word2vec is trained will prime you to learn about contrastive training in Chapter 10."
    - **Plain English:** word2vec primes contrastive training concepts.
    - **Technical terms:** contrastive training.

### Using Pretrained Word Embeddings

77. **Quote:** "Let's look at how we can download pretrained word embeddings (like word2vec or GloVe) using the Gensim library... model = api.load('glove-wiki-gigaword-50')... Here, we've downloaded the embeddings of a large number of words trained on Wikipedia."
    - **Plain English:** Gensim downloads pretrained GloVe embeddings.
    - **Technical terms:** Gensim; GloVe.

78. **Quote:** "We can then explore the embedding space by seeing the nearest neighbors of a specific word, 'king' for example... model.most_similar([model['king']], topn=11). This outputs: [('king', 1.0), ('prince', 0.82), ('queen', 0.78), ('ii', 0.77), ('emperor', 0.77), ('son', 0.77), ('uncle', 0.76), ('kingdom', 0.75), ('throne', 0.75), ('brother', 0.75), ('ruler', 0.74)]."
    - **Plain English:** "king"'s nearest neighbors are semantically related words.
    - **Technical terms:** nearest neighbors; similarity scores.

### The Word2vec Algorithm and Contrastive Training

79. **Quote:** "Just like LLMs, word2vec is trained on examples generated from text. Let's say, for example, we have the text 'Thou shalt not make a machine in the likeness of a human mind' from the Dune novels... The algorithm uses a sliding window to generate training examples. We can, for example, have a window size two, meaning that we consider two neighbors on each side of a central word."
    - **Plain English:** A sliding window generates training examples.
    - **Technical terms:** sliding window.

80. **Quote:** "The embeddings are generated from a classification task. This task is used to train a neural network to predict if words commonly appear in the same context or not... We can think of this as a neural network that takes two words and outputs 1 if they tend to appear in the same context, and 0 if they do not."
    - **Plain English:** word2vec is a neighbor-prediction classification task.
    - **Technical terms:** classification task.

81. **Quote:** "In each of the produced training examples, the word in the center is used as one input, and each of its neighbors is a distinct second input in each training example. We expect the final trained model to be able to classify this neighbor relationship and output 1 if the two input words it receives are indeed neighbors."
    - **Plain English:** Central word + each neighbor = positive training example.
    - **Technical terms:** positive examples.

82. **Quote:** "If, however, we have a dataset of only a target value of 1, then a model can cheat and ace it by outputting 1 all the time. To get around this, we need to enrich our training dataset with examples of words that are not typically neighbors. These are called negative examples."
    - **Plain English:** Negative examples prevent trivial all-1 predictions.
    - **Technical terms:** negative examples.

83. **Quote:** "It turns out that we don't have to be too scientific in how we choose the negative examples. A lot of useful models result from the simple ability to detect positive examples from randomly generated examples (inspired by an important idea called noise-contrastive estimation)... So in this case, we get random words and add them to the dataset and indicate that they are not neighbors."
    - **Plain English:** Random negatives work (noise-contrastive estimation).
    - **Technical terms:** noise-contrastive estimation.

84. **Quote:** "With this, we've seen two of the main concepts of word2vec: skip-gram, the method of selecting neighboring words, and negative sampling, adding negative examples by random sampling from the dataset."
    - **Plain English:** word2vec = skip-gram + negative sampling.
    - **Technical terms:** skip-gram; negative sampling.

85. **Quote:** "We can generate millions and even billions of training examples like this from running text. Before proceeding to train a neural network on this dataset, we need to make a couple of tokenization decisions, which, just like we've seen with LLM tokenizers, include how to deal with capitalization and punctuation and how many tokens we want in our vocabulary."
    - **Plain English:** Millions of examples from text; tokenization decisions mirror LLM tokenizers.
    - **Technical terms:** training example generation.

86. **Quote:** "We then create an embedding vector for each token, and randomly initialize them... In practice, this is a matrix of dimensions vocab_size x embedding_dimensions."
    - **Plain English:** Randomly initialized vocab_size × embedding_dims matrix.
    - **Technical terms:** embedding matrix.

87. **Quote:** "A model is then trained on each example to take in two embedding vectors and predict if they're related or not... Based on whether its prediction was correct or not, the typical machine learning training step updates the embeddings so that the next time the model is presented with those two vectors, it has a better chance of being more correct. And by the end of the training process, we have better embeddings for all the tokens in our vocabulary."
    - **Plain English:** Training updates embeddings to improve predictions.
    - **Technical terms:** embedding updates.

88. **Quote:** "This idea of a model that takes two vectors and predicts if they have a certain relation is one of the most powerful ideas in machine learning, and time after time has proven to work very well with language models. This is why we're dedicating Chapter 10 to this concept and how it optimizes language models for specific tasks (like sentence embeddings and retrieval). The same idea is also central to bridging modalities like text and images, which is key to AI image generation models, as we'll see in Chapter 9... In that formulation, a model is presented with an image and a caption, and it should predict whether that caption describes the image or not."
    - **Plain English:** Two-vector relation prediction is powerful; used in Ch 10 (embeddings/retrieval) and Ch 9 (multimodal).
    - **Technical terms:** contrastive training; multimodal.

### Embeddings for Recommendation Systems

89. **Quote:** "As we've mentioned, the concept of embeddings is useful in so many other domains. In industry, it's widely used for recommendation systems, for example."
    - **Plain English:** Embeddings drive recommendation systems.
    - **Technical terms:** recommendation systems.

90. **Quote:** "In this section we'll use the word2vec algorithm to embed songs using human-made music playlists. Imagine if we treated each song as we would a word or token, and we treated each playlist like a sentence. These embeddings can then be used to recommend similar songs that often appear together in playlists."
    - **Plain English:** Songs = words, playlists = sentences; recommend co-occurring songs.
    - **Technical terms:** playlist embeddings.

91. **Quote:** "The dataset we'll use was collected by Shuo Chen from Cornell University. It contains playlists from hundreds of radio stations around the US."
    - **Plain English:** Dataset = playlists from US radio stations (Cornell).
    - **Technical terms:** dataset provenance.

92. **Quote:** "Let's start by giving it Michael Jackson's 'Billie Jean,' the song with ID 3822... That looks reasonable. Madonna, Prince, and other Michael Jackson songs are the nearest neighbors... Let's step away from pop and into rap, and see the neighbors of 2Pac's 'California Love'... Another quite reasonable list!"
    - **Plain English:** Recommendations are sensible (pop → pop/MJ; rap → rap/hip-hop).
    - **Technical terms:** nearest-neighbor recommendations.

### Training a Song Embedding Model

93. **Quote:** "We'll start by loading the dataset containing the song playlists as well as each song's metadata, such as its title and artist... Remove playlists with only one song... Load song metadata..."
    - **Plain English:** Load and clean the playlist + metadata data.
    - **Technical terms:** pandas DataFrame.

94. **Quote:** "Let's train the model... model = Word2Vec(playlists, vector_size=32, window=20, negative=50, min_count=1, workers=4). That takes a minute or two to train and results in embeddings being calculated for each song that we have."
    - **Plain English:** Train word2vec on playlists (32-dim, window 20, 50 negatives).
    - **Technical terms:** Word2Vec parameters.

95. **Quote:** "Now we can use those embeddings to find similar songs exactly as we did earlier with words... That is the list of the songs whose embeddings are most similar to song 2172. In this case, the song is: title Fade To Black, artist Metallica. This results in recommendations that are all in the same heavy metal and hard rock genre."
    - **Plain English:** Metallica's "Fade to Black" → metal/hard-rock recommendations.
    - **Technical terms:** genre-consistent recommendations.

---

## Summary

96. **Quote:** "In this chapter, we have covered LLM tokens, tokenizers, and useful approaches to using token embeddings. This prepares us to start looking closer at language models in the next chapter, and also opens the door to learn about how embeddings are used beyond language models."
    - **Plain English:** Recap of chapter scope.
    - **Technical terms:** tokens; tokenizers; token embeddings.

97. **Quote:** "We explored how tokenizers are the first step in processing input to an LLM, transforming raw textual input into token IDs. Common tokenization schemes include breaking text down into words, subword tokens, characters, or bytes, depending on the specific requirements of a given application."
    - **Plain English:** Recap: tokenizers → token IDs; four schemes.
    - **Technical terms:** word/subword/character/byte schemes.

98. **Quote:** "A tour of real-world pretrained tokenizers (from BERT to GPT-2, GPT-4, and other models) showed us areas where some tokenizers are better (e.g., preserving information like capitalization, newlines, or tokens in other languages) and other areas where tokenizers are just different from each other (e.g., how they break down certain words)."
    - **Plain English:** Recap: tokenizer comparison highlights trade-offs.
    - **Technical terms:** tokenizer trade-offs.

99. **Quote:** "Three of the major tokenizer design decisions are the tokenizer algorithm (e.g., BPE, WordPiece, SentencePiece), tokenization parameters (including vocabulary size, special tokens, capitalization...), and the dataset the tokenizer is trained on."
    - **Plain English:** Recap: three design decisions.
    - **Technical terms:** algorithm; parameters; dataset.

100. **Quote:** "Language models are also creators of high-quality contextualized token embeddings that improve on raw static embeddings. Those contextualized token embeddings are what's used for tasks including named-entity recognition (NER), extractive text summarization, and text classification. In addition to producing token embeddings, language models can produce text embeddings that cover entire sentences or even documents."
    - **Plain English:** Recap: contextualized token embeddings + text embeddings.
    - **Technical terms:** NER; extractive summarization; text classification.

101. **Quote:** "Before LLMs, word embedding methods like word2vec, GloVe, and fastText were popular. In language processing, this has largely been replaced with contextualized word embeddings produced by language models. The word2vec algorithm relies on two main ideas: skip-gram and negative sampling. It also uses contrastive training similar to the type we'll see in Chapter 10."
    - **Plain English:** Recap: word2vec/GloVe/fastText replaced by contextualized embeddings; word2vec = skip-gram + negative sampling.
    - **Technical terms:** GloVe; fastText.

102. **Quote:** "Embeddings are useful for creating and improving recommender systems as we discussed in the music recommender we built from curated song playlists."
    - **Plain English:** Recap: playlist-based music recommender.
    - **Technical terms:** recommender systems.

103. **Quote:** "In the next chapter, we will take a deep dive into the process after tokenization: how does an LLM process these tokens and generate text? We will look at some of the main intuitions of how LLMs that use the Transformer architecture work."
    - **Plain English:** Preview: Chapter 3 dives into Transformer processing.
    - **Technical terms:** Transformer architecture.
