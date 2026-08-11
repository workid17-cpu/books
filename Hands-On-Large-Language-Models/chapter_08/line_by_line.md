# 📘 Chapter 8 Line-by-Line: Semantic Search and Retrieval-Augmented Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 8
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "Search was one of the first language model applications to see broad industry adoption. Months after the release of the seminal 'BERT: Pre-training of deep bidirectional transformers for language understanding' (2018) paper, Google announced it was using it to power Google Search and that it represented 'one of the biggest leaps forward in the history of Search.' Not to be outdone, Microsoft Bing also stated that 'Starting from April of this year, we used large transformer models to deliver the largest quality improvements to our Bing customers in the past year.'"
   - **Plain English:** BERT (2018) was quickly adopted by Google Search and Bing to dramatically improve search quality.
   - **Word meanings:** seminal = highly influential; outdone = surpassed.
   - **Technical terms:** BERT; deep bidirectional transformers; language model.

2. **Quote:** "This is a clear testament to the power and usefulness of these models. Their addition instantly and dramatically improves some of the most mature, well-maintained systems that billions of people around the planet rely on. The ability they add is called semantic search, which enables searching by meaning, and not simply keyword matching."
   - **Plain English:** Semantic search (searching by meaning, not keywords) is the key capability LMs added to mature search systems.
   - **Word meanings:** testament = proof/evidence; mature = long-established.
   - **Technical terms:** semantic search; keyword matching.

3. **Quote:** "On a separate track, the fast adoption of text generation models led many users to ask the models questions and expect factual answers. And while the models were able to answer fluently and confidently, their answers were not always correct or up-to-date. This problem grew to be known as model 'hallucinations,' and one of the leading ways to reduce it is to build systems that can retrieve relevant information and provide it to the LLM to aid it in generating more factual answers. This method, called RAG, is one of the most popular applications of LLMs."
   - **Plain English:** LLMs hallucinate (fluent but wrong/outdated answers); RAG reduces this by retrieving relevant info to feed the model.
   - **Word meanings:** fluently = smoothly; up-to-date = current.
   - **Technical terms:** hallucinations; RAG (Retrieval-Augmented Generation); text generation models.

### Overview of Semantic Search and RAG

4. **Quote:** "There's a lot of research on how to best use language models for search. Three broad categories of these models are dense retrieval, reranking, and RAG."
   - **Plain English:** The chapter organizes LM search systems into three categories: dense retrieval, reranking, RAG.
   - **Technical terms:** dense retrieval; reranking; RAG.

### Dense retrieval

5. **Quote:** "Dense retrieval systems rely on the concept of embeddings, the same concept we've encountered in the previous chapters, and turn the search problem into retrieving the nearest neighbors of the search query (after both the query and the documents are converted into embeddings). Figure 8-1 shows how dense retrieval takes a search query, consults its archive of texts, and outputs a set of relevant results."
   - **Plain English:** Dense retrieval embeds query + documents and returns the nearest neighbors to the query.
   - **Technical terms:** embeddings; dense retrieval; nearest neighbors.

### Reranking

6. **Quote:** "Search systems are often pipelines of multiple steps. A reranking language model is one of these steps and is tasked with scoring the relevance of a subset of results against the query; the order of results is then changed based on these scores."
   - **Plain English:** Rerankers score a subset of results by relevance and reorder them.
   - **Technical terms:** reranking; relevance scoring; pipeline.

### RAG

7. **Quote:** "The growing LLM capability of text generation led to a new type of search systems that include a model that generates an answer in response to a query. ... Generative search is a subset of a broader type of category of systems better called RAG systems. These are text generation systems that incorporate search capabilities to reduce hallucinations, increase factuality, and/or ground the generation model on a specific dataset."
   - **Plain English:** RAG = generation + search; reduces hallucinations, improves factuality, grounds the model on a dataset.
   - **Word meanings:** incorporate = include.
   - **Technical terms:** generative search; RAG; grounding; factuality.

8. **Quote:** "The rest of the chapter covers these three types of systems in more detail. While these are the major categories, they are not the only LLM applications in the domain of search."
   - **Plain English:** The three categories are major but not exhaustive.
   - **Technical terms:** LLM applications.

### Semantic Search with Language Models

### Dense Retrieval

9. **Quote:** "Recall that embeddings turn text into numeric representations. Those can be thought of as points in space, as we can see in Figure 8-4. Points that are close together mean that the text they represent is similar. So in this example, text 1 and text 2 are more similar to each other (because they are near each other) than text 3 (because it's farther away)."
   - **Plain English:** Embeddings are points in space; nearby points represent similar texts.
   - **Word meanings:** recall = remember.
   - **Technical terms:** embeddings; numeric representations.

10. **Quote:** "This is the property that is used to build search systems. In this scenario, when a user enters a search query, we embed the query, thus projecting it into the same space as our text archive. Then we simply find the nearest documents to the query in that space, and those would be the search results."
    - **Plain English:** Embed the query into the archive's space; nearest documents = results.
    - **Technical terms:** embedding; search query; text archive; nearest neighbors.

11. **Quote:** "Judging by the distances in Figure 8-5, 'text 2' is the best result for this query, followed by 'text 1.' Two questions could arise here, however: Should text 3 even be returned as a result? That's a decision for you, the system designer. It's sometimes desirable to have a max threshold of similarity score to filter out irrelevant results (in case the corpus has no relevant results for the query). Are a query and its best result semantically similar? Not always. This is why language models need to be trained on question-answer pairs to become better at retrieval. This process is explained in more detail in Chapter 10."
    - **Plain English:** Design decisions: use a similarity threshold to filter irrelevant results; models may need training on Q-A pairs to retrieve well (Ch 10).
    - **Word meanings:** threshold = cutoff point; desirable = wanted.
    - **Technical terms:** similarity threshold; corpus; question-answer pairs; retrieval.

12. **Quote:** "Figure 8-6 shows how we chunk a document before proceeding to embed each chunk. Those embedding vectors are then stored in the vector database and are ready for retrieval."
    - **Plain English:** Documents are chunked, embedded per chunk, and stored in a vector database for retrieval.
    - **Technical terms:** chunking; embedding vectors; vector database.

