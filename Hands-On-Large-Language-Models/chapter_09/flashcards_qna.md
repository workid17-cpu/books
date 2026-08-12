# 📘 Chapter 9 Flashcards: Multimodal Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 9

---

## Part 1: Terms → Definitions

**Q:** What is a modality?
**A:** A type/data domain of input or output a model handles (e.g., text, images, audio, video, sensors). A model handling text and images is called multimodal.

**Q:** What is a multimodal model?
**A:** A model able to deal with different types (modalities) of data, such as images, audio, video, or sensors (Figure 9-1).

**Q:** Can a multimodal model accept a modality as input but not generate in that modality?
**A:** Yes — it's possible to accept a modality as input yet not be able to generate in that modality.

**Q:** What is the Vision Transformer (ViT)?
**A:** An adaptation of the original Transformer (encoder) to computer vision; it converts images into patches, linearly embeds them, and passes them through the encoder as if they were text tokens. Paper: "An Image is Worth 16x16 Words" (Dosovitskiy et al., 2020).

**Q:** What did ViT outperform compared to CNNs?
**A:** ViT does tremendously well on image recognition tasks compared to the previously default convolutional neural networks (CNNs).

**Q:** What is a patch in the ViT context?
**A:** A piece of an image created by cutting it horizontally and vertically into subimages (e.g., 3×3 for illustration, 16×16 in the original implementation); the image equivalent of a token.

**Q:** How does ViT "tokenize" an image?
**A:** It converts the image into patches of subimages, flattens them, and linearly embeds the flattened patches to create numerical representations (embeddings) used as Transformer input.

**Q:** Why can't image patches be assigned vocabulary IDs like text tokens?
**A:** Because patches will rarely be found in other images, unlike the vocabulary of a text — there's no shared "vocabulary" of patches.

**Q:** How are patches treated after being passed to the encoder?
**A:** They are treated exactly as if they were textual tokens — from that point forward there is no difference in how text or image trains.

**Q:** What patch size did the original ViT implementation use?
**A:** 16 × 16 patches (the paper is titled "An Image is Worth 16x16 Words"); illustrations in the chapter use 3×3 for clarity.

**Q:** What is a multimodal embedding model?
**A:** An embedding model that can create embeddings for multiple modalities (e.g., text and images) in the same vector space, allowing cross-modal comparison.

**Q:** What is CLIP?
**A:** Contrastive Language-Image Pre-training — the most well-known and most-used multimodal embedding model (Radford et al., 2021); it computes embeddings of both images and texts in a shared vector space.

**Q:** What four tasks does CLIP's comparison capability support?
**A:** Zero-shot classification, clustering, search (across billions of texts/images), and generation (e.g., driving stable diffusion image generation).

**Q:** What is zero-shot classification with CLIP?
**A:** Comparing the embedding of an image with the embeddings of the descriptions of its possible classes to find which class is most similar — no training examples needed.

**Q:** What is contrastive learning?
**A:** Training that maximizes similarity (cosine similarity) for similar pairs (image↔its caption) and minimizes it for dissimilar pairs; requires negative examples of unrelated pairs.

**Q:** What are negative examples in contrastive training?
**A:** Pairs of images and captions that are not related, included so the model learns what makes things dissimilar — modeling similarity means knowing what makes things different too.

**Q:** What is OpenCLIP?
**A:** The open source variant of CLIP; using it (or any CLIP model) boils down to processing the textual and image inputs before passing them to the main model.

**Q:** What three components does OpenCLIP use?
**A:** A tokenizer (text), a preprocessor (images, resizing/normalizing), and the main model that converts the previous outputs to embeddings.

**Q:** What is cosine similarity?
**A:** The cosine of the angle between vectors, calculated as the dot product of the embeddings divided by the product of their lengths (seen in Chapter 4); used to compare CLIP image/text embeddings.

**Q:** What are the `<|startoftext|>` and `<|endoftext|>` tokens in CLIP?
**A:** Special tokens marking the start and end of the text input, separating it from a potential image embedding.

**Q:** Why is the [CLS] token "missing" from CLIP's text tokenization?
**A:** Because in CLIP the [CLS] token is actually used to represent the image embedding.

**Q:** What does `get_text_features` return in OpenCLIP?
**A:** The text embedding — for a single caption, shape `torch.Size([1, 512])`.

