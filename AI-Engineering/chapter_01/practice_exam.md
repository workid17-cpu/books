# Practice Exam — Chapter 1: Introduction to Building AI Applications with Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 1
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What does "scale" refer to in the chapter's opening about AI post-2020?
   a) The size of open source communities
   b) The enormous size and resource demands of AI models
   c) The number of AI startups
   d) The scale of salaries for AI engineers

2. Which of the following is a consequence of the high cost of training large language models?
   a) Everyone trains their own models
   b) Models are developed only by hobbyists
   c) Model as a service: a few organizations train models and offer them as services
   d) Training data became unlimited

3. What is a language model?
   a) A model that encodes statistical information about how likely a word is in a given context
   b) A model that only translates languages
   c) A dictionary of all English words
   d) A grammar checker

4. What is the basic unit of a language model?
   a) Character only
   b) Word only
   c) Sentence
   d) Token (which can be a character, word, or part of a word)

5. How does GPT-4 tokenize "I can't wait to build AI applications"?
   a) Into nine tokens, with "can't" split into "can" and "'t"
   b) Into five tokens
   c) Into 100 tokens
   d) Into words only

6. For GPT-4, an average token is approximately what fraction of the length of a word?
   a) 1/4
   b) 3/4
   c) 1/2
   d) 2/3

7. What is the vocabulary size of GPT-4?
   a) 32,000
   b) 100,256
   c) 50,000
   d) 1,000

8. Why do language models use tokens instead of words?
   a) Tokens break words into meaningful components and reduce vocabulary size
   b) Tokens are faster to generate than words
   c) Tokens are required by law in NLP
   d) Words are too short for training

9. A masked language model:
   a) Predicts the next token using only preceding tokens
   b) Is trained to fill in the blank using context from both before and after
   c) Cannot process text
   d) Only works on images

10. Which of the following is a well-known masked language model?
    a) GPT-4
    b) Mixtral 8x7B
    c) BERT
    d) CLIP

11. Autoregressive language models:
    a) Predict the next token using only preceding tokens and can generate text token by token
    b) Predict missing tokens in the middle of a sequence
    c) Only work on image data
    d) Cannot generate text

12. Which statement about the term "language model" in this book is true?
    a) It refers to masked language models only
    b) It refers to autoregressive models unless stated otherwise
    c) It refers to translation models
    d) It refers to image models

13. What does the phrase "completion machine" mean?
    a) A machine that finishes training
    b) A machine that fills forms
    c) A model that checks grammar
    d) A language model that completes a given text (prompt) with the most probable next tokens

14. Which task CANNOT be framed as a completion task according to the chapter?
    a) Translation
    b) Summarization
    c) Spam classification
    d) Framing tasks as completion is so general that all the above can be framed this way

15. What is supervision in ML?
    a) Training using labeled data, which can be expensive and slow to obtain
    b) Monitoring a model after deployment
    c) Training without any data
    d) Testing on unlabeled data

16. In self-supervision, labels are:
    a) Provided by human annotators
    b) Inferred from the input data itself
    c) Randomly generated
    d) Not needed at all (this is unsupervised learning)

17. How many training samples does the sentence "I love street food." give for language modeling?
    a) Four
    b) Ten
    c) Six
    d) One

18. What do <BOS> and <EOS> represent?
    a) Tokens that represent numbers
    b) Padding tokens only
    c) Beginning and end of a sequence markers
    d) Image tokens

19. The <EOS> marker is especially important because:
    a) It marks the most important word
    b) It helps language models know when to end their responses
    c) It speeds up training
    d) It is required for translation

20. What is a parameter in an ML model?
    a) A hyperparameter set by the user
    b) A variable updated through the training process
    c) A constant never changed
    d) A type of token

21. GPT-1 (June 2018) had how many parameters?
    a) 1.5 billion
    b) 100 billion
    c) 117 million
    d) 1 million

22. Why do larger models need more data?
    a) Because they are slower
    b) Because they overfit easily regardless
    c) Because they have more capacity to learn and need data to maximize performance
    d) They don't need more data; this is a myth

23. What is a foundation model?
    a) A model trained only on images
    b) A small task-specific model
    c) A large model that can be built upon for different needs (LLMs + LMMs)
    d) A model that doesn't use transformers

24. A generative multimodal model is called a:
    a) GAN
    b) Large multimodal model (LMM)
    c) BERT
    d) Tokenizer

