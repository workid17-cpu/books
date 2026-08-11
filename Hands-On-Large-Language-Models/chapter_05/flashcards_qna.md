# 📘 Chapter 5 Flashcards: Text Clustering and Topic Modeling
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 5

---

## Part 1: Terms → Definitions

**Q:** What is text clustering?
**A:** Grouping similar texts based on their semantic content, meaning, and relationships; an unsupervised technique.

**Q:** What is an unsupervised technique?
**A:** A method that does not require labeled data.

**Q:** What is topic modeling?
**A:** Discovering (abstract) topics that appear in large collections of textual data — giving meaning to clusters of documents.

**Q:** What is an embedding?
**A:** A numerical representation of text that attempts to capture its meaning; the features to be clustered.

**Q:** Which embedding model is used in this chapter and why?
**A:** `thenlper/gte-small` — more recent than all-mpnet-base-v2, outperforms it on clustering, and is faster for inference.

**Q:** What is dimensionality reduction?
**A:** Reducing the size of the dimensional space to represent the same data with fewer dimensions; aims to preserve global structure. A compression technique — information is always lost.

**Q:** What is PCA?
**A:** Principal Component Analysis — a classic dimensionality reduction method (Hotelling, 1933).

**Q:** What is UMAP?
**A:** Uniform Manifold Approximation and Projection — a dimensionality reduction method that handles nonlinear relationships better than PCA (McInnes et al., 2018).

**Q:** What is a cluster model?
**A:** A model that finds groups of semantically similar documents from reduced embeddings.

**Q:** What is centroid-based clustering (e.g., k-means)?
**A:** A clustering type that requires a set number of clusters and forces all data points into clusters.

**Q:** What is density-based clustering?
**A:** A clustering type that freely calculates the number of clusters, does not force all points into clusters, and can detect outliers.

**Q:** What is DBSCAN?
**A:** A density-based clustering algorithm.

**Q:** What is HDBSCAN?
**A:** Hierarchical Density-Based Spatial Clustering of Applications with Noise — a hierarchical variation of DBSCAN that finds dense (micro-)clusters without specifying the number of clusters; detects and ignores outliers (label −1).

**Q:** What is an outlier?
**A:** A data point that does not belong to any cluster; HDBSCAN does not force it into a cluster.

**Q:** What does the `min_cluster_size` parameter do in HDBSCAN?
**A:** Sets the minimum size a cluster can take; lowering it creates more clusters.

**Q:** What is a bag-of-words?
**A:** Counting the number of times each word appears in a document — a classical representation.

**Q:** What is c-TF?
**A:** Class-based term frequency — frequency of words calculated within an entire cluster instead of per document.

**Q:** What is TF-IDF?
**A:** Term frequency–inverse document frequency — a weighting scheme.

**Q:** What is c-TF-IDF?
**A:** BERTopic's class-based TF-IDF weighting — more weight on words meaningful to a cluster, less on words used across all clusters. IDF = log(average frequency of all words across all clusters ÷ total frequency of each word).

**Q:** What is BERTopic?
**A:** A topic modeling technique leveraging clusters of semantically similar texts to extract various topic representations; a highly modular text clustering and topic modeling framework (Grootendorst, 2022).

**Q:** What is LDA?
**A:** Latent Dirichlet allocation — a classic topic modeling approach assuming each topic is a probability distribution of words in the corpus vocabulary (Blei, Ng, Jordan, 2003).

**Q:** What is a representation model in BERTopic?
**A:** A reranker block that takes the initial topic representation (c-TF-IDF) and produces an improved representation.

**Q:** What is KeyBERT?
**A:** A keyword extraction package (Grootendorst, 2020) that compares word and document embeddings through cosine similarity.

**Q:** What is KeyBERTInspired?
**A:** A BERTopic representation model that uses c-TF-IDF to find representative documents per topic, computes the average document embedding per topic, and compares it to candidate keyword embeddings to rerank keywords.

**Q:** What is maximal marginal relevance (MMR)?
**A:** An algorithm that diversifies topic representations — finds keywords diverse from one another but still related to the documents; uses a diversity parameter (e.g., from ~30 initial keywords to ~10).

**Q:** What is the text generation block in BERTopic?
**A:** Uses generative LLMs + prompt engineering to create labels for topics from keywords and a small set of representative documents.

**Q:** What does `reduce_outliers()` do?
**A:** Reassigns outliers to topics.

**Q:** What does `find_topics()` do?
**A:** Searches for specific topics based on a search term; returns topic IDs with similarity scores.

**Q:** What does `get_topic()` do?
**A:** Inspects an individual topic's keywords with weights.

