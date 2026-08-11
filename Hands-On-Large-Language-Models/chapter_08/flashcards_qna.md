# 📘 Chapter 8 Flashcards: Semantic Search and Retrieval-Augmented Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 8

---

## Part 1: Terms → Definitions

**Q:** What is semantic search?
**A:** Searching by meaning rather than keyword matching; the key ability language models added to search engines.

**Q:** What is dense retrieval?
**A:** A type of semantic search that relies on embedding similarity — embedding both the query and the documents, then retrieving the nearest neighbors of the query.

**Q:** What is reranking?
**A:** A search-pipeline step where a language model scores the relevance of a subset of results against the query; the order of results is then changed based on these scores.

**Q:** What is RAG (Retrieval-Augmented Generation)?
**A:** Text generation systems that incorporate search capabilities to reduce hallucinations, increase factuality, and/or ground the generation model on a specific dataset; from the 2020 paper "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks."

**Q:** What is generative search?
**A:** A subset of RAG systems that include a model that generates an answer in response to a query.

**Q:** What is an embedding, conceptually?
**A:** A numeric representation of text, thought of as a point in space; points close together mean the texts they represent are similar.

**Q:** What is a search index?
**A:** A structure that stores embeddings and is optimized to quickly retrieve nearest neighbors even for very large numbers of points.

**Q:** What is FAISS (as used here)?
**A:** A library for fast nearest-neighbor search over embeddings; the chapter uses `faiss.IndexFlatL2` (L2/Euclidean distance).

**Q:** What is BM25?
**A:** A leading lexical (keyword) search algorithm, implemented via `rank_bm25` (`BM25Okapi`); used as a comparison baseline to dense retrieval.

**Q:** What is hybrid search?
**A:** A search system that includes both semantic search and keyword search; advised instead of relying solely on dense retrieval.

**Q:** Why do we chunk long texts before embedding?
**A:** Because transformer language models have limited context sizes — long texts cannot be fed above the supported number of words/tokens.

**Q:** What is a vector database?
**A:** A vector retrieval system (e.g., Weaviate, Pinecone) that allows adding or deleting vectors without rebuilding the index and supports filtering/customization beyond vector distances.

**Q:** What are approximate nearest neighbor (ANN) libraries?
**A:** Optimized retrieval libraries (e.g., Annoy, FAISS) for millions of vectors that retrieve results from massive indexes in milliseconds, sometimes using GPUs and cluster scaling.

**Q:** What is a cross-encoder (reranker)?
**A:** A way of building LLM search rerankers where the query and a candidate result are presented to the model at the same time so it can view both before assigning a relevance score; referred to as monoBERT (from "Multi-stage document ranking with BERT").

**Q:** What is mean average precision (MAP)?
**A:** An Information Retrieval metric that scores a search system across all queries in a test suite by taking the mean of the average precision of each query.

**Q:** What is average precision (AP)?
**A:** A per-query metric that considers the precision at k of all relevant documents, penalizing relevant results placed at lower positions.

**Q:** What is precision at k?
**A:** The number of relevant results at position k divided by the position being looked at (counting up to k).

**Q:** What is nDCG?
**A:** Normalized discounted cumulative gain — a search metric where relevance is not binary (graded), so one document can be labeled more relevant than another.

**Q:** What is grounded generation?
**A:** The generation step where retrieved relevant information establishes context that grounds the LLM in the domain of interest; the LLM is prompted with the question and the retrieved information.

