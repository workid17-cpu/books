# Chapter 9 — Line-by-Line Detailed Explanation
**Source:** *An Illustrated Guide to AI Agents*, Chapter 9 "Multi-Modal Understanding"
**Note:** Each numbered item quotes a paragraph/section from the book, then gives (1) a plain-English explanation, (2) word meanings, and (3) technical terms explained. Code listings are paraphrased/annotated; every substantive paragraph is covered.

---

## Section 1: Introduction

1. **"Agentic systems have great potential when developed for real-world applications. These applications, however, don't limit themselves to only text-based information, as is the case with generic LLMs. An agent may need to process a video, process an image to judge a website design, or be able to listen to your spoken request."**
   - **Plain-English:** Real agents need to handle more than text: videos, images, and speech.
   - **Word meanings:** *limit themselves to* = restricted to; *generic* = ordinary, text-only.
   - **Technical terms:** modality = type of input (text, image, audio, video).

2. **"This understanding of different types of information, not just written text, but also images, audio, and videos, is called multi-modal understanding. When you watch a video and read the caption, your brain combines all those signals into one clear understanding."**
   - **Plain-English:** Multi-modal understanding = combining several kinds of signals (like video + caption) into one understanding.
   - **Technical terms:** multi-modal = multiple modalities at once.

3. **"Multi-modal LLMs are designed to do something similar: take in multiple kinds of input (different modalities) and process them together to form a more accurate response. An agent that can only read or write text will miss important parts of what's going on in everyday situations, such as developing the UI of an application or an agentic browser needing to process images and texts of websites."**
   - **Plain-English:** MLLMs combine inputs for better answers; text-only agents miss visual context (UI work, browsing).
   - **Technical terms:** MLLM = multi-modal LLM; agentic browser = an agent that browses and acts on websites.

4. **"Some multi-modal LLMs are not only able to process different modalities but also output them. Imagine a system where the LLM is not only able to output text, but also audio or images. Such a model could use their 'voice' to speak to you or to generate stock images for your website."**
   - **Plain-English:** Some MLLMs also generate audio or images, not just text.
   - **Technical terms:** multi-modal generation = outputting non-text modalities.

5. **"Multi-modal LLMs take an important place in single- and multi-agent systems (MAS). Since they can process all kinds of information, they are often the main 'brain' behind these systems and are more capable of delegating tasks. Models such as GPT-5, Google Gemini 3.0, and the Claude 4.5 family of models are great examples of LLMs that can process more than just text. These models can also process other modalities, such as images and audio, and even reason about them."**
   - **Plain-English:** MLLMs are the "brains" of agent systems; GPT-5, Gemini 3.0, Claude 4.5 are examples.
   - **Word meanings:** *delegating* = assigning tasks.
   - **Technical terms:** MAS = multi-agent system.

## Section 2: What Are Multi-Modal LLMs?

6. **"MLLMs are LLMs that can process and/or generate more types of inputs (modalities) than text. Common modalities are text, image, audio, and video inputs (Figure 9-1)."**
   - **Plain-English:** MLLM = LLM handling more input/output types than text.
   - **Technical terms:** modality = type of input/output.

7. **"Most MLLMs generally focus on encoding information (input) rather than generating information (output). It's a much cheaper operation without necessarily requiring large architectural changes (Figure 9-2). As such, these input modalities are often used to guide the textual generation. Consider it contextual information for a given task."**
   - **Plain-English:** Most MLLMs add inputs (cheaper) and use them to guide text output.
   - **Word meanings:** *architectural changes* = changes to model structure.
   - **Technical terms:** encoding = turning input into embeddings.

8. **"Imagine you have an LLM agent that is in charge of building a website. Wouldn't it then be nice if the LLM agent could actually 'see' the website (Figure 9-3)?"**
   - **Plain-English:** Website-building agents benefit from seeing the site.
   - **Technical terms:** visual input = images of the website.

9. **"There are three main components (with an optional fourth) in creating an MLLM (Figure 9-4): Encoder — encodes a modality (images, audio, etc.) to features (embeddings); Connector — converts encoded features so the LLM can use them; LLM — (pre-trained) model that processes encoded features and generates text; (Optional) Generator — generates modalities aside from text."**
   - **Plain-English:** MLLM = Encoder + Connector + LLM (+ optional Generator).
   - **Word meanings:** *features* = numerical representations.
   - **Technical terms:** embeddings = numeric vectors representing inputs.

10. **"As Figure 9-4 suggests, creating an MLLM generally involves separately training a base LLM and then adding multi-modality to it. Note that this chapter will focus on multi-modal understanding (input modalities) instead of multi-modal generation (output modalities)."**
    - **Plain-English:** Build a text LLM first, then add modalities; chapter focuses on inputs.
    - **Technical terms:** multi-modal understanding = input processing focus.

## Section 3: Encoding Modalities

11. **"Encoding different modalities is generally a process of converting a given input (text, image, audio, etc.) to embeddings (Figure 9-5). This process is a vital part of the 'making LLMs multi-modal' pipeline because the LLM expects embeddings as input (Figure 9-6)."**
    - **Plain-English:** Encoding = converting inputs to embeddings; LLMs need embeddings.
    - **Technical terms:** embedding = numeric representation of an input.

### Text

12. **"A common approach to LLM architecture design is to use a tokenizer to split a given text up into pieces (also tokens), where each token is embedded before being passed to a decoder-only LLM (Figure 9-7)."**
    - **Plain-English:** Text is split into tokens, embedded, then fed to the LLM.
    - **Word meanings:** *split up* = divide.
    - **Technical terms:** tokenizer, token, decoder-only LLM.