### Dense retrieval example

13. **Quote:** "Let's take a look at a dense retrieval example by using Cohere to search the Wikipedia page for the film Interstellar. In this example, we will do the following: 1. Get the text we want to make searchable and apply some light processing to chunk it into sentences. 2. Embed the sentences. 3. Build the search index. 4. Search and see the results."
    - **Plain English:** Steps: chunk text into sentences → embed → build index → search.
    - **Technical terms:** chunk; embed; search index.

14. **Quote:** "Get your Cohere API key by signing up at https://oreil.ly/GxrQ1. Paste it in the following code. You will not have to pay anything to run through this example."
    - **Plain English:** Sign up for a (free) Cohere API key.
    - **Technical terms:** Cohere API key.

### Code Block 1: Imports and Cohere client setup
```python
import cohere
import numpy as np
import pandas as pd
from tqdm import tqdm
api_key = ''
co = cohere.Client(api_key)
```
- **Explanation:** Imports libraries and creates the Cohere client; user pastes their API key (warned not to share publicly).
- **Fits the architecture:** Cohere's managed APIs (embed, rerank, chat) drive the chapter's examples.

### Code Block 2: The Interstellar text archive (chunking)
```python
text = """Interstellar is a 2014 epic science fiction film ..."""
texts = text.split('.')
texts = [t.strip(' \n') for t in texts]
```
- **Explanation:** The first section of the Wikipedia article on Interstellar is split into cleaned sentences on periods.
- **Fits the architecture:** Light processing + sentence chunking for embedding.

### Code Block 3: Embedding the text chunks
```python
response = co.embed(texts=texts, input_type="search_document").embeddings
embeds = np.array(response)
print(embeds.shape)
```
- **Explanation:** Cohere embeds the sentences (`input_type="search_document"`); returns a (15, 4096) NumPy array.
- **Fits the architecture:** 15 sentence vectors of size 4,096.

15. **Quote:** "This outputs (15, 4096), which indicates that we have 15 vectors, each one of size 4,096."
    - **Plain English:** 15 vectors, each of dimension 4096.
    - **Technical terms:** vectors; dimensions.

### Code Block 4: Building the search index with FAISS
```python
import faiss
dim = embeds.shape[1]
index = faiss.IndexFlatL2(dim)
print(index.is_trained)
index.add(np.float32(embeds))
```
- **Explanation:** Creates a FAISS L2 (Euclidean) index of dimension 4096 and adds the float32 embeddings.
- **Fits the architecture:** Index is optimized for fast nearest-neighbor retrieval.

16. **Quote:** "An index stores the embeddings and is optimized to quickly retrieve the nearest neighbors even if we have a very large number of points."
    - **Plain English:** An index enables fast nearest-neighbor search over huge embedding collections.
    - **Technical terms:** index; nearest neighbors.

### Code Block 5: The search function
```python
def search(query, number_of_results=3):
  query_embed = co.embed(texts=[query], input_type="search_query").embeddings[0]
  distances, similar_item_ids = index.search(np.float32([query_embed]), number_of_results)
  texts_np = np.array(texts)
  results = pd.DataFrame(data={'texts': texts_np[similar_item_ids[0]], 'distance': distances[0]})
  print(f"Query:'{query}'\nNearest neighbors:")
  return results
```
- **Explanation:** Embeds the query (`input_type="search_query"`), searches for the k nearest neighbors, and returns a DataFrame of texts + distances.
- **Fits the architecture:** Query embedding is projected into the same space as the archive.

### Code Block 6: Searching with a query
```python
query = "how precise was the science"
results = search(query)
```
- **Explanation:** Top result is the sentence about scientific accuracy (distance 10757.38) — which answers the query perfectly without sharing its keywords.
- **Fits the architecture:** Demonstrates semantic search's advantage over keyword matching.

17. **Quote:** "The first result has the least distance, and so is the most similar to the query. Looking at it, it answers the question perfectly. Notice that this wouldn't have been possible if we were only doing keyword search because the top result did not include the same keywords in the query."
    - **Plain English:** Smallest distance = most similar = best answer; keyword search would have missed it.
    - **Technical terms:** distance; keyword search; dense retrieval.

18. **Quote:** "We can actually verify that by defining a keyword search function to compare the two. We'll use the BM25 algorithm, which is one of the leading lexical search methods."
    - **Plain English:** BM25 is used as a leading lexical (keyword) search comparison.
    - **Technical terms:** BM25; lexical search.

### Code Block 7: BM25 tokenizer and corpus
```python
from rank_bm25 import BM25Okapi
from sklearn.feature_extraction import _stop_words
import string
def bm25_tokenizer(text):
    tokenized_doc = []
    for token in text.lower().split():
        token = token.strip(string.punctuation)
        if len(token) > 0 and token not in _stop_words.ENGLISH_STOP_WORDS:
            tokenized_doc.append(token)
    return tokenized_doc
tokenized_corpus = []
for passage in tqdm(texts):
    tokenized_corpus.append(bm25_tokenizer(passage))
bm25 = BM25Okapi(tokenized_corpus)
```
- **Explanation:** Tokenizer lowercases, strips punctuation, and removes stop words; builds a BM25Okapi lexical index over the tokenized corpus.
- **Fits the architecture:** The keyword baseline to compare against dense retrieval.

### Code Block 8: The keyword search function
```python
def keyword_search(query, top_k=3, num_candidates=15):
    print("Input question:", query)
    bm25_scores = bm25.get_scores(bm25_tokenizer(query))
    top_n = np.argpartition(bm25_scores, -num_candidates)[-num_candidates:]
    bm25_hits = [{'corpus_id': idx, 'score': bm25_scores[idx]} for idx in top_n]
    bm25_hits = sorted(bm25_hits, key=lambda x: x['score'], reverse=True)
    print(f"Top-3 lexical search (BM25) hits")
    for hit in bm25_hits[0:top_k]:
        print("\t{:.3f}\t{}".format(hit['score'], texts[hit['corpus_id']].replace("\n", " ")))
```
- **Explanation:** Scores the query with BM25, takes top candidates, sorts descending, prints top-k.
- **Fits the architecture:** Its top hit (1.789) shares the word "science" but doesn't answer the question — lexical limitation.

