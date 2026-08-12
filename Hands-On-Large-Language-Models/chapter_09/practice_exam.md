# 📘 Practice Exam — Chapter 9: Multimodal Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 9
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. What is a modality?
   a) A type of data a model handles (e.g., text, images, audio, video, sensors)
   b) A specific Transformer architecture
   c) A training dataset of captioned images
   d) A type of tokenizer used for images

2. What does the Vision Transformer (ViT) demonstrate compared to CNNs?
   a) It is slower on image recognition tasks
   b) It does tremendously well on image recognition tasks compared to the previously default CNNs
   c) It only works on text data
   d) It cannot be used with the Transformer encoder

3. How does ViT "tokenize" an image?
   a) By converting it to a long string of text
   b) By assigning every pixel a vocabulary ID
   c) By cutting the image into patches of subimages (horizontally and vertically)
   d) By feeding the raw pixel matrix directly to a CNN

4. How are image patches converted into numerical representations?
   a) By looking them up in a patch vocabulary
   b) By hashing each pixel value
   c) By averaging all patches into one vector
   d) By linearly embedding the flattened patches to create embeddings

5. What patch size did the original ViT implementation use?
   a) 16 × 16
   b) 3 × 3
   c) 8 × 8
   d) 224 × 224

6. Why can't each image patch be assigned an ID like a text token?
   a) Patches are too large to store
   b) Patches will rarely be found in other images, unlike the vocabulary of a text
   c) The encoder cannot process IDs
   d) There are too many possible patch colors

7. Once patch embeddings are passed to the encoder, how are they treated?
   a) As completely separate from text
   b) As CNN feature maps
   c) Exactly as if they were textual tokens
   d) As raw pixel values

8. What does CLIP stand for?
   a) Combined Language and Image Processing
   b) Contrastive Language-Image Pre-training
   c) Contextual Latent Image Projection
   d) Convolutional Language Image Prediction

9. Which two encoders does CLIP use to produce image and text embeddings?
   a) A single shared encoder for both
   b) A CNN and a GPT model
   c) A text encoder and an image encoder
   d) A Q-Former and a decoder

10. How does CLIP compare image and text embeddings?
    a) By Euclidean distance only
    b) By cosine similarity (dot product divided by product of lengths)
    c) By BM25 scoring
    d) By exact token matching

11. What is contrastive learning in the CLIP context?
    a) Training with only positive examples
    b) Training the model to memorize image captions
    c) Maximizing similarity for similar image/caption pairs and minimizing it for dissimilar pairs
    d) Contrasting model sizes to find the best one

12. Why should negative examples be included in CLIP training?
    a) To double the dataset size
    b) To speed up training
    c) To balance class labels
    d) Because modeling similarity also means knowing what makes things dissimilar

13. Using OpenCLIP (or any CLIP model) boils down to which two things?
    a) Processing the textual and image inputs before passing them to the main model
    b) Training a new encoder and decoder
    c) Fine-tuning a CNN on images
    d) Downloading the model and running inference without preprocessing

14. What is the shape of the CLIP text embedding for a single caption?
    a) torch.Size([1, 768])
    b) torch.Size([1, 512])
    c) torch.Size([1, 3, 224, 224])
    d) torch.Size([512, 512])

15. The CLIP processor resized the 512 × 512 puppy image to what shape?
    a) [1, 512, 512]
    b) [1, 3, 224, 224]
    c) [1, 224]
    d) [3, 512, 512]

16. In CLIP, what does the [CLS] token represent?
    a) The start of the text
    b) The end of the caption
    c) A padding token
    d) The image embedding

17. What similarity score did the puppy image and its caption produce in the OpenCLIP example?
    a) ≈ 0.33
    b) ≈ 0.85
    c) ≈ 1.00
    d) ≈ 0.05

18. Which sentence-transformers model ID is used to load CLIP in the chapter?
    a) "clip-ViT-B-16"
    b) "clip-ViT-B-32"
    c) "clip-ViT-L-14"
    d) "openai/clip-vit-large"

19. What is the Q-Former in BLIP-2?
    a) A frozen image encoder
    b) A frozen LLM
    c) The only trainable component, bridging a pretrained image encoder and a pretrained LLM
    d) A tokenizer for multimodal input

