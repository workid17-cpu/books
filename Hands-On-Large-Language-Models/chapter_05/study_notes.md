# 📘 Chapter 5 Study Bundle: Text Clustering and Topic Modeling
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 5

---

## §1. Study Notes

### Core Theme
This chapter covers unsupervised learning with LLMs: text clustering and topic modeling. It builds a common three-step clustering pipeline — (1) embed documents, (2) reduce embedding dimensionality, (3) cluster the reduced embeddings — using an ArXiv cs.CL dataset, the gte-small embedding model, UMAP dimensionality reduction, and HDBSCAN clustering. It then transitions to topic modeling with BERTopic, a modular framework that extends clustering with class-based TF-IDF (c-TF-IDF) to represent each cluster as a keyword-ranked topic. The chapter demonstrates BERTopic's modularity via "Lego block" representation models — KeyBERTInspired, Maximal Marginal Relevance (MMR), and text generation with Flan-T5 and GPT-3.5 — showing how encoder-only, decoder-only, and classical (bag-of-words) methods combine.

### Key Definitions
- **Text clustering**: Grouping similar texts based on their semantic content, meaning, and relationships; unsupervised.
- **Unsupervised technique**: A method that does not require labeled data.
- **Topic modeling**: Discovering (abstract) topics that appear in large collections of textual data; giving meaning to clusters of documents.
- **Embedding**: Numerical representation of text that attempts to capture its meaning; the features to be clustered.
- **Embedding model**: Converts input documents to embeddings; choosing models optimized for semantic similarity is important for clustering (e.g., gte-small).
- **Dimensionality reduction**: Reducing the size of the dimensional space to represent the same data with fewer dimensions; aims to preserve global structure of high-dimensional data. It's a compression technique — information is always lost.
- **PCA (Principal Component Analysis)**: A classic dimensionality reduction method (Hotelling, 1933).
- **UMAP (Uniform Manifold Approximation and Projection)**: A dimensionality reduction method that tends to handle nonlinear relationships and structures better than PCA (McInnes et al., 2018).
- **Cluster model**: Finds groups of semantically similar documents from reduced embeddings.
- **Centroid-based clustering (e.g., k-means)**: Requires a set number of clusters; forces all data points into clusters.
- **Density-based clustering**: Freely calculates the number of clusters; does not force all data points to be part of a cluster; can detect outliers.
- **DBSCAN**: A density-based clustering algorithm.
- **HDBSCAN (Hierarchical Density-Based Spatial Clustering of Applications with Noise)**: A hierarchical variation of DBSCAN that finds dense (micro-)clusters without specifying the number of clusters; detects and ignores outliers (data points assigned to no cluster, label −1).
- **Outlier**: A data point that does not belong to any cluster; HDBSCAN does not force it into a cluster.
- **min_cluster_size**: HDBSCAN parameter — the minimum size a cluster can take; lowering it creates more clusters.
- **Topic**: Described by keywords/keyphrases; ideally has a single overarching label.
- **Bag-of-words**: Counting the number of times each word appears in a document (a classical representation).
- **c-TF (class-based term frequency)**: Frequency of words calculated within an entire cluster instead of per document.
- **TF-IDF (term frequency–inverse document frequency)**: A weighting scheme.
- **c-TF-IDF (class-based TF-IDF)**: BERTopic's weighting — puts more weight on words meaningful to a cluster, less weight on words used across all clusters. IDF = log of (average frequency of all words across all clusters / total frequency of each word).
- **BERTopic**: A topic modeling technique that leverages clusters of semantically similar texts to extract various types of topic representations; a highly modular text clustering and topic modeling framework (Grootendorst, 2022).
- **Latent Dirichlet allocation (LDA)**: A classic topic modeling approach assuming each topic is characterized by a probability distribution of words in a corpus's vocabulary (Blei, Ng, Jordan, 2003).
- **Representation model (BERTopic)**: A reranker block that takes the initial topic representation (c-TF-IDF) and produces an improved representation.
- **KeyBERT**: A keyword extraction package (Grootendorst, 2020) that extracts keywords by comparing word and document embeddings through cosine similarity.
- **KeyBERTInspired**: A BERTopic representation model that uses c-TF-IDF to extract the most representative documents per topic, computes the average document embedding per topic, and compares it to embeddings of candidate keywords to rerank keywords.
- **Maximal marginal relevance (MMR)**: An algorithm that diversifies topic representations — finds a set of keywords diverse from one another but still related to the documents; uses a diversity parameter; e.g., from 30 initial keywords to 10 more diverse ones.
- **Text generation block**: Uses generative LLMs + prompt engineering to create labels for topics from keywords and a small set of representative documents.
- **reduce_outliers()**: A BERTopic function to reassign outliers to topics.
- **find_topics()**: Searches for specific topics based on a search term; returns topic IDs with similarity scores.
- **get_topic()**: Inspects an individual topic's keywords with weights.
- **get_topic_info()**: Gives a quick description of the topics found (Topic, Count, Name/Representation).
- **update_topics()**: Updates topic representations using a representation model without redoing dimensionality reduction and clustering.
- **prompt engineering**: Using prompts to guide generative models (Ch 6 focus).

