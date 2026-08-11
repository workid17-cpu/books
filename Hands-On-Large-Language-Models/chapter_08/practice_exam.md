# 📘 Practice Exam — Chapter 8: Semantic Search and Retrieval-Augmented Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 8
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which three broad categories of language-model search systems does the chapter describe?
   a) Tokenization, embeddings, generation
   b) Dense retrieval, reranking, RAG
   c) Fine-tuning, classification, clustering
   d) Semantic search, exact match, hybrid

2. What does semantic search enable that keyword search cannot?
   a) Exact phrase matching
   b) Faster indexing
   c) Searching by meaning
   d) Smaller models

3. Dense retrieval systems retrieve results by:
   a) Finding the nearest neighbors of the search query in embedding space
   b) Matching keywords between query and documents
   c) Scoring documents with BM25
   d) Fine-tuning on each query

4. In Figure 8-5, "text 2" is the best result for the query because:
   a) It has the highest BM25 score
   b) It is the longest document
   c) It shares the most keywords
   d) It is closest (smallest distance) to the query in embedding space

5. What is one reason to set a max threshold of similarity score in dense retrieval?
   a) To speed up the search index
   b) To filter out irrelevant results when the corpus has nothing relevant
   c) To force keyword matching
   d) To reduce embedding size

6. The Cohere embeddings of the 15 Interstellar sentences had what shape?
   a) (4,096, 15)
   b) (15, 512)
   c) (15, 4096)
   d) (4096,)

7. Which FAISS index type is used in the dense retrieval example?
   a) IndexFlatL2
   b) IndexFlatIP
   c) IndexHNSW
   d) IndexIVFFlat

8. In the search function, the query is embedded with `input_type=` what?
   a) "search_document"
   b) "embed"
   c) "text"
   d) "search_query"

9. Which algorithm is used as the lexical (keyword) search baseline?
   a) TF-IDF
   b) BM25
   c) PageRank
   d) LSA

10. Why did BM25's top result for "how precise was the science" fail to answer the question?
    a) The index was too small
    b) The tokenizer removed all tokens
    c) Its top hit shared the word "science" but was not semantically relevant
    d) Dense retrieval was not run

11. What is a recommended mitigation when the corpus contains no relevant result for a query?
    a) Set a threshold (maximum distance) for relevance
    b) Fine-tune the model instantly
    c) Delete the query
    d) Use only keyword search

12. Which case is a "perfect" use for keyword matching rather than dense retrieval?
    a) Semantic similarity
    b) Finding related topics
    c) Concept-based search
    d) Exact match for a specific phrase

13. Why do we need to chunk long texts before embedding?
    a) To reduce storage
    b) Transformer models have limited context sizes
    c) To make them keyword-searchable
    d) Embeddings only work on short strings

14. In the "one vector per document" approach, what is the downside of averaging chunk vectors?
    a) It is too slow
    b) It requires a GPU
    c) It produces a highly compressed vector that loses information
    d) It doubles the index size

15. Why is the multiple-vectors-per-document (chunking) approach preferred?
    a) It has full text coverage and captures individual concepts
    b) It is the only approach that works with FAISS
    c) It never requires overlapping chunks
    d) It avoids the need for an index

16. Which technique adds surrounding context to chunks?
    a) Averaging vectors
    b) Training on question-answer pairs
    c) Reducing dimension
    d) Overlapping chunks that include adjacent text

17. For millions of vectors, which approach is recommended?
    a) NumPy distance computation
    b) Approximate nearest neighbor libraries like Annoy or FAISS
    c) Exhaustive search over all pairs
    d) A single representative vector

18. What can vector databases (e.g., Weaviate, Pinecone) do that a plain index cannot?
    a) Embed text
    b) Fine-tune models
    c) Add/delete vectors without rebuilding the index and filter search
    d) Compute BM25 scores

19. In fine-tuning for dense retrieval, which pairs are used as training data?
    a) Queries and their relevant/irrelevant results
    b) Text and its summary
    c) Sentences and their translations
    d) Documents and their titles