20. What two modules make up the Q-Former, and what do they share?
    a) A CNN and an RNN; they share loss functions
    b) An encoder and a decoder; they share parameters
    c) A ViT and a CLIP encoder; they share weights
    d) An Image Transformer and a Text Transformer; they share attention layers

21. Which three objectives is the Q-Former trained on in step 1?
    a) Image-text contrastive learning, image-text matching, image-grounded text generation
    b) Language modeling, next-sentence prediction, token classification
    c) Image classification, object detection, segmentation
    d) Reranking, dense retrieval, RAG

22. What is image-text matching in BLIP-2?
    a) A generation task producing captions
    b) A retrieval task finding similar images
    c) A segmentation task labeling pixels
    d) A classification task predicting whether an image–text pair is positive (matched) or negative (unmatched)

23. What are soft visual prompts in BLIP-2?
    a) Actual image pixels sent to the LLM
    b) Text descriptions of the image
    c) Learnable embeddings passed to the LLM that condition it on the visual representations
    d) Hard-coded prompts in the model weights

24. Why is a fully connected linear layer placed between the Q-Former and the LLM?
    a) To add non-linearity for better accuracy
    b) To regularize the embeddings
    c) To convert images back to pixels
    d) To make sure the learnable embeddings have the same shape the LLM expects

25. Which BLIP-2 model does the chapter load?
    a) Salesforce/blip2-opt-2.7b
    b) Salesforce/blip2-opt-7b
    c) Salesforce/blip2-flan-t5-xl
    d) openai/clip-vit-base-patch32

26. To what size does the BLIP-2 processor resize input images?
    a) 512 × 512
    b) 224 × 224
    c) 128 × 128
    d) 256 × 256

27. Which tokenizer does the loaded BLIP-2 model use?
    a) CLIPTokenizerFast
    b) A LlamaTokenizer
    c) A GPT2Tokenizer
    d) A T5Tokenizer

28. What does the Ġ symbol represent in the GPT2Tokenizer output?
    a) A newline character
    b) A punctuation mark
    c) A padding token
    d) A space (its code point was moved up by 256 to be printable)

29. What caption did BLIP-2 generate for the supercar image?
    a) "an orange supercar driving on the road at sunset"
    b) "a red sports car parked in a garage"
    c) "a race car on a track during the day"
    d) "a blue car on a highway at noon"

30. What caption did BLIP-2 generate for the Rorschach inkblot?
    a) "a black and white picture of a butterfly"
    b) "a black and white ink drawing of a bat"
    c) "a colorful abstract painting"
    d) "a photograph of a bird"

31. What is the VQA prompt format used in the chapter?
    a) "Describe: ..."
    b) "Summarize: ..."
    c) "Question: ... Answer:"
    d) "Image: ... Text:"

32. What did BLIP-2 answer when asked the cost to drive the supercar as a follow-up question?
    a) "$100,000"
    b) "$10,000,000"
    c) "$1,000"
    d) "$1,000,000"

33. What is ipywidgets?
    a) A Jupyter-notebook extension for interactive buttons and text input
    b) A multimodal embedding model
    c) A PyTorch data loader
    d) A tokenizer for multimodal text

34. How does the ipywidgets BLIP-2 chatbot maintain the conversation?
    a) By re-training the model on each answer
    b) By storing all Q&A pairs in a memory list and rebuilding the prompt
    c) By fine-tuning a classifier
    d) By storing the image only

35. What is LLaVA?
   a) A variant of CLIP for zero-shot classification
   b) A dataset of image–caption pairs
   c) A tokenizer for vision models
   d) A framework for making textual LLMs multimodal (visual instruction tuning)

36. Idefics 2 is an efficient visual LLM based on which LLM?
    a) Llama 2 7B
    b) Phi-3 mini
    c) GPT-3.5
    d) Mistral 7B

37. Why is creating a multimodal LLM from scratch not easily feasible?
    a) It requires billions of images, text, and image-text pairs plus significant computing power
    b) The Transformer architecture cannot support images
    c) GPUs are not powerful enough for any training
    d) There is no training data available for images

