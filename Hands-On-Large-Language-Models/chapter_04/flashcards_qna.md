# 📘 Chapter 4 Flashcards: Text Classification
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 4

---

## Part 1: Terms → Definitions

**Q:** What is text classification?
**A:** Training a model to assign a label or class to some input text.

**Q:** Name four applications of text classification.
**A:** Sentiment analysis, intent detection, extracting entities, and detecting language.

**Q:** What is a task-specific model?
**A:** A representation model (e.g., BERT/RoBERTa) trained for a specific task like sentiment analysis.

**Q:** What is an embedding model?
**A:** A model generating general-purpose embeddings usable for a variety of tasks, not limited to classification (e.g., semantic search).

**Q:** What is a foundation model?
**A:** A pretrained base model (e.g., BERT) mostly intended to be fine-tuned on a downstream task.

**Q:** What does "frozen (nontrainable)" mean for a model?
**A:** Its weights are kept fixed; only its output is used (no weight updates during training).

**Q:** What is fine-tuning?
**A:** Training a foundation model on a specific downstream task (creating task-specific or embedding models).

**Q:** What is the confusion matrix?
**A:** A matrix describing the four types of predictions (correct/incorrect × positive/negative).

**Q:** What is precision?
**A:** How many of the items found are relevant — the accuracy of the relevant results.

**Q:** What is recall?
**A:** How many relevant classes were found — the ability to find all relevant results.

**Q:** What is accuracy?
**A:** How many correct predictions the model makes out of all predictions — overall correctness.

**Q:** What is the F1 score?
**A:** A metric that balances precision and recall to create a model's overall performance.

**Q:** Which average does the book use for the F1 score, and why?
**A:** The weighted average, to make sure each class is treated equally.

**Q:** What is zero-shot classification?
**A:** Predicting the labels of input text even though the model was not trained on those labels — no labeled data, only the labels themselves.

**Q:** What is cosine similarity?
**A:** The cosine of the angle between two vectors/embeddings — the dot product of the embeddings divided by the product of their lengths.

**Q:** What is a sequence-to-sequence model?
**A:** A model that takes a sequence of tokens as input and generates a sequence of tokens as output (encoder-decoder and decoder-only generative models).

**Q:** What is prompt engineering?
**A:** Iteratively improving your prompt to get your preferred output from a generative model.

**Q:** What is the Text-to-Text Transfer Transformer (T5)?
**A:** An encoder-decoder model family where every task is converted to a sequence-to-sequence (text-to-text) task; 12 encoders + 12 decoders stacked (T5-base).

**Q:** How was T5 pretrained?
**A:** With masked language modeling where sets of tokens (token spans) were masked, instead of individual tokens.

**Q:** How is T5 fine-tuned?
**A:** Each task is converted to a sequence-to-sequence task and trained simultaneously on a wide variety of tasks.

**Q:** What is Flan-T5?
**A:** A T5 family extended by "Scaling instruction-finetuned language models" (Chung et al.), trained on more than a thousand instruction tasks.

**Q:** What are the Flan-T5 sizes?
**A:** flan-t5-small/base/large/xl/xxl.

**Q:** What is instruction data?
**A:** Manually created desired outputs to input prompts, used to create a first variant of a model.

**Q:** What is preference data?
**A:** Manually ranked outputs (best to worst) demonstrating a preference for certain outputs; used to create ChatGPT.

**Q:** Why is preference data beneficial over instruction data?
**A:** It represents nuance — by showing the difference between a good and better output, the model learns to generate text that resembles human preference.

**Q:** What is the assumed architecture of GPT-3.5/ChatGPT?
**A:** Decoder-only (based on the GPT family), though the architecture is not officially shared.

**Q:** What is a rate limit error?
**A:** An error that appears when you call an API too often, since some APIs limit usage per minute or hour.

**Q:** What is exponential backoff?
**A:** A retry method that sleeps briefly on each rate-limit error, retries the request, and increases the sleep length on each failure until success or a maximum number of retries.

**Q:** What is the Rotten Tomatoes dataset?
**A:** A Hugging Face Hub dataset of 5,331 positive and 5,331 negative movie reviews (binary sentiment).

**Q:** What is the MTEB leaderboard?
**A:** A leaderboard of open and closed source embedding models benchmarked across several tasks.

**Q:** What is the two-step embedding classification approach?
**A:** Step 1: a frozen embedding model converts text to embeddings; Step 2: a trainable classifier (e.g., logistic regression) trains on those embeddings as features.

**Q:** What is TF-IDF?
**A:** A classic text representation baseline the book advises comparing LLM results against (with a logistic regression classifier on top).

**Q:** What is the `text2text-generation` pipeline task?
**A:** The Hugging Face pipeline task generally reserved for encoder-decoder models like T5/Flan-T5.

**Q:** What does `return_all_scores=True` do in a pipeline?
**A:** Returns the scores for all classes rather than just the top one.