**Q:** What is query rewriting?
**A:** Using an LLM to rewrite a verbose or conversational user message into a concise query that aids the retrieval step (e.g., Cohere's dedicated query-rewriting mode for co.chat).

**Q:** What is multi-query RAG?
**A:** Extending query rewriting to search multiple queries when more than one is needed to answer a question (e.g., "Compare Nvidia 2020 vs 2023" → two separate queries).

**Q:** What is multi-hop RAG?
**A:** A series of sequential queries where each step's answer informs the next (e.g., largest car manufacturers → their EV status).

**Q:** What is query routing?
**A:** Giving the model the ability to search multiple data sources (e.g., HR questions → Notion, customer data → CRM/Salesforce).

**Q:** What is agentic RAG?
**A:** RAG that delegates more responsibility to the LLM — gauging information needs and utilizing multiple data sources, with data sources abstracted into tools (e.g., post to Notion, not just search); approaches an agent acting on the world.

**Q:** What is LLM-as-a-judge?
**A:** Using a capable LLM to score different generations along different evaluation axes to automate RAG evaluation.

**Q:** What is Ragas?
**A:** A software library that automates RAG evaluation, scoring metrics like faithfulness and answer relevance.

**Q:** What is fluency (RAG eval axis)?
**A:** Whether the generated text is fluent and cohesive.

**Q:** What is perceived utility (RAG eval axis)?
**A:** Whether the generated answer is helpful and informative.

**Q:** What is citation recall (RAG eval axis)?
**A:** The proportion of generated statements about the external world that are fully supported by their citations.

**Q:** What is citation precision (RAG eval axis)?
**A:** The proportion of generated citations that support their associated statements.

**Q:** What is faithfulness (Ragas metric)?
**A:** Whether the answer is consistent with the provided context.

**Q:** What is answer relevance (Ragas metric)?
**A:** How relevant the answer is to the question.

## Part 2: Short Answer

**Q:** Which three broad categories of LM search systems does the chapter describe?
**A:** Dense retrieval, reranking, and RAG.

**Q:** How does dense retrieval turn search into a nearest-neighbor problem?
**A:** Both the query and documents are converted into embeddings; the query is embedded into the same space as the archive, and the nearest documents to the query are returned as results.

**Q:** In the dense retrieval example, what do "text 2" and "text 1" being close together mean?
**A:** Their texts are semantically similar; the query is closest to "text 2," so that is the best result.

**Q:** Why might a designer add a max threshold of similarity score to a dense retrieval system?
**A:** To filter out irrelevant results in case the corpus has no relevant results for the query.

**Q:** What were the four steps of the Cohere dense retrieval example?
**A:** 1) Get text + light processing, chunk into sentences; 2) embed the sentences; 3) build the search index; 4) search and see the results.

**Q:** What shape did the Cohere embeddings output have and what does it mean?
**A:** (15, 4096) — 15 sentence vectors, each of size 4,096.

**Q:** Which FAISS index type is used and why?
**A:** `faiss.IndexFlatL2` with dimension `embeds.shape[1]`; it uses L2 (Euclidean) distance to find nearest neighbors.

**Q:** In the search function, what is the difference between `input_type="search_document"` and `input_type="search_query"`?
**A:** `search_document` embeds the archive texts for indexing; `search_query` embeds the user query for retrieval.

**Q:** Why did BM25's top result for "how precise was the science" not answer the question?
**A:** Because BM25 is lexical — its top hit shared the word "science" with the query but wasn't semantically about scientific accuracy.

**Q:** What are the three caveats of dense retrieval?
**A:** 1) It still returns results + distances when no answer exists (use a threshold or let users judge); 2) it is weak at exact-phrase matches (hybrid search is advised); 3) it degrades on domains it wasn't trained on (e.g., legal texts).

**Q:** What are the two approaches for handling long documents?
**A:** One vector per document, or multiple vectors per document (chunking).

**Q:** What are the two one-vector-per-document options and their downsides?
**A:** Embed only a representative part (title/beginning — leaves most info unsearchable; quick for demos) or embed chunks then aggregate by averaging (highly compressed, lossy vector).

**Q:** Why is the multiple-vectors-per-document (chunking) approach preferred?
**A:** It has full coverage of the text and the vectors capture individual concepts inside the text → a more expressive search index.

**Q:** What are the chunking strategies mentioned for incorporating context?
**A:** Adding the title of the document to the chunk, and adding some text before/after so chunks overlap with surrounding context.

**Q:** When is NumPy distance computation reasonable vs ANN libraries?
**A:** NumPy for thousands or tens of thousands of vectors; Annoy/FAISS (approximate nearest neighbor) beyond millions, with GPU/cluster scaling.

**Q:** What can vector databases (Weaviate, Pinecone) do that a plain index can't?
**A:** Add or delete vectors without rebuilding the index, and filter/customize search beyond mere vector distances.

**Q:** How does fine-tuning improve dense retrieval?
**A:** On (query, relevant document) positive pairs and (query, irrelevant document) negative pairs — pulling relevant query embeddings closer to the document and pushing irrelevant ones farther. Retrieval optimizes text embeddings, not just token embeddings.

**Q:** In the fine-tuning example, what were the positive and negative pairs?
**A:** Positive: "Interstellar release date" and "When did Interstellar premier" with the premier sentence; negative: "Interstellar cast" with the same sentence.

**Q:** What did Microsoft Bing add to achieve its BERT-like improvements?
**A:** A reranking step as a final stage in its search pipeline.

**Q:** How does Cohere's Rerank endpoint work and does it need training?
**A:** Pass the query and documents (`co.rerank(query=..., documents=..., top_n=3, return_documents=True)`); it returns relevance scores and the top-n ordering. No training or tuning needed.