38. Why might BLIP-2 captioning fail on domain-specific images like cartoon characters?
    a) The processor cannot resize such images
    b) The tokenizer does not support them
    c) The model was trained on largely public data
    d) The model only captions photographs

39. What answer did BLIP-2 give for the VQA prompt "Question: Write down what you see in this picture. Answer:" on the supercar?
    a) "an orange supercar driving on the road at sunset"
    b) "$1,000,000"
    c) "A sports car driving on the road at sunset"
    d) "a black and white ink drawing of a bat"

40. What does Chapter 10 preview?
    a) Creating and fine-tuning a text embedding model
    b) Image generation with stable diffusion
    c) Building a multimodal chatbot
    d) Fine-tuning a vision Transformer

---

## Section B: True/False (1 point each)

41. A model can accept a modality as input yet not be able to generate in that modality. (T/F)
42. ViT relies on the decoder component of the Transformer architecture. (T/F)
43. Image patches are assigned vocabulary IDs just like text tokens. (T/F)
44. The original ViT implementation used 3 × 3 patches. (T/F)
45. Multimodal embedding models place text and image embeddings in the same vector space. (T/F)
46. CLIP is trained only on images with no captions. (T/F)
47. During CLIP training, similarity should be maximized for matching image/caption pairs and minimized for dissimilar pairs. (T/F)
48. In CLIP, the [CLS] token is used to represent the image embedding. (T/F)
49. BLIP-2 trains the image encoder and LLM from scratch. (T/F)
50. The Q-Former is the only trainable component of the BLIP-2 pipeline. (T/F)
51. In BLIP-2 step 2, the learned embeddings serve as soft visual prompts to the LLM. (T/F)
52. BLIP-2 resizes all input images to 224 × 224 squares, so very wide or tall images might get distorted. (T/F)
53. The Ġ symbol in GPT2Tokenizer output represents a space character. (T/F)
54. Visual question answering presents the image and a question to the model simultaneously. (T/F)
55. Image captioning and visual question answering are identical tasks with no difference. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Explain how ViT converts an image into numerical representations, and why patches are linearly embedded rather than assigned IDs.
57. What is contrastive learning in CLIP, and why are negative examples required?
58. Describe the three components used with OpenCLIP and what each does, including the model ID used.
59. What is the significance of the text and image embeddings having the same shape (e.g., [1, 512]) in CLIP, and how is similarity computed?
60. Explain the BLIP-2 architecture: what is the Q-Former, what does it connect, and why is it the only trainable component?
61. Describe the three objectives the Q-Former is trained on in step 1 and the purpose of the linear projection layer in step 2.
62. What does the Ġ symbol represent in GPT2Tokenizer output, and how is it derived (code points)?
63. Explain the difference between image captioning and visual question answering, using the prompt format "Question: ... Answer:".
64. How does chat-based prompting with BLIP-2 work, and how does the ipywidgets chatbot maintain conversation memory?
65. Why might BLIP-2 fail on domain-specific images, and what image-related preprocessing caution should users observe?

---

## Section D: Essay / Applied (5 points each)

66. **Transformers for vision.** Explain why Transformers were extended to computer vision, how ViT "tokenizes" images (patches, flattening, linear embedding, 16×16 original size, treated as textual tokens), the role of the encoder, and why patches cannot be assigned vocabulary IDs.
67. **CLIP and multimodal embeddings.** Describe how CLIP works end-to-end: the image–caption dataset, text/image encoders, cosine similarity, contrastive learning with negative examples, the shared vector space, tasks enabled (zero-shot classification, clustering, search, generation), the OpenCLIP components and code outputs (token IDs, [1,512] text embedding, [1,3,224,224] pixels, 0.33 similarity), and the sentence-transformers alternative.
68. **BLIP-2 and the Q-Former.** Explain why BLIP-2 avoids training from scratch, the Q-Former architecture (Image Transformer + Text Transformer sharing attention), the two training stages (three joint objectives in step 1; soft visual prompts through a projection layer in step 2), and how LLaVA and Idefics 2 follow the same bridging idea.
69. **Preprocessing multimodal inputs.** Detail the BLIP-2 processor: how it handles images (resize to 224×224 squares, distortion warning for wide/tall images) and text (GPT2Tokenizer, vocab 50265, Ġ-as-space code point 32→288, underscores marking word starts), plus loading the model (Salesforce/blip2-opt-2.7b, float16, GPU).
70. **Use cases.** Describe the practical applications demonstrated: image captioning (supercar → "an orange supercar driving on the road at sunset"; Rorschach inkblot → "a black and white ink drawing of a bat"; domain limitation), visual question answering ("Question: ... Answer:" prompt), chat-based prompting ($1,000,000 follow-up), and the ipywidgets interactive chatbot (memory of Q&A tuples, prompt template, max_new_tokens, trimming on "Question").