**Q:** What does `clip_processor(text=None, images=image, ...)["pixel_values"]` return?
**A:** The preprocessed image tensor; a 512×512 image becomes shape `torch.Size([1, 3, 224, 224])` (resized to 224×224).

**Q:** What is the shape of the CLIP image embedding?
**A:** `torch.Size([1, 512])` — the same shape as the text embedding, which allows the two to be compared.

**Q:** How do you compute the CLIP text–image similarity score in code?
**A:** Normalize both embeddings (`embedding /= embedding.norm(dim=-1, keepdim=True)`), then compute `np.dot(text_embedding, image_embedding.T)`.

**Q:** What similarity score does the puppy image/caption example produce?
**A:** ≈ 0.33 (`array([[0.33149648]])`) — which is high relative to unrelated pairs in the 3×3 similarity matrix (Figure 9-14).

**Q:** What is sentence-transformers' `clip-ViT-B-32` model?
**A:** A CLIP-based model loaded via `SentenceTransformer("clip-ViT-B-32")` that makes multimodal embedding creation much easier (`model.encode(images)`, `model.encode(captions)`, `util.cos_sim`).

**Q:** What is BLIP-2?
**A:** Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation (Li et al., 2023) — an easy-to-use, modular technique that introduces vision capabilities to existing language models.

**Q:** What is the Q-Former?
**A:** The Querying Transformer — the only trainable component of BLIP-2; the bridge connecting a pretrained (frozen) image encoder (ViT) and a pretrained (frozen) LLM.

**Q:** What two modules does the Q-Former have, and what do they share?
**A:** An Image Transformer (interacts with the frozen ViT for feature extraction) and a Text Transformer (interacts with the LLM); they share their attention layers.

**Q:** What three tasks is the Q-Former trained on in step 1?
**A:** Image-text contrastive learning, image-text matching (matched/unmatched classification), and image-grounded text generation — jointly optimized.

**Q:** What is image-text contrastive learning (BLIP-2)?
**A:** A task that aligns pairs of image and text embeddings such that they maximize their mutual information.

**Q:** What is image-text matching (BLIP-2)?
**A:** A classification task that predicts whether an image and text pair is positive (matched) or negative (unmatched).

**Q:** What is image-grounded text generation (BLIP-2)?
**A:** Training the model to generate text based on information extracted from the input image.

**Q:** What is a soft visual prompt?
**A:** The learnable embeddings produced by the Q-Former and passed to the LLM; they condition the LLM on the visual representations, similar to providing context when prompting an LLM.

**Q:** Why is there a fully connected linear layer between the Q-Former and the LLM?
**A:** To make sure the learnable embeddings have the same shape that the LLM expects (the projection layer).

**Q:** What are the two stages of BLIP-2 training?
**A:** Step 1: representation learning on image–document pairs (three joint objectives); Step 2: converting the learned representations to soft visual prompts fed to the LLM through a projection layer.

**Q:** What is LLaVA?
**A:** A framework for making textual LLMs multimodal (Liu et al., "Visual instruction tuning," NeurIPS 36, 2024); connects a pretrained CLIP-like visual encoder with a textual LLM.

**Q:** What is Idefics 2?
**A:** An efficient visual LLM based on the Mistral 7B LLM (Laurençon et al., 2024); connects a CLIP-like visual encoder with a textual LLM.

**Q:** What is the shared goal of LLaVA, Idefics 2, and BLIP-2?
**A:** To project visual features from input images into language embeddings that can be used as input for an LLM — bridging the gap between images and text.

**Q:** What is the BLIP-2 processor?
**A:** Comparable to the tokenizer of language models; it converts unstructured input (images and text) into representations the model expects (e.g., resizing images to 224×224).

**Q:** What model ID does the chapter use for BLIP-2?
**A:** `Salesforce/blip2-opt-2.7b`, loaded with `torch_dtype=torch.float16`.

**Q:** What tokenizer does the `blip2-opt-2.7b` model use?
**A:** A GPT2Tokenizer (vocab_size 50265, special tokens `<s>`, `</s>`, `<pad>`).

**Q:** What does the Ġ symbol mean in GPT2Tokenizer output?
**A:** It represents a space character — an internal function moves the space's code point up by 256 (space = 32 → Ġ = 288) to make it printable.

**Q:** After replacing Ġ with _, what does the underscore indicate?
**A:** The beginning of a word, so that words made up of multiple tokens can be recognized (e.g., `_vocal` + `ization`).