13. **"The main encoding process of the text modality involves the tokenizer and the embedding layer (Figure 9-8). The resulting token embeddings are the LLM's main component to process the input information. Aside from the tokens and token IDs, these numeric representations can be seen as the main input of the LLM."**
    - **Plain-English:** Tokenizer + embedding layer = the text encoder; embeddings are the LLM's input.
    - **Technical terms:** token embedding = vector for each token.

14. **"This is also one of the primary places we add other modalities. By encoding other modalities into embeddings, the LLM now has the same type of information to process (Figure 9-9)."**
    - **Plain-English:** Adding other modalities means giving the LLM embeddings, same as text.
    - **Technical terms:** embedding space = shared representation type.

15. **"The tokenizer, together with the embedding layer (token embeddings), can be seen as the text encoder (Figure 9-10)."**
    - **Plain-English:** Text encoder = tokenizer + embedding layer.

16. **"Note that encoding other modalities is insufficient. The structure and dimensions of embeddings generated by one modality (e.g., images) might be completely different from another modality (e.g., audio), as shown in Figure 9-11. To achieve this, we require a model that can project these embeddings onto a common set of characteristics, ensuring they have the same dimensions and similar value distributions."**
    - **Plain-English:** Different encoders produce incompatible embeddings; we need a projector to unify them.
    - **Word meanings:** *insufficient* = not enough; *common set of characteristics* = shared dimensions/distributions.
    - **Technical terms:** projection = mapping embeddings to a shared space.

### Images

17. **"To encode images to embeddings, similar to the process of text encoder, an adaptation of the Transformer is used, namely the Vision-Transformer (ViT)."**
    - **Plain-English:** ViT adapts the Transformer to images.
    - **Technical terms:** ViT = Vision-Transformer.

18. **"The first step of the ViT is to mimic textual tokens. Instead of actually creating tokens, ViT creates patches of images where each patch is considered a 'word' (Figure 9-12)."**
    - **Plain-English:** ViT cuts an image into patches, treating each as a word/token.
    - **Technical terms:** patch = image piece treated as a token.

19. **"After its 'tokenization' process of the input image, it flattens the resulting patches and applies a projection so that a regular Transformer Encoder can process the input as if it were tokens (Figure 9-13). The resulting embeddings can be used in the same way as regular embeddings, for classification, clustering, search, etc."**
    - **Plain-English:** Flatten + project patches so a Transformer Encoder handles them like tokens; embeddings usable downstream.
    - **Technical terms:** flatten, projection, Transformer Encoder.

20. **"Although ViT is a great image encoder, the arguably most used image encoder is actually able to encode both text and images. This method is called Contrastive Language-Image Pre-training (CLIP). CLIP uses ViT together with a regular transformer encoder to embed both images and text. It then uses contrastive learning using labeled similar and dissimilar pairs of images and text to align the embeddings of both modalities (Figure 9-14)."**
    - **Plain-English:** CLIP embeds both text and images; contrastive learning aligns them.
    - **Word meanings:** *align* = bring into agreement.
    - **Technical terms:** contrastive learning = learning from similar/dissimilar pairs.

21. **"As a result, the encoders are updated such that (Figure 9-15): Similar pairs resulting in embeddings with a high similarity; Dissimilar pairs resulting in embeddings with a low similarity. When trained properly, the embeddings of similar text should be close to one another (high similarity) while the embeddings of dissimilar text should be further apart (low similarity)."**
    - **Plain-English:** After training, similar items are close in embedding space, dissimilar items far apart.
    - **Technical terms:** similarity = closeness in embedding space.

22. **"Contrastive learning is an exceptionally powerful technique. By having the model learn what is and isn't similar, it creates accurate semantic representations of the input. As we'll explore later in this chapter, CLIP is a very popular image encoder for MLLMs because the image embeddings have already had 'exposure' to the textual modality."**
    - **Plain-English:** Contrastive learning is powerful; CLIP is popular because its image embeddings "know" text.
    - **Technical terms:** semantic representation = meaning-bearing embedding.

### Audio

23. **"To encode audio into embeddings, many different methods exist that make use of the Transformer architecture, which are in part inspired by the image and text encoders."**
    - **Plain-English:** Audio encoders reuse Transformer ideas from text/image.

24. **"One of the first more successful methods for encoding audio is called wav2vec 2.0. This technique samples overlapping (strided) parts of the audio input as a way to create tokens similar to the text and image examples (Figure 9-16)."**
    - **Plain-English:** wav2vec 2.0 cuts audio into overlapping strided pieces as tokens.
    - **Technical terms:** strided = overlapping sampled windows.

25. **"Those strided parts are then processed using a Convolutional Neural Network (CNN) to create features (embeddings). They are finally passed to a regular Transformer Encoder to create contextualized features (Figure 9-17)."**
    - **Plain-English:** CNN extracts features, then a Transformer contextualizes them.
    - **Technical terms:** CNN = convolutional neural network; contextualized features = embeddings enriched by context.

26. **"The training procedure consists of three important steps (Figure 9-18): Masked language modeling — Randomly masks part of the input for the Transformer to predict; Quantized embeddings — Creates a codebook (vocabulary) of quantized embeddings to reduce the number of embeddings to represent; Contrastive learning — Updates the model to produce embeddings close to similar inputs and further away from dissimilar inputs."**
    - **Plain-English:** wav2vec 2.0 trains with masked prediction, quantized embedding codebook, and contrastive learning.
    - **Technical terms:** masked language modeling, quantization, codebook.

