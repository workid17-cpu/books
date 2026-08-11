# 📘 Chapter 4 Line-by-Line: Text Classification
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 4
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "A common task in natural language processing is classification. The goal of the task is to train a model to assign a label or class to some input text. Classifying text is used across the world for a wide range of applications, from sentiment analysis and intent detection to extracting entities and detecting language."
   - **Plain English:** Classification = assigning a label to input text; used in many apps.
   - **Technical terms:** classification; sentiment analysis; intent detection; entity extraction; language detection.

2. **Quote:** "The impact of language models, both representative and generative, on classification cannot be understated."
   - **Plain English:** LLMs (both kinds) have massively impacted classification.
   - **Technical terms:** representative (representation) models; generative models.

3. **Quote:** "In this chapter, we will discuss several ways to use language models for classifying text. It will serve as an accessible introduction to using language models that already have been trained."
   - **Plain English:** Chapter covers several ways to classify text with pretrained models.
   - **Technical terms:** pretrained language models.

4. **Quote:** "Text Classification with Representation Models... demonstrates the flexibility of nongenerative models for classification. We will cover both task-specific models and embedding models."
   - **Plain English:** Part 1 covers representation models (task-specific + embedding).
   - **Technical terms:** task-specific model; embedding model.

5. **Quote:** "Text Classification with Generative Models is an introduction to generative language models as most of them can be used for classification. We will cover both an open source as well as a closed source language model."
   - **Plain English:** Part 2 covers generative models (open source + closed source).
   - **Technical terms:** open source model; closed source model.

6. **Quote:** "In this chapter, we will focus on leveraging pretrained language models, models that already have been trained on large amounts of data that can be used for classifying text."
   - **Plain English:** We use models already trained on large datasets.
   - **Technical terms:** pretrained models.

7. **Quote:** "This chapter serves as an introduction to a variety of language models, both generative and nongenerative. We will encounter common packages for loading and using these models."
   - **Plain English:** Introduces packages for loading/using various models.
   - **Technical terms:** Hugging Face packages (transformers, datasets, sentence-transformers, sklearn).

8. **Quote (advice box):** "Although this book focuses on LLMs, it is highly advised to compare these examples against classic, but strong baselines such as representing text with TF-IDF and training a logistic regression classifier on top of that."
   - **Plain English:** Compare LLM results to TF-IDF + logistic regression baselines.
   - **Technical terms:** TF-IDF; logistic regression baseline.

### The Sentiment of Movie Reviews

9. **Quote:** "You can find the data we use to explore techniques for classifying text on the Hugging Face Hub, a platform for hosting models but also data. We will use the well-known 'rotten_tomatoes' dataset to train and evaluate our models. It contains 5,331 positive and 5,331 negative movie reviews from Rotten Tomatoes."
   - **Plain English:** Dataset = Rotten Tomatoes reviews; 5,331 positive + 5,331 negative.
   - **Technical terms:** Hugging Face Hub; rotten_tomatoes dataset.

### Code Block: Loading the dataset
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
- **Explanation:** `load_dataset` fetches the dataset with train (8,530), validation (1,066), test (1,066) splits, each with `text` and `label` features.
- **Fits the architecture:** Train for training, test for validation, validation for further generalization checks.

10. **Quote:** "The data is split up into train, test, and validation splits. Throughout this chapter, we will use the train split when we train a model and the test split for validating the results. Note that the additional validation split can be used to further validate generalization if you used the train and test splits to perform hyperparameter tuning."
    - **Plain English:** Train on train, validate on test; validation split is for extra checks.
    - **Technical terms:** train/test/validation splits; hyperparameter tuning; generalization.

### Code Block: Inspecting examples
```python
data["train"][0, -1]
```
```
{'text': ['the rock is destined to be the 21st century's new " conan " and that he's
          going to make a splash even greater than arnold schwarzenegger ...',
          'things really get weird , though not particularly scary : the movie is all
          portent and no content .'],
 'label': [1, 0]}
```
- **Explanation:** Shows two short reviews labeled 1 (positive) and 0 (negative).
- **Fits the architecture:** Binary sentiment classification: 1 = positive, 0 = negative.

11. **Quote:** "These short reviews are either labeled as positive (1) or negative (0). This means that we will focus on binary sentiment classification."
    - **Plain English:** Two labels → binary sentiment task.
    - **Technical terms:** binary sentiment classification.

