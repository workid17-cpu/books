# 📘 Chapter 9 Study Bundle: Multimodal Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 9

---

## §1. Study Notes

### Core Theme
This chapter explores how LLMs are extended to handle **multiple modalities** (types of data — text, images, audio, video, sensors). It covers three topics in sequence: (1) **Transformers for vision** — the Vision Transformer (ViT), which converts images into patches, embeds them linearly, and feeds them to a Transformer encoder exactly as if they were text tokens; (2) **Multimodal embedding models** — chiefly **CLIP** (Contrastive Language-Image Pre-training), which uses a text encoder + image encoder and contrastive learning (cosine-similarity maximization/minimization on image–caption pairs) to place text and image embeddings in the **same vector space**, enabling zero-shot classification, clustering, search, and generation; and (3) **Making text generation models multimodal** — chiefly **BLIP-2**, which bridges a frozen image encoder (ViT) and a frozen LLM using a trainable **Querying Transformer (Q-Former)**, trained in two stages (representation learning + soft visual prompt), enabling image captioning and multimodal chat-based prompting (visual question answering). The chapter ends with the practical code (OpenCLIP embeddings, BLIP-2 captioning/VQA/chat) and previews Chapter 10 (creating/fine-tuning a text embedding model).

### Key Definitions
- **Modality:** A type of data a model handles (e.g., text, images, audio, video, sensors). A model able to handle text and images is said to be **multimodal**.
- **Vision Transformer (ViT):** An adaptation of the original Transformer (encoder) to computer vision; converts an image into patches of subimages, linearly embeds the flattened patches, and passes those "patch tokens" through the encoder. Paper: "An Image is Worth 16x16 Words" (Dosovitskiy et al., 2020).
- **Patch:** A piece of an image cut horizontally and vertically (e.g., 3×3 or 16×16 grid); the image equivalent of a token.
- **Patch embedding:** The numerical representation created by linearly projecting (embedding) flattened image patches; these are used as the input of a Transformer and treated exactly like text-token embeddings.
- **Multimodal embedding model:** An embedding model that can embed multiple modalities (e.g., text and images) into the **same vector space**, so text and image embeddings can be compared (e.g., find images matching text).
- **CLIP (Contrastive Language-Image Pre-training):** The most well-known/used multimodal embedding model (Radford et al., 2021); computes embeddings of both images and texts in a shared vector space using a text encoder and an image encoder trained with contrastive learning on image–caption pairs.
- **Contrastive learning:** Training that maximizes similarity (cosine similarity) for similar pairs (image↔its caption) and minimizes it for dissimilar pairs; requires negative examples (unrelated pairs). Detailed in Chapter 10.
- **OpenCLIP:** The open source variant of CLIP; using it (or any CLIP model) means processing textual and image inputs before passing them to the main model.
- **CLIPTokenizerFast / CLIPProcessor / CLIPModel:** The three Hugging Face components used with CLIP: tokenizer (text → IDs), processor (images → resized/normalized pixel values), and model (previous outputs → embeddings).
- **Cosine similarity:** The cosine of the angle between two vectors = dot product of embeddings divided by the product of their lengths; used to compare image and text embeddings.
- **[CLS] token:** In CLIP, the token that represents the **image embedding** (contrast with text where [CLS] is used in BERT-style models); hence it is "missing" from the text tokenization.
- **`<|startoftext|>` / `<|endoftext|>`:** Special CLIP tokens marking the start and end of text, separating it from a potential image embedding.
- **Sentence Transformer (sentence-transformers):** A library implementing CLIP-based models (e.g., `clip-ViT-B-32`) that make creating multimodal embeddings much easier (a few lines of code, `util.cos_sim`).
- **BLIP-2 (Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation):** A modular, easy-to-use technique (Li et al., 2023) that adds vision capabilities to existing LLMs by connecting a frozen image encoder and a frozen LLM through a trainable bridge — the Q-Former.
- **Q-Former (Querying Transformer):** The only trainable component of BLIP-2; the bridge between vision (ViT) and text (LLM). Has two modules sharing attention layers: an **Image Transformer** (interacts with the frozen ViT for feature extraction) and a **Text Transformer** (interacts with the LLM).
- **Image-text contrastive learning (BLIP-2 task):** Aligns pairs of image and text embeddings to maximize mutual information.
- **Image-text matching (BLIP-2 task):** A classification task predicting whether an image–text pair is positive (matched) or negative (unmatched).
- **Image-grounded text generation (BLIP-2 task):** Trains the model to generate text based on information extracted from the input image.
- **Soft visual prompt:** The learnable embeddings produced by the Q-Former that are passed to the LLM; they condition the LLM on the visual representations, similar to how you'd give an LLM context when prompting.
- **Projection layer:** A fully connected linear layer between the Q-Former and the LLM that makes the learnable embeddings have the same shape the LLM expects.
- **Processor (BLIP-2 context):** Comparable to the tokenizer of language models; converts unstructured input (images and text) into representations the model expects (e.g., resizing images to 224×224, tokenizing text).
- **Visual question answering (VQA):** Presenting both an image and a question about that image at once so the model can answer; a multimodal chat-based use case.
- **ipywidgets:** A Jupyter-notebook extension used to build interactive widgets (text input, buttons); used to build the interactive BLIP-2 chatbot.
- **LLaVA:** A framework for making textual LLMs multimodal (Liu et al., "Visual instruction tuning," 2024).
- **Idefics 2:** An efficient visual LLM based on the Mistral 7B LLM (Laurençon et al., 2024).
- **Rorschach test:** A psychological experiment using inkblots; individuals' perceptions of inkblots supposedly reveal personality characteristics (used as a fun BLIP-2 captioning example — the model captioned it "a black and white ink drawing of a bat").

