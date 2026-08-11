# 📘 Practice Exam — Chapter 4: Text Classification
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 4
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What is the goal of the text classification task?
   a) Generate new text from a prompt
   b) Embed text into a vector space
   c) Assign a label or class to some input text
   d) Cluster documents into groups

2. Which of the following is NOT a text-classification application mentioned in the chapter?
   a) Sentiment analysis
   b) Detecting language
   c) Intent detection
   d) Generating a movie script

3. Which dataset is used throughout this chapter?
   a) rotten_tomatoes
   b) IMDB reviews
   c) SST-2
   d) AG News

4. How many positive and negative reviews does the Rotten Tomatoes dataset contain?
   a) 8,530 positive and 8,530 negative
   b) 5,331 positive and 5,331 negative
   c) 10,662 positive and 10,662 negative
   d) 1,066 positive and 1,066 negative

5. In the dataset, label value 1 means:
   a) Negative review
   b) Positive review
   c) Neutral review
   d) Unlabeled

6. How many rows are in the train split of the Rotten Tomatoes dataset?
   a) 8,530
   b) 1,066
   c) 5,331
   d) 10,662

7. Which model is chosen as the task-specific model for sentiment analysis?
   a) bert-tiny
   b) ALBERT base v2
   c) cardiffnlp/twitter-roberta-base-sentiment-latest
   d) google/flan-t5-small

8. The Twitter-RoBERTa sentiment model was originally fine-tuned on:
   a) Movie reviews
   b) News articles
   c) Wikipedia
   d) Tweets

9. Which embedding model is used throughout the section?
   a) sentence-transformers/all-mpnet-base-v2
   b) cardiffnlp/twitter-roberta-base
   c) google/flan-t5-small
   d) bert-base-uncased

10. In the pipeline output for the Twitter-RoBERTa model, the positive score is found at index:
    a) 0
    b) 2
    c) 1
    d) 3

11. The four metrics derived from the confusion matrix are:
    a) precision, recall, F2, and error rate
    b) accuracy, error, AUC, and ROC
    c) precision, recall, accuracy, and F1 score
    d) true positive, true negative, false positive, false negative

12. Precision measures:
    a) How many of the items found are relevant
    b) How many relevant classes were found
    c) How many correct predictions out of all predictions
    d) The balance of recall and accuracy

13. Recall measures:
    a) How many of the items found are relevant
    b) The overall correctness of the model
    c) The balance of precision and F1
    d) How many relevant classes were found

14. Which F1 average is used throughout the book to treat each class equally?
    a) Macro average
    b) Weighted average
    c) Micro average
    d) Simple average

15. The F1 score of the task-specific (Twitter-RoBERTa) model was:
    a) 0.85
    b) 0.78
    c) 0.80
    d) 0.91

16. Which model is suggested as a better task-specific choice trained on movie-review domain data?
    a) ALBERT base v2
    b) bert-tiny
    c) DeBERTa base
    d) DistilBERT base uncased fine-tuned on SST-2

17. How many models for text classification were on the Hugging Face Hub at the time of writing?
    a) Over 60,000
    b) Over 8,000
    c) Over 1,000
    d) Over 600,000

18. The shape of the train embeddings is:
    a) (768, 8530)
    b) (8530, 768)
    c) (1066, 768)
    d) (8530, 32064)

19. Which classifier is trained on top of the embeddings in the two-step approach?
    a) Random forest
    b) k-nearest neighbors
    c) SVM
    d) Logistic regression

20. The F1 score of the embedding + logistic regression approach was:
    a) 0.80
    b) 0.78
    c) 0.85
    d) 0.91

21. What is the main benefit of the two-step embedding classification approach?
    a) We do not need to fine-tune the embedding model and can train a lightweight classifier on the CPU
    b) The embedding model is trainable and updated during training
    c) It requires extensive GPU compute
    d) It requires labeled data for the embedding model

22. Which external APIs are mentioned as alternatives for generating embeddings to avoid GPU dependency?
    a) Google and Amazon
    b) Cohere's and OpenAI's offerings
    c) Microsoft and Apple
    d) Hugging Face and Kaggle

23. Zero-shot classification is used when:
    a) We have plenty of labeled data
    b) We want to fine-tune the model
    c) We have no labeled data, only the labels themselves
    d) We only have validation data