### Text Classification with Representation Models

12. **Quote:** "Classification with pretrained representation models generally comes in two flavors, either using a task-specific model or an embedding model. As we explored in the previous chapter, these models are created by fine-tuning a foundation model, like BERT, on a specific downstream task."
    - **Plain English:** Two flavors: task-specific model or embedding model; both from fine-tuning a foundation model.
    - **Technical terms:** task-specific model; embedding model; fine-tuning; foundation model; downstream task.

13. **Quote:** "A task-specific model is a representation model, such as BERT, trained for a specific task, like sentiment analysis. As we explored in Chapter 1, an embedding model generates general-purpose embeddings that can be used for a variety of tasks not limited to classification, like semantic search (see Chapter 8)."
    - **Plain English:** Task-specific = for one task; embedding model = general-purpose vectors.
    - **Technical terms:** task-specific model; general-purpose embeddings; semantic search.

14. **Quote:** "The process of fine-tuning a BERT model for classification is covered in Chapter 11 while creating an embedding model is covered in Chapter 10. In this chapter, we keep both models frozen (nontrainable) and only use their output."
    - **Plain English:** Fine-tuning covered in Ch 10/11; here models are frozen, only outputs used.
    - **Technical terms:** frozen (nontrainable) models.

### Model Selection

15. **Quote:** "Choosing the right models is not as straightforward as you might think with over 60,000 models on the Hugging Face Hub for text classification and more than 8,000 models that generate embeddings at the moment of writing. Moreover, it's crucial to select a model that fits your use case and consider its language compatibility, the underlying architecture, size, and performance."
    - **Plain English:** Model selection is hard given 60K+ classifiers and 8K+ embedding models; consider compatibility, architecture, size, performance.
    - **Technical terms:** model selection; Hugging Face Hub; language compatibility; architecture; performance.

16. **Quote:** "As we explored in Chapter 1, BERT, a well-known encoder-only architecture, is a popular choice for creating task-specific and embedding models. While generative models, like the GPT family, are incredible models, encoder-only models similarly excel in task-specific use cases and tend to be significantly smaller in size."
    - **Plain English:** BERT (encoder-only) is popular for task-specific/embedding models; smaller than generative models.
    - **Technical terms:** encoder-only architecture; BERT; GPT family.

17. **Quote:** "Over the years, many variations of BERT have been developed, including RoBERTa, DistilBERT, ALBERT, and DeBERTa, each trained in various contexts."
    - **Plain English:** BERT has many variants trained in different contexts.
    - **Technical terms:** RoBERTa; DistilBERT; ALBERT; DeBERTa.

18. **Quote:** "Selecting the right model for the job can be a form of art in itself. Trying thousands of pretrained models that can be found on Hugging Face's Hub is not feasible so we need to be efficient with the models that we choose."
    - **Plain English:** Can't try thousands of models; be efficient.
    - **Word meanings:** feasible = practical/doable.
    - **Technical terms:** model selection efficiency.

19. **Quote:** "Several models are great starting points and give you an idea of the base performance of these kinds of models. Consider them solid baselines: BERT base model (uncased), RoBERTa base model, DistilBERT base model (uncased), DeBERTa base model, bert-tiny, ALBERT base v2."
    - **Plain English:** List of solid baseline models.
    - **Technical terms:** baselines; BERT base; RoBERTa base; DistilBERT base; DeBERTa base; bert-tiny; ALBERT base v2.

20. **Quote:** "For the task-specific model, we are choosing the Twitter-RoBERTa-base for Sentiment Analysis model. This is a RoBERTa model fine-tuned on tweets for sentiment analysis. Although this was not trained specifically for movie reviews, it is interesting to explore how this model generalizes."
    - **Plain English:** Chosen task-specific model = Twitter-RoBERTa-base sentiment; tests generalization.
    - **Technical terms:** Twitter-RoBERTa; generalization.

21. **Quote:** "When selecting models to generate embeddings from, the MTEB leaderboard is a great place to start. It contains open and closed source models benchmarked across several tasks. Make sure to not only take performance into account. The importance of inference speed should not be underestimated in real-life solutions. As such, we will use sentence-transformers/all-mpnet-base-v2 as the embedding throughout this section. It is a small but performant model."
    - **Plain English:** MTEB leaderboard for embedding models; all-mpnet-base-v2 chosen (small but performant).
    - **Technical terms:** MTEB leaderboard; inference speed; all-mpnet-base-v2.

