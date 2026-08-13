# Chapter 1 Line-by-Line: Introduction to Building AI Applications with Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 1
**Format:** Each numbered item quotes a passage (or closely paraphrases it), then gives a plain-English explanation, word meanings, and technical terms. Figures and tables annotated.

---

## Opening

1. **Quote:** "If I could use only one word to describe AI post-2020, it'd be scale. The AI models behind applications like ChatGPT, Google's Gemini, and Midjourney are at such a scale that they're consuming a nontrivial portion of the world's electricity, and we're at risk of running out of publicly available internet data to train them."
   - **Plain English:** Post-2020 AI is defined by enormous scale — models are so big they use significant electricity and may exhaust public training data.
   - **Word meanings:** nontrivial = significant/substantial; post-2020 = after the year 2020.
   - **Technical terms:** scale; training data; ChatGPT/Gemini/Midjourney.

2. **Quote:** "The scaling up of AI models has two major consequences. First, AI models are becoming more powerful and capable of more tasks, enabling more applications... Second, training large language models (LLMs) requires data, compute resources, and specialized talent that only a few organizations can afford. This has led to the emergence of model as a service: models developed by these few organizations are made available for others to use as a service."
   - **Plain English:** Scale has two effects: (1) models do more, enabling more apps; (2) training is so expensive only a few orgs can afford it, so those orgs offer their models as services.
   - **Word meanings:** compute resources = processing power (GPUs); specialized talent = rare experts.
   - **Technical terms:** LLM; model as a service.

3. **Quote:** "In short, the demand for AI applications has increased while the barrier to entry for building AI applications has decreased. This has turned AI engineering—the process of building applications on top of readily available models—into one of the fastest-growing engineering disciplines."
   - **Plain English:** More demand + lower barrier = AI engineering booms.
   - **Word meanings:** barrier to entry = difficulty of starting; readily available = easy to get.
   - **Technical terms:** AI engineering; foundation of the book's definition.

4. **Quote (footnote):** "In this book, I use traditional ML to refer to all ML before foundation models."
   - **Plain English:** "Traditional ML" = everything in machine learning before foundation models existed.
   - **Technical terms:** traditional ML; foundation models.

5. **Quote:** "Building applications on top of machine learning (ML) models isn't new. Long before LLMs became prominent, AI was already powering many applications, including product recommendations, fraud detection, and churn prediction. While many principles of productionizing AI applications remain the same, the new generation of large-scale, readily available models brings about new possibilities and new challenges, which are the focus of this book."
   - **Plain English:** AI applications aren't new; the new models bring both new opportunities and new problems.
   - **Word meanings:** churn = customer leaving; productionizing = making ready for real-world use.
   - **Technical terms:** ML; product recommendations; fraud detection; churn prediction.

6. **Quote:** "This chapter begins with an overview of foundation models, the key catalyst behind the explosion of AI engineering... existing application patterns can help uncover opportunities today and offer clues about how AI may continue to be used in the future."
   - **Plain English:** The chapter explains foundation models and studies existing AI application patterns.
   - **Word meanings:** catalyst = accelerator/trigger.
   - **Technical terms:** foundation models; application patterns.

## The Rise of AI Engineering

7. **Quote:** "Foundation models emerged from large language models, which, in turn, originated as just language models. While applications like ChatGPT and GitHub's Copilot may seem to have come out of nowhere, they are the culmination of decades of technology advancements, with the first language models emerging in the 1950s."
   - **Plain English:** The evolution runs language models → LLMs → foundation models; ChatGPT is the result of decades of work.
   - **Word meanings:** culmination = peak result.
   - **Technical terms:** language models; LLMs; foundation models.

### From Language Models to Large Language Models

8. **Quote:** "A language model encodes statistical information about one or more languages. Intuitively, this information tells us how likely a word is to appear in a given context. For example, given the context 'My favorite color is __', a language model that encodes English should predict 'blue' more often than 'car'."
   - **Plain English:** A language model stores statistics about how likely words are in context; it fills blanks with the most probable word.
   - **Word meanings:** encodes = stores/represents.
   - **Technical terms:** language model; context; probability.

9. **Quote:** "The statistical nature of languages was discovered centuries ago. In the 1905 story 'The Adventure of the Dancing Men', Sherlock Holmes leveraged simple statistical information of English to decode sequences of mysterious stick figures. Since the most common letter in English is E, Holmes deduced that the most common stick figure must stand for E."
   - **Plain English:** Language statistics are old; Sherlock Holmes used letter frequency to break a code.
   - **Word meanings:** leveraged = used; stick figures = simple drawn human figures.
   - **Technical terms:** statistical/letter-frequency analysis.

10. **Quote:** "Later on, Claude Shannon used more sophisticated statistics to decipher enemies' messages during the Second World War. His work on how to model English was published in his 1951 landmark paper 'Prediction and Entropy of Printed English'. Many concepts introduced in this paper, including entropy, are still used for language modeling today."
    - **Plain English:** Shannon did WWII code-breaking and his 1951 paper introduced ideas (like entropy) still used in language modeling.
    - **Word meanings:** decipher = decode; landmark = ground-breaking; entropy = a measure of information/unpredictability.
    - **Technical terms:** entropy; Shannon; statistical language modeling.

11. **Quote:** "The basic unit of a language model is token. A token can be a character, a word, or a part of a word (like -tion), depending on the model. For example, GPT-4... breaks the phrase 'I can't wait to build AI applications' into nine tokens... the word 'can't' is broken into two tokens, can and 't."
    - **Plain English:** Tokens are the model's units (characters, words, or word-parts); GPT-4 splits the example into 9 tokens, and "can't" becomes two.
    - **Word meanings:** unit = basic building block.
    - **Technical terms:** token; tokenization; GPT-4. (Footnote: a single Unicode character can be multiple tokens in non-English languages.)

12. **Quote:** "The process of breaking the original text into tokens is called tokenization. For GPT-4, an average token is approximately ¾ the length of a word. So, 100 tokens are approximately 75 words. The set of all tokens a model can work with is the model's vocabulary... The Mixtral 8x7B model has a vocabulary size of 32,000. GPT-4's vocabulary size is 100,256."
    - **Plain English:** Tokenization breaks text into tokens; ~100 tokens ≈ 75 words; vocabulary sizes vary (Mixtral 32k, GPT-4 100,256).
    - **Word meanings:** approximately = about.
    - **Technical terms:** tokenization; vocabulary; Mixtral 8x7B.

13. **Quote (three reasons for tokens over words/characters):** "Compared to characters, tokens allow the model to break words into meaningful components. For example, 'cooking' can be broken into 'cook' and 'ing'... Because there are fewer unique tokens than unique words, this reduces the model's vocabulary size, making the model more efficient... Tokens also help the model process unknown words. For instance, a made-up word like 'chatgpting' could be split into 'chatgpt' and 'ing'... Tokens balance having fewer units than words while retaining more meaning than individual characters."
    - **Plain English:** Tokens (1) keep meaning (cook+ing), (2) shrink vocabulary for efficiency, (3) handle unknown words.
    - **Word meanings:** retains = keeps.
    - **Technical terms:** sub-word tokens; vocabulary size; out-of-vocabulary words.

14. **Quote:** "Masked language model... trained to predict missing tokens anywhere in a sequence, using the context from both before and after the missing tokens. In essence, a masked language model is trained to be able to fill in the blank... A well-known example... is bidirectional encoder representations from transformers, or BERT (Devlin et al., 2018)."
    - **Plain English:** Masked LMs fill in blanks using both left and right context; BERT is the famous example.
    - **Word meanings:** masked = hidden/blanked.
    - **Technical terms:** masked language model; BERT; bidirectional context.

15. **Quote:** "Autoregressive language model... is trained to predict the next token in a sequence, using only the preceding tokens... An autoregressive model can continually generate one token after another. Today, autoregressive language models are the models of choice for text generation, and for this reason, they are much more popular than masked language models."
    - **Plain English:** Autoregressive LMs predict the next token from previous tokens and can generate text token-by-token; they dominate text generation.
    - **Word meanings:** preceding = before; continually = repeatedly.
    - **Technical terms:** autoregressive (causal) language model; next-token prediction. (Footnote: autoregressive LMs are sometimes called causal language models.)

16. **Quote:** "In this book, unless explicitly stated, language model will refer to an autoregressive model."
    - **Plain English:** The book's default meaning of "language model" is the autoregressive type.
    - **Technical terms:** autoregressive model.

17. **Quote:** "The outputs of language models are open-ended. A language model can use its fixed, finite vocabulary to construct infinite possible outputs. A model that can generate open-ended outputs is called generative, hence the term generative AI. You can think of a language model as a completion machine: given a text (prompt), it tries to complete that text. Prompt (from user): 'To be or not to be' → Completion: ', that is the question.'"
    - **Plain English:** Models produce open-ended completions; they're "completion machines," not answer machines.
    - **Word meanings:** open-ended = not limited to fixed answers.
    - **Technical terms:** generative AI; completion; prompt. (Figure 1-1: GPT-4 tokenization.)

18. **Quote:** "It's important to note that completions are predictions, based on probabilities, and not guaranteed to be correct. This probabilistic nature of language models makes them both so exciting and frustrating to use."
    - **Plain English:** Completions are probability-based guesses, not guaranteed-correct answers.
    - **Word meanings:** probabilistic = based on chance/probability.
    - **Technical terms:** probabilistic outputs (explored in Chapter 2).

19. **Quote:** "As simple as it sounds, completion is incredibly powerful. Many tasks, including translation, summarization, coding, and solving math problems, can be framed as completion tasks. For example, given the prompt: 'How are you in French is …', a language model might be able to complete it with: 'Comment ça va'... given the prompt: 'Question: Is this email likely spam? Here's the email: <email content> Answer:' ... it might complete with 'Likely spam', which turns this language model into a spam classifier."
    - **Plain English:** Completion can be framed for translation, summarization, coding, math, even spam classification.
    - **Word meanings:** framed = set up as.
    - **Technical terms:** task framing; completion-based classification.

