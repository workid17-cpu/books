# 📘 Chapter 5 Line-by-Line: Text Clustering and Topic Modeling
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 5
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "Although supervised techniques, such as classification, have reigned supreme over the last few years in the industry, the potential of unsupervised techniques such as text clustering cannot be understated."
   - **Plain English:** Supervised methods dominate industry, but unsupervised text clustering has huge untapped potential.
   - **Word meanings:** reigned supreme = dominated; understated = downplayed.
   - **Technical terms:** supervised techniques; unsupervised techniques; text clustering.

2. **Quote:** "Text clustering aims to group similar texts based on their semantic content, meaning, and relationships. As illustrated in Figure 5-1, the resulting clusters of semantically similar documents not only facilitate efficient categorization of large volumes of unstructured text but also allow for quick exploratory data analysis."
   - **Plain English:** Clustering groups similar texts by meaning; the groups help categorize large text collections and enable fast exploration.
   - **Word meanings:** facilitate = make easier; exploratory data analysis = initial data exploration.
   - **Technical terms:** semantic content; clusters; unstructured text.

3. **Quote:** "The recent evolution of language models, which enable contextual and semantic representations of text, has enhanced the effectiveness of text clustering. Language is more than a bag of words, and recent language models have proved to be quite capable of capturing that notion."
   - **Plain English:** Modern LLMs capture meaning/context, so clustering works much better; language is more than just word counts.
   - **Technical terms:** language models; contextual representations; bag of words.

4. **Quote:** "Text clustering, unbound by supervision, allows for creative solutions and diverse applications, such as finding outliers, speedup labeling, and finding incorrectly labeled data."
   - **Plain English:** Without labels, clustering enables creative uses: finding outliers, speeding up labeling, and catching mislabeled data.
   - **Word meanings:** unbound = not restricted; outliers = unusual data points.
   - **Technical terms:** outliers; labeling.

5. **Quote:** "Text clustering has also found itself in the realm of topic modeling, where we want to discover (abstract) topics that appear in large collections of textual data. As shown in Figure 5-2, we generally describe a topic using keywords or keyphrases and, ideally, have a single overarching label."
   - **Plain English:** Clustering leads to topic modeling — discovering hidden topics in large text collections, described by keywords (ideally one overall label).
   - **Word meanings:** overarching = covering everything.
   - **Technical terms:** topic modeling; keywords; keyphrases; label.

6. **Quote:** "In this chapter, we will first explore how to perform clustering with embedding models and then transition to a text-clustering-inspired method of topic modeling, namely BERTopic."
   - **Plain English:** Chapter first covers embedding-based clustering, then BERTopic for topic modeling.
   - **Technical terms:** embedding models; BERTopic.

7. **Quote:** "Text clustering and topic modeling have an important role in this book as they explore creative ways to combine a variety of different language models. We will explore how combining encoder-only (embeddings), decoder-only (generative), and even classical methods (bag-of-words) can result in amazing new techniques and pipelines."
   - **Plain English:** Chapter theme: combining encoder-only, decoder-only, and classical bag-of-words methods into new pipelines.
   - **Technical terms:** encoder-only models; decoder-only models; bag-of-words; pipelines.

### ArXiv's Articles: Computation and Language

8. **Quote:** "Throughout this chapter, we will be running clustering and topic modeling algorithms on ArXiv articles. ArXiv is an open-access platform for scholarly articles, mostly in the fields of computer science, mathematics, and physics. We will explore articles in the field of Computation and Language to keep with the theme of this book. The dataset contains 44,949 abstracts between 1991 and 2024 from ArXiv's cs.CL (Computation and Language) section."
   - **Plain English:** Dataset = 44,949 ArXiv abstracts (1991–2024) from the cs.CL section.
   - **Word meanings:** open-access = freely available; abstracts = short summaries.
   - **Technical terms:** ArXiv; cs.CL.

### Code Block: Loading the ArXiv dataset
```python
# Load data from Hugging Face
from datasets import load_dataset
dataset = load_dataset("maartengr/arxiv_nlp")["train"]
# Extract metadata
abstracts = dataset["Abstracts"]
titles = dataset["Titles"]
```
- **Explanation:** Loads the arxiv_nlp dataset from Hugging Face (44,949 abstracts) and extracts abstracts and titles into separate variables.
- **Fits the architecture:** Abstracts are the documents to cluster; titles are used for visualization.

### A Common Pipeline for Text Clustering

9. **Quote:** "Text clustering allows for discovering patterns in data that you may or may not be familiar with. It allows for getting an intuitive understanding of the task, for example, a classification task, but also of its complexity."
   - **Plain English:** Clustering reveals both known and unknown patterns and gives intuition about a task's structure/complexity.
   - **Word meanings:** intuitive = based on understanding, not formal rules.
   - **Technical terms:** pattern discovery.

10. **Quote:** "Although there are many methods for text clustering, from graph-based neural networks to centroid-based clustering techniques, a common pipeline that has gained popularity involves three steps and algorithms: 1. Convert the input documents to embeddings with an embedding model. 2. Reduce the dimensionality of embeddings with a dimensionality reduction model. 3. Find groups of semantically similar documents with a cluster model."
    - **Plain English:** Many clustering methods exist; the common popular pipeline has three steps: embed → reduce dimensionality → cluster.
    - **Technical terms:** graph-based neural networks; centroid-based clustering; embeddings; dimensionality reduction; cluster model.

### Embedding Documents

11. **Quote:** "The first step is to convert our textual data to embeddings, as illustrated in Figure 5-3. Recall from previous chapters that embeddings are numerical representations of text that attempt to capture its meaning."
    - **Plain English:** Step 1: convert text to numeric embeddings that capture meaning.
    - **Technical terms:** embeddings; numerical representation.