### Using a Task-Specific Model

### Code Block: Loading the task-specific pipeline
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
- **Explanation:** Loads the Twitter-RoBERTa sentiment model and its tokenizer (automatically loaded, shown for illustration), returning all class scores.
- **Fits the architecture:** Input → tokenizer → task-specific model → per-class scores.

22. **Quote:** "As we load our model, we also load the tokenizer, which is responsible for converting input text into individual tokens. Although that parameter is not needed as it is loaded automatically, it illustrates what is happening under the hood."
    - **Plain English:** Tokenizer converts text to tokens; loaded automatically but shown explicitly.
    - **Technical terms:** tokenizer.

23. **Quote:** "A major benefit of these tokens is that they can be combined to generate representations even if they were not in the training data."
    - **Plain English:** Tokens can represent unseen words by combining.
    - **Technical terms:** tokenization; unseen words.

### Code Block: Running inference on the test split
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
- **Explanation:** Reads negative score (index 0) and positive score (index 2) for each review; `np.argmax` chooses the predicted class.
- **Fits the architecture:** The task-specific model outputs per-class probabilities; argmax selects the label.

### Code Block: The evaluation helper
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
- **Explanation:** A reusable wrapper printing scikit-learn's classification report.
- **Fits the architecture:** Used to evaluate every method in the chapter.

### Code Block: Task-specific model report
```
                precision    recall  f1-score   support
Negative Review       0.76      0.88      0.81       533
Positive Review       0.86      0.72      0.78       533
       accuracy                           0.80      1066
      macro avg       0.81      0.80      0.80      1066
   weighted avg       0.81      0.80      0.80      1066
```
- **Explanation:** Accuracy 0.80, weighted F1 0.80.
- **Fits the architecture:** Metrics derive from the confusion matrix.

24. **Quote:** "To read the resulting classification report, let's first start by exploring how we can identify correct and incorrect predictions. There are four combinations depending on whether we predict something correctly (True) versus incorrectly (False) and whether we predict the correct class (Positive) versus incorrect class (Negative). We can illustrate these combinations as a matrix, commonly referred to as a confusion matrix."
    - **Plain English:** The confusion matrix shows four prediction outcomes.
    - **Technical terms:** confusion matrix; true/false positives/negatives.

25. **Quote:** "Using the confusion matrix, we can derive several formulas to describe the quality of the model. In the previously generated classification report we can see four such methods, namely precision, recall, accuracy, and the F1 score."
    - **Plain English:** Four metrics derive from the confusion matrix.
    - **Technical terms:** precision; recall; accuracy; F1 score.

26. **Quote:** "Precision measures how many of the items found are relevant, which indicates the accuracy of the relevant results."
    - **Plain English:** Precision = share of found items that are actually relevant.
    - **Technical terms:** precision.

27. **Quote:** "Recall refers to how many relevant classes were found, which indicates its ability to find all relevant results."
    - **Plain English:** Recall = share of all relevant items that were found.
    - **Technical terms:** recall.

28. **Quote:** "Accuracy refers to how many correct predictions the model makes out of all predictions, which indicates the overall correctness of the model."
    - **Plain English:** Accuracy = correct predictions / all predictions.
    - **Technical terms:** accuracy.

29. **Quote:** "The F1 score balances both precision and recall to create a model's overall performance."
    - **Plain English:** F1 balances precision and recall.
    - **Technical terms:** F1 score.

30. **Quote:** "We will consider the weighted average of the F1 score throughout the examples in this book to make sure each class is treated equally. Our pretrained BERT model gives us an F1 score of 0.80 (we are reading this from the weighted avg row and the f1-score column), which is great for a model not trained specifically on our domain data!"
    - **Plain English:** Weighted-avg F1 used; BERT gives 0.80 — great without domain training.
    - **Technical terms:** weighted average; F1 score.

31. **Quote:** "To improve the performance of our selected model, we could do a few different things including selecting a model trained on our domain data, movie reviews in this case, like DistilBERT base uncased finetuned SST-2. We could also shift our focus to another flavor of representation models, namely embedding models."
    - **Plain English:** Improvements: use a domain-trained model (DistilBERT SST-2) or switch to embeddings.
    - **Technical terms:** domain data; SST-2; embedding models.