27. **"A popular extension to this technique is HuBERT, which replaces the quantization step with a clustering algorithm to create a set of features for the contrastive learning task."**
    - **Plain-English:** HuBERT swaps quantization for clustering.
    - **Technical terms:** clustering = grouping similar items.

28. **"A popular model for encoding audio in LLMs is Whisper. Whisper is used for automatic speech recognition (ASR) and has an encoder-decoder architecture to represent the audio (encoder) and to generate a transcription (decoder)."**
    - **Plain-English:** Whisper = ASR model with encoder (audio) + decoder (transcription).
    - **Technical terms:** ASR = automatic speech recognition; encoder-decoder architecture.

29. **"The architecture is quite straightforward. To represent the audio, it is converted to a spectrogram, which is an image of sound that shows how the pitch and loudness of a given sound change over time (Figure 9-19). Features are extracted using convolutional layers and passed to the encoder-decoder model for contextual processing."**
    - **Plain-English:** Whisper turns audio into a spectrogram image, extracts features with convolutions, then encodes/decodes.
    - **Technical terms:** spectrogram = image of pitch/loudness over time.

30. **"The model is trained on different tasks, such as multilingual speech recognition and translation. Each task has its own tokens, such as 'TRANSCRIBE' for transcription and 'TRANSLATE' for translation tasks, so the decoder understands which task should be executed. Additional tokens are added for language, if there is speech, time tokens, etc."**
    - **Plain-English:** Whisper uses task tokens (TRANSCRIBE/TRANSLATE), language tokens, time tokens.
    - **Technical terms:** task token = special token selecting the task.

31. **"When Whisper is used in MLLMs, typically only the Encoder is used since it serves to create the audio representations. Instead, the LLM can handle the generative part (Figure 9-20). Like CLIP, Whisper is a popular audio encoder for MLLMs since the audio embeddings have already had 'exposure' to the textual modality."**
    - **Plain-English:** In MLLMs, only Whisper's encoder is used; the LLM generates.
    - **Technical terms:** encoder-only usage = embedding extraction.

32. **"Contrastive Language-Audio Pre-training (CLAP) is based on CLIP but processes audio inputs instead of images. The audio waveforms in CLAP (Figure 9-21) are processed in one of two ways, given a chunk duration d (e.g., 10 seconds): If the audio is less than 10 seconds, repeat the input, then pad it with zero values. If the audio is more than 10 seconds, the entire input is downsampled to 10 seconds. Then, three slices of 10 seconds are randomly sampled."**
    - **Plain-English:** CLAP (like CLIP but audio) handles variable-length audio: repeats short audio, downsamples/samples long audio.
    - **Word meanings:** *downsampled* = reduced; *pad* = add zeros.
    - **Technical terms:** chunk duration, random sampling.

33. **"The authors tested several audio encoders and found that the Hierarchical Token Semantic-Audio Transformer (HTS-AT) worked best. HTS-AT converts the audio into a spectrogram and extracts patches from it to generate 'tokens,' much like the image examples we explored. The large token/patch embeddings are processed by several hierarchical Transformer models to downscale the embeddings (Figure 9-22)."**
    - **Plain-English:** CLAP uses HTS-AT: spectrogram → patches → tokens → hierarchical transformers to shrink embeddings.
    - **Technical terms:** HTS-AT = Hierarchical Token Semantic-Audio Transformer.

34. **"Using the text and audio encoders, CLAP then uses contrastive learning based on audio/text pairs. These are often captions or descriptions of audio samples."**
    - **Plain-English:** CLAP aligns audio with text captions via contrastive learning.
    - **Technical terms:** audio/text pair = caption matched to a clip.

### Video

35. **"Encoding videos has a significant overlap with encoding images, but with several (potential) differences. Videos have an additional temporal dimension that needs to be encoded, namely as a sequence of images. You can also consider adding the audio and/or subtitles to the encoding process, as those are tightly related (Figure 9-23). In other words, the main challenge in encoding the video modality is how to model that temporal dimension. How do we keep track of this sequence of images?"**
    - **Plain-English:** Video = images over time (+ audio/subtitles); key challenge is the temporal dimension.
    - **Technical terms:** temporal dimension = time ordering of frames.

36. **"The most basic version is to use a ViT or CLIP-like model, sample a number of frames, embed, and finally average them (Figure 9-24). However, this averages away the temporal dimension and prevents the LLM from understanding how the video started and ended."**
    - **Plain-English:** Naive approach: sample frames, embed, average — but that loses time order.
    - **Technical terms:** frame averaging = losing temporal info.

37. **"An early example of adapting ViT-like models for video, is called the Video Vision Transformer (ViViT). This method uses different combinations of Transformer models for both the spatial information (the image itself) and the temporal information (changes between subsequent images)."**
    - **Plain-English:** ViViT uses separate transformers for spatial and temporal info.
    - **Technical terms:** spatial vs temporal information.