12. **Quote:** "Choosing embedding models optimized for semantic similarity tasks is especially important for clustering as we attempt to find groups of semantically similar documents. Fortunately, most embedding models at the time of writing focus on just that, semantic similarity."
    - **Plain English:** Pick embedding models optimized for semantic similarity since clustering finds meaning-similar groups.
    - **Technical terms:** semantic similarity; embedding model selection.

13. **Quote:** "As we did in the previous chapter, we will use the MTEB leaderboard to select an embedding model. We will need an embedding model that has a decent score on clustering tasks but also is small enough to run quickly. Instead of using the 'sentence-transformers/all-mpnet-base-v2' model we used in the previous chapter, we use the 'thenlper/gte-small' model instead. It is a more recent model that outperforms the previous model on clustering tasks and due to its small size is even faster for inference."
    - **Plain English:** MTEB leaderboard guides selection; gte-small chosen over all-mpnet-base-v2 because it's newer, better on clustering, and faster.
    - **Technical terms:** MTEB leaderboard; thenlper/gte-small; all-mpnet-base-v2; inference speed.

### Code Block: Embedding the abstracts
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
- **Explanation:** Embeds every abstract with gte-small; shape (44949, 384) — 44,949 documents × 384 values each.
- **Fits the architecture:** Step 1 of the pipeline — embeddings are the features to be clustered.

14. **Quote:** "Each embedding has 384 values that together represent the semantic representation of the document. You can view these embeddings as the features that we want to cluster."
    - **Plain English:** Each document is a 384-value vector; these are the features for clustering.
    - **Technical terms:** features; embedding dimension.

### Reducing the Dimensionality of Embeddings

15. **Quote:** "Before we cluster the embeddings, we will first need to take their high dimensionality into account. As the number of dimensions increases, there is an exponential growth in the number of possible values within each dimension. Finding all subspaces within each dimension becomes increasingly complex."
    - **Plain English:** High-dimensional data explodes exponentially in possible values, making it hard to find structure.
    - **Word meanings:** subspaces = lower-dimensional regions within the space.
    - **Technical terms:** high dimensionality; exponential growth.

16. **Quote:** "As a result, high-dimensional data can be troublesome for many clustering techniques as it gets more difficult to identify meaningful clusters. Instead, we can make use of dimensionality reduction. As illustrated in Figure 5-4, this technique allows us to reduce the size of the dimensional space and represent the same data with fewer dimensions. Dimensionality reduction techniques aim to preserve the global structure of high-dimensional data by finding low-dimensional representations."
    - **Plain English:** Reduce dimensions to help clustering; techniques try to preserve global structure.
    - **Technical terms:** dimensionality reduction; global structure; low-dimensional representation.

17. **Quote (advice box):** "Dimensionality reduction techniques, however, are not flawless. They do not perfectly capture high-dimensional data in a lower-dimensional representation. Information will always be lost with this procedure. There is a balance between reducing dimensionality and keeping as much information as possible."
    - **Plain English:** Dimensionality reduction is lossy — it always loses information; balance reduction vs. retained info.
    - **Word meanings:** flawless = without defects; lossy = loses information.
    - **Technical terms:** information loss; compression.

18. **Quote:** "Well-known methods for dimensionality reduction are Principal Component Analysis (PCA) and Uniform Manifold Approximation and Projection (UMAP). For this pipeline, we are going with UMAP as it tends to handle nonlinear relationships and structures a bit better than PCA."
    - **Plain English:** PCA and UMAP are common reduction methods; UMAP chosen because it handles nonlinear structure better.
    - **Technical terms:** PCA; UMAP; nonlinear relationships.

### Code Block: Dimensionality reduction with UMAP
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

19. **Quote:** "We can use the n_components parameter to decide the shape of the lower-dimensional space, namely 5 dimensions. Generally, values between 5 and 10 work well to capture high-dimensional global structures. The min_dist parameter is the minimum distance between embedded points. We are setting this to 0 as that generally results in tighter clusters. We set metric to 'cosine' as Euclidean-based methods have issues dealing with high-dimensional data. Note that setting a random_state in UMAP will make the results reproducible across sessions but will disable parallelism and therefore slow down training."
    - **Plain English:** n_components=5 (5–10 is good); min_dist=0 → tighter clusters; metric='cosine' (not Euclidean); random_state → reproducible but slower (no parallelism).
    - **Technical terms:** n_components; min_dist; cosine metric; random_state; parallelism.

### Cluster the Reduced Embeddings

20. **Quote:** "Although a common choice is a centroid-based algorithm like k-means, which requires a set of clusters to be generated, we do not know the number of clusters beforehand. Instead, a density-based algorithm freely calculates the number of clusters and does not force all data points to be part of a cluster, as illustrated in Figure 5-7."
    - **Plain English:** k-means needs a preset cluster count; density-based methods (like HDBSCAN) figure it out and don't force every point into a cluster.
    - **Technical terms:** centroid-based algorithm; k-means; density-based algorithm.

21. **Quote:** "A common density-based model is Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN). HDBSCAN is a hierarchical variation of a clustering algorithm called DBSCAN that allows for dense (micro)-clusters to be found without having to explicitly specify the number of clusters. As a density-based method, HDBSCAN can also detect outliers in the data, which are data points that do not belong to any cluster. These outliers will not be assigned or forced to belong to any cluster. In other words, they are ignored. Since ArXiv articles might contain some niche papers, using a model that detects outliers could be helpful."
    - **Plain English:** HDBSCAN = hierarchical DBSCAN; finds dense clusters without a preset count, flags outliers (ignored, not forced). Good for ArXiv's niche papers.
    - **Word meanings:** niche = specialized/obscure.
    - **Technical terms:** HDBSCAN; DBSCAN; dense micro-clusters; outliers.

### Code Block: Clustering with HDBSCAN
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