20. **Quote:** "While completion is powerful, completion isn't the same as engaging in a conversation. For example, if you ask a completion machine a question, it can complete what you said by adding another question instead of answering the question. 'Post-Training' on page 78 discusses how to make a model respond appropriately to a user's request."
    - **Plain English:** Completion ≠ conversation; making a model answer properly comes from post-training.
    - **Word meanings:** engaging = participating.
    - **Technical terms:** completion vs conversation; post-training (cross-reference).

### Self-supervision

21. **Quote:** "Language modeling is just one of many ML algorithms... What's special about language models that made them the center of the scaling approach that caused the ChatGPT moment? The answer is that language models can be trained using self-supervision, while many other models require supervision. Supervision refers to the process of training ML algorithms using labeled data, which can be expensive and slow to obtain. Self-supervision helps overcome this data labeling bottleneck to create larger datasets for models to learn from, effectively allowing models to scale up."
    - **Plain English:** LMs are special because they can be trained with self-supervision, avoiding the expensive labeling that other models need — which enabled scaling.
    - **Word meanings:** bottleneck = limiting step.
    - **Technical terms:** self-supervision; supervision; labeled data; data labeling.

22. **Quote:** "The success of AI models in the 2010s lay in supervision. The model that started the deep learning revolution, AlexNet (Krizhevsky et al., 2012), was supervised. It was trained to learn how to classify over 1 million images in the dataset ImageNet. It classified each image into one of 1,000 categories such as 'car', 'balloon', or 'monkey'."
    - **Plain English:** The 2010s deep learning revolution (AlexNet on ImageNet) used supervised learning.
    - **Technical terms:** AlexNet; ImageNet; supervised learning.

23. **Quote:** "A drawback of supervision is that data labeling is expensive and time-consuming. If it costs 5 cents for one person to label one image, it'd cost $50,000 to label a million images for ImageNet. If you want two different people to label each image—so that you could cross-check label quality—it'd cost twice as much... To scale up to 1 million categories, the labeling cost alone would increase to $50 million."
    - **Plain English:** Labeling is expensive; a million categories could cost $50M just in labeling.
    - **Word meanings:** drawback = downside.
    - **Technical terms:** labeling cost; quality cross-checking. (Footnote: SageMaker Ground Truth charges 8¢/image under 50k, 2¢/image over 1M, Sept 2024.)

24. **Quote:** "Self-supervision helps overcome the data labeling bottleneck. In self-supervision, instead of requiring explicit labels, the model can infer labels from the input data. Language modeling is self-supervised because each input sequence provides both the labels (tokens to be predicted) and the contexts the model can use to predict these labels. For example, the sentence 'I love street food.' gives six training samples, as shown in Table 1-1."
    - **Plain English:** Self-supervision infers labels from the data itself; one sentence yields six training samples.
    - **Word meanings:** infer = deduce.
    - **Technical terms:** self-supervision; training samples (Table 1-1).

25. **Table 1-1 annotation (Training samples for "I love street food."):**
    | Input (context) | Output (next token) |
    |---|---|
    | `<BOS>` | I |
    | `<BOS>, I` | love |
    | `<BOS>, I, love` | street |
    | `<BOS>, I, love, street` | food |
    | `<BOS>, I, love, street, food` | . |
    | `<BOS>, I, love, street, food, .` | `<EOS>` |
    - **Plain English:** Each prefix predicts the next token; the sentence yields 6 samples with start/end markers.
    - **Word meanings:** context = what the model has seen so far.
    - **Technical terms:** `<BOS>` (begin-of-sequence), `<EOS>` (end-of-sequence) special tokens; next-token prediction.

26. **Quote:** "In Table 1-1, <BOS> and <EOS> mark the beginning and the end of a sequence. These markers are necessary for a language model to work with multiple sequences. Each marker is typically treated as one special token by the model. The end-of-sequence marker is especially important as it helps language models know when to end their responses."
    - **Plain English:** `<BOS>`/`<EOS>` delimit sequences; `<EOS>` tells the model when to stop generating.
    - **Technical terms:** special tokens; `<EOS>`; response termination. (Footnote: like knowing when to stop talking.)

27. **Quote (box):** "Self-supervision differs from unsupervision. In self-supervised learning, labels are inferred from the input data. In unsupervised learning, you don't need labels at all."
    - **Plain English:** Self-supervision infers labels from input; unsupervised learning needs no labels — they differ.
    - **Technical terms:** self-supervised vs unsupervised learning. (Common exam trap.)

28. **Quote:** "Because text sequences are everywhere—in books, blog posts, articles, and Reddit comments—it's possible to construct a massive amount of training data, allowing language models to scale up to become LLMs."
    - **Plain English:** Text is everywhere, so self-supervised LMs can be trained on massive corpora → LLMs.
    - **Word meanings:** corpus/corpora = text collections.
    - **Technical terms:** web-scale training data; LLM.

29. **Quote:** "LLM, however, is hardly a scientific term. How large does a language model have to be to be considered large? What is large today might be considered tiny tomorrow. A model's size is typically measured by its number of parameters... In general, though this is not always true, the more parameters a model has, the greater its capacity to learn desired behaviors. When OpenAI's first generative pre-trained transformer (GPT) model came out in June 2018, it had 117 million parameters, and that was considered large. In February 2019, when OpenAI introduced GPT-2 with 1.5 billion parameters, 117 million was downgraded to be considered small. As of the writing of this book, a model with 100 billion parameters is considered large."
    - **Plain English:** "Large" is fuzzy; size = parameter count; 117M (2018) → 1.5B (2019) → ~100B (now) is the progression.
    - **Word meanings:** downgraded = reduced in status.
    - **Technical terms:** parameters; capacity; GPT-1; GPT-2. (Footnote: "weights" now generically means all parameters, incl. biases.)

30. **Quote (footnote):** "It seems counterintuitive that larger models require more training data. If a model is more powerful, shouldn't it require fewer examples to learn from? However, we're not trying to get a large model to match the performance of a small model using the same data. We're trying to maximize model performance."
    - **Plain English:** Larger models need more data to *maximize* performance, not just to match a smaller model.
    - **Word meanings:** counterintuitive = opposite of what you'd expect.
    - **Technical terms:** model capacity; data requirements.

31. **Quote:** "Why do larger models need more data? Larger models have more capacity to learn, and, therefore, would need more training data to maximize their performance. You can train a large model on a small dataset too, but it'd be a waste of compute. You could have achieved similar or better results on this dataset with smaller models."
    - **Plain English:** Big models need lots of data; training a big model on little data wastes compute.
    - **Technical terms:** capacity; compute waste; data scaling.

### From Large Language Models to Foundation Models

32. **Quote:** "While language models are capable of incredible tasks, they are limited to text. As humans, we perceive the world not just via language but also through vision, hearing, touch, and more. Being able to process data beyond text is essential for AI to operate in the real world... GPT-4V and Claude 3 can understand images and texts. Some models even understand videos, 3D assets, protein structures, and so on."
    - **Plain English:** Text-only LMs are limited; multimodal models (GPT-4V, Claude 3) handle images and more.
    - **Word meanings:** perceive = take in through senses; modalities = data types.
    - **Technical terms:** modality; multimodal models; GPT-4V; Claude 3.

33. **Quote:** "While many people still call Gemini and GPT-4V LLMs, they're better characterized as foundation models. The word foundation signifies both the importance of these models in AI applications and the fact that they can be built upon for different needs."
    - **Plain English:** Gemini/GPT-4V are foundation models — important bases that others build on.
    - **Technical terms:** foundation model.

34. **Quote:** "Foundation models mark a breakthrough from the traditional structure of AI research. For a long time, AI research was divided by data modalities. Natural language processing (NLP) deals only with text. Computer vision deals only with vision... A model that can work with more than one data modality is also called a multimodal model. A generative multimodal model is also called a large multimodal model (LMM). If a language model generates the next token conditioned on text-only tokens, a multimodal model generates the next token conditioned on both text and image tokens... as shown in Figure 1-3."
    - **Plain English:** AI used to be split by data type (NLP/vision/audio); foundation models break that; generative multimodal models are LMMs.
    - **Word meanings:** breakthrough = major advance.
    - **Technical terms:** NLP; computer vision; STT/TTS; multimodal model; LMM. (Figure 1-3: next token from text + image tokens.)

35. **Quote:** "Just like language models, multimodal models need data to scale up. Self-supervision works for multimodal models too. For example, OpenAI used a variant of self-supervision called natural language supervision to train their language-image model CLIP (OpenAI, 2021). Instead of manually generating labels for each image, they found (image, text) pairs that co-occurred on the internet. They were able to generate a dataset of 400 million (image, text) pairs, which was 400 times larger than ImageNet, without manual labeling cost. This dataset enabled CLIP to become the first model that could generalize to multiple image classification tasks without requiring additional training."
    - **Plain English:** CLIP used "natural language supervision" — scraping 400M (image, text) pairs (400× ImageNet) — to learn without manual labels.
    - **Word meanings:** co-occurred = appeared together.
    - **Technical terms:** natural language supervision; CLIP; (image, text) pairs; zero-shot generalization.

36. **Quote (box):** "This book uses the term foundation models to refer to both large language models and large multimodal models."
    - **Plain English:** In this book, "foundation models" = LLMs + LMMs.
    - **Technical terms:** foundation models; LLMs; LMMs.

37. **Quote:** "Note that CLIP isn't a generative model—it wasn't trained to generate open-ended outputs. CLIP is an embedding model, trained to produce joint embeddings of both texts and images... you can think of embeddings as vectors that aim to capture the meanings of the original data. Multimodal embedding models like CLIP are the backbones of generative multimodal models, such as Flamingo, LLaVA, and Gemini (previously Bard)."
    - **Plain English:** CLIP is an embedding (vector) model, not generative; it's the backbone of generative multimodal models.
    - **Word meanings:** backbone = core foundation.
    - **Technical terms:** embedding model; embeddings; Flamingo; LLaVA; Gemini/Bard.

38. **Quote:** "Foundation models also mark the transition from task-specific models to general-purpose models. Previously, models were often developed for specific tasks, such as sentiment analysis or translation. A model trained for sentiment analysis wouldn't be able to do translation, and vice versa. Foundation models, thanks to their scale and the way they are trained, are capable of a wide range of tasks. Out of the box, general-purpose models can work relatively well for many tasks. An LLM can do both sentiment analysis and translation."
    - **Plain English:** Foundation models are general-purpose (many tasks), unlike old task-specific models.
    - **Word meanings:** vice versa = the reverse also applies.
    - **Technical terms:** task-specific vs general-purpose models; Super-NaturalInstructions benchmark (Figure 1-4, Wang et al., 2022).