19. **Quote:** "Note that the first result does not really answer the question despite it sharing the word 'science' with the query. In the next section, we'll see how adding a reranker can improve this search system."
    - **Plain English:** Keyword top hit is irrelevant despite keyword overlap; a reranker can fix this.
    - **Technical terms:** keyword search; reranker.

### Caveats of dense retrieval

20. **Quote:** "It's useful to be aware of some of the drawbacks of dense retrieval and how to address them. What happens, for example, if the texts don't contain the answer? We still get results and their distances."
    - **Plain English:** Dense retrieval returns results even when no answer exists in the corpus.
    - **Technical terms:** dense retrieval.

21. **Quote:** "In cases like this, one possible heuristic is to set a threshold level—a maximum distance for relevance, for example. A lot of search systems present the user with the best info they can get and leave it up to the user to decide if it's relevant or not. Tracking the information of whether the user clicked on a result (and were satisfied by it) can improve future versions of the search system."
    - **Plain English:** Mitigations: a max-distance threshold, or let users judge; track click/satisfaction to improve future versions.
    - **Word meanings:** heuristic = practical rule.
    - **Technical terms:** threshold; relevance; click data.

22. **Quote:** "Another caveat of dense retrieval is when a user wants to find an exact match for a specific phrase. That's a case that's perfect for keyword matching. That's one reason why hybrid search, which includes both semantic search and keyword search, is advised instead of relying solely on dense retrieval."
    - **Plain English:** Exact-phrase queries favor keyword search → use hybrid search (semantic + keyword).
    - **Word meanings:** caveat = warning/limitation.
    - **Technical terms:** exact match; hybrid search; keyword search.

23. **Quote:** "Dense retrieval systems also find it challenging to work properly in domains other than the ones that they were trained on. So, for example, if you train a retrieval model on internet and Wikipedia data, and then deploy it on legal texts (without having enough legal data as part of the training set), the model will not work as well in that legal domain."
    - **Plain English:** Retrieval models degrade on out-of-domain data (e.g., trained on internet/Wikipedia, deployed on legal texts).
    - **Technical terms:** domains; training set; out-of-domain generalization.

24. **Quote:** "The final thing we'd like to point out is that this is a case where each sentence contained a piece of information, and we showed queries that specifically ask for that information. What about questions whose answers span multiple sentences? This highlights one of the important design parameters of dense retrieval systems: what is the best way to chunk long texts? And why do we need to chunk them in the first place?"
    - **Plain English:** Questions spanning multiple sentences highlight chunking as a key design parameter.
    - **Technical terms:** chunking; design parameters.

### Chunking long texts

25. **Quote:** "One limitation of Transformer language models is that they are limited in context sizes, meaning we cannot feed them very long texts that go above the number of words or tokens that the model supports. So how do we embed long texts?"
    - **Plain English:** Transformers have context limits, so long texts can't be embedded whole.
    - **Technical terms:** context size; tokens; embedding.

26. **Quote:** "There are several possible ways, and two possible approaches shown in Figure 8-7 include indexing one vector per document and indexing multiple vectors per document."
    - **Plain English:** Two approaches: one vector per document or multiple vectors per document.
    - **Technical terms:** vector; document indexing.

27. **Quote:** "One vector per document. In this approach, we use a single vector to represent the whole document. The possibilities here include: Embedding only a representative part of the document and ignoring the rest of the text. This may mean embedding only the title, or only the beginning of the document. This is useful to get quickly started with building a demo but it leaves a lot of information unindexed and therefore unsearchable. As an approach, it may work better for documents where the beginning captures the main points of a document (think: Wikipedia article). But it's really not the best approach for a real system because a lot of information would be left out of the index and would be unsearchable."
    - **Plain English:** Embedding only the title/beginning is fast for demos but leaves most info unsearchable.
    - **Word meanings:** unindexed = not in the index.
    - **Technical terms:** one vector per document; indexing.

28. **Quote:** "Embedding the document in chunks, embedding those chunks, and then aggregating those chunks into a single vector. The usual method of aggregation here is to average those vectors. A downside of this approach is that it results in a highly compressed vector that loses a lot of the information in the document."
    - **Plain English:** Chunk + average vectors into one → highly compressed, lossy.
    - **Word meanings:** aggregating = combining; downside = disadvantage.
    - **Technical terms:** aggregation; averaging; compressed vector.

29. **Quote:** "This approach can satisfy some information needs, but not others. A lot of the time, a search is for a specific piece of information contained in an article, which is better captured if the concept had its own vector."
    - **Plain English:** A single vector loses the specific-info granularity; separate vectors per concept are better.
    - **Technical terms:** vector; granularity.

30. **Quote:** "Multiple vectors per document. In this approach, we chunk the document into smaller pieces, and embed those chunks. Our search index then becomes that of chunk embeddings, not entire document embeddings."
    - **Plain English:** Chunk + embed each chunk; the index stores chunk embeddings.
    - **Technical terms:** chunk embeddings; multiple vectors per document.

31. **Quote:** "The chunking approach is better because it has full coverage of the text and because the vectors tend to capture individual concepts inside the text. This leads to a more expressive search index."
    - **Plain English:** Multi-vector chunking gives full text coverage and concept-level vectors → more expressive index.
    - **Word meanings:** expressive = capable of capturing more meaning.
    - **Technical terms:** coverage; expressive search index.

32. **Quote:** "The best way of chunking a long text will depend on the types of texts and queries your system anticipates. Approaches include: Each sentence is a chunk. The issue here is this could be too granular and the vectors don't capture enough of the context. Each paragraph is a chunk. This is great if the text is made up of short paragraphs. Otherwise, it may be that every 3–8 sentences is a chunk. Some chunks derive a lot of their meaning from the text around them. So we can incorporate some context via: Adding the title of the document to the chunk. Adding some of the text before and after them to the chunk. This way, the chunks can overlap so they include some surrounding text that also appears in adjacent chunks."
    - **Plain English:** Chunking options: sentence (too granular), paragraph (or every 3–8 sentences), plus context tricks (add title, overlap surrounding text).
    - **Word meanings:** granular = fine-grained; adjacent = neighboring.
    - **Technical terms:** chunking; overlapping chunks; context.