25. What is natural language supervision (as used to train CLIP)?
    a) Labeling every image manually
    b) Training only on text
    c) Using (image, text) pairs co-occurring on the internet as training data
    d) Using reinforcement learning

26. Which of the following is TRUE about CLIP?
    a) It is a generative model
    b) It is an embedding model producing joint embeddings of texts and images
    c) It cannot process text
    d) It was trained on ImageNet labels

27. What are the three common AI engineering adaptation techniques?
    a) Tokenization, translation, and summarization
    b) Quantization, distillation, and parallelism
    c) Prompt engineering, RAG, and finetuning
    d) Training, testing, and deployment

28. Adapting an existing model is roughly:
    a) 1 million examples and one weekend
    b) Impossible
    c) Ten examples and one weekend versus 1 million examples and six months
    d) More expensive than building from scratch

29. What is model as a service?
    a) Free models only
    b) A model that serves customers
    c) A type of database
    d) Models exposed via APIs that receive user queries and return outputs

30. According to Goldman Sachs, AI investment could approach how much in the US by 2025?
    a) $200 billion
    b) $1 billion
    c) $900 billion
    d) $100 billion

31. According to Eloundou et al. (2023), a task is "exposed" if AI can reduce the time to complete it by at least:
    a) 25%
    b) 75%
    c) 100%
    d) 50%

32. Which occupation has NO exposure to AI according to the study?
    a) Interpreters
    b) Tax preparers
    c) Writers
    d) Cooks

33. GitHub Copilot's annual recurring revenue crossed $100 million:
    a) Ten years after its launch
    b) Six months after its launch
    c) One week after its launch
    d) Two years after its launch

34. In the MIT study (Noy and Zhang, 2023), ChatGPT exposure decreased writing time by:
    a) 18%
    b) 75%
    c) 10%
    d) 40%

35. The Chegg share price fell from $28 (Nov 2022) to $2 (Sept 2024). Why?
    a) It was acquired by OpenAI
    b) The company lost its data
    c) Regulation forced it to close
    d) Students turned to AI for homework help

36. According to Salesforce's 2023 research, what percentage of generative AI users use it to distill complex ideas and summarize information?
    a) 74%
    b) 40%
    c) 18%
    d) 95%

37. AIs that can plan and use external tools are called:
    a) Agents
    b) Tokens
    c) Parameters
    d) Embeddings

38. Microsoft's framework for gradually increasing AI automation is:
    a) Crawl-Walk-Run
    b) Run-Walk-Crawl
    c) Jump-Fly-Go
    d) Start-Stop-Go

39. What are the three types of competitive advantages in AI?
    a) Technology, data, and distribution
    b) Speed, cost, and quality
    c) Compute, talent, and money
    d) Prompts, models, and data

40. In the December 2023 Gemini MMLU comparison, Google evaluated Gemini using CoT@32. What does CoT@32 mean?
    a) 32 chain-of-thought examples were shown to Gemini
    b) 32 models were compared
    c) A 32-layer transformer was used
    d) 32 tokens of output were generated

## Section B: True/False (1 point each)

41. **T / F** — Post-2020 AI is characterized primarily by scale.

42. **T / F** — A masked language model predicts the next token using only preceding tokens.

43. **T / F** — Self-supervision and unsupervised learning are exactly the same.

44. **T / F** — <EOS> helps language models know when to end their responses.

45. **T / F** — GPT-2 (2019) with 1.5 billion parameters made the previous 117M-parameter GPT-1 seem small.

46. **T / F** — CLIP is a generative model trained to produce open-ended outputs.

47. **T / F** — Foundation models are task-specific models.

48. **T / F** — Feeding journal entries into ChatGPT via context is technically prompt engineering, not training.

49. **T / F** — Quantization changes model weight values and is considered training.

50. **T / F** — Pre-training is usually the most resource-intensive phase (up to 98% of compute for InstructGPT).

51. **T / F** — Prompt-based adaptation techniques require updating model weights.

52. **T / F** — Reactive features (like a chatbot) are those that respond to users' requests.

53. **T / F** — Proactive features (like Google Maps traffic alerts) typically have a higher quality bar because they can be intrusive.

54. **T / F** — In the Gemini MMLU story, when both Gemini and ChatGPT were shown five examples, ChatGPT performed better.

55. **T / F** — AI engineering focuses more on modeling and training than on model adaptation and evaluation.

## Section C: Short Answer (model answers)

56. Trace the four-step evolution from language models to AI engineering.