39. **Quote:** "Imagine you're working with a retailer to build an application to generate product descriptions for their website. An out-of-the-box model might be able to generate accurate descriptions but might fail to capture the brand's voice or highlight the brand's messaging... There are multiple techniques you can use to get the model to generate what you want. For example, you can craft detailed instructions with examples of the desirable product descriptions. This approach is prompt engineering. You can connect the model to a database of customer reviews... is called retrieval-augmented generation (RAG). You can also finetune—further train—the model on a dataset of high-quality product descriptions. Prompt engineering, RAG, and finetuning are three very common AI engineering techniques."
    - **Plain English:** To adapt a general model, use prompt engineering, RAG, or finetuning — the three core techniques.
    - **Word meanings:** out-of-the-box = without adaptation.
    - **Technical terms:** prompt engineering; RAG; finetuning; model adaptation.

40. **Quote:** "Adapting an existing powerful model to your task is generally a lot easier than building a model for your task from scratch—for example, ten examples and one weekend versus 1 million examples and six months. Foundation models make it cheaper to develop AI applications and reduce time to market... However, there are still many benefits to task-specific models, for example, they might be a lot smaller, making them faster and cheaper to use."
    - **Plain English:** Adaptation (10 examples, a weekend) beats building from scratch (1M examples, 6 months); but task-specific models can be smaller/faster/cheaper.
    - **Word meanings:** from scratch = from nothing.
    - **Technical terms:** model adaptation; time to market; task-specific models.

### From Foundation Models to AI Engineering

41. **Quote:** "AI engineering refers to the process of building applications on top of foundation models. People have been building AI applications for over a decade—a process often known as ML engineering or MLOps (short for ML operations). Why do we talk about AI engineering now? If traditional ML engineering involves developing ML models, AI engineering leverages existing ones."
    - **Plain English:** AI engineering builds apps on existing foundation models; ML engineering traditionally develops models.
    - **Word meanings:** leverages = builds on.
    - **Technical terms:** AI engineering; ML engineering; MLOps.

42. **Quote (Factor 1):** "General-purpose AI capabilities... Since AI can now write as well as humans, sometimes even better, AI can automate or partially automate every task that requires communication, which is pretty much everything... AI can even be used to synthesize training data, develop algorithms, and write code, all of which will help train even more powerful models in the future."
    - **Plain English:** AI's general abilities (writing, communication, code) expand what can be automated, driving demand.
    - **Word meanings:** synthesize = create from parts.
    - **Technical terms:** general-purpose AI; automation; data synthesis.

43. **Quote (Factor 2):** "Increased AI investments. The success of ChatGPT prompted a sharp increase in investments in AI, both from venture capitalists and enterprises... Matt Ross, a senior manager of applied research at Scribd, told me that the estimated AI cost for his use cases has gone down two orders of magnitude from April 2022 to April 2023. Goldman Sachs Research estimated that AI investment could approach $100 billion in the US and $200 billion globally by 2025... FactSet found that one in three S&P 500 companies mentioned AI in their earnings calls for the second quarter of 2023, three times more than did so the year earlier."
    - **Plain English:** ChatGPT triggered investment boom; costs dropped 100× in a year; ~$100B US / $200B global by 2025; 1 in 3 S&P 500 companies mention AI.
    - **Word meanings:** prompted = triggered; two orders of magnitude = ~100×.
    - **Technical terms:** AI investment; ROI; earnings calls (Figures 1-5). (Footnote: US public school spending ~$900B, only 9× US AI investment.)

44. **Quote:** "According to WallStreetZen, companies that mentioned AI in their earning calls saw their stock price increase more than those that didn't: an average of a 4.6% increase compared to 2.4%. It's unclear whether it's causation (AI makes these companies more successful) or correlation."
    - **Plain English:** AI-mentioning companies' stocks rose 4.6% vs 2.4%; cause vs correlation is unclear.
    - **Word meanings:** causation = cause; correlation = association.
    - **Technical terms:** correlation vs causation.

45. **Quote (Factor 3):** "Low entrance barrier to building AI applications. The model as a service approach popularized by OpenAI and other model providers makes it easier to leverage AI to build applications. In this approach, models are exposed via APIs that receive user queries and return model outputs... Not only that, AI also makes it possible to build applications with minimal coding. First, AI can write code for you... Second, you can work with these models in plain English instead of having to use a programming language. Anyone, and I mean anyone, can now develop AI applications."
    - **Plain English:** Low barrier: models via APIs, AI writes code, plain-English interaction — anyone can build.
    - **Word meanings:** exposed via = made available through.
    - **Technical terms:** model as a service; APIs; minimal coding; plain English prompts.

46. **Quote:** "Because of the resources it takes to develop foundation models, this process is possible only for big corporations (Google, Meta, Microsoft, Baidu, Tencent), governments (Japan, the UAE), and ambitious, well-funded startups (OpenAI, Anthropic, Mistral). In a September 2022 interview, Sam Altman, CEO of OpenAI, said that the biggest opportunity for the vast majority of people will be to adapt these models for specific applications."
    - **Plain English:** Only a few big players can build foundation models; the biggest opportunity for most is adapting them.
    - **Word meanings:** well-funded = having lots of money.
    - **Technical terms:** foundation-model development; model adaptation (Sam Altman quote).

47. **Quote:** "The world is quick to embrace this opportunity. AI engineering has rapidly emerged as one of the fastest, and quite possibly the fastest-growing, engineering discipline. Tools for AI engineering are gaining traction faster than any previous software engineering tools. Within just two years, four open source AI engineering tools (AutoGPT, Stable Diffusion web UI, LangChain, Ollama) have already garnered more stars on GitHub than Bitcoin. They are on track to surpass even the most popular web development frameworks, including React and Vue, in star count."
    - **Plain English:** AI engineering tools grew faster on GitHub than Bitcoin in 2 years, on track to pass React/Vue.
    - **Word meanings:** gained traction = became popular; garnered = collected.
    - **Technical terms:** GitHub stars; open source AI engineering tools (Figure 1-6).

48. **Quote:** "A LinkedIn survey from August 2023 shows that the number of professionals adding terms like 'Generative AI,' 'ChatGPT,' 'Prompt Engineering,' and 'Prompt Crafting' to their profile increased on average 75% each month. ComputerWorld declared that 'teaching AI to behave is the fastest-growing career skill'."
    - **Plain English:** AI-related profile keywords grew ~75%/month; "teaching AI to behave" is the fastest-growing career skill.
    - **Word meanings:** profession = career field.
    - **Technical terms:** prompt engineering; career skill growth.

### Why the Term "AI Engineering?"

49. **Quote:** "Many terms are being used to describe the process of building applications on top of foundation models, including ML engineering, MLOps, AIOps, LLMOps, etc. Why did I choose to go with AI engineering?... I didn't go with the term ML engineering because... working with foundation models differs from working with traditional ML models in several important aspects... However, ML engineering is a great term to encompass both processes. I didn't go with all the terms that end with 'Ops' because, while there are operational components of the process, the focus is more on tweaking (engineering) foundation models to do what you want. Finally, I surveyed 20 people who were developing applications on top of foundation models about what term they would use... Most people preferred AI engineering. I decided to go with the people."
    - **Plain English:** The book chose "AI engineering": not "ML engineering" (too narrow for the differences) and not "Ops" terms (too operational); a survey of 20 practitioners preferred it.
    - **Word meanings:** tweaking = adjusting/fine-tuning; encompass = include.
    - **Technical terms:** AI engineering vs MLOps/AIOps/LLMOps naming.

## Foundation Model Use Cases

50. **Quote:** "It's impossible to list all potential use cases for AI. Even attempting to categorize these use cases is challenging, as different surveys use different categorizations. For example, Amazon Web Services (AWS) has categorized enterprise generative AI use cases into three buckets: customer experience, employee productivity, and process optimization. A 2024 O'Reilly survey categorized the use cases into eight categories: programming, data analysis, customer support, marketing copy, other copy, research, web design, and art."
    - **Plain English:** Use-case lists/categorizations vary; AWS uses 3 buckets, O'Reilly 8 categories.
    - **Word meanings:** buckets = groups.
    - **Technical terms:** use-case categorization.

51. **Quote:** "Some organizations, like Deloitte, have categorized use cases by value capture, such as cost reduction, process efficiency, growth, and accelerating innovation. For value capture, Gartner has a category for business continuity, meaning an organization might go out of business if it doesn't adopt generative AI. Of the 2,500 executives Gartner surveyed in 2023, 7% cited business continuity as the motivation for embracing generative AI."
    - **Plain English:** Deloitte groups by value capture; Gartner adds "business continuity" (7% of 2,500 execs).
    - **Word meanings:** business continuity = surviving as a business.
    - **Technical terms:** value capture; business continuity.

52. **Quote:** "Eloundou et al. (2023) has excellent research on how exposed different occupations are to AI. They defined a task as exposed if AI and AI-powered software can reduce the time needed to complete this task by at least 50%. An occupation with 80% exposure means that 80% of the occupation's tasks are exposed. According to the study, occupations with 100% or close to 100% exposure include interpreters and translators, tax preparers, web designers, and writers. Not unsurprisingly, occupations with no exposure to AI include cooks, stonemasons, and athletes."
    - **Plain English:** A task is "exposed" if AI cuts its time ≥50%; exposed occupations include interpreters/tax preparers/web designers/writers; unaffected ones include cooks, stonemasons, athletes.
    - **Word meanings:** exposed = vulnerable to AI substitution.
    - **Technical terms:** task exposure; occupation exposure. (Table 1-2: Human α interpreters/translators 76.5%; Human β survey researchers 84.4%; Human ζ mathematicians 100.0%; 15 occupations labeled "fully exposed".)

53. **Quote:** "When analyzing the use cases, I looked at both enterprise and consumer applications. To understand enterprise use cases, I interviewed 50 companies on their AI strategies and read over 100 case studies. To understand consumer applications, I examined 205 open source AI applications with at least 500 stars on GitHub. I categorized applications into eight groups, as shown in Table 1-3."
    - **Plain English:** Methodology: 50 company interviews + 100+ case studies (enterprise); 205 open-source GitHub apps (consumer).
    - **Word meanings:** examined = studied.
    - **Technical terms:** use-case analysis (Table 1-3: 8 groups).

