# 📘 Practice Exam — Chapter 5: Text Clustering and Topic Modeling
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 5
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which embedding model is used for clustering in this chapter?
   a) bert-base-uncased
   b) sentence-transformers/all-mpnet-base-v2
   c) thenlper/gte-small
   d) google/flan-t5-small

2. How many abstracts does the ArXiv dataset (`maartengr/arxiv_nlp`) contain?
   a) 44,949
   b) 5,331
   c) 8,530
   d) 14,520

3. What is the shape of the generated document embeddings?
   a) (384, 44949)
   b) (44949, 384)
   c) (8530, 768)
   d) (44949, 5)

4. To how many dimensions does UMAP reduce the embeddings for clustering?
   a) 2
   b) 384
   c) 10
   d) 5

5. Which metric does UMAP use in this chapter?
   a) cosine
   b) euclidean
   c) manhattan
   d) jaccard

6. The `min_dist=0.0` UMAP parameter generally results in:
   a) wider clusters
   b) more dimensions
   c) tighter clusters
   d) faster training

7. Which dimensionality reduction method handles nonlinear relationships better?
   a) PCA
   b) UMAP
   c) LDA
   d) MMR

8. HDBSCAN is a hierarchical variation of which algorithm?
   a) k-means
   b) LDA
   c) UMAP
   d) DBSCAN

9. What label does HDBSCAN assign to outlier points?
   a) −1
   b) 0
   c) 1
   d) 156

10. What was the HDBSCAN `min_cluster_size` parameter set to?
    a) 5
    b) 50
    c) 100
    d) 156

11. How many clusters did HDBSCAN generate in the example?
    a) 100
    b) 50
    c) 156
    d) 149

12. To create more clusters, we need to:
    a) increase min_cluster_size
    b) increase n_components
    c) lower the diversity parameter
    d) reduce min_cluster_size

13. Manual inspection revealed that cluster 0 contains documents about:
    a) sign language translation
    b) sentiment analysis
    c) document summarization
    d) backdoor attacks

14. Which is a centroid-based clustering algorithm?
    a) HDBSCAN
    b) DBSCAN
    c) k-means
    d) UMAP

15. Which classical approach assumes each topic is a probability distribution of words in the vocabulary?
    a) c-TF-IDF
    b) LDA
    c) MMR
    d) KeyBERT

16. What does the "Name" column of `get_topic_info()` show?
    a) the GPT-3.5-generated label
    b) the number of documents in the topic
    c) the topic's similarity score
    d) the four best keywords concatenated with "_"

17. Topic −1 contains:
    a) documents that could not be fitted within a topic (outliers)
    b) the largest real topic in the dataset
    c) the sign-language documents
    d) documents about prompt optimization

18. The IDF value is calculated by taking the logarithm of:
    a) total frequency divided by average frequency
    b) the number of documents
    c) average frequency of all words across all clusters divided by total frequency of each word
    d) the product of the frequencies

19. In c-TF-IDF, c-TF stands for:
    a) cross term frequency
    b) class-based term frequency
    c) cluster term formatting
    d) cosine term frequency

20. KeyBERTInspired uses which values to extract the most representative documents per topic?
    a) raw embeddings
    b) keyword lists
    c) n_components
    d) c-TF-IDF values

21. KeyBERT extracts keywords by comparing which embeddings through cosine similarity?
    a) word and document embeddings
    b) topic and sentence embeddings
    c) sentence and paragraph embeddings
    d) audio and text embeddings

22. MMR with `diversity=0.2` reduces the keyword set from ~30 to approximately:
    a) 5
    b) 10
    c) 15
    d) 20

23. Which representation block tends to remove nearly all stop words?
    a) MMR
    b) TextGeneration
    c) KeyBERTInspired
    d) OpenAI GPT-3.5

24. The [DOCUMENTS] tag in the text-generation prompt typically contains how many documents?
    a) 10
    b) 30
    c) 2
    d) 4