57. Explain how self-supervision overcomes the data labeling bottleneck, using "I love street food." as an example.

58. Contrast masked and autoregressive language models, and state which one the book uses by default.

59. What are the three adaptation techniques, and which ones change model weights?

60. State the three differences between AI engineering and ML engineering.

61. What are the three layers of the AI engineering stack, and what does each contain?

62. Explain the Gemini MMLU evaluation controversy and what lesson it teaches about evaluation.

63. Distinguish pre-training, finetuning, and post-training, and clarify why quantization is not training.

64. What is the Crawl-Walk-Run framework and how might the role of humans change over time?

65. Why is evaluation a much bigger problem in AI engineering than in traditional ML engineering?

## Section D: Essay (grading notes)

66. Explain the role of self-supervision in the scaling of LLMs. Cover: supervision vs self-supervision, the labeling bottleneck and its cost, training samples from a sentence, BOS/EOS, and why text abundance enabled scale.

67. Describe the three factors that created ideal conditions for the rapid growth of AI engineering, with supporting evidence (investments, entrance barrier, general-purpose capabilities).

68. Discuss the use-case landscape: the 8 groups from Table 1-3, the enterprise preference for low-risk internal/close-ended applications, and evidence for coding as the most popular use case.

69. Explain the build-or-buy decision, product defensibility, and the three types of competitive advantages, using Calendly/Mailchimp/Photoroom and the "layer on top of a foundation model" risk.

70. Compare AI engineering to both ML engineering and full-stack engineering: three key differences vs ML engineering, the shift toward JavaScript tooling and interfaces, and the change in development workflow (product-first).

---

## Answer Key

### Section A: Multiple Choice
1. b
2. c
3. a
4. d
5. a
6. b
7. b
8. a
9. b
10. c
11. a
12. b
13. d
14. d
15. a
16. b
17. c
18. c
19. b
20. b
21. c
22. c
23. c
24. b
25. c
26. b
27. c
28. c
29. d
30. d
31. d
32. d
33. d
34. d
35. d
36. a
37. a
38. a
39. a
40. a

### Section B: True/False
41. **T** — Scale is the defining word for post-2020 AI.
42. **F** — A masked LM predicts missing tokens using context from BOTH before and after; it fills in the blank.
43. **F** — Self-supervision infers labels from input; unsupervised learning needs no labels at all.
44. **T** — <EOS> helps models know when to end responses.
45. **T** — GPT-2's 1.5B parameters made GPT-1's 117M seem small.
46. **F** — CLIP is an EMBEDDING model, not a generative model.
47. **F** — Foundation models are general-purpose models, not task-specific.
48. **T** — Feeding context via prompts is prompt engineering, not training/finetuning.
49. **F** — Quantization changes weight values but is NOT considered training.
50. **T** — Pre-training is the most resource-intensive phase (up to 98% for InstructGPT).
51. **F** — Prompt-based techniques adapt WITHOUT updating weights.
52. **T** — Reactive features respond to users' requests.
53. **T** — Proactive features have a higher quality bar because they can be intrusive.
54. **T** — With 5 examples for both, ChatGPT performed better.
55. **F** — AI engineering focuses MORE on model adaptation and evaluation, less on modeling/training.

### Section C: Short Answer (model answers)
56. **Evolution.** Language models (statistical next-token predictors, since the 1950s) → large language models (LLMs, enabled by self-supervision on massive text corpora) → foundation models (LLMs + LMMs handling multiple modalities, general-purpose) → AI engineering (building applications on top of these readily available models).

57. **Self-supervision.** Supervision requires labeled data (e.g., 5¢/image → $50k for 1M images); self-supervision infers labels from the input itself. "I love street food." yields six (context → next-token) training samples with <BOS>/<EOS>: each prefix predicts the next token. Because text is everywhere, models scale on unlabeled web text without human annotation.

58. **Masked vs autoregressive.** Masked LMs predict missing tokens using context from both sides (fill-in-the-blank; BERT), good for non-generative tasks. Autoregressive LMs predict the next token using only preceding tokens and generate one token after another. The book uses autoregressive as the default meaning of "language model."

59. **Three techniques.** Prompt engineering (no weight change), RAG — retrieval-augmented generation (no weight change; adds external context), finetuning (changes weights).

60. **Three differences.** (1) Use a pretrained model → focus on model adaptation, less modeling/training; (2) bigger, more compute- and latency-heavy models → more pressure for inference optimization and GPU-cluster expertise; (3) open-ended outputs → evaluation is a much bigger problem.