22. **Quote:** "With HDBSCAN, we generated 156 clusters in our dataset. To create more clusters, we will need to reduce the value of min_cluster_size as it represents the minimum size that a cluster can take."
    - **Plain English:** 156 clusters resulted; to get more clusters, lower min_cluster_size.
    - **Technical terms:** min_cluster_size.

### Inspecting the Clusters

23. **Quote:** "Now that we have generated our clusters, we can inspect each cluster manually and explore the assigned documents to get an understanding of its content. For example, let us take a few random documents from cluster 0:"
    - **Plain English:** Manually inspect clusters by reading their documents.
    - **Technical terms:** manual inspection.

### Code Block: Inspecting cluster 0 documents
```python
import numpy as np
# Print first three documents in cluster 0
cluster = 0
for index in np.where(clusters==cluster)[0][:3]:
    print(abstracts[index][:300] + "... \n")
```
- **Explanation:** Finds the first three document indices in cluster 0 and prints the first 300 chars of each abstract.
- **Fits the architecture:** Manual inspection reveals cluster 0 is about sign language translation.

24. **Quote:** "From these documents, it seems that this cluster contains documents mostly about translation from and to sign language, interesting!"
    - **Plain English:** Cluster 0 = sign language translation papers.
    - **Technical terms:** (none) — interpretation of results.

25. **Quote:** "We can take this one step further and attempt to visualize our results instead of going through all documents manually. To do so, we will need to reduce our document embeddings to two dimensions, as that allows us to plot the documents on an x/y plane:"
    - **Plain English:** Visualize clusters by reducing embeddings to 2D for x/y plotting.
    - **Technical terms:** 2D visualization.

### Code Block: 2D visualization of clusters
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
- **Explanation:** Reduces embeddings to 2D, builds a DataFrame (x, y, title, cluster), separates outliers (−1) from clusters.
- **Fits the architecture:** Prepares data for plotting clusters vs. outliers separately.

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
- **Explanation:** Plots outliers in grey (alpha=0.05) and clusters colored by cluster id (cmap="tab20b", alpha=0.6).
- **Fits the architecture:** Visualizes the clustering result; colors cycle among many clusters.

26. **Quote (advice box):** "Using any dimensionality reduction technique for visualization purposes creates information loss. It is merely an approximation of what our original embeddings look like. Although it is informative, it might push clusters together and drive them further apart than they actually are. Human evaluation, inspecting the clusters ourselves, is therefore a key component of cluster analysis!"
    - **Plain English:** 2D plots are lossy approximations — clusters may look closer/farther than reality. Human inspection remains essential.
    - **Word meanings:** approximation = rough estimate.
    - **Technical terms:** information loss; approximation.

27. **Quote:** "As we can see in Figure 5-8, it tends to capture major clusters quite well. Note how clusters of points are colored in the same color, indicating that HDBSCAN put them in a group together. Since we have a large number of clusters, the plotting library cycles the colors between clusters, so don't think that all green points are one cluster, for example."
    - **Plain English:** Plot captures major clusters; same color = same cluster, but colors cycle, so same color ≠ same cluster.
    - **Technical terms:** color cycling.

28. **Quote:** "This is visually appealing but does not yet allow us to see what is happening inside the clusters. Instead, we can extend this visualization by going from text clustering to topic modeling."
    - **Plain English:** Static plots don't reveal cluster content — motivation to move to topic modeling.
    - **Technical terms:** topic modeling.

### From Text Clustering to Topic Modeling

29. **Quote:** "Text clustering is a powerful tool for finding structure among large collections of documents. In our previous example, we could manually inspect each cluster and identify them based on their collection of documents. For instance, we explored a cluster that contained documents about sign language. We could say that the topic of that cluster is 'sign language.'"
    - **Plain English:** We manually labeled the sign-language cluster as a "topic".
    - **Technical terms:** topic.

30. **Quote:** "This idea of finding themes or latent topics in a collection of textual data is often referred to as topic modeling. Traditionally, it involves finding a set of keywords or phrases that best represent and capture the meaning of the topic, as we illustrate in Figure 5-9."
    - **Plain English:** Topic modeling = automatically finding themes/latent topics, traditionally via keywords.
    - **Word meanings:** latent = hidden.
    - **Technical terms:** topic modeling; keywords; keyphrases.

31. **Quote:** "Instead of labeling a topic as 'sign language,' these techniques use keywords such as 'sign,' 'language,' and 'translation' to describe the topic. As such, this does not give a single label to a topic and instead requires the user to understand the meaning of the topic through those keywords."
    - **Plain English:** Topics described by keywords rather than a single label; reader interprets the meaning.
    - **Technical terms:** keyword representation.

32. **Quote:** "Classic approaches, like latent Dirichlet allocation, assume that each topic is characterized by a probability distribution of words in a corpus's vocabulary. Figure 5-10 demonstrates how each word in a vocabulary is scored against its relevance to each topic."
    - **Plain English:** LDA treats each topic as a probability distribution over the vocabulary; words scored by relevance to each topic.
    - **Technical terms:** latent Dirichlet allocation (LDA); probability distribution; vocabulary.

33. **Quote:** "These approaches generally use a bag-of-words technique for the main features of the textual data, which does not take the context nor the meaning of words and phrases into account. In contrast, our text clustering example does take both into account as it relies on Transformer-based embeddings that are optimized for semantic similarity and contextual meaning through attention."
    - **Plain English:** LDA uses bag-of-words (no context); our clustering uses Transformer embeddings (context + meaning).
    - **Technical terms:** bag-of-words; Transformer-based embeddings; attention.

34. **Quote:** "In this section, we will extend text clustering into the realm of topic modeling through a highly modular text clustering and topic modeling framework, namely BERTopic."
    - **Plain English:** Introduce BERTopic — a modular topic modeling framework.
    - **Technical terms:** BERTopic; modular framework.