### Core Concepts & Frameworks
- **Multimodality motivation (Figure 9-1):** LLMs are language models, but they're far more useful if they can handle other data types (e.g., glance at a picture and answer questions). A model can **accept a modality as input yet not be able to generate in that modality** — input and output modalities are independent.
- **Emergent abilities (footnote: Wei et al., 2022):** As models grow larger and smarter, their skill sets grow (generalization, reasoning, arithmetic, linguistics). Multimodal input could further unlock capabilities previously locked. Language doesn't live in a vacuum — body language, facial expressions, and intonation enhance spoken word; enabling LLMs to reason over multimodal info lets them solve new kinds of problems.
- **Chapter structure:** (1) How images are converted to numerical representations via an adaptation of the Transformer (ViT); (2) how LLMs are extended to include vision tasks using this Transformer; practical use cases.
- **Vision Transformer pipeline (Figures 9-2–9-5):**
  1. Text needs tokenization before the encoder; images have no words, so this can't be used directly.
  2. ViT cuts an image into **patches** (illustrated as 3×3, original implementation 16×16 — hence "An Image is Worth 16x16 Words").
  3. Unlike text tokens, patches can't be assigned vocabulary IDs (patches rarely recur across images), so they are **linearly embedded** to create embeddings.
  4. Patch embeddings are passed to the encoder and **treated exactly as if they were textual tokens** — from that point forward there is no difference in how text or image trains.