38. **"To start, the authors consider extracting tokens from videos in two ways. The first is uniform frame sampling, where frames are sampled and passed through ViT to create tokens for each patch in each sampled frame (Figure 9-25). The second method is to create tubelet embeddings. A tubelet is a block of pixels taken from consecutive frames, forming a 3D patch that captures a part of the image as well as its changes over time. Instead of passing a single image to a ViT model, you would now pass non-overlapping tubelets that cover multiple images, producing tubelet embeddings. In other words, it would span both the spatial and temporal nature of videos (Figure 9-26)."**
    - **Plain-English:** ViViT extracts tokens either per sampled frame or via 3D tubelets spanning space + time.
    - **Technical terms:** uniform frame sampling; tubelet = 3D pixel patch over consecutive frames.

39. **"The authors of the ViViT paper experimented with several models, but the most prominent one was to pass either embedding type to a set of Transformer Encoders that primarily process spatial information (Figure 9-27)."**
    - **Plain-English:** ViViT feeds embeddings into spatial-focused Transformer Encoders.
    - **Technical terms:** spatial Transformer Encoder.

40. **"The idea of separating temporal and spatial dimensions of video encoding tasks was also explored in a single Transformer model, namely TimeSformer. Like ViViT, frames are sampled and split up into patches. These patches are not processed at first like tubelets but instead flattened and passed directly to a Transformer (Figure 9-28)."**
    - **Plain-English:** TimeSformer keeps space/time in one Transformer, using flattened patches.
    - **Technical terms:** single-Transformer video model.

41. **"Instead of applying different transformers for different dimensions (temporal versus spatial), TimeSformer applies attention mechanisms that each focus on a different aspect, namely Time Attention and Space Attention (Figure 9-29). Both forms of attention work the same as Self-attention (which we covered in Chapter 2) but operate on different groupings of image patches. Time Attention creates tubelets to operate along the time dimension as a way to capture motion patterns. In contrast, Space Attention operates on patches created within individual frames to focus on spatial structures such as object relationships."**
    - **Plain-English:** TimeSformer uses one transformer with two attention types: Time Attention (motion across frames) and Space Attention (structure within frames).
    - **Technical terms:** self-attention; Time Attention; Space Attention.

42. **"These attention mechanisms are called Divided Space-Time Attention and allow for better representations than if only Space Attention were used or ViT (Figure 9-30)."**
    - **Plain-English:** Divided Space-Time Attention outperforms space-only or ViT.
    - **Technical terms:** Divided Space-Time Attention.

43. **"Note how all these techniques have very similar underlying principles of sampling frames, creating patch tokens, embedding them, and finally using them as token embeddings in some variant of a Transformer Encoder. Popular examples include VideoBERT, Frozen in Time, and VideoMae."**
    - **Plain-English:** All video methods share the same pattern: sample, patch, embed, Transformer.
    - **Technical terms:** VideoBERT, Frozen in Time, VideoMAE = video models.

44. **"'Just' CLIP: "There is a bit of redundancy here. Note that there are now two Transformer Encoders before the embeddings are passed to the LLM. Instead, we could also skip the second and have the LLM figure out the temporal dimensions by having the connector do a bit more processing (Figure 9-32). The underlying idea here is that since decoder LLMs are already good at processing temporal dimensions in text, the same could be done for videos if each frame (or patch) is considered as a sequence of tokens. As such, both text and video can be seen as having similar time-based sequential natures (Figure 9-33)."**
    - **Plain-English:** Skip the separate temporal encoder; let the LLM handle video-as-token-sequence like text.
    - **Technical terms:** temporal redundancy; video tokens as a sequence.

45. **"As a result, we can pass sample frames from the video, create embeddings for each image, and then pass them as a sequence (like with text) to the LLM. The connector can then either add temporal data or simply convert the image embeddings to the type of embeddings the LLM expects (Figure 9-34)."**
    - **Plain-English:** Feed frame embeddings as a sequence to the LLM; the connector adds temporal data or converts embeddings.
    - **Technical terms:** sequence of image embeddings.

46. **"These video 'tokens' can either be formed as the entire image or as individual image tokens. A great example is the VideoMamba paper that uses positional embeddings to 'enhance' the image embeddings (Figure 9-35). It first adds spatial information to the embeddings (to which part of the frame does an embedding belong?) and then temporal information (in which order are the frames?)."**
    - **Plain-English:** VideoMamba adds spatial then temporal positional info to frame embeddings.
    - **Technical terms:** positional embedding = encodes position/order.

### Many-Modality: ImageBind

47. **"In previous examples, modalities are typically encoded in isolation or in pairs with other modalities. A disadvantage of this approach is that we need different models for different modalities. The generated embeddings by different models cannot be compared to one another (Figure 9-36)."**
    - **Plain-English:** Separate encoders produce incomparable embeddings.
    - **Word meanings:** *in isolation* = separately.

48. **"A popular technique for finding a singular embedding space that can hold many different modalities is called ImageBind. This combines contrastive learning with an interesting idea, namely that a single image can bind different modalities. An image of a rainy day in the park can remind us of the sound of waves, the cold touch of mist on skin, and even evoke a quiet mood."**
    - **Plain-English:** ImageBind creates one embedding space by using images to connect all modalities.
    - **Technical terms:** singular embedding space; image binding.

49. **"As we have seen before, many modalities are actually processed through the lens of images. Video and audio encoders, for instance, use image encoders as the main encoder for processing these modalities (Figure 9-37)."**
    - **Plain-English:** Video/audio use image encoders under the hood.
    - **Word meanings:** *through the lens of* = from the perspective of.

