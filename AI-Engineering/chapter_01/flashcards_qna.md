# Chapter 1 Flashcards: Introduction to Building AI Applications with Foundation Models
**Source:** *AI Engineering* (Chip Huyen), Chapter 1

---

## Part 1Terms → Definitions

**Q:** What is AI engineering?
**A:** The process of building applications on top of readily available foundation models; one of the fastest-growing engineering disciplines.

**Q:** What is a language model?
**A:** A model that encodes statistical information about one or more languages — how likely a word is to appear in a given context (e.g., "My favorite color is __" → "blue" more often than "car").

**Q:** What is a token?
**A:** The basic unit of a language model — a character, a word, or a part of a word (like "-tion"), depending on the model.

**Q:** What is tokenization?
**A:** The process of breaking original text into tokens. For GPT-4, an average token is approximately ¾ the length of a word, so 100 tokens ≈ 75 words.

**Q:** What is a model's vocabulary?
**A:** The set of all tokens a model can work with. Mixtral 8x7B has 32,000 tokens; GPT-4 has 100,256.

**Q:** Why do language models use tokens instead of words or characters? (3 reasons)
**A:** (1) Tokens break words into meaningful components ("cooking" → "cook"+"ing"); (2) fewer unique tokens than words → smaller vocabulary → more efficient; (3) tokens help process unknown words ("chatgpting" → "chatgpt"+"ing").

**Q:** What is a masked language model?
**A:** Trained to predict missing tokens anywhere in a sequence using context from both before and after (fill-in-the-blank); e.g., BERT (Devlin et al., 2018). Used for non-generative tasks like sentiment analysis and text classification, plus code debugging.

**Q:** What is an autoregressive language model?
**A:** Trained to predict the next token in a sequence using only preceding tokens; it can generate one token after another. The model of choice for text generation (also called a causal language model).

**Q:** In this book, what does "language model" mean by default?
**A:** An autoregressive model, unless explicitly stated otherwise.

**Q:** What is generative AI?
**A:** AI that can generate open-ended outputs; a language model uses its finite vocabulary to construct infinite possible outputs — it is a completion machine.

**Q:** What is a completion?
**A:** The model's prediction of the most probable next tokens, based on probabilities and not guaranteed correct; e.g., "To be or not to be" → ", that is the question."

**Q:** Is completion the same as conversation?
**A:** No — completion continues text; making a model respond appropriately comes from post-training.

**Q:** What is supervision?
**A:** Training ML algorithms using labeled data, which can be expensive and slow to obtain (e.g., AlexNet trained on 1M labeled ImageNet images).

**Q:** What is self-supervision?
**A:** Training where the model infers labels from the input data itself; language modeling is self-supervised because each sequence provides both labels (tokens to predict) and contexts. It overcomes the data labeling bottleneck.

**Q:** How does self-supervision differ from unsupervised learning?
**A:** In self-supervision, labels are inferred from the input data; in unsupervised learning, you don't need labels at all.

**Q:** What are `<BOS>` and `<EOS>`?
**A:** Special tokens marking the beginning and end of a sequence; `<EOS>` is especially important because it helps models know when to end their responses.

**Q:** What is a parameter?
**A:** A variable within an ML model updated through the training process; model size is measured by parameter count. (Today "weights" generally refers to all parameters.)

**Q:** What was the size of GPT-1 and GPT-2, and what is "large" today?
**A:** GPT-1 (June 2018): 117M parameters (then "large"); GPT-2 (Feb 2019): 1.5B; as of writing, ~100B parameters is considered large.

**Q:** Why do larger models need more data?
**A:** Larger models have more capacity to learn, so they need more training data to maximize performance; training a large model on a small dataset wastes compute.

**Q:** What is a modality?
**A:** A type of data (text, images, audio, video, etc.); models handling more than one modality are multimodal.

**Q:** What is an LMM?
**A:** A large multimodal model — a generative multimodal model (e.g., GPT-4V, Claude 3); it generates the next token conditioned on both text and image tokens.

**Q:** What is a foundation model?
**A:** In this book, the umbrella term for both large language models and large multimodal models; "foundation" signals both their importance and that they can be built upon for different needs.