54. **Table 1-3 annotation (8 use-case groups):**
    | Category | Consumer examples | Enterprise examples |
    |---|---|---|
    | Coding | Coding | Coding |
    | Image and video production | Photo/video editing, Design, Presentation | Ad generation |
    | Writing | Email, Social media & blog posts | Copywriting/SEO, Reports/memos/design docs |
    | Education | Tutoring, Essay grading | Employee onboarding, Upskill training |
    | Conversational bots | General chatbot, AI companion | Customer support, Product copilots |
    | Information aggregation | Summarization, Talk-to-your-docs | Summarization, Market research |
    | Data organization | Image search, Memex | Knowledge management, Document processing |
    | Workflow automation | Travel planning, Event planning | Data extraction/entry/annotation, Lead generation |
    - **Plain English:** Applications fall into 8 broad groups; an app can belong to more than one.
    - **Technical terms:** application patterns; Table 1-3.

55. **Quote:** "The enterprise world generally prefers applications with lower risks. For example, a 2024 a16z Growth report showed that companies are faster to deploy internal-facing applications (internal knowledge management) than external-facing applications (customer support chatbots)... Internal applications help companies develop their AI engineering expertise while minimizing the risks associated with data privacy, compliance, and potential catastrophic failures. Similarly, while foundation models are open-ended and can be used for any task, many applications built on top of them are still close-ended, such as classification. Classification tasks are easier to evaluate, which makes their risks easier to estimate."
    - **Plain English:** Companies prefer low-risk, internal-facing apps first; close-ended tasks (classification) are easier to evaluate and de-risk.
    - **Word meanings:** catastrophic = disastrous.
    - **Technical terms:** internal vs external-facing apps; close-ended tasks (Figures 1-7, 1-8).

### Coding

56. **Quote:** "In multiple generative AI surveys, coding is hands down the most popular use case... One of the earliest successes of foundation models in production is the code completion tool GitHub Copilot, whose annual recurring revenue crossed $100 million only two years after its launch. As of this writing, AI-powered coding startups have raised hundreds of millions of dollars, with Magic raising $320 million and Anysphere raising $60 million, both in August 2024. Open source coding tools like gpt-engineer and screenshot-to-code both got 50,000 stars on GitHub within a year."
    - **Plain English:** Coding is the #1 use case; Copilot >$100M ARR in 2 years; Magic $320M / Anysphere $60M; gpt-engineer & screenshot-to-code 50k stars in a year.
    - **Word meanings:** hands down = without doubt.
    - **Technical terms:** ARR; GitHub Copilot; code completion.

57. **Quote (specialized coding tasks):** "Other than tools that help with general coding, many tools specialize in certain coding tasks... Extracting structured data from web pages and PDFs (AgentGPT) • Converting English to code (DB-GPT, SQL Chat, PandasAI) • Given a design or a screenshot, generating code that will render into a website that looks like the given image (screenshot-to-code, draw-a-ui) • Translating from one programming language or framework to another (GPT-Migrate, AI Code Translator) • Writing documentation (Autodoc) • Creating tests (PentestGPT) • Generating commit messages (AI Commits)."
    - **Plain English:** Specialized AI coding tools exist for data extraction, English→code, design→website, translation, docs, tests, and commit messages.
    - **Technical terms:** code generation; design-to-code; docs/tests automation.

58. **Quote:** "The question is whether AI can automate software engineering altogether. At one end of the spectrum, Jensen Huang, CEO of NVIDIA, predicts that AI will replace human software engineers and that we should stop saying kids should learn to code. In a leaked recording, AWS CEO Matt Garman shared that in the near future, most developers will stop coding... At the other end are many software engineers who are convinced that they will never be replaced by AI, both for technical and emotional reasons."
    - **Plain English:** Views range from "AI replaces engineers" (Huang) to "AI changes their jobs" (Garman) to "never."
    - **Word meanings:** spectrum = range of views.
    - **Technical terms:** AI automation of software engineering.

59. **Quote:** "Software engineering consists of many tasks. AI is better at some than others. McKinsey researchers found that AI can help developers be twice as productive for documentation, and 25–50% more productive for code generation and code refactoring. Minimal productivity improvement was observed for highly complex tasks... many told me that they've noticed that AI is much better at frontend development than backend development."
    - **Plain English:** McKinsey: 2× for docs, +25–50% for code gen/refactoring, minimal for complex tasks; AI better at frontend than backend.
    - **Word meanings:** refactoring = restructuring code.
    - **Technical terms:** developer productivity (Figure 1-9); frontend vs backend. (Footnote: 11% of company budget is marketing.)

### Image and Video Production

60. **Quote:** "Thanks to its probabilistic nature, AI is great for creative tasks. Some of the most successful AI startups are creative applications, such as Midjourney for image generation, Adobe Firefly for photo editing, and Runway, Pika Labs, and Sora for video generation. In late 2023, at one and a half years old, Midjourney had already generated $200 million in annual recurring revenue. As of December 2023, among the top 10 free apps for Graphics & Design on the Apple App Store, half have AI in their names."
    - **Plain English:** AI excels at creative tasks; Midjourney hit $200M ARR at 1.5 years; half of top-10 free design apps have "AI" in their names.
    - **Word meanings:** probabilistic = random-ish/creative.
    - **Technical terms:** image/video generation; ARR.

61. **Quote:** "It's now common to use AI to generate profile pictures for social media... In 2019, Facebook banned accounts using AI-generated profile photos for safety reasons. In 2023, many social media apps provide tools that let users use AI to generate profile photos. For enterprises, ads and marketing have been quick to incorporate AI... You can use AI to generate variations of your ads according to seasons and locations. For example, you can use AI to change leaf colors during fall or add snow to the ground during winter."
    - **Plain English:** AI profile pictures went from banned (2019) to built-in (2023); enterprises generate seasonal ad variations.
    - **Word meanings:** variations = different versions.
    - **Technical terms:** AI-generated imagery; ad personalization.

### Writing

62. **Quote:** "Writing is an ideal application for AI because we do it a lot, it can be quite tedious, and we have a high tolerance for mistakes... An MIT study (Noy and Zhang, 2023) assigned occupation-specific writing tasks to 453 college-educated professionals and randomly exposed half of them to ChatGPT. Their results show that among those exposed to ChatGPT, the average time taken decreased by 40% and output quality rose by 18%. ChatGPT helps close the gap in output quality between workers... Workers exposed to ChatGPT during the experiment were 2 times as likely to report using it in their real job two weeks after the experiment and 1.6 times as likely two months after that."
    - **Plain English:** Writing is ideal for AI; MIT study: ChatGPT cut writing time 40%, raised quality 18%, narrowed quality gaps; usage persisted (2× after 2 weeks, 1.6× after 2 months).
    - **Word meanings:** tedious = boring/repetitive; tolerance = acceptance.
    - **Technical terms:** writing automation (Noy & Zhang 2023).

63. **Quote:** "AI's ability to write can also be abused. In 2023, the New York Times reported that Amazon was flooded with shoddy AI-generated travel guidebooks, each outfitted with an author bio, a website, and rave reviews, all AI-generated."
    - **Plain English:** AI writing can be abused — e.g., fake AI-generated travel guidebooks on Amazon.
    - **Word meanings:** shoddy = low quality; rave = extremely positive.
    - **Technical terms:** AI content abuse.

64. **Quote:** "AI seems particularly good with SEO, perhaps because many AI models are trained with data from the internet, which is populated with SEO-optimized text. AI is so good at SEO that it has enabled a new generation of content farms. These farms set up junk websites and fill them with AI-generated content to get them to rank high on Google... In June 2023, NewsGuard identified almost 400 ads from 141 popular brands on junk AI-generated websites. One of those junk websites produced 1,200 articles a day."
    - **Plain English:** AI excels at SEO and powers content farms — ~400 ads on junk sites, one site making 1,200 articles/day.
    - **Word meanings:** junk = low-value/trash.
    - **Technical terms:** SEO; content farms; ad networks.

### Education

65. **Quote:** "Whenever ChatGPT is down, OpenAI's Discord server is flooded with students complaining about being unable to complete their homework. Several education boards, including the New York City Public Schools and the Los Angeles Unified School District, were quick to ban ChatGPT for fear of students using it for cheating, but reversed their decisions just a few months later."
    - **Plain English:** Students rely on ChatGPT; NYC and LA briefly banned then un-banned it.
    - **Word meanings:** reversed = turned around.
    - **Technical terms:** educational policy.

66. **Quote:** "AI can summarize textbooks and generate personalized lecture plans for each student... Pajak and Bicknell (Duolingo, 2022) found that out of four stages of course creation, lesson personalization is the stage that can benefit the most from AI, as shown in Figure 1-10."
    - **Plain English:** AI personalizes education; at Duolingo, lesson personalization benefits most from AI.
    - **Word meanings:** personalization = customizing to each learner.
    - **Technical terms:** course-creation stages (Figure 1-10, Duolingo).

67. **Quote:** "AI can generate quizzes, both multiple-choice and open-ended, and evaluate the answers... For example, Khan Academy offers AI-powered teaching assistants to students... An innovative teaching method I've seen is that teachers assign AI-generated essays for students to find and correct mistakes. While many education companies embrace AI to build better products, many find their lunches taken by AI. For example, Chegg, a company that helps students with their homework, saw its share price plummet from $28 when ChatGPT launched in November 2022 to $2 in September 2024, as students have been turning to AI for help."
    - **Plain English:** AI aids teaching (quizzes, TAs, error-finding); it also devastated Chegg ($28 → $2).
    - **Word meanings:** plummet = fall sharply.
    - **Technical terms:** AI tutoring; disruption of education companies.

### Conversational Bots