50. **"The primary objective of the ImageBind project is to utilize images as the basis for contrastive learning. As explored before, contrastive learning typically trains on pairs of data. With ImageBind, one of those modalities will always be related to the visual modality (images or video)."**
    - **Plain-English:** ImageBind always pairs something with the visual modality.
    - **Technical terms:** visual-anchored contrastive learning.

51. **"For each pair of modalities, a linear projection is first applied to the original embeddings to make sure that the embeddings of different pairs of modalities are aligned (Figure 9-38). During training, the following pairs were trained: (Image, Text); (Image, Depth Sensor Data); (Image, Thermal Data); (Video, Inertial Measurement Unit [IMU; e.g., accelerometer data]); (Video, Audio)."**
    - **Plain-English:** Linear projections align pairs; training uses image/video anchored pairs (text, depth, thermal, IMU, audio).
    - **Technical terms:** linear projection; IMU = inertial measurement unit.

52. **"Interestingly, although there were many unseen pairs of modalities during training, such as (Audio, Text), their embeddings were aligned in the embedding space (Figure 9-39). This emergent alignment of unseen pairs of modalities showcases how strong this 'binding' factor of images can be."**
    - **Plain-English:** Unseen pairs (audio-text) align automatically — emergent alignment.
    - **Technical terms:** emergent alignment = alignment not directly trained.

## Section 4: Connecting Modalities

53. **"We explored how representations (embeddings) can be created for many different modalities. Before we can feed those to the LLM, making it multi-modal, there's a big problem with the generated embeddings. The encoders for each modality generally create different embeddings with respect to their size, dimensions, and embedding space than what the LLM might expect. Likewise, there might be differences in positional encoding and tokenization schemas. As such, we need a connector that transforms those embeddings to the same type of embeddings as its token embeddings (Figure 9-40)."**
    - **Plain-English:** Connectors convert per-modality embeddings into LLM-compatible token embeddings.
    - **Technical terms:** connector = alignment module.

54. **"There are roughly three types of connectors (Figure 9-41): Projection-based — A Multi-Layer Perceptron (MLP) to project multi-modal embeddings into an embedding size compatible with the LLM; Query-based — Leveraging groups of learnable query tokens to extract information in a query-based manner; Fusion-based — Adding additional (cross-attention) layers in the LLM that can process both text and other multi-modal embeddings."**
    - **Plain-English:** Three connector families: MLP projection, learnable queries, cross-attention fusion.
    - **Technical terms:** MLP, learnable query tokens, cross-attention.

### Projection-based Connector

55. **"The most straightforward method of aligning the multi-modal embeddings with the text embeddings that an LLM expects is by simply projecting them. This is typically done with a single linear layer to convert these non-text embeddings to token embeddings (Figure 9-42)."**
    - **Plain-English:** Project non-text embeddings to token embeddings with one linear layer.
    - **Technical terms:** linear layer = simple learned projection.

56. **"The projection doesn't always have to be up (from fewer to more values) but can also be down projected (from more to fewer values) if the non-text embeddings are smaller than the token embeddings the LLM expects. The resulting projection of the non-text embeddings is then concatenated with the token embeddings of the text input. The LLM will treat every embedding as if it were a token embedding (Figure 9-43)."**
    - **Plain-English:** Project up or down as needed; concatenate with text embeddings.
    - **Technical terms:** concatenation = joining along the sequence dimension.

57. **"A great example of this technique is used in Large Language and Vision Assistant (LLaVA). LLaVA is an MLLM that is not only able to process text but also reason about images. This technique attempts to make Vicuna, a Llama 2 variant that can process only text, multi-modal. The authors of LLaVA chose this model because, at the time (2023), it had the best instruction-following capabilities."**
    - **Plain-English:** LLaVA makes text-only Vicuna multi-modal; Vicuna was chosen for instruction following (2023).
    - **Technical terms:** Vicuna = Llama 2-based chatbot.

58. **"LLaVA was created in two steps, using the same projection scheme we explored previously (Figure 9-44): Step 1 - Pre-training for feature alignment: Only the projection layer is trained. The weights of both the ViT and LLM (Vicuna) are frozen. Step 2 - Fine-tuning end-to-end: Both the projection layer and the LLM are trained. Only the ViT weights are frozen."**
    - **Plain-English:** LLaVA trains projection first (ViT+LLM frozen), then projection+LLM (ViT frozen).
    - **Technical terms:** frozen weights = not updated during training.

59. **"As shown, this projection-based connector needs to be trained (as is the case with all connectors) to properly project the initial embeddings to what a specific LLM needs, in this case the Vicuna model. This training procedure uses text/image pairs to train the MLP and LLM. The first step is meant to align the type of embeddings to what the Vicuna model expects. The second step allows the LLM to learn to reason about the text/image pairs."**
    - **Plain-English:** Step 1 aligns embeddings; Step 2 teaches reasoning over image/text pairs.
    - **Technical terms:** feature alignment; instruction tuning.

60. **"The resulting model demonstrated, at the time, state-of-the-art multi-modal chat capabilities for publicly available models. Likewise, this training procedure has made it relatively straightforward to create a multi-modal LLM. As such, the projection-based technique, albeit simple in nature, is still used for more recent architectures like Qwen2.5-VL, Phi-4-multimodal, and Janus. Moreover, this projection can be used for any modality, not only images."**
    - **Plain-English:** Projection method was SOTA and still powers Qwen2.5-VL, Phi-4-multimodal, Janus; works for any modality.
    - **Word meanings:** *albeit* = although.
    - **Technical terms:** SOTA = state of the art.

### Query-based Connector