### BERTopic: A Modular Topic Modeling Framework

35. **Quote:** "BERTopic is a topic modeling technique that leverages clusters of semantically similar texts to extract various types of topic representations. The underlying algorithm can be thought of in two steps. First, as illustrated in Figure 5-11, we follow the same procedure as we did before in our text clustering example. We embed documents, reduce their dimensionality, and finally cluster the reduced embedding to create groups of semantically similar documents."
    - **Plain English:** BERTopic step 1 = same clustering pipeline (embed, reduce, cluster).
    - **Technical terms:** topic representations; clustering pipeline.

36. **Quote:** "Second, it models a distribution over words in the corpus's vocabulary by leveraging a classic method, namely bag-of-words. The bag-of-words, as we discussed briefly in Chapter 1 and illustrate in Figure 5-12, does exactly what its name implies, counting the number of times each word appears in a document. The resulting representation could be used to extract the most frequent words inside a document."
    - **Plain English:** BERTopic step 2 = model word distribution via bag-of-words (count words per document).
    - **Technical terms:** bag-of-words; term frequency.

37. **Quote:** "There are two caveats, however. First, this is a representation on a document level and we are interested in a cluster-level perspective. To address this, the frequency of words is calculated within the entire cluster instead of only the document, as illustrated in Figure 5-13."
    - **Plain English:** Caveat 1: need cluster-level (not document-level) word frequencies → c-TF counts per cluster.
    - **Word meanings:** caveats = warnings/qualifications.
    - **Technical terms:** c-TF (class-based term frequency).

38. **Quote:** "Second, stop words like 'the' and 'I' tend to appear often in documents and provide little meaning to the actual documents. BERTopic uses a class-based variant of term frequency–inverse document frequency (c-TF-IDF) to put more weight on words that are more meaningful to a cluster and put less weight on words that are used across all clusters."
    - **Plain English:** Caveat 2: stop words are uninformative; c-TF-IDF down-weights words used across all clusters.
    - **Technical terms:** stop words; term frequency–inverse document frequency (TF-IDF); c-TF-IDF.

39. **Quote:** "Each word in the bag-of-words, the c-TF in c-TF-IDF, is multiplied by the IDF value of each word. As shown in Figure 5-14, the IDF value is calculated by taking the logarithm of the average frequency of all words across all clusters divided by the total frequency of each word."
    - **Plain English:** c-TF-IDF = word frequency × IDF; IDF = log(average frequency across all clusters ÷ total frequency of each word).
    - **Technical terms:** IDF; logarithm; weighting scheme.

40. **Quote:** "The result is a weight ('IDF') for each word that we can multiply with their frequency ('c-TF') to get the weighted values ('c-TF-IDF')."
    - **Plain English:** weight × frequency = c-TF-IDF value.
    - **Technical terms:** c-TF-IDF weighting.

41. **Quote:** "This second part of the procedure, as shown in Figure 5-15, allows for generating a distribution over words as we have seen before. We can use scikit-learn's CountVectorizer to generate the bag-of-words (or term frequency) representation. Here, each cluster is considered a topic that has a specific ranking of the corpus's vocabulary."
    - **Plain English:** CountVectorizer builds the bag-of-words; each cluster = a topic with a vocabulary ranking.
    - **Technical terms:** CountVectorizer; term frequency; ranking.

42. **Quote:** "Putting the two steps together, clustering and representing topics, results in the full pipeline of BERTopic, as illustrated in Figure 5-16. With this pipeline, we can cluster semantically similar documents and from the clusters generate topics represented by several keywords. The higher a word's weight in a topic, the more representative it is of that topic."
    - **Plain English:** Full BERTopic = clustering + topic representation; higher word weight = more representative.
    - **Technical terms:** full pipeline; representative keywords.

43. **Quote:** "A major advantage of this pipeline is that the two steps, clustering and topic representation, are largely independent of one another. For instance, with c-TF-IDF, we are not dependent on the models used in clustering the documents. This allows for significant modularity throughout every component of the pipeline. And as we will explore later in this chapter, it is a great starting point to fine-tune the topic representations."
    - **Plain English:** Clustering and topic representation are independent → high modularity; good starting point for fine-tuning.
    - **Word meanings:** independent = not reliant on each other.
    - **Technical terms:** modularity; fine-tuning topic representations.

44. **Quote:** "As illustrated in Figure 5-17, although sentence-transformers is used as the default embedding model, we can swap it with any other embedding technique. The same applies to all other steps. If you do not want outliers generated with HDBSCAN, you can use k-means instead."
    - **Plain English:** Every component is swappable — even the embedding model or clustering algorithm.
    - **Technical terms:** interchangeable components.

45. **Quote:** "You can think of this modularity as building with Lego blocks; each part of the pipeline is completely replaceable with another, similar algorithm. Through this modularity, newly released models can be integrated within its architecture. As the field of Language AI grows, so does BERTopic!"
    - **Plain English:** BERTopic = Lego blocks; new models plug in as the field grows.
    - **Technical terms:** modular architecture; integration.

### The Modularity of BERTopic

46. **Quote:** "The modularity of BERTopic has another advantage: it allows it to be used and adapted to different use cases using the same base model. For instance, BERTopic supports a wide variety of algorithmic variants: Guided topic modeling; (Semi-)supervised topic modeling; Hierarchical topic modeling; Dynamic topic modeling; Multimodal topic modeling; Multi-aspect topic modeling; Online and incremental topic modeling; Zero-shot topic modeling; Etc."
    - **Plain English:** Same base BERTopic adapts to many variants: guided, (semi-)supervised, hierarchical, dynamic, multimodal, multi-aspect, online/incremental, zero-shot.
    - **Technical terms:** guided topic modeling; (semi-)supervised; hierarchical; dynamic; multimodal; multi-aspect; online/incremental; zero-shot topic modeling.