25. The text-generation prompt uses which two tags?
    a) [DOCUMENTS] and [KEYWORDS]
    b) [TOPIC] and [LABEL]
    c) [INPUT] and [OUTPUT]
    d) [TEXT] and [WORDS]

26. Which generative model produced the too-broad label "Science/Tech" for topic 1?
    a) GPT-3.5
    b) Flan-T5
    c) BERT
    d) HDBSCAN

27. The OpenAI representation block in BERTopic used which model?
    a) gpt-4
    b) flan-t5-small
    c) gpt-3.5-turbo
    d) gte-small

28. What format did the GPT-3.5 prompt instruct the model to output for topic labels?
    a) "label: <short label>"
    b) "<topic>: keywords"
    c) "keywords: <list>"
    d) "topic: <short topic label>"

29. Which BERTopic function reassigns outliers to topics?
    a) reduce_outliers()
    b) get_topic()
    c) find_topics()
    d) update_topics()

30. Which function searches for specific topics based on a search term?
    a) get_topic()
    b) find_topics()
    c) get_topic_info()
    d) visualize_barchart()

31. To which topic was BERTopic's own abstract assigned?
    a) 0
    b) −1
    c) 22
    d) 30

32. Which visualization shows the potential hierarchical structure of topics?
    a) visualize_barchart
    b) visualize_heatmap
    c) visualize_documents
    d) visualize_hierarchy

33. Representation blocks need to be applied once per:
    a) topic
    b) document
    c) cluster point
    d) keyword

34. The idea of reranking an initial set of results is a main staple in:
    a) text classification
    b) text clustering
    c) neural search
    d) machine translation

35. Which function updates topic representations without redoing dimensionality reduction and clustering?
    a) fit()
    b) update_topics()
    c) reduce_outliers()
    d) visualize_documents()

36. `visualize_heatmap` was called with n_clusters equal to:
    a) 156
    b) 5
    c) 20
    d) 30

37. What was the GPT-3.5 label for topic 4 (summarization)?
    a) "Document Summarization Techniques"
    b) "Summarization"
    c) "Science/Tech"
    d) "Attention-based neural machine translation"

38. What was the Flan-T5 label for topic 3 (translation)?
    a) "Neural Machine Translation Enhancements"
    b) "Science/Tech"
    c) "Attention-based neural machine translation"
    d) "Speech-to-description"

39. Which of the following is NOT one of the three steps of the common clustering pipeline?
    a) Convert documents to embeddings
    b) Generate topic labels with GPT-3.5
    c) Reduce the dimensionality of embeddings
    d) Find groups of semantically similar documents

40. In the interactive `visualize_documents` plot, hovering over a document shows:
    a) the abstract
    b) the keywords
    c) the cluster id
    d) the title

---

## Section B: True/False (1 point each)

41. Text clustering is a supervised technique that requires labeled data. (T/F)
42. The ArXiv dataset contains abstracts from ArXiv's cs.CL (Computation and Language) section. (T/F)
43. The three-step clustering pipeline is: embed documents, reduce dimensionality, then cluster. (T/F)
44. Dimensionality reduction never loses information. (T/F)
45. UMAP handles nonlinear relationships better than PCA. (T/F)
46. k-means freely calculates the number of clusters without needing it as input. (T/F)
47. HDBSCAN forces all data points to belong to a cluster. (T/F)
48. To create more clusters, lower the `min_cluster_size` value. (T/F)
49. BERTopic represents topics using a bag-of-words approach enhanced with c-TF-IDF. (T/F)
50. In c-TF-IDF, word frequency is calculated per document rather than per cluster. (T/F)
51. The Name column of `get_topic_info()` shows the two best keywords of a topic. (T/F)
52. KeyBERTInspired removes nearly all stop words because it focuses on semantic relationships. (T/F)
53. MMR increases redundancy in topic representations. (T/F)
54. The text-generation block runs a generative model once for every document. (T/F)
55. BERTopic is confined to using only OpenAI's offering and has no local backends. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Describe the ArXiv dataset: source, composition, time range, and section.
57. Walk through the three-step clustering pipeline (models, parameters, and shapes).
58. Why is k-means not chosen, and what does HDBSCAN do differently?
59. Explain how c-TF-IDF weighting works (c-TF and IDF components).
60. What is topic −1, how many documents did it contain in the example, and what are the two ways to handle outliers?
61. How does `find_topics()` work, and what was the result for "topic modeling"?
62. Explain how KeyBERTInspired reranks keywords and one downside of embedding-based reranking.
63. How does maximal marginal relevance (MMR) diversify keywords, and what diversity parameter was used?
64. Explain the two prompt components of the text-generation Lego block ([DOCUMENTS] and [KEYWORDS]).
65. Compare the Flan-T5 and GPT-3.5 label-generation results, and explain why generating multiple representations is advised.