### Classification Tasks That Leverage Embeddings

32. **Quote:** "In the previous example, we used a pretrained task-specific model for sentiment analysis. However, what if we cannot find a model that was pretrained for this specific task? Do we need to fine-tune a representation model ourselves? The answer is no!"
    - **Plain English:** No task-specific model available → you don't have to fine-tune yourself.
    - **Technical terms:** fine-tuning.

33. **Quote:** "There might be times when you want to fine-tune the model yourself if you have sufficient computing available (see Chapter 11). However, not everyone has access to extensive computing. This is where general-purpose embedding models come in."
    - **Plain English:** Embedding models are the alternative when compute is limited.
    - **Technical terms:** general-purpose embedding models.

### Supervised Classification

34. **Quote:** "Unlike the previous example, we can perform part of the training process ourselves by approaching it from a more classical perspective. Instead of directly using the representation model for classification, we will use an embedding model for generating features. Those features can then be fed into a classifier, thereby creating a two-step approach."
    - **Plain English:** Two-step approach: embeddings as features → separate classifier.
    - **Technical terms:** feature extraction; two-step approach.

35. **Quote:** "A major benefit of this separation is that we do not need to fine-tune our embedding model, which can be costly. In contrast, we can train a classifier, like a logistic regression, on the CPU instead."
    - **Plain English:** No embedding fine-tuning needed; lightweight classifier trains on CPU.
    - **Technical terms:** logistic regression; CPU training.

36. **Quote:** "In the first step, we convert our textual input to embeddings using the embedding model. Note that this model is similarly kept frozen and is not updated during the training process."
    - **Plain English:** Step 1: frozen embedding model converts text to vectors.
    - **Technical terms:** frozen embedding model.

### Code Block: Creating embeddings
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
- **Explanation:** Encodes train/test texts into embeddings; shape (8530, 768).
- **Fits the architecture:** Step 1 — features extracted; 768 values per document.

37. **Quote:** "As we covered in Chapter 1, these embeddings are numerical representations of the input text. The number of values, or dimension, of the embedding depends on the underlying embedding model."
    - **Plain English:** Embeddings are numerical text representations; dimension depends on the model.
    - **Technical terms:** embedding dimension.

38. **Quote:** "This shows that each of our 8,530 input documents has an embedding dimension of 768 and therefore each embedding contains 768 numerical values."
    - **Plain English:** 8,530 documents × 768 values each.
    - **Technical terms:** embedding dimension.

39. **Quote:** "In the second step, these embeddings serve as the input features to the classifier. The classifier is trainable and not limited to logistic regression and can take on any form as long as it performs classification."
    - **Plain English:** Step 2: embeddings as features for a trainable classifier (any classifier works).
    - **Technical terms:** trainable classifier; logistic regression.

### Code Block: Training and evaluating logistic regression
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
- **Explanation:** Logistic regression (random_state=42) trained on embeddings; F1 0.85.
- **Fits the architecture:** Lightweight classifier on frozen embeddings beats the task-specific model.

40. **Quote:** "By training a classifier on top of our embeddings, we managed to get an F1 score of 0.85! This demonstrates the possibilities of training a lightweight classifier while keeping the underlying embedding model frozen."
    - **Plain English:** F1 0.85 with frozen embeddings + lightweight classifier.
    - **Technical terms:** frozen model; lightweight classifier.

41. **Quote (note box):** "In this example, we used sentence-transformers to extract our embeddings, which benefits from a GPU to speed up inference. However, we can remove this GPU dependency by using an external API to create the embeddings. Popular choices for generating embeddings are Cohere's and OpenAI's offerings. As a result, this would allow the pipeline to run entirely on the CPU."
    - **Plain English:** External embedding APIs (Cohere, OpenAI) remove the GPU dependency.
    - **Technical terms:** GPU dependency; embedding APIs.

### What If We Do Not Have Labeled Data?

42. **Quote:** "In our previous example, we had labeled data that we could leverage, but this might not always be the case in practice. Getting labeled data is a resource-intensive task that can require significant human labor. Moreover, is it actually worthwhile to collect these labels?"
    - **Plain English:** Labeled data is expensive; is it worth collecting?
    - **Technical terms:** labeled data; human labor.