**Q:** What is visual question answering (VQA)?
**A:** Presenting the model an image along with a question about that image so it can answer; the model processes both the image and the question at once.

**Q:** What is the VQA prompt format used in the chapter?
**A:** `"Question: Write down what you see in this picture. Answer:"` — the model completes the answer.

**Q:** What is ipywidgets?
**A:** A Jupyter-notebook extension that allows creating interactive buttons, text inputs, etc.; used to build the interactive BLIP-2 chatbot.

**Q:** What is the Rorschach test?
**A:** An old psychological experiment testing an individual's perception of inkblots; what someone sees supposedly reveals personality characteristics (BLIP-2 captioned the blot "a black and white ink drawing of a bat").

**Q:** What are the special tokens in the CLIP text tokenization for "a puppy playing in the snow"?
**A:** `<|startoftext|>` at the start and `<|endoftext|>` at the end, with tokens like `a</w>`, `puppy</w>`, `playing</w>`, `in</w>`, `the</w>`, `snow</w>` (the `</w>` marks word endings).

**Q:** What does the `</w>` marker indicate in CLIP tokens?
**A:** That the token ends a word (word-boundary marker).

**Q:** What is `model.get_image_features()` used for in OpenCLIP?
**A:** Converting preprocessed image pixel values into an image embedding (shape `[1, 512]`).

**Q:** What does `.norm(dim=-1, keepdim=True)` do before computing similarity?
**A:** It L2-normalizes the embeddings so the dot product equals cosine similarity.

**Q:** What does `util.cos_sim` from sentence-transformers return?
**A:** A cosine-similarity matrix between the encoded image embeddings and text embeddings.

## Part 2: Short Answer

**Q:** Why might multimodal input increase LLM capabilities?
**A:** Language doesn't live in a vacuum — body language, facial expressions, and intonation enhance communication; enabling LLMs to reason about multimodal information can unlock capabilities previously locked and allow deployment for new kinds of problems.

**Q:** Describe the full ViT pipeline from image to embeddings.
**A:** 1) Cut the image into patches (grid cuts); 2) flatten each patch; 3) linearly embed the flattened patches into embeddings; 4) pass the patch embeddings to the Transformer encoder, where they're treated exactly like textual tokens.

**Q:** Why do we linearly embed patches instead of assigning them IDs?
**A:** Because patches rarely appear in other images (unlike text vocabulary), so there's no reusable vocabulary of patch IDs — embeddings are the natural numerical representation.

**Q:** How does CLIP place text and images in the same vector space?
**A:** It trains a text encoder and an image encoder on millions of image–caption pairs using contrastive learning: maximize cosine similarity for matching image–caption pairs and minimize it for mismatching pairs, updating both encoders each batch.

**Q:** Why are negative examples important in CLIP training?
**A:** Because modeling similarity is not only knowing what makes things similar but also what makes them different and dissimilar; negatives teach the model to separate unrelated representations.

**Q:** In CLIP, why must the text and image embeddings have the same shape?
**A:** Because they need to be compared (e.g., via cosine similarity) — same shape `[1, 512]` makes the comparison possible.

**Q:** What is the advantage of multimodal embeddings lying in the same vector space?
**A:** Representations from different modalities with similar meaning end up close together, so we can compare/retrieve across modalities (e.g., find images matching text, or documents related to an image).

**Q:** How does the BLIP-2 Q-Former connect vision and language?
**A:** It has an Image Transformer that interacts with the frozen ViT for feature extraction and a Text Transformer that interacts with the LLM; the two share attention layers, and the Q-Former is the only trainable component.