20. In the fine-tuning example, what is the negative (irrelevant) query?
    a) "Interstellar release date"
    b) "When did Interstellar premier"
    c) "Interstellar plot"
    d) "Interstellar cast"

21. After fine-tuning, relevant query embeddings are ________ the document and irrelevant ones are ________.
    a) farther from; closer to
    b) closer to; farther from
    c) equidistant from; closer to
    d) identical to; far from

22. What did Microsoft Bing add to achieve its BERT-like search improvements?
    a) Dense retrieval only
    b) A larger model
    c) A reranking step as a final stage in the pipeline
    d) Fine-tuning on Bing queries

23. Cohere's Rerank endpoint returns:
    a) Relevance scores and a reordered set of documents
    b) New embeddings for each document
    c) A summary of the query
    d) Keywords extracted from documents

24. In a two-stage search pipeline, what is the role of the first-stage retriever?
    a) Rerank the final results
    b) Generate the answer
    c) Score each document 0–1
    d) Shortlist candidates (e.g., 100–1000) from a large index

25. On the MIRACL benchmark, a reranker boosted nDCG@10 from:
    a) 26.5 to 42.8
    b) 36.5 to 62.8
    c) 36.5 to 52.8
    d) 62.8 to 90.1

26. A reranker built as a cross-encoder is sometimes referred to as:
    a) BERT-base
    b) RankNet
    c) monoBERT
    d) GPT-3.5

27. How is reranking formulated as a classification problem?
    a) Output a relevance score from 0–1 for each query-document pair
    b) Predict the next token
    c) Classify documents into topics
    d) Predict the query length

28. What three components are needed to evaluate a search system?
    a) Archive, query set, relevance judgments
    b) Model, tokenizer, prompt
    c) Index, embeddings, GPU
    d) Queries, answers, citations

29. For a query with one relevant document, placing it at position 1 yields an average precision of:
    a) 0.5
    b) 1.0
    c) 0.3
    d) 0.0

30. If the only relevant document is placed at position 3, the precision at that position is:
    a) 3
    b) 1
    c) 1/3
    d) 0

31. MAP is computed by:
    a) Taking the mean of the average precisions of each query in the test suite
    b) Summing all precision@10 scores
    c) Averaging BM25 scores
    d) Taking the median of relevance judgments

32. How does nDCG differ from MAP?
    a) It only uses binary relevance
    b) It ignores document order
    c) It uses no relevance judgments
    d) It allows graded (non-binary) relevance

33. What motivated the industry to adopt RAG?
    a) To reduce storage costs
    b) LLMs confidently answer many questions incorrectly (hallucinations)
    c) To make models smaller
    d) To replace embeddings

34. Which of the following is a RAG use case mentioned in the chapter?
    a) Image generation
    b) Token classification
    c) "Chat with my data"
    d) Sentiment analysis

35. Which products incorporate LLMs to summarize results or answer search questions?
    a) Perplexity, Microsoft Bing AI, Google Gemini
    b) Twitter, Facebook, Instagram
    c) Google Drive, Dropbox, Box
    d) YouTube, Netflix, Spotify

36. In the Cohere grounded-generation example, what accompanies the answer text?
    a) A list of keywords
    b) New embeddings
    c) A summary
    d) Span citations (e.g., ChatCitation with start/end and document_ids)

37. Which embedding model is used in the local RAG code example?
    a) BAAI/bge-large
    b) thenlper/gte-small
    c) sentence-transformers/all-mpnet-base-v2
    d) openai/text-embedding-3

38. What is the role of the `{context}` variable in the RAG prompt template?
    a) It stores the chat history
    b) It defines the output format
    c) It communicates the retrieved documents to the LLM
    d) It sets the temperature

39. What chain type is used in `RetrievalQA.from_chain_type`?
    a) 'stuff'
    b) 'map'
    c) 'refine'
    d) 'reduce'