43. **Quote:** "To test this, we can perform zero-shot classification, where we have no labeled data to explore whether the task seems feasible. Although we know the definition of the labels (their names), we do not have labeled data to support them. Zero-shot classification attempts to predict the labels of input text even though it was not trained on them."
    - **Plain English:** Zero-shot: no labeled data, only label names; model predicts anyway.
    - **Technical terms:** zero-shot classification; candidate labels.

44. **Quote:** "To perform zero-shot classification with embeddings, there is a neat trick that we can use. We can describe our labels based on what they should represent. For example, a negative label for movie reviews can be described as 'This is a negative movie review.' By describing and embedding the labels and documents, we have data that we can work with."
    - **Plain English:** Describe labels, embed descriptions, and you have usable data.
    - **Technical terms:** label description; label embedding.

45. **Quote:** "This process allows us to generate our own target labels without the need to actually have any labeled data."
    - **Plain English:** Self-generated labels replace labeled data.
    - **Technical terms:** generated target labels.

### Code Block: Embedding the labels
```python
# Create embeddings for our labels
label_embeddings = model.encode(["A negative review",  "A positive review"])
```
- **Explanation:** Embeds the two label descriptions with the same embedding model.
- **Fits the architecture:** Puts labels in the same vector space as documents.

46. **Quote:** "To assign labels to documents, we can apply cosine similarity to the document label pairs. This is the cosine of the angle between vectors, which is calculated through the dot product of the embeddings and divided by the product of their lengths."
    - **Plain English:** Cosine similarity = dot product / product of lengths.
    - **Technical terms:** cosine similarity; dot product; vector length.

### Code Block: Cosine similarity for zero-shot classification
```python
from sklearn.metrics.pairwise import cosine_similarity
# Find the best matching label for each document
sim_matrix = cosine_similarity(test_embeddings, label_embeddings)
y_pred = np.argmax(sim_matrix, axis=1)
```
- **Explanation:** Computes similarity between each document embedding and each label embedding; argmax picks the best-matching label.
- **Fits the architecture:** Highest-similarity label is assigned.

47. **Quote:** "We can use cosine similarity to check how similar a given document is to the description of the candidate labels. The label with the highest similarity to the document is chosen."
    - **Plain English:** Highest-similarity label wins.
    - **Technical terms:** candidate labels; similarity.

### Code Block: Zero-shot report
```
               precision    recall  f1-score   support
Negative Review       0.78      0.77      0.78       533
Positive Review       0.77      0.79      0.78       533
       accuracy                           0.78      1066
      macro avg       0.78      0.78      0.78      1066
   weighted avg       0.78      0.78      0.78      1066
```
- **Explanation:** F1 0.78 with no labeled data at all.
- **Fits the architecture:** Demonstrates embedding flexibility.

48. **Quote:** "And that is it! We only needed to come up with names for our labels to perform our classification tasks."
    - **Plain English:** Only label names are needed.
    - **Technical terms:** label names.

49. **Quote:** "An F1 score of 0.78 is quite impressive considering we did not use any labeled data at all! This just shows how versatile and useful embeddings are, especially if you are a bit creative with how they are used."
    - **Plain English:** F1 0.78 impressive without labels; embeddings are versatile.
    - **Word meanings:** versatile = adaptable/multipurpose.

50. **Quote (note box):** "We decided upon 'A negative/positive review' as the name of our labels but that can be improved. Instead, we can make them a bit more concrete and specific toward our data by using 'A very negative/positive movie review' instead. This way, the embedding will capture that it is a movie review and will focus a bit more on the extremes of the two labels. Try it out and explore how it affects the results."
    - **Plain English:** More specific label descriptions ("A very negative/positive movie review") can improve results.
    - **Technical terms:** label description engineering.

51. **Quote (note box):** "Although natural language inference models are amazing for zero-shot classification, the example here demonstrates the flexibility of embeddings for a variety of tasks. As you will see throughout the book, embeddings can be found in most Language AI use cases and are often an underestimated but incredibly vital component."
    - **Plain English:** Embeddings (vs NLI) chosen to show flexibility; they're vital across Language AI.
    - **Technical terms:** natural language inference (NLI); embeddings.

### Text Classification with Generative Models

52. **Quote:** "Classification with generative language models, such as OpenAI's GPT models, works a bit differently from what we have done thus far. These models take as input some text and generate text and are thereby aptly named sequence-to-sequence models."
    - **Plain English:** Generative models = text in, text out (sequence-to-sequence).
    - **Technical terms:** generative models; sequence-to-sequence models.