- **Why ViT matters for LLMs:** Because the encoder treats patch embeddings like token embeddings, the ViT is often used to make language models multimodal; the most straightforward way is during training of embedding models.
- **Multimodal embedding models (Figures 9-6, 9-7):** Create embeddings for multiple modalities in the same vector space, allowing comparison (image↔text). Example: find images similar to "pictures of a puppy," or find documents related to an image. Embeddings with similar meaning are close to each other regardless of modality.
- **CLIP uses:** Zero-shot classification (compare image embedding with class-description embeddings), clustering (cluster images + keywords), search (across billions of texts/images), and generation (drive image generation, e.g., stable diffusion — Rombach et al., 2022).
- **How CLIP works (Figures 9-8–9-11):** Train on millions of image–caption pairs. Use a text encoder (text) and image encoder (image) to produce two embeddings per pair. Compare via cosine similarity. Initially low similarity (not yet aligned); during training maximize similarity for matching pairs and minimize for non-matching pairs; update both encoders each batch. Eventually a cat image's embedding is close to "a picture of a cat." **Negative examples** (unrelated image/caption pairs) must be included — modeling similarity means knowing what makes things dissimilar too.
- **OpenCLIP usage (three components):** Load (1) `CLIPTokenizerFast` for text, (2) `CLIPProcessor` to preprocess/resize images, (3) `CLIPModel` for embeddings. Model ID: `openai/clip-vit-base-patch32`.
- **CLIP preprocessing details:** Tokenizing "a puppy playing in the snow" yields `input_ids` with special `<|startoftext|>` and `<|endoftext|>` tokens and a full `attention_mask`; tokens like `a</w>`, `puppy</w>` (the `</w>` marks word boundaries). The `[CLS]` token is missing from text because in CLIP it represents the **image** embedding. Text embedding shape `[1, 512]`. Image preprocessing (via `clip_processor`) resizes a 512×512 image to **224×224** → pixel_values shape `[1, 3, 224, 224]`; image embedding shape also `[1, 512]` (same shape as text — required for comparison). Similarity computed by normalizing both embeddings then dot product: score ≈ **0.33** for the puppy image/caption, which is high relative to unrelated pairs (Figure 9-14 similarity matrix).
- **Sentence-transformers CLIP:** `SentenceTransformer("clip-ViT-B-32")`; `model.encode(images)` / `model.encode(captions)`; `util.cos_sim(image_embeddings, text_embeddings)` for the similarity matrix.
- **Why BLIP-2 (bridging the gap):** Building a multimodal LLM from scratch needs billions of images/text/pairs and enormous compute. BLIP-2 instead **bridges** a pretrained image encoder and a pretrained LLM with a trainable **Q-Former**, so only the bridge is trained (Figure 9-16).
- **Q-Former architecture:** Two modules sharing attention layers — an Image Transformer (interacts with frozen ViT for feature extraction) and a Text Transformer (interacts with the LLM).
- **BLIP-2 training — two stages (Figures 9-17–9-20):**
  - **Step 1 (representation learning, one per modality):** Use image–document (caption) pairs. Images → frozen ViT → vision embeddings → Q-Former's Image Transformer; captions → Q-Former's Text Transformer. Train the Q-Former on **three objectives jointly**: image-text contrastive learning, image-text matching (classification matched/unmatched), and image-grounded text generation. This injects textual information into the frozen ViT's embeddings.
  - **Step 2 (soft visual prompts):** The learnable embeddings from Step 1 (now containing visual info in the same dimensional space as textual info) are passed through a fully connected **linear projection layer** (to match the LLM's expected shape) into the frozen LLM as **soft visual prompts**, conditioning the LLM on the visual representations.
- **Other visual LLMs:** LLaVA (visual instruction tuning; Liu et al., 2024) and Idefics 2 (efficient visual LLM based on Mistral 7B; Laurençon et al., 2024). Both connect pretrained CLIP-like visual encoders with textual LLMs, projecting visual features from input images to language embeddings usable as LLM input — same bridging idea as Q-Former.
- **BLIP-2 usage pipeline:** Load `AutoProcessor` + `Blip2ForConditionalGeneration` (`Salesforce/blip2-opt-2.7b`; `torch_dtype=torch.float16`; `device = "cuda" if torch.cuda.is_available() else "cpu"`). The processor = tokenizer-like component for images+text. `model.vision_model` and `model.language_model` reveal which ViT and generative model are used.
- **Image preprocessing (BLIP-2):** A 520×492 image becomes pixel_values shape `[1, 3, 224, 224]` — resized to **224×224 squares**; very wide or tall images may get **distorted**, so be careful.
- **Text preprocessing (BLIP-2):** Uses a **GPT2Tokenizer** (vocab_size 50265, special tokens `<s>`, `</s>`, `<pad>`). Example: "Her vocalization was remarkably melodic" → tokens `['</s>', 'Her', 'Ġvocal', 'ization', 'Ġwas', 'Ġremarkably', 'Ġmel', 'odic']`. The **Ġ symbol represents a space** (internal function moves code points up by 256 to make them printable: space code point 32 → Ġ code point 288). Replacing `Ġ` with `_` shows the underscore marks the **beginning of a word**, so multi-token words can be recognized.
- **Use Case 1 — Image captioning:** Image → pixel values → BLIP-2 → soft visual prompts → LLM generates a caption. Example: supercar image → "an orange supercar driving on the road at sunset" (`model.generate(**inputs, max_new_tokens=20)`, `batch_decode` with `skip_special_tokens=True`). Domain-specific images (specific cartoon characters, imaginary creations) may fail because the model was trained on largely public data. Rorschach inkblot (Roy Schafer, 1954) → "a black and white ink drawing of a bat."
- **Use Case 2 — Multimodal chat-based prompting / VQA:** Give the model an image **and** a question; it processes both at once. Prompt format: `"Question: Write down what you see in this picture. Answer:"` → "A sports car driving on the road at sunset." Chat-like prompting includes the previous Q&A in the prompt: `"Question: ... Answer: A sports car driving on the road at sunset. Question: What would it cost me to drive that car? Answer:"` → "$1,000,000." Building an interactive chatbot with **ipywidgets** (VBox of output + text input, `observe(text_eventhandler, "value")`, `memory` list of (question, answer) tuples, template `"Question: {} Answer: {}."`, `max_new_tokens=100`, split on "Question") creates a chatbot that can reason about images.
- **Chapter 10 preview:** Part III covers training and fine-tuning; Chapter 10 = creating and fine-tuning a text embedding model — the core technology driving many LM applications and the deep dive into contrastive learning.

### Important Numbers / Stats / Tokens
- Emergent abilities paper: Wei et al., "Emergent abilities of large language models," arXiv:2206.07682 (2022) (p.260).
- ViT paper: Dosovitskiy et al., "An image is worth 16x16 words: Transformers for image recognition at scale," arXiv:2010.11929 (2020) (p.260).
- Example cat image: 512 × 512 pixels (p.261).
- Illustrative patching 3 × 3; original implementation 16 × 16 patches (p.262).
- CLIP paper: Radford et al., "Learning transferable visual models from natural language supervision," ICML, PMLR, 2021 (p.265).
- Stable diffusion (image generation) citation: Rombach et al., "High-resolution image synthesis with latent diffusion models," CVPR, 2022 (p.265).
- OpenCLIP model ID: `openai/clip-vit-base-patch32` (p.269).
- CLIP puppy caption tokens: `['<|startoftext|>', 'a</w>', 'puppy</w>', 'playing</w>', 'in</w>', 'the</w>', 'snow</w>', '<|endoftext|>']`; input_ids `[49406, 320, 6829, 1629, 530, 518, 2583, 49407]` (p.269).
- CLIP text embedding shape `torch.Size([1, 512])`; image pixel_values shape `torch.Size([1, 3, 224, 224])` (512×512 original → 224×224); image embedding shape `torch.Size([1, 512])` (p.270–271).
- Puppy image–caption similarity score: `array([[0.33149648]])` ≈ 0.33 (p.271).
- BLIP-2 paper: Li et al., "BLIP-2: Bootstrapping language-image pretraining with frozen image encoders and large language models," ICML, PMLR, 2023 (p.274).
- LLaVA: Liu et al., "Visual instruction tuning," NeurIPS 36 (2024) (p.277).
- Idefics 2: Laurençon et al., "What matters when building vision-language models?," arXiv:2405.02246 (2024); based on Mistral 7B LLM (p.277).
- BLIP-2 model ID: `Salesforce/blip2-opt-2.7b`; loaded with `torch_dtype=torch.float16` (p.278).
- BLIP-2 image preprocessing: 520×492 image → `torch.Size([1, 3, 224, 224])` (p.279).
- BLIP-2 uses GPT2Tokenizer: vocab_size 50265, special tokens `{'bos_token': '</s>', 'eos_token': '</s>', 'unk_token': '</s>', 'pad_token': '<pad>'}` (p.279).
- GPT2Tokenizer space handling: space (code point 32) → Ġ (code point 288) — characters moved up by 256 to be printable (p.280).
- Captioning examples: supercar → "an orange supercar driving on the road at sunset" (p.281); Rorschach inkblot → "a black and white ink drawing of a bat" (p.283).
- Rorschach reference: Roy Schafer, *Psychoanalytic Interpretation in Rorschach Testing: Theory and Application* (1954) (p.282).
- VQA prompt output: "A sports car driving on the road at sunset" (p.284); chat follow-up answer "$1,000,000" (p.284).
- Chatbot memory: `memory` list of (question, answer) tuples; template `"Question: {} Answer: {}."`; `max_new_tokens=100`; text split on `"Question"` (p.284–285).

### Algorithms & Formulæ
- **ViT image tokenization:** image → patches (horizontal+vertical cuts) → flatten each patch → linear projection (embedding) → pass to Transformer encoder as if textual tokens.
- **Cosine similarity:** `cos(θ) = (A·B) / (|A||B|)` — dot product of embeddings divided by product of lengths; used to compare CLIP image/text embeddings.
- **CLIP contrastive training:** for each image–caption pair, maximize cosine similarity of the image embedding and its caption embedding; minimize similarity for negative (unrelated) pairs; update both text and image encoders via gradient descent each batch.
- **Embedding similarity (code):** normalize (`embedding /= embedding.norm(dim=-1, keepdim=True)`) then `np.dot(text_embedding, image_embedding.T)` → similarity score.
- **BLIP-2 step 1 (three objectives):** image-text contrastive (align embeddings, maximize mutual information) + image-text matching (classification: matched/unmatched) + image-grounded text generation (generate text from image-extracted info) — jointly optimized.
- **BLIP-2 step 2 (projection):** learnable Q-Former embeddings → fully connected linear layer → LLM as soft visual prompt.
- **GPT2Tokenizer space encoding:** internal function moves characters in certain code points up by 256 to make them printable → space (32) becomes Ġ (288); `Ġ` marks the start of a word.

### Diagrams / Visuals
- **Figure 9-1** — Multimodal models handle different data types (images, audio, video, sensors); a model may accept a modality as input yet not generate in it.
- **Figure 9-2** — Original Transformer and Vision Transformer both convert unstructured data to numerical representations for tasks like classification.
- **Figure 9-3** — Text is passed to encoders by first tokenizing it.
- **Figure 9-4** — The "tokenization" process for images: image → patches of subimages.
- **Figure 9-5** — Main ViT algorithm: patch → linear projection → patch embeddings → encoder, treated like textual tokens.
- **Figure 9-6** — Multimodal embedding models create embeddings for multiple modalities in the same vector space.
- **Figure 9-7** — Embeddings with similar meaning are close in vector space even across modalities.
- **Figure 9-8** — Data needed to train a multimodal embedding model: millions of images with captions.
- **Figure 9-9** — Step 1 of CLIP training: image and text embedded with image/text encoders.
- **Figure 9-10** — Step 2 of CLIP training: cosine similarity between sentence and image embeddings.
- **Figure 9-11** — Step 3 of CLIP training: encoders updated so similar inputs move closer in vector space.
- **Figure 9-12** — AI-generated (stable diffusion) image of a puppy playing in the snow.
- **Figure 9-13** — The preprocessed (resized) input image by CLIP.
- **Figure 9-14** — Similarity matrix between three images and three captions (0.33 is relatively high).
- **Figure 9-15** — A multimodal text generation model (BLIP-2) reasoning about input images.
- **Figure 9-16** — The Q-Former is the bridge between vision (ViT) and text (LLM); the only trainable component.
- **Figure 9-17** — Two-stage BLIP-2: step 1 representation learning for vision+language; step 2 converts representations to soft visual prompts for the LLM.
- **Figure 9-18** — Step 1: frozen ViT output + caption, trained on three contrastive-like tasks.
- **Figure 9-19** — Step 2: Q-Former embeddings passed to the LLM through a projection layer as a soft visual prompt.
- **Figure 9-20** — The full BLIP-2 procedure.
- **Figure 9-21** — A Rorschach test inkblot (BLIP-2 captions it "a black and white ink drawing of a bat").

### Common Exam Traps
- **Input vs output modalities:** A model can accept a modality as input but not generate in that modality (Figure 9-1) — don't assume input and output are symmetric.
- **ViT vs CNN:** ViT (Transformer encoder on image patches) replaced CNNs as the default for image recognition; don't describe ViT as a CNN.
- **Patches ≠ tokens with IDs:** Image patches are **not** assigned vocabulary IDs (they rarely recur across images); they are **linearly embedded** instead.
- **ViT patch sizes:** illustrations use 3×3; the original implementation uses **16×16** (paper title "An Image is Worth 16x16 Words").
- **Once embedded, patches are treated exactly like textual tokens** — no difference in how text or image trains after that point.
- **CLIP = contrastive learning:** maximize similarity for matching image–caption pairs, minimize for non-matching; **negative examples are required** — similarity modeling includes knowing what makes things dissimilar.
- **CLIP [CLS] token:** represents the **image** embedding, which is why it's "missing" from the text tokenization.
- **CLIP shapes:** text embedding `[1, 512]`, image pixel_values `[1, 3, 224, 224]` (from 512×512), image embedding `[1, 512]` — text and image embeddings share the same shape for comparability.
- **0.33 similarity is high relatively** — raw scores are hard to interpret in isolation; compare against a similarity matrix of unrelated pairs.
- **BLIP-2 trains ONLY the bridge (Q-Former):** the image encoder (ViT) and LLM are **frozen**; building from scratch would need billions of images/text/pairs.
- **Q-Former = two modules sharing attention:** Image Transformer (with frozen ViT) + Text Transformer (with the LLM).
- **BLIP-2 three objectives:** image-text contrastive learning, image-text matching (matched/unmatched classification), image-grounded text generation — all jointly optimized in step 1.
- **Step 2 = soft visual prompts:** learnable embeddings → linear projection layer → frozen LLM; the projection makes embeddings match the LLM's expected shape.
- **Processor vs tokenizer:** In BLIP-2 the processor handles images AND text (resize to 224×224 squares; tokenize text). Wide/tall images may be **distorted**.
- **BLIP-2 text tokenizer = GPT2Tokenizer** (vocab 50265) — not the CLIP tokenizer.
- **Ġ = space in GPT2Tokenizer:** space code point 32 moved up by 256 → Ġ (code point 288); underscores replace Ġ and mark **word beginnings** (multi-token words).
- **Captioning vs VQA:** captioning = image → caption (no prompt); VQA/chat = image + question/prompt together (`"Question: ... Answer:"` format); chat keeps previous Q&A in the prompt and uses `memory`.
- **Domain limitation:** BLIP-2 trained on largely public data → domain-specific images (cartoon characters, imaginary creations) may fail.
- **Other visual LLMs:** LLaVA (visual instruction tuning) and Idefics 2 (based on Mistral 7B) also connect CLIP-like visual encoders with textual LLMs — same bridging idea.
- **Chapter 10 preview:** text embedding model creation/fine-tuning; deep dive into contrastive learning (mentioned repeatedly in this chapter).

### Chapter Summary
Chapter 9 shows how LLMs become **multimodal** by bridging text and vision. **Transformers for vision:** the Vision Transformer (ViT) cuts images into 16×16 patches, linearly embeds them, and feeds them through the Transformer encoder exactly like text tokens. **Multimodal embedding models:** CLIP uses a text encoder and an image encoder trained with **contrastive learning** (maximize cosine similarity for matching image–caption pairs, minimize for mismatches) to place text and image embeddings in one shared vector space — enabling zero-shot classification, clustering, search, and generation; the open source **OpenCLIP** makes this practical (tokenizer + processor + model; `openai/clip-vit-base-patch32`; text and image embeddings both `[1, 512]`, images resized to 224×224). **Multimodal text generation:** BLIP-2 bridges a frozen ViT and a frozen LLM with a trainable **Q-Former**, trained in two stages (three joint objectives: contrastive learning, image-text matching, image-grounded text generation; then soft visual prompts through a projection layer). Practical use cases include **image captioning** ("an orange supercar driving on the road at sunset"; Rorschach bat) and **multimodal chat-based prompting / VQA** (question+image prompts, chat-like Q&A history, an ipywidgets chatbot). The next chapter (10) covers creating and fine-tuning a text embedding model.

### Confidence Check
- **Sure:** modality definition; ViT patch-based pipeline (16×16, linear embedding, treated as text tokens); CLIP architecture (text + image encoders, shared vector space, contrastive learning, negative examples); OpenCLIP components and shapes (`[1,512]` text/image embeddings, `[1,3,224,224]` pixels); cosine similarity for comparison; 0.33 puppy score; BLIP-2 design (frozen ViT + frozen LLM + trainable Q-Former, only the bridge trained); Q-Former's two modules sharing attention; three training objectives; two training stages (representation learning + soft visual prompts with projection layer); processor resizing to 224×224 squares and distortion warning; GPT2Tokenizer `Ġ` = space (code point 32→288) and word-start underscore; captioning and VQA/chat prompt formats (`Question: ... Answer:`), memory tuple list, ipywidgets chatbot; captioning examples (supercar, Rorschach bat); $1,000,000 chat answer; LLaVA/Idefics 2 (Mistral 7B) bridging; Chapter 10 preview (text embedding model + contrastive learning deep dive).
- **Uncertain:** exact page anchors for figures (PDF text-flow approximate); precise wording of some quoted outputs where extraction broke lines mid-sentence; exact character offsets in code snippets paraphrased from extraction; the exact ipywidgets layout strings (e.g., `flex_flow="column-reverse"`).

---

## §2. Code & Pseudocode Breakdown

### Code Block 1: Loading the puppy image
```python
from urllib.request import urlopen
from PIL import Image
# Load an AI-generated image of a puppy playing in the snow
puppy_path = "https://raw.githubusercontent.com/HandsOnLLM/Hands-On-Large-
Language-Models/main/chapter09/images/puppy.png"
image = Image.open(urlopen(puppy_path)).convert("RGB")
caption = "a puppy playing in the snow"
```
- Loads the AI-generated puppy image from the book's GitHub repo and defines its caption.
- **Terms:** `urlopen` opens a URL; `.convert("RGB")` normalizes color channels; `caption` is the text paired with the image.

### Code Block 2: Loading OpenCLIP components
```python
from transformers import CLIPTokenizerFast, CLIPProcessor, CLIPModel
model_id = "openai/clip-vit-base-patch32"
clip_tokenizer = CLIPTokenizerFast.from_pretrained(model_id)
clip_processor = CLIPProcessor.from_pretrained(model_id)
model = CLIPModel.from_pretrained(model_id)
```
- Loads the three CLIP components: tokenizer (text), processor (images), and model (embeddings).
- **Terms:** `CLIPTokenizerFast` tokenizes text; `CLIPProcessor` preprocesses/resizes images; `CLIPModel` converts both into embeddings.

### Code Block 3: Tokenizing the caption
```python
inputs = clip_tokenizer(caption, return_tensors="pt")
inputs
# {'input_ids': tensor([[49406, 320, 6829, 1629, 530, 518, 2583, 49407]]),
#  'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1]])}
```
- Tokenizing the caption produces `input_ids` and an `attention_mask`.
- **Terms:** `return_tensors="pt"` returns PyTorch tensors; `input_ids` are token IDs; `attention_mask` marks real vs padding tokens.

### Code Block 4: Converting IDs back to tokens
```python
clip_tokenizer.convert_ids_to_tokens(inputs["input_ids"][0])
# ['<|startoftext|>', 'a</w>', 'puppy</w>', 'playing</w>', 'in</w>', 'the</w>', 'snow</w>', '<|endoftext|>']
```
- Shows the tokens: special start/end tokens plus word tokens with `</w>` word-boundary markers.
- **Terms:** `convert_ids_to_tokens` maps IDs to tokens; `</w>` indicates the token ends a word; `<|startoftext|>` / `<|endoftext|>` delimit text (the `[CLS]` token is absent because in CLIP it represents the image embedding).

### Code Block 5: Creating the text embedding
```python
text_embedding = model.get_text_features(**inputs)
text_embedding.shape
# torch.Size([1, 512])
```
- The text encoder produces a single 512-dimension embedding for the caption.
- **Terms:** `get_text_features` returns the text embedding; shape `[1, 512]` = 1 sample × 512 features.

### Code Block 6: Preprocessing the image
```python
processed_image = clip_processor(
    text=None, images=image, return_tensors="pt"
)["pixel_values"]
processed_image.shape
# torch.Size([1, 3, 224, 224])
```
- The processor resizes the 512×512 image to 224×224 (the model's expected input size).
- **Terms:** `pixel_values` = normalized image tensors; `[1, 3, 224, 224]` = batch × RGB channels × height × width.

### Code Block 7: Visualizing the preprocessed image
```python
import torch
import numpy as np
import matplotlib.pyplot as plt
img = processed_image.squeeze(0)
img = img.permute(*torch.arange(img.ndim - 1, -1, -1))
img = np.einsum("ijk->jik", img)
plt.imshow(img)
plt.axis("off")
```
- Rearranges tensor axes so the image can be displayed with matplotlib.
- **Terms:** `squeeze` drops the batch dim; `permute`/`einsum` reorder channel/height/width axes; `plt.imshow` displays the image.

### Code Block 8: Creating the image embedding
```python
image_embedding = model.get_image_features(processed_image)
image_embedding.shape
# torch.Size([1, 512])
```
- The image encoder produces a 512-dimension embedding — the **same shape** as the text embedding, which enables comparison.
- **Terms:** `get_image_features` returns the image embedding; same `[1, 512]` shape as text.

### Code Block 9: Computing text–image similarity
```python
text_embedding /= text_embedding.norm(dim=-1, keepdim=True)
image_embedding /= image_embedding.norm(dim=-1, keepdim=True)
text_embedding = text_embedding.detach().cpu().numpy()
image_embedding = image_embedding.detach().cpu().numpy()
score = np.dot(text_embedding, image_embedding.T)
score
# array([[0.33149648]], dtype=float32)
```
- Normalizes both embeddings then takes the dot product (cosine similarity) → score ≈ 0.33.
- **Terms:** `.norm(dim=-1, keepdim=True)` L2-normalizes; `.detach().cpu().numpy()` moves tensors off the graph to NumPy; `np.dot` computes the cosine similarity after normalization.

### Code Block 10: Sentence-transformers CLIP loading
```python
from sentence_transformers import SentenceTransformer, util
model = SentenceTransformer("clip-ViT-B-32")
image_embeddings = model.encode(images)
text_embeddings = model.encode(captions)
sim_matrix = util.cos_sim(image_embeddings, text_embeddings)
```
- Loads a CLIP-based model through sentence-transformers and computes a full image–caption cosine-similarity matrix in a few lines.
- **Terms:** `SentenceTransformer` wrapper; `encode` produces embeddings for both images and captions; `util.cos_sim` builds the similarity matrix (Figure 9-14).

### Code Block 11: Loading BLIP-2
```python
from transformers import AutoProcessor, Blip2ForConditionalGeneration
import torch
blip_processor = AutoProcessor.from_pretrained("Salesforce/blip2-opt-2.7b")
model = Blip2ForConditionalGeneration.from_pretrained(
    "Salesforce/blip2-opt-2.7b",
    torch_dtype=torch.float16
)
device = "cuda" if torch.cuda.is_available() else "cpu"
model.to(device)
```
- Loads the BLIP-2 processor and model; `model.vision_model` and `model.language_model` reveal the ViT and generative model.
- **Terms:** `AutoProcessor` handles both images and text; `Blip2ForConditionalGeneration` is the multimodal model; `torch_dtype=torch.float16` for memory/speed; `device` = GPU if available.

### Code Block 12: Loading the supercar image
```python
car_path = "https://raw.githubusercontent.com/HandsOnLLM/Hands-On-Large-
Language-Models/main/chapter09/images/car.png"
image = Image.open(urlopen(car_path)).convert("RGB")
image
```
- Loads the AI-generated supercar image (520×492 pixels, an unusual format).

### Code Block 13: Preprocessing the image with BLIP-2
```python
inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)
inputs["pixel_values"].shape
# torch.Size([1, 3, 224, 224])
```
- The processor resizes the 520×492 image to a 224×224 square; very wide/tall images may be distorted.
- **Terms:** `.to(device, torch.float16)` moves to GPU in half precision.

### Code Block 14: Inspecting BLIP-2's tokenizer
```python
blip_processor.tokenizer
# GPT2TokenizerFast(name_or_path='Salesforce/blip2-opt-2.7b', vocab_size=50265, ...,
#   special_tokens={'bos_token': '</s>', 'eos_token': '</s>', 'unk_token': '</s>', 'pad_token': '<pad>'})
```
- BLIP-2 uses a GPT2Tokenizer (vocab 50265) with special tokens.

### Code Block 15: Tokenizing text with BLIP-2
```python
text = "Her vocalization was remarkably melodic"
token_ids = blip_processor(image, text=text, return_tensors="pt")
token_ids = token_ids.to(device, torch.float16)["input_ids"][0]
tokens = blip_processor.tokenizer.convert_ids_to_tokens(token_ids)
tokens
# ['</s>', 'Her', 'Ġvocal', 'ization', 'Ġwas', 'Ġremarkably', 'Ġmel', 'odic']
```
- Tokenizing shows the Ġ symbol — an internal function moves the space character (code point 32) up by 256 to make it printable, so the space becomes Ġ (code point 288).
- **Terms:** `Ġ` represents a space; e.g., "Her vocalization" → `'Her', 'Ġvocal', 'ization'`.

### Code Block 16: Replacing the space token with underscores
```python
tokens = [token.replace("Ġ", "_") for token in tokens]
tokens
# ['</s>', 'Her', '_vocal', 'ization', '_was', '_remarkably', '_mel', 'odic']
```
- Replacing `Ġ` with `_` shows the underscore marks the **beginning of a word**, so multi-token words (e.g., `_vocal` + `ization`) can be recognized.

### Code Block 17: Image captioning (supercar)
```python
image = Image.open(urlopen(car_path)).convert("RGB")
inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)
generated_ids = model.generate(**inputs, max_new_tokens=20)
generated_text = blip_processor.batch_decode(
    generated_ids, skip_special_tokens=True
)
generated_text = generated_text[0].strip()
generated_text
# 'an orange supercar driving on the road at sunset'
```
- Captioning: image → pixel values → BLIP-2 → soft visual prompts → LLM generates a caption.
- **Terms:** `model.generate` produces output token IDs; `batch_decode` converts them to text; `skip_special_tokens=True` removes special tokens; `max_new_tokens=20` limits length.

### Code Block 18: Captioning the Rorschach inkblot
```python
url = "https://upload.wikimedia.org/wikipedia/commons/7/70/Rorschach_blot_01.jpg"
image = Image.open(urlopen(url)).convert("RGB")
inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)
generated_ids = model.generate(**inputs, max_new_tokens=20)
generated_text = blip_processor.batch_decode(
    generated_ids, skip_special_tokens=True
)
generated_text = generated_text[0].strip()
# 'a black and white ink drawing of a bat'
```
- The model captions the Rorschach inkblot as "a black and white ink drawing of a bat."

### Code Block 19: Visual question answering
```python
prompt = "Question: Write down what you see in this picture. Answer:"
inputs = blip_processor(image, text=prompt, return_tensors="pt").to(device, torch.float16)
generated_ids = model.generate(**inputs, max_new_tokens=30)
generated_text = blip_processor.batch_decode(
    generated_ids, skip_special_tokens=True
)
generated_text = generated_text[0].strip()
# 'A sports car driving on the road at sunset'
```
- VQA: image + question prompt are processed together; without the prompt the model would just caption.
- **Terms:** `text=prompt` supplies the question; the `Question: ... Answer:` format steers the model.

### Code Block 20: Chat-like prompting
```python
prompt = "Question: Write down what you see in this picture. Answer: A sports \
car driving on the road at sunset. Question: What would it cost me to drive \
that car? Answer:"
inputs = blip_processor(image, text=prompt, return_tensors="pt").to(device, torch.float16)
generated_ids = model.generate(**inputs, max_new_tokens=30)
generated_text = blip_processor.batch_decode(
    generated_ids, skip_special_tokens=True
)
generated_text = generated_text[0].strip()
# '$1,000,000'
```
- Chat-like prompting: previous Q&A is included in the prompt; the model answers the follow-up question.
- **Terms:** the conversation history is concatenated into the prompt; answer "$1,000,000" shows chat-like behavior.

### Code Block 21: Interactive chatbot with ipywidgets
```python
from IPython.display import HTML, display
import ipywidgets as widgets

def text_eventhandler(*args):
    question = args[0]["new"]
    if question:
        args[0]["owner"].value = ""
        if not memory:
            prompt = " Question: " + question + " Answer:"
        else:
            template = "Question: {} Answer: {}."
            prompt = " ".join(
                [template.format(memory[i][0], memory[i][1])
                 for i in range(len(memory))]
            ) + " Question: " + question + " Answer:"
        inputs = blip_processor(image, text=prompt, return_tensors="pt")
        inputs = inputs.to(device, torch.float16)
        generated_ids = model.generate(**inputs, max_new_tokens=100)
        generated_text = blip_processor.batch_decode(
            generated_ids, skip_special_tokens=True
        )
        generated_text = generated_text[0].strip().split("Question")[0]
        memory.append((question, generated_text))
        output.append_display_data(HTML("<b>USER:</b> " + question))
        output.append_display_data(HTML("<b>BLIP-2:</b> " + generated_text))
        output.append_display_data(HTML("<br>"))

in_text = widgets.Text()
in_text.continuous_update = False
in_text.observe(text_eventhandler, "value")
output = widgets.Output()
memory = []
display(
    widgets.VBox(
        children=[output, in_text],
        layout=widgets.Layout(display="inline-flex", flex_flow="column-reverse"),
    )
)
```
- Builds an interactive chatbot that keeps a `memory` list of (question, answer) tuples, builds each prompt from the conversation history, generates with `max_new_tokens=100`, and displays USER/BLIP-2 messages.
- **Terms:** `ipywidgets` provides interactive widgets; `observe(text_eventhandler, "value")` triggers on text entry; `memory` stores Q&A pairs; `.split("Question")[0]` trims the answer text; `VBox` stacks the output and input boxes.