33. **Quote:** "Expect more chunking strategies to arise as the field develops—some of which may even use LLMs to dynamically split a text into meaningful chunks."
    - **Plain English:** Future chunking may use LLMs to split text dynamically.
    - **Technical terms:** LLM-based chunking.

### Nearest neighbor search versus vector databases

34. **Quote:** "Once the query is embedded, we need to find the nearest vectors to it from our text archive. The most straightforward way to find the nearest neighbors is to calculate the distances between the query and the archive. That can easily be done with NumPy and is a reasonable approach if you have thousands or tens of thousands of vectors in your archive."
    - **Plain English:** For small archives, compute distances directly with NumPy.
    - **Technical terms:** nearest neighbors; NumPy; distances.

35. **Quote:** "As you scale beyond to the millions of vectors, an optimized approach for retrieval is to rely on approximate nearest neighbor search libraries like Annoy or FAISS. These allow you to retrieve results from massive indexes in milliseconds and some of them can improve their performance by utilizing GPUs and scaling to clusters of machines to serve very large indices."
    - **Plain English:** For millions of vectors, use ANN libraries (Annoy/FAISS) with GPU/cluster scaling — milliseconds retrieval.
    - **Technical terms:** approximate nearest neighbor; Annoy; FAISS; GPU; clusters.

36. **Quote:** "Another class of vector retrieval systems are vector databases like Weaviate or Pinecone. A vector database allows you to add or delete vectors without having to rebuild the index. They also provide ways to filter your search or customize it in ways beyond merely vector distances."
    - **Plain English:** Vector databases (Weaviate, Pinecone) support dynamic add/delete and filtering beyond distances.
    - **Technical terms:** vector databases; Weaviate; Pinecone; filtering.

### Fine-tuning embedding models for dense retrieval

37. **Quote:** "Just as we discussed in Chapter 4 on text classification, we can improve the performance of an LLM on a task using fine-tuning. As in that case, retrieval needs to optimize text embeddings and not simply token embeddings. The process for this fine-tuning is to get training data composed of queries and relevant results."
    - **Plain English:** Fine-tune to optimize text embeddings using query/result training data.
    - **Technical terms:** fine-tuning; text embeddings; token embeddings.

38. **Quote:** "Let's look at one example from our dataset, the sentence 'Interstellar premiered on October 26, 2014, in Los Angeles.' Two possible queries where this is a relevant result are: Relevant query 1: 'Interstellar release date'; Relevant query 2: 'When did Interstellar premier'. The fine-tuning process aims to make the embeddings of these queries close to the embedding of the resulting sentence. It also needs to see negative examples of queries that are not relevant to the sentence, for example: Irrelevant query: 'Interstellar cast'."
    - **Plain English:** Positive pairs (release date / premier queries ↔ premier sentence) and a negative pair ('Interstellar cast') guide fine-tuning.
    - **Technical terms:** positive examples; negative examples; query-document pairs.

39. **Quote:** "With these examples, we now have three pairs—two positive pairs and one negative pair. Let's assume, as we can see in Figure 8-12, that before fine-tuning, all three queries have the same distance from the result document. That's not far-fetched because they all talk about Interstellar. The fine-tuning step works to make the relevant queries closer to the document and at the same time make irrelevant queries farther from the document."
    - **Plain English:** Fine-tuning pulls relevant query embeddings closer and pushes irrelevant ones farther.
    - **Word meanings:** far-fetched = unlikely.
    - **Technical terms:** fine-tuning; embeddings; distance.

### Reranking

40. **Quote:** "A lot of organizations have already built search systems. For those organizations, an easier way to incorporate language models is as a final step inside their search pipeline. This step is tasked with changing the order of the search results based on relevance to the search query. This one step can vastly improve search results and it's in fact what Microsoft Bing added to achieve the improvements to search results using BERT-like models."
    - **Plain English:** Reranking is an easy final step in existing pipelines; Bing used it with BERT-like models.
    - **Word meanings:** vastly = greatly.
    - **Technical terms:** reranking; search pipeline; BERT-like models.

### Reranking example

41. **Quote:** "A reranker takes in the search query and a number of search results, and returns the optimal ordering of these documents so the most relevant ones to the query are higher in ranking. Cohere's Rerank endpoint is a simple way to start using a first reranker. We simply pass it the query and texts and get the results back. We don't need to train or tune it."
    - **Plain English:** Cohere's Rerank endpoint reorders documents by relevance with no training/tuning.
    - **Technical terms:** reranker; optimal ordering; relevance.

### Code Block 9: Cohere Rerank (direct)
```python
query = "how precise was the science"
results = co.rerank(query=query, documents=texts, top_n=3, return_documents=True)
for idx, result in enumerate(results.results):
  print(idx, result.relevance_score, result.document.text)
```
- **Explanation:** Passes query + all documents to Rerank; top 3 with relevance scores 0.1698185 / 0.07004896 / 0.0043994132.
- **Fits the architecture:** Reranker is much more confident about the top result.

42. **Quote:** "This shows the reranker is much more confident about the first result, assigning it a relevance score of 0.16, while the other results are scored much lower in relevance."
    - **Plain English:** First result scored ~0.16, far above the others.
    - **Technical terms:** relevance score.

43. **Quote:** "In this basic example, we passed our reranker all 15 of our documents. More often, however, our index would have thousands or millions of entries, and we need to shortlist, say one hundred or one thousand results and then present those to the reranker. This shortlisting step is called the first stage of the search pipeline."
    - **Plain English:** Real pipelines shortlist 100–1000 results first (first stage), then rerank.
    - **Word meanings:** shortlist = reduce to a candidate set.
    - **Technical terms:** first stage; shortlisting; search pipeline.

44. **Quote:** "The first-stage retriever can be keyword search, dense retrieval, or better yet—hybrid search that uses both of them. We can revisit our previous example to see how adding a reranker after a keyword search system improves its performance."
    - **Plain English:** First stage can be keyword, dense, or hybrid; rerank after keyword improves it.
    - **Technical terms:** first-stage retriever; hybrid search; reranker.

