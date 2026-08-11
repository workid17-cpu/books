# 📘 Chapter 8 Study Bundle: Semantic Search and Retrieval-Augmented Generation
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 8

---

## §1. Study Notes

### Core Theme
This chapter explores how language models power modern search systems, dividing the space into three broad categories: **dense retrieval** (embedding-based nearest-neighbor search), **reranking** (a second-stage model that reorders shortlisted results by relevance), and **Retrieval-Augmented Generation (RAG)** (adding a grounded text-generation step that formulates answers from retrieved documents). It covers the implementation details of each (Cohere API examples, FAISS, BM25 keyword search, Sentence Transformers), the caveats of dense retrieval and text chunking strategies, fine-tuning embedding models for retrieval, search evaluation metrics (mean average precision, nDCG), advanced RAG techniques (query rewriting, multi-query, multi-hop, query routing, agentic RAG), and RAG evaluation (fluency, perceived utility, citation recall/precision, Ragas, LLM-as-a-judge). The next chapter moves to multimodal LLMs.

### Key Definitions
- **Semantic search:** Searching by meaning rather than keyword matching; the ability added by language models to search engines.
- **Dense retrieval:** A type of semantic search that relies on the similarity of text embeddings — turning the search problem into retrieving the nearest neighbors of the search query (after both query and documents are converted into embeddings).
- **Reranking:** A search-pipeline step where a language model scores the relevance of a subset of results against the query; the order of results is then changed based on these scores.
- **RAG (Retrieval-Augmented Generation):** Text generation systems that incorporate search capabilities to reduce hallucinations, increase factuality, and/or ground the generation model on a specific dataset; described in the paper "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (2020, Patrick Lewis et al.).
- **Generative search:** A subset of RAG systems that include a model that generates an answer in response to a query.
- **Embedding:** A numeric representation of text, thought of as a point in space; points that are close together mean the texts they represent are similar.
- **Search index:** A structure that stores embeddings and is optimized to quickly retrieve nearest neighbors even for very large numbers of points.
- **FAISS:** `faiss.IndexFlatL2` — a library/index used for fast nearest-neighbor search over embeddings (L2/Euclidean distance).
- **BM25:** A leading lexical (keyword) search algorithm, implemented via `rank_bm25` (`BM25Okapi`); used to compare against dense retrieval.
- **Hybrid search:** A search system that includes both semantic search and keyword search; advised instead of relying solely on dense retrieval.
- **Chunking:** Breaking long texts into smaller pieces before embedding; needed because transformer models have limited context sizes.
- **Vector database:** A vector retrieval system (e.g., Weaviate, Pinecone) that allows adding or deleting vectors without rebuilding the index and supports filtering/customization beyond vector distances.
- **Approximate nearest neighbor (ANN):** Optimized retrieval approach (e.g., Annoy, FAISS) for millions of vectors; can retrieve results from massive indexes in milliseconds, sometimes using GPUs and scaling to clusters.
- **Cross-encoder:** A way of building LLM search rerankers where the query and a possible result are presented to the model at the same time so it can view both before assigning a relevance score; referred to as monoBERT (from "Multi-stage document ranking with BERT").
- **Mean average precision (MAP):** An Information Retrieval evaluation metric that scores a search system across all queries in a test suite by taking the mean of the average precision of each query.
- **Average precision (AP):** A per-query metric that considers the precision at k of all relevant documents, penalizing relevant results placed at lower positions.
- **Precision at k:** The number of relevant results at position k divided by the position currently being looked at (counting up to k).
- **nDCG (normalized discounted cumulative gain):** A search metric more nuanced than MAP because relevance is not binary — one document can be labeled as more relevant than another.
- **Grounded generation:** The generation step where retrieved relevant information establishes context that grounds the LLM in the domain of interest; the LLM is prompted with the question and the retrieved information.
- **Query rewriting:** Using an LLM to rewrite a verbose or conversational user message into a concise query that aids the retrieval step (e.g., Cohere's dedicated query-rewriting mode for co.chat).
- **Multi-query RAG:** Extending query rewriting to search multiple queries when more than one is needed to answer a question (e.g., "Compare Nvidia 2020 vs 2023" → two queries).
- **Multi-hop RAG:** A series of sequential queries where each step's answer informs the next (e.g., largest car manufacturers → their EV status).
- **Query routing:** Giving the model the ability to search multiple data sources (e.g., HR → Notion, customer data → CRM/Salesforce).
- **Agentic RAG:** RAG where the LLM delegates more responsibility — gauging information needs, utilizing multiple data sources, and having data sources abstracted into tools (e.g., post to Notion, not just search); approaches agent behavior.
- **LLM-as-a-judge:** Using a capable LLM to score different generations along different axes to automate evaluation.
- **Ragas:** A software library that automates RAG evaluation by scoring metrics like faithfulness and answer relevance.
- **Fluency (RAG eval):** Whether the generated text is fluent and cohesive.
- **Perceived utility (RAG eval):** Whether the generated answer is helpful and informative.
- **Citation recall (RAG eval):** The proportion of generated statements about the external world that are fully supported by their citations.
- **Citation precision (RAG eval):** The proportion of generated citations that support their associated statements.
- **Faithfulness (Ragas):** Whether the answer is consistent with the provided context.
- **Answer relevance (Ragas):** How relevant the answer is to the question.

### Core Concepts & Frameworks
- **Search as a first LLM application:** Months after the BERT (2018) paper, Google announced it was using it to power Google Search ("one of the biggest leaps forward in the history of Search"); Microsoft Bing also said it used large transformer models to deliver its largest quality improvements. Semantic search enables searching by meaning, not just keywords.
- **Three broad categories:** dense retrieval (embedding nearest neighbors), reranking (scoring/ordering a subset of results), and RAG (generation systems with search to ground the model). These are the major categories but not the only LLM search applications.
- **Dense retrieval intuition:** Embeddings are points in space; similar texts are near each other. A user query is embedded into the same space as the text archive; the nearest documents to the query are the search results (Figure 8-5: "text 2" is best, then "text 1").
- **Two design questions in dense retrieval:** (1) Should an irrelevant result be returned at all? A max threshold on similarity can filter out irrelevant results when the corpus has nothing relevant. (2) Are a query and its best result always semantically similar? Not always — models need to be trained on question-answer pairs to get better at retrieval (detailed in Chapter 10).
- **Dense retrieval workflow (Cohere example):** (1) Get text + light processing, chunk into sentences (`text.split('.')`, strip whitespace); (2) embed sentences via `co.embed(texts=texts, input_type="search_document")` → shape (15, 4096), 15 vectors of size 4096; (3) build index: `faiss.IndexFlatL2(dim)`, `index.add(np.float32(embeds))`; (4) search: embed query with `input_type="search_query"`, `index.search(...)`, return a pandas DataFrame of texts and distances. The best result is the one with the least distance.
- **Dense vs keyword (BM25):** For "how precise was the science", dense retrieval's top hit is the sentence about scientific accuracy — which keyword search misses (its top hit shares the word "science" but doesn't answer the question). This shows semantic search's advantage over lexical matching.
- **Caveats of dense retrieval:** (1) If texts don't contain the answer, we still get results + distances — use a threshold (maximum distance) as a heuristic, or present best info and let the user judge; tracking click/satisfaction data can improve future versions. (2) Exact-match phrase queries are perfect for keyword matching → hybrid search (semantic + keyword) is advised. (3) Domain transfer — models trained on internet/Wikipedia data work less well on other domains (e.g., legal texts) without enough in-domain training data.
- **Chunking long texts:** Transformer models have limited context sizes, so long texts can't be embedded whole. Two approaches: (a) one vector per document, or (b) multiple vectors per document. One-vector options: embed only a representative part (title or beginning — quick for demos, leaves lots of info unsearchable; OK for documents where the beginning captures the main points) or embed chunks then aggregate (usually by averaging — highly compressed, loses information). Multiple vectors per document: chunk into pieces and embed each chunk — better because of full text coverage and vectors capturing individual concepts → a more expressive search index.
- **Chunking strategies (Figure 8-9):** sentence-per-chunk (possibly too granular, not enough context), paragraph-per-chunk (great for short paragraphs, otherwise every 3–8 sentences), incorporating context (add the document title to the chunk, or add text before/after so chunks overlap — Figure 8-10). Expect more strategies (some may use LLMs to dynamically split text into meaningful chunks).
- **Nearest neighbor search vs vector databases:** For thousands/tens of thousands of vectors, compute distances directly with NumPy. Beyond millions, use approximate nearest neighbor libraries (Annoy or FAISS) — milliseconds on massive indexes, some with GPU/cluster scaling. Vector databases (Weaviate, Pinecone) allow adding/deleting vectors without rebuilding the index and provide filtering/customization beyond vector distances.
- **Fine-tuning embedding models for dense retrieval:** As in text classification (Ch 4), fine-tuning improves performance. Retrieval needs to optimize text embeddings (not just token embeddings). Training data = queries + relevant results. Example: document "Interstellar premiered on October 26, 2014, in Los Angeles" with relevant queries "Interstellar release date" and "When did Interstellar premier", plus an irrelevant query "Interstellar cast" (negative example). Before fine-tuning, all three queries may be equidistant from the document (all talk about Interstellar); fine-tuning makes relevant queries closer and irrelevant ones farther.
- **Reranking:** For organizations with existing search systems, an easier way to incorporate LMs is as a final step that reorders results by relevance to the query. This is what Bing added with BERT-like models (a two-stage system, Figure 8-14). A reranker takes the query and a number of search results and returns the optimal ordering.
- **Cohere Rerank example:** `co.rerank(query=query, documents=texts, top_n=3, return_documents=True)`; no training/tuning needed. Output relevance scores: top result 0.1698185 (the scientific accuracy sentence) vs 0.070 for second and 0.004 for third — the reranker is much more confident about the first result.
- **Two-stage search pipeline:** The first stage (first-stage retriever) shortlists results (e.g., one hundred or one thousand from thousands/millions); the reranker is the second stage. The retriever can be keyword search, dense retrieval, or better yet hybrid. Example: keyword search retrieves top 10, then rerank picks top 3 from those 10 — the reranker elevates the most relevant result (the Kip Thorne sentence) appropriately.
- **MIRACL benchmark:** On a multilingual benchmark, a reranker boosted performance from 36.5 to 62.8 (measured as nDCG@10).
- **Local retrieval/reranking:** Use the Sentence Transformers library (docs: "Retrieve & Re-Rank" section) for local setup.
- **How rerankers work:** Present the query and each result to an LLM working as a cross-encoder — query and candidate are shown simultaneously so the model views both before assigning a relevance score; all documents are processed as a batch yet each is evaluated independently. Method from "Multi-stage document ranking with BERT" (monoBERT). This is essentially a classification problem: score from 0–1 where 0 = irrelevant, 1 = highly relevant (familiar from Chapter 4 classification). Recommended reading: "Pretrained transformers for text ranking: BERT and beyond".
- **Search evaluation components:** A text archive, a set of queries, and relevance judgments (which documents are relevant for each query).
- **MAP intuition:** Count of relevant results isn't enough — position matters. Example: both systems get 1 relevant result in top 3, but system 1's is at position 1 (better) and system 2's at a lower position. Average precision quantifies this: system 1 gets AP of 1, system 2 gets 0.3.
- **AP calculation:** For a query with one relevant document at position 1 → precision at position 1 = 1.0 → perfect score of 1. If the relevant document is at position 3, precision is penalized (nonrelevant docs ahead of it reduce precision at that position). For multiple relevant documents, AP considers precision at k for all relevant documents, then averages. MAP = mean of average precisions across all queries (Figure 8-23). The "mean" vs "average" duplication is likely an aesthetic choice (MAP sounds better than "average average precision").
- **nDCG:** Normalized discounted cumulative gain — relevance is non-binary (one document can be more relevant than another), so it's more nuanced than MAP.
- **RAG motivation:** LLMs confidently answer lots of questions incorrectly (hallucinations). The leading industry remedy is RAG (2020 paper). RAG reduces hallucinations, improves factuality, and enables "chat with my data" (grounding on internal company data or a specific data source like a book). Search engines now incorporate LLMs to summarize results or answer questions (Perplexity, Microsoft Bing AI, Google Gemini).
- **From search to RAG:** Add an LLM to the end of the search pipeline; present the question + top retrieved documents, ask the LLM to answer given the context. This generation step is grounded generation — retrieved info grounds the LLM in the domain of interest.
- **Grounded generation with Cohere:** Embedding search retrieves top documents; pass them as `documents=docs_dict` to `co.chat(message=query, documents=docs_dict)`. The response includes citations: `ChatCitation(start=21, end=36, text='worldwide gross', document_ids=['doc_0'])` etc. Result: "The film generated a worldwide gross of over $677 million, or $773 million with subsequent re-releases."
- **RAG with local models:** Download Phi-3 GGUF (`Phi-3-mini-4k-instruct-fp16.gguf`), load with `LlamaCpp` (same params as Ch 7). Load embedding model with `HuggingFaceEmbeddings(model_name='thenlper/gte-small')` (the book mentions BAAI/bge-small-en-v1.5 as high on MTEB leaderboard and relatively small). Build local vector DB: `FAISS.from_texts(texts, embedding_model)`. RAG prompt template with `{context}` and `{question}` variables. Build the pipeline: `RetrievalQA.from_chain_type(llm=llm, chain_type='stuff', retriever=db.as_retriever(), chain_type_kwargs={"prompt": prompt}, verbose=True)`. `rag.invoke('Income generated')` produces a grounded answer (~$677 million, $773 million with re-releases). Prompt can be adjusted to control answer length/tone. Trade-off: lose span citations and the smaller local model won't work as well as the larger managed model.
- **Advanced RAG techniques:**
  - **Query rewriting:** Chatbot RAG struggles if a question is verbose or references previous conversation context; use an LLM to rewrite into a concise retrieval query (e.g., a rambling essay/penguin/dolphin message → "Where do dolphins live"). Done via a prompt or API call (Cohere has a dedicated query-rewriting mode for co.chat).
  - **Multi-query RAG:** Search multiple queries if more than one is needed (e.g., "Compare the financial results of Nvidia in 2020 vs. 2023" → "Nvidia 2020 financial results" + "Nvidia 2023 financial results"); present top results of both to the model. Small improvement: let the rewriter decide if no search is required (directly generate a confident answer).
  - **Multi-hop RAG:** A series of sequential queries (e.g., "Who are the largest car manufacturers in 2023? Do they each make EVs?" → query 1 "largest car manufacturers 2023" → Toyota, Volkswagen, Hyundai → follow-ups per manufacturer on electric vehicles).
  - **Query routing:** Give the model the ability to search multiple data sources (HR → Notion, customer data → CRM/Salesforce).
  - **Agentic RAG:** These enhancements delegate more responsibility to the LLM — gauging information needs and utilizing multiple data sources; data sources become tools (search Notion, but also post to Notion). Approaches an agent that acts on the world. Not all LLMs have these capabilities (likely only the largest managed models); Cohere's Command R+ excels and is available as open-weights.