**Q:** What is natural language supervision?
**A:** A self-supervision variant where (image, text) pairs that co-occur on the internet serve as training data (used to train CLIP on 400M pairs — 400× ImageNet — without manual labeling).

**Q:** Is CLIP a generative model?
**A:** No — CLIP is an embedding model trained to produce joint embeddings of texts and images; embedding models like CLIP are the backbones of generative multimodal models (Flamingo, LLaVA, Gemini).

**Q:** What are the three common AI engineering adaptation techniques?
**A:** Prompt engineering (instructions/examples in the input), RAG (connecting the model to a database/external data), and finetuning (further training on task data).

**Q:** What is prompt engineering?
**A:** Getting a model to express desirable behaviors from the input alone, without changing model weights.

**Q:** What is finetuning?
**A:** Continuing to train (further training) a previously trained model on a task-specific dataset; requires updating model weights.

**Q:** Why is adapting a foundation model easier than building one from scratch?
**A:** Roughly "ten examples and one weekend" vs "1 million examples and six months"; adaptation reduces cost and time to market, though task-specific models can be smaller/faster/cheaper.

**Q:** What are the three factors driving AI engineering's rapid growth?
**A:** (1) General-purpose AI capabilities; (2) increased AI investments; (3) low entrance barrier (model as a service, AI writes code, plain-English workflows).

**Q:** Why "AI engineering" rather than "ML engineering" or "LLMOps"?
**A:** Not "ML engineering" because foundation-model work differs from traditional ML in important ways (though ML engineering encompasses both); not "-Ops" terms because the focus is on tweaking/engineering models, not just operations; a survey of 20 practitioners preferred "AI engineering."

**Q:** What is a task "exposed" to AI (Eloundou et al., 2023)?
**A:** A task is exposed if AI or AI-powered software can reduce the time needed to complete it by at least 50%; an occupation with 80% exposure means 80% of its tasks are exposed.

**Q:** Which occupations have highest/lowest AI exposure (Eloundou et al., 2023)?
**A:** Highest: interpreters/translators, tax preparers, web designers, writers (humans labeled 15 occupations "fully exposed"). None: cooks, stonemasons, athletes.

**Q:** What is an agent?
**A:** An AI application that can plan and use external tools (e.g., to book a restaurant: search, call, add to calendar); a central topic of Chapter 6.

**Q:** What are the three types of competitive advantages in AI?
**A:** Technology, data, and distribution (getting your product in front of users). With foundation models, technology is similar across companies, so data and distribution differentiate.

**Q:** What is a defensibility "moat"?
**A:** A protective advantage against competitors; e.g., a data flywheel — first-mover gathers usage data to continually improve, making data the moat.

**Q:** What is a usefulness threshold?
**A:** How good the product has to be for it to be useful — defined via quality, latency (TTFT/TPOT/total), cost per inference, and other metrics (interpretability, fairness).

**Q:** What is TTFT?
**A:** Time to first token — a latency metric measuring how long until the model emits its first output token.

**Q:** What is TPOT?
**A:** Time per output token — a latency metric measuring the interval between output tokens.

**Q:** What is MMLU?
**A:** Massive Multitask Language Understanding (Hendrycks et al., 2020), a popular foundation-model benchmark; central to the Gemini evaluation controversy.

**Q:** What is CoT@N in evaluation?
**A:** Chain-of-thought prompting with N examples; Google evaluated Gemini with CoT@32 (32 examples) while ChatGPT was shown only 5, making the comparison unequal.

**Q:** What is pre-training?
**A:** Training a model from scratch with randomly initialized weights, often for text completion; the most resource-intensive phase (up to 98% of compute/data for InstructGPT); an art practiced by few.

**Q:** What is post-training?
**A:** Training a model after the pre-training phase; conceptually the same as finetuning (interchangeable), but "post-training" is usually done by model developers (e.g., OpenAI teaching instruction-following), "finetuning" by application developers.

**Q:** What is quantization?
**A:** The process of reducing the precision of model weights; it changes weight values but isn't considered training.

**Q:** What is dataset engineering?
**A:** Curating, generating, and annotating the data needed for training and adapting AI models; for foundation models this is more about deduplication, tokenization, context retrieval, and quality control than feature engineering.

