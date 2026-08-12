# 📘 Chapter 9 Line-by-Line: Multimodal Large Language Models
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 9
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "When you think about large language models (LLMs), multimodality might not be the first thing that comes to mind. After all, they are language models! But we can quickly see that models can be much more useful if they're able to handle types of data other than text. It's very useful, for example, if a language model is able to glance at a picture and answer questions about it. A model that is able to handle text and images (each of which is called a modality) is said to be multimodal, as we can see in Figure 9-1."
   - **Plain English:** LLMs become far more useful when they can handle other data types (like images) in addition to text; a model handling text + images is multimodal.
   - **Word meanings:** modality = a type/data domain of input or output; glance = take a quick look.
   - **Technical terms:** modality; multimodal; Figure 9-1.

2. **Quote (Figure 9-1 caption):** "Models that are able to deal with different types (or modalities) of data, such as images, audio, video, or sensors, are said to be multimodal. It's possible for a model to accept a modality as input yet not be able to generate in that modality."
   - **Plain English:** Multimodal models handle many data types; accepting an input modality doesn't mean the model can generate in that modality.
   - **Word meanings:** sensors = devices that detect physical input; generate = produce output.
   - **Technical terms:** modalities; input vs output modality asymmetry.

3. **Quote:** "We have seen all manner of emerging behaviors rising from LLMs, from generalization capabilities and reasoning to arithmetic and linguistics. As models grow larger and smarter, so do their skill sets. The ability to receive and reason with multimodal input might further increase and help emerge capabilities that were previously locked."
   - **Plain English:** LLMs keep gaining skills (reasoning, arithmetic, language) as they scale; multimodal input may unlock even more capabilities.
   - **Word meanings:** emerging = newly appearing; arithmetic = math; linguistics = language study.
   - **Technical terms:** emergent abilities; multimodal input; reasoning. (Footnote: Jason Wei et al., "Emergent abilities of large language models," arXiv:2206.07682, 2022.)

4. **Quote:** "In practice, language does not solely live in a vacuum. As an example, your body language, facial expressions, intonation, etc. are all methods of communication that enhance the spoken word. The same thing applies to LLMs; if we can enable them to reason about multimodal information, their capabilities might increase and we become able to deploy them to solve new kinds of problems."
   - **Plain English:** Communication uses more than words (gestures, tone); giving LLMs multimodal reasoning lets them solve new problems.
   - **Word meanings:** solely = only; intonation = pitch/tone of voice; deploy = put into use.
   - **Technical terms:** multimodal information; multimodal reasoning.

5. **Quote:** "In this chapter, we will explore a number of different LLMs that have multimodal capabilities and what that means for practical use cases. We will start by exploring how images are converted to numerical representations using an adaptation of the original Transformer technique. Then, we will show how LLMs can be extended to include vision tasks using this Transformer."
   - **Plain English:** The chapter explores multimodal LLMs, starting with converting images to numbers via a Transformer adaptation, then extending LLMs to vision tasks.
   - **Technical terms:** numerical representations; Transformer; vision tasks.

## Transformers for Vision

6. **Quote:** "Throughout the chapters of this book, we have seen the success of using Transformer-based models for a variety of language modeling tasks, from classification and clustering to search and generative modeling. So it might not be surprising that researchers have been looking at a way to generalize some of the Transformer's success to the field of computer vision."
   - **Plain English:** Transformers succeeded across NLP tasks, so researchers extended the same idea to computer vision.
   - **Technical terms:** Transformer-based models; computer vision; classification/clustering/search/generative modeling.

7. **Quote:** "The method they came up with is called the Vision Transformer (ViT), which has been shown to do tremendously well on image recognition tasks compared to the previously default convolutional neural networks (CNNs)."
   - **Plain English:** ViT (a Transformer for images) outperforms the older default CNNs on image recognition.
   - **Technical terms:** Vision Transformer (ViT); convolutional neural networks (CNNs); image recognition. (Footnote: Alexey Dosovitskiy et al., "An image is worth 16x16 words," arXiv:2010.11929, 2020.)

8. **Quote:** "Like the original Transformer, ViT is used to transform unstructured data, an image, into representations that can be used for a variety of tasks, like classification. ViT relies on an important component of the Transformer architecture, namely the encoder. As we saw in Chapter 1, the encoder is responsible for converting textual input into numerical representations before being passed to the decoder. However, before the encoder can perform its duties, the textual input needs to be tokenized first."
   - **Plain English:** ViT uses the Transformer encoder to turn images into representations; text must be tokenized before the encoder works.
   - **Word meanings:** duties = responsibilities/jobs.
   - **Technical terms:** encoder; decoder; tokenization; numerical representations.

9. **Quote:** "Since an image does not consist of words this tokenization process cannot be used for visual data. Instead, the authors of ViT came up with a method for tokenizing images into 'words,' which allowed them to use the original encoder structure."
   - **Plain English:** Images aren't words, so ViT invents image "tokens" so the standard encoder can process them.
   - **Technical terms:** image tokenization; patches; encoder structure.

10. **Quote:** "Imagine that you have an image of a cat. This image is represented by a number of pixels, let's say 512 × 512 pixels. Each individual pixel does not convey much information but when you combine patches of pixels, you slowly start to see more information."
    - **Plain English:** A single pixel carries little meaning, but combining pixels into patches reveals structure.
    - **Word meanings:** convey = communicate/carry.
    - **Technical terms:** pixels; patches.