- **RAG evaluation:** Good paper: "Evaluating verifiability in generative search engines" (2023, Liu, Zhang, Liang) — human evaluations on generative search systems along four axes: fluency, perceived utility, citation recall, citation precision. Human evaluation is preferred, but automation via LLM-as-a-judge scores generations along the axes. Ragas library automates this and adds metrics like faithfulness and answer relevance (formulas on the Ragas docs site).

### Important Numbers / Stats / Tokens
- BERT paper released 2018; Google announced using it months later ("one of the biggest leaps forward in the history of Search"); Bing used large transformer models from April of that year (p.1).
- Cohere embeddings of 15 sentences → shape (15, 4096): 15 vectors, each size 4,096 (p.8).
- Dense retrieval search distances for "how precise was the science": 10757.38 (top), 11566.13, 11922.83 — smaller distance = more similar (p.9).
- BM25 scores for the same query: 1.789, 1.373, 0.000 (p.10).
- Missing-answer query ("What is the mass of the moon?") still returns results with distances 1.298, 1.324, 1.328 (p.10).
- Rerank relevance scores: 0.1698185, 0.07004896, 0.0043994132 (p.17).
- MIRACL benchmark: reranker boosts nDCG@10 from 36.5 to 62.8 (p.19).
- AP example: relevant result at position 1 → score 1; system 2 gets 0.3 (p.23); perfect score 1 when only relevant doc is at top (p.23).
- RAG paper: "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (2020), Patrick Lewis et al., NeurIPS 33: 9459–9474 (p.25).
- Grounded generation citations: `ChatCitation(start=21, end=36, text='worldwide gross', document_ids=['doc_0'])`; result "$677 million ... $773 million with subsequent re-releases" (p.28).
- Local RAG: `RetrievalQA.from_chain_type(llm, chain_type='stuff', retriever=db.as_retriever(), ...)`; embedding model `thenlper/gte-small` (book suggests BAAI/bge-small-en-v1.5, high on MTEB) (p.29-30).
- Local model answer: "over $677 million worldwide ... tenth-highest grossing film of that year ... approximately $773 million" (p.30).
- RAG eval axes: fluency, perceived utility, citation recall, citation precision (p.33); Ragas metrics: faithfulness, answer relevance (p.33).
- Next chapter: multimodal LLMs (text + images) (p.34).