24. In zero-shot classification with embeddings, label embeddings are created by:
    a) Training a classifier on them
    b) Using a task-specific model
    c) Taking the average of all document embeddings
    d) Describing and embedding the labels (e.g., "A negative review")

25. The similarity metric used to match documents to labels in zero-shot classification is:
    a) Cosine similarity
    b) Euclidean distance
    c) Manhattan distance
    d) Jaccard similarity

26. The F1 score of the zero-shot embedding approach was:
    a) 0.85
    b) 0.80
    c) 0.78
    d) 0.91

27. Which improved label description does the chapter suggest for better zero-shot results?
    a) "A negative review" and "A positive review"
    b) "A very negative movie review" and "A very positive movie review"
    c) "Bad" and "Good"
    d) "This is a movie review"

28. Why does the chapter illustrate zero-shot with embeddings rather than natural language inference (NLI) models?
    a) NLI models do not work for sentiment
    b) NLI models require labeled data
    c) Embeddings are slower
    d) To demonstrate the flexibility of embeddings for a variety of tasks

29. Generative models are described as:
    a) Models that output a class for a given text
    b) Encoder-only models like BERT
    c) Sequence-to-sequence models that take text and generate text
    d) Models trained only on labeled data

30. The T5 model architecture consists of:
    a) 6 decoders and 6 encoders
    b) 12 decoders and 12 encoders
    c) 24 decoders only
    d) 12 encoders only

31. During T5 pretraining, what is masked?
    a) Individual tokens only
    b) Whole documents
    c) Only special tokens
    d) Sets of tokens (token spans)

32. What resulted in the Flan-T5 family of models?
    a) Instruction fine-tuning with more than a thousand tasks
    b) Pretraining on masked language modeling only
    c) Fine-tuning on a single task
    d) Using only encoder blocks

33. Which Flan-T5 model size is used to speed things up?
    a) flan-t5-xxl
    b) flan-t5-large
    c) flan-t5-small
    d) flan-t5-base

34. The prompt prefix used for Flan-T5 classification is:
    a) "Classify this review:"
    b) "Is the following sentence positive or negative?"
    c) "Sentiment analysis:"
    d) "Predict the label:"

35. How is Flan-T5's textual output converted to numerical values?
    a) "negative" maps to 0, "positive" maps to 1
    b) "negative" maps to 1, "positive" maps to 0
    c) The output is used directly without mapping
    d) A logistic regression converts the text

36. The F1 score of the Flan-T5-small model was:
    a) 0.78
    b) 0.85
    c) 0.91
    d) 0.84

37. The architecture of the original ChatGPT model (GPT-3.5) is assumed to be:
    a) Encoder-only
    b) Decoder-only
    c) Encoder-decoder
    d) Unknown and unmentioned

38. OpenAI's training procedure first used which data to create a first model variant?
    a) Preference data (ranked outputs)
    b) Unlabeled data
    c) Instruction data (manually created desired outputs)
    d) The Rotten Tomatoes dataset

39. Which API parameter is set to 0 in the chatgpt_generation function?
    a) temperature
    b) max_tokens
    c) top_p
    d) seed

40. What was the F1 score of the ChatGPT (gpt-3.5-turbo-0125) model?
    a) 0.84
    b) 0.85
    c) 0.80
    d) 0.91

---

## Section B: True/False (1 point each)