**Q:** What does `get_topic_info()` do?
**A:** Gives a quick description of the topics found (Topic, Count, Name/Representation).

**Q:** What does `update_topics()` do?
**A:** Updates topic representations using a representation model without redoing dimensionality reduction and clustering.

**Q:** What is prompt engineering?
**A:** Using prompts to guide generative models (covered in depth in Chapter 6).

**Q:** What is the three-step clustering pipeline?
**A:** (1) Embed documents with an embedding model; (2) reduce dimensionality of embeddings; (3) cluster the reduced embeddings with a cluster model.

**Q:** Why is dimensionality reduction needed for clustering?
**A:** As dimensions increase there is exponential growth in possible values per dimension, making it harder to find meaningful clusters; high-dimensional data is troublesome for many clustering techniques.

**Q:** What are the three relationship visualizations in BERTopic?
**A:** `visualize_barchart()` (ranked keywords), `visualize_heatmap(n_clusters=30)` (relationships between topics), and `visualize_hierarchy()` (potential hierarchical structure).

---

## Part 2: Short Answer

**Q:** Describe the ArXiv dataset used in this chapter.
**A:** `maartengr/arxiv_nlp` — 44,949 abstracts from 1991 to 2024 from ArXiv's cs.CL (Computation and Language) section, loaded via `load_dataset("maartengr/arxiv_nlp")["train"]`.

**Q:** Why is choosing an embedding model optimized for semantic similarity important for clustering?
**A:** Clustering attempts to find groups of semantically similar documents, so embeddings optimized for semantic similarity produce better clusters.

**Q:** What is the shape of the embeddings produced by gte-small, and what does it mean?
**A:** (44949, 384) — 44,949 documents, each represented by 384 values; these embeddings are the features to be clustered.

**Q:** Why is UMAP preferred over PCA in this chapter?
**A:** UMAP tends to handle nonlinear relationships and structures better than PCA.

**Q:** What is the downside of dimensionality reduction?
**A:** It is a compression technique — information is always lost; there is a balance between reducing dimensionality and keeping information.

**Q:** What are the UMAP parameters used, and why?
**A:** `n_components=5` (5–10 works well to capture high-dimensional global structures), `min_dist=0.0` (tighter clusters), `metric='cosine'` (Euclidean-based methods have issues with high-dimensional data), `random_state=42` (reproducible but disables parallelism → slower training).

**Q:** Why not use k-means for clustering?
**A:** k-means requires a set number of clusters, which we don't know beforehand; HDBSCAN (density-based) freely calculates the number and doesn't force all points into clusters.

**Q:** What are the HDBSCAN parameters and result?
**A:** `min_cluster_size=50`, `metric="euclidean"`, `cluster_selection_method="eom"` → 156 clusters. Lowering min_cluster_size creates more clusters.

**Q:** Why use a density-based model like HDBSCAN for ArXiv articles?
**A:** ArXiv may contain niche papers, so outlier detection is helpful; outliers (label −1) are ignored, not forced into clusters.

**Q:** What did manual inspection of cluster 0 reveal?
**A:** The cluster contained documents about translation from and to sign language (statistical machine translation to ASL, signed languages research, sign language translation software).

**Q:** How are clusters visualized, and what is the caveat?
**A:** Reduce embeddings to 2D with UMAP, build a DataFrame (x, y, title, cluster), plot outliers (grey) and clusters (colored, cmap="tab20b"). Caveat: visualization dimensionality reduction loses information — it's an approximation; human inspection is key.

**Q:** How does LDA represent topics, and how does it differ from Transformer-based clustering?
**A:** LDA assumes each topic is a probability distribution of words over the vocabulary and uses bag-of-words (no context/meaning); Transformer-based embeddings use attention for contextual meaning and semantic similarity.

**Q:** What are the two steps of BERTopic?
**A:** (1) The same clustering procedure as the text clustering example (embed, reduce, cluster); (2) modeling a distribution over words using bag-of-words enhanced with c-TF-IDF.

**Q:** How is c-TF-IDF calculated?
**A:** Word frequency per cluster (c-TF) × IDF weight, where IDF = log(average frequency of all words across all clusters ÷ total frequency of each word). scikit-learn's CountVectorizer generates the bag-of-words.

**Q:** Why is the modularity of BERTopic a major advantage?
**A:** The two steps (clustering and topic representation) are largely independent; every component is replaceable like "Lego blocks" — swap the embedding model, use k-means instead of HDBSCAN, integrate newly released models.

**Q:** What algorithmic variants does BERTopic support?
**A:** Guided, (semi-)supervised, hierarchical, dynamic, multimodal, multi-aspect, online/incremental, zero-shot topic modeling, etc.

