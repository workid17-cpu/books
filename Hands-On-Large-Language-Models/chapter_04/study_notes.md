# 📘 Chapter 4 Study Bundle: Text Classification
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 4

---

## §1. Study Notes

### Core Theme
This chapter is an accessible introduction to using pretrained language models for text classification — assigning a label/class to input text (e.g., sentiment analysis). It covers two families of models: representation models (a task-specific model fine-tuned for sentiment; and an embedding model whose frozen embeddings feed a separately trained classifier) and generative models (open-source Flan-T5, an encoder-decoder text-to-text model, and closed-source ChatGPT/GPT-3.5 via the OpenAI API). The chapter uses the Rotten Tomatoes movie-review dataset, introduces model selection on the Hugging Face Hub, evaluates everything with a classification report (precision, recall, accuracy, F1), and demonstrates zero-shot classification using label descriptions + cosine similarity with embeddings.

### Key Definitions
- **Classification**: A common NLP task — training a model to assign a label or class to some input text.
- **Text classification applications**: Sentiment analysis, intent detection, extracting entities, detecting language.
- **Representation model (nongenerative)**: A model like BERT (encoder-only) that produces embeddings/representations rather than generated text; used for classification either as a task-specific model or an embedding model.
- **Generative model**: A model (e.g., GPT, T5) that generates sequences of tokens from sequences of tokens (sequence-to-sequence).
- **Task-specific model**: A representation model (e.g., BERT/RoBERTa) trained for a specific task like sentiment analysis (fine-tuned on a downstream task).
- **Embedding model**: Generates general-purpose embeddings usable for a variety of tasks, not limited to classification (e.g., semantic search — Ch 8). Fine-tuning an embedding model is covered in Ch 10.
- **Foundation model**: A pretrained base model (e.g., BERT) intended to be fine-tuned on a downstream task.
- **Model selection**: Choosing the right model — considering language compatibility, underlying architecture, size, and performance; there are 60,000+ text-classification models and 8,000+ embedding models on the Hub.
- **Fine-tuning**: Training a foundation model on a specific downstream task (creating task-specific/embedding models); covered in Ch 11 (BERT classification) and Ch 10 (embeddings).
- **Frozen (nontrainable) model**: A model whose weights are kept fixed; only its output is used.
- **Confusion matrix**: A matrix describing the four types of predictions: true positive, true negative, false positive, false negative.
- **Precision**: How many of the items found are relevant — the accuracy of the relevant results.
- **Recall**: How many relevant classes were found — the ability to find all relevant results.
- **Accuracy**: How many correct predictions the model makes out of all predictions — overall correctness.
- **F1 score**: Balances precision and recall to create a model's overall performance.
- **Weighted average (macro/weighted avg)**: Used for the F1 score throughout the book to make sure each class is treated equally.
- **Zero-shot classification**: Predicting labels of input text even though the model was not trained on those labels — no labeled data, only the labels themselves.
- **Cosine similarity**: The cosine of the angle between two vectors/embeddings — the dot product of the embeddings divided by the product of their lengths; used to match documents to label descriptions.
- **Embedding dimension**: The number of numerical values per embedding vector (e.g., 768 for all-mpnet-base-v2).
- **Sequence-to-sequence model**: A model that takes a sequence of tokens as input and generates a sequence of tokens as output (encoder-decoder and decoder-only generative models).
- **Prompt engineering**: Iteratively improving your prompt to get your preferred output from a generative model.
- **Text-to-Text Transfer Transformer (T5)**: An encoder-decoder model family where every task is converted to a sequence-to-sequence (text-to-text) task; 12 decoders and 12 encoders stacked (T5-base structure).
- **Masked language modeling / span masking**: T5's pretraining masked sets of tokens (token spans) instead of individual tokens.
- **Instruction tuning**: Converting specific tasks to textual instructions and training simultaneously on many tasks.
- **Flan-T5**: The T5 family extended by "Scaling instruction-finetuned language models" (Chung et al.), introducing more than a thousand tasks during fine-tuning that follow instructions like GPT models; sizes small/base/large/xl/xxl.
- **Preference tuning / preference data**: Manually ranked outputs (best to worst) demonstrating a preference for certain outputs; used to create ChatGPT. Benefits: learns to generate text resembling human preference, with nuance.
- **Instruction data**: Manually created desired outputs to input prompts, used to create a first variant of the model.
- **Exponential backoff**: A retry method for rate-limit errors — sleep briefly on each rate-limit error, retry, and increase the sleep length until the request succeeds or a maximum retries is reached.
- **Rate limit**: Some APIs limit how often you can call them per minute or hour.