11. **Quote:** "ViT uses a principle much like that. Instead of splitting up text into tokens, it converts the original image into patches of images. In other words, it cuts the image into a number of pieces horizontally and vertically as illustrated in Figure 9-4."
    - **Plain English:** ViT "tokenizes" an image by cutting it into patches (grid pieces).
    - **Technical terms:** patches; horizontal/vertical cuts; Figure 9-4.

12. **Quote:** "Just like we are converting text into tokens of text, we are converting an image into patches of images. The flattened input of image patches can be thought of as the tokens in a piece of text. However, unlike tokens, we cannot just assign each patch with an ID since these patches will rarely be found in other images, unlike the vocabulary of a text."
    - **Plain English:** Patches act like tokens, but can't be given vocabulary IDs because patches don't recur across images like words do.
    - **Word meanings:** flattened = stretched into one dimension; recur = repeat.
    - **Technical terms:** flattened patches; vocabulary; token IDs.

13. **Quote:** "Instead, the patches are linearly embedded to create numerical representations, namely embeddings. These can then be used as the input of a Transformer model. That way, the patches of images are treated the same way as tokens. The full process is illustrated in Figure 9-5."
    - **Plain English:** Patches are linearly embedded into numerical embeddings, then fed to the Transformer like token embeddings.
    - **Technical terms:** linear embedding; embeddings; Transformer input; Figure 9-5.