47. **Quote:** "The modularity and algorithmic flexibility are the foundation of the author's aim to make BERTopic the one-stop-shop for topic modeling. You can find a full overview of its capabilities in the documentation or the repository."
    - **Plain English:** BERTopic aims to be the one-stop-shop for topic modeling.
    - **Word meanings:** one-stop-shop = everything in one place.
    - **Technical terms:** (none).

### Code Block: Training BERTopic
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

48. **Quote:** "Let us start by exploring the topics that were created. The get_topic_info() method is useful to get a quick description of the topics that we found:"
    - **Plain English:** get_topic_info() summarizes the found topics.
    - **Technical terms:** get_topic_info().

### Code Block: Exploring topic info
```python
topic_model.get_topic_info()
```
```
Topic Count Name  Representation
-1  14520  -1_the_of_and_to  [the, of, and, to, in, we, that, language, for...
0   2290   0_speech_asr_recognition_end  [speech, asr, recognition, end, acoustic, spea...
1   1403   1_medical_clinical_biomedical_patient  [medical, clinical, biomedical, patient, healt...
2   1156   2_sentiment_aspect_analysis_reviews  [sentiment, aspect, analysis, reviews, opinion...
3   986    3_translation_nmt_machine_neural  [translation, nmt, machine, neural, bleu, engl...
...
```
- **Explanation:** Shows each topic's ID, document count, and Name (four best keywords joined with "_"). Topic −1 = outliers (14,520 documents).
- **Fits the architecture:** The Name column quickly conveys each topic's content.

49. **Quote:** "Each of these topics is represented by several keywords, which are concatenated with a '_' in the Name column. This Name column allows us to quickly get a feeling of what the topic is about as it shows the four keywords that best represent it."
    - **Plain English:** Name = four best keywords joined by "_".
    - **Word meanings:** concatenated = joined together.
    - **Technical terms:** Name column.