**Q:** How is BERTopic trained in the chapter?
**A:** `BERTopic(embedding_model=embedding_model, umap_model=umap_model, hdbscan_model=hdbscan_model, verbose=True).fit(abstracts, embeddings)` — reusing previously defined models and embeddings.

**Q:** What is topic −1 in `get_topic_info()`?
**A:** The outliers — documents that could not be fitted within a topic (14,520 docs in the example). Remove via k-means (non-outlier algorithm) or `reduce_outliers()`.

**Q:** What is the "Name" column in `get_topic_info()`?
**A:** The four best keywords of a topic concatenated with "_" (e.g., `0_speech_asr_recognition_end`).

**Q:** What is topic 0 about, based on its keywords?
**A:** Automatic speech recognition (ASR) — keywords: speech, asr, recognition, end, acoustic, speaker, audio, the, error, automatic.

**Q:** What did `find_topics("topic modeling")` return?
**A:** Topic IDs [22, -1, 1, 47, 32] with similarities [0.95456535, 0.91173744, ...]; topic 22 had 0.95 similarity — and BERTopic's own abstract was assigned to topic 22.

**Q:** What are topic 22's keywords?
**A:** topic, topics, lda, latent, document, documents, modeling, dirichlet, word, allocation.

**Q:** What does the interactive `visualize_documents` plot show?
**A:** Topics and documents with hover titles (not abstracts); zoom in to view documents or double-click a topic to view only it. Uses `reduced_embeddings`, `width=1200`, `hide_annotations=True`.

**Q:** Why use a reranker/representation block on top of c-TF-IDF?
**A:** Bag-of-words doesn't take semantic structures into account; use its speed for an initial representation, then tweak with more powerful but slower techniques (embedding models, generative models). Reranking initial results is a staple of neural search (Ch 8).

**Q:** What is the efficiency benefit of representation blocks?
**A:** Optimization only needs to run once per topic, not per document — e.g., millions of documents but a hundred topics → run the block once per topic. Blocks can also be stacked.

**Q:** What is the `topic_differences` helper used for?
**A:** Comparing the top 5 words per topic between original and updated representations in a DataFrame.

**Q:** How does KeyBERTInspired work?
**A:** Uses c-TF-IDF to extract the most representative documents per topic, computes the average document embedding per topic, compares it to candidate keyword embeddings, and reranks the keywords.

**Q:** What is the downside of KeyBERTInspired?
**A:** Embedding-based techniques can remove informative abbreviations (e.g., "nmt" = neural machine translation) the model can't represent well; domain experts find such abbreviations highly informative.

**Q:** How does MMR work, and what is the diversity parameter?
**A:** Embeds candidate keywords and iteratively calculates the next best keyword to add — diverse from already-chosen keywords but related to documents. The diversity parameter (e.g., 0.2) controls how diverse keywords must be; MMR goes from ~30 initial keywords to ~10.

**Q:** What are the two prompt components for text generation?
**A:** [DOCUMENTS] tag — typically four documents best representing the topic (highest cosine similarity of c-TF-IDF values to the topic); and [KEYWORDS] tag — the topic's keywords (from c-TF-IDF or other representations).

**Q:** What labels did Flan-T5 produce for topics 0–4?
**A:** "Speech-to-description", "Science/Tech" (too broad), "Review", "Attention-based neural machine translation", "Summarization".

**Q:** What labels did GPT-3.5 produce for topics 0–4?
**A:** "Leveraging External Data for Improving Low-Resource...", "Improved Representation Learning for Biomedica...", "Advancements in Aspect-Based Sentiment Analys...", "Neural Machine Translation Enhancements", "Document Summarization Techniques".

**Q:** What are the OpenAI representation block parameters?
**A:** `client = openai.OpenAI(api_key=...)`, `OpenAI(client, model="gpt-3.5-turbo", exponential_backoff=True, chat=True, prompt=prompt)`, with the prompt extracting "topic: <short topic label>".

**Q:** Why generate multiple topic representations?
**A:** No model is perfect; generating multiple representations (e.g., KeyBERTInspired, MMR, and GPT-3.5 side by side) provides different perspectives on the same topic.

**Q:** What is the chapter's meta-theme?
**A:** Combining encoder-only (embeddings), decoder-only (generative), and classical methods (bag-of-words) results in amazing new techniques and pipelines.

**Q:** What is the `visualize_document_datamap` function?
**A:** Creates beautiful datamapplot illustrations of the top 20 topics (`topics=list(range(20))`, `reduced_embeddings=...`, `width=1200`, `label_font_size=11`, `label_wrap_width=20`, `use_medoids=True`).