### Code Block 10: Keyword search + reranking function
```python
def keyword_and_reranking_search(query, top_k=3, num_candidates=10):
    print("Input question:", query)
    bm25_scores = bm25.get_scores(bm25_tokenizer(query))
    top_n = np.argpartition(bm25_scores, -num_candidates)[-num_candidates:]
    bm25_hits = [{'corpus_id': idx, 'score': bm25_scores[idx]} for idx in top_n]
    bm25_hits = sorted(bm25_hits, key=lambda x: x['score'], reverse=True)
    print(f"Top-3 lexical search (BM25) hits")
    for hit in bm25_hits[0:top_k]:
        print("\t{:.3f}\t{}".format(hit['score'], texts[hit['corpus_id']].replace("\n", " ")))
    docs = [texts[hit['corpus_id']] for hit in bm25_hits]
    print(f"\nTop-3 hits by rank-API ({len(bm25_hits)} BM25 hits re-ranked)")
    results = co.rerank(query=query, documents=docs, top_n=top_k, return_documents=True)
    for hit in results.results:
        print("\t{:.3f}\t{}".format(hit.relevance_score, hit.document.text.replace("\n", " ")))
```
- **Explanation:** BM25 shortlists top 10, then Cohere Rerank re-ranks them to top 3 — elevating the most relevant result (the Kip Thorne sentence).
- **Fits the architecture:** The two-stage pipeline: cheap retriever + relevance reranker.

45. **Quote:** "We see that keyword search assigns scores to only two results that share some of the keywords. In the second set of results, the reranker elevates the second result appropriately as the most relevant result for the query. This is a toy example that gives us a glimpse of the effect, but in practice, such a pipeline significantly improves search quality. On a multilingual benchmark like MIRACL, a reranker can boost performance from 36.5 to 62.8, measured as nDCG@10 (more on evaluation later in this chapter)."
    - **Plain English:** Reranking fixes keyword-search ordering; on MIRACL it boosts nDCG@10 from 36.5 to 62.8.
    - **Word meanings:** toy example = simple illustration.
    - **Technical terms:** MIRACL; nDCG@10; reranker.

46. **Quote:** "If you want to locally set up retrieval and reranking on your own machine, then you can use the Sentence Transformers library. Refer to the documentation at https://oreil.ly/jJOhV for setup. Check the 'Retrieve & Re-Rank' section for instructions and code examples."
    - **Plain English:** Sentence Transformers library enables local retrieval + reranking.
    - **Technical terms:** Sentence Transformers; retrieve & re-rank.

### How reranking models work

47. **Quote:** "One popular way of building LLM search rerankers is to present the query and each result to an LLM working as a cross-encoder. This means that a query and possible result are presented to the model at the same time allowing the model to view both these texts before it assigns a relevance score. All of the documents are processed simultaneously as a batch yet each document is evaluated against the query independently. The scores then determine the new order of the results. This method is described in more detail in a paper titled 'Multi-stage document ranking with BERT' and is sometimes referred to as monoBERT."
    - **Plain English:** Cross-encoders (monoBERT) see query + document together to assign relevance; documents processed as a batch but scored independently.
    - **Technical terms:** cross-encoder; monoBERT; relevance score; batch.

48. **Quote:** "This formulation of search as relevance scoring basically boils down to being a classification problem. Given those inputs, the model outputs a score from 0–1 where 0 is irrelevant and 1 is highly relevant. This should be familiar from our classification discussions in Chapter 4."
    - **Plain English:** Reranking is a classification problem with a 0–1 relevance score.
    - **Word meanings:** boils down to = reduces to.
    - **Technical terms:** classification; relevance score.

49. **Quote:** "To learn more about the development of using LLMs for search, 'Pretrained transformers for text ranking: BERT and beyond' is a highly recommended look at the developments of these models until about 2021."
    - **Plain English:** Recommended reading on pretrained transformers for text ranking.
    - **Technical terms:** pretrained transformers; text ranking.

### Retrieval Evaluation Metrics

50. **Quote:** "Semantic search is evaluated using metrics from the Information Retrieval (IR) field. Let's discuss one of these popular metrics: mean average precision (MAP)."
    - **Plain English:** Search is evaluated with IR metrics; MAP is a popular one.
    - **Technical terms:** Information Retrieval (IR); mean average precision (MAP).

51. **Quote:** "Evaluating search systems needs three major components: a text archive, a set of queries, and relevance judgments indicating which documents are relevant for each query."
    - **Plain English:** Evaluation needs: archive, query set, relevance judgments.
    - **Technical terms:** text archive; queries; relevance judgments.

52. **Quote:** "Using this test suite, we can proceed to explore evaluating search systems. Let's start with a simple example. Let's assume we pass query 1 to two different search systems. And get two sets of results. Say we limit the number of results to three."
    - **Plain English:** Compare two systems by passing the same query and looking at top results.
    - **Technical terms:** test suite; search systems.

53. **Quote:** "This shows us a clear case where system 1 is better than system 2. Intuitively, we may just count how many relevant results each system retrieved. System 1 got two out of three correct, and system 2 got only one out of three correct. But what about a case like Figure 8-19 where both systems only get one relevant result out of three, but they're in different positions?"
    - **Plain English:** Counting relevant hits isn't enough — position of relevant results matters.
    - **Technical terms:** relevant results; position.

54. **Quote:** "In this case, we can intuit that system 1 did a better job than system 2 because the result in the first position (the most important position) is correct. But how can we assign a number or score to how much better that result is? Mean average precision is a measure that is able to quantify this distinction."
    - **Plain English:** MAP quantifies how much better a top-position relevant result is.
    - **Word meanings:** intuit = sense; quantify = measure numerically.
    - **Technical terms:** mean average precision.