68. **Quote:** "Conversational bots are versatile. They can help us find information, explain concepts, and brainstorm ideas. AI can be your companion and therapist. It can emulate personalities, letting you talk to a digital copy of anyone you like... Many are already spending more time talking to bots than to humans... For enterprises, the most popular bots are customer support bots. They can help companies save costs while improving customer experience because they can respond to users sooner than human agents. AI can also be product copilots that guide customers through painful and confusing tasks such as filing insurance claims, doing taxes, or looking up corporate policies."
    - **Plain English:** Bots serve as companions, therapists, customer-support agents, and product copilots.
    - **Word meanings:** emulate = imitate; copilots = assistants.
    - **Technical terms:** conversational bots; product copilots; customer support automation.

69. **Quote:** "The success of ChatGPT prompted a wave of text-based conversational bots. However, text isn't the only interface for conversational agents. Voice assistants such as Google Assistant, Siri, and Alexa have been around for years. 3D conversational bots are already common in games... One use case of AI-powered 3D characters is smart NPCs, non-player characters (see NVIDIA's demos of Inworld and Convai). NPCs are essential for advancing the storyline of many games. Without AI, NPCs are typically scripted to do simple actions with a limited range of dialogues. AI can make these NPCs much smarter. Intelligent bots can change the dynamics of existing games like The Sims and Skyrim as well as enable new games never possible before."
    - **Plain English:** Bots aren't just text: voice assistants and 3D smart NPCs in games (NVIDIA's Inworld/Convai) advance storylines.
    - **Word meanings:** NPC = non-player character.
    - **Technical terms:** voice assistants; smart NPCs; 3D conversational bots.

### Information Aggregation

70. **Quote:** "Many people believe that our success depends on our ability to filter and digest useful information... According to Salesforce's 2023 Generative AI Snapshot Research, 74% of generative AI users use it to distill complex ideas and summarize information."
    - **Plain English:** AI aggregates/summarizes information; 74% of generative AI users use it for distillation.
    - **Word meanings:** distill = condense.
    - **Technical terms:** information aggregation; summarization.

71. **Quote:** "For consumers, many applications can process your documents—contracts, disclosures, papers—and let you retrieve information in a conversational manner. This use case is also called talk-to-your-docs... When Instacart launched an internal prompt marketplace, it discovered that one of the most popular prompt templates is 'Fast Breakdown'. This template asks AI to summarize meeting notes, emails, and Slack conversations with facts, open questions, and action items. These action items can then be automatically inserted into a project tracking tool and assigned to the right owners."
    - **Plain English:** "Talk-to-your-docs" lets users query their documents; Instacart's "Fast Breakdown" template summarizes meetings/emails/Slack into facts, questions, and action items.
    - **Word meanings:** disclosures = formal documents.
    - **Technical terms:** talk-to-your-docs; prompt templates; RAG precursor.

### Data Organization

72. **Quote:** "One thing certain about the future is that we'll continue producing more and more data... Photos, videos, logs, and PDFs are all unstructured or semistructured data. It's essential to organize all this data in a way that can be searched later. AI can help with exactly that. AI can automatically generate text descriptions about images and videos, or help match text queries with visuals that match those queries. Services like Google Photos are already using AI... Google Image Search goes a step further: if there's no existing image matching users' needs, it can generate some."
    - **Plain English:** AI organizes unstructured data — describing images/videos, matching queries, even generating missing images.
    - **Word meanings:** semistructured = partially organized.
    - **Technical terms:** unstructured data; image search; data organization.

73. **Quote:** "AI is very good with data analysis. It can write programs to generate data visualization, identify outliers, and make predictions like revenue forecasts. Enterprises can use AI to extract structured information from unstructured data... Simple use cases include automatically extracting information from credit cards, driver's licenses, receipts, tickets, contact information from email footers... More complex use cases include extracting data from contracts, reports, charts... It's estimated that the IDP, intelligent data processing, industry will reach $12.81 billion by 2030, growing 32.9% each year."
    - **Plain English:** AI does data analysis (visualizations, outliers, forecasts) and extraction (receipts, contracts); IDP market ≈ $12.81B by 2030, +32.9%/yr.
    - **Word meanings:** outliers = unusual data points.
    - **Technical terms:** data analysis; IDP (intelligent data processing); structured extraction.

### Workflow Automation

74. **Quote:** "Ultimately, AI should automate as much as possible. For end users, automation can help with boring daily tasks like booking restaurants, requesting refunds, planning trips, and filling out forms. For enterprises, AI can automate repetitive tasks such as lead management, invoicing, reimbursements, managing customer requests, data entry, and so on. One especially exciting use case is using AI models to synthesize data, which can then be used to improve the models themselves. You can use AI to create labels for your data, looping in humans to improve the labels."
    - **Plain English:** AI automates daily and enterprise tasks, and can even synthesize training data/labels.
    - **Word meanings:** repetitive = repeated.
    - **Technical terms:** workflow automation; data synthesis (Ch 8).

75. **Quote:** "Access to external tools is required to accomplish many tasks. To book a restaurant, an application might need permission to open a search engine to look up the restaurant's number, use your phone to make calls, and add appointments to your calendar. AIs that can plan and use tools are called agents. The level of interest around agents borders on obsession... AI agents have the potential to make every person vastly more productive and generate vastly more economic value. Agents are a central topic in Chapter 6."
    - **Plain English:** Tasks need external tools → AI that plans and uses tools = "agents" (a central Chapter 6 topic).
    - **Word meanings:** borders on = nearly reaches.
    - **Technical terms:** agents; tool use.

## Planning AI Applications

76. **Quote:** "However, if you're doing this for a living, it might be worthwhile to take a step back and consider why you're building this and how you should go about it. It's easy to build a cool demo with foundation models. It's hard to create a profitable product."
    - **Plain English:** Easy demos ≠ profitable products; plan carefully.
    - **Word meanings:** profitable = money-making.
    - **Technical terms:** product-market fit.

### Use Case Evaluation

77. **Quote (risk levels, high to low):** "1. If you don't do this, competitors with AI can make you obsolete. If AI poses a major existential threat to your business, incorporating AI must have the highest priority. In the 2023 Gartner study, 7% cited business continuity as their reason for embracing AI... 2. If you don't do this, you'll miss opportunities to boost profits and productivity... 3. You're unsure where AI will fit into your business yet, but you don't want to be left behind. While a company shouldn't chase every hype train, many have failed by waiting too long to take the leap (cue Kodak, Blockbuster, and BlackBerry)."
    - **Plain English:** Ranked reasons: (1) existential threat, (2) missed opportunities, (3) fear of being left behind (Kodak/Blockbuster/BlackBerry).
    - **Word meanings:** obsolete = out of date; existential = threatening existence.
    - **Technical terms:** use case evaluation; business continuity; "GPTs are GPTs" (Eloundou et al., 2023).

78. **Quote:** "Once you've found a good reason to develop this use case, you might consider whether you have to build it yourself. If AI poses an existential threat to your business, you might want to do AI in-house instead of outsourcing it to a competitor. However, if you're using AI to boost profits and productivity, you might have plenty of buy options that can save you time and money while giving you better performance."
    - **Plain English:** Build vs buy: existential threat → build in-house; profit/productivity boost → buy.
    - **Word meanings:** in-house = within your own org.
    - **Technical terms:** build-or-buy decision.

### The role of AI and humans in the application

79. **Quote (Critical or complementary):** "If an app can still work without AI, AI is complementary to the app. For example, Face ID wouldn't work without AI-powered facial recognition, whereas Gmail would still work without Smart Compose. The more critical AI is to the application, the more accurate and reliable the AI part has to be. People are more accepting of mistakes when AI isn't core to the application."
    - **Plain English:** Critical AI (Face ID) needs high accuracy; complementary AI (Smart Compose) tolerates mistakes.
    - **Word meanings:** complementary = supporting, not essential.
    - **Technical terms:** critical vs complementary AI.

80. **Quote (Reactive or proactive):** "A reactive feature shows its responses in reaction to users' requests or specific actions, whereas a proactive feature shows its responses when there's an opportunity for it. For example, a chatbot is reactive, whereas traffic alerts on Google Maps are proactive... Because users don't ask for proactive features, they can view them as intrusive or annoying if the quality is low. Therefore, proactive predictions and generations typically have a higher quality bar."
    - **Plain English:** Reactive = responds on request (chatbot); proactive = surfaces when opportune (Maps traffic) — proactive needs a higher quality bar.
    - **Word meanings:** intrusive = unwelcome.
    - **Technical terms:** reactive vs proactive features.

81. **Quote (Dynamic or static):** "Dynamic features are updated continually with user feedback, whereas static features are updated periodically. For example, Face ID needs to be updated as people's faces change over time. However, object detection in Google Photos is likely updated only when Google Photos is upgraded. In the case of AI, dynamic features might mean that each user has their own model, continually finetuned on their data, or other mechanisms for personalization such as ChatGPT's memory feature... However, static features might have one model for a group of users."
    - **Plain English:** Dynamic = continually updated with feedback (per-user models, ChatGPT memory); static = updated periodically (shared model).
    - **Word meanings:** periodically = at intervals.
    - **Technical terms:** dynamic vs static features; per-user finetuning; ChatGPT memory.

82. **Quote (human role):** "It's also important to clarify the role of humans in the application. Will AI provide background support to humans, make decisions directly, or both? For example, for a customer support chatbot, AI responses can be used in different ways: • AI shows several responses that human agents can reference to write faster responses. • AI responds only to simple requests and routes more complex requests to humans. • AI responds to all requests directly, without human involvement. Involving humans in AI's decision-making processes is called human-in-the-loop."
    - **Plain English:** Human role options: AI supports humans, AI handles simple + routes hard to humans, or AI handles everything; human involvement = human-in-the-loop.
    - **Word meanings:** routes = directs.
    - **Technical terms:** human-in-the-loop; escalation.

83. **Quote (Crawl-Walk-Run):** "Microsoft (2023) proposed a framework for gradually increasing AI automation in products that they call Crawl-Walk-Run: 1. Crawl means human involvement is mandatory. 2. Walk means AI can directly interact with internal employees. 3. Run means increased automation, potentially including direct AI interactions with external users. The role of humans can change over time as the quality of the AI system improves. For example, in the beginning... you might use it to generate suggestions for human agents. If the acceptance rate by human agents is high, for example, 95% of AI-suggested responses to simple requests are used by human agents verbatim, you can let customers interact with AI directly for those simple requests."
    - **Plain English:** Crawl (humans mandatory) → Walk (AI-internal) → Run (AI-external); escalate when quality is proven (e.g., 95% verbatim acceptance).
    - **Word meanings:** verbatim = word-for-word; mandatory = required.
    - **Technical terms:** Crawl-Walk-Run; AI automation stages.