61. **Stack.** Application development (prompts, context, evaluation, interfaces) → model development (modeling/training, dataset engineering, inference optimization) → infrastructure (model serving, data/compute management, monitoring). Start at top, move down as needed.

62. **Gemini MMLU.** Google claimed Gemini beat ChatGPT on MMLU, but evaluated Gemini with CoT@32 (32 examples) while ChatGPT was shown only 5. When both used 5 examples, ChatGPT performed better. Lesson: evaluation settings (prompt technique, number of examples) must be identical and reported to make comparisons fair.

63. **Training terms.** Pre-training = from scratch, random weights, text completion, most resource-intensive (up to 98% for InstructGPT). Finetuning = continuing training of a pretrained model (fewer resources). Post-training = training after pre-training, conceptually same as finetuning, but usually done by model developers (instruction-following) while finetuning is by application developers. Quantization reduces weight precision and changes weight values but isn't training.

64. **Crawl-Walk-Run.** Crawl = human involvement mandatory; Walk = AI interacts with internal employees; Run = increased automation, direct AI interaction with external users. As quality improves (e.g., 95% verbatim acceptance of AI-suggested responses), you escalate from human-reviewed suggestions to direct AI–customer interaction.

65. **Evaluation difficulty.** Close-ended tasks (fraud detection) have ground truths to compare against. Foundation models produce open-ended outputs — chatbots have too many valid responses to curate exhaustive ground-truth lists. Adaptation techniques (prompts) also change scores, so comparisons must control for settings; hence evaluation is much bigger problem.

### Section D: Essay (grading notes)
66. **Expect** supervision uses labeled data (expensive: 5¢/image → $50k per 1M, $50M for 1M categories); self-supervision infers labels from input; each sequence gives contexts + next-token labels ("I love street food." → 6 samples, Table 1-1); <BOS>/<EOS> delimit sequences, <EOS> ends responses; web-scale text (books, blogs, Reddit) → massive unlabeled corpora → LLM scaling; self-supervision ≠ unsupervised learning (no labels at all).

67. **Expect** Factor 1: general-purpose capabilities (communication tasks, image/video generation, even writing code/data synthesis); Factor 2: investments (Scribd cost down 2 orders of magnitude, Goldman ~$100B US/$200B global by 2025, 1 in 3 S&P 500 companies mention AI in Q2 2023 earnings, stock +4.6% vs 2.4%); Factor 3: low entrance barrier (model-as-a-service APIs, AI writes code, plain English); foundation-model dev limited to big corps/governments/well-funded startups (Sam Altman Sept 2022 quote); GitHub star evidence (AutoGPT, Stable Diffusion web UI, LangChain, Ollama surpassed Bitcoin in 2 years).

68. **Expect** 8 groups (Coding, Image and video production, Writing, Education, Conversational bots, Information aggregation, Data organization, Workflow automation); methodology (50 company interviews, 100+ case studies, 205 open-source apps with ≥500 stars); enterprise prefers internal-facing (a16z 2024) and close-ended classification (easier to evaluate, lower risk); coding most popular (Copilot >$100M ARR in 2 years, Magic $320M, Anysphere $60M, gpt-engineer/screenshot-to-code 50k stars; McKinsey 2× docs, +25–50% code gen, minimal complex; AI better at frontend).

69. **Expect** build-or-buy: existential threat → in-house, profit/productivity boost → buy; defensibility: low barrier = blessing and curse; layer-on-top risk (base model improves → subsumed; PDF-parsing example); three advantages: technology, data, distribution; data moat via usage-data flywheel (first-mover); Calendly/Mailchimp/Photoroom could've been features of bigger products; startups can overtake by building overlooked features.

70. **Expect** vs ML engineering: 3 differences (adaptation vs training; bigger/compute-heavy → inference optimization + GPU clusters; open-ended → harder evaluation); stack shifts toward JS tooling (LangChain.js, Transformers.js, OpenAI Node library, Vercel AI SDK) and interfaces (standalone apps, extensions, chat bots, plugins, voice, embodied); traditional ML = data+model first, product last; AI engineering = build product first, invest in data/models once it shows promise (Figure 1-16, after Shawn Wang 2023); AI engineers more involved in product building.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely tokenization details, self-supervision vs unsupervised, training terminology, adaptation techniques, or the Gemini MMLU story).
- **Below 70%**: Re-read the chapter study notes and retake.