40. Which advanced RAG technique splits a question into multiple separate search queries?
    a) Query rewriting
    b) Query routing
    c) Agentic RAG
    d) Multi-query RAG

---

## Section B: True/False (1 point each)

41. Semantic search is based on keyword matching. (T/F)
42. Dense retrieval converts both the query and the documents into embeddings. (T/F)
43. A larger distance between embeddings means greater similarity. (T/F)
44. BM25 is a lexical (keyword) search method. (T/F)
45. Hybrid search combines semantic search and keyword search. (T/F)
46. Transformer models have unlimited context sizes. (T/F)
47. Embedding only the beginning of a document captures all its information. (T/F)
48. NumPy distance computation is reasonable for thousands or tens of thousands of vectors. (T/F)
49. Vector databases require rebuilding the index to add new vectors. (T/F)
50. Fine-tuning for retrieval optimizes text embeddings, not just token embeddings. (T/F)
51. A reranker evaluates each document independently while processing the batch. (T/F)
52. In average precision, a relevant result at a lower position is penalized. (T/F)
53. nDCG treats relevance as binary only. (T/F)
54. RAG systems reduce hallucinations and improve factuality. (T/F)
55. Query routing gives the model the ability to search multiple data sources. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Explain how dense retrieval turns search into a nearest-neighbor problem, and describe the four steps of the Cohere example.
57. What is BM25, why is it used in the chapter, and what did its result for "how precise was the science" demonstrate?
58. List and explain the caveats of dense retrieval and the recommended mitigations.
59. Compare "one vector per document" vs "multiple vectors per document" for chunking long texts, including the averaging downside and overlapping-chunk context strategies.
60. When should you use NumPy, ANN libraries (Annoy/FAISS), or vector databases (Weaviate/Pinecone)?
61. How does fine-tuning improve dense retrieval, using the Interstellar premiere example (positive and negative pairs)?
62. Explain reranking: what it is, how a cross-encoder/monoBERT works, and how the two-stage pipeline functions.
63. How is average precision calculated for a single query, and how is it extended to MAP across queries?
64. What is RAG, what problems does it solve, and how does grounded generation work with Cohere (including citations)?
65. Describe the advanced RAG techniques: query rewriting, multi-query, multi-hop, query routing, and agentic RAG.

---

## Section D: Essay / Applied (5 points each)

66. **Dense retrieval end-to-end.** Explain the intuition (embeddings as points in space, nearest neighbors), the full Cohere example (chunking sentences, co.embed with search_document/search_query, the (15, 4096) shape, FAISS IndexFlatL2 index, the search function), the comparison with BM25 keyword search, and the caveats (thresholds, exact-phrase/hybrid search, domain transfer).
67. **Chunking and scalable search.** Discuss why chunking is needed (context limits), the two approaches (one vector per document — title/beginning or averaging; multiple vectors per document), the chunking strategies (sentence, paragraph/3–8 sentences, adding title, overlapping chunks), and scaling (NumPy vs Annoy/FAISS vs vector databases like Weaviate/Pinecone).
68. **Fine-tuning and reranking.** Explain fine-tuning embedding models for retrieval (query/document positive and negative pairs; making relevant closer, irrelevant farther), then reranking: the second-stage cross-encoder/monoBERT, Cohere's Rerank endpoint, the two-stage pipeline (first-stage shortlisting with keyword/dense/hybrid), the keyword+rerank example, and the MIRACL 36.5→62.8 improvement.
69. **Evaluating search.** Describe the three evaluation components (archive, queries, relevance judgments), why counting relevant results is insufficient, how average precision is computed (precision@k, position-1=1, position-3 penalty, multiple relevant documents), how MAP aggregates across queries, and how nDCG differs (graded relevance).
70. **From search to RAG and its evaluation.** Explain RAG's motivation (hallucinations, factuality, "chat with my data"), the search→RAG grounded-generation flow (Cohere co.chat with citations; local Phi-3 + gte-small + FAISS + RetrievalQA stuff chain), the advanced techniques (query rewriting, multi-query, multi-hop, query routing, agentic RAG / Command R+), and RAG evaluation (fluency, perceived utility, citation recall/precision; Ragas faithfulness and answer relevance; LLM-as-a-judge).