**Q:** Walk through BLIP-2 step 1 (representation learning).
**A:** Image–caption pairs are used; images are fed to the frozen ViT to extract vision embeddings (input to Q-Former's Image Transformer), captions go into the Text Transformer; the Q-Former is trained on three joint objectives (contrastive learning, image-text matching, image-grounded text generation) to inject textual info into the visual embeddings.

**Q:** Walk through BLIP-2 step 2 (soft visual prompts).
**A:** The learnable embeddings from step 1 (containing visual info in the same dimensional space as textual info) are passed through a fully connected linear projection layer into the frozen LLM, where they serve as soft visual prompts that condition the LLM on the visual representations.

**Q:** Why does building a multimodal LLM from scratch require so much compute/data?
**A:** It requires billions of images, text, and image-text pairs — not easily feasible; BLIP-2 avoids this by leveraging pretrained models and only training the bridge (Q-Former).

**Q:** What are the use cases of BLIP-2 demonstrated in the chapter?
**A:** Image captioning (supercar → "an orange supercar driving on the road at sunset"; Rorschach → "a black and white ink drawing of a bat") and multimodal chat-based prompting/VQA (image + question → answer; follow-up chat; interactive ipywidgets chatbot).

**Q:** Why might BLIP-2 captioning fail on some images?
**A:** Domain-specific images (e.g., specific cartoon characters or imaginary creations) may fail because the model was trained on largely public data.

**Q:** Why should you be careful inputting very wide or tall images to BLIP-2?
**A:** The processor resizes all images to 224×224 squares, so very wide or tall images might get distorted.

**Q:** How does the BLIP-2 chat-based prompting work?
**A:** The previous conversation (including the model's answers) is included in the prompt (template `"Question: {} Answer: {}."`), then a new `"Question: ... Answer:"` is appended; the model generates the next answer (e.g., "$1,000,000" for the supercar cost follow-up).

**Q:** How does the ipywidgets BLIP-2 chatbot maintain conversation?
**A:** It keeps a `memory` list of (question, answer) tuples; on each text entry it builds the prompt from memory plus the new question, generates with `max_new_tokens=100`, trims the answer (`.split("Question")[0]`), appends to memory, and displays USER/BLIP-2 messages.

**Q:** What is the relationship between the text and image embeddings in a multimodal embedding model?
**A:** They lie in the same vector space, so embeddings with similar meaning are close together even though they came from different modalities.

**Q:** What happens during CLIP training as batches progress?
**A:** After calculating similarity, the model (both text and image encoders) is updated and the process repeats with new batches and updated representations, gradually moving matching pairs closer and mismatches apart.

**Q:** What does `model.vision_model` and `model.language_model` reveal in BLIP-2?
**A:** Which ViT (vision model) and which generative model (LLM) are used inside the loaded BLIP-2 model.

**Q:** What does the `"Question: Write down what you see in this picture. Answer:"` prompt cause BLIP-2 to do?
**A:** Perform visual question answering — describe the input image (without the prompt it would simply generate a caption).

**Q:** What is the significance of the similarity score 0.33 in the CLIP example?
**A:** In isolation it's hard to interpret, but when compared against a similarity matrix of three images and three captions it is high relative to the other (unrelated) pairs.

**Q:** What is the difference between image captioning and VQA in BLIP-2 usage?
**A:** Captioning passes only the image (the model generates a caption); VQA passes the image AND a text prompt/question so the model answers a specific question about the image.

**Q:** Why does BLIP-2 need both a processor and a model?
**A:** The processor converts unstructured input (images and text) into representations the model expects; the model performs the generation.

**Q:** What is the overall takeaway of Chapter 9 about making LLMs multimodal?
**A:** Bridging text and vision (via ViT patch embeddings, CLIP contrastive embeddings, and BLIP-2's Q-Former soft visual prompts) enables new applications like image captioning, cross-modal search, and multimodal chat-based prompting.

**Q:** How is the CLIP similarity score of 0.33 interpreted properly?
**A:** By comparing it against a similarity matrix of three images and three captions (Figure 9-14) — relative to the unrelated pairs, 0.33 is high.

**Q:** What data is needed to train a multimodal embedding model like CLIP?
**A:** Millions of images alongside captions (image–caption pairs), used to create two representations per pair (image and caption embeddings).

**Q:** How does image captioning differ from plain text preprocessing in BLIP-2?
**A:** Captioning passes only the image through the processor to pixel values, then the model generates a caption; text is only involved when using VQA/prompting.

**Q:** Why does the Q-Former's two modules sharing attention layers matter?
**A:** It lets vision features (from the frozen ViT) and text features (for the LLM) be processed in a coordinated way by a single trainable bridge.

## Part 3: Fill-in-the-Blank

**Q:** A model able to handle text and images is said to be __________.
**A:** multimodal.

**Q:** It's possible for a model to accept a modality as __________ yet not be able to __________ in that modality.
**A:** input; generate.

**Q:** The Vision Transformer cuts an image into pieces called __________, which are then linearly embedded.
**A:** patches.

**Q:** The ViT paper is titled "An Image is Worth __________ Words."
**A:** 16x16 (256).

**Q:** In ViT, the image patches are treated exactly as if they were __________ tokens once passed to the encoder.
**A:** textual.

**Q:** ViT outperformed the previously default __________ (CNNs) on image recognition.
**A:** convolutional neural networks.

**Q:** The most well-known multimodal embedding model is __________ (Contrastive Language-Image Pre-training).
**A:** CLIP.

**Q:** CLIP compares image and text embeddings using __________ similarity.
**A:** cosine.

**Q:** CLIP's training method, which maximizes similarity for matching pairs and minimizes it for mismatches, is called __________ learning.
**A:** contrastive.

**Q:** To model dissimilarity, the training process must include __________ examples of unrelated images and captions.
**A:** negative.

**Q:** The open source variant of CLIP is called __________.
**A:** OpenCLIP.

**Q:** The OpenCLIP model ID used in the chapter is __________.
**A:** `openai/clip-vit-base-patch32`.

**Q:** The CLIP text embedding has shape __________.
**A:** `torch.Size([1, 512])`.

**Q:** The CLIP processor resizes a 512×512 image to __________ pixels.
**A:** 224 × 224.

**Q:** The CLIP image pixel_values shape is __________.
**A:** `torch.Size([1, 3, 224, 224])`.

**Q:** In CLIP, the [CLS] token represents the __________ embedding.
**A:** image.

**Q:** The puppy image/caption similarity score in the chapter is ≈ __________.
**A:** 0.33 (0.33149648).

**Q:** BLIP-2 stands for Bootstrapping __________ Pre-training for Unified Vision-Language Understanding and Generation.
**A:** Language-Image.

**Q:** The bridge in BLIP-2 that connects a frozen image encoder and a frozen LLM is called the __________ (Querying Transformer).
**A:** Q-Former.

**Q:** The Q-Former has an Image Transformer and a __________ Transformer, which share attention layers.
**A:** Text.

**Q:** In BLIP-2 step 1, the Q-Former is trained on three objectives: image-text contrastive learning, image-text __________, and image-grounded text generation.
**A:** matching.

**Q:** In BLIP-2 step 2, the learned embeddings are passed to the LLM through a fully connected __________ layer as soft visual prompts.
**A:** linear (projection).

**Q:** The BLIP-2 model loaded in the chapter is __________.
**A:** `Salesforce/blip2-opt-2.7b`.

**Q:** BLIP-2's text tokenizer is a __________.
**A:** GPT2Tokenizer (vocab 50265).

**Q:** In GPT2Tokenizer output, the __________ symbol represents a space (space code point 32 moved up by 256).
**A:** Ġ.

**Q:** BLIP-2 resizes input images to __________ pixels.
**A:** 224 × 224.

**Q:** BLIP-2 captioned the supercar image as "an orange supercar driving on the road at __________."
**A:** sunset.

**Q:** BLIP-2 captioned the Rorschach inkblot as "a black and white __________ of a bat."
**A:** ink drawing.

**Q:** In the VQA example, BLIP-2 answered the supercar's cost follow-up with __________.
**A:** "$1,000,000."

**Q:** Presenting both an image and a question about it to a model is called __________ question answering.
**A:** visual.

**Q:** The interactive BLIP-2 chatbot is built with the Jupyter extension __________.
**A:** ipywidgets.

**Q:** The chatbot keeps conversation history in a __________ list of (question, answer) tuples.
**A:** memory.

**Q:** The VQA prompt template starts with "__________: Write down what you see in this picture. __________:"
**A:** Question; Answer.

**Q:** LLaVA is a framework for making textual LLMs multimodal based on __________ instruction tuning.
**A:** visual.

**Q:** Idefics 2 is an efficient visual LLM based on the __________ 7B LLM.
**A:** Mistral.

**Q:** In the Rorschach example, the inkblot test is from an old __________ experiment.
**A:** psychological.

**Q:** Chapter 10 previews creating and fine-tuning a text __________ model.
**A:** embedding.

**Q:** The chapter's contrastive-learning deep dive (creating an embedding model) is promised in Chapter __________.
**A:** 10.

**Q:** The input_ids tensor for the puppy caption begins with the token ID __________ (the `<|startoftext|>` token).
**A:** 49406.

**Q:** The attention_mask returned by the CLIP tokenizer for the caption is __________.
**A:** all ones (`[1, 1, 1, 1, 1, 1, 1, 1]`).

**Q:** In the similarity computation, embeddings are first __________ before the dot product.
**A:** normalized.

**Q:** The supercar image used in the captioning example has __________ pixels (an unusual format).
**A:** 520 × 492.