**Q:** What are the two stages of a typical search pipeline with a reranker?
**A:** First stage: a cheap retriever shortlists candidates (e.g., 100 or 1000 from millions) using keyword, dense, or hybrid search; second stage: the reranker reorders them by relevance.

**Q:** What is the effect of adding a reranker on the MIRACL benchmark?
**A:** It boosts performance from 36.5 to 62.8, measured as nDCG@10.

**Q:** How does a reranker (cross-encoder/monoBERT) assign scores?
**A:** The query and each document are presented to the model simultaneously (as a batch, but each scored independently); it outputs a relevance score from 0–1 — a classification problem (0 = irrelevant, 1 = highly relevant).

**Q:** What three components are needed to evaluate a search system?
**A:** A text archive, a set of queries, and relevance judgments indicating which documents are relevant for each query.

**Q:** Why is counting relevant results not enough to compare two systems?
**A:** Because position matters — a relevant result at the first position is better than one ranked lower; MAP quantifies this.

**Q:** How is average precision calculated for a query with one relevant document?
**A:** Precision is computed at each position of a relevant document (precision@k = # relevant up to k / k). Relevant at position 1 → precision@1 = 1/1 = 1.0 (perfect); relevant at position 3 → penalized (1/3).

**Q:** How do you compute MAP?
**A:** Take the average precision of each query in the test suite, then compute the mean across all queries.

**Q:** Why is "mean" and "average" both used in MAP?
**A:** It's likely an aesthetic choice — MAP sounds better than "average average precision."

**Q:** How does nDCG differ from MAP?
**A:** nDCG uses graded (non-binary) relevance — one document can be labeled more relevant than another.

**Q:** What motivated RAG?
**A:** LLMs confidently answer lots of questions incorrectly (hallucinations); RAG retrieves relevant info to aid factual generation.

**Q:** What are the benefits and use cases of RAG?
**A:** Reduces hallucinations, improves factuality, and enables "chat with my data" — grounding on internal company data or a specific data source (e.g., chatting with a book).

**Q:** Which search products incorporate LLMs to summarize/answer?
**A:** Perplexity, Microsoft Bing AI, and Google Gemini.

**Q:** How does Cohere's grounded generation return source information?
**A:** The chat response includes citations like `ChatCitation(start=21, end=36, text='worldwide gross', document_ids=['doc_0'])` — span-level source references.

**Q:** What do we lose when using local models for RAG?
**A:** The ability to do span citations, and the smaller local model doesn't work as well as the larger managed model.

**Q:** What embedding model does the book highlight and which is used in the code?
**A:** The book highlights BAAI/bge-small-en-v1.5 (high on MTEB leaderboard, relatively small); the code uses `HuggingFaceEmbeddings(model_name='thenlper/gte-small')`.

**Q:** What is the RAG prompt template's role?
**A:** It is the central place that communicates the retrieved documents to the LLM via a `{context}` input variable, along with the `{question}`.

**Q:** How is the local RAG pipeline built?
**A:** `db = FAISS.from_texts(texts, embedding_model)`; then `rag = RetrievalQA.from_chain_type(llm=llm, chain_type='stuff', retriever=db.as_retriever(), chain_type_kwargs={"prompt": prompt}, verbose=True)`; query with `rag.invoke(...)`.

**Q:** Why is query rewriting useful in a RAG chatbot?
**A:** Verbose or conversation-referencing questions would struggle in the search step; rewriting yields a concise query that aids retrieval.

**Q:** Give the query-rewriting example from the chapter.
**A:** A rambling message about an essay on penguins/dolphins is rewritten into the concise query "Where do dolphins live."

**Q:** Give the multi-query RAG example.
**A:** "Compare the financial results of Nvidia in 2020 vs. 2023" → two queries: "Nvidia 2020 financial results" and "Nvidia 2023 financial results."

**Q:** Give the multi-hop RAG example.
**A:** "Who are the largest car manufacturers in 2023? Do they each make EVs or not?" → first query "largest car manufacturers 2023" (→ Toyota, Volkswagen, Hyundai), then follow-ups per manufacturer about electric vehicles.

**Q:** Give the query routing example.
**A:** HR questions → the company's HR information system (Notion); customer data questions → the CRM (Salesforce).

**Q:** How does agentic RAG move toward agents?
**A:** It delegates more responsibility to the LLM (gauging information needs, using multiple data sources), and data sources become tools — e.g., not just searching Notion but posting to Notion too.

**Q:** Which model excels at agentic RAG tasks and is open-weights?
**A:** Cohere's Command R+; note that likely only the largest managed models can attempt these behaviors at the time of writing.

**Q:** What paper evaluates generative search and along which axes?
**A:** "Evaluating verifiability in generative search engines" (2023, Liu, Zhang, Liang); axes: fluency, perceived utility, citation recall, citation precision.

**Q:** Why might you use LLM-as-a-judge or Ragas?
**A:** To automate RAG evaluation when human evaluation isn't feasible; Ragas also scores faithfulness (consistency with context) and answer relevance.

## Part 3: Fill-in-the-Blank

**Q:** Search was one of the first language model applications, powered by the ________ (2018) paper within months.
**A:** BERT.

**Q:** ________ enables searching by meaning, not simply keyword matching.
**A:** Semantic search.

**Q:** The three broad categories of LM search systems are ________ retrieval, ________, and RAG.
**A:** dense; reranking.

**Q:** Dense retrieval retrieves the ________ ________ of the search query in embedding space.
**A:** nearest neighbors.

**Q:** In dense retrieval, a ________ distance means the text is more similar to the query.
**A:** smaller (lower).

**Q:** The Cohere embeddings output shape (15, 4096) means ________ vectors each of size ________.
**A:** 15; 4,096.

**Q:** The chapter builds its index with ________, using L2 distance.
**A:** FAISS (faiss.IndexFlatL2).

**Q:** BM25 is a leading ________ (keyword) search method.
**A:** lexical.

**Q:** For exact-phrase matching, ________ search (semantic + keyword) is advised over dense retrieval alone.
**A:** hybrid.

**Q:** Transformers are limited in ________ sizes, so long texts must be chunked.
**A:** context.

**Q:** The ________-vectors-per-document approach gives full text coverage and captures individual concepts.
**A:** multiple.

**Q:** Overlapping chunks and adding the document ________ help preserve context around chunks.
**A:** title.

**Q:** For millions of vectors, use ________ nearest neighbor libraries like Annoy or FAISS.
**A:** approximate.

**Q:** Vector databases like ________ and Pinecone allow adding/deleting vectors without rebuilding the index.
**A:** Weaviate.

**Q:** Fine-tuning for retrieval optimizes ________ embeddings on query/result pairs.
**A:** text.

**Q:** A reranker scores each document from 0 to ________, where 1 is highly relevant.
**A:** 1.

**Q:** The cross-encoder reranking method is sometimes called ________.
**A:** monoBERT.

**Q:** On the ________ benchmark, a reranker boosted nDCG@10 from 36.5 to 62.8.
**A:** MIRACL.

**Q:** Evaluating search systems needs a text archive, a set of ________, and relevance judgments.
**A:** queries.

**Q:** A relevant document at position 1 gives a perfect average precision score of ________.
**A:** 1 (1.0).

**Q:** MAP = the ________ of the average precisions of each query.
**A:** mean.

**Q:** nDCG allows ________ (non-binary) relevance, unlike MAP's binary relevance.
**A:** graded.

**Q:** RAG systems reduce ________ and improve factuality by adding search to generation.
**A:** hallucinations.

**Q:** The generation step that provides retrieved context to the LLM is called ________ generation.
**A:** grounded.

**Q:** Cohere's grounded chat returns ________ (span-level source references) with document IDs.
**A:** citations (ChatCitation).

**Q:** The local RAG pipeline uses ________QA with chain_type 'stuff'.
**A:** Retrieval.

**Q:** ________ rewriting converts verbose user messages into concise retrieval queries.
**A:** Query.

**Q:** ________-query RAG searches multiple queries when more than one is needed.
**A:** Multi.

**Q:** ________-hop RAG uses a series of sequential queries where each answer informs the next.
**A:** Multi.

**Q:** ________ routing sends questions to different data sources (e.g., HR → Notion).
**A:** Query.

**Q:** ________ RAG abstracts data sources into tools and approaches agent behavior.
**A:** Agentic.

**Q:** ________ is an open-weights Cohere model that excels at agentic RAG tasks.
**A:** Command R+.

**Q:** Citation ________ is the proportion of statements fully supported by citations.
**A:** recall.

**Q:** Citation ________ is the proportion of citations that support their associated statements.
**A:** precision.

**Q:** The ________ library automates RAG evaluation and scores faithfulness and answer relevance.
**A:** Ragas.

**Q:** Using a capable LLM to score generations along evaluation axes is called LLM-as-a-________.
**A:** judge.

**Q:** The next chapter explores ________ LLMs that reason about images as well as text.
**A:** multimodal.