53. **Quote:** "This is in stark contrast to our task-specific model, which outputs a class instead."
    - **Plain English:** Task-specific models output a class; generative models output text.
    - **Technical terms:** class output vs generated text.

54. **Quote:** "These generative models are generally trained on a wide variety of tasks and usually do not perform your use case out of the box. For instance, if we give a generative model a movie review without any context, it has no idea what to do with it."
    - **Plain English:** Generative models need guidance; they don't classify out of the box.
    - **Technical terms:** out-of-the-box performance.

55. **Quote:** "Instead, we need to help it understand the context and guide it toward the answers that we are looking for. As demonstrated in Figure 4-18, this guiding process is done mainly through the instruction, or prompt, that you give such a model. Iteratively improving your prompt to get your preferred output is called prompt engineering."
    - **Plain English:** Guide via prompts; iteratively improving prompts = prompt engineering.
    - **Technical terms:** instruction; prompt; prompt engineering.

### Using the Text-to-Text Transfer Transformer

56. **Quote:** "Throughout this book, we will explore mostly encoder-only (representation) models like BERT and decoder-only (generative) models like ChatGPT. However, as discussed in Chapter 1, the original Transformer architecture actually consists of an encoder-decoder architecture. Like the decoder-only models, these encoder-decoder models are sequence-to-sequence models and generally fall in the category of generative models."
    - **Plain English:** Encoder-decoder models (like the original Transformer) are also generative/sequence-to-sequence.
    - **Technical terms:** encoder-only; decoder-only; encoder-decoder; sequence-to-sequence.

57. **Quote:** "An interesting family of models that leverage this architecture is the Text-to-Text Transfer Transformer or T5 model. Its architecture is similar to the original Transformer where 12 decoders and 12 encoders are stacked together."
    - **Plain English:** T5 = encoder-decoder; 12 encoders + 12 decoders.
    - **Technical terms:** T5; text-to-text; encoder-decoder stack.

58. **Quote:** "With this architecture, these models were first pretrained using masked language modeling. In the first step of training, instead of masking individual tokens, sets of tokens (or token spans) were masked during pretraining."
    - **Plain English:** T5 pretraining masks token spans, not individual tokens.
    - **Technical terms:** masked language modeling; token spans.

59. **Quote:** "The second step of training, namely fine-tuning the base model, is where the real magic happens. Instead of fine-tuning the model for one specific task, each task is converted to a sequence-to-sequence task and trained simultaneously."
    - **Plain English:** Fine-tuning converts each task into a text-to-text task, trained together.
    - **Technical terms:** fine-tuning; text-to-text task; multitask training.

60. **Quote:** "This method of fine-tuning was extended in the paper 'Scaling instruction-finetuned language models', which introduced more than a thousand tasks during fine-tuning that more closely follow instructions as we know them from GPT models. This resulted in the Flan-T5 family of models that benefit from this large variety of tasks."
    - **Plain English:** Flan-T5 trained on 1,000+ instruction tasks (from Chung et al.).
    - **Technical terms:** instruction fine-tuning; Flan-T5.

### Code Block: Loading Flan-T5
```python
# Load our model
pipe = pipeline(
    "text2text-generation", 
    model="google/flan-t5-small", 
    device="cuda:0"
)
```
- **Explanation:** Loads the smallest Flan-T5 with the `text2text-generation` task (reserved for encoder-decoder models).
- **Fits the architecture:** T5 is a text-to-text (encoder-decoder) generative model.

61. **Quote:** "The Flan-T5 model comes in various sizes (flan-t5-small/base/large/xl/xxl) and we will use the smallest to speed things up a bit. However, feel free to play around with larger models to see if you can improve the results."
    - **Plain English:** Smallest Flan-T5 used for speed; larger sizes may improve results.
    - **Technical terms:** model sizes.

62. **Quote:** "Compared to our task-specific model, we cannot just give the model some text and hope it will output the sentiment. Instead, we will have to instruct the model to do so. Thus, we prefix each document with the prompt 'Is the following sentence positive or negative?'"
    - **Plain English:** Each document is prefixed with an instruction prompt.
    - **Technical terms:** prompt prefix; instruction.