### Algorithms & Formulæ
- **Dense retrieval:** embed query → find nearest neighbors in embedding space (Euclidean/L2 distance via `faiss.IndexFlatL2`); nearest = smallest distance.
- **BM25 (lexical search):** tokenize (lowercase, strip punctuation, remove stop words), score passages by term frequency; `bm25.get_scores(bm25_tokenizer(query))`, `np.argpartition` to find top candidates.
- **Average precision (single query):** For each relevant document at rank k, compute precision at k (# relevant up to k / k); average those precision values over all relevant documents. Relevant doc at position 1 → precision@1 = 1/1 = 1.0; relevant doc at position 3 → precision@3 = 1/3 ≈ 0.33, etc.
- **MAP:** mean of average precision across all queries in the test suite — one number to compare search systems.
- **nDCG:** like DCG but normalized; uses graded (non-binary) relevance labels.
- **Reranking (cross-encoder/monoBERT):** for each (query, document) pair presented together, output a relevance score in [0,1] (classification); reorder results by score.
- **Fine-tuning for retrieval:** contrastive training on (query, positive doc) and (query, negative doc) pairs — pull positive query embeddings closer to the document, push negative ones farther.

### Diagrams / Visuals
- **Figure 8-1** — Dense retrieval: takes a query, consults an archive of texts, outputs relevant results via embedding similarity.
- **Figure 8-2** — Rerankers take a query + a collection of results and reorder them by relevance.
- **Figure 8-3** — A RAG system formulates an answer to a question and (preferably) cites its information sources.
- **Figure 8-4** — Embedding intuition: each text is a point; similar texts are close together.
- **Figure 8-5** — Dense retrieval relies on queries being close to their relevant results.
- **Figure 8-6** — Converting an external knowledge base to a vector database, then querying it.
- **Figure 8-7** — One vector per document vs splitting longer documents into chunks with their own embeddings (better).
- **Figure 8-8** — Several chunking methods; overlapping chunks prevent absence of context.
- **Figure 8-9** — Possible options for chunking a document for embedding.
- **Figure 8-10** — Overlapping segments to retain more context around different segments.
- **Figure 8-11** — Comparing embeddings to find the most similar documents to a query.
- **Figure 8-12** — Before fine-tuning, relevant and irrelevant queries may be equidistant from a document.
- **Figure 8-13** — After fine-tuning, relevant queries are closer and irrelevant queries farther from the document.
- **Figure 8-14** — LLM rerankers as the second stage in a two-stage search system.
- **Figure 8-15** — A reranker assigns a relevance score per document by looking at document and query simultaneously (cross-encoder).
- **Figure 8-16** — Evaluation test suite: queries + relevance judgments over the archive.
- **Figure 8-17** — Passing the same query to two systems and looking at top results.
- **Figure 8-18** — Relevance judgments show system 1 did better than system 2.
- **Figure 8-19** — Both systems get one relevant result but at different positions — position matters.
- **Figure 8-20** — Calculating precision at position 1 (relevant result at top → 1.0).
- **Figure 8-21** — Nonrelevant docs ahead of a relevant doc penalize precision.
- **Figure 8-22** — Average precision with multiple relevant documents: consider precision at k of all relevant documents.
- **Figure 8-23** — MAP = mean of the average precisions of each query.
- **Figure 8-24** — Basic RAG pipeline: search step followed by grounded generation step.
- **Figure 8-25** — Generative search formulates answers and summaries at the end of a search pipeline while citing sources.
- **Figure 8-26** — Find the most relevant information by embedding similarity; add it to the prompt before giving it to the LLM.

### Common Exam Traps
- **Semantic search = meaning-based**, not keyword matching; dense retrieval relies on embedding similarity (nearest neighbors).
- **Three categories:** dense retrieval, reranking, RAG — do not conflate reranking (reorders results) with RAG (generates grounded answers).
- **Rerankers take an extra input** vs dense retrieval: a set of results from a previous pipeline step (first-stage shortlisting).
- **Smaller distance = more similar** in dense retrieval (L2 distance); larger distance = less relevant. Rerank relevance scores are the opposite (higher = more relevant, on 0–1 scale).
- **Cohere embeddings shape (15, 4096)** — 15 sentence vectors of size 4096 (from the Interstellar example).
- **BM25 is lexical/keyword** search; its top hit may share words with the query but not answer it — dense retrieval finds the meaning-based match.
- **Dense retrieval caveats:** returns results even when no answer exists (use a threshold); bad at exact phrase matches (hybrid search advised); domain-transfer degradation (trained on internet/Wikipedia ≠ legal domain).
- **Chunking:** transformers have context limits; one vector per doc (embed title/beginning only, or average chunk vectors — compressed/lossy) vs multiple vectors per doc (chunks with full coverage, capture individual concepts — preferred). Overlapping chunks + adding the title preserve context.
- **NumPy distances** are fine for thousands/tens of thousands of vectors; **Annoy/FAISS** for millions (approximate nearest neighbor); **vector databases** (Weaviate, Pinecone) allow add/delete without rebuild + filtering.
- **Fine-tuning for retrieval optimizes text embeddings** on (query, relevant doc) positive pairs and (query, irrelevant doc) negative pairs — not token embeddings.
- **Reranker = cross-encoder/monoBERT:** query and document seen simultaneously; scores 0–1; essentially classification (from Ch 4).
- **MAP vs nDCG:** MAP averages per-query average precision (binary relevance); nDCG allows graded/non-binary relevance.
- **AP calculation:** precision@k computed only at positions of relevant documents; relevant doc at top → AP 1; lower position → penalized (e.g., system scored 1 vs 0.3).
- **RAG = search + grounded generation** (2020 paper); reduces hallucinations, improves factuality, enables "chat with my data".
- **Grounding** = providing retrieved context so the LLM stays in the domain; the prompt template has `{context}` and `{question}` variables.
- **RAG with local models:** `HuggingFaceEmbeddings` (bge-small-en-v1.5/gte-small), `FAISS.from_texts`, `RetrievalQA` with `chain_type='stuff'`; loses span citations and quality vs managed models.
- **Advanced RAG:** query rewriting (verbose → concise), multi-query (multiple searches), multi-hop (sequential queries), query routing (multiple data sources), agentic RAG (data sources as tools; Command R+ open-weights). Not all LLMs can do these.
- **RAG evaluation axes:** fluency, perceived utility, citation recall, citation precision. Ragas adds faithfulness and answer relevance; LLM-as-a-judge automates scoring. Human evaluation is preferred.
- **MIRACL** reranker boost: 36.5 → 62.8 nDCG@10.
- **Generative search examples:** Perplexity, Microsoft Bing AI, Google Gemini.

### Chapter Summary
Chapter 8 covers three ways language models upgrade search. **Dense retrieval** embeds queries and documents, retrieving nearest neighbors by embedding similarity — demonstrated with Cohere embeddings (15×4096) and FAISS, compared against BM25 keyword search, with caveats (missing answers → thresholds; exact-phrase weakness → hybrid search; domain transfer), text-chunking strategies (one vs multiple vectors per document, overlapping chunks), scalability options (NumPy → Annoy/FAISS → vector databases), and fine-tuning embedding models on query/document pairs. **Reranking** adds a second stage (cross-encoder/monoBERT) that scores a shortlisted set by relevance (Cohere Rerank; keyword top-10 → rerank top-3; MIRACL 36.5→62.8). **Retrieval evaluation** introduces mean average precision (precision@k for relevant documents, averaged per query then across queries) and nDCG for graded relevance. **RAG** adds a grounded generation step (Cohere co.chat with citations; local Phi-3 + gte-small + FAISS + RetrievalQA), with advanced techniques — query rewriting, multi-query, multi-hop, query routing, and agentic RAG — and evaluation via fluency, perceived utility, citation recall/precision (Ragas, LLM-as-a-judge). The next chapter covers multimodal LLMs.

### Confidence Check
- **Sure**: semantic search definition; dense retrieval mechanics (embed + nearest neighbors); Cohere example (15×4096, FAISS IndexFlatL2, input_type search_document/search_query); BM25 as lexical baseline and its failure vs dense; the three dense-retrieval caveats; chunking strategies (one vs multiple vectors, overlap, title); NumPy vs Annoy/FAISS vs vector databases (Weaviate/Pinecone); fine-tuning on query/doc positive + negative pairs; reranking as second stage / cross-encoder / monoBERT; MIRACL 36.5→62.8; MAP/AP/precision@k calculation and the position-1=1 / position-3 penalty example; nDCG graded relevance; RAG definition (2020 paper), grounding, Cohere citations, local-model RAG (HuggingFaceEmbeddings, FAISS.from_texts, RetrievalQA stuff chain); advanced RAG (query rewriting, multi-query, multi-hop, routing, agentic RAG, Command R+); RAG eval axes (fluency, perceived utility, citation recall, citation precision) + Ragas (faithfulness, answer relevance) + LLM-as-a-judge; generative search examples (Perplexity, Bing AI, Gemini).
- **Uncertain**: exact page anchors for figures (PDF text-flow approximate); precise wording of some quoted outputs where extraction broke lines mid-sentence; the exact citation character offsets (paraphrased from extraction).

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Imports and Cohere client setup
```python
import cohere
import numpy as np
import pandas as pd
from tqdm import tqdm
# Paste your API key here. Remember to not share publicly
api_key = ''
# Create and retrieve a Cohere API key from os.cohere.ai
co = cohere.Client(api_key)
```
- **Explanation:** Imports libraries and creates a Cohere client; the API key (from os.cohere.ai) is a placeholder that the user must paste. Warns not to share the key publicly.
- **Fits the architecture:** Cohere provides managed embedding, rerank, and chat APIs used throughout the chapter.

### Code Block 2: The Interstellar text archive
```python
text = """
Interstellar is a 2014 epic science fiction film co-written, directed, and pro
duced by Christopher Nolan. ... (Wikipedia first section)
"""
# Split into a list of sentences
texts = text.split('.')
# Clean up to remove empty spaces and new lines
texts = [t.strip(' \n') for t in texts]
```
- **Explanation:** Takes the first section of the Wikipedia article on Interstellar and splits it into cleaned sentences by splitting on periods and stripping whitespace/newlines.
- **Fits the architecture:** This is the "get text + light processing, chunk into sentences" step of dense retrieval.

### Code Block 3: Embedding the text chunks
```python
# Get the embeddings
response = co.embed(
  texts=texts,
  input_type="search_document",
).embeddings
embeds = np.array(response)
print(embeds.shape)
```
- **Explanation:** Sends the texts to Cohere's embed API with `input_type="search_document"` and converts the returned embeddings to a NumPy array.
- **Fits the architecture:** Produces a (15, 4096) array — 15 sentence vectors, each of size 4,096.

### Code Block 4: Building the search index with FAISS
```python
import faiss
dim = embeds.shape[1]
index = faiss.IndexFlatL2(dim)
print(index.is_trained)
index.add(np.float32(embeds))
```
- **Explanation:** Creates a FAISS index using L2 (Euclidean) distance for the 4,096-dim vectors, then adds the embeddings (as float32) to the index.
- **Fits the architecture:** The index is optimized to quickly retrieve nearest neighbors even for very large numbers of points.

### Code Block 5: The search function
```python
def search(query, number_of_results=3):
  # 1. Get the query's embedding
  query_embed = co.embed(texts=[query], 
                input_type="search_query",).embeddings[0]
  # 2. Retrieve the nearest neighbors
  distances , similar_item_ids = index.search(np.float32([query_embed]), number_of_results) 
  # 3. Format the results
  texts_np = np.array(texts) # Convert texts list to numpy for easier indexing
  results = pd.DataFrame(data={'texts': texts_np[similar_item_ids[0]], 
                              'distance': distances[0]})
  # 4. Print and return the results
  print(f"Query:'{query}'\nNearest neighbors:")
  return results
```
- **Explanation:** Embeds the query (with `input_type="search_query"`), searches the index for the k nearest neighbors, formats texts + distances into a pandas DataFrame, and returns it.
- **Fits the architecture:** The query is embedded into the same space as the archive; nearest documents = results (smallest distance = most similar).

### Code Block 6: Searching with a query
```python
query = "how precise was the science"
results = search(query)
results
```
- **Explanation:** Runs the query. Top result: "It has also received praise from many astronomers for its scientific accuracy..." (distance 10757.38) — which perfectly answers the query without sharing its keywords.
- **Fits the architecture:** Demonstrates semantic search's advantage over keyword search.

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
- **Explanation:** Defines a tokenizer (lowercase, strip punctuation, drop empty tokens and English stop words), tokenizes the corpus, and builds a BM25Okapi lexical search index.
- **Fits the architecture:** Provides the keyword/lexical search baseline to compare against dense retrieval.

### Code Block 8: The keyword search function
```python
def keyword_search(query, top_k=3, num_candidates=15):
    print("Input question:", query)
    ##### BM25 search (lexical search) #####
    bm25_scores = bm25.get_scores(bm25_tokenizer(query))
    top_n = np.argpartition(bm25_scores, -num_candidates)[-num_candidates:]
    bm25_hits = [{'corpus_id': idx, 'score': bm25_scores[idx]} for idx in top_n]
    bm25_hits = sorted(bm25_hits, key=lambda x: x['score'], reverse=True)
    print(f"Top-3 lexical search (BM25) hits")
    for hit in bm25_hits[0:top_k]:
        print("\t{:.3f}\t{}".format(hit['score'], texts[hit['corpus_id']].replace("\n", " ")))
```
- **Explanation:** Scores the query against the corpus with BM25, takes the top candidates, sorts descending by score, and prints the top-k hits.
- **Fits the architecture:** Its top hit for "how precise was the science" (1.789) shares the word "science" but doesn't answer the question — showing lexical search's limitation.

### Code Block 9: Keyword search + reranking function
```python
def keyword_and_reranking_search(query, top_k=3, num_candidates=10):
    print("Input question:", query)
    ##### BM25 search (lexical search) #####
    bm25_scores = bm25.get_scores(bm25_tokenizer(query))
    top_n = np.argpartition(bm25_scores, -num_candidates)[-num_candidates:]
    bm25_hits = [{'corpus_id': idx, 'score': bm25_scores[idx]} for idx in top_n]
    bm25_hits = sorted(bm25_hits, key=lambda x: x['score'], reverse=True)
    print(f"Top-3 lexical search (BM25) hits")
    for hit in bm25_hits[0:top_k]:
        print("\t{:.3f}\t{}".format(hit['score'], texts[hit['corpus_id']].replace("\n", " ")))
    # Add re-ranking
    docs = [texts[hit['corpus_id']] for hit in bm25_hits]
    print(f"\nTop-3 hits by rank-API ({len(bm25_hits)} BM25 hits re-ranked)")
    results = co.rerank(query=query, documents=docs, top_n=top_k, return_documents=True)
    for hit in results.results:
        print("\t{:.3f}\t{}".format(hit.relevance_score, hit.document.text.replace("\n", " ")))
```
- **Explanation:** First stage: BM25 shortlists the top 10 candidates. Second stage: Cohere Rerank re-ranks those 10 and returns the top 3. The reranker elevates the most relevant result (the Kip Thorne sentence) appropriately.
- **Fits the architecture:** The classic two-stage pipeline — a cheap first-stage retriever shortlists, then a reranker reorders by relevance.

### Code Block 10: Cohere Rerank (direct)
```python
query = "how precise was the science"
results = co.rerank(query=query, documents=texts, top_n=3, return_documents=True)
for idx, result in enumerate(results.results):
  print(idx, result.relevance_score , result.document.text)
```
- **Explanation:** Passes the query and all documents to Cohere's Rerank endpoint; returns top 3 with relevance scores (0.1698185, 0.07004896, 0.0043994132).
- **Fits the architecture:** No training/tuning needed — the reranker is much more confident about the top result.

### Code Block 11: Grounded generation with Cohere chat
```python
query = "income generated"
# 1- Retrieval
results = search(query)
# 2- Grounded Generation
docs_dict = [{'text': text} for text in results['texts']]
response = co.chat(
    message = query,
    documents=docs_dict
)
print(response.text)
# The film generated a worldwide gross of over $677 million, or $773 million with subsequent re-releases.
```
- **Explanation:** Uses embedding search to retrieve top documents, then passes them as `documents` to `co.chat` to produce a grounded answer. The response includes `citations=[ChatCitation(...)]` with start/end offsets, text spans, and document IDs.
- **Fits the architecture:** This is a complete RAG system — retrieval step followed by grounded generation with citations.

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
- **Explanation:** Downloads the quantized Phi-3 GGUF and loads it via LlamaCpp (same parameters as Chapter 7).
- **Fits the architecture:** Local text generation model for the RAG generation step.

### Code Block 13: Loading the embedding model
```python
from langchain.embeddings.huggingface import HuggingFaceEmbeddings
# Embedding model for converting text to numerical representations
embedding_model = HuggingFaceEmbeddings(
    model_name='thenlper/gte-small'
)
```
- **Explanation:** Loads a Sentence-Transformers embedding model (gte-small) via LangChain's HuggingFaceEmbeddings wrapper. (The book mentions BAAI/bge-small-en-v1.5 — high on MTEB leaderboard, relatively small.)
- **Fits the architecture:** Converts text to numerical representations for the local vector database.

### Code Block 14: Building the local vector database
```python
from langchain.vectorstores import FAISS
# Create a local vector database
db = FAISS.from_texts(texts, embedding_model)
```
- **Explanation:** Creates a local FAISS vector store directly from the texts using the embedding model.
- **Fits the architecture:** This is the search/retrieval index for the local RAG pipeline.

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
prompt = PromptTemplate(
    template=template,
    input_variables=["context", "question"]
)
```
- **Explanation:** The RAG prompt template is the central place that communicates the retrieved documents to the LLM via a `{context}` variable, plus the user `{question}`.
- **Fits the architecture:** The prompt grounds the LLM in the retrieved domain information.

### Code Block 16: Building the RAG pipeline
```python
from langchain.chains import RetrievalQA
# RAG pipeline
rag = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type='stuff',
    retriever=db.as_retriever(),
    chain_type_kwargs={
        "prompt": prompt
    },
    verbose=True
)
```
- **Explanation:** Builds a RetrievalQA chain that retrieves context from the FAISS store (`db.as_retriever()`) and feeds it (chain_type 'stuff') to the LLM with the custom prompt.
- **Fits the architecture:** Combines the retriever and the grounded generation LLM into one RAG pipeline.

### Code Block 17: Querying the local RAG pipeline
```python
rag.invoke('Income generated')
```
- **Explanation:** The pipeline retrieves relevant documents and generates a grounded answer (~$677 million worldwide; $773 million with re-releases; tenth-highest grossing film of 2014). The prompt can be adjusted to control answer length and tone.
- **Fits the architecture:** Demonstrates RAG locally — though without span citations and with a smaller model that doesn't perform as well as the managed model.