---

## ANSWER KEY

### Section A: Multiple Choice
1. b
2. c
3. a
4. d
5. b
6. c
7. a
8. d
9. b
10. c
11. a
12. d
13. b
14. c
15. a
16. d
17. b
18. c
19. a
20. d
21. b
22. c
23. a
24. d
25. b
26. c
27. a
28. d
29. b
30. c
31. a
32. d
33. b
34. c
35. a
36. d
37. b
38. c
39. a
40. d

### Section B: True/False
41. **F** — Semantic search searches by meaning, not keyword matching.
42. **T** — Dense retrieval embeds both query and documents.
43. **F** — A SMALLER distance means greater similarity.
44. **T** — BM25 is a lexical (keyword) search method.
45. **T** — Hybrid search combines semantic and keyword search.
46. **F** — Transformers have limited context sizes.
47. **F** — Embedding only the beginning leaves much information unindexed/unsearchable.
48. **T** — NumPy is reasonable for thousands/tens of thousands of vectors.
49. **F** — Vector databases allow adding/deleting vectors without rebuilding the index.
50. **T** — Fine-tuning optimizes text embeddings (not just token embeddings).
51. **T** — All documents are processed as a batch, but each is evaluated independently.
52. **T** — Lower-position relevant results are penalized in average precision.
53. **F** — nDCG allows graded (non-binary) relevance.
54. **T** — RAG reduces hallucinations and improves factuality.
55. **T** — Query routing sends questions to multiple data sources.