---

## Section D: Essay / Applied (5 points each)

66. **The clustering pipeline.** Describe how the chapter clusters 44,949 ArXiv abstracts end to end: the embedding model and embeddings shape, the UMAP parameters and the rationale for each, why UMAP is chosen over PCA, the HDBSCAN parameters and result, how clusters are manually inspected, and the 2D visualization procedure. Include the information-loss caveat and why human evaluation is key.
67. **BERTopic and c-TF-IDF.** Explain BERTopic's two-step architecture: the clustering step and the bag-of-words/c-TF-IDF topic-representation step. Include what c-TF is, how IDF is calculated, what the higher weight means, the CountVectorizer role, the "Name" column, topic −1, and the modular "Lego block" design and the algorithmic variants it enables.
68. **Reranking representation blocks.** Explain why BERTopic adds representation blocks on top of c-TF-IDF (semantic-structure limitation; reranking as a staple of neural search), the efficiency benefit (once per topic vs per document), how KeyBERTInspired works (c-TF-IDF representative docs → average document embedding → keyword reranking) including its downside (removing abbreviations like "nmt"), and how MMR works (diversity parameter, ~30 → ~10 keywords, redundancy removal).
69. **Generative topic labels.** Explain the text-generation Lego block: why a label is generated per topic instead of per document, the [DOCUMENTS] (typically 4, highest c-TF-IDF cosine similarity) and [KEYWORDS] tags, the Flan-T5 setup (pipeline, doc_length, tokenizer) and its results (including the too-broad "Science/Tech"), the GPT-3.5 setup (client, model, exponential_backoff, chat, prompt format "topic: <short topic label>") and its more informative results, and why multiple representations are advised (KeyBERTInspired + MMR + GPT-3.5 side by side).
70. **Chapter meta-theme.** Discuss how the chapter combines encoder-only (embeddings), decoder-only (generative), and classical methods (bag-of-words) into one pipeline. Include the embedding model (gte-small), the generative labelers (Flan-T5, GPT-3.5), the classical representation (c-TF-IDF), how the same LLMs from Chapter 4 are reused differently, and how Chapter 6 (prompt engineering) builds on this.

---

## ANSWER KEY

### Section A: Multiple Choice
1. c
2. a
3. b
4. d
5. a
6. c
7. b
8. d
9. a
10. b
11. c
12. d
13. a
14. c
15. b
16. d
17. a
18. c
19. b
20. d
21. a
22. b
23. c
24. d
25. a
26. b
27. c
28. d
29. a
30. b
31. c
32. d
33. a
34. c
35. b
36. d
37. a
38. c
39. b
40. d