55. **Quote:** "One common way to assign numeric scores in this scenario is average precision, which evaluates system 1's result for the query to be 1 and system 2's to be 0.3. So let's see how average precision is calculated to evaluate one set of results, and then how it's aggregated to evaluate a system across all the queries in the test suite."
    - **Plain English:** Average precision gives 1 (top position) vs 0.3 (lower position); then averaged across queries.
    - **Technical terms:** average precision; aggregation.

### Scoring a single query with average precision

56. **Quote:** "To score a search system on this query, we can focus on scoring the relevant documents. Let's start by looking at a query that only has one relevant document in the test suite. The first one is easy: the search system placed the relevant result (the only available one for this query) at the top. This gets the system the perfect score of 1. Figure 8-20 shows this calculation: looking at the first position, we have a relevant result leading to a precision at position 1 of 1.0 (calculated as the number of relevant results at position 1, divided by the position we're currently looking at)."
    - **Plain English:** Relevant doc at position 1 → precision@1 = 1/1 = 1.0 (perfect score).
    - **Technical terms:** precision at position 1; relevant result.

57. **Quote:** "Since we're only scoring relevant documents we can ignore the scores of nonrelevant documents and stop our calculation here. What if the system actually placed the only relevant result at the third position, however? How would that affect the score? Figure 8-21 shows how that results in a penalty."
    - **Plain English:** Only relevant documents are scored; placing the relevant doc lower results in a penalty.
    - **Word meanings:** penalty = score reduction.
    - **Technical terms:** precision; relevant documents.

58. **Quote:** "Let's now look at a query with more than one relevant document. Figure 8-22 shows that calculation and how averaging now comes into the picture."
    - **Plain English:** With multiple relevant docs, average the precision values at each relevant position.
    - **Technical terms:** average precision; averaging.

### Scoring across multiple queries with mean average precision

59. **Quote:** "Now that we're familiar with precision at k and average precision, we can extend this knowledge to a metric that can score a search system against all the queries in our test suite. That metric is called mean average precision. Figure 8-23 shows how to calculate this metric by taking the mean of the average precisions of each query."
    - **Plain English:** MAP = mean of each query's average precision across the test suite.
    - **Technical terms:** precision at k; average precision; mean average precision.

60. **Quote:** "You may be wondering why the same operation is called 'mean' and 'average.' It's likely an aesthetic choice because MAP sounds better than average average precision."
    - **Plain English:** "Mean" and "average" are the same operation — MAP just sounds better.
    - **Word meanings:** aesthetic = stylistic.
    - **Technical terms:** mean; average; MAP.

61. **Quote:** "Now we have a single metric that we can use to compare different systems. If you want to learn more about evaluation metrics, see the 'Evaluation in Information Retrieval' chapter of Introduction to Information Retrieval (Cambridge University Press) by Christopher D. Manning, Prabhakar Raghavan, and Hinrich Schütze."
    - **Plain English:** MAP gives a single comparison number; reference: Introduction to Information Retrieval.
    - **Technical terms:** evaluation metrics; Information Retrieval.

62. **Quote:** "In addition to mean average precision, another metric commonly used for search systems is normalized discounted cumulative gain (nDCG), which is more nuanced in that the relevance of documents is not binary (relevant versus not relevant) and one document can be labeled as more relevant than another in the test suite and scoring mechanism."
    - **Plain English:** nDCG handles graded (non-binary) relevance — more nuanced than MAP.
    - **Word meanings:** nuanced = subtle/detailed.
    - **Technical terms:** nDCG; relevance labels.

### Retrieval-Augmented Generation (RAG)

63. **Quote:** "The mass adoption of LLMs quickly led to people asking them questions and expecting factual answers. While the models can answer some questions correctly, they also confidently answer lots of questions incorrectly. The leading method the industry turned to remedy this behavior is RAG, described in the paper 'Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks' (2020) and illustrated in Figure 8-24."
    - **Plain English:** RAG (2020 paper) is the leading remedy for confident-but-wrong LLM answers.
    - **Word meanings:** remedy = fix.
    - **Technical terms:** RAG; factual answers.

64. **Quote:** "RAG systems incorporate search capabilities in addition to generation capabilities. They can be seen as an improvement to generation systems because they reduce their hallucinations and improve their factuality. They also enable use cases of 'chat with my data' that consumers and companies can use to ground an LLM on internal company data, or a specific data source of interest (e.g., chatting with a book)."
    - **Plain English:** RAG reduces hallucinations, improves factuality, and enables "chat with my data."
    - **Word meanings:** incorporate = include.
    - **Technical terms:** RAG; hallucinations; factuality; grounding.

65. **Quote:** "This also extends to search systems. More search engines are incorporating an LLM to summarize results or answer questions submitted to the search engine. Examples include Perplexity, Microsoft Bing AI, and Google Gemini."
    - **Plain English:** Search engines now use LLMs to summarize/answer; examples: Perplexity, Bing AI, Gemini.
    - **Technical terms:** generative search; Perplexity; Bing AI; Gemini.

### From Search to RAG

66. **Quote:** "Let's now turn our search system into a RAG system. We do that by adding an LLM to the end of the search pipeline. We present the question and the top retrieved documents to the LLM, and ask it to answer the question given the context provided by the search results."
    - **Plain English:** RAG = search pipeline + an LLM at the end that answers using retrieved context.
    - **Technical terms:** RAG; search pipeline; retrieved documents.

67. **Quote:** "This generation step is called grounded generation because the retrieved relevant information we provide the LLM establishes a certain context that grounds the LLM in the domain we're interested in."
    - **Plain English:** Grounded generation: retrieved context grounds the LLM in the domain.
    - **Technical terms:** grounded generation; grounding; context.

### Example: Grounded Generation with an LLM API

68. **Quote:** "Let's look at how to add a grounded generation step after the search results to create our first RAG system. For this example, we'll use Cohere's managed LLM, which builds on the search systems we've seen earlier in the chapter. We'll use embedding search to retrieve the top documents, then we'll pass those to the co.chat endpoint along with the questions to provide a grounded answer."
    - **Plain English:** Use Cohere's chat API with retrieved documents for grounded answers.
    - **Technical terms:** managed LLM; co.chat; grounded generation.