### Code Block: Preparing the data with a prompt prefix
```python
# Prepare our data
prompt = "Is the following sentence positive or negative? "
data = data.map(lambda example: {"t5": prompt + example['text']})
```
```
DatasetDict({
    train: Dataset({ features: ['text', 'label', 't5'], num_rows: 8530 })
    validation: Dataset({ features: ['text', 'label', 't5'], num_rows: 1066 })
    test: Dataset({ features: ['text', 'label', 't5'], num_rows: 1066 })
})
```
- **Explanation:** Adds a `t5` column with the instruction prompt prepended to each review.
- **Fits the architecture:** The prompt guides the generative model to output the sentiment.

### Code Block: Flan-T5 inference
```python
# Run inference
y_pred = []
for output in tqdm(pipe(KeyDataset(data["test"], "t5")), total=len(data["test"])):
    text = output[0]["generated_text"]
    y_pred.append(0 if text == "negative" else 1)
```
- **Explanation:** Runs the text2text pipeline; maps "negative" → 0, otherwise → 1.
- **Fits the architecture:** Generated text is converted to numeric labels.

63. **Quote:** "Since this model generates text, we did need to convert the textual output to numerical values. The output word 'negative' was mapped to 0 whereas 'positive' was mapped to 1."
    - **Plain English:** Textual output converted to numeric labels.
    - **Technical terms:** label mapping.

### Code Block: Flan-T5 report
```
               precision    recall  f1-score   support
Negative Review       0.83      0.85      0.84       533
Positive Review       0.85      0.83      0.84       533
       accuracy                           0.84      1066
      macro avg       0.84      0.84      0.84      1066
   weighted avg       0.84      0.84      0.84      1066
```
- **Explanation:** F1 0.84 — a great first look at generative models.
- **Fits the architecture:** Open-source encoder-decoder generative classification.

64. **Quote:** "With an F1 score of 0.84, it is clear this Flan-T5 model is an amazing first look into the capabilities of generative models."
    - **Plain English:** Flan-T5 shows generative models' capabilities.
    - **Technical terms:** generative model capabilities.

### ChatGPT for Classification

65. **Quote:** "Although we focus throughout the book on open source models, another major component of the Language AI field is closed sourced models; in particular, ChatGPT. Although the underlying architecture of the original ChatGPT model (GPT-3.5) is not shared, we can assume from its name that it is based on the decoder-only architecture that we have seen in the GPT models thus far."
    - **Plain English:** ChatGPT = closed source; GPT-3.5 assumed decoder-only (architecture not shared).
    - **Technical terms:** closed source model; decoder-only architecture; GPT-3.5.

66. **Quote:** "Fortunately, OpenAI shared an overview of the training procedure that involved an important component, namely preference tuning. As illustrated in Figure 4-22, OpenAI first manually created the desired output to an input prompt (instruction data) and used that data to create a first variant of its model."
    - **Plain English:** First step: manually created instruction data → first model variant.
    - **Technical terms:** instruction data; instruction-tuning; model variant.

67. **Quote:** "OpenAI used the resulting model to generate multiple outputs that were manually ranked from best to worst. As shown in Figure 4-23, this ranking demonstrates a preference for certain outputs (preference data) and was used to create its final model, ChatGPT."
    - **Plain English:** Second step: ranked outputs (preference data) → final model ChatGPT.
    - **Technical terms:** preference data; ranking; preference tuning.

68. **Quote:** "A major benefit of using preference data over instruction data is the nuance it represents. By demonstrating the difference between a good and better output the generative model learns to generate text that resembles human preference. In Chapter 12, we will explore how these fine-tuning and preference-tuning methodologies work and how you can perform them yourself."
    - **Plain English:** Preference data adds nuance (good vs better); Ch 12 covers the methods.
    - **Word meanings:** nuance = subtle difference.
    - **Technical terms:** preference tuning; human preference.

69. **Quote:** "The process of using a closed sourced model is quite different from the open sourced examples we have seen thus far. Instead of loading the model, we can access the model through OpenAI's API."
    - **Plain English:** Closed-source models are accessed via API, not loaded locally.
    - **Technical terms:** OpenAI API.

70. **Quote:** "Before we go into the classification example, you will first need to create a free account on https://oreil.ly/AEXvA and create an API key here: https://oreil.ly/lrTXl. After doing so, you can use your API to communicate with OpenAI's servers."
    - **Plain English:** Requires a free account and API key.
    - **Technical terms:** API key.