50. **Quote (advice box):** "You might also have noticed that the very first topic is labeled -1. That topic contains all documents that could not be fitted within a topic and are considered outliers. This is a result of the clustering algorithm, HDBSCAN, which does not force all points to be clustered. To remove outliers, we could either use a non-outlier algorithm like k-means or use BERTopic's reduce_outliers() function to reassign the outliers to topics."
    - **Plain English:** Topic −1 = outliers (HDBSCAN doesn't force clustering). Remove via k-means or reduce_outliers().
    - **Technical terms:** outliers; reduce_outliers().

51. **Quote:** "We can inspect individual topics and explore which keywords best represent them with the get_topic function. For example, topic 0 contains the following keywords:"
    - **Plain English:** get_topic() shows a topic's best keywords.
    - **Technical terms:** get_topic().

### Code Block: Inspecting and searching topics
```python
topic_model.get_topic(0)
```
```
[('speech', 0.028177697715245358),
 ('asr', 0.018971184497453525),
 ('recognition', 0.013457745472471012),
 ('end', 0.00980445092749381),
 ('acoustic', 0.009452082794507863), ...]
```
- **Explanation:** Returns (keyword, weight) tuples for topic 0 — highest-weight words speech, asr, recognition.
- **Fits the architecture:** Higher weight = more representative of the topic.

```python
topic_model.find_topics("topic modeling")
([22, -1, 1, 47, 32],
 [0.95456535, 0.91173744, 0.9074769, 0.9067007, 0.90510106])
```
- **Explanation:** Searches topics by a search term, returning topic IDs and similarity scores — topic 22 at 0.95.
- **Fits the architecture:** find_topics enables searching for topics of interest.

```python
topic_model.topics_[titles.index("BERTopic: Neural topic modeling with a class-based TF-IDF procedure")]
22
```
- **Explanation:** BERTopic's own abstract is assigned to topic 22 — matching our find_topics result.
- **Fits the architecture:** Confirms topic 22 really is about topic modeling.

52. **Quote:** "For example, topic 0 contains the keywords 'speech,' 'asr,' and 'recognition.' Based on these keywords, it seems that the topic is about automatic speech recognition (ASR)."
    - **Plain English:** Topic 0 keywords → automatic speech recognition.
    - **Technical terms:** ASR.

53. **Quote:** "We can use the find_topics() function to search for specific topics based on a search term. Let's search for a topic about topic modeling: ... This returns that topic 22 has a relatively high similarity (0.95) with our search term. If we then inspect the topic, we can see that it is indeed a topic about topic modeling:"
    - **Plain English:** find_topics("topic modeling") → topic 22 at 0.95 similarity; inspecting confirms it.
    - **Technical terms:** find_topics(); similarity.

54. **Quote:** "These functionalities allow us to quickly find the topics that we are interested in."
    - **Plain English:** Search/inspection make topic exploration fast.
    - **Technical terms:** (none).

55. **Quote (advice box):** "The modularity of BERTopic gives you a lot of choices, which can be overwhelming. For that purpose, the author created a best practices guide that goes through common practices to speed up training, improve representations, and more."
    - **Plain English:** Author provides a best-practices guide for training speed and representation quality.
    - **Word meanings:** overwhelming = too much to handle.
    - **Technical terms:** best practices guide.

56. **Quote:** "To make exploration of the topics a bit easier, we can look back at our text clustering example. There, we created a static visualization to see the general structure of the created topic. With BERTopic, we can create an interactive variant that allows us to quickly explore which topics exist and which documents they contain. Doing so requires us to use the two-dimensional embeddings, reduced_embeddings, that we created with UMAP. Moreover, when we hover over documents, we will show the title instead of the abstract to quickly get an understanding of the documents in a topic:"
    - **Plain English:** BERTopic offers interactive visualization using the 2D UMAP embeddings; hover shows titles.
    - **Technical terms:** interactive visualization; reduced_embeddings.

### Code Block: Interactive topic visualization
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
- **Explanation:** Creates an interactive plot (hover shows titles) using the 2D reduced embeddings.
- **Fits the architecture:** Interactive exploration of topics and documents.

57. **Quote:** "As we can see in Figure 5-18, this interactive plot quickly gives us a sense of the created topics. You can zoom in to view individual documents or double-click a topic on the righthand side to only view it."
    - **Plain English:** Interactive plot: zoom into documents; double-click a topic to isolate it.
    - **Technical terms:** interactive plot.

58. **Quote:** "There is a wide range of visualization options in BERTopic. There are three that are worthwhile to explore to get an idea of the relationships between topics:"
    - **Plain English:** Three relationship visualizations worth exploring.
    - **Technical terms:** visualization options.

### Code Block: Three relationship visualizations
```python
# Visualize barchart with ranked keywords
topic_model.visualize_barchart()
# Visualize relationships between topics
topic_model.visualize_heatmap(n_clusters=30)
# Visualize the potential hierarchical structure of topics
topic_model.visualize_hierarchy()
```
- **Explanation:** visualize_barchart (ranked keywords), visualize_heatmap (30 cluster groups), visualize_hierarchy (hierarchical structure).
- **Fits the architecture:** Visualization functions let users explore topics and their relationships.

### Adding a Special Lego Block

59. **Quote:** "The pipeline in BERTopic that we have explored thus far, albeit fast and modular, has a disadvantage: it still represents a topic through a bag-of-words without taking into account semantic structures."
    - **Plain English:** Bag-of-words representation ignores semantic structure — the key disadvantage.
    - **Word meanings:** albeit = although.
    - **Technical terms:** bag-of-words; semantic structure.

60. **Quote:** "The solution is to leverage the strength of the bag-of-words representation, which is its speed to generate a meaningful representation. We can use this first meaningful representation and tweak it using more powerful but slower techniques, like embedding models. As shown in Figure 5-19, we can rerank the initial distribution of words to improve the resulting representation. Note that this idea of reranking an initial set of results is a main staple in neural search, a subject that we cover in Chapter 8."
    - **Plain English:** Use bag-of-words for speed, then rerank with slower, more powerful techniques (embedding models). Reranking is a staple of neural search (Ch 8).
    - **Word meanings:** staple = central/main element; tweak = adjust slightly.
    - **Technical terms:** reranking; neural search.

61. **Quote:** "As a result, we can design a new Lego block, as shown in Figure 5-20, that takes in this first topic representation and spits out an improved representation. In BERTopic, such reranker models are referred to as representation models."
    - **Plain English:** The new Lego block = representation model — a reranker that improves topic representations.
    - **Technical terms:** representation models; reranker.

62. **Quote:** "A major benefit of this approach is that the optimization of topic representations only needs to be done as many times as we have topics. For instance, if we have millions of documents and a hundred topics, the representation block only needs to be applied once for every topic instead of for every document."
    - **Plain English:** Representation blocks run once per topic, not per document — huge efficiency gain.
    - **Technical terms:** efficiency; per-topic optimization.

63. **Quote:** "As shown in Figure 5-21, a wide variety of representation blocks have been designed for BERTopic that allows you to fine-tune the representations. The representation block can even be stacked multiple times to fine-tune representations using different methodologies."
    - **Plain English:** Many representation blocks exist; they can be stacked.
    - **Technical terms:** stacking representation blocks.

### Code Block: Saving original representations
```python
# Save original representations
from copy import deepcopy
original_topics = deepcopy(topic_model.topic_representations_)
```
- **Explanation:** Deep-copies the original topic representations for later comparison.
- **Fits the architecture:** Enables comparing representations with and without representation models.

### Code Block: The topic_differences helper
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

### KeyBERTInspired

64. **Quote:** "The first representation block that we are going to explore is KeyBERTInspired. KeyBERTInspired is, as you might have guessed, a method inspired by the keyword extraction package, KeyBERT. KeyBERT extracts keywords from texts by comparing word and document embeddings through cosine similarity."
    - **Plain English:** KeyBERTInspired = inspired by KeyBERT, which extracts keywords via cosine similarity between word and document embeddings.
    - **Technical terms:** KeyBERT; KeyBERTInspired; cosine similarity; keyword extraction.

65. **Quote:** "BERTopic uses a similar approach. KeyBERTInspired uses c-TF-IDF to extract the most representative documents per topic by calculating the similarity between a document's c-TF-IDF values and those of the topic they correspond to. As shown in Figure 5-22, the average document embedding per topic is calculated and compared to the embeddings of candidate keywords to rerank the keywords."
    - **Plain English:** KeyBERTInspired: pick most representative docs per topic (c-TF-IDF similarity), average their embeddings, compare to candidate keyword embeddings, rerank keywords.
    - **Technical terms:** c-TF-IDF; representative documents; average document embedding; candidate keywords.

66. **Quote:** "Due to the modular nature of BERTopic, we can update our initial topic representations with KeyBERTInspired without needing to perform the dimensionality reduction and clustering steps:"
    - **Plain English:** update_topics() swaps representations without redoing reduction/clustering.
    - **Technical terms:** update_topics().

### Code Block: KeyBERTInspired representation
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
- **Explanation:** Reranks keywords by comparing average document embeddings per topic with candidate keyword embeddings. Results are easier to read, but abbreviations like "nmt" may be removed.
- **Fits the architecture:** Representation blocks update topics without redoing dimensionality reduction and clustering.

67. **Quote:** "The updated model shows that the topics are easier to read compared to the original model. It also demonstrates the downside of using embedding-based techniques. Words in the original model, like nmt (topic 3), which stands for neural machine translation, are removed as the model could not properly represent the entity. For domain experts, these abbreviations are highly informative."
    - **Plain English:** KeyBERTInspired makes topics more readable but drops informative abbreviations (e.g., "nmt") it can't represent well.
    - **Technical terms:** embedding-based techniques; abbreviation loss; domain expertise.

### Maximal Marginal Relevance

68. **Quote:** "With c-TF-IDF and the previously shown KeyBERTInspired techniques, we still have significant redundancy in the resulting topic representations. For instance, having both the words 'summaries' and 'summary' in a topic representation introduces redundancy as they are quite similar."
    - **Plain English:** Representations have redundancy (e.g., "summary" and "summaries" both present).
    - **Technical terms:** redundancy.

69. **Quote:** "We can use maximal marginal relevance (MMR) to diversify our topic representations. The algorithm attempts to find a set of keywords that are diverse from one another but still relate to the documents they are compared to. It does so by embedding a set of candidate keywords and iteratively calculating the next best keyword to add. Doing so requires setting a diversity parameter, which indicates how diverse keywords need to be."
    - **Plain English:** MMR finds diverse-but-relevant keywords by embedding candidates and iteratively picking the next best; a diversity parameter controls diversity.
    - **Technical terms:** maximal marginal relevance (MMR); diversity parameter.

70. **Quote:** "In BERTopic, we use MMR to go from a set of initial keywords, let's say 30, to a smaller but more diverse set of keywords, let's say 10. It filters out redundant words and only keeps words that contribute something new to the topic representation."
    - **Plain English:** MMR reduces ~30 keywords to ~10 diverse ones, removing redundancy.
    - **Technical terms:** keyword diversification.

### Code Block: MMR representation
```python
from bertopic.representation import MaximalMarginalRelevance
# Update our topic representations to MaximalMarginalRelevance
representation_model = MaximalMarginalRelevance(diversity=0.2)
topic_model.update_topics(abstracts, representation_model=representation_model)
# Show topic differences
topic_differences(topic_model, original_topics)
```
```
Topic Original                        Updated
0    speech | asr | recognition | end | acoustic   speech | asr | error | model | training
1    medical | clinical | biomedical | patient | he...  clinical | biomedical | patient | healthcare |...
2    sentiment | aspect | analysis | reviews | opinion  sentiment | analysis | reviews | absa | polarity
3    translation | nmt | machine | neural | bleu   translation | nmt | bleu | parallel | multilin...
4    summarization | summaries | summary | abstract...  summarization | document | extractive | rouge ...
```
- **Explanation:** Diversifies keywords — from ~30 initial keywords to ~10 more diverse ones (e.g., topic 4 keeps only one "summary"-like word).
- **Fits the architecture:** Reduces redundancy in topic representations.

71. **Quote:** "The resulting topics demonstrate more diversity in their representations. For instance, topic 4 only shows one 'summary'-like word and instead adds other words that might contribute more to the overall representation."
    - **Plain English:** MMR result: topic 4 keeps one "summary"-like word and adds new contributing words.
    - **Technical terms:** diversity.

72. **Quote (advice box):** "Both KeyBERTInspired and MMR are amazing techniques for improving the first set of topic representations. KeyBERTInspired especially tends to remove nearly all stop words since it focuses on the semantic relationships between words and documents."
    - **Plain English:** KeyBERTInspired removes nearly all stop words (focuses on semantics).
    - **Technical terms:** stop words; semantic relationships.

### The Text Generation Lego Block

73. **Quote:** "The representation block in BERTopic has been acting as a reranking block in our previous examples. However, as we already explored in the previous chapter, generative models have great potential for a wide variety of tasks."
    - **Plain English:** So far representation blocks rerank; generative models can do more.
    - **Technical terms:** generative models.

74. **Quote:** "We can use generative models in BERTopic quite efficiently by following a part of the reranking procedure. Instead of using a generative model to identify the topic of all documents, of which there can potentially be millions, we will use the model to generate a label for our topic. As illustrated in Figure 5-23, instead of generating or reranking keywords, we ask the model to generate a short label based on keywords that were previously generated and a small set of representative documents."
    - **Plain English:** Generative model generates one label per topic (not per document) from keywords + a few representative documents.
    - **Technical terms:** label generation; prompt engineering.

75. **Quote:** "There are two components to the illustrated prompt. First, the documents that are inserted using the [DOCUMENTS] tag are a small subset of documents, typically four, that best represent the topic. The documents with the highest cosine similarity of their c-TF-IDF values with those of the topic are selected. Second, the keywords that make up a topic are also passed to the prompt and referenced using the [KEYWORDS] tag. The keywords could be generated by c-TF-IDF or any of the other representations we discussed thus far."
    - **Plain English:** Prompt has two components: [DOCUMENTS] (typically ~4 best-representing docs by c-TF-IDF cosine similarity) and [KEYWORDS] (topic keywords).
    - **Technical terms:** [DOCUMENTS] tag; [KEYWORDS] tag; cosine similarity.

76. **Quote:** "As a result, we only need to use the generative model once for every topic, of which there could be potentially hundreds, instead of once for each document, of which there could potentially be millions. There are many generative models that we can choose from, both open source and proprietary. Let's start with a model that we have explored in the previous chapter, the Flan-T5 model."
    - **Plain English:** One generation per topic (hundreds) instead of per document (millions); start with Flan-T5.
    - **Word meanings:** proprietary = closed/owned by a company.
    - **Technical terms:** Flan-T5; open source; proprietary.

### Code Block: Flan-T5 text generation representation
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
```
Topic Original                        Updated
0    speech | asr | recognition | end | acoustic   Speech-to-description
1    medical | clinical | biomedical | patient | he...  Science/Tech
2    sentiment | aspect | analysis | reviews | opinion  Review
3    translation | nmt | machine | neural | bleu   Attention-based neural machine translation
4    summarization | summaries | summary | abstract...  Summarization
```
- **Explanation:** Generates a short label per topic using Flan-T5 with a prompt containing [DOCUMENTS] and [KEYWORDS]. Labels: topic 0 "Speech-to-description", topic 3 "Attention-based neural machine translation", etc.
- **Fits the architecture:** Generative models label each topic once (not each document), improving interpretability.

77. **Quote:** "Some of these labels, like 'Summarization' seem to be logical when comparing them to the original representations. Others, however, like 'Science/Tech,' seem quite broad and do not do the original topic justice. Let's explore instead how OpenAI's GPT-3.5 would perform considering the model is not only larger but expected to have more linguistic capabilities:"
    - **Plain English:** Some Flan-T5 labels good, some too broad ("Science/Tech"); try GPT-3.5 — larger, more linguistic capability.
    - **Word meanings:** do justice = represent fairly/well.
    - **Technical terms:** GPT-3.5; linguistic capabilities.

### Code Block: GPT-3.5 text generation representation
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
```
Topic Original                        Updated
0    speech | asr | recognition | end | acoustic   Leveraging External Data for Improving Low-Res...
1    medical | clinical | biomedical | patient | he...  Improved Representation Learning for Biomedica...
2    sentiment | aspect | analysis | reviews | opinion  Advancements in Aspect-Based Sentiment Analys...
3    translation | nmt | machine | neural | bleu   Neural Machine Translation Enhancements
4    summarization | summaries | summary | abstract...  Document Summarization Techniques
```
- **Explanation:** Uses GPT-3.5 (via OpenAI API) to generate topic labels in the format "topic: <short topic label>". Results are more informative (e.g., "Document Summarization Techniques").
- **Fits the architecture:** Closed-source generative models as representation blocks; BERTopic also supports local backends.

78. **Quote:** "The resulting labels are quite impressive! We are not even using GPT-4 and the resulting labels seem to be more informative than our previous example. Note that BERTopic is not confined to only using OpenAI's offering but has local backends as well."
    - **Plain English:** GPT-3.5 labels are impressive; BERTopic also supports local backends (not just OpenAI).
    - **Word meanings:** confined = limited/restricted.
    - **Technical terms:** local backends.

79. **Quote (advice box):** "Although it seems like we do not need the keywords anymore, they are still representative of the input documents. No model is perfect and it is generally advised to generate multiple topic representations. BERTopic allows for all topics to be represented by different representations. You could, for example, use KeyBERTInspired, MMR, and GPT-3.5 side by side to get different perspectives on the same topic."
    - **Plain English:** Keywords still matter; generate multiple representations (e.g., KeyBERTInspired + MMR + GPT-3.5 side by side) for different perspectives.
    - **Technical terms:** multiple topic representations.

80. **Quote:** "With these GPT-3.5 generated labels, we can create beautiful illustrations using the datamapplot package (Figure 5-24):"
    - **Plain English:** datamapplot visualizes topics with the generated labels.
    - **Technical terms:** datamapplot.

### Code Block: datamapplot visualization
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

### Summary

81. **Quote:** "In this chapter, we explored how LLMs, both generative and representative, can be used in the domain of unsupervised learning. Despite supervised methods like classification being prevalent in recent years, unsupervised approaches such as text clustering hold immense potential due to their ability to group texts based on semantic content without prior labeling."
    - **Plain English:** Chapter showed both generative and representation LLMs in unsupervised learning; clustering groups texts by meaning without labels.
    - **Word meanings:** prevalent = widespread; immense = very large.
    - **Technical terms:** unsupervised learning; text clustering.

82. **Quote:** "We covered a common pipeline for clustering textual documents that starts with converting input text into numerical representations, which we call embeddings. Then, dimensionality reduction is applied to these embeddings to simplify high-dimensional data for better clustering outcomes. Finally, a clustering algorithm on the dimensionality-reduced embeddings is applied to cluster the input text. Manually inspecting the clusters helped us understand which documents they contained and how to interpret these clusters."
    - **Plain English:** Pipeline recap: embed → reduce → cluster; manual inspection aids interpretation.
    - **Technical terms:** embeddings; dimensionality reduction; clustering algorithm.

83. **Quote:** "To transition away from this manual inspection, we explored how BERTopic extends this text clustering pipeline with a method for automatically representing the clusters. This methodology is often referred to as topic modeling, which attempts to uncover themes within large amounts of documents. BERTopic generates these topic representations through a bag-of-words approach enhanced with c-TF-IDF, which weighs words based on their cluster relevance and frequency across all clusters."
    - **Plain English:** BERTopic automates cluster representation via bag-of-words + c-TF-IDF weighting.
    - **Technical terms:** topic modeling; bag-of-words; c-TF-IDF.

84. **Quote:** "A major benefit of BERTopic is its modular nature. In BERTopic, you can choose any model in the pipeline, which allows for additional representations of topics that create multiple perspectives of the same topic. We explored maximal marginal relevance and KeyBERTInspired as methodologies to fine-tune the topic representations generated with c-TF-IDF. Additionally, we used the same generative LLMs as in the previous chapter (Flan-T5 and GPT-3.5) to further improve the interpretability of topics by generating highly interpretable labels."
    - **Plain English:** Modularity → multiple perspectives; fine-tuned with MMR/KeyBERTInspired and generative labels (Flan-T5, GPT-3.5).
    - **Technical terms:** modularity; KeyBERTInspired; MMR; interpretability.

85. **Quote:** "In the next chapter, we shift focus and explore a common method for improving the output of generative models, namely prompt engineering."
    - **Plain English:** Next chapter (6) = prompt engineering.
    - **Technical terms:** prompt engineering.