### Code Block 11: Grounded generation with Cohere chat
```python
query = "income generated"
results = search(query)
docs_dict = [{'text': text} for text in results['texts']]
response = co.chat(message=query, documents=docs_dict)
print(response.text)
```
- **Explanation:** Retrieves top docs via embedding search, passes them as `documents` to `co.chat`, and prints the grounded answer.
- **Fits the architecture:** A complete RAG loop — retrieval then grounded generation.

69. **Quote:** "Result: The film generated a worldwide gross of over $677 million, or $773 million with subsequent re-releases. We are highlighting some of the text because the model indicated the source for these spans of text to be the first document we passed in: citations=[ChatCitation(start=21, end=36, text='worldwide gross', document_ids=['doc_0']), ChatCitation(start=40, end=57, text='over $677 million', document_ids=['doc_0']), ChatCitation(start=62, end=103, text='$773 million with subsequent re-releases.', document_ids=['doc_0'])]"
    - **Plain English:** The managed model returns a grounded answer with span-level citations pointing to the source document.
    - **Technical terms:** citations; ChatCitation; document_ids.

### Example: RAG with Local Models

70. **Quote:** "Let us now replicate this basic functionality with local models. We will lose the ability to do span citations and the smaller local model isn't going to work as well as the larger managed model, but it's useful to demonstrate the flow."
    - **Plain English:** Local models replicate RAG but lose span citations and quality.
    - **Technical terms:** local models; span citations.

### Code Block 12: Downloading and loading the local generation model
```python
!wget https://huggingface.co/microsoft/Phi-3-mini-4k-instruct-gguf/resolve/main/Phi-3-mini-4k-instruct-fp16.gguf
from langchain import LlamaCpp
llm = LlamaCpp(
    model_path="Phi-3-mini-4k-instruct-fp16.gguf",
    n_gpu_layers=-1,
    max_tokens=500,
    n_ctx=2048,
    seed=42,
    verbose=False
)
```
- **Explanation:** Downloads the quantized Phi-3 GGUF and loads it with LlamaCpp (same params as Ch 7).
- **Fits the architecture:** Local text-generation model for the RAG generation step.

71. **Quote:** "Loading the embedding model. Let's now load an embedding language model. In this example, we will choose the BAAI/bge-small-en-v1.5 model. At the time of writing, it is high on the MTEB leaderboard for embedding models and relatively small."
    - **Plain English:** bge-small-en-v1.5 is chosen for embeddings — high on MTEB, relatively small.
    - **Technical terms:** embedding model; MTEB leaderboard.

### Code Block 13: Loading the embedding model
```python
from langchain.embeddings.huggingface import HuggingFaceEmbeddings
embedding_model = HuggingFaceEmbeddings(model_name='thenlper/gte-small')
```
- **Explanation:** Loads a Sentence-Transformers embedding model (gte-small) via HuggingFaceEmbeddings.
- **Fits the architecture:** Converts text into numerical representations for the vector database.

### Code Block 14: Building the local vector database
```python
from langchain.vectorstores import FAISS
db = FAISS.from_texts(texts, embedding_model)
```
- **Explanation:** Builds a local FAISS vector store from the texts using the embedding model.
- **Fits the architecture:** The retrieval index for local RAG.

### Code Block 15: The RAG prompt template
```python
from langchain import PromptTemplate
template = """<|user|>
Relevant information:
{context}
Provide a concise answer the following question using the relevant information 
provided above:
{question}<|end|>
<|assistant|>"""
prompt = PromptTemplate(template=template, input_variables=["context", "question"])
```
- **Explanation:** Adds a `{context}` variable to communicate the retrieved documents, plus the `{question}`.
- **Fits the architecture:** The prompt is the central place that grounds the LLM.

72. **Quote:** "A prompt template plays a vital part in the RAG pipeline. It is the central place where we communicate the relevant documents to the LLM. To do so, we will create an additional input variable named context that can provide the LLM with the retrieved documents."
    - **Plain English:** The `context` variable is how retrieved documents reach the LLM.
    - **Technical terms:** prompt template; context variable.

### Code Block 16: Building the RAG pipeline
```python
from langchain.chains import RetrievalQA
rag = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type='stuff',
    retriever=db.as_retriever(),
    chain_type_kwargs={"prompt": prompt},
    verbose=True
)
```
- **Explanation:** Builds a RetrievalQA chain: retrieves from the FAISS store and 'stuffs' the context into the prompt for the LLM.
- **Fits the architecture:** Combines retriever + grounded generation into one RAG pipeline.

### Code Block 17: Querying the local RAG pipeline
```python
rag.invoke('Income generated')
```
- **Explanation:** Retrieves and generates a grounded answer (~$677 million; ~$773 million with re-releases; tenth-highest grossing film of 2014).
- **Fits the architecture:** Local RAG works, though without span citations and with a smaller, weaker model.

73. **Quote:** "As always, we can adjust the prompt to control the model's generation (e.g., answer length and tone)."
    - **Plain English:** The prompt controls generation length and tone.
    - **Technical terms:** prompt engineering; generation.

### Advanced RAG Techniques

### Query rewriting

74. **Quote:** "If the RAG system is a chatbot, the preceding simple RAG implementation would likely struggle with the search step if a question is too verbose, or to refer to context in previous messages in the conversation. This is why it's a good idea to use an LLM to rewrite the query into one that aids the retrieval step in getting the right information."
    - **Plain English:** LLM rewrites verbose/conversational questions into concise retrieval-friendly queries.
    - **Word meanings:** verbose = wordy.
    - **Technical terms:** query rewriting; retrieval.

75. **Quote:** "This rewriting behavior can be done through a prompt (or through an API call). Cohere's API, for example, has a dedicated query-rewriting mode for co.chat."
    - **Plain English:** Query rewriting via prompt or API (Cohere has a dedicated mode).
    - **Technical terms:** query rewriting; co.chat.

### Multi-query RAG

76. **Quote:** "The next improvement we can introduce is to extend the query rewriting to be able to search multiple queries if more than one is needed to answer a specific question. Take for example: User Question: 'Compare the financial results of Nvidia in 2020 vs. 2023'... We're better off making two search queries: Query 1: 'Nvidia 2020 financial results'; Query 2: 'Nvidia 2023 financial results'. We then present the top results of both queries to the model for grounded generation."
    - **Plain English:** Multi-query RAG splits a question into multiple search queries and merges their top results.
    - **Technical terms:** multi-query RAG; grounded generation.