61. **"A more involved method of connecting modalities is through a query-based connector. This connector is meant to extract relevant features from all non-textual inputs before feeding them to the LLM (Figure 9-45). These relevant features are generally referred to as 'queries' and are learnable by this connector, commonly referred to as the Q-Former."**
    - **Plain-English:** The Q-Former learns queries that extract relevant features from non-text inputs.
    - **Technical terms:** Q-Former = querying transformer.

62. **"Instead of projecting all ViT features to the LLM, like the projection-based connector, the Q-Former creates a fixed number of features that the LLM can process. A major benefit to this approach is that it learns only the most relevant visual information."**
    - **Plain-English:** Q-Former compresses visual info to a fixed number of relevant features.
    - **Technical terms:** fixed feature count = constant input length.

63. **"The Q-Former was first introduced in BLIP-2, an MLLM that uses a transformer model to process both image features and text features. During training, the transformer model will receive learnable queries, embeddings that get 'updated' with relevant visual and textual information. This transformer model has two modes, one with cross-attention for processing the images and one without cross-attention to process the text (Figure 9-46)."**
    - **Plain-English:** BLIP-2's Q-Former has two modes: with cross-attention (images) and without (text).
    - **Technical terms:** cross-attention = attention to external features.

64. **"Since this transformer model has two modes, this model can then be trained using contrastive learning using image/text pairs. The output embeddings of this model are the learned queries, which contain the image embeddings' most relevant information. It's essentially a way to compress visual tokens into a smaller number of representation vectors."**
    - **Plain-English:** Queries compress visual tokens into fewer vectors; trained by contrastive learning.
    - **Technical terms:** compression of visual tokens.

65. **"There are two stages in this training process. In stage 1, the Q-Former was trained on three tasks such that the queries can learn to extract the visual representation that is most informative of the text: Image-text contrastive learning — Aligns image and text representations by maximizing mutual information; Image-grounded text generation — Trains the Q-Former to generate text conditioned on images; Image-text matching — Binary classification to predict if image-text pairs match."**
    - **Plain-English:** Stage 1 trains the Q-Former with three tasks: contrastive alignment, image-grounded text generation, and image-text matching.
    - **Technical terms:** mutual information; image-grounded generation; binary classification.

66. **"In stage 2, the trained Q-Former is connected to an LLM (OPT) to improve the LLM's generative capabilities. The output query embeddings are projected through an MLP to match the LLM's input dimensions. Then, only the Q-Former and MLP are trained; the LLM and image encoder are frozen (Figure 9-47)."**
    - **Plain-English:** Stage 2: connect Q-Former to OPT; train only Q-Former + MLP; freeze LLM and encoder.
    - **Technical terms:** OPT = Open Pre-trained Transformer.

67. **"This technique was used in Qwen's first multi-modal LLM, namely Qwen-VL. Like the projection-based connector, this connector can be used for any modality."**
    - **Plain-English:** Qwen-VL used the Q-Former; works for any modality.
    - **Technical terms:** Qwen-VL = Qwen vision-language model.

### Fusion-based Connector

68. **"The previous connectors do not change the LLM's architecture to enable multi-modal understanding. With a fusion-based connector, extra modules are added to the LLM's architecture to enable a deep interaction and fusion between text and non-text features."**
    - **Plain-English:** Fusion connectors modify the LLM itself for deep text/non-text fusion.
    - **Technical terms:** architectural modification.

69. **"One of the first methods in doing so is called Flamingo. This model uses pre-trained LLMs as their basis and adds trainable cross-attention layers that can process the input images. More specifically, images are processed with a vision encoder, and its output is processed through a perceiver resampler. This resampler is a Transformer model, much like the Q-Former, and maps the image embeddings to a fixed sequence size. The output of the resampler is fed to the cross-attention layers that were added to the LLM (Figure 9-48)."**
    - **Plain-English:** Flamingo adds cross-attention layers to a pretrained LLM; a perceiver resampler fixes image token count.
    - **Technical terms:** perceiver resampler = fixed-size feature mapper.

70. **"Only the added cross-attention and perceiver resampler are trained; the pre-trained Vision Encoder and LLM are frozen (Figure 9-49)."**
    - **Plain-English:** Flamingo trains only the new modules.
    - **Technical terms:** frozen base model.

71. **"The goal of the perceiver resampler is to extract the most important vision features and output them in a fixed size, much like the Q-Former. These learned queries will have the same shape as expected by the LLM, and the fixed size allows for reduced computation by focusing on only the most important visual features. The output embeddings are then passed to the additional cross-attention blocks that are intertwined with LLM's regular blocks. In these cross-attention blocks, the LLM can then attend to both the image and text features."**
    - **Plain-English:** Perceiver resampler gives fixed-size, LLM-shaped features; cross-attention blocks let the LLM attend to image + text.
    - **Technical terms:** attention to multi-modal features.

72. **"They are referred to as 'gated' cross-attention because they use a special gating trick, which makes the conditioned model behave identically to the original LLM at initialization. This is done by multiplying the layer outputs by tanh(α), where α is a learnable scalar per layer initialized to 0. Since tanh(0) = 0, these layers initially contribute nothing, preserving the pretrained LLM's behavior. As training progresses, α learns to open the gate. This initialization strategy enhances training stability and final performance (Figure 9-50)."**
    - **Plain-English:** Gated cross-attention scales outputs by tanh(α); α=0 initially → no effect; training opens the gate.
    - **Technical terms:** gating = tanh(α) scaling; learnable scalar.