### Section B: True/False
41. **F** — Text clustering is an unsupervised technique; it does not require labeled data.
42. **T** — The dataset contains abstracts from ArXiv's cs.CL (Computation and Language) section.
43. **T** — The pipeline is: embed documents → reduce dimensionality → cluster.
44. **F** — Dimensionality reduction is a compression technique; information is always lost.
45. **T** — UMAP tends to handle nonlinear relationships and structures better than PCA.
46. **F** — k-means requires a set number of clusters to be specified beforehand.
47. **F** — HDBSCAN does not force all points into clusters; it detects and ignores outliers (label −1).
48. **T** — Lowering min_cluster_size creates more clusters.
49. **T** — BERTopic represents topics with a bag-of-words approach enhanced with c-TF-IDF.
50. **F** — In c-TF-IDF, word frequency is calculated per cluster (c-TF), not per document.
51. **F** — The Name column shows the four best keywords concatenated with "_".
52. **T** — KeyBERTInspired tends to remove nearly all stop words since it focuses on semantic relationships.
53. **F** — MMR reduces redundancy/diversifies; it does not increase redundancy.
54. **F** — The text-generation block runs once per topic, not once per document.
55. **F** — BERTopic has local backends as well; it is not confined to OpenAI.

### Section C: Short Answer (model answers)
56. **ArXiv dataset.** `maartengr/arxiv_nlp`, loaded via `load_dataset("maartengr/arxiv_nlp")["train"]`; 44,949 abstracts from 1991 to 2024; ArXiv's cs.CL (Computation and Language) section; abstracts and titles extracted into separate variables.
57. **Three-step pipeline.** (1) `SentenceTransformer("thenlper/gte-small")` encodes abstracts → embeddings shape (44949, 384); (2) `UMAP(n_components=5, min_dist=0.0, metric='cosine', random_state=42)` reduces to 5 dims; (3) `HDBSCAN(min_cluster_size=50, metric="euclidean", cluster_selection_method="eom")` → 156 clusters.
58. **k-means vs HDBSCAN.** k-means requires a set number of clusters (unknown beforehand) and forces all points into clusters. HDBSCAN freely calculates the number of clusters, doesn't force all points (outliers get label −1 and are ignored), and handles ArXiv's niche papers via outlier detection.
59. **c-TF-IDF.** c-TF = word frequency per cluster (not per document). IDF = log(average frequency of all words across all clusters ÷ total frequency of each word). c-TF-IDF = weight (IDF) × frequency (c-TF). CountVectorizer generates the bag-of-words; each cluster is a topic with a vocabulary ranking.
60. **Topic −1.** Contains documents not fitted within a topic (outliers); 14,520 documents in the example. Handle outliers with a non-outlier algorithm like k-means or BERTopic's `reduce_outliers()` to reassign them.
61. **find_topics().** Searches for topics based on a search term, returning topic IDs and similarity scores. `find_topics("topic modeling")` returned topics [22, -1, 1, 47, 32] with topic 22 at 0.95 similarity; `get_topic(22)` keywords (topic, topics, lda, latent, document, documents, modeling, dirichlet, word, allocation) confirmed it, and BERTopic's own abstract was assigned to topic 22.
62. **KeyBERTInspired.** Uses c-TF-IDF to extract the most representative documents per topic, computes the average document embedding per topic, compares it to candidate keyword embeddings, and reranks keywords. Downside: embedding-based techniques may remove informative abbreviations the model can't represent (e.g., "nmt" for neural machine translation), which domain experts find highly informative.
63. **MMR.** Embeds candidate keywords and iteratively calculates the next best keyword to add — diverse from already-chosen keywords but related to the documents. The diversity parameter (diversity=0.2) controls diversity; reduces ~30 initial keywords to ~10 more diverse ones (e.g., topic 4 keeps only one "summary"-like word).
64. **Text-generation prompt.** [DOCUMENTS] — a small subset of documents, typically four, with the highest cosine similarity of their c-TF-IDF values to the topic; [KEYWORDS] — the topic's keywords (from c-TF-IDF or other representations). The model generates a short label per topic (once per topic, not per document).
65. **Flan-T5 vs GPT-3.5.** Flan-T5 produced labels like "Speech-to-description", "Review", "Summarization" but also too-broad ones like "Science/Tech". GPT-3.5 (larger, more linguistic capability) produced more informative labels like "Document Summarization Techniques" and "Neural Machine Translation Enhancements". Multiple representations advised because no model is perfect — use KeyBERTInspired, MMR, and GPT-3.5 side by side for different perspectives.