---

## Part 3: Fill-in-the-Blank

**Q:** The dataset `maartengr/arxiv_nlp` contains ______ abstracts from ______ to ______ from ArXiv's cs.CL section.
**A:** 44,949; 1991; 2024.

**Q:** The embedding model used is ______.
**A:** thenlper/gte-small.

**Q:** The embeddings shape is ______.
**A:** (44949, 384).

**Q:** The clustering pipeline has three steps: embed → ______ → ______.
**A:** reduce dimensionality; cluster.

**Q:** UMAP reduces the embeddings from 384 to ______ dimensions.
**A:** 5.

**Q:** The UMAP parameters were n_components=______, min_dist=______, metric=______, random_state=______.
**A:** 5; 0.0; 'cosine'; 42.

**Q:** Values between ______ and ______ dimensions generally work well to capture high-dimensional global structures.
**A:** 5; 10.

**Q:** The HDBSCAN parameters were min_cluster_size=______, metric=______, cluster_selection_method=______.
**A:** 50; "euclidean"; "eom".

**Q:** HDBSCAN generated ______ clusters.
**A:** 156.

**Q:** To create more clusters, we need to ______ the value of min_cluster_size.
**A:** reduce (lower).

**Q:** HDBSCAN is a hierarchical variation of ______.
**A:** DBSCAN.

**Q:** Cluster ______ contained documents about sign language translation.
**A:** 0.

**Q:** In the 2D visualization, outliers were plotted with c=______ and clusters with cmap=______.
**A:** "grey"; "tab20b".

**Q:** The classical topic modeling approach that assumes each topic is a probability distribution of words is ______.
**A:** Latent Dirichlet allocation (LDA).

**Q:** The two steps of BERTopic are ______ and ______.
**A:** clustering; topic representation.

**Q:** BERTopic uses ______ to generate the bag-of-words representation.
**A:** scikit-learn's CountVectorizer.

**Q:** c-TF-IDF = ______ × ______.
**A:** c-TF (word frequency per cluster); IDF weight.

**Q:** IDF = log(______ ÷ total frequency of each word).
**A:** average frequency of all words across all clusters.

**Q:** Topic ______ contains all documents considered outliers.
**A:** -1.

**Q:** In the example, topic −1 contained ______ documents.
**A:** 14,520.

**Q:** Topic 0's keywords speech, asr, recognition indicate it is about ______.
**A:** automatic speech recognition (ASR).

**Q:** `find_topics("topic modeling")` returned topic ______ at similarity ______.
**A:** 22; 0.95.

**Q:** The BERTopic paper's own abstract was assigned to topic ______.
**A:** 22.

**Q:** The Name column shows the ______ best keywords concatenated with a "_".
**A:** four (4).

**Q:** To remove outliers, use a non-outlier algorithm like ______ or BERTopic's ______ function.
**A:** k-means; reduce_outliers().

**Q:** `visualize_heatmap` was called with n_clusters=______.
**A:** 30.

**Q:** Representation blocks run once per ______, not per document.
**A:** topic.

**Q:** KeyBERTInspired is inspired by the keyword extraction package ______.
**A:** KeyBERT.

**Q:** The MMR diversity parameter used was ______; MMR reduced ~______ initial keywords to ~______ diverse ones.
**A:** 0.2; 30; 10.

**Q:** KeyBERTInspired tends to remove nearly all ______ words.
**A:** stop.

**Q:** The [DOCUMENTS] tag typically contains ______ documents best representing the topic.
**A:** four (4).

**Q:** The TextGeneration block used doc_length=______ and tokenizer=______.
**A:** 50; "whitespace".

**Q:** The Flan-T5 label for topic 0 was ______.
**A:** "Speech-to-description".

**Q:** The Flan-T5 label "______" was considered too broad for topic 1.
**A:** Science/Tech.

**Q:** The OpenAI block used model ______ with exponential_backoff=______ and chat=______.
**A:** "gpt-3.5-turbo"; True; True.

**Q:** The GPT-3.5 prompt instructed the model to extract a topic label in the format ______.
**A:** "topic: <short topic label>".

**Q:** The GPT-3.5 label for topic 4 was ______.
**A:** "Document Summarization Techniques".

**Q:** BERTopic is not confined to OpenAI — it has ______ backends as well.
**A:** local.

**Q:** `visualize_document_datamap` visualized the top ______ topics with use_medoids=______.
**A:** 20; True.

**Q:** The reranking idea is a main staple in ______, covered in Chapter 8.
**A:** neural search.

**Q:** The next chapter (6) covers ______.
**A:** prompt engineering.