73. **"This fusion-based connector needs to adjust the architecture of the original LLM, which makes it a bit less flexible than any of the previous techniques. However, by integrating attention into its architecture, there is the potential for improved performance."**
    - **Plain-English:** Fusion is less flexible (needs architecture changes) but can perform better.
    - **Technical terms:** flexibility vs performance trade-off.

### Advantages and Disadvantages

74. **Table 9-1 (Projection-based): "Advantages — Simple: Lightweight to implement. Computational efficiency: Low latency and few parameters. Disadvantages — Scalability: It can produce a large number of tokens, which quickly fills up the LLM's context window. Performance: Since it's decoupled from the LLM and the model is rather small, it has limited expressive power."**
    - **Plain-English:** Projection = simple + efficient but bloats context and has limited expressiveness.
    - **Technical terms:** context window; expressive power.

75. **Table 9-1 (Query-based): "Advantages — Scalability: The fixed number of queries allows for scaling with large multi-modal inputs (e.g., videos). Performance: Has more expressive power than projection-based connectors. Disadvantages — Complexity: Requires relatively complex training procedures. Computational efficiency: Due to the larger models, it adds more latency than a projection-based connector."**
    - **Plain-English:** Query = scalable + more expressive but complex to train and slower.
    - **Technical terms:** fixed query count; latency.

76. **Table 9-1 (Fusion-based): "Advantages — Integration: The integration with the LLM itself allows it to 'see' multi-modal features at every layer instead of just as a prefix. Performance: Has more expressive power than both projection-based and query-based connectors. Disadvantages — Complexity: Requires changes to the architecture of the LLM. Computational efficiency: Requires the LLM to undergo extensive fine-tuning procedures, more so than the other connectors."**
    - **Plain-English:** Fusion = most expressive, sees features everywhere, but needs architecture changes + heavy fine-tuning.
    - **Technical terms:** every-layer integration; extensive fine-tuning.

77. **"There are three main axes at which we can view the potential (dis)advantages of these connectors, namely their performance, computational efficiency, and scalability. Generally, projection-based connectors are used for simplicity and computational efficiency because these connectors are lightweight and easy to train. However, they may quickly fill up the LLM's context window since the input is not compressed. Query-based connectors are primarily used for efficiency and scaling because they keep the input length constant regardless of the image/video resolution. However, a query-based connector may skip over details as it does not have the fine-grained representation of projection-based connectors. Finally, fusion-based connectors tend to perform best because the processing of the multi-modal input is done throughout the entire model, rather than just as token embeddings. However, they're the most complex connectors to implement (requiring changes to the LLM's architecture) and are expensive to train."**
    - **Plain-English:** Projection = simple but fills context; query = constant length but skips details; fusion = best performance but most complex/expensive.
    - **Technical terms:** three axes: performance, computational efficiency, scalability.

## Section 5: TinyAgent — Adding Multi-Modal Understanding

78. **"Throughout this chapter, we covered how multi-modal models are created using methods like SFT and RL. The resulting models can process more than text and typically have either vision or audio understanding. The model that we've been using thus far, Gemma 4 E4B, has both. It can process both images and audio. To do so, it has two encoders to process those modalities, a vision encoder for processing images and an audio encoder for processing sound."**
    - **Plain-English:** Gemma 4 E4B has vision + audio encoders.
    - **Technical terms:** SFT = supervised fine-tuning; RL = reinforcement learning.

79. **"The vision encoder in Gemma 4 E4B generates a set of embeddings (also called vision tokens or image tokens) that the LLM can use. In the raw chat template, that tends to look something like this: `<|image> <|image|><|image|><|image|><|image|><|image|> … <|image|> <image|>`."**
    - **Plain-English:** Vision encoder outputs vision tokens shown in the chat template.
    - **Technical terms:** vision token = image embedding token.

80. **"Those are special image tokens used to indicate that a given embedding belongs to an image. The `<|image>` and `<image|>` tokens indicate the start and end, respectively, of a sequence of image tokens. The `<|image|>` is a vision token. In Gemma 4 E4B, there are generally either a maximum of 70, 140, 280, 560, or 1,120 vision tokens. A larger number of vision tokens means that the input image will be processed at a higher resolution."**
    - **Plain-English:** Start/end markers plus vision tokens; 70–1,120 tokens depending on resolution.
    - **Technical terms:** vision token count = resolution setting.

81. **"As we saw in Chapter 5, the processing of this chat template is almost always handled by the inference engine. So there's no need for us to create templates with potentially 1,120 vision tokens. The OpenAI endpoint that we've been using thus far expects the following format: We now have two fields in the content, one for the image and one for the text itself. These will be processed in the chat template of the model. Note that in some cases, the URL needs to be the image itself (in base64) rather than a link to the image. This depends on the inference engine. Some will attempt to convert the image themselves, while others can only process the base64 image."**
    - **Plain-English:** The inference engine builds the template; the API accepts an image_url (base64 or link) plus text.
    - **Technical terms:** inference engine; base64 image.

82. **"Note how this example showcases the message's structure that we explored in detail in Chapter 4, where we covered memory. The messages not only function as the memory of the model but also as the way we communicate with it. As such, the place to enable image inputs is in the Memory class!"**
    - **Plain-English:** Enable images by adding image content to messages in the Memory class.
    - **Technical terms:** Memory class = conversation message store.