**Q:** What is the KeyDataset helper?
**A:** A `transformers.pipelines.pt_utils` utility for iterating over a dataset column with a pipeline.

**Q:** What is the classification report?
**A:** A scikit-learn report showing precision, recall, and F1 per class, plus accuracy and macro/weighted averages.

**Q:** What is the embedding dimension?
**A:** The number of numerical values per embedding vector (e.g., 768 for all-mpnet-base-v2).

**Q:** What are the BERT-like model variants mentioned?
**A:** RoBERTa, DistilBERT, ALBERT, and DeBERTa.

**Q:** What solid baseline models does the book list?
**A:** BERT base (uncased), RoBERTa base, DistilBERT base (uncased), DeBERTa base, bert-tiny, and ALBERT base v2.

**Q:** What is a label description (in zero-shot embedding classification)?
**A:** A textual description of what a label should represent (e.g., "A negative review"), which is embedded to stand in for labeled data.

---

## Part 2: Short Answer

**Q:** Describe the Rotten Tomatoes dataset: source, size, splits, and label meaning.
**A:** Hugging Face Hub `rotten_tomatoes`; 5,331 positive + 5,331 negative reviews; train 8,530 / validation 1,066 / test 1,066; label 1 = positive, 0 = negative (binary sentiment).

**Q:** What are the two flavors of representation-model classification?
**A:** (1) A task-specific model used directly for classification; (2) an embedding model whose embeddings feed a separately trained classifier.

**Q:** Which task-specific model was chosen and why?
**A:** `cardiffnlp/twitter-roberta-base-sentiment-latest` — a RoBERTa model fine-tuned on tweets, chosen to explore how it generalizes to movie reviews.

**Q:** Which embedding model was used and why?
**A:** `sentence-transformers/all-mpnet-base-v2` — small but performant, selected with inference speed in mind (per the MTEB leaderboard).

**Q:** How were scores extracted from the task-specific pipeline?
**A:** `negative_score = output[0]["score"]`, `positive_score = output[2]["score"]`, then `np.argmax([negative_score, positive_score])` assigned the class.

**Q:** Define the four metrics using the confusion matrix.
**A:** Precision = relevant items found / items found; Recall = relevant items found / all relevant items; Accuracy = correct predictions / all predictions; F1 = balance of precision and recall.

**Q:** What were the F1 results for each method in the chapter?
**A:** Task-specific Twitter-RoBERTa: 0.80; embeddings + logistic regression: 0.85; zero-shot embeddings: 0.78; Flan-T5-small: 0.84; ChatGPT (gpt-3.5-turbo-0125): 0.91.

**Q:** How does the two-step embedding classification work?
**A:** (1) `SentenceTransformer("sentence-transformers/all-mpnet-base-v2")` encodes train/test text to 768-dim embeddings `(8530, 768)`; (2) `LogisticRegression(random_state=42)` trains on train embeddings and predicts on test embeddings.

**Q:** What is the benefit of the two-step approach?
**A:** No costly embedding fine-tuning — a lightweight classifier (e.g., logistic regression) trains on the CPU while the embedding model stays frozen.

**Q:** How can the GPU dependency of sentence-transformers be removed?
**A:** Use an external embedding API — popular choices are Cohere's and OpenAI's offerings — letting the pipeline run entirely on the CPU.

**Q:** How does zero-shot classification with embeddings work?
**A:** Describe each label (e.g., "A negative review", "A positive review"), embed the descriptions, compute cosine similarity between document and label embeddings, and assign the label with highest similarity (`np.argmax(sim_matrix, axis=1)`).

**Q:** How can zero-shot label descriptions be improved?
**A:** Make them more concrete/specific to the data — e.g., "A very negative/positive movie review" — so the embedding captures it's a movie review and focuses on the extremes.

**Q:** Why does the book use embeddings for the zero-shot example instead of natural language inference models?
**A:** NLI models are great for zero-shot, but the example demonstrates the flexibility of embeddings, which are vital and underestimated across most Language AI use cases.

**Q:** Why are generative models considered sequence-to-sequence models?
**A:** They take text (a sequence of tokens) as input and generate text (a sequence of tokens) as output, in contrast to task-specific models that output a class.

**Q:** Why must generative models be prompted rather than fed a review directly?
**A:** They are trained on a wide variety of tasks and don't perform your use case out of the box; the instruction/prompt guides them toward the answer.

**Q:** Describe the T5 architecture and training.
**A:** Encoder-decoder like the original Transformer (12 encoders + 12 decoders); pretrained with masked language modeling on token spans; fine-tuned by converting each task into a text-to-text task trained simultaneously.

**Q:** How was Flan-T5 created?
**A:** The instruction fine-tuning method from "Scaling instruction-finetuned language models" introduced more than a thousand tasks during fine-tuning, resulting in the Flan-T5 family.