**Q:** What is inference optimization?
**A:** Making models faster and cheaper to run; with autoregressive models generating tokens sequentially (10ms/token → 1s per 100 tokens), hitting the ~100ms internet-app latency bar is a huge challenge.

**Q:** What is the AI engineering stack?
**A:** Three layers: application development (top), model development, and infrastructure (bottom); start at the top and move down as needed.

**Q:** What are the three responsibilities of model development?
**A:** Modeling and training, dataset engineering, and inference optimization.

**Q:** What are the three responsibilities of application development?
**A:** Evaluation, prompt engineering, and AI interface.

**Q:** What is an AI interface?
**A:** The interface for end users to interact with an AI application: standalone web/desktop/mobile apps, browser extensions, chatbots in chat apps (Slack, Discord, WeChat, WhatsApp), plugin APIs, voice, or embodied (AR/VR).

**Q:** What is a ground truth?
**A:** An expected/correct output used to evaluate a model; close-ended tasks (fraud detection) have ground truths, but chatbots have too many valid responses to curate exhaustive ground-truth lists.

**Q:** What is the "last mile" challenge?
**A:** Initial demos are easy (a weekend), but a polished product takes months or years; UltraChat: "the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging." LinkedIn: 1 month to 80%, 4 more months to surpass 95%.

**Q:** What is "critical vs complementary" AI?
**A:** Critical: app fails without AI (Face ID needs facial recognition). Complementary: app works without it (Gmail works without Smart Compose). Critical AI must be more accurate and reliable.

**Q:** What is the build-or-buy rule of thumb?
**A:** If AI poses an existential threat, build in-house; if you're boosting profits/productivity, buy options can save time and money with better performance.


## Part 2Short Answer

**Q:** Trace the evolution from language models to AI engineering.
**A:** Language models (statistical next-token predictors, since the 1950s) → LLMs (enabled by self-supervision on web-scale text) → foundation models (LLMs + LMMs handling multiple modalities, general-purpose) → AI engineering (building applications on top of these ready-made models).

**Q:** Why is language modeling self-supervised?
**A:** Each input sequence provides both the labels (the tokens to be predicted) and the contexts used to predict them, so no human annotation is needed — one sentence yields many (context → next token) training samples.

**Q:** Explain Table 1-1 with "I love street food."
**A:** With `<BOS>` and `<EOS>` markers, the sentence yields 6 samples: `<BOS>`→I, `<BOS>,I`→love, `<BOS>,I,love`→street, `<BOS>,I,love,street`→food, `<BOS>,I,love,street,food`→`.`, `<BOS>,I,love,street,food,.`→`<EOS>`.