77. **Quote:** "An additional small improvement here is to also give the query rewriter the option to determine if no search is required and if it can directly generate a confident answer without searching."
    - **Plain English:** The rewriter can decide no search is needed and answer directly.
    - **Technical terms:** query rewriter; direct generation.

### Multi-hop RAG

78. **Quote:** "A more advanced question may require a series of sequential queries. Take for example a question like: User Question: 'Who are the largest car manufacturers in 2023? Do they each make EVs or not?' To answer this, the system must first search for: Step 1, Query 1: 'largest car manufacturers 2023'. Then after it gets this information (the result being Toyota, Volkswagen, and Hyundai), it should ask follow-up questions: Step 2, Query 1: 'Toyota Motor Corporation electric vehicles'; Step 2, Query 2: 'Volkswagen AG electric vehicles'; Step 2, Query 3: 'Hyundai Motor Company electric vehicles'."
    - **Plain English:** Multi-hop RAG chains sequential queries where each step's answer drives the next.
    - **Word meanings:** sequential = ordered.
    - **Technical terms:** multi-hop RAG; sequential queries.

### Query routing

79. **Quote:** "An additional enhancement is to give the model the ability to search multiple data sources. We can, for example, specify for the model that if it gets a question about HR, it should search the company's HR information system (e.g., Notion) but if the question is about customer data, that it should search the customer relationship management (CRM) (e.g., Salesforce)."
    - **Plain English:** Query routing sends questions to the right data source (HR → Notion, customer data → CRM/Salesforce).
    - **Technical terms:** query routing; data sources; CRM.

### Agentic RAG

80. **Quote:** "You may be able to now see that the list of previous enhancements slowly delegates more and more responsibility to the LLM to solve more and more complex problems. This relies on the LLM's capability to gauge the required information needs as well as its ability to utilize multiple data sources. This new nature of the LLM starts to become closer and closer to an agent that acts on the world. The data sources can also now be abstracted into tools. We saw, for example, that we can search Notion, but by the same token, we should be able to post to Notion as well."
    - **Plain English:** Agentic RAG delegates more to the LLM — gauging needs, using multiple sources, with data sources as tools (search AND post to Notion).
    - **Word meanings:** delegates = hands over; gauges = estimates.
    - **Technical terms:** agentic RAG; tools; agents.

81. **Quote:** "Not all LLMs will have the RAG capabilities mentioned here. At the time of writing, likely only the largest managed models may be able to attempt this behavior. Thankfully, Cohere's Command R+ excels at these tasks and is available as an open-weights model as well."
    - **Plain English:** Only large managed models attempt agentic RAG; Command R+ excels and is open-weights.
    - **Word meanings:** excels = performs exceptionally.
    - **Technical terms:** managed models; open-weights model; Command R+.

### RAG Evaluation

82. **Quote:** "There are still ongoing developments in how to evaluate RAG models. A good paper to read on this topic is 'Evaluating verifiability in generative search engines' (2023), which runs human evaluations on different generative search systems. It evaluates results along four axes: Fluency — Whether the generated text is fluent and cohesive. Perceived utility — Whether the generated answer is helpful and informative. Citation recall — The proportion of generated statements about the external world that are fully supported by their citations. Citation precision — The proportion of generated citations that support their associated statements."
    - **Plain English:** RAG evaluation axes: fluency, perceived utility, citation recall, citation precision (Liu, Zhang, Liang 2023).
    - **Word meanings:** verifiability = checkability; cohesive = well-connected.
    - **Technical terms:** fluency; perceived utility; citation recall; citation precision.

83. **Quote:** "While human evaluation is always preferred, there are approaches that attempt to automate these evaluations by having a capable LLM act as a judge (called LLM-as-a-judge) and score the different generations along the different axes. Ragas is a software library that does exactly this. It also scores some additional useful metrics like: Faithfulness — Whether the answer is consistent with the provided context; Answer relevance — How relevant the answer is to the question."
    - **Plain English:** LLM-as-a-judge automates evaluation; Ragas library adds faithfulness and answer relevance.
    - **Technical terms:** LLM-as-a-judge; Ragas; faithfulness; answer relevance.

84. **Quote:** "The Ragas documentation site provides more details about the formulas to actually calculate these metrics."
    - **Plain English:** Ragas docs describe the metric formulas.
    - **Technical terms:** Ragas; metrics formulas.

### Summary

85. **Quote:** "In this chapter, we looked at different ways of using language models to improve existing search systems and even be the core of new, more powerful search systems. These include: Dense retrieval, which relies on the similarity of text embeddings. These are systems that embed a search query and retrieve the documents with the nearest embeddings to the query's embedding. Rerankers, systems (like monoBERT) that look at a query and candidate results and score the relevance of each document to that query. These relevance scores are then used to order the shortlisted results according to their relevance to the query, often producing an improved results ranking. RAG, where search systems have a generative LLM at the end of the pipeline to formulate an answer based on retrieved documents while citing sources."
    - **Plain English:** Recap of the three systems: dense retrieval, rerankers (monoBERT), RAG.
    - **Technical terms:** dense retrieval; rerankers; monoBERT; RAG.

86. **Quote:** "We also looked at one of the possible methods of evaluating search systems. Mean average precision allows us to score search systems to be able to compare across a test suite of queries and their known relevance to the test queries. Evaluating RAG systems requires multiple axes, however, like faithfulness, fluency, and others that can be evaluated by humans or by LLM-as-a-judge."
    - **Plain English:** MAP scores search systems; RAG evaluation needs multiple axes (humans or LLM-as-a-judge).
    - **Technical terms:** mean average precision; faithfulness; fluency; LLM-as-a-judge.

87. **Quote:** "In the next chapter, we will explore how language models can be made multimodal and reason not just about text but images as well."
    - **Plain English:** Next chapter: multimodal LLMs (text + images).
    - **Technical terms:** multimodal LLMs.