### Core Concepts & Frameworks
- **Unsupervised potential**: Text clustering, unbound by supervision, allows creative solutions and diverse applications — finding outliers, speedup labeling, and finding incorrectly labeled data.
- **The dataset**: ArXiv open-access platform (mostly computer science, math, physics). Dataset `maartengr/arxiv_nlp` — 44,949 abstracts from 1991 to 2024 from ArXiv's cs.CL (Computation and Language) section. Loaded via `load_dataset("maartengr/arxiv_nlp")["train"]`; metadata extracted as `abstracts`, `titles` (and years).
- **The three-step clustering pipeline** (popular common pipeline):
  1. Convert input documents to embeddings with an embedding model.
  2. Reduce the dimensionality of embeddings with a dimensionality reduction model.
  3. Find groups of semantically similar documents with a cluster model.
- **Embedding model selection**: MTEB leaderboard used to select a model with a decent clustering score that's small enough to run quickly. `thenlper/gte-small` chosen — more recent than all-mpnet-base-v2, outperforms it on clustering, and is faster for inference. Embeddings shape `(44949, 384)` — 384 values per document.
- **Why reduce dimensionality**: As dimensions increase, there's exponential growth in possible values per dimension; finding all subspaces becomes increasingly complex. High-dimensional data is troublesome for many clustering techniques.
- **UMAP parameters**: `n_components=5` (values between 5 and 10 generally work well to capture high-dimensional global structures); `min_dist=0.0` (minimum distance between embedded points; 0 gives tighter clusters); `metric='cosine'` (Euclidean-based methods have issues with high-dimensional data); `random_state=42` (reproducible results but disables parallelism → slower training).
- **Why not k-means**: Centroid-based k-means requires a set number of clusters, which we don't know beforehand. HDBSCAN (density-based) freely calculates the number of clusters and doesn't force all points into a cluster.
- **HDBSCAN parameters**: `min_cluster_size=50`, `metric="euclidean"`, `cluster_selection_method="eom"`. Result: 156 clusters. Lowering min_cluster_size creates more clusters.
- **Why HDBSCAN for ArXiv**: ArXiv may contain niche papers; outlier detection is helpful. Outliers (label −1) are ignored, not forced into clusters.
- **Inspecting clusters**: Manually print documents from a cluster (e.g., cluster 0 contained documents about sign language translation). Cluster 0 example documents: statistical machine translation to American Sign Language, signed languages research, sign language translation software.
- **Visualization**: Reduce embeddings to 2D (x/y plane) with UMAP; create a pandas DataFrame with title and cluster; plot outliers (grey) and clusters (colored) with matplotlib (`alpha`, `s`, `cmap="tab20b"`, `axis("off")`). Caveat: any dimensionality reduction for visualization creates information loss — it's an approximation; clusters may be pushed together or apart. Human evaluation/inspection is a key component of cluster analysis.
- **From clustering to topic modeling**: Manually inspecting clusters (e.g., identifying the sign-language cluster) leads to the idea of finding themes/latent topics = topic modeling. Traditionally topics are represented by keywords (e.g., "sign", "language", "translation") rather than a single label.
- **LDA**: Each topic = a probability distribution of words in the corpus vocabulary; each word scored against its relevance to each topic. Uses bag-of-words features, which do not take context or meaning into account — unlike Transformer-based embeddings.
- **BERTopic's two steps**: (1) Same clustering procedure as the text clustering example (embed, reduce, cluster); (2) model a distribution over words using bag-of-words, enhanced with c-TF-IDF.
- **c-TF-IDF details**: Frequency of words calculated per cluster (c-TF), not per document. IDF = logarithm of (average frequency of all words across all clusters / total frequency of each word). Result: weight (IDF) × frequency (c-TF) = c-TF-IDF. scikit-learn's CountVectorizer generates the bag-of-words; each cluster is a topic with a ranking of the corpus vocabulary. The higher a word's weight in a topic, the more representative it is.
- **BERTopic modularity**: The two steps (clustering and topic representation) are largely independent — c-TF-IDF doesn't depend on the clustering models. Every component is replaceable ("Lego blocks"): swap the embedding model, use k-means instead of HDBSCAN, etc. New models can be integrated as the field grows.
- **Algorithmic variants supported**: guided, (semi-)supervised, hierarchical, dynamic, multimodal, multi-aspect, online/incremental, zero-shot topic modeling, etc.
- **Running BERTopic**: `BERTopic(embedding_model=embedding_model, umap_model=umap_model, hdbscan_model=hdbscan_model, verbose=True).fit(abstracts, embeddings)` — can reuse previously defined models/embeddings.
- **Topic info**: `get_topic_info()` shows Topic, Count, Name (four best keywords concatenated with "_"). First topic is −1 = outliers (14,520 docs in the example) — documents not fitted within a topic. Remove outliers via k-means (non-outlier algorithm) or `reduce_outliers()`.
- **Topic 0 example**: keywords speech, asr, recognition, end, acoustic... = automatic speech recognition (ASR). Other topics: medical/clinical/biomedical, sentiment/aspect/analysis/reviews, translation/nmt/machine/neural, coherence/discourse, prompt optimization, sentence/sts/embeddings, counseling/mental health, backdoor attacks.
- **get_topic(0)** returns (keyword, weight) tuples, e.g., ('speech', 0.028), ('asr', 0.019), ('recognition', 0.013).
- **find_topics("topic modeling")**: returns topic IDs and similarities — topic 22 at 0.95 similarity; `get_topic(22)` keywords: topic, topics, lda, latent, document, documents, modeling, dirichlet, word, allocation. BERTopic's own abstract assigned to topic 22 (`topic_model.topics_[titles.index("BERTopic: ...")]` → 22).
- **Interactive visualization**: `visualize_documents(titles, reduced_embeddings=..., width=1200, hide_annotations=True)`; `fig.update_layout(font=dict(size=16))`. Hover shows titles (not abstracts). Zoom in / double-click a topic to view only it.
- **Three relationship visualizations**: `visualize_barchart()` (ranked keywords), `visualize_heatmap(n_clusters=30)` (relationships between topics), `visualize_hierarchy()` (potential hierarchical structure).
- **Why rerank (the "Lego block")**: c-TF-IDF bag-of-words doesn't take semantic structures into account. Solution: leverage the speed of bag-of-words for an initial meaningful representation, then tweak it with more powerful but slower techniques (embedding models, generative models). Reranking an initial set of results is a main staple of neural search (Ch 8).
- **Representation blocks efficiency**: The optimization only needs to run once per topic, not per document — e.g., millions of documents but a hundred topics → apply representation block once per topic. Blocks can be stacked multiple times.
- **topic_differences() helper**: Compares top 5 words per topic between original and updated representations in a DataFrame.
- **KeyBERTInspired results**: Topics become easier to read, but embedding-based techniques can remove informative abbreviations (e.g., "nmt" = neural machine translation removed because the model couldn't represent the entity) — domain experts find such abbreviations highly informative.
- **MMR usage**: `MaximalMarginalRelevance(diversity=0.2)` — reduces redundancy (e.g., both "summaries" and "summary" are redundant); goes from ~30 initial keywords to ~10 more diverse ones. Topic 4 after MMR: only one "summary"-like word plus other contributing words.
- **KeyBERTInspired note**: tends to remove nearly all stop words since it focuses on semantic relationships between words and documents.
- **Text generation block**: Instead of generating/reranking keywords for all documents (potentially millions), generate a short label per topic based on previously generated keywords + a small set of representative documents. Prompt components: [DOCUMENTS] tag (typically four documents best representing the topic — those with highest cosine similarity of c-TF-IDF values to the topic) and [KEYWORDS] tag.
- **Flan-T5 label generation**: `pipeline("text2text-generation", model="google/flan-t5-small")`; `TextGeneration(generator, prompt=prompt, doc_length=50, tokenizer="whitespace")`. Prompt: "I have a topic that contains the following documents: [DOCUMENTS] The topic is described by the following keywords: '[KEYWORDS]'. Based on the documents and keywords, what is this topic about?" Results: topic 0 "Speech-to-description", topic 1 "Science/Tech" (too broad), topic 3 "Attention-based neural machine translation", topic 4 "Summarization".
- **GPT-3.5 label generation**: `client = openai.OpenAI(api_key="YOUR_KEY_HERE")`; `OpenAI(client, model="gpt-3.5-turbo", exponential_backoff=True, chat=True, prompt=prompt)`. Prompt asks to extract "topic: <short topic label>". Results more informative (topic 0 "Leveraging External Data for Improving Low-Res...", topic 4 "Document Summarization Techniques"). Not confined to OpenAI — BERTopic has local backends too.
- **Multiple representations advised**: No model is perfect; generate multiple topic representations — e.g., use KeyBERTInspired, MMR, and GPT-3.5 side by side for different perspectives.
- **datamapplot visualization**: `visualize_document_datamap(titles, topics=list(range(20)), reduced_embeddings=..., width=1200, label_font_size=11, label_wrap_width=20, use_medoids=True)` — beautiful illustrations of top 20 topics.
- **Chapter's meta-theme**: combining encoder-only (embeddings), decoder-only (generative), and classical methods (bag-of-words) results in amazing new techniques and pipelines.
- **Next chapter**: prompt engineering (Ch 6) — a common method for improving the output of generative models.

### Important Numbers / Stats / Tokens
- ArXiv dataset: 44,949 abstracts, 1991–2024, cs.CL (Computation and Language) section (p.2).
- Embeddings shape: `(44949, 384)` — 384 values per document embedding (p.4).
- UMAP: `n_components=5` (5–10 generally work well), `min_dist=0.0`, `metric='cosine'`, `random_state=42` (p.6).
- HDBSCAN: `min_cluster_size=50`, `metric="euclidean"`, `cluster_selection_method="eom"` → 156 clusters (p.8).
- Cluster 0 example: sign language translation documents (p.8).
- Visualization UMAP: `n_components=2` for x/y plot; matplotlib `alpha=0.05`/`s=2`/`c="grey"` for outliers; `alpha=0.6`, `s=2`, `cmap="tab20b"` for clusters (p.9).
- Topic −1 (outliers) count: 14,520 in the example (p.17).
- Topic 0 count: 2,290; topic 1: 1,403; topic 2: 1,156; topic 3: 986 (p.17).
- Topic 0 keywords: speech (0.028), asr (0.019), recognition (0.013), end (0.0098), acoustic, speaker, audio, the, error, automatic (p.17).
- find_topics("topic modeling") → topic 22 at similarity 0.95 (also −1 at 0.91, 1 at 0.907, 47 at 0.907, 32 at 0.905) (p.18).
- Topic 22 keywords: topic, topics, lda, latent, document, documents, modeling, dirichlet, word, allocation (p.18).
- BERTopic's own abstract → topic 22 (p.18).
- visualize_heatmap uses `n_clusters=30` (p.20).
- MMR: diversity=0.2; from ~30 initial keywords to ~10 (p.23-24).
- TextGeneration: doc_length=50, tokenizer="whitespace" (p.25).
- Representative documents for prompt: typically four ([DOCUMENTS] tag) (p.25).
- Example topic labels (Flan-T5): topic 0 "Speech-to-description", topic 1 "Science/Tech", topic 2 "Review", topic 3 "Attention-based neural machine translation", topic 4 "Summarization" (p.26).
- Example topic labels (GPT-3.5): topic 0 "Leveraging External Data for Improving Low-Resource...", topic 4 "Document Summarization Techniques" (p.27).
- datamapplot: topics=list(range(20)) top 20 topics (p.27).

### Algorithms & Formulæ
- **Three-step clustering pipeline**:
  1. `embedding_model = SentenceTransformer("thenlper/gte-small")`; `embeddings = embedding_model.encode(abstracts)`.
  2. `umap_model = UMAP(n_components=5, min_dist=0.0, metric='cosine', random_state=42)`; `reduced_embeddings = umap_model.fit_transform(embeddings)`.
  3. `hdbscan_model = HDBSCAN(min_cluster_size=50, metric="euclidean", cluster_selection_method="eom").fit(reduced_embeddings)`; `clusters = hdbscan_model.labels_`.
- **c-TF-IDF**: Each word's frequency within a cluster (c-TF) × IDF weight. IDF = log(average frequency of all words across all clusters / total frequency of each word).
- **KeyBERTInspired**: c-TF-IDF → most representative documents per topic → average document embedding per topic → compare with candidate keyword embeddings (cosine) → rerank keywords.
- **MMR**: embed candidate keywords; iteratively add the next best keyword — diverse from already-chosen keywords but related to documents; diversity parameter controls how diverse keywords need to be.
- **TextGeneration (Flan-T5)**: prompt with [DOCUMENTS] (top 4 docs by c-TF-IDF cosine similarity) and [KEYWORDS]; generate topic label per topic.
- **GPT-3.5 OpenAI block**: prompt asking for "topic: <short topic label>"; `exponential_backoff=True`, `chat=True`, `model="gpt-3.5-turbo"`.
- **Visualization**: 2D UMAP → DataFrame(x, y, title, cluster) → scatter outliers (grey) + clusters (tab20b colormap).

### Diagrams / Visuals
- **Figure 5-1** — Clustering unstructured textual data (groups of semantically similar documents).
- **Figure 5-2** — Topic modeling gives meaning to clusters of textual documents.
- **Figure 5-3** — Step 1: convert documents to embeddings using an embedding model.
- **Figure 5-4** — Dimensionality reduction compresses high-dimensional data to a lower-dimensional representation (global structure preserved).
- **Figure 5-5** — Step 2: embeddings are reduced to a lower-dimensional space using dimensionality reduction.
- **Figure 5-6** — Step 3: cluster the documents using embeddings with reduced dimensionality.
- **Figure 5-7** — The clustering algorithm impacts how clusters are generated and how they are viewed (centroid vs density-based).
- **Figure 5-8** — Generated clusters (colored) and outliers (gray) as a 2D visualization.
- **Figure 5-9** — Traditionally, topics are represented by keywords but can take other forms.
- **Figure 5-10** — Keywords are extracted based on their distribution over a single topic (LDA).
- **Figure 5-11** — The first part of BERTopic's pipeline: create clusters of semantically similar documents.
- **Figure 5-12** — A bag-of-words counts the number of times each word appears in a document.
- **Figure 5-13** — Generating c-TF: counting word frequency per cluster instead of per document.
- **Figure 5-14** — Creating a weighting scheme (IDF calculation).
- **Figure 5-15** — The second part of BERTopic's pipeline: representing topics — calculating the weight of term x in a class c.
- **Figure 5-16** — The full BERTopic pipeline: clustering and topic representation.
- **Figure 5-17** — BERTopic modularity: swap any component (embedding model, clustering algorithm, etc.).
- **Figure 5-18** — Output of visualizing documents and topics (interactive plot).
- **Figure 5-19** — Fine-tuning topic representations by reranking the original c-TF-IDF distributions.
- **Figure 5-20** — The reranker (representation) block sits on top of the c-TF-IDF representation.
- **Figure 5-21** — After c-TF-IDF weighting, topics can be fine-tuned with a wide variety of representation models (many are LLMs).
- **Figure 5-22** — KeyBERTInspired representation model procedure.
- **Figure 5-23** — Use text generative LLMs and prompt engineering to create labels for topics from keywords and documents.
- **Figure 5-24** — The top 20 topics visualized (datamapplot).

### Common Exam Traps
- **The three-step pipeline order**: embed → reduce dimensionality → cluster. Don't cluster before reducing.
- **Embedding model swap**: This chapter uses `thenlper/gte-small`, NOT all-mpnet-base-v2 (that was Ch 4). gte-small outperforms it on clustering and is faster due to small size.
- **Embeddings shape (44949, 384)**: 44,949 abstracts × 384 dims — not (384, 44949).
- **Why reduce dimensionality**: exponential growth of possible values per dimension; not just "speed". It's a compression technique — information is always lost.
- **UMAP vs PCA**: UMAP chosen because it handles nonlinear relationships/structures better than PCA.
- **min_dist=0.0** → tighter clusters; **metric='cosine'** because Euclidean-based methods have issues with high-dimensional data; **random_state=42** → reproducible but disables parallelism (slower).
- **k-means vs HDBSCAN**: k-means (centroid-based) requires the number of clusters; HDBSCAN (density-based) freely calculates it and doesn't force all points into clusters. Outliers → label −1, ignored.
- **min_cluster_size**: lowering it creates MORE clusters (it's the minimum cluster size).
- **156 clusters** result; topic −1 = outliers (14,520 docs in example).
- **Visualization info loss**: 2D approximations may push clusters together or apart; human inspection is key.
- **LDA vs BERTopic**: LDA assumes each topic is a probability distribution of words, uses bag-of-words (no context); BERTopic uses embeddings + c-TF-IDF.
- **c-TF vs bag-of-words**: c-TF counts word frequency per CLUSTER, not per document.
- **c-TF-IDF weighting**: IDF = log(average frequency across all clusters / total frequency of each word). Higher weight = more representative of the topic.
- **BERTopic name "Name" column**: four best keywords concatenated with "_" (e.g., `0_speech_asr_recognition_end`).
- **find_topics vs get_topic**: find_topics searches by term (returns IDs + similarities); get_topic returns a topic's keywords with weights.
- **Outlier removal**: k-means (non-outlier algorithm) or `reduce_outliers()`; NOT deleting.
- **Representation blocks run per topic, not per document**: efficiency benefit (millions of docs, hundreds of topics).
- **KeyBERTInspired downside**: can remove informative abbreviations (e.g., "nmt") that domain experts find highly informative.
- **MMR purpose**: diversity/redundancy reduction (removes near-duplicates like "summary"/"summaries"); diversity parameter; e.g., 30 → 10 keywords.
- **TextGeneration uses prompts**: [DOCUMENTS] (typically ~4 most representative docs) + [KEYWORDS]; generates a short label per topic.
- **GPT-3.5 block params**: model="gpt-3.5-turbo", exponential_backoff=True, chat=True.
- **Multiple representations advised**: no model is perfect; combine KeyBERTInspired, MMR, GPT-3.5 side by side.
- **BERTopic modularity**: components are "Lego blocks" — fully replaceable; new models can be integrated.
- **Meta-theme**: combines encoder-only (embeddings), decoder-only (generative), classical (bag-of-words).

### Chapter Summary
Chapter 5 explores unsupervised learning: text clustering and topic modeling. It introduces a three-step clustering pipeline (embed documents, reduce dimensionality, cluster) applied to 44,949 ArXiv cs.CL abstracts: gte-small embeddings (44949×384), UMAP reduction to 5 dims, and HDBSCAN clustering (156 clusters, with outliers ignored). Manual cluster inspection and 2D visualization show the clusters' structure, though dimensionality reduction always loses information.

Topic modeling extends this by representing clusters automatically. BERTopic clusters semantically similar documents, then represents each cluster as a topic using bag-of-words enhanced with class-based TF-IDF (c-TF-IDF). Its modular "Lego block" architecture allows replacing any component and adding representation models to fine-tune topic representations: KeyBERTInspired (reranking via embedding similarity), Maximal Marginal Relevance (diversifying keywords), and generative text labels via Flan-T5 and GPT-3.5 prompt engineering. The chapter emphasizes combining encoder-only, decoder-only, and classical methods, and generating multiple topic representations for different perspectives. Chapter 6 shifts to prompt engineering.

### Confidence Check
- **Sure**: three-step clustering pipeline order; ArXiv dataset stats (44,949 abstracts, cs.CL, 1991–2024); gte-small model + (44949, 384) shape; UMAP parameters (n_components=5, min_dist=0.0, cosine, random_state=42) and trade-offs; HDBSCAN (density-based, 156 clusters, −1 outliers, min_cluster_size=50); cluster 0 = sign language; LDA vs BERTopic; c-TF-IDF mechanics (per-cluster frequency, IDF log formula); BERTopic two steps + modularity; KeyBERTInspired, MMR (diversity, 30→10), Flan-T5/GPT-3.5 text generation labels; find_topics/get_topic/get_topic_info; visualization functions (barchart, heatmap, hierarchy, datamap).
- **Uncertain**: exact figure numbers in the printed page flow (page anchors from PDF text are approximate); the precise wording of some quoted passages where PDF extraction broke lines mid-sentence (numbered-list markers duplicated by extraction); minor — the exact topic counts for topics 5–149 (only selected rows shown in the table).

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading the ArXiv dataset
```python
# Load data from Hugging Face
from datasets import load_dataset
dataset = load_dataset("maartengr/arxiv_nlp")["train"]
# Extract metadata
abstracts = dataset["Abstracts"]
titles = dataset["Titles"]
```
- **Explanation:** Loads the arxiv_nlp dataset (44,949 abstracts, 1991–2024, cs.CL section) and extracts the abstracts and titles into separate variables.
- **Fits the architecture:** Abstracts are the documents to cluster; titles are used for visualization.

### Code Block 2: Embedding the abstracts
```python
from sentence_transformers import SentenceTransformer
# Create an embedding for each abstract
embedding_model = SentenceTransformer("thenlper/gte-small")
embeddings = embedding_model.encode(abstracts, show_progress_bar=True)
```
```python
# Check the dimensions of the resulting embeddings
embeddings.shape
(44949, 384)
```
- **Explanation:** Embeds every abstract with gte-small; the result has shape (44949, 384) — 44,949 documents, 384 values each.
- **Fits the architecture:** Step 1 of the pipeline — embeddings are the features to be clustered.

### Code Block 3: Dimensionality reduction with UMAP
```python
from umap import UMAP
# We reduce the input embeddings from 384 dimensions to 5 dimensions
umap_model = UMAP(
    n_components=5, min_dist=0.0, metric='cosine', random_state=42
)
reduced_embeddings = umap_model.fit_transform(embeddings)
```
- **Explanation:** Reduces 384-dim embeddings to 5 dims. n_components=5 (5–10 works well), min_dist=0.0 (tighter clusters), metric='cosine' (Euclidean struggles with high-dim data), random_state=42 (reproducible, but disables parallelism).
- **Fits the architecture:** Step 2 — reduce dimensionality to help the cluster model create meaningful clusters.

### Code Block 4: Clustering with HDBSCAN
```python
from hdbscan import HDBSCAN
# We fit the model and extract the clusters
hdbscan_model = HDBSCAN(
    min_cluster_size=50, metric="euclidean", cluster_selection_method="eom"
).fit(reduced_embeddings)
clusters = hdbscan_model.labels_
# How many clusters did we generate?
len(set(clusters))
156
```
- **Explanation:** Clusters the reduced embeddings with HDBSCAN (density-based), producing 156 clusters; outliers get label −1. Lowering min_cluster_size creates more clusters.
- **Fits the architecture:** Step 3 — density-based clustering without specifying the number of clusters.

### Code Block 5: Inspecting cluster 0 documents
```python
import numpy as np
# Print first three documents in cluster 0
cluster = 0
for index in np.where(clusters==cluster)[0][:3]:
    print(abstracts[index][:300] + "... \n")
```
- **Explanation:** Finds the first three document indices in cluster 0 and prints the first 300 chars of each abstract.
- **Fits the architecture:** Manual inspection reveals cluster 0 is about sign language translation.

### Code Block 6: 2D visualization of clusters
```python
import pandas as pd
# Reduce 384-dimensional embeddings to two dimensions for easier visualization
reduced_embeddings = UMAP(
    n_components=2, min_dist=0.0, metric="cosine", random_state=42
).fit_transform(embeddings)
# Create dataframe
df = pd.DataFrame(reduced_embeddings, columns=["x", "y"])
df["title"] = titles
df["cluster"] = [str(c) for c in clusters]
# Select outliers and non-outliers (clusters)
to_plot = df.loc[df.cluster != "-1", :]
outliers = df.loc[df.cluster == "-1", :]
```
```python
import matplotlib.pyplot as plt
# Plot outliers and non-outliers separately
plt.scatter(outliers_df.x, outliers_df.y, alpha=0.05, s=2, c="grey")
plt.scatter(
    clusters_df.x, clusters_df.y, c=clusters_df.cluster.astype(int),
    alpha=0.6, s=2, cmap="tab20b"
)
plt.axis("off")
```
- **Explanation:** Reduces embeddings to 2D, builds a DataFrame (x, y, title, cluster), separates outliers (−1) from clusters, and plots outliers in grey and clusters colored by cluster id.
- **Fits the architecture:** Visualization is an approximation — dimensionality reduction for visualization loses information, so human inspection remains key.

### Code Block 7: Training BERTopic
```python
from bertopic import BERTopic
# Train our model with our previously defined models
topic_model = BERTopic(
    embedding_model=embedding_model,
    umap_model=umap_model,
    hdbscan_model=hdbscan_model,
    verbose=True
).fit(abstracts, embeddings)
```
- **Explanation:** Trains BERTopic reusing the previously defined embedding model, UMAP model, and HDBSCAN model.
- **Fits the architecture:** BERTopic = clustering step + topic representation step; components are interchangeable.

### Code Block 8: Exploring topic info
```python
topic_model.get_topic_info()
```
```
Topic Count Name  Representation
-1  14520  -1_the_of_and_to  [the, of, and, to, in, we, that, language, for...
0   2290   0_speech_asr_recognition_end  [speech, asr, recognition, end, acoustic, spea...
1   1403   1_medical_clinical_biomedical_patient  [medical, clinical, biomedical, patient, healt...
2   1156   2_sentiment_aspect_analysis_reviews  [sentiment, aspect, analysis, reviews, opinion...
...
```
- **Explanation:** Shows each topic's ID, document count, and Name (four best keywords joined with "_"). Topic −1 = outliers (14,520 documents).
- **Fits the architecture:** The Name column quickly conveys each topic's content.

### Code Block 9: Inspecting and searching topics
```python
topic_model.get_topic(0)
```
```
[('speech', 0.028177697715245358),
 ('asr', 0.018971184497453525),
 ('recognition', 0.013457745472471012),
 ('end', 0.00980445092749381),
 ...]
```
```python
topic_model.find_topics("topic modeling")
([22, -1, 1, 47, 32],
 [0.95456535, 0.91173744, 0.9074769, 0.9067007, 0.90510106])
```
```python
topic_model.topics_[titles.index("BERTopic: Neural topic modeling with a class-based TF-IDF procedure")]
22
```
- **Explanation:** `get_topic(0)` returns (keyword, weight) tuples (topic 0 = ASR). `find_topics("topic modeling")` returns topic IDs + similarities — topic 22 at 0.95. The BERTopic paper's own abstract is assigned to topic 22.
- **Fits the architecture:** Search and inspection functions make topic exploration fast.

### Code Block 10: Interactive topic visualization
```python
# Visualize topics and documents
fig = topic_model.visualize_documents(
    titles, 
    reduced_embeddings=reduced_embeddings, 
    width=1200, 
    hide_annotations=True
)
# Update fonts of legend for easier visualization
fig.update_layout(font=dict(size=16))
```
```python
# Visualize barchart with ranked keywords
topic_model.visualize_barchart()
# Visualize relationships between topics
topic_model.visualize_heatmap(n_clusters=30)
# Visualize the potential hierarchical structure of topics
topic_model.visualize_hierarchy()
```
- **Explanation:** Creates an interactive plot (hover shows titles) plus barchart, heatmap (30 cluster groups), and hierarchy visualizations.
- **Fits the architecture:** Visualization functions let users explore topics and their relationships.

### Code Block 11: Saving original representations
```python
# Save original representations
from copy import deepcopy
original_topics = deepcopy(topic_model.topic_representations_)
```
- **Explanation:** Deep-copies the original topic representations for later comparison.
- **Fits the architecture:** Enables comparing representations with and without representation models.

### Code Block 12: The topic_differences helper
```python
def topic_differences(model, original_topics, nr_topics=5):
    """Show the differences in topic representations between two models """
    df = pd.DataFrame(columns=["Topic", "Original", "Updated"])
    for topic in range(nr_topics):
        # Extract top 5 words per topic per model
        og_words = " | ".join(list(zip(*original_topics[topic]))[0][:5])
        new_words = " | ".join(list(zip(*model.get_topic(topic)))[0][:5])
        df.loc[len(df)] = [topic, og_words, new_words]
    return df
```
- **Explanation:** Builds a DataFrame comparing the top 5 keywords per topic between the original and updated representations.
- **Fits the architecture:** Quick visual comparison of representation-model effects.

### Code Block 13: KeyBERTInspired representation
```python
from bertopic.representation import KeyBERTInspired
# Update our topic representations using KeyBERTInspired
representation_model = KeyBERTInspired()
topic_model.update_topics(abstracts, representation_model=representation_model)
# Show topic differences
topic_differences(topic_model, original_topics)
```
```
Topic Original                        Updated
0    speech | asr | recognition | end | acoustic   speech | encoder | phonetic | language | trans...
1    medical | clinical | biomedical | patient | he...  nlp | ehr | clinical | biomedical | language
...
```
- **Explanation:** Reranks keywords by comparing average document embeddings per topic with candidate keyword embeddings (KeyBERT-style). Results are easier to read, but abbreviations like "nmt" may be removed.
- **Fits the architecture:** Representation blocks update topics without redoing dimensionality reduction and clustering.

### Code Block 14: MMR representation
```python
from bertopic.representation import MaximalMarginalRelevance
# Update our topic representations to MaximalMarginalRelevance
representation_model = MaximalMarginalRelevance(diversity=0.2)
topic_model.update_topics(abstracts, representation_model=representation_model)
# Show topic differences
topic_differences(topic_model, original_topics)
```
- **Explanation:** Diversifies keywords — from ~30 initial keywords to ~10 more diverse ones (e.g., topic 4 keeps only one "summary"-like word).
- **Fits the architecture:** Reduces redundancy in topic representations.

### Code Block 15: Flan-T5 text generation representation
```python
from transformers import pipeline
from bertopic.representation import TextGeneration
prompt = """I have a topic that contains the following documents: 
[DOCUMENTS]
The topic is described by the following keywords: '[KEYWORDS]'.
Based on the documents and keywords, what is this topic about?"""
# Update our topic representations using Flan-T5
generator = pipeline("text2text-generation", model="google/flan-t5-small")
representation_model = TextGeneration(
    generator, prompt=prompt, doc_length=50, tokenizer="whitespace"
)
topic_model.update_topics(abstracts, representation_model=representation_model)
# Show topic differences
topic_differences(topic_model, original_topics)
```
- **Explanation:** Generates a short label per topic using Flan-T5 with a prompt containing [DOCUMENTS] and [KEYWORDS]. Labels: topic 0 "Speech-to-description", topic 3 "Attention-based neural machine translation", etc.
- **Fits the architecture:** Generative models label each topic once (not each document), improving interpretability.

### Code Block 16: GPT-3.5 text generation representation
```python
import openai
from bertopic.representation import OpenAI
prompt = """
I have a topic that contains the following documents:
[DOCUMENTS]
The topic is described by the following keywords: [KEYWORDS]
Based on the information above, extract a short topic label in the following 
format:
topic: <short topic label>
"""
# Update our topic representations using GPT-3.5
client = openai.OpenAI(api_key="YOUR_KEY_HERE")
representation_model = OpenAI(
    client, model="gpt-3.5-turbo", exponential_backoff=True, chat=True, 
    prompt=prompt
)
topic_model.update_topics(abstracts, representation_model=representation_model)
# Show topic differences
topic_differences(topic_model, original_topics)
```
- **Explanation:** Uses GPT-3.5 (via OpenAI API) to generate topic labels in the format "topic: <short topic label>". Results are more informative (e.g., "Document Summarization Techniques").
- **Fits the architecture:** Closed-source generative models as representation blocks; BERTopic also supports local backends.

### Code Block 17: datamapplot visualization
```python
# Visualize topics and documents
fig = topic_model.visualize_document_datamap(
    titles,
    topics=list(range(20)),
    reduced_embeddings=reduced_embeddings,
    width=1200,
    label_font_size=11,
    label_wrap_width=20,
    use_medoids=True,
)
```
- **Explanation:** Creates a datamapplot visualization of the top 20 topics with labels.
- **Fits the architecture:** Beautiful illustrations of topics using GPT-3.5-generated labels.

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*

## §4. Practice Exam
*(Separate file: `practice_exam.md`)*