**Q:** What's the difference between training, pre-training, finetuning, and post-training?
**A:** Training always changes weights (but not all weight changes are training — quantization isn't). Pre-training = from scratch, random weights, most resource-intensive (up to 98% of compute for InstructGPT). Finetuning = continuing training of a pretrained model, fewer resources. Post-training = training after pre-training, conceptually same as finetuning; done by model developers (instruction-following) vs finetuning by application developers. Feeding journal entries to ChatGPT is prompt engineering, not training.

**Q:** How would you adapt a foundation model for a retailer's product descriptions?
**A:** Prompt engineering: craft detailed instructions with examples capturing the brand voice; RAG: connect the model to a database of customer reviews; finetuning: train further on high-quality product descriptions. Escalate to finetuning only if simpler techniques are insufficient.

**Q:** What is the role of evaluation in AI engineering and why is it harder than in ML?
**A:** Evaluation selects models, benchmarks progress, gates deployment, and detects production issues. It's harder with foundation models because outputs are open-ended — chatbots have too many valid responses to enumerate ground truths — and because many adaptation techniques (prompts) change scores, so results must be compared under identical settings.

**Q:** Explain the December 2023 Gemini MMLU controversy.
**A:** Google claimed Gemini beat ChatGPT on MMLU, but evaluated Gemini with CoT@32 (32 chain-of-thought examples) while ChatGPT was shown only 5 examples; when both used 5 examples, ChatGPT performed better. Scores (Table 1-5) vary wildly with prompt technique — e.g., Gemini Ultra 90.04% CoT@32 vs 83.7% CoT vs 71.8% 5-shot.

**Q:** What are the three layers of the AI engineering stack and what's in each?
**A:** Application development (prompts, context, evaluation, interfaces); model development (modeling/training, dataset engineering, inference optimization); infrastructure (model serving, data/compute management, monitoring). Start from the top and move down as needed.

**Q:** What three ways does AI engineering differ from ML engineering?
**A:** (1) You use someone else's trained model → focus shifts from modeling/training to model adaptation; (2) models are bigger, more compute- and latency-heavy → more pressure for inference optimization and GPU-cluster expertise; (3) open-ended outputs → evaluation is a much bigger problem.

**Q:** How does AI engineering relate to full-stack engineering?
**A:** The rising importance of interfaces pulls AI engineering toward full-stack; tooling is adding JavaScript support (LangChain.js, Transformers.js, OpenAI Node library, Vercel AI SDK). Full-stack engineers can turn ideas into demos fast and iterate; the new workflow builds the product first and invests in data/models only after the product shows promise.

**Q:** What business metrics would you track for a customer-support chatbot?
**A:** % of messages automated, how many more messages can be processed, how much quicker responses are, how much human labor is saved — plus customer satisfaction/feedback and usefulness-threshold metrics (quality, TTFT/TPOT/total latency, cost per inference, interpretability/fairness).

**Q:** Why do companies favor internal-facing AI apps first?
**A:** Internal apps (e.g., knowledge management) build AI engineering expertise while minimizing risks of data privacy, compliance, and catastrophic failures; close-ended tasks (classification) are easier to evaluate, making risks easier to estimate.

**Q:** What makes a layer-on-top-of-a-foundation-model application risky, and what moats can it build?
**A:** If the base model expands capabilities, your layer may be subsumed (e.g., a PDF-parsing app built on ChatGPT's limits). Moats: technology, data, distribution. With foundation models, technology is commoditized; a first-mover's usage-data flywheel can create a data moat (Calendly/Mailchimp/Photoroom started as would-be features of bigger products).

**Q:** What maintenance challenges does an AI product face?
**A:** The field moves like a bullet train: good changes (longer context, better outputs, cheaper inference) still force cost-benefit re-analysis (in-house vs providers whose prices halve; third-party providers going out of business); regulatory changes (GDPR ~$9B compliance, compute restrictions like the US Oct 2023 Executive Order, evolving IP law) can even be fatal.

**Q:** Why is hitting a 100ms latency target hard for foundation models?
**A:** Autoregressive models generate tokens sequentially: at 10ms/token, a 100-token output takes ~1 second; getting to the ~100ms latency users expect from internet applications is a huge challenge, making inference optimization an active subfield (quantization, distillation, parallelism — Ch 7–9).

**Q:** How does dataset engineering differ between traditional ML and AI engineering?
**A:** Traditional ML: mostly feature engineering with tabular data and close-ended annotation (easy to judge spam vs not-spam). AI engineering: open-ended outputs make annotation much harder; work is more about deduplication, tokenization, context retrieval, and quality control (removing sensitive and toxic data). Data needs: training from scratch > finetuning > prompt engineering.

**Q:** What role did Claude Shannon play in language modeling?
**A:** He used sophisticated statistics to decipher enemies' WWII messages; his 1951 landmark paper "Prediction and Entropy of Printed English" introduced concepts like entropy still used in language modeling today.

**Q:** What are the eight application-pattern groups from the 205 open-source apps (Table 1-3)?
**A:** Coding; Image and video production; Writing; Education; Conversational bots; Information aggregation; Data organization; Workflow automation. (An app can belong to more than one.)

**Q:** What evidence shows coding is the most popular use case?
**A:** GitHub Copilot's ARR crossed $100M two years after launch; Magic raised $320M and Anysphere $60M (Aug 2024); gpt-engineer and screenshot-to-code each hit 50,000 GitHub stars within a year; McKinsey: 2× productivity on documentation, +25–50% on code gen/refactoring, minimal on highly complex tasks; AI is better at frontend than backend.

**Q:** What evidence shows AI is good at creative/image work?
**A:** Midjourney hit $200M ARR at 1.5 years old; as of Dec 2023 half of the top-10 free Graphics & Design apps on the Apple App Store have "AI" in their names; tools like Adobe Firefly (photo editing) and Runway/Pika Labs/Sora (video); enterprises generate seasonal/location ad variations with AI.

**Q:** What did the MIT writing study (Noy and Zhang, 2023) find?
**A:** With 453 college-educated professionals, ChatGPT exposure cut average writing time 40% and raised output quality 18%, narrowing the quality gap between workers; exposed workers were 2× as likely to use it after two weeks and 1.6× after two months.

**Q:** Give examples of AI misuse described in the chapter.
**A:** Amazon flooded with shoddy AI-generated travel guidebooks (fake author bios, websites, rave reviews); SEO content farms with junk websites — NewsGuard found ~400 ads from 141 brands on them, one site producing 1,200 articles/day.

**Q:** How does AI affect education, both positively and negatively?
**A:** Positively: personalized lectures, quizzes, AI tutors (Khan Academy), debate partners, Duolingo's lesson personalization (the stage benefiting most from AI). Negatively: cheating fears (NYC and LA banned then un-banned ChatGPT); Chegg's stock fell from $28 (Nov 2022) to $2 (Sept 2024).

**Q:** What are agents and why does the chapter call them a "central topic"?
**A:** Agents are AIs that can plan and use external tools (search, calls, calendar). Interest borders on obsession because agents could make every person vastly more productive and generate huge economic value; Chapter 6 covers them in depth.

**Q:** What is talk-to-your-docs and the "Fast Breakdown" template?
**A:** Talk-to-your-docs: applications that process your documents (contracts, papers) and let you retrieve information conversationally. Instacart's popular "Fast Breakdown" prompt template summarizes meeting notes, emails, and Slack conversations with facts, open questions, and action items.

**Q:** What is IDP and how big is it?
**A:** Intelligent data processing — using AI to extract structured information from unstructured data (receipts, contracts, reports). Estimated to reach $12.81 billion by 2030, growing 32.9% each year.

**Q:** Why is "training a model" different from what a user does with ChatGPT prompts?
**A:** Training changes model weights. Feeding journal entries into ChatGPT via context is prompt engineering — technically not training or finetuning, even if colloquially people call it "training."

**Q:** What three tools are cited for modeling/training, and what ML knowledge does it require?
**A:** TensorFlow (Google), Hugging Face Transformers, PyTorch (Meta). Requires knowing ML algorithms (clustering, logistic regression, decision trees, collaborative filtering), architectures (feedforward, recurrent, convolutional, transformer), and learning concepts (gradient descent, loss function, regularization) — a nice-to-have, not a must-have, with foundation models.

**Q:** What are the interface types for AI applications?
**A:** Standalone web/desktop/mobile apps (Streamlit, Gradio, Plotly Dash); browser extensions; chatbots in chat apps (Slack, Discord, WeChat, WhatsApp); plugin/add-on APIs (VSCode, Shopify, Microsoft 365 — also usable by agents); voice-based; and embodied (AR/VR). Chat is the most common.

**Q:** What two purposes does Chapter 1 serve?
**A:** (1) Explain the emergence of AI engineering as a discipline thanks to foundation models; (2) give an overview of the process of building applications on top of these models.


## Part 3Fill-in-the-Blank

**Q:** If I could use only one word to describe AI post-2020, it'd be ______.
**A:** scale.

**Q:** The demand for AI applications has increased while the ______ to entry for building AI applications has decreased.
**A:** barrier.

**Q:** A ______ model is trained to fill in the blank using context from both before and after.
**A:** masked language.

**Q:** An ______ language model predicts the next token using only the preceding tokens.
**A:** autoregressive (causal).

**Q:** In self-supervision, labels are inferred from the ______, whereas in unsupervised learning you don't need labels at all.
**A:** input data.

**Q:** The ______ marker is especially important because it helps language models know when to end their responses.
**A:** end-of-sequence (<EOS>).

**Q:** A model's size is typically measured by its number of ______.
**A:** parameters.

**Q:** GPT-1 (June 2018) had ______ million parameters; GPT-2 (Feb 2019) had ______ billion.
**A:** 117; 1.5.

**Q:** OpenAI trained CLIP on ______ million (image, text) pairs — 400 times larger than ImageNet.
**A:** 400.

**Q:** Adapting an existing model might take ten examples and one weekend versus ______ examples and six months to build from scratch.
**A:** 1 million.

**Q:** Goldman Sachs estimated AI investment could approach $______ billion in the US and $______ billion globally by 2025.
**A:** 100; 200.

**Q:** FactSet found that ______ in three S&P 500 companies mentioned AI in their Q2 2023 earnings calls, three times more than the year earlier.
**A:** one.

**Q:** Companies that mentioned AI saw their stock price increase an average of ______% vs ______% for those that didn't.
**A:** 4.6; 2.4.

**Q:** In September 2022, ______, CEO of OpenAI, said the biggest opportunity for most people will be to adapt models for specific applications.
**A:** Sam Altman.

**Q:** Eloundou et al. defined a task as exposed if AI can reduce its completion time by at least ______%.
**A:** 50.

**Q:** GitHub Copilot's annual recurring revenue crossed $______ million two years after launch.
**A:** 100.

**Q:** Midjourney had generated $______ million in annual recurring revenue at one and a half years old.
**A:** 200.

**Q:** In the MIT study (Noy and Zhang, 2023), ChatGPT exposure decreased average writing time by ______% and raised output quality by ______%.
**A:** 40; 18.

**Q:** Chegg's share price fell from $______ (Nov 2022) to $______ (Sept 2024).
**A:** 28; 2.

**Q:** According to Salesforce's 2023 research, ______% of generative AI users use it to distill complex ideas and summarize information.
**A:** 74.

**Q:** The IDP (intelligent data processing) industry is estimated to reach $______ billion by 2030, growing ______% each year.
**A:** 12.81; 32.9.

**Q:** In Gartner's 2023 survey, ______% of 2,500 executives cited business continuity as their reason for embracing generative AI.
**A:** 7.

**Q:** Microsoft's framework for gradually increasing AI automation is called ______.
**A:** Crawl-Walk-Run.

**Q:** Face ID wouldn't work without AI, so AI is ______ to the app; Gmail would still work without Smart Compose, so AI is ______.
**A:** critical; complementary.

**Q:** A chatbot is a ______ feature; traffic alerts on Google Maps are a ______ feature.
**A:** reactive; proactive.

**Q:** The three types of competitive advantage in AI are technology, data, and ______.
**A:** distribution.

**Q:** Calendly could've been a feature of Google ______; Mailchimp of Gmail; Photoroom of Google ______.
**A:** Calendar; Photos.

**Q:** Latency metrics include ______ (time to first token), ______ (time per output token), and total latency.
**A:** TTFT; TPOT.

**Q:** In the UltraChat paper, Ding et al. wrote that "the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly ______."
**A:** challenging.

**Q:** LinkedIn took one month to achieve ______% of the experience they wanted, and four more months to surpass ______%.
**A:** 80; 95.

**Q:** Europe's GDPR was estimated to cost businesses $______ billion to become compliant.
**A:** 9.

**Q:** Quantization reduces the ______ of model weights but isn't considered training.
**A:** precision.

**Q:** For InstructGPT, pre-training takes up to ______% of the overall compute and data resources.
**A:** 98.

**Q:** ______ refers to training a model from scratch with randomly initialized weights.
**A:** Pre-training.

**Q:** Feeding childhood journal entries into ChatGPT to make it mimic you is technically ______, not training.
**A:** prompt engineering.

**Q:** Training a model from scratch requires more data than ______, which requires more data than ______ engineering.
**A:** finetuning; prompt.

**Q:** If a model generates a token in 10 ms, a 100-token output takes about ______ second(s).
**A:** 1 (one).

**Q:** Gemini Ultra's MMLU performance went from ______% to ______% when a different prompt engineering technique was used.
**A:** 83.7; 90.04.

**Q:** The application development layer consists of evaluation, prompt engineering, and the ______.
**A:** AI interface.

**Q:** ChatGPT and Perplexity are ______ products; GitHub's Copilot is commonly used as a ______ in VSCode; Grammarly as a browser ______.
**A:** standalone; plug-in; extension.