### Section C: Short Answer (model answers)
56. **Dense retrieval.** Embeddings are points in space; similar texts are close. Both query and documents are embedded into the same space; the nearest documents to the query are the results. Steps: 1) get text + light processing, chunk into sentences; 2) embed sentences (co.embed, input_type="search_document"); 3) build index (faiss.IndexFlatL2, add float32 embeddings); 4) embed query (input_type="search_query"), search, return a DataFrame of texts and distances.
57. **BM25** is a leading lexical (keyword) search algorithm (rank_bm25 BM25Okapi). It provides the keyword baseline. For "how precise was the science," its top hit shared the word "science" but didn't answer the question — demonstrating that keyword search misses meaning-based matches that dense retrieval finds.
58. **Caveats:** 1) Still returns results + distances when no answer exists → use a max-distance threshold or present results for the user to judge (track clicks/satisfaction); 2) weak at exact-phrase matches → use hybrid search (semantic + keyword); 3) domain transfer — degrades on domains not in training data (e.g., legal texts) → need in-domain data.
59. **One vector per document:** embed only a representative part (title/beginning — quick for demos, leaves info unsearchable) or embed chunks then average (highly compressed, lossy). **Multiple vectors per document:** chunk and embed each piece — full coverage, captures individual concepts, more expressive index. Context strategies: add the document title to chunks, and overlap chunks with surrounding text.
60. **NumPy** for thousands/tens of thousands of vectors (direct distance computation). **ANN libraries (Annoy/FAISS)** for millions of vectors — milliseconds, GPU/cluster scaling. **Vector databases (Weaviate, Pinecone)** when you need dynamic add/delete without rebuilding and filtering/customization beyond distances.
61. **Fine-tuning:** train on queries + relevant results. Example: document "Interstellar premiered on October 26, 2014" with positive queries "Interstellar release date" and "When did Interstellar premier" and negative query "Interstellar cast." Before fine-tuning all three may be equidistant (all about Interstellar); fine-tuning pulls positives closer and pushes the negative farther.
62. **Reranking** is a second-stage step that reorders shortlisted results by relevance to the query. A cross-encoder/monoBERT presents query + document simultaneously, scoring each independently on 0–1 (classification). The first-stage retriever (keyword, dense, or hybrid) shortlists candidates (100–1000); the reranker then reorders them (e.g., BM25 top-10 → rerank top-3).
63. **Average precision:** compute precision at each position containing a relevant document (precision@k = # relevant up to k / k); relevant at position 1 → 1.0; at position 3 → 1/3. For multiple relevant docs, average the precision values at their positions. **MAP** = mean of each query's average precision across the test suite.
64. **RAG** = text generation systems incorporating search to reduce hallucinations, improve factuality, and ground the model on a dataset ("chat with my data"). Grounded generation adds retrieved documents as context to the LLM. With Cohere: embedding search retrieves top docs, passed as `documents` to `co.chat`; the answer includes span citations (ChatCitation with start/end, text, document_ids).
65. **Advanced RAG:** Query rewriting — LLM rewrites verbose/conversational messages into concise queries (e.g., → "Where do dolphins live"). Multi-query — splits a question into several searches (Nvidia 2020 vs 2023). Multi-hop — sequential queries where answers inform follow-ups (car manufacturers → their EVs). Query routing — chooses the right data source (HR → Notion, customer data → CRM). Agentic RAG — delegates more to the LLM with data sources as tools (search AND post to Notion); Command R+ excels and is open-weights.

### Section D: Essay (grading notes)
66. **Expect** embedding intuition (points in space, similar texts nearby); the Cohere pipeline (text.split('.') chunking, co.embed search_document → (15, 4096), faiss.IndexFlatL2 + add, search function with input_type search_query, index.search, DataFrame of texts/distances, smallest distance = most similar); top result answering "how precise was the science"; BM25 keyword baseline missing the meaning-based match; caveats (threshold for missing answers, exact-phrase/hybrid search, domain transfer/legal texts).
67. **Expect** context-limit rationale; one vector per document (embed title/beginning — demo-only, unsearchable info; or chunk-then-average — compressed/lossy); multiple vectors per document (full coverage, concept capture, expressive index); strategies (sentence = too granular, paragraph or every 3–8 sentences, add title, overlapping chunks); scaling (NumPy for small, Annoy/FAISS ANN for millions with GPU/cluster, Weaviate/Pinecone vector DBs with dynamic add/delete + filtering).
68. **Expect** fine-tuning on (query, relevant) and (query, irrelevant) pairs; the Interstellar premiere example (release date / premier = positive; cast = negative); before-fine-tuning equidistance and after fine-tuning closer/farther; reranking as final pipeline stage (Bing/BERT); cross-encoder monoBERT (query+document together, batch but independent scoring, 0–1 classification); Cohere Rerank (no training, relevance scores); two-stage pipeline (shortlist 100–1000 then rerank); keyword top-10 → rerank top-3 example; MIRACL 36.5→62.8 nDCG@10.
69. **Expect** three components (archive, queries, relevance judgments); why counting isn't enough (position matters; system 1 relevant at position 1 vs system 2 lower); AP calculation (precision@k at relevant positions; position 1 = 1/1 = 1.0 perfect; position 3 penalty; multiple relevant docs → average of precisions at their positions); MAP = mean of per-query APs; nDCG graded/non-binary relevance.
70. **Expect** RAG motivation (confident hallucinations, factuality, "chat with my data," grounding on company data/book); search→RAG (LLM at end of pipeline, question + top documents, grounded generation); Cohere example (co.chat documents, citations ChatCitation); local RAG (Phi-3 GGUF LlamaCpp, HuggingFaceEmbeddings gte-small / bge-small-en-v1.5, FAISS.from_texts, RetrievalQA stuff chain, prompt with context/question); advanced techniques (query rewriting, multi-query, multi-hop, routing, agentic RAG with tools and Command R+); evaluation (fluency, perceived utility, citation recall, citation precision; Ragas faithfulness + answer relevance; LLM-as-a-judge).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely dense retrieval caveats/chunking, AP/MAP calculation, or advanced RAG techniques).
- **<70%**: Re-read the chapter and study notes, then retake Sections A and B before attempting C and D again.