### Code Block: Creating the OpenAI client and generation function
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
- **Explanation:** Creates the client; `chatgpt_generation` builds a system + user message, replaces `[DOCUMENT]`, calls the chat completions API at temperature=0, returns the assistant's content.
- **Fits the architecture:** API access to a closed-source model; temperature=0 for deterministic output.

### Code Block: The ChatGPT prompt template
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
- **Explanation:** The template instructs the model to return only 1 or 0; example document is "unpretentious, charming, quirky, original".
- **Fits the architecture:** Prompt engineering constrains the generative output.

71. **Quote:** "This template is merely an example and can be changed however you want. For now, we kept it as simple as possible to illustrate how to use such a template."
    - **Plain English:** Templates are customizable; kept simple for illustration.
    - **Technical terms:** prompt template.

72. **Quote:** "Before you use this over a potentially large dataset, it is important to always keep track of your usage. External APIs such as OpenAI's offering can quickly become costly if you perform many requests. At the time of writing, running our test dataset using the 'gpt-3.5-turbo-0125' model costs 3 cents, which is covered by the free account, but this might change in the future."
    - **Plain English:** Watch API usage; test set cost ~3 cents at writing (free-account covered).
    - **Technical terms:** API usage; cost; free account.

73. **Quote (note box):** "When dealing with external APIs, you might run into rate limit errors. These appear when you call the API too often as some APIs might limit the rate with which you can use it per minute or hour. To prevent these errors, we can implement several methods for retrying the request, including something referred to as exponential backoff. It performs a short sleep each time we hit a rate limit error and then retries the unsuccessful request. Whenever it is unsuccessful again, the sleep length is increased until the request is successful or we hit a maximum number of retries."
    - **Plain English:** Rate limits → retry with exponential backoff (increasing sleep between attempts).
    - **Technical terms:** rate limit; exponential backoff; retry.

### Code Block: Running ChatGPT over the test set and evaluating
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
- **Explanation:** Runs the API over the test set, converts outputs to ints, evaluates. F1 0.91.
- **Fits the architecture:** Closed-source generative classification via API.

74. **Quote:** "The F1 score of 0.91 already gives a glimpse into the performance of the model that brought generative AI to the masses. However, since we do not know what data the model was trained on, we cannot easily use these kinds of metrics for evaluating the model. For all we know, it might have actually been trained on our dataset!"
    - **Plain English:** F1 0.91, but unknown training data limits evaluation validity.
    - **Technical terms:** evaluation validity; training data leakage.

75. **Quote:** "In Chapter 12, we will explore how we can evaluate both open source and closed source models on more generalized tasks."
    - **Plain English:** Ch 12 covers evaluating models on generalized tasks.
    - **Technical terms:** generalized evaluation.

### Summary

76. **Quote:** "In this chapter, we discussed many different techniques for performing a wide variety of classification tasks, from fine-tuning your entire model to no tuning at all! Classifying textual data is not as straightforward as it may seem on the surface and there is an incredible amount of creative techniques for doing so."
    - **Plain English:** Many techniques from full fine-tuning to no tuning at all.
    - **Technical terms:** fine-tuning; zero-shot.

77. **Quote:** "We explored two types of representation models, a task-specific model and an embedding model. The task-specific model was pretrained on a large dataset specifically for sentiment analysis and showed us that pretrained models are a great technique for classifying documents. The embedding model was used to generate multipurpose embeddings that we used as the input to train a classifier."
    - **Plain English:** Recap of the two representation-model approaches.
    - **Technical terms:** task-specific model; embedding model; multipurpose embeddings.

78. **Quote:** "Similarly, we explored two types of generative models, an open source encoder-decoder model (Flan-T5) and a closed source decoder-only model (GPT-3.5). We used these generative models in text classification without requiring specific (additional) training on domain data or labeled datasets."
    - **Plain English:** Recap of the two generative-model approaches.
    - **Technical terms:** Flan-T5 (encoder-decoder); GPT-3.5 (decoder-only).

79. **Quote:** "In the next chapter, we will continue with classification but focus instead on unsupervised classification. What can we do if we have textual data without any labels? What information can we extract? We will focus on clustering our data as well as naming the clusters with topic modeling techniques."
    - **Plain English:** Ch 5: unsupervised classification — clustering + topic modeling.
    - **Technical terms:** unsupervised classification; clustering; topic modeling.