83. **"To do so, we need only a very minor change to the Memory class, and that is to add the image_url to the messages if there is an input image. Everything else can stay exactly the same."**
    - **Plain-English:** One small change: add image_url to user messages.

84. **Code (message structure):**
    ```python
    [
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": "https://raw.githubusercontent.com/HandsOnLLM/..."
                    },
                },
                {"type": "text", "text": "What's in this image?"},
            ],
        }
    ]
    ```
    - **Plain-English:** A user message carries both an image_url and text content.
    - **Technical terms:** content array with typed blocks (image_url, text).

85. **Code (MultimodalMemory):**
    ```python
    from illustrated_agents.chapters.ch4 import Memory

    class MultimodalMemory(Memory):
        """Simple memory module to store conversation history."""

        def add(
            self,
            role: str,
            content: str,
            tool_call: dict = None,
            image_data: str = None,
        ):
            """Add a message to memory."""
            # Image
            if image_data:
                is_url = image_data.startswith(("http://", "https://"))
                url = image_data if is_url else f"data:image/png;base64,{image_data}"
                content = [
                    {"type": "image_url", "image_url": {"url": url}},
                    {"type": "text", "text": content},
                ]

            # Main message
            message = {"role": role, "content": content}

            # Tool call
            if tool_call:
                message["tool_calls"] = [tool_call]

            # Append message to memory
            self.messages.append(message)
    ```
    - **Plain-English:** MultimodalMemory wraps text + image (URL or base64) into one message and stores it.
    - **Technical terms:** base64 data URI = `data:image/png;base64,...`.

86. **"Note that we added the option to use either a URL or the base64-encoded image. This allows for support for different inference engines."**
    - **Plain-English:** URL-or-base64 support covers different engines.
    - **Technical terms:** inference engine compatibility.

87. **Code (TinyAgent.run with image support):**
    ```python
    from illustrated_agents.chapters import ch6

    class TinyAgent(ch6.TinyAgent):
        def run(self, task: str, image_data: str = None) -> str:
            """Run the agent on a task."""
            self.memory.add("user", task, image_data=image_data)
            self.trajectory.initialize(task)

            # *Autonomy* loop
            for step in range(self.planner.max_steps):
                result = self._step()
                if result is not None:
                    return result

            return "Max steps reached without completion."
    ```
    - **Plain-English:** run() now accepts image_data and passes it into memory; rest unchanged.
    - **Technical terms:** autonomy loop = plan–act loop over max_steps.

88. **"The only changes we made are: Add the image_data parameter to the run function. Add the image_data parameter to self.memory.add. That is all that's needed to use the image-understanding capabilities of your model!"**
    - **Plain-English:** Two tiny changes enable image understanding.
    - **Technical terms:** minimal integration.

89. **"Let's try it out with an example. We ask the agent to tell us which animal is on the cover of the Hands-On Large Language Model book, but we do not provide it with an image: Which gives: It doesn't know which animal is on the cover. Now, let's give it the image:"**
    - **Plain-English:** Without the image, the model can't answer.
    - **Technical terms:** ablation = testing without the input.

90. **Code (loading the image):**
    ```python
    import base64
    import httpx

    # Download and encode the image
    image_url = "https://raw.githubusercontent.com/HandsOnLLM/.../book_cover.png"
    image_data = base64.b64encode(httpx.get(image_url).content).decode("utf-8")

    # Run Agent with Image Data
    agent.run(
      "Which animal is on the cover of 'Hands-On Large Language Models'?", image_data=image_data
    )
    "The animal on the cover of 'Hands-On Large Language Models' is a **kangaroo**."
    ```
    - **Plain-English:** Download the image, base64-encode it, pass it in; the agent answers "kangaroo".
    - **Technical terms:** base64 encoding of image bytes.

91. **"Which gives: That is correct! All that was needed to make use of its multi-modal capabilities was to process the image in the chat template, which was handled by the inference engine for us."**
    - **Plain-English:** The inference engine handled the template; the agent correctly identified the kangaroo.
    - **Technical terms:** chat template = model-specific message formatting.

92. **"What We Built — TinyAgent/ ├── agent.py ← Updated (The Agent processes images) ├── evaluator.py ├── llm.py ├── memory.py ← Updated (Track images in the messages) ├── planning.py ├── toolbox.py ├── tools.py └── trajectory.py"**
    - **Plain-English:** Only agent.py and memory.py changed; the rest of TinyAgent is untouched.
    - **Technical terms:** modular agent structure.

## Chapter 9 Key Takeaways
- MLLMs process and/or generate more modalities than text; most focus on input encoding to guide text generation (cheaper).
- MLLM components: Encoder + Connector + LLM + optional Generator.
- Encoders: text = tokenizer + embedding layer; images = ViT (patches) & CLIP (contrastive); audio = wav2vec 2.0 (strided waveforms + CNN), HuBERT (clustering), Whisper (ASR, spectrogram), CLAP (HTS-AT); video = ViViT (tubelets), TimeSformer (divided space-time attention), "just CLIP" (LLM handles temporal), VideoMamba (positional embeddings); many-modality = ImageBind (image-anchored contrastive learning, emergent alignment).
- Connectors: projection (LLaVA — simple, fills context), query (Q-Former/BLIP-2/Qwen-VL — fixed queries, scalable), fusion (Flamingo — cross-attention + perceiver resampler + tanh(α) gating, best performance, most complex).
- TinyAgent: enabling images is just adding `image_url` (URL or base64) to Memory messages; the inference engine builds the chat template.