41. The Rotten Tomatoes dataset is hosted on the Hugging Face Hub. (T/F)
42. The Rotten Tomatoes dataset contains 8,530 positive and 8,530 negative reviews. (T/F)
43. In the confusion matrix, the four combinations are based on True/False and Positive/Negative. (T/F)
44. The F1 score balances both precision and recall. (T/F)
45. The task-specific Twitter-RoBERTa model was specifically trained on movie reviews. (T/F)
46. Encoder-only models like BERT tend to be significantly smaller in size than generative models like GPT. (T/F)
47. The embedding model is trainable and updated during the logistic regression training. (T/F)
48. Cosine similarity is calculated as the dot product of the embeddings divided by the product of their lengths. (T/F)
49. Zero-shot classification requires at least some labeled data. (T/F)
50. The zero-shot approach achieved an F1 score of 0.78 without using any labeled data. (T/F)
51. T5 is a decoder-only architecture. (T/F)
52. Flan-T5 was fine-tuned on more than a thousand tasks that follow instructions like GPT models. (T/F)
53. Generative models can perform classification out of the box without any prompt. (T/F)
54. The chapter uses the test split for validating the results. (T/F)
55. Running the test dataset through gpt-3.5-turbo-0125 cost 3 cents at the time of writing. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Describe the Rotten Tomatoes dataset: source, composition, label meaning, and the three splits with their sizes.
57. Explain the difference between a task-specific model and an embedding model.
58. Define precision, recall, accuracy, and the F1 score, and state which average of F1 the book uses.
59. Walk through the two-step embedding classification approach (models, shapes, classifier, and result).
60. Explain how zero-shot classification with embeddings works, including the label-description trick and the similarity metric.
61. What was the F1 result of the zero-shot approach, and why is it impressive?
62. Why are generative models called sequence-to-sequence models, and why do they need prompts?
63. Describe the T5 architecture and its two training steps (pretraining and fine-tuning).
64. Explain the two-step training procedure OpenAI used to create ChatGPT (instruction data and preference data).
65. What caveat prevents reliable evaluation of the ChatGPT results, and how much did the test run cost?

---

## Section D: Essay / Applied (5 points each)

66. **Representation models for classification.** Compare the task-specific model approach (Twitter-RoBERTa) with the embedding + logistic regression approach. Include the models used, how each works, the two-step nature of the second approach, the shapes involved, and the F1 results (0.80 vs 0.85). Discuss model selection considerations from the Hugging Face Hub.
67. **Zero-shot classification with embeddings.** Explain why labeled data is a bottleneck, what zero-shot classification is, and how the label-description + cosine similarity trick works. Include the label descriptions used, how the embeddings are computed, how the best label is chosen, the F1 result, and how improving the label descriptions can improve results.
68. **Generative models for classification: Flan-T5.** Describe why generative models need prompts, the T5 encoder-decoder architecture, its pretraining (span masking) and fine-tuning (text-to-text multitask), how instruction fine-tuning produced Flan-T5, and the full Flan-T5 classification workflow (pipeline, prompt prefix, output mapping) with the F1 result.
69. **Closed-source classification with ChatGPT.** Explain how ChatGPT was trained (instruction data → first variant; preference data → final model), how it is accessed via the OpenAI API (client, chatgpt_generation function, prompt template, temperature), the cost and rate-limit considerations (exponential backoff), the F1 result of 0.91, and the caveat about unknown training data.
70. **Evaluating classification models.** Using the confusion matrix, explain how precision, recall, accuracy, and F1 are derived, how to read a classification report (per-class rows, support, macro vs weighted averages), and compare the F1 results across all five methods in the chapter (0.80, 0.85, 0.78, 0.84, 0.91). Discuss why the weighted average is used throughout the book.

---

## ANSWER KEY

### Section A: Multiple Choice
1. c
2. d
3. a
4. b
5. b
6. a
7. c
8. d
9. a
10. b
11. c
12. a
13. d
14. b
15. c
16. d
17. a
18. b
19. d
20. c
21. a
22. b
23. c
24. d
25. a
26. c
27. b
28. d
29. c
30. b
31. d
32. a
33. c
34. b
35. a
36. d
37. b
38. c
39. a
40. d

### Section B: True/False
41. **T** — The dataset is loaded from the Hugging Face Hub via `load_dataset("rotten_tomatoes")`.
42. **F** — It contains 5,331 positive and 5,331 negative reviews.
43. **T** — The confusion matrix has four combinations (True/False × Positive/Negative).
44. **T** — F1 balances precision and recall.
45. **F** — It was fine-tuned on tweets; it was NOT trained on movie reviews.
46. **T** — Encoder-only models tend to be significantly smaller than generative models.
47. **F** — The embedding model is kept frozen; only the classifier is trainable.
48. **T** — Cosine similarity = dot product / (product of lengths).
49. **F** — Zero-shot uses no labeled data, only label descriptions.
50. **T** — Zero-shot achieved F1 0.78 with no labeled data.
51. **F** — T5 is an encoder-decoder (text-to-text) architecture.
52. **T** — Flan-T5 was instruction-fine-tuned on 1,000+ tasks.
53. **F** — Generative models need instructions/prompts; they don't classify out of the box.
54. **T** — The train split trains; the test split validates results.
55. **T** — Running the test set through gpt-3.5-turbo-0125 cost 3 cents at writing.