---

## ANSWER KEY

### Section A: Multiple Choice
1. a
2. b
3. c
4. d
5. a
6. b
7. c
8. b
9. c
10. b
11. c
12. d
13. a
14. b
15. b
16. d
17. a
18. b
19. c
20. d
21. a
22. d
23. c
24. d
25. a
26. b
27. c
28. d
29. a
30. b
31. c
32. d
33. a
34. b
35. d
36. d
37. a
38. c
39. c
40. a

### Section B: True/False
41. **T** — A model may accept a modality as input without generating in it (Figure 9-1).
42. **F** — ViT relies on the ENCODER component, not the decoder.
43. **F** — Patches are linearly embedded; they are NOT assigned vocabulary IDs.
44. **F** — The original implementation used 16 × 16 patches (3×3 was only for illustration).
45. **T** — Multimodal embedding models embed multiple modalities into the same vector space.
46. **F** — CLIP is trained on millions of images ALONGSIDE captions (image–caption pairs).
47. **T** — Contrastive learning maximizes matching-pair similarity and minimizes dissimilar-pair similarity.
48. **T** — In CLIP the [CLS] token represents the image embedding (hence it's "missing" from text).
49. **F** — BLIP-2 uses FROZEN pretrained image encoder and LLM; only the bridge (Q-Former) is trained.
50. **T** — The Q-Former is the only trainable component of the BLIP-2 pipeline.
51. **T** — In step 2, Q-Former embeddings are passed to the LLM as soft visual prompts.
52. **T** — All images are resized to 224 × 224 squares; wide/tall images may be distorted.
53. **T** — Ġ represents a space (code point 32 shifted up by 256 → 288).
54. **T** — VQA presents the image and a question together.
55. **F** — Captioning passes only the image; VQA adds a question/prompt.

### Section C: Short Answer (model answers)
56. **ViT.** It cuts the image into patches (grid cuts horizontally/vertically), flattens each patch, and linearly embeds the flattened patches into embeddings. These are passed to the Transformer encoder and treated exactly like textual tokens. Patches are linearly embedded rather than assigned IDs because patches rarely recur across images, so there is no reusable patch vocabulary.
57. **Contrastive learning.** CLIP trains a text encoder and image encoder on image–caption pairs; it maximizes cosine similarity for matching pairs and minimizes it for dissimilar pairs. Negative examples (unrelated image/caption pairs) are required because modeling similarity also requires knowing what makes representations dissimilar.
58. **OpenCLIP components.** (1) `CLIPTokenizerFast` tokenizes text; (2) `CLIPProcessor` preprocesses/resizes images; (3) `CLIPModel` converts the outputs into embeddings. Model ID: `openai/clip-vit-base-patch32`.
59. **Shared shape.** Text and image embeddings both have shape [1, 512], which makes them comparable. Similarity is computed by normalizing both embeddings (L2 norm) then taking the dot product (cosine similarity) — the puppy example scored ≈ 0.33.
60. **BLIP-2 architecture.** The Q-Former (Querying Transformer) is a bridge connecting a frozen pretrained image encoder (ViT) and a frozen pretrained LLM. It has an Image Transformer and a Text Transformer sharing attention layers. Because both endpoints are frozen, only the Q-Former is trained, avoiding the huge compute/data cost of building from scratch.
61. **Three objectives (step 1).** Image-text contrastive learning (align pairs, maximize mutual information), image-text matching (classify matched vs unmatched pairs), and image-grounded text generation (generate text from image info). In step 2, a fully connected linear projection layer reshapes the learnable embeddings to match the LLM's expected shape, serving as soft visual prompts.
62. **Ġ symbol.** It represents a space: an internal tokenizer function moves the space character (code point 32) up by 256 to make it printable, producing Ġ (code point 288). Replacing Ġ with underscores shows word beginnings so multi-token words can be recognized.
63. **Captioning vs VQA.** Captioning passes only the image and the model generates a caption. VQA presents the image AND a question (prompt format "Question: ... Answer:"), and the model answers that specific question about the image (e.g., "A sports car driving on the road at sunset").
64. **Chat-based prompting.** The previous conversation (Q&A pairs) is included in the prompt (template "Question: {} Answer: {}."), then a new "Question: ... Answer:" is appended; the model continues the answer (e.g., "$1,000,000"). The ipywidgets chatbot stores all Q&A pairs in a `memory` list, rebuilds the prompt from memory on each entry, generates with `max_new_tokens=100`, trims on "Question", appends the new pair, and displays USER/BLIP-2 messages.
65. **Limitations.** BLIP-2 was trained on largely public data, so domain-specific images (specific cartoon characters, imaginary creations) may fail. Preprocessing caution: the processor resizes all images to 224×224 squares, so very wide or tall images may be distorted.

### Section D: Essay (grading notes)
66. **Expect** Transformers' NLP success → computer vision; ViT outperforms CNNs; image "tokenization" = patches (grid cuts), flattened, linearly embedded; original 16×16 (3×3 illustration); encoder converts to numerical representations; patches treated as textual tokens (no difference in training); patches can't get vocabulary IDs because they rarely recur across images; enables multimodal LMs and embedding models.
67. **Expect** image–caption dataset (millions of pairs); text encoder + image encoder; cosine similarity (dot product / product of lengths); contrastive learning (maximize matching, minimize mismatching, negative examples, updates each batch); shared vector space enables cross-modal comparison; tasks: zero-shot classification, clustering, search, generation (stable diffusion); OpenCLIP components (CLIPTokenizerFast, CLIPProcessor, CLIPModel; openai/clip-vit-base-patch32); code outputs (input_ids with <|startoftext|>/<|endoftext|>, [1,512] text embedding, [1,3,224,224] pixels, [1,512] image embedding, 0.33 similarity); sentence-transformers (clip-ViT-B-32, encode, util.cos_sim).
68. **Expect** from-scratch infeasibility (billions of images/text/pairs + compute); BLIP-2 = frozen ViT + frozen LLM + trainable Q-Former; Q-Former modules (Image Transformer for frozen ViT features, Text Transformer for LLM) sharing attention; step 1 three joint objectives (contrastive, image-text matching, image-grounded text generation); step 2 soft visual prompts through linear projection; LLaVA (visual instruction tuning) and Idefics 2 (Mistral 7B-based) follow the same CLIP-encoder + LLM bridging idea.
69. **Expect** processor ≈ tokenizer for images+text; images resized to 224×224 squares (520×492 example → [1,3,224,224]); distortion warning for wide/tall images; GPT2Tokenizer (vocab 50265, special tokens <s>, </s>, <pad>); Ġ = space (code point 32 → 288); underscore marks word start ('Her', '_vocal', 'ization'); loading Salesforce/blip2-opt-2.7b with float16 and GPU/cuda device.
70. **Expect** captioning flow (image → pixel values → BLIP-2 → soft visual prompts → caption); outputs ("an orange supercar driving on the road at sunset", "a black and white ink drawing of a bat"); domain limitation (public training data); VQA prompt "Question: Write down what you see in this picture. Answer:" → "A sports car driving on the road at sunset"; chat prompt with history → "$1,000,000"; ipywidgets chatbot (memory list, template "Question: {} Answer: {}.", max_new_tokens=100, split on "Question", VBox layout with output + text input).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely ViT patching details, CLIP contrastive learning, BLIP-2 two-stage training, or preprocessing/Ġ details).
- **<70%**: Re-read the chapter and study notes, then retake Sections A and B before attempting C and D again.