14. **Quote:** "For illustrative purposes, the images in the examples were patched into 3 × 3 patches but the original implementation used 16 × 16 patches. After all, the paper is called 'An Image is Worth 16x16 Words.'"
    - **Plain English:** Examples draw 3×3 grids, but the real ViT uses 16×16 patches (per the paper's title).
    - **Technical terms:** 16×16 patches; "An Image is Worth 16x16 Words."

15. **Quote:** "What is so interesting about this approach is that the moment the embeddings are passed to the encoder, they are treated as if they were textual tokens. From that point forward, there is no difference in how a text or image trains."
    - **Plain English:** After embedding, image patches are handled identically to text tokens during training.
    - **Technical terms:** patch embeddings; encoder; unified training.

16. **Quote:** "Due to these similarities, the ViT is often used to make all kinds of language models multimodal. One of the most straightforward ways to use it is during the training of embedding models."
    - **Plain English:** ViT makes language models multimodal, especially by powering multimodal embedding models.
    - **Technical terms:** ViT; multimodal language models; embedding models.

## Multimodal Embedding Models

17. **Quote:** "In previous chapters, we used embedding models to capture the semantic content of textual representations, such as papers and documents. We saw that we could use these embeddings or numerical representations to find similar documents, apply classification tasks, and even perform topic modeling."
    - **Plain English:** Text embedding models capture meaning and support similarity search, classification, and topic modeling.
    - **Technical terms:** embedding models; semantic content; topic modeling; classification.

18. **Quote:** "As we have seen many times before, embeddings often are an important driver behind LLM applications. They are an efficient method for capturing large-scale information and searching for the needle in the haystack of information."
    - **Plain English:** Embeddings efficiently encode large amounts of information for search.
    - **Word meanings:** needle in a haystack = finding one tiny thing among lots.
    - **Technical terms:** embeddings; large-scale search.

19. **Quote:** "That said, we have looked at text-only embedding models thus far, which focus on generating embeddings for textual representations. Although embedding models exist for solely embedding imagery, we will look at embedding models that can capture both textual as well as visual representations. We illustrate this in Figure 9-6."
    - **Plain English:** Beyond text-only (or image-only) embedders, the chapter covers models embedding both text and images.
    - **Technical terms:** text-only embedding models; multimodal embedding models; Figure 9-6.

20. **Quote:** "An advantage is that this allows for comparing multimodal representations since the resulting embeddings lie in the same vector space (Figure 9-7). For instance, using such a multimodal embedding model, we can find images based on input text. What images would we find if we search for images similar to 'pictures of a puppy'? Vice versa would also be possible. Which documents are best related to this question?"
    - **Plain English:** Shared vector space lets us compare text and images, e.g., find images matching "pictures of a puppy" or documents related to an image.
    - **Word meanings:** vice versa = the reverse also applies.
    - **Technical terms:** vector space; multimodal comparison; cross-modal retrieval.

21. **Quote:** "There are a number of multimodal embedding models, but the most well-known and currently most-used model is Contrastive Language-Image Pre-training (CLIP)."
    - **Plain English:** CLIP is the most popular multimodal embedding model.
    - **Technical terms:** CLIP (Contrastive Language-Image Pre-training).

## CLIP: Connecting Text and Images

22. **Quote:** "CLIP is an embedding model that can compute embeddings of both images and texts. The resulting embeddings lie in the same vector space, which means that the embeddings of images can be compared with the embeddings of text. This comparison capability makes CLIP, and similar models, usable for tasks such as: Zero-shot classification — compare the embedding of an image with that of the description of its possible classes to find which class is most similar. Clustering — cluster both images and a collection of keywords to find which keywords belong to which sets of images. Search — across billions of texts or images, we can quickly find what relates to an input text or image. Generation — use multimodal embeddings to drive the generation of images (e.g., stable diffusion)."
    - **Plain English:** CLIP's shared vector space powers zero-shot classification, clustering, search, and image generation.
    - **Technical terms:** zero-shot classification; clustering; search; stable diffusion; embeddings. (Footnotes: Radford et al., CLIP, ICML 2021; Rombach et al., stable diffusion, CVPR 2022.)

## How Can CLIP Generate Multimodal Embeddings?

23. **Quote:** "The procedure of CLIP is actually quite straightforward. Imagine that you have a dataset with millions of images alongside captions as we illustrate in Figure 9-8. This dataset can be used to create two representations for each pair, the image and its caption. To do so, CLIP uses a text encoder to embed text and an image encoder to embed images. As is shown in Figure 9-9, the result is an embedding for both the image and its corresponding caption."
    - **Plain English:** CLIP trains on millions of image–caption pairs, embedding each image and caption with separate encoders.
    - **Technical terms:** image–caption pairs; text encoder; image encoder; embeddings.

24. **Quote:** "The pair of embeddings that are generated are compared through cosine similarity. As we saw in Chapter 4, cosine similarity is the cosine of the angle between vectors, which is calculated through the dot product of the embeddings and divided by the product of their lengths."
    - **Plain English:** Image and text embeddings are compared with cosine similarity (dot product / product of lengths).
    - **Technical terms:** cosine similarity; dot product; vector angle.

25. **Quote:** "When we start training, the similarity between the image embedding and text embedding will be low as they are not yet optimized to be within the same vector space. During training, we optimize for the similarity between the embeddings and want to maximize them for similar image/caption pairs and minimize them for dissimilar image/caption pairs (Figure 9-10). After calculating their similarity, the model is updated and the process starts again with new batches of data and updated representations (Figure 9-11). This method is called contrastive learning, and we will go in depth into its inner workings in Chapter 10 where we will create our own embedding model."
    - **Plain English:** CLIP maximizes similarity for matching image–caption pairs and minimizes it for mismatches — that's contrastive learning, detailed in Chapter 10.
    - **Word meanings:** optimize = tune toward best values; dissimilar = different/unrelated.
    - **Technical terms:** contrastive learning; cosine similarity optimization; batches.

26. **Quote:** "Eventually, we expect the embedding of an image of a cat would be similar to the embedding of the phrase 'a picture of a cat.' As we will see in Chapter 10, to make sure the representations are as accurate as possible, negative examples of images and captions that are not related should also be included in the training process. Modeling similarity is not only knowing what makes things similar to one another, but also what makes them different and dissimilar."
    - **Plain English:** A cat image's embedding should sit near "a picture of a cat"; negative (unrelated) examples teach the model dissimilarity too.
    - **Technical terms:** negative examples; similarity/dissimilarity modeling; contrastive pairs.

## OpenCLIP

27. **Quote:** "For our next example, we are going to be using models from the open source variant of CLIP, namely OpenCLIP. Using OpenCLIP, or any CLIP model, boils down to two things: processing the textual and image inputs before passing them to the main model."
    - **Plain English:** OpenCLIP is the open-source CLIP; usage = preprocessing text and images before the main model.
    - **Word meanings:** boils down to = simplifies to.
    - **Technical terms:** OpenCLIP; preprocessing; main model.

28. **Quote:** "Before doing so, let's take a look at a small example where we will be using one of the images we have seen before, namely, an AI-generated image (through stable diffusion) of a puppy playing in the snow."
    - **Plain English:** The example uses an AI-generated image of a puppy playing in the snow (Figure 9-12).
    - **Technical terms:** stable diffusion; AI-generated image.

29. **Quote:** "Since we have a caption for this image, we can use OpenCLIP to generate embeddings for both. To do so, we load in three models: A tokenizer for tokenizing the textual input; A preprocessor to preprocess and resize the image; The main model that converts the previous outputs to embeddings."
    - **Plain English:** OpenCLIP needs three components: tokenizer (text), preprocessor (images), and the main embedding model.
    - **Technical terms:** tokenizer; preprocessor; embedding model.

### Code: Loading OpenCLIP
30. **Code Listing:** `from transformers import CLIPTokenizerFast, CLIPProcessor, CLIPModel`, `model_id = "openai/clip-vit-base-patch32"`, `clip_tokenizer = CLIPTokenizerFast.from_pretrained(model_id)`, `clip_processor = CLIPProcessor.from_pretrained(model_id)`, `model = CLIPModel.from_pretrained(model_id)`.
    - **Plain English:** Loads the CLIP tokenizer, processor, and model from Hugging Face using model ID `openai/clip-vit-base-patch32`.
    - **Technical terms:** `CLIPTokenizerFast`; `CLIPProcessor`; `CLIPModel`; `from_pretrained`.

31. **Code Listing:** Tokenize the caption: `inputs = clip_tokenizer(caption, return_tensors="pt")` → `{'input_ids': tensor([[49406, 320, 6829, 1629, 530, 518, 2583, 49407]]), 'attention_mask': tensor([[1, 1, 1, 1, 1, 1, 1, 1]])}`.
    - **Plain English:** Tokenizing returns `input_ids` and an all-ones `attention_mask`.
    - **Technical terms:** `input_ids`; `attention_mask`; `return_tensors="pt"`.

32. **Code Listing:** `clip_tokenizer.convert_ids_to_tokens(inputs["input_ids"][0])` → `['<|startoftext|>', 'a</w>', 'puppy</w>', 'playing</w>', 'in</w>', 'the</w>', 'snow</w>', '<|endoftext|>']`.
    - **Plain English:** The tokens include start/end markers and word tokens with `</w>` boundaries.
    - **Technical terms:** `<|startoftext|>`; `<|endoftext|>`; `</w>` word boundary.

33. **Quote:** "As we often have seen before, the text is split up into tokens. Additionally, we now also see that the start and end of the text is indicated to separate it from a potential image embedding. You might also notice that the [CLS] token is missing. In CLIP, the [CLS] token is actually used to represent the image embedding."
    - **Plain English:** CLIP marks text boundaries, and uses the [CLS] token (missing here) to represent the image embedding.
    - **Technical terms:** [CLS] token; image embedding; special tokens.

34. **Code Listing:** Text embedding: `text_embedding = model.get_text_features(**inputs)`, `text_embedding.shape` → `torch.Size([1, 512])`.
    - **Plain English:** The caption becomes a 512-value embedding.
    - **Technical terms:** `get_text_features`; shape `[1, 512]`.

35. **Code Listing:** Image preprocessing: `processed_image = clip_processor(text=None, images=image, return_tensors="pt")["pixel_values"]`, `processed_image.shape` → `torch.Size([1, 3, 224, 224])`.
    - **Plain English:** The processor shrinks the 512×512 puppy image to 224×224 with RGB channels.
    - **Technical terms:** `pixel_values`; shape `[1, 3, 224, 224]`; resize.

36. **Code Listing:** Visualization prep: `import torch, numpy as np, matplotlib.pyplot as plt`, `img = processed_image.squeeze(0)`, `img = img.permute(*torch.arange(img.ndim - 1, -1, -1))`, `img = np.einsum("ijk->jik", img)`, `plt.imshow(img)`, `plt.axis("off")`.
    - **Plain English:** Rearranges tensor axes so the processed image can be displayed.
    - **Technical terms:** `squeeze`; `permute`; `einsum`; `plt.imshow`.

37. **Code Listing:** Image embedding: `image_embedding = model.get_image_features(processed_image)`, `image_embedding.shape` → `torch.Size([1, 512])`.
    - **Plain English:** The image embedding has the same 512-dimension shape as the text embedding — required for comparison.
    - **Technical terms:** `get_image_features`; shared embedding shape.

38. **Code Listing:** Similarity: normalize both (`/= norm(dim=-1, keepdim=True)`), convert to NumPy, then `score = np.dot(text_embedding, image_embedding.T)` → `array([[0.33149648]])`.
    - **Plain English:** Normalized dot product yields a similarity score of about 0.33.
    - **Technical terms:** L2 normalization; dot product; cosine similarity.

39. **Quote:** "We get a similarity score of 0.33, which is difficult to interpret considering we don't know what the model considers a low versus a high similarity score. Instead, let's extend the example with more images and captions as illustrated in Figure 9-14. It seems that a score of 0.33 is indeed high considering the similarities with other images are quite a bit lower."
    - **Plain English:** 0.33 is hard to interpret alone, but against a 3×3 similarity matrix it turns out to be high.
    - **Technical terms:** similarity matrix; relative interpretation.

## Using sentence-transformers to Load CLIP

40. **Quote:** "sentence-transformers implements a few CLIP-based models that make it much easier to create embeddings. It only takes a few lines of code."
    - **Plain English:** sentence-transformers wraps CLIP for easy embedding generation.
    - **Technical terms:** sentence-transformers; CLIP-based models.

41. **Code Listing:** `from sentence_transformers import SentenceTransformer, util`, `model = SentenceTransformer("clip-ViT-B-32")`, `image_embeddings = model.encode(images)`, `text_embeddings = model.encode(captions)`, `sim_matrix = util.cos_sim(image_embeddings, text_embeddings)`.
    - **Plain English:** Loads `clip-ViT-B-32` and computes a full image–caption cosine-similarity matrix.
    - **Technical terms:** `SentenceTransformer`; `encode`; `util.cos_sim`; similarity matrix.

## Making Text Generation Models Multimodal

42. **Quote:** "Traditionally, text generation models have been, as you might expect, models that interpret textual representations. Models like Llama 2 and ChatGPT excel at reasoning about textual information and responding with natural language. They are, however, limited to the modality they were trained in, namely text. As we have seen before with multimodal embedding models, the addition of vision can enhance the capabilities of a model."
    - **Plain English:** Text generators (Llama 2, ChatGPT) excel at text but are limited to text; adding vision enhances them.
    - **Word meanings:** excel = do extremely well; enhance = improve.
    - **Technical terms:** text generation models; modality limitation; vision enhancement.

43. **Quote:** "In the case of text generation models, we would like it to reason about certain input images. For example, we could give it an image of a pizza and ask it what ingredients it contains. You could show it a picture of the Eiffel Tower and ask when it was built or where it is located. This conversational ability is further illustrated in Figure 9-15."
    - **Plain English:** We want LLMs to reason about images, e.g., pizza ingredients or the Eiffel Tower's history/location.
    - **Technical terms:** multimodal text generation; visual reasoning; Figure 9-15.

44. **Quote:** "To bridge the gap between these two domains, attempts have been made to introduce a form of multimodality to existing models. One such method is called BLIP-2: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation. BLIP-2 is an easy-to-use and modular technique that allows for introducing vision capabilities to existing language models."
    - **Plain English:** BLIP-2 is an easy, modular way to add vision to existing LLMs.
    - **Technical terms:** BLIP-2; vision-language understanding; modular technique.

## BLIP-2: Bridging the Modality Gap

45. **Quote:** "Creating a multimodal language model from scratch requires significant computing power and data. We would have to use billions of images, text, and image-text pairs to create such a model. As you can imagine, this is not easily feasible!"
    - **Plain English:** Building a multimodal LLM from scratch needs billions of images/text/pairs and huge compute — not practical.
    - **Word meanings:** feasible = possible/practical.
    - **Technical terms:** training from scratch; multimodal data.

46. **Quote:** "Instead of building the architecture from scratch, BLIP-2 bridges the vision-language gap by building a bridge, named the Querying Transformer (Q-Former), that connects a pretrained image encoder and a pretrained LLM. By leveraging pretrained models, BLIP-2 only needs to train the bridge without needing to train the image encoder and LLM from scratch. It makes great use of the technology and models that are already out there!"
    - **Plain English:** BLIP-2 connects a frozen image encoder and frozen LLM via a trainable Q-Former bridge; only the bridge is trained.
    - **Word meanings:** leveraging = making use of.
    - **Technical terms:** Q-Former; frozen pretrained models; bridge. (Footnote: Junnan Li et al., "BLIP-2: Bootstrapping language-image pretraining with frozen image encoders and large language models," ICML, PMLR, 2023.)

47. **Quote:** "To connect the two pretrained models, the Q-Former mimics their architectures. It has two modules that share their attention layers: An Image Transformer to interact with the frozen Vision Transformer for feature extraction; A Text Transformer that can interact with the LLM."
    - **Plain English:** Q-Former has an Image Transformer (for the frozen ViT) and a Text Transformer (for the LLM), sharing attention layers.
    - **Technical terms:** Q-Former modules; Image Transformer; Text Transformer; shared attention; feature extraction.

48. **Quote:** "The Q-Former is trained in two stages, one for each modality. In step 1, image-document pairs are used to train the Q-Former to represent both images and text. These pairs are generally captions of images, as we have seen before when training CLIP. The images are fed to the frozen ViT to extract vision embeddings. These embeddings are used as the input of Q-Former's ViT. The captions are used as the input of Q-Former's Text Transformer."
    - **Plain English:** Step 1 trains the Q-Former on image–caption pairs, feeding frozen-ViT vision embeddings and caption text into its two transformers.
    - **Technical terms:** two-stage training; frozen ViT; vision embeddings; image-document pairs.

49. **Quote:** "With these inputs, the Q-Former is then trained on three tasks: Image-text contrastive learning — this task attempts to align pairs of image and text embeddings such that they maximize their mutual information. Image-text matching — a classification task to predict whether an image and text pair is positive (matched) or negative (unmatched). Image-grounded text generation — trains the model to generate text based on information extracted from the input image."
    - **Plain English:** The Q-Former trains on three joint objectives: contrastive alignment, matched/unmatched classification, and image-grounded text generation.
    - **Technical terms:** image-text contrastive learning; mutual information; image-text matching; image-grounded text generation.

50. **Quote:** "These three objectives are jointly optimized to improve the visual representations that are extracted from the frozen ViT. In a way, we are trying to inject textual information into the embeddings of the frozen ViT so that we can use them in the LLM."
    - **Plain English:** Joint optimization injects textual info into the frozen ViT's embeddings for later use by the LLM.
    - **Technical terms:** jointly optimized objectives; textual information injection; frozen ViT embeddings.

51. **Quote:** "In step 2, the learnable embeddings derived from step 1 now contain visual information in the same dimensional space as the corresponding textual information. The learnable embeddings are then passed to the LLM. In a way, these embeddings serve as soft visual prompts that condition the LLM on the visual representations that were extracted by the Q-Former."
    - **Plain English:** Step-2 embeddings carry visual info in text's dimensional space and act as soft visual prompts conditioning the LLM.
    - **Technical terms:** learnable embeddings; soft visual prompts; conditioning.

52. **Quote:** "There is also a fully connected linear layer in between them to make sure that the learnable embeddings have the same shape as the LLM expects. This second step of converting vision to language is represented in Figure 9-19."
    - **Plain English:** A linear projection layer reshapes the embeddings to match the LLM's expected input shape.
    - **Technical terms:** fully connected linear layer; projection layer; shape matching.

53. **Quote:** "When we put these steps together, they make it possible for the Q-Former to learn visual and textual representations in the same dimensional space, which can be used as a soft prompt to the LLM. As a result, the LLM will be given information about the image in a similar manner to the context you would provide an LLM when prompting. The full in-depth process is illustrated in Figure 9-20."
    - **Plain English:** Together the steps let the Q-Former learn shared representations used as a soft prompt, like giving the LLM context.
    - **Technical terms:** shared dimensional space; soft prompt; prompting context; Figure 9-20.

54. **Quote:** "Since BLIP-2, many other visual LLMs have been released that have similar processes, like LLaVA, a framework for making textual LLMs multimodal or Idefics 2, an efficient visual LLM based on the Mistral 7B LLM. Both visual LLMs, although having different architectures, connect pretrained CLIP-like visual encoders with textual LLMs. The goal of these architectures is to project visual features from the input images to language embeddings such that they can be used as the input for an LLM. Similar to the Q-Former, they attempt to bridge the gap between images and text."
    - **Plain English:** LLaVA and Idefics 2 (Mistral 7B-based) also connect CLIP-like visual encoders to textual LLMs, projecting visual features to language embeddings.
    - **Technical terms:** LLaVA; visual instruction tuning; Idefics 2; Mistral 7B; CLIP-like visual encoders; projection of visual features. (Footnotes: Liu et al., "Visual instruction tuning," NeurIPS 36 (2024); Laurençon et al., "What matters when building vision-language models?," arXiv:2405.02246, 2024.)

## Preprocessing Multimodal Inputs

55. **Quote:** "Now that we know how BLIP-2 is created, there are a number of interesting use cases for such a model, not limited to captioning images, answering visual questions, and even performing prompting."
    - **Plain English:** BLIP-2 supports image captioning, visual question answering, and prompting.
    - **Technical terms:** image captioning; visual question answering; prompting.

56. **Quote:** "We loaded two components that make up our full pipeline: a processor and a model. The processor can be compared to the tokenizer of language models. It converts unstructured input, such as images and text, to representations that the model generally expects."
    - **Plain English:** The BLIP-2 processor (like an LLM tokenizer) converts raw images and text into model-ready representations.
    - **Technical terms:** processor; unstructured input; model-ready representations.

### Code: Loading BLIP-2
57. **Code Listing:** `from transformers import AutoProcessor, Blip2ForConditionalGeneration`, `import torch`, `blip_processor = AutoProcessor.from_pretrained("Salesforce/blip2-opt-2.7b")`, `model = Blip2ForConditionalGeneration.from_pretrained("Salesforce/blip2-opt-2.7b", torch_dtype=torch.float16)`, `device = "cuda" if torch.cuda.is_available() else "cpu"`, `model.to(device)`.
    - **Plain English:** Loads the BLIP-2 processor and model (in float16) and moves it to GPU if available. `model.vision_model` and `model.language_model` reveal the ViT and generative model.
    - **Technical terms:** `AutoProcessor`; `Blip2ForConditionalGeneration`; `torch_dtype=torch.float16`; GPU/CUDA.

58. **Code Listing:** Preprocessing images: `car_path = ".../chapter09/images/car.png"`, `image = Image.open(urlopen(car_path)).convert("RGB")`, `inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)`, `inputs["pixel_values"].shape` → `torch.Size([1, 3, 224, 224])`.
    - **Plain English:** A 520×492 supercar image becomes a 224×224 square; other shapes are resized to squares too, so wide/tall images may be distorted.
    - **Technical terms:** pixel_values; resizing; 224×224; distortion.

59. **Quote:** "The result is a 224 × 224-sized image. Quite a bit smaller than we initially had! This also means that all the original different shapes of the image will be processed into squares. So be careful inputting very wide or tall images as they might get distorted."
    - **Plain English:** All images are squashed into 224×224 squares; very wide/tall images may look distorted.
    - **Word meanings:** distorted = stretched/warped.
    - **Technical terms:** square resizing; aspect-ratio loss.

60. **Code Listing:** Preprocessing text: `blip_processor.tokenizer` → `GPT2TokenizerFast(name_or_path='Salesforce/blip2-opt-2.7b', vocab_size=50265, ..., special_tokens={'bos_token': '</s>', 'eos_token': '</s>', 'unk_token': '</s>', 'pad_token': '<pad>'})`.
    - **Plain English:** BLIP-2 uses a GPT2Tokenizer with vocab size 50265 and special tokens.
    - **Technical terms:** GPT2Tokenizer; vocab_size; bos/eos/unk/pad tokens.

61. **Quote:** "The BLIP-2 model here uses a GPT2Tokenizer. As we explored in Chapter 2, how tokenizers deal with input text can differ greatly."
    - **Plain English:** Different models use different tokenizers, and tokenizers handle text very differently.
    - **Technical terms:** tokenizer differences; GPT2Tokenizer.

62. **Code Listing:** `text = "Her vocalization was remarkably melodic"`, `token_ids = blip_processor(image, text=text, return_tensors="pt")`, `tokens = blip_processor.tokenizer.convert_ids_to_tokens(token_ids)` → `['</s>', 'Her', 'Ġvocal', 'ization', 'Ġwas', 'Ġremarkably', 'Ġmel', 'odic']`.
    - **Plain English:** Tokenizing shows tokens starting with Ġ, e.g., 'Her', 'Ġvocal', 'ization'.
    - **Technical terms:** convert_ids_to_tokens; Ġ symbol; subword tokens.

63. **Quote:** "When we inspect the tokens, you might notice a strange symbol at the beginning of some tokens, namely, the Ġ symbol. This is actually supposed to be a space. However, an internal function takes characters in certain code points and moves them up by 256 to make them printable. As a result, the space (code point 32) becomes Ġ (code point 288)."
    - **Plain English:** Ġ is really a space character; the tokenizer shifts space's code point up by 256 (32 → 288) to make it printable.
    - **Technical terms:** code point; printable characters; Ġ encoding.

64. **Code Listing:** `tokens = [token.replace("Ġ", "_") for token in tokens]` → `['</s>', 'Her', '_vocal', 'ization', '_was', '_remarkably', '_mel', 'odic']`.
    - **Plain English:** Replacing Ġ with underscores makes the word-start markers visible.
    - **Technical terms:** string replace; word-start marker; multi-token words.

65. **Quote:** "The output shows that the underscore indicates the beginning of a word. That way, words that are made up of multiple tokens can be recognized."
    - **Plain English:** The underscore (originally Ġ/space) marks a word's start, so multi-token words (e.g., `_vocal` + `ization`) are identifiable.
    - **Technical terms:** subword tokens; word boundaries.

## Use Case 1: Image Captioning

66. **Quote:** "The most straightforward usage of a model like BLIP-2 is to create captions of images that you have in your data. You might be a store that wants to create descriptions of its clothing or perhaps you are a photographer that does not have the time to manually label the 1,000+ pictures of a wedding."
    - **Plain English:** Image captioning automates descriptions (store product photos, wedding photos).
    - **Word meanings:** straightforward = simple/direct.
    - **Technical terms:** image captioning.

67. **Quote:** "The process of captioning an image closely follows the processing. An image is converted to pixel values that the model can read. These pixel values are passed to BLIP-2 to be converted into soft visual prompts that the LLM can use to decide on a proper caption."
    - **Plain English:** Captioning = image → pixel values → BLIP-2 → soft visual prompts → LLM writes the caption.
    - **Technical terms:** pixel values; soft visual prompts; caption generation.

68. **Code Listing:** `inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)`, `generated_ids = model.generate(**inputs, max_new_tokens=20)`, `generated_text = blip_processor.batch_decode(generated_ids, skip_special_tokens=True)`, `generated_text = generated_text[0].strip()` → `'an orange supercar driving on the road at sunset'`.
    - **Plain English:** BLIP-2 captions the supercar image as "an orange supercar driving on the road at sunset."
    - **Technical terms:** `model.generate`; `batch_decode`; `skip_special_tokens`; `max_new_tokens`.

69. **Quote:** "Image captioning is a great way to get to learn this model before stepping into more complex use cases. Try it out with a few images yourself and see where it performs well and where it performs poorly. Domain-specific images, like pictures of specific cartoon characters or imaginary creations, may fail as the model was trained on largely public data."
    - **Plain English:** Captioning is a good first test; domain-specific images (cartoon characters, imaginary creations) may fail since training data is mostly public.
    - **Technical terms:** domain-specific images; public data; model limitations.

70. **Quote:** "Let's end this use case with a fun example, namely an image from the Rorschach test, which is illustrated in Figure 9-21. It is part of an old psychological experiment that tests the individual's perception of inkblots. What someone sees in such an inkblot supposedly tells you something about a person's personality characteristics. It is quite a subjective test but that just makes it more fun!"
    - **Plain English:** The Rorschach test uses inkblots where what you see supposedly reveals personality — a subjective, fun example.
    - **Word meanings:** inkblot = a blot of ink used as a test image; subjective = based on personal opinion.
    - **Technical terms:** Rorschach test; perception. (Footnote: Roy Schafer, *Psychoanalytic Interpretation in Rorschach Testing*, 1954.)

71. **Code Listing:** `url = "https://upload.wikimedia.org/wikipedia/commons/7/70/Rorschach_blot_01.jpg"`, `image = Image.open(urlopen(url)).convert("RGB")`, `inputs = blip_processor(image, return_tensors="pt").to(device, torch.float16)`, `generated_ids = model.generate(**inputs, max_new_tokens=20)`, `generated_text = ...` → `'a black and white ink drawing of a bat'`.
    - **Plain English:** BLIP-2 captions the Rorschach inkblot as "a black and white ink drawing of a bat."
    - **Technical terms:** Rorschach; caption generation.

72. **Quote:** "I can definitely see how the model would caption this image using such a description. Since this is a Rorschach test, what do you think it says about the model?"
    - **Plain English:** The caption is plausible; the authors playfully ask what the inkblot "says" about the model.
    - **Word meanings:** definitely = certainly.

## Use Case 2: Multimodal Chat-Based Prompting

73. **Quote:** "Although captioning is an important task, we can extend its use case even further. In the previous example, we showed going from one modality, vision (image), to another, text (caption). Instead of following this linear structure, we can try to present both modalities simultaneously by performing what is called visual question answering. In this particular use case, we give the model an image along with a question about that specific image for it to answer. The model needs to process both the image as well as the question at once."
    - **Plain English:** VQA presents an image and a question together; the model processes both simultaneously.
    - **Technical terms:** visual question answering (VQA); dual-modality input.

74. **Code Listing:** `image = Image.open(urlopen(car_path)).convert("RGB")`, `prompt = "Question: Write down what you see in this picture. Answer:"`, `inputs = blip_processor(image, text=prompt, return_tensors="pt").to(device, torch.float16)`, `generated_ids = model.generate(**inputs, max_new_tokens=30)`, ... → `'A sports car driving on the road at sunset'`.
    - **Plain English:** With the "Question: ... Answer:" prompt, BLIP-2 describes the image; without a prompt it would just caption.
    - **Technical terms:** VQA prompt format; image+text inputs.

75. **Quote:** "It correctly describes the image. However, this is a rather simple example since our question is essentially asking the model to create a caption. Instead, we can ask follow-up questions in a chat-based manner. To do so, we can give the model our previous conversation, including its answer to our question. We then ask it a follow-up question."
    - **Plain English:** Chat-based prompting re-feeds the previous Q&A so the model can answer follow-ups.
    - **Technical terms:** chat-based prompting; conversation history.

76. **Code Listing:** `prompt = "Question: Write down what you see in this picture. Answer: A sports car driving on the road at sunset. Question: What would it cost me to drive that car? Answer:"` → `'$1,000,000'`.
    - **Plain English:** Given the prior answer, BLIP-2 answers the cost follow-up with "$1,000,000."
    - **Technical terms:** conversation context; follow-up answering.

77. **Quote:** "$1,000,000 is highly specific! This shows more chat-like behavior from BLIP-2, which allows for some interesting conversations."
    - **Plain English:** The specific answer demonstrates chat-like behavior.
    - **Word meanings:** specific = precise/detailed.

78. **Quote:** "Finally, we can make this process a bit smoother by creating an interactive chatbot using ipywidgets, an extension for Jupyter notebooks that allows us to make interactive buttons, input text, etc."
    - **Plain English:** ipywidgets (a Jupyter extension) builds an interactive BLIP-2 chatbot.
    - **Technical terms:** ipywidgets; Jupyter notebooks; interactive widgets.

79. **Code Listing (chatbot):** `from IPython.display import HTML, display`, `import ipywidgets as widgets`; `def text_eventhandler(*args):` reads `args[0]["new"]` as the question, clears the input, builds the prompt from `memory` (template `"Question: {} Answer: {}."`), calls `blip_processor(image, text=prompt, return_tensors="pt")`, `model.generate(**inputs, max_new_tokens=100)`, decodes, `.split("Question")[0]` trims the answer, appends `(question, generated_text)` to `memory`, and displays USER/BLIP-2 HTML lines. Widgets: `in_text = widgets.Text()`, `in_text.observe(text_eventhandler, "value")`, `output = widgets.Output()`, `memory = []`, `display(widgets.VBox(children=[output, in_text], layout=widgets.Layout(display="inline-flex", flex_flow="column-reverse")))`.
    - **Plain English:** The chatbot keeps a memory list of Q&A pairs, builds each prompt from the conversation, generates answers, and displays them interactively.
    - **Technical terms:** event handler; `observe`; `VBox` layout; `memory` list; prompt template.

80. **Quote:** "It seems that we can continue the conversation and ask a bunch of questions. Using this chat-based approach, we essentially created a chatbot that can reason about images!"
    - **Plain English:** The chat-based approach yields a chatbot that reasons about images.
    - **Technical terms:** multimodal chatbot; image reasoning.

## Summary

81. **Quote:** "In this chapter, we explored various methods for making LLMs multimodal by bridging the gap between textual and visual representations. We started by discussing Transformers for vision, which are models that convert images into numerical representations. This was achieved through the use of image encoders and patch embeddings, which allow the model to process images at various scales."
    - **Plain English:** The chapter covered vision Transformers (image encoders + patch embeddings) that convert images to numbers.
    - **Technical terms:** Transformers for vision; image encoders; patch embeddings.

82. **Quote:** "We then explored the creation of embedding models that can convert both images and text to numerical representations using CLIP. We saw how CLIP uses contrastive learning to align image and text embeddings in a shared space, allowing for tasks like zero-shot classification, clustering, and search. The chapter also introduced OpenCLIP, an open source variant of CLIP that is easy to use for multimodal embedding tasks."
    - **Plain English:** CLIP uses contrastive learning to align image/text embeddings for classification, clustering, search; OpenCLIP is its open-source variant.
    - **Technical terms:** CLIP; contrastive learning; shared embedding space; OpenCLIP.

83. **Quote:** "Finally, we explored how text generation models could be made multimodal and dived into the BLIP-2 model. The core idea of these multimodal text generation models involves projecting visual features from input images to text embeddings that can be used by LLMs. We saw how this model could be used for image captioning and multimodal chat-based prompting, where both modalities are combined to generate responses. Overall, this chapter highlighted the power of multimodality in LLMs and demonstrated its applications in various areas such as image captioning, search, and chat-based prompting."
    - **Plain English:** Multimodal text generation (BLIP-2) projects visual features to language embeddings for captioning and chat; multimodality powers captioning, search, chat.
    - **Technical terms:** visual feature projection; language embeddings; multimodal chat-based prompting; image captioning.

84. **Quote:** "In Part III of the book, we will cover training and fine-tuning techniques. In Chapter 10, we will explore how to create and fine-tune a text embedding model, which is a core technology that drives many language modeling applications. This next chapter serves as an introduction into both training and fine-tuning language models."
    - **Plain English:** Chapter 10 (Part III) covers creating and fine-tuning a text embedding model — an intro to training/fine-tuning LLMs.
    - **Technical terms:** Part III; text embedding model; fine-tuning; contrastive learning deep dive.