### Section C: Short Answer (model answers)
56. **Rotten Tomatoes.** From the Hugging Face Hub (`rotten_tomatoes`). 5,331 positive + 5,331 negative reviews. Label 1 = positive, 0 = negative. Splits: train 8,530, validation 1,066, test 1,066.
57. **Task-specific vs embedding model.** A task-specific model (e.g., BERT) is trained for a specific task like sentiment analysis and used directly. An embedding model generates general-purpose embeddings usable for many tasks (e.g., semantic search), which can feed a separate classifier.
58. **Metrics.** Precision = how many items found are relevant; Recall = how many relevant classes were found; Accuracy = correct predictions out of all predictions; F1 = balance of precision and recall. The book uses the weighted average of F1.
59. **Two-step embedding classification.** (1) `SentenceTransformer("sentence-transformers/all-mpnet-base-v2")` encodes text to embeddings — train shape (8530, 768); (2) `LogisticRegression(random_state=42)` trains on train embeddings and predicts test embeddings. Result: F1 0.85.
60. **Zero-shot with embeddings.** Describe each label (e.g., "A negative review", "A positive review"), embed the descriptions, compute cosine similarity between document and label embeddings, and assign the label with highest similarity (`np.argmax(sim_matrix, axis=1)`).
61. **Zero-shot result.** F1 0.78 — impressive because it used no labeled data at all, only label descriptions.
62. **Sequence-to-sequence.** Generative models take text (a token sequence) as input and generate text (a token sequence) as output. They need prompts because they're trained on a wide variety of tasks and don't perform your use case out of the box.
63. **T5.** Encoder-decoder architecture (12 encoders + 12 decoders). Pretrained with masked language modeling on token spans; fine-tuned by converting each task to a sequence-to-sequence task and training simultaneously.
64. **ChatGPT training.** First, manually created instruction data (prompt → desired output) created a first model variant via instruction-tuning. Then the model generated multiple outputs that were manually ranked best-to-worst (preference data), used to create the final model ChatGPT.
65. **ChatGPT caveat/cost.** We don't know what data the model was trained on — it might have been trained on our dataset — so the metrics can't reliably evaluate the model. Running the test set cost ~3 cents at writing.

### Section D: Essay (grading notes)
66. **Expect** Twitter-RoBERTa used directly via pipeline (negative index 0, positive index 2, argmax) → F1 0.80; embedding model all-mpnet-base-v2 (shape (8530, 768)) + frozen embeddings + logistic regression → F1 0.85; model selection (60K+ Hub models, baselines list, MTEB leaderboard, inference speed).
67. **Expect** labeled data bottleneck (human labor); zero-shot definition (no labeled data, only labels); label descriptions embedded; cosine similarity (dot product / product of lengths); argmax label choice; F1 0.78; improved descriptions "A very negative/positive movie review".
68. **Expect** generative models need prompts; T5 encoder-decoder 12+12; span masking pretraining; text-to-text multitask fine-tuning; instruction fine-tuning >1,000 tasks → Flan-T5; pipeline `text2text-generation` google/flan-t5-small; prompt prefix "Is the following sentence positive or negative?"; output mapping negative→0/positive→1; F1 0.84.
69. **Expect** instruction data → first variant; preference data (ranked outputs) → final ChatGPT; OpenAI API client + chatgpt_generation (messages, [DOCUMENT] replacement, temperature=0); prompt template returning 1/0; ~3 cents cost; rate limits + exponential backoff; F1 0.91; unknown-training-data caveat.
70. **Expect** confusion matrix (four combos); precision/recall/accuracy/F1 definitions; classification report reading (per-class rows, support, accuracy, macro/weighted avg); comparison of all five F1 results (0.80 task-specific, 0.85 embeddings+LR, 0.78 zero-shot, 0.84 Flan-T5, 0.91 ChatGPT); weighted average rationale (treat each class equally).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely evaluation metrics or the T5/ChatGPT training procedures).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