**Q:** How does Flan-T5 classify the reviews?
**A:** Prefix each document with the prompt "Is the following sentence positive or negative?", run `pipeline("text2text-generation", model="google/flan-t5-small")`, map generated "negative" → 0 and "positive" → 1.

**Q:** Describe OpenAI's two-step training procedure for ChatGPT.
**A:** (1) Manually created instruction data (prompt → desired output) produced a first model variant; (2) the model generated multiple outputs manually ranked best→worst (preference data), used to create the final ChatGPT model.

**Q:** How is ChatGPT used for classification here?
**A:** Via the OpenAI API — `openai.OpenAI(api_key=...)` client; a `chatgpt_generation` helper builds system + user messages with `[DOCUMENT]` replaced, calls `client.chat.completions.create(..., temperature=0)`, and returns the assistant content; a prompt template instructs returning 1 (positive) or 0 (negative) only.

**Q:** What cost does the chapter report for running the test set through GPT?
**A:** ~3 cents at time of writing using gpt-3.5-turbo-0125, covered by the free account.

**Q:** How are rate-limit errors handled?
**A:** Retrying with exponential backoff — sleep briefly on each rate-limit error, retry, and increase the sleep length on each failure until success or a maximum retries.

**Q:** Why can't GPT's F1 of 0.91 be used to reliably evaluate the model?
**A:** We don't know what data the model was trained on — it might have been trained on our dataset, so the metrics don't necessarily measure generalization.

**Q:** What does the chapter advise for baselines?
**A:** Compare LLM examples against classic strong baselines like TF-IDF + logistic regression.

**Q:** What comes next in Chapter 5?
**A:** Unsupervised classification — clustering data and naming clusters with topic modeling, for textual data without labels.

---

## Part 3: Fill-in-the-Blank

**Q:** The Rotten Tomatoes dataset contains ______ positive and ______ negative reviews.
**A:** 5,331; 5,331.

**Q:** The dataset splits are train ______, validation ______, test ______.
**A:** 8,530; 1,066; 1,066.

**Q:** Label ______ = positive; label ______ = negative.
**A:** 1; 0.

**Q:** There are over ______ models on the Hugging Face Hub for text classification and more than ______ embedding models.
**A:** 60,000; 8,000.

**Q:** The task-specific model used was ______.
**A:** cardiffnlp/twitter-roberta-base-sentiment-latest.

**Q:** The embedding model used was ______.
**A:** sentence-transformers/all-mpnet-base-v2.

**Q:** In the Twitter-RoBERTa pipeline, output index ______ is the negative score and index ______ is the positive score.
**A:** 0; 2.

**Q:** The F1 score of the task-specific model was ______.
**A:** 0.80.

**Q:** `train_embeddings.shape` was ______.
**A:** (8530, 768).

**Q:** The two-step approach used a ______ classifier with random_state=42.
**A:** LogisticRegression.

**Q:** The F1 score of the embedding + logistic regression method was ______.
**A:** 0.85.

**Q:** The zero-shot label embeddings were created from "A negative review" and ______.
**A:** "A positive review".

**Q:** Cosine similarity equals the ______ of the embeddings divided by the product of their lengths.
**A:** Dot product.

**Q:** The zero-shot F1 score was ______.
**A:** 0.78.

**Q:** The suggested improved label descriptions were "______ review" and "______ review".
**A:** A very negative; a very positive (movie).

**Q:** The generative model Flan-T5 was loaded with the ______ pipeline task.
**A:** text2text-generation.

**Q:** The Flan-T5 model used was ______.
**A:** google/flan-t5-small.

**Q:** The prompt prefix for Flan-T5 was "______?".
**A:** Is the following sentence positive or negative?

**Q:** The Flan-T5 textual output "negative" was mapped to ______ and "positive" to ______.
**A:** 0; 1.

**Q:** The Flan-T5 F1 score was ______.
**A:** 0.84.

**Q:** T5-base stacks ______ encoders and ______ decoders.
**A:** 12; 12.

**Q:** T5 pretraining masked ______ of tokens rather than individual tokens.
**A:** Sets (spans).

**Q:** Flan-T5 fine-tuning introduced more than ______ tasks.
**A:** A thousand (1,000).

**Q:** ChatGPT was created from ______ data (first variant) and ______ data (final model).
**A:** Instruction; preference.

**Q:** The ChatGPT classification call used temperature ______.
**A:** 0.

**Q:** The ChatGPT prompt template instructed returning ______ for positive and ______ for negative.
**A:** 1; 0.

**Q:** The ChatGPT F1 score was ______.
**A:** 0.91.

**Q:** Running the test set through gpt-3.5-turbo-0125 cost about ______ cents at writing.
**A:** 3.

**Q:** The retry strategy for rate-limit errors is called ______.
**A:** Exponential backoff.

**Q:** The four metrics from the confusion matrix are precision, recall, accuracy, and ______.
**A:** F1 score.

**Q:** The book uses the ______ average of F1 to treat each class equally.
**A:** Weighted.