### Core Concepts & Frameworks
- **The dataset**: Rotten Tomatoes movie reviews from the Hugging Face Hub (`load_dataset("rotten_tomatoes")`). Contains 5,331 positive and 5,331 negative reviews (10,662 total); splits: train 8,530, validation 1,066, test 1,066. Label 1 = positive, 0 = negative → binary sentiment classification. (Source: Bo Pang & Lillian Lee, "Seeing stars", 2005.)
- **Train/test/validation roles**: Train split used to train; test split for validating results; the validation split can further validate generalization if you used train/test for hyperparameter tuning.
- **TF-IDF + logistic regression baseline**: The book advises comparing LLM examples against classic strong baselines (TF-IDF representation + logistic regression classifier).
- **Two flavors of representation-model classification**: (1) task-specific model (e.g., Twitter-RoBERTa-base Sentiment) used directly via pipeline; (2) embedding model (e.g., all-mpnet-base-v2) producing features that feed a separately trained classifier (two-step approach).
- **BERT-like foundation models timeline**: RoBERTa, DistilBERT, ALBERT, DeBERTa — various contexts; all foundation models meant to be fine-tuned. Solid baseline starting points: BERT base (uncased), RoBERTa base, DistilBERT base (uncased), DeBERTa base, bert-tiny, ALBERT base v2.
- **Model selection practicalities**: Trying thousands of Hub models isn't feasible — pick solid baselines; for embeddings consult the MTEB leaderboard; consider inference speed, not just performance (all-mpnet-base-v2 is small but performant).
- **Tokens handle unseen words**: A major benefit of tokens is they can be combined to generate representations even if they weren't in the training data (breaking an unknown word into tokens still yields embeddings) — Figure 4-7.
- **Twitter-RoBERTa example**: `cardiffnlp/twitter-roberta-base-sentiment-latest` — RoBERTa fine-tuned on tweets for sentiment. Output ordering in the pipeline: index 0 = negative, index 2 = positive (used `negative_score = output[0]["score"]`, `positive_score = output[2]["score"]`, then `np.argmax([neg, pos])`). Interesting to see how it generalizes to movie reviews (it wasn't trained on them). Result: F1 0.80 (weighted avg).
- **Classification report metrics**: precision, recall, accuracy, F1; report shows per-class rows + accuracy + macro avg + weighted avg. For the task-specific model: Neg precision 0.76/recall 0.88/F1 0.81; Pos 0.86/0.72/0.78; accuracy 0.80.
- **Improving the task-specific model**: choose a model trained on domain data (e.g., DistilBERT base uncased fine-tuned on SST-2, which uses movie reviews) or switch to embedding models.
- **Two-step supervised classification with embeddings**: Step 1 — frozen embedding model converts text to embeddings (sentence-transformers `model.encode`); Step 2 — train a trainable classifier (logistic regression, `random_state=42`) on the embeddings as features. Benefit: no costly embedding fine-tuning; classifier trains on CPU. Embeddings shape `(8530, 768)` — 8,530 documents × 768 dims. Result: F1 0.85.
- **GPU-dependency note**: sentence-transformers benefits from a GPU; alternatively use external APIs (Cohere's, OpenAI's offerings) to create embeddings, allowing the pipeline to run entirely on the CPU.
- **Zero-shot with embeddings**: When you have no labeled data, describe each label (e.g., "A negative review", "A positive review"), embed the descriptions with the same embedding model, then compute cosine similarity between each document embedding and each label embedding; the label with highest similarity is assigned (`np.argmax(sim_matrix, axis=1)`). Result: F1 0.78 — impressive with no labeled data at all.
- **Zero-shot label-description trick**: Make label descriptions more concrete/specific to the data (e.g., "A very negative/positive movie review") so the embedding captures that it's a movie review and focuses on the extremes — can improve results.
- **Why embeddings for zero-shot (not NLI)**: Although natural-language-inference models are amazing for zero-shot classification, the example demonstrates the flexibility of embeddings across tasks; embeddings are an underestimated but vital component of most Language AI use cases.
- **Generative classification differs**: Generative models are sequence-to-sequence; a task-specific model outputs a class (numerical values) while a generative model generates sequences of tokens from sequences of tokens. Generative models generally don't perform your use case out of the box — you guide them with instructions/prompts (prompt engineering).
- **T5 family**: Original Transformer architecture is encoder-decoder; T5 stacks 12 decoders and 12 encoders. Pretrained with masked language modeling on token spans; then fine-tuned by converting each task to a sequence-to-sequence task and training simultaneously on many tasks.
- **Flan-T5 usage**: `pipeline("text2text-generation", model="google/flan-t5-small")`; the small variant is used for speed. Prefix each document with the prompt "Is the following sentence positive or negative?" (stored in a new `t5` column). Convert textual output to numbers: "negative" → 0, else 1. Result: F1 0.84.
- **ChatGPT (GPT-3.5)**: Closed source; architecture (not shared) assumed decoder-only like GPT. OpenAI's training procedure: (1) instruction data (manually created desired outputs to prompts) → first model variant via instruction-tuning; (2) generate multiple outputs, manually ranked best→worst (preference data) → final model ChatGPT.
- **ChatGPT classification**: Access via OpenAI API (needs account + API key); `openai.OpenAI(api_key=...)` client; `chatgpt_generation(prompt, document, model="gpt-3.5-turbo-0125")` builds system + user messages (system: "You are a helpful assistant."; user: prompt with `[DOCUMENT]` replaced by the document), calls `client.chat.completions.create(messages=messages, model=model, temperature=0)`, returns `choices[0].message.content`. Prompt template instructs: predict positive/negative movie review; return 1 if positive, 0 if negative; no other answers.
- **API cost/rate limits**: Running the test dataset with gpt-3.5-turbo-0125 cost ~3 cents at writing (covered by free account). Track usage; APIs can become costly. Rate-limit errors handled with retries, including exponential backoff (short sleep, retry, increase sleep on each failure until success or max retries).
- **GPT results**: F1 0.91 (Neg 0.92, Pos 0.91, accuracy 0.91). Caveat: since we don't know what data the model was trained on, we can't easily use these metrics to evaluate — it might have been trained on our dataset! (Ch 12 explores evaluation of open/closed models on generalized tasks.)
- **Chapter roadmap**: Ch 5 continues classification but with unsupervised methods — clustering data + topic modeling to name clusters.

### Important Numbers / Stats / Tokens
- Rotten Tomatoes dataset: 5,331 positive + 5,331 negative reviews (p.2). Train 8,530 / validation 1,066 / test 1,066 (p.3).
- Hugging Face Hub: 60,000+ text-classification models, 8,000+ embedding models (p.5).
- Solid baseline models list: BERT base (uncased), RoBERTa base, DistilBERT base (uncased), DeBERTa base, bert-tiny, ALBERT base v2 (p.6).
- `train_embeddings.shape` = `(8530, 768)` — each embedding 768 dims (p.12).
- Task-specific model (Twitter-RoBERTa) results: accuracy 0.80; Neg P/R/F1 0.76/0.88/0.81; Pos 0.86/0.72/0.78; weighted F1 0.80 (p.9).
- Embedding + logistic regression results: accuracy 0.85; both classes F1 0.85; weighted F1 0.85 (p.13).
- Zero-shot embedding results: accuracy 0.78; Neg 0.78/0.77/0.78; Pos 0.77/0.79/0.78; weighted F1 0.78 (p.16).
- Flan-T5-small results: accuracy 0.84; Neg 0.83/0.85/0.84; Pos 0.85/0.83/0.84; weighted F1 0.84 (p.21).
- ChatGPT/gpt-3.5-turbo-0125 results: accuracy 0.91; Neg 0.87/0.97/0.92; Pos 0.96/0.86/0.91; weighted F1 0.91 (p.25).
- T5-base architecture: 12 decoders + 12 encoders (p.18).
- Test-set inference cost for gpt-3.5-turbo-0125: ~3 cents (free-account covered) (p.24).
- BERT-like variants: RoBERTa, DistilBERT, ALBERT, DeBERTa (p.5).
- Label descriptions used: "A negative review", "A positive review" (p.15); improvement suggestion: "A very negative/positive movie review" (p.16).
- T5 sizes: flan-t5-small/base/large/xl/xxl (p.20).
- Example review: "unpretentious, charming, quirky, original" used with ChatGPT prompt (p.24).

### Algorithms & Formulæ
- **Task-specific model pipeline**:
  1. Load pipeline: `pipeline(model="cardiffnlp/twitter-roberta-base-sentiment-latest", tokenizer=..., return_all_scores=True, device="cuda:0")`.
  2. Run inference over test text via `pipe(KeyDataset(data["test"], "text"))`.
  3. Extract scores: `negative_score = output[0]["score"]`, `positive_score = output[2]["score"]`; `assignment = np.argmax([negative_score, positive_score])`.
  4. Evaluate with `classification_report`.
- **Confusion matrix / metrics**: Four combos (correct/incorrect × positive/negative). Precision = relevant found / found; Recall = relevant found / relevant total; Accuracy = correct / total; F1 = harmonic balance of precision & recall.
- **Two-step embedding classification**:
  1. `model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")`; encode train and test texts.
  2. `clf = LogisticRegression(random_state=42)`; `clf.fit(train_embeddings, data["train"]["label"])`.
  3. `y_pred = clf.predict(test_embeddings)`; evaluate.
- **Zero-shot with embeddings**:
  1. `label_embeddings = model.encode(["A negative review", "A positive review"])`.
  2. `sim_matrix = cosine_similarity(test_embeddings, label_embeddings)`.
  3. `y_pred = np.argmax(sim_matrix, axis=1)`.
  4. Cosine similarity = dot product of embeddings / product of their lengths.
- **Flan-T5 classification**:
  1. `pipe = pipeline("text2text-generation", model="google/flan-t5-small", device="cuda:0")`.
  2. `data = data.map(lambda example: {"t5": prompt + example['text']})` with `prompt = "Is the following sentence positive or negative? "`.
  3. Run `pipe(KeyDataset(data["test"], "t5"))`; `text = output[0]["generated_text"]`; `y_pred.append(0 if text == "negative" else 1)`.
- **ChatGPT classification**:
  1. `client = openai.OpenAI(api_key="YOUR_KEY_HERE")`.
  2. `chatgpt_generation(prompt, document, model="gpt-3.5-turbo-0125")`: build messages (system: "You are a helpful assistant."; user: prompt with `[DOCUMENT]` replaced), `client.chat.completions.create(messages=messages, model=model, temperature=0)`, return `choices[0].message.content`.
  3. Prompt template instructs to return 1 (positive) or 0 (negative), no other answers.
  4. `predictions = [chatgpt_generation(prompt, doc) for doc in tqdm(data["test"]["text"])]`; `y_pred = [int(pred) for pred in predictions]`.
- **Exponential backoff**: On rate-limit error → sleep briefly → retry; on each failure, increase sleep length until success or max retries.

### Diagrams / Visuals
- **Figure 4-1** — Using a language model to classify text (assigning a label to input).
- **Figure 4-2** — Both representation and generative models can be used for classification, but their approaches differ.
- **Figure 4-3** — A foundation model is fine-tuned for specific tasks (classification, general-purpose embeddings).
- **Figure 4-4** — Classification directly with a task-specific model or indirectly with general-purpose embeddings.
- **Figure 4-5** — Timeline of common BERT-like model releases (foundation models to be fine-tuned).
- **Figure 4-6** — An input sentence is first fed to a tokenizer before the task-specific model.
- **Figure 4-7** — Breaking an unknown word into tokens still allows word embeddings to be generated.
- **Figure 4-8** — The confusion matrix describes four types of predictions.
- **Figure 4-9** — The classification report describes several metrics (precision, recall, accuracy, F1).
- **Figure 4-10** — The feature extraction step and classification step are separated (two-step approach).
- **Figure 4-11** — Step 1: embedding model extracts features / converts input text to embeddings (frozen).
- **Figure 4-12** — Step 2: embeddings as features train a logistic regression on training data.
- **Figure 4-13** — Zero-shot classification: no labeled data, only the labels themselves; the model decides how input relates to candidate labels.
- **Figure 4-14** — To embed labels, give them a description (e.g., "a negative movie review"), then embed via sentence-transformers.
- **Figure 4-15** — Cosine similarity = angle between two vectors/embeddings (dot product / product of lengths).
- **Figure 4-16** — After embedding label descriptions and documents, cosine similarity for each label–document pair; highest similarity label chosen.
- **Figure 4-17** — Task-specific model generates numerical values from token sequences; generative model generates token sequences from token sequences.
- **Figure 4-18** — Prompt engineering: prompts updated to improve model output.
- **Figure 4-19** — T5 architecture is similar to the original Transformer — a decoder-encoder architecture (12 decoders + 12 encoders).
- **Figure 4-20** — T5 pretraining: predict masks that could contain multiple tokens (span masking).
- **Figure 4-21** — T5 fine-tuning: converting specific tasks to textual instructions trains a variety of tasks simultaneously.
- **Figure 4-22** — Manually labeled instruction data (prompt → output) used for instruction-tuning the first model variant.
- **Figure 4-23** — Manually ranked preference data used to generate the final model, ChatGPT.

### Common Exam Traps
- **Rotten Tomatoes split sizes**: train 8,530 / validation 1,066 / test 1,066; total 5,331 + 5,331 = 10,662 reviews. Don't confuse the split counts.
- **Label semantics**: label 1 = positive, label 0 = negative.
- **Pipeline score indexing**: For `twitter-roberta-base-sentiment-latest`, output[0] = negative, output[2] = positive (there are 3 outputs; index 1 is presumably neutral). The code reads `output[0]["score"]` for negative and `output[2]["score"]` for positive.
- **Task-specific vs embedding model**: task-specific = model fine-tuned for a specific task, used directly; embedding model = general-purpose embeddings feeding a separately trained classifier. Both keep the base model frozen in this chapter.
- **Confusion-matrix definitions**: precision = relevant items found / items found; recall = relevant items found / all relevant items. Don't swap them.
- **F1 = balance of precision and recall**, not just accuracy.
- **Weighted average used for F1** throughout the book (each class treated equally).
- **Embeddings shape (8530, 768)** — 8,530 train documents × 768 dims.
- **Zero-shot needs no labels**: only label names/descriptions; F1 0.78 achieved with zero labeled data.
- **Cosine similarity formula**: dot product / (product of lengths); it's the cosine of the angle, not distance.
- **Label descriptions matter**: "A negative/positive review" vs the more specific "A very negative/positive movie review" changes results.
- **Generative models need prompts**: they don't classify out of the box; you must instruct them (prompt engineering). Flan-T5 output text "negative"/"positive" must be mapped to 0/1.
- **T5 = encoder-decoder (text-to-text)**: not decoder-only; not a pure encoder like BERT. 12 encoders + 12 decoders (T5-base).
- **T5 pretraining masks token spans** (sets of tokens), not just individual tokens.
- **Flan-T5 = instruction-tuned T5**: >1,000 tasks during fine-tuning (from Chung et al. 2022).
- **ChatGPT training**: instruction data → first variant; preference data (ranked outputs) → final model. Preference data adds nuance vs instruction data.
- **temperature=0** used in the ChatGPT classification call (deterministic output).
- **GPT-3.5 architecture not shared**: assumed decoder-only from the name (GPT models).
- **API cost**: ~3 cents for the test dataset at writing; track usage. Rate limits → exponential backoff (increasing sleep between retries).
- **GPT evaluation caveat**: unknown training data → metrics can't be used to reliably evaluate (it may have seen our dataset).
- **Advice**: compare LLM results to TF-IDF + logistic regression baselines.
- **External embedding APIs**: Cohere's and OpenAI's offerings can replace local sentence-transformers GPU dependency, enabling CPU-only pipelines.

### Chapter Summary
Chapter 4 introduces text classification with pretrained language models using the Rotten Tomatoes movie-review dataset (binary sentiment). It first covers representation models in two flavors: a task-specific model (Twitter-RoBERTa-base sentiment, used directly through a pipeline, achieving weighted F1 0.80) and an embedding model (all-mpnet-base-v2) whose frozen 768-dim embeddings feed a trainable logistic-regression classifier in a two-step approach (F1 0.85). It then demonstrates zero-shot classification using only label descriptions ("A negative/positive review") and cosine similarity between document and label embeddings (F1 0.78) — no labeled data at all.

The second half covers generative models. Flan-T5-small (an open-source encoder-decoder text-to-text model, pipeline task `text2text-generation`) classifies when each document is prefixed with an instruction prompt, mapping generated "negative"/"positive" text to 0/1 (F1 0.84). ChatGPT (GPT-3.5-turbo-0125, closed-source, accessed via the OpenAI API with a prompt template, temperature=0) reaches F1 0.91, though evaluation is complicated by unknown training data. Throughout, the chapter emphasizes model selection (Hub baselines, MTEB leaderboard), evaluation via classification reports (precision, recall, accuracy, F1), and creative prompt/label engineering. Chapter 5 continues with unsupervised classification — clustering and topic modeling.

### Confidence Check
- **Sure**: Rotten Tomatoes dataset stats and splits; binary sentiment labels (1=pos, 0=neg); task-specific vs embedding model distinction; pipeline score indexing (output[0] negative, output[2] positive); F1 scores for each method (0.80/0.85/0.78/0.84/0.91); embeddings shape (8530, 768); confusion matrix/metrics definitions; two-step embedding classification; zero-shot via label descriptions + cosine similarity; T5 encoder-decoder text-to-text, span masking, >1,000-task instruction fine-tuning (Flan-T5); ChatGPT instruction-data → preference-data training; API cost ~3 cents; exponential backoff.
- **Uncertain**: Exact figure numbers in the printed page flow (page anchors from PDF text are approximate); the precise makeup of the pipeline's three outputs (index 1 presumably neutral — the chapter only references indices 0 and 2); minor — the exact wording of a few quoted passages where PDF extraction broke lines mid-sentence.

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading the Rotten Tomatoes dataset
```python
from datasets import load_dataset
# Load our data
data = load_dataset("rotten_tomatoes")
data
```
```
DatasetDict({
    train: Dataset({ features: ['text', 'label'], num_rows: 8530 })
    validation: Dataset({ features: ['text', 'label'], num_rows: 1066 })
    test: Dataset({ features: ['text', 'label'], num_rows: 1066 })
})
```
- **Explanation:** Loads the Rotten Tomatoes dataset from the Hugging Face Hub. Splits: train (8,530), validation (1,066), test (1,066), each with `text` and `label` features.
- **Fits the architecture:** Train split is used to train; test split validates results; the validation split can further check generalization after hyperparameter tuning.

### Code Block 2: Inspecting examples
```python
data["train"][0, -1]
```
```
{'text': ['the rock is destined to be the 21st century's new " conan " ...', 
          'things really get weird , though not particularly scary : ...'],
 'label': [1, 0]}
```
- **Explanation:** Shows two short reviews labeled 1 (positive) and 0 (negative) — binary sentiment classification.
- **Fits the architecture:** Label 1 = positive, 0 = negative throughout the chapter.

### Code Block 3: Loading the task-specific model into a pipeline
```python
from transformers import pipeline
# Path to our HF model
model_path = "cardiffnlp/twitter-roberta-base-sentiment-latest"
# Load model into pipeline
pipe = pipeline(
    model=model_path,
    tokenizer=model_path,
    return_all_scores=True,
    device="cuda:0"
)
```
- **Explanation:** Loads Twitter-RoBERTa-base sentiment (fine-tuned on tweets) with its tokenizer (loaded automatically, but shown to illustrate what happens under the hood). `return_all_scores=True` returns scores for all classes.
- **Fits the architecture:** Input text → tokenizer → tokens → task-specific model → class scores.

### Code Block 4: Running inference on the test split
```python
import numpy as np
from tqdm import tqdm
from transformers.pipelines.pt_utils import KeyDataset
# Run inference
y_pred = []
for output in tqdm(pipe(KeyDataset(data["test"], "text")), total=len(data["test"])):
    negative_score = output[0]["score"]
    positive_score = output[2]["score"]
    assignment = np.argmax([negative_score, positive_score])
    y_pred.append(assignment)
```
- **Explanation:** Iterates over test reviews, reading the negative score (index 0) and positive score (index 2), and assigns the class with the higher score.
- **Fits the architecture:** The model outputs per-class probability scores; `np.argmax` picks the predicted class (0 = negative, 1 = positive).

### Code Block 5: The evaluation helper
```python
from sklearn.metrics import classification_report
def evaluate_performance(y_true, y_pred):
    """Create and print the classification report"""
    performance = classification_report(
        y_true, y_pred,
        target_names=["Negative Review", "Positive Review"]
    )
    print(performance)
```
- **Explanation:** Wraps scikit-learn's `classification_report` with class names, printing precision/recall/f1 per class plus accuracy and macro/weighted averages.
- **Fits the architecture:** Reused throughout the chapter for every method.

### Code Block 6: Classification report for the task-specific model
```
                precision    recall  f1-score   support
Negative Review       0.76      0.88      0.81       533
Positive Review       0.86      0.72      0.78       533
       accuracy                           0.80      1066
      macro avg       0.81      0.80      0.80      1066
   weighted avg       0.81      0.80      0.80      1066
```
- **Explanation:** Accuracy 0.80; weighted avg F1 0.80 — great for a model not trained on the domain data.
- **Fits the architecture:** Metrics derive from the confusion matrix (precision, recall, accuracy, F1).

### Code Block 7: Embeddings with sentence-transformers
```python
from sentence_transformers import SentenceTransformer
# Load model
model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")
# Convert text to embeddings
train_embeddings = model.encode(data["train"]["text"], show_progress_bar=True)
test_embeddings = model.encode(data["test"]["text"], show_progress_bar=True)
```
```python
train_embeddings.shape
(8530, 768)
```
- **Explanation:** Encodes train/test texts into 768-dim embeddings (8,530 documents).
- **Fits the architecture:** Step 1 of the two-step approach — frozen embedding model extracts features.

### Code Block 8: Training the logistic regression classifier
```python
from sklearn.linear_model import LogisticRegression
# Train a logistic regression on our train embeddings
clf = LogisticRegression(random_state=42)
clf.fit(train_embeddings, data["train"]["label"])
# Predict previously unseen instances
y_pred = clf.predict(test_embeddings)
evaluate_performance(data["test"]["label"], y_pred)
```
```
               precision    recall  f1-score   support
Negative Review       0.85      0.86      0.85       533
Positive Review       0.86      0.85      0.85       533
       accuracy                           0.85      1066
      macro avg       0.85      0.85      0.85      1066
   weighted avg       0.85      0.85      0.85      1066
```
- **Explanation:** Trains a lightweight logistic regression (random_state=42) on the embeddings — step 2. F1 0.85, better than the task-specific model.
- **Fits the architecture:** The classifier is trainable while the embedding model stays frozen.

### Code Block 9: Zero-shot label embeddings
```python
# Create embeddings for our labels
label_embeddings = model.encode(["A negative review",  "A positive review"])
```
- **Explanation:** Embeds label descriptions with the same embedding model, creating our own target labels without labeled data.
- **Fits the architecture:** Describing labels lets the embedding model place them in the same vector space as the documents.

### Code Block 10: Cosine similarity for zero-shot classification
```python
from sklearn.metrics.pairwise import cosine_similarity
# Find the best matching label for each document
sim_matrix = cosine_similarity(test_embeddings, label_embeddings)
y_pred = np.argmax(sim_matrix, axis=1)
```
```
               precision    recall  f1-score   support
Negative Review       0.78      0.77      0.78       533
Positive Review       0.77      0.79      0.78       533
       accuracy                           0.78      1066
      macro avg       0.78      0.78      0.78      1066
   weighted avg       0.78      0.78      0.78      1066
```
- **Explanation:** Cosine similarity between each document embedding and each label embedding; `np.argmax` selects the best-matching label per document. F1 0.78 with no labeled data.
- **Fits the architecture:** Cosine similarity = angle between vectors (dot product / product of lengths).

### Code Block 11: Loading Flan-T5
```python
# Load our model
pipe = pipeline(
    "text2text-generation", 
    model="google/flan-t5-small", 
    device="cuda:0"
)
```
- **Explanation:** Loads the smallest Flan-T5 (google/flan-t5-small) with the `text2text-generation` task, reserved for encoder-decoder models.
- **Fits the architecture:** T5 is an encoder-decoder (text-to-text) sequence-to-sequence model.

### Code Block 12: Preparing data with a prompt prefix
```python
# Prepare our data
prompt = "Is the following sentence positive or negative? "
data = data.map(lambda example: {"t5": prompt + example['text']})
```
- **Explanation:** Adds a new `t5` column prefixing each document with the instruction prompt.
- **Fits the architecture:** Generative models need instructions; the model generates "positive" or "negative".

### Code Block 13: Flan-T5 inference and mapping output
```python
# Run inference
y_pred = []
for output in tqdm(pipe(KeyDataset(data["test"], "t5")), total=len(data["test"])):
    text = output[0]["generated_text"]
    y_pred.append(0 if text == "negative" else 1)
```
- **Explanation:** Runs the text2text pipeline; maps generated "negative" → 0, otherwise → 1. F1 0.84.
- **Fits the architecture:** Textual output must be converted to numeric labels for evaluation.

### Code Block 14: OpenAI client and generation function
```python
import openai
# Create client
client = openai.OpenAI(api_key="YOUR_KEY_HERE")
```
```python
def chatgpt_generation(prompt, document, model="gpt-3.5-turbo-0125"):
    """Generate an output based on a prompt and an input document."""
    messages=[
        {
            "role": "system",
            "content": "You are a helpful assistant."
            },
        {
            "role": "user",
            "content":   prompt.replace("[DOCUMENT]", document)
            }
    ]
    chat_completion = client.chat.completions.create(
      messages=messages,
      model=model,
      temperature=0
    )
    return chat_completion.choices[0].message.content
```
- **Explanation:** Creates an OpenAI client and a helper that builds a system + user message pair, replaces `[DOCUMENT]` in the prompt with the document, and calls the chat completions API at temperature=0 (deterministic).
- **Fits the architecture:** Closed-source models are accessed via API rather than loaded locally.

### Code Block 15: ChatGPT prompt template and usage
```python
# Define a prompt template as a base
prompt = """Predict whether the following document is a positive or negative 
movie review:
[DOCUMENT]
If it is positive return 1 and if it is negative return 0. Do not give any 
other answers.
"""
# Predict the target using GPT
document = "unpretentious , charming , quirky , original"
chatgpt_generation(prompt, document)
```
- **Explanation:** The template instructs the model to output 1 or 0 only. Example document: "unpretentious, charming, quirky, original".
- **Fits the architecture:** Prompt engineering guides the generative model toward the desired structured output.

### Code Block 16: Running ChatGPT on the test set and evaluating
```python
# You can skip this if you want to save your (free) credits
predictions = [
    chatgpt_generation(prompt, doc) for doc in tqdm(data["test"]["text"])
]
# Extract predictions
y_pred = [int(pred) for pred in predictions]
# Evaluate performance
evaluate_performance(data["test"]["label"], y_pred)
```
```
               precision    recall  f1-score   support
Negative Review       0.87      0.97      0.92       533
Positive Review       0.96      0.86      0.91       533
       accuracy                           0.91      1066
      macro avg       0.92      0.91      0.91      1066
   weighted avg       0.92      0.91      0.91      1066
```
- **Explanation:** Runs the API over the test set, converts outputs to ints, evaluates. F1 0.91. Caveat: unknown training data limits interpretability of the metrics.
- **Fits the architecture:** API calls cost ~3 cents for the test set at writing; watch usage and rate limits.

---

## §3. Chapter-Specific Flashcards
*(Separate file: `flashcards_qna.md`)*

## §4. Practice Exam
*(Separate file: `practice_exam.md`)*