### Section D: Essay (grading notes)
66. **Expect** embedding model gte-small (more recent, better on clustering, faster than all-mpnet-base-v2), shape (44949, 384); UMAP params n_components=5 (5–10 works well), min_dist=0.0 (tighter clusters), metric='cosine' (Euclidean has issues with high-dim), random_state=42 (reproducible, disables parallelism); UMAP over PCA (nonlinear); HDBSCAN min_cluster_size=50, euclidean, eom → 156 clusters; manual inspection of cluster 0 (sign language); 2D UMAP visualization (outliers grey alpha=0.05, clusters cmap="tab20b" alpha=0.6, axis off); caveat: visualization reduction loses information (approximation, clusters pushed together/apart), so human evaluation is key.
67. **Expect** BERTopic two steps: (1) clustering (embed, reduce, cluster) and (2) topic representation via bag-of-words + c-TF-IDF; c-TF = per-cluster frequency; IDF = log(average frequency across all clusters ÷ total frequency of each word); c-TF-IDF = weight × frequency; higher weight = more representative; CountVectorizer builds the bag-of-words; "Name" column = four best keywords joined by "_"; topic −1 = outliers (14,520 docs); modular "Lego block" design (any component replaceable, e.g., swap embedding model, use k-means); variants: guided, (semi-)supervised, hierarchical, dynamic, multimodal, multi-aspect, online/incremental, zero-shot.
68. **Expect** bag-of-words ignores semantic structures; solution = rerank initial c-TF-IDF with more powerful slower techniques (embedding models, generative models); reranking is a staple of neural search (Ch 8); efficiency: once per topic not per document (millions of docs, hundreds of topics); KeyBERTInspired procedure (c-TF-IDF representative docs → average doc embedding → compare to candidate keyword embeddings → rerank); downside: removes informative abbreviations like "nmt"; MMR: embed candidates, iteratively add next best keyword, diversity parameter 0.2, ~30 → ~10 keywords, removes redundancy (e.g., "summary"/"summaries"); KeyBERTInspired removes nearly all stop words.
69. **Expect** generate a label per topic (not per document, potentially millions); [DOCUMENTS] typically 4 docs with highest cosine similarity of c-TF-IDF to the topic; [KEYWORDS] from c-TF-IDF or other representations; Flan-T5: `pipeline("text2text-generation", model="google/flan-t5-small")`, `TextGeneration(generator, prompt=..., doc_length=50, tokenizer="whitespace")`; labels incl. "Speech-to-description", "Summarization", too-broad "Science/Tech"; GPT-3.5: `openai.OpenAI(api_key=...)`, `OpenAI(client, model="gpt-3.5-turbo", exponential_backoff=True, chat=True, prompt=...)` with "topic: <short topic label>" format; more informative labels ("Document Summarization Techniques"); BERTopic supports local backends; multiple representations advised (KeyBERTInspired + MMR + GPT-3.5 side by side).
70. **Expect** encoder-only (gte-small embeddings for clustering), decoder-only (Flan-T5 and GPT-3.5 generative labelers), classical (bag-of-words / c-TF-IDF representation); the three-step pipeline (embed, reduce, cluster) is common; same LLMs as Ch 4 (Flan-T5, GPT-3.5) reused differently (per-topic labels instead of classification); reranking combines classical speed with neural quality; BERTopic's modularity as the enabler; Chapter 6 = prompt engineering, improving generative-model output.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely the c-TF-IDF formula, UMAP/HDBSCAN parameters, or the representation blocks).