### AI product defensibility

84. **Quote:** "If you're selling AI applications as standalone products, it's important to consider their defensibility. The low entry barrier is both a blessing and a curse. If something is easy for you to build, it's also easy for your competitors. What moats do you have to defend your product? In a way, building applications on top of foundation models means providing a layer on top of these models. This also means that if the underlying models expand in capabilities, the layer you provide might be subsumed by the models, rendering your application obsolete."
    - **Plain English:** Low barrier means easy competition; your layer can be absorbed when the base model improves.
    - **Word meanings:** moats = protective advantages; subsumed = absorbed.
    - **Technical terms:** product defensibility; model-layer risk (e.g., PDF-parsing app built on ChatGPT's limits).

85. **Quote:** "In AI, there are generally three types of competitive advantages: technology, data, and distribution—the ability to bring your product in front of users. With foundation models, the core technologies of most companies will be similar. The distribution advantage likely belongs to big companies. The data advantage is more nuanced. Big companies likely have more existing data. However, if a startup can get to market first and gather sufficient usage data to continually improve their products, data will be their moat."
    - **Plain English:** Three moats: technology, data, distribution; with foundation models, technology is similar for all, so data and distribution matter.
    - **Word meanings:** nuanced = subtle; moat = barrier to competition.
    - **Technical terms:** competitive advantage; data flywheel (footnote).

86. **Quote:** "There have been many successful companies whose original products could've been features of larger products. Calendly could've been a feature of Google Calendar. Mailchimp could've been a feature of Gmail. Photoroom could've been a feature of Google Photos. Many startups eventually overtake bigger competitors, starting by building a feature that these bigger competitors overlooked. Perhaps yours can be the next one."
    - **Plain English:** Calendly, Mailchimp, Photoroom started as would-be "features" of bigger products yet won; feature-into-product is a viable path.
    - **Word meanings:** overlooked = ignored.
    - **Technical terms:** feature vs product; competitive positioning.

### Setting Expectations

87. **Quote:** "The most important metric is how this will impact your business. For example, if it's a customer support chatbot, the business metrics can include the following: • What percentage of customer messages do you want the chatbot to automate? • How many more messages should the chatbot allow you to process? • How much quicker can you respond using the chatbot? • How much human labor can the chatbot save you?"
    - **Plain English:** Start with business metrics (automation %, capacity, speed, labor saved).
    - **Word meanings:** metrics = measurements.
    - **Technical terms:** business metrics; success measurement.

88. **Quote:** "To ensure a product isn't put in front of customers before it's ready, have clear expectations on its usefulness threshold: how good it has to be for it to be useful. Usefulness thresholds might include the following metrics groups: • Quality metrics to measure the quality of the chatbot's responses. • Latency metrics including TTFT (time to first token), TPOT (time per output token), and total latency. What is considered acceptable latency depends on your use case. If all of your customer requests are currently being processed by humans with a median response time of an hour, anything faster than this might be good enough. • Cost metrics: how much it costs per inference request. • Other metrics such as interpretability and fairness."
    - **Plain English:** Define a usefulness threshold: quality, latency (TTFT/TPOT/total), cost per inference, interpretability/fairness; latency bar depends on baseline (humans = ~1 hour).
    - **Word meanings:** threshold = minimum bar.
    - **Technical terms:** TTFT; TPOT; total latency; cost per inference; usefulness threshold.

### Milestone Planning

89. **Quote:** "Once you've set measurable goals, you need a plan to achieve these goals. How to get to the goals depends on where you start. Evaluate existing models to understand their capabilities. The stronger the off-the-shelf models, the less work you'll have to do. For example, if your goal is to automate 60% of customer support tickets and the off-the-shelf model you want to use can already automate 30% of the tickets, the effort you need to put in might be less than if it can automate no tickets at all. It's likely that your goals will change after evaluation."
    - **Plain English:** Start by evaluating existing models; a strong off-the-shelf model means less work; goals often change after evaluation.
    - **Word meanings:** off-the-shelf = ready-made.
    - **Technical terms:** model evaluation; milestone planning.

90. **Quote (last mile):** "Planning an AI product needs to account for its last mile challenge. Initial success with foundation models can be misleading... It might take a weekend to build a demo but months, and even years, to build a product. In the paper UltraChat, Ding et al. (2023) shared that 'the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging.' LinkedIn (2024) shared the same sentiment. It took them one month to achieve 80% of the experience they wanted... they found it took them four more months to finally surpass 95%. A lot of time was spent working on the product kinks and dealing with hallucinations. The slow speed of achieving each subsequent 1% gain was discouraging."
    - **Plain English:** The last mile is hard: demo in a weekend, product in months/years; UltraChat "0→60 easy, 60→100 exceedingly challenging"; LinkedIn: 1 month to 80%, +4 months to 95% (hallucinations, kinks).
    - **Word meanings:** kinks = small defects; hallucinations = false outputs.
    - **Technical terms:** last-mile problem; hallucinations.

### Maintenance

91. **Quote:** "Maintenance of an AI product has the added challenge of AI's fast pace of change... Building on top of foundation models today means committing to riding this bullet train. Many changes are good. For example, the limitations of many models are being addressed. Context lengths are getting longer. Model outputs are getting better. Model inference, the process of computing an output given an input, is getting faster and cheaper. Figure 1-11 shows the evolution of inference cost and model performance on Massive Multitask Language Understanding (MMLU) (Hendrycks et al., 2020), a popular foundation model benchmark, between 2022 and 2024."
    - **Plain English:** AI changes fast ("bullet train"); good changes: longer context, better outputs, faster/cheaper inference (Figure 1-11, MMLU benchmark).
    - **Word meanings:** inference = computing an output for an input.
    - **Technical terms:** maintenance; MMLU; inference cost.

92. **Quote:** "However, even these good changes can cause friction in your workflows. You'll have to constantly be on your guard and run a cost-benefit analysis of each technology investment. The best option today might turn into the worst option tomorrow. You may decide to build a model in-house because it seems cheaper than paying for model providers, only to find out after three months that model providers have dropped their prices in half, making in-house the expensive option. You might invest in a third-party solution... only for the provider to go out of business after failing to secure funding."
    - **Plain English:** Fast change causes friction: prices halve, providers die, in-house decisions go stale.
    - **Word meanings:** friction = resistance/hassle.
    - **Technical terms:** technology risk; cost-benefit analysis.

93. **Quote:** "Some changes are easier to adapt to. For example, as model providers converge to the same API, it's becoming easier to swap one model API for another. However, as each model has its quirks, strengths, and weaknesses, developers working with the new model will need to adjust their workflows, prompts, and data to this new model. Without proper infrastructure for versioning and evaluation in place, the process can cause a lot of headaches."
    - **Plain English:** Converging APIs ease model swaps, but each model needs workflow/prompt/data adjustment; versioning + evaluation infrastructure is essential.
    - **Word meanings:** quirks = peculiarities; versioning = tracking versions.
    - **Technical terms:** API convergence; model swapping; versioning.

94. **Quote:** "Some changes are harder to adapt to, especially those around regulations. Technologies surrounding AI are considered national security issues for many countries... The introduction of Europe's General Data Protection Regulation (GDPR), for example, was estimated to cost businesses $9 billion to become compliant. Compute availability can change overnight as new laws put more restrictions on who can buy and sell compute resources (see the US October 2023 Executive Order). If your GPU vendor is suddenly banned from selling GPUs to your country, you're in trouble. Some changes can even be fatal. For example, regulations around intellectual property (IP) and AI usage are still evolving... Many IP-heavy companies I've talked to, such as game studios, hesitate to use AI for fear of losing their IPs later on."
    - **Plain English:** Regulatory risk: GDPR compliance ~$9B, compute restrictions (US Oct 2023 EO), evolving IP law makes game studios cautious.
    - **Word meanings:** compliance = following rules; fatal = deadly to the product.
    - **Technical terms:** GDPR; compute regulation; intellectual property (IP) risk.

## The AI Engineering Stack

95. **Quote:** "AI engineering's rapid growth also induced an incredible amount of hype and FOMO (fear of missing out)... Instead of trying to keep up with the constantly shifting sand, let's look into the fundamental building blocks of AI engineering. To understand AI engineering, it's important to recognize that AI engineering evolved out of ML engineering. When a company starts experimenting with foundation models, it's natural that its existing ML team should lead the effort. Some companies treat AI engineering the same as ML engineering... Some companies have separate job descriptions for AI engineering... Regardless of where organizations position AI engineers and ML engineers, their roles have significant overlap. Existing ML engineers can add AI engineering to their lists of skills... However, there are also AI engineers with no previous ML experience."
    - **Plain English:** Ignore the hype; AI engineering grew out of ML engineering; roles overlap heavily, and AI engineers needn't have ML backgrounds.
    - **Word meanings:** FOMO = fear of missing out.
    - **Technical terms:** AI engineering vs ML engineering roles (Figures 1-12, 1-13).

### Three Layers of the AI Stack

96. **Quote:** "There are three layers to any AI application stack: application development, model development, and infrastructure. When developing an AI application, you'll likely start from the top layer and move down as needed: Application development: ... involves providing a model with good prompts and necessary context. This layer requires rigorous evaluation. Good applications also demand good interfaces. Model development: This layer provides tooling for developing models, including frameworks for modeling, training, finetuning, and inference optimization. Because data is central to model development, this layer also contains dataset engineering. Model development also requires rigorous evaluation. Infrastructure: At the bottom is the stack is infrastructure, which includes tooling for model serving, managing data and compute, and monitoring."
    - **Plain English:** The stack: application development (top) → model development → infrastructure (bottom); start at top, move down as needed.
    - **Word meanings:** serving = running models for users; rigorous = strict/thorough.
    - **Technical terms:** three-layer AI stack (Figure 1-14); model serving; monitoring.

97. **Quote:** "To get a sense of how the landscape has evolved with foundation models, in March 2024, I searched GitHub for all AI-related repositories with at least 500 stars... I found a total of 920 repositories. Figure 1-15 shows the cumulative number of repositories in each category month-over-month. The data shows a big jump in the number of AI toolings in 2023, after the introduction of Stable Diffusion and ChatGPT. In 2023, the categories that saw the highest increases were applications and application development. The infrastructure layer saw some growth, but it was much less... Even though models and applications have changed, the core infrastructural needs—resource management, serving, monitoring, etc.—remain the same."
    - **Plain English:** 920 AI repos (≥500 stars) as of Mar 2024; biggest 2023 growth in applications/application development; infrastructure grew least because core needs are unchanged.
    - **Technical terms:** GitHub ecosystem analysis (Figures 1-15).

98. **Quote:** "While the level of excitement and creativity around foundation models is unprecedented, many principles of building AI applications remain the same... it's still essential to map from business metrics to ML metrics and vice versa. You still need to do systematic experimentation. With classical ML engineering, you experiment with different hyperparameters. With foundation models, you experiment with different models, prompts, retrieval algorithms, sampling variables, and more. (Sampling variables are discussed in Chapter 2.) We still want to make models run faster and cheaper. It's still important to set up a feedback loop so that we can iteratively improve our applications with production data."
    - **Plain English:** Enduring ML principles: business↔ML metrics mapping, systematic experimentation (now over models/prompts/retrieval/sampling), speed/cost, feedback loops.
    - **Word meanings:** unprecedented = never seen before; iteratively = in repeated cycles.
    - **Technical terms:** business→ML metrics; experimentation; feedback loop; sampling variables (Ch 2).

### AI Engineering Versus ML Engineering

99. **Quote:** "At a high level, building applications using foundation models today differs from traditional ML engineering in three major ways: 1. Without foundation models, you have to train your own models for your applications. With AI engineering, you use a model someone else has trained for you. This means that AI engineering focuses less on modeling and training, and more on model adaptation. 2. AI engineering works with models that are bigger, consume more compute resources, and incur higher latency than traditional ML engineering. This means that there's more pressure for efficient training and inference optimization... there's more need for engineers who know how to work with GPUs and big clusters. 3. AI engineering works with models that can produce open-ended outputs. Open-ended outputs give models the flexibility to be used for more tasks, but they are also harder to evaluate. This makes evaluation a much bigger problem in AI engineering."
    - **Plain English:** The 3 differences: (1) adapt pretrained models rather than train; (2) bigger/compute-heavy models → inference optimization + GPU clusters; (3) open-ended outputs → much harder evaluation.
    - **Word meanings:** corollary = natural consequence; incur = cause.
    - **Technical terms:** model adaptation; inference optimization; GPU clusters; open-ended evaluation. (Footnote: Fortune 500 head: teams know 10 GPUs, not 1,000.)

100. **Quote:** "In short, AI engineering differs from ML engineering in that it's less about model development and more about adapting and evaluating models."
     - **Plain English:** One-line summary of the discipline's shift.
     - **Technical terms:** model adaptation; evaluation.

101. **Quote (prompt-based vs finetuning):** "In general, model adaptation techniques can be divided into two categories, depending on whether they require updating model weights. Prompt-based techniques, which include prompt engineering, adapt a model without updating the model weights. You adapt a model by giving it instructions and context instead of changing the model itself. Prompt engineering is easier to get started and requires less data... However, prompt engineering might not be enough for complex tasks or applications with strict performance requirements. Finetuning, on the other hand, requires updating model weights... finetuning techniques are more complicated and require more data, but they can improve your model's quality, latency, and cost significantly. Many things aren't possible without changing model weights, such as adapting the model to a new task it wasn't exposed to during training."
     - **Plain English:** Prompt-based adaptation = no weight changes (easy, less data, may be insufficient); finetuning = weight changes (harder, more data, better quality/latency/cost, enables new tasks).
     - **Word meanings:** strict = demanding.
     - **Technical terms:** prompt-based adaptation; finetuning; model weights.

### Model development

102. **Quote:** "Model development is the layer most commonly associated with traditional ML engineering. It has three main responsibilities: modeling and training, dataset engineering, and inference optimization."
     - **Plain English:** Model development = modeling & training + dataset engineering + inference optimization.
     - **Technical terms:** modeling and training; dataset engineering; inference optimization.

103. **Quote (Modeling and training):** "Modeling and training refers to the process of coming up with a model architecture, training it, and finetuning it. Examples of tools in this category are Google's TensorFlow, Hugging Face's Transformers, and Meta's PyTorch. Developing ML models requires specialized ML knowledge. It requires knowing different types of ML algorithms (such as clustering, logistic regression, decision trees, and collaborative filtering) and neural network architectures (such as feedforward, recurrent, convolutional, and transformer)... With the availability of foundation models, ML knowledge is no longer a must-have for building AI applications... However, ML knowledge is still extremely valuable, as it expands the set of tools that you can use and helps troubleshooting when a model doesn't work as expected."
     - **Plain English:** Modeling/training needs ML knowledge (algorithms, architectures, learning concepts); with foundation models it's nice-to-have but still valuable.
     - **Word meanings:** must-have = required.
     - **Technical terms:** TensorFlow; PyTorch; Hugging Face Transformers; gradient descent; loss function; regularization.

104. **Quote (training vs pre/fine/post-training):** "Training always involves changing model weights, but not all changes to model weights constitute training. For example, quantization, the process of reducing the precision of model weights, technically changes the model's weight values but isn't considered training."
     - **Plain English:** Weight changes ≠ always training; quantization changes weight values but isn't training.
     - **Word meanings:** constitute = count as.
     - **Technical terms:** quantization; training.

105. **Quote (Pre-training):** "Pre-training refers to training a model from scratch—the model weights are randomly initialized. For LLMs, pre-training often involves training a model for text completion. Out of all training steps, pre-training is often the most resource-intensive by a long shot. For the InstructGPT model, pre-training takes up to 98% of the overall compute and data resources... A small mistake during pre-training can incur a significant financial loss and set back the project significantly. Due to the resource-intensive nature of pre-training, this has become an art that only a few practice."
     - **Plain English:** Pre-training = training from scratch (random weights), text completion, ~98% of resources (InstructGPT); an art practiced by few.
     - **Word meanings:** by a long shot = by far.
     - **Technical terms:** pre-training; randomly initialized weights; InstructGPT.

106. **Quote (Finetuning):** "Finetuning means continuing to train a previously trained model—the model weights are obtained from the previous training process. Because the model already has certain knowledge from pre-training, finetuning typically requires fewer resources (e.g., data and compute) than pre-training."
     - **Plain English:** Finetuning = continuing training from pretrained weights; needs fewer resources.
     - **Technical terms:** finetuning; transfer from pre-training.

107. **Quote (Post-training):** "Many people use post-training to refer to the process of training a model after the pre-training phase. Conceptually, post-training and finetuning are the same and can be used interchangeably. However, sometimes, people might use them differently to signify the different goals. It's usually post-training when it's done by model developers. For example, OpenAI might post-train a model to make it better at following instructions before releasing it. It's finetuning when it's done by application developers. For example, you might finetune an OpenAI model (which might have been post-trained itself) to adapt it to your needs. Pre-training and post-training make up a spectrum."
     - **Plain English:** Post-training = finetuning after pre-training; "post-training" by model developers (instruction-following), "finetuning" by app developers; they form a spectrum.
     - **Word meanings:** interchangeably = same meaning; spectrum = continuous range.
     - **Technical terms:** post-training; finetuning; instruction-following.

108. **Quote (misuse of "training"):** "Some people use the term training to refer to prompt engineering, which isn't correct. I read a Business Insider article where the author said she trained ChatGPT to mimic her younger self. She did so by feeding her childhood journal entries into ChatGPT... if you teach a model what to do via the context input into the model, you're doing prompt engineering. Similarly, I've seen people using the term finetuning when what they do is prompt engineering."
     - **Plain English:** Feeding context to a model = prompt engineering, not training/finetuning.
     - **Word meanings:** mimic = imitate; colloquially = informally.
     - **Technical terms:** prompt engineering vs training (terminology trap).

109. **Quote (Dataset engineering):** "Dataset engineering refers to curating, generating, and annotating the data needed for training and adapting AI models. In traditional ML engineering, most use cases are close-ended—a model's output can only be among predefined values. For example, spam classification with only two possible outputs, 'spam' and 'not spam', is close-ended. Foundation models, however, are open-ended. Annotating open-ended queries is much harder than annotating close-ended queries—it's easier to determine whether an email is spam than to write an essay. So data annotation is a much bigger challenge for AI engineering. Another difference is that traditional ML engineering works more with tabular data, whereas foundation models work with unstructured data. In AI engineering, data manipulation is more about deduplication, tokenization, context retrieval, and quality control, including removing sensitive information and toxic data."
     - **Plain English:** Dataset engineering curates/generates/annotates data; open-ended outputs make annotation harder; focus shifts from feature engineering to dedup/tokenization/context retrieval/quality control.
     - **Word meanings:** tabular = table-form; deduplication = removing duplicates.
     - **Technical terms:** dataset engineering; close-ended vs open-ended; feature engineering; quality control (Ch 8).

110. **Quote:** "Many people argue that because models are now commodities, data will be the main differentiator, making dataset engineering more important than ever. How much data you need depends on the adapter technique you use. Training a model from scratch generally requires more data than finetuning, which, in turn, requires more data than prompt engineering. Regardless of how much data you need, expertise in data is useful when examining a model, as its training data gives important clues about that model's strengths and weaknesses."
     - **Plain English:** Models are commodities → data is the differentiator; data needs: training from scratch > finetuning > prompt engineering; training data reveals model strengths/weaknesses.
     - **Word meanings:** commodities = interchangeable goods; differentiator = distinguishing factor.
     - **Technical terms:** data moat; data needs by technique (Table 1-4).

111. **Quote (Inference optimization):** "Inference optimization means making models faster and cheaper... One challenge with foundation models is that they are often autoregressive—tokens are generated sequentially. If it takes 10 ms for a model to generate a token, it'll take a second to generate an output of 100 tokens, and even more for longer outputs. As users are getting notoriously impatient, getting AI applications' latency down to the 100 ms latency expected for a typical internet application is a huge challenge. Inference optimization has become an active subfield in both industry and academia."
     - **Plain English:** Inference optimization = faster/cheaper models; autoregressive generation is sequential (10ms/token → 1s for 100 tokens); hitting the 100ms internet-app latency bar is hard.
     - **Word meanings:** notoriously = famously; academia = universities/research.
     - **Technical terms:** inference optimization; autoregressive latency; 100ms target. (Techniques: quantization, distillation, parallelism, Ch 7–9.)

### Application development

112. **Quote:** "With traditional ML engineering, where teams build applications using their proprietary models, the model quality is a differentiation. With foundation models, where many teams use the same model, differentiation must be gained through the application development process. The application development layer consists of these responsibilities: evaluation, prompt engineering, and AI interface."
     - **Plain English:** With shared foundation models, differentiation comes from application development: evaluation + prompt engineering + AI interface.
     - **Word meanings:** proprietary = owned/private.
     - **Technical terms:** application development; AI interface.

113. **Quote (Evaluation):** "Evaluation is about mitigating risks and uncovering opportunities. Evaluation is necessary throughout the whole model adaptation process. Evaluation is needed to select models, to benchmark progress, to determine whether an application is ready for deployment, and to detect issues and opportunities for improvement in production."
     - **Plain English:** Evaluation serves model selection, progress benchmarking, deployment readiness, and production issue detection.
     - **Word meanings:** mitigating = reducing.
     - **Technical terms:** evaluation; deployment readiness.

114. **Quote (open-ended evaluation):** "The challenges of evaluating foundation models... chiefly arise from foundation models' open-ended nature and expanded capabilities. For example, in close-ended ML tasks like fraud detection, there are usually expected ground truths that you can compare your model's outputs against. If a model's output differs from the expected output, you know the model is wrong. For a task like chatbots, however, there are so many possible responses to each prompt that it is impossible to curate an exhaustive list of ground truths to compare a model's response to."
     - **Plain English:** Close-ended tasks have ground truths; chatbots have too many valid responses to curate exhaustive ground truths — evaluation is harder.
     - **Word meanings:** curate = assemble; exhaustive = complete.
     - **Technical terms:** ground truth; open-ended evaluation challenge.

115. **Quote (Gemini MMLU story):** "When Google launched Gemini in December 2023, they claimed that Gemini is better than ChatGPT in the MMLU benchmark (Hendrycks et al., 2020). Google had evaluated Gemini using a prompt engineering technique called CoT@32. In this technique, Gemini was shown 32 examples, while ChatGPT was shown only 5 examples. When both were shown five examples, ChatGPT performed better, as shown in Table 1-5."
     - **Plain English:** Google's Gemini-vs-ChatGPT MMLU claim used CoT@32 (32 examples) vs ChatGPT's 5; with equal settings (5), ChatGPT won.
     - **Word meanings:** claimed = asserted.
     - **Technical terms:** MMLU; CoT@N (chain-of-thought with N examples); evaluation fairness.

116. **Table 1-5 annotation (MMLU performance by prompt technique):**
    | Model | MMLU | Technique |
    |---|---|---|
    | Gemini Ultra | 90.04% | CoT@32 |
    | Gemini Pro | 79.13% | CoT@8 |
    | GPT-4 | 87.29% | CoT@32 (via API) |
    | GPT-3.5 | 70% | 5-shot |
    | PaLM 2-L | 78.4% | 5-shot |
    | Claude 2 | 78.5% | 5-shot |
    | Inflection-2 | 79.6% | CoT |
    | Grok 1 | 73.0% | 5-shot |
    | Llama-2 | 68.0% | 5-shot |
    | Gemini Ultra (reported by Google) | 83.7% / 71.8% | CoT / 5-shot |
    | Gemini Pro (reported by Google) | 86.4% | 5-shot |
    - **Plain English:** Same benchmark, wildly different scores depending on prompt technique — scores are only comparable under identical settings.
    - **Word meanings:** CoT = chain-of-thought; N-shot = N examples given.
    - **Technical terms:** MMLU; CoT; few-shot evaluation.

117. **Quote (Prompt engineering & context construction):** "Prompt engineering is about getting AI models to express the desirable behaviors from the input alone, without changing the model weights. The Gemini evaluation story highlights the impact of prompt engineering on model performance. By using a different prompt engineering technique, Gemini Ultra's performance on MMLU went from 83.7% to 90.04%. It's possible to get a model to do amazing things with just prompts... Prompt engineering is not just about telling a model what to do. It's also about giving the model the necessary context and tools to do a given task. For complex tasks with long context, you might also need to provide the model with a memory management system so that the model can keep track of its history."
     - **Plain English:** Prompt engineering changes only the input; it moved Gemini Ultra's MMLU from 83.7% → 90.04%; it also means giving context, tools, and memory management.
     - **Word meanings:** desirable = wanted.
     - **Technical terms:** prompt engineering; context construction; memory management (Ch 5 & 6).

118. **Quote (AI interface):** "AI interface means creating an interface for end users to interact with your AI applications. Before foundation models, only organizations with sufficient resources to develop AI models could develop AI applications. These applications were often embedded into the organizations' existing products. For example, fraud detection was embedded into Stripe, Venmo, and PayPal. Recommender systems were part of social networks and media apps like Netflix, TikTok, and Spotify. With foundation models, anyone can build AI applications. You can serve your AI applications as standalone products or embed them into other products... For example, ChatGPT and Perplexity are standalone products, whereas GitHub's Copilot is commonly used as a plug-in in VSCode, and Grammarly is commonly used as a browser extension for Google Docs. Midjourney can either be used via its standalone web app or via its integration in Discord."
     - **Plain English:** Interfaces: standalone products (ChatGPT, Perplexity) or embedded (Copilot plug-in, Grammarly extension, Midjourney in Discord); pre-foundation apps were embedded in big products.
     - **Word meanings:** standalone = on its own.
     - **Technical terms:** AI interface; standalone vs embedded; plug-ins/extensions.

119. **Quote (interface types):** "Here are just some of the interfaces that are gaining popularity for AI applications: • Standalone web, desktop, and mobile apps. • Browser extensions that let users quickly query AI models while browsing. • Chatbots integrated into chat apps like Slack, Discord, WeChat, and WhatsApp. • Many products, including VSCode, Shopify, and Microsoft 365, provide APIs that let developers integrate AI into their products as plug-ins and add-ons. These APIs can also be used by AI agents to interact with the world... While the chat interface is the most commonly used, AI interfaces can also be voice-based (such as with voice assistants) or embodied (such as in augmented and virtual reality)."
     - **Plain English:** Interface categories: standalone apps, browser extensions, chat-app bots, plugin APIs (also used by agents), voice, and embodied (AR/VR).
     - **Word meanings:** embodied = having a physical/virtual body.
     - **Technical terms:** AI interfaces; chat/voice/embodied interfaces; agent tool use. (Tools: Streamlit, Gradio, Plotly Dash for web apps — footnote.)

120. **Quote:** "These new AI interfaces also mean new ways to collect and extract user feedback. The conversation interface makes it so much easier for users to give feedback in natural language, but this feedback is harder to extract. User feedback design is discussed in Chapter 10."
     - **Plain English:** Conversational interfaces make feedback easy to give but hard to extract.
     - **Technical terms:** user feedback; feedback extraction (Ch 10).

### AI Engineering Versus Full-Stack Engineering

121. **Quote:** "The increased emphasis on application development, especially on interfaces, brings AI engineering closer to full-stack development. The rising importance of interfaces leads to a shift in the design of AI toolings to attract more frontend engineers. Traditionally, ML engineering is Python-centric. Before foundation models, the most popular ML frameworks supported mostly Python APIs. Today, Python is still popular, but there is also increasing support for JavaScript APIs, with LangChain.js, Transformers.js, OpenAI's Node library, and Vercel's AI SDK."
     - **Plain English:** AI engineering is closer to full-stack; tooling now supports JavaScript (LangChain.js, Transformers.js, Node, Vercel AI SDK), not just Python.
     - **Word meanings:** emphasis = focus; frontend = user-facing.
     - **Technical terms:** full-stack engineering; JS APIs for AI.

122. **Quote:** "While many AI engineers come from traditional ML backgrounds, more are increasingly coming from web development or full-stack backgrounds. An advantage that full-stack engineers have over traditional ML engineers is their ability to quickly turn ideas into demos, get feedback, and iterate. With traditional ML engineering, you usually start with gathering data and training a model. Building the product comes last. However, with AI models readily available today, it's possible to start with building the product first, and only invest in data and models once the product shows promise, as visualized in Figure 1-16. In traditional ML engineering, model development and product development are often disjointed processes... however, with foundation models, AI engineers tend to be much more involved in building the product."
     - **Plain English:** Full-stack engineers iterate fast (demo → feedback → iterate); AI engineering flips the order: build the product first, invest in data/models later.
     - **Word meanings:** iterate = improve in cycles; disjointed = separate.
     - **Technical terms:** build-product-first workflow (Figure 1-16, after Shawn Wang's "The Rise of the AI Engineer", 2023).

## Summary

123. **Quote:** "I meant this chapter to serve two purposes. One is to explain the emergence of AI engineering as a discipline, thanks to the availability of foundation models. Two is to give an overview of the process needed to build applications on top of these models."
     - **Plain English:** Chapter purpose: (1) why AI engineering emerged; (2) the process overview.
     - **Technical terms:** AI engineering; foundation models.

124. **Quote:** "The chapter discussed the rapid evolution of AI in recent years. It walked through some of the most notable transformations, starting with the transition from language models to large language models, thanks to a training approach called self-supervision. It then traced how language models incorporated other data modalities to become foundation models, and how foundation models gave rise to AI engineering."
     - **Plain English:** The chapter traces: language models → LLMs (via self-supervision) → foundation models (modalities) → AI engineering.
     - **Technical terms:** self-supervision; foundation models; AI engineering.

125. **Quote:** "The more overwhelming a space is, the more important it is to have a framework to help us navigate it. This book aims to provide such a framework. The rest of the book will explore this framework step-by-step, starting with the fundamental building block of AI engineering: the foundation models that make so many amazing applications possible."
     - **Plain English:** The book provides a framework; Chapter 2 begins with foundation models.
     - **Word meanings:** navigate = find your way through.
     - **Technical terms:** AI engineering framework; foundation models (Ch 2 preview).
