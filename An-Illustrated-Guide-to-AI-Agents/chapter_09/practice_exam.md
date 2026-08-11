# AI Agents — Practice Exam (Chapter 9)
**Source:** *An Illustrated Guide to AI Agents*, Chapter 9 "Multi-Modal Understanding"
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end.

---

## Section A: Multiple Choice (1 point each)

1. An MLLM is best defined as:
   a) A model that can only process text
   b) A model that generates images instead of text
   c) An LLM that can process and/or generate more modalities than text
   d) A tokenizer for audio inputs

2. Which is NOT one of the four common modalities?
   a) Physical
   b) Text
   c) Audio
   d) Video

3. Why do most MLLMs focus on encoding input rather than generating output?
   a) Generating output is more accurate
   b) It requires larger models
   c) It removes the need for encoders
   d) It is cheaper and avoids large architectural changes

4. The three main components of an MLLM are:
   a) Tokenizer, embedding layer, decoder
   b) Encoder, connector, LLM
   c) ViT, CLIP, CNN
   d) Memory, planner, tools

5. The optional fourth component of an MLLM is:
   a) The generator
   b) The connector
   c) The encoder
   d) The tokenizer

6. The text encoder consists of:
   a) The CNN and the decoder
   b) The ViT and the connector
   c) The tokenizer and the embedding layer
   d) The projection layer and the Q-Former

7. The Vision-Transformer creates image pieces called:
   a) Tokens
   b) Patches
   c) Tubelets
   d) Spectrograms

8. CLIP uses contrastive learning to:
   a) Generate new images
   b) Transcribe speech
   c) Compress audio to spectrograms
   d) Align image and text embeddings

9. In contrastive learning, similar pairs should have:
   a) High embedding similarity
   b) Low embedding similarity
   c) Identical embeddings
   d) Random similarity

10. wav2vec 2.0 creates audio tokens by:
    a) Converting audio to a spectrogram image
    b) Applying a single linear projection
    c) Sampling overlapping (strided) parts of the audio
    d) Sampling uniform frames

11. HuBERT differs from wav2vec 2.0 by:
    a) Using a CNN instead of a Transformer
    b) Replacing quantization with a clustering algorithm
    c) Adding a codebook vocabulary
    d) Removing contrastive learning

12. Whisper is primarily used for:
    a) Image classification
    b) Video generation
    c) Text translation only
    d) Automatic speech recognition

13. Whisper represents audio as:
    a) A spectrogram
    b) A sequence of strided waveforms
    c) A set of patches
    d) A 3D tubelet

14. When Whisper is used in MLLMs, typically only the ______ is used:
    a) Decoder
    b) Tokenizer
    c) Encoder
    d) Cross-attention layer

15. CLAP processes audio using which encoder?
    a) ViT
    b) HTS-AT
    c) wav2vec 2.0
    d) Whisper

16. For audio longer than the CLAP chunk duration, the input is:
    a) Repeated and zero-padded
    b) Split into individual words
    c) Converted to a single patch
    d) Downsampled and randomly sliced into three samples

17. The main challenge in encoding video is:
    a) Modeling the temporal dimension
    b) Capturing color
    c) Handling subtitles
    d) Generating audio

18. The most basic video approach that loses temporal info:
    a) Tubelet embeddings
    b) Divided Space-Time Attention
    c) Sampling frames and averaging their embeddings
    d) Passing frames as text tokens

19. A tubelet is:
    a) A single-frame patch
    b) A 3D patch from consecutive frames capturing space and time
    c) A spectrogram slice
    d) A vision token marker

20. TimeSformer uses which two attention mechanisms?
    a) Image Attention and Text Attention
    b) Query Attention and Value Attention
    c) Self Attention and Cross Attention
    d) Time Attention and Space Attention

21. The "just CLIP" video approach:
    a) Skips explicit temporal encoding and lets the LLM handle temporal dimensions
    b) Adds a second Transformer Encoder
    c) Only uses audio tokens
    d) Requires tubelet sampling

22. VideoMamba enhances image embeddings with:
    a) Cross-attention gating
    b) Quantized codebooks
    c) Positional embeddings (spatial then temporal)
    d) Contrastive loss

23. ImageBind creates:
    a) Separate embedding spaces per modality
    b) A singular embedding space binding many modalities
    c) A spectrogram database
    d) A patch vocabulary

24. In ImageBind, every contrastive pair includes:
    a) The audio modality
    b) The text modality
    c) The thermal modality
    d) The visual modality (images or video)

25. ImageBind demonstrated emergent alignment of:
    a) Text and video
    b) Images and depth
    c) Unseen pairs such as (Audio, Text)
    d) Only trained pairs

26. A connector is needed because encoders produce embeddings that:
    a) Differ in size, dimensions, embedding space, positional encoding, and tokenization from what the LLM expects
    b) Are too small for any LLM
    c) Are always the same as text embeddings
    d) Cannot be concatenated with anything

27. Which is a projection-based connector?
    a) Q-Former
    b) Cross-attention layers
    c) Perceiver resampler
    d) A linear layer / MLP projecting embeddings to LLM size

28. After projection, non-text embeddings are:
    a) Averaged with text embeddings
    b) Concatenated with the text token embeddings
    c) Discarded
    d) Quantized into a codebook

29. LLaVA made which model multi-modal?
    a) OPT
    b) GPT-4
    c) Vicuna (a Llama 2 variant)
    d) Qwen-VL

30. In LLaVA's step 1, which weights are trained?
    a) Only the projection layer
    b) The ViT and the projection layer
    c) The LLM and the ViT
    d) All weights end-to-end

31. Recent architectures still using projection-based connectors:
    a) BLIP-2, Qwen-VL
    b) Flamingo, LLaVA
    c) ViViT, TimeSformer
    d) Qwen2.5-VL, Phi-4-multimodal, Janus

32. The Q-Former was first introduced in:
    a) Flamingo
    b) BLIP-2
    c) CLIP
    d) Whisper

33. A major benefit of the Q-Former is:
    a) Passing all ViT features to the LLM
    b) Avoiding training entirely
    c) Learning only the most relevant visual information into a fixed number of features
    d) Increasing context window size

34. Which is NOT one of the Q-Former's stage-1 training tasks?
    a) Masked language modeling of code
    b) Image-text contrastive learning
    c) Image-grounded text generation
    d) Image-text matching

35. In Q-Former stage 2, which components are trained?
    a) The LLM and the image encoder
    b) The projection layer and ViT
    c) The cross-attention layers
    d) Only the Q-Former and the MLP

36. Qwen-VL used which connector technique?
    a) Projection-based
    b) Query-based (Q-Former)
    c) Fusion-based
    d) Gated cross-attention

37. Flamingo is an example of a:
    a) Projection-based connector
    b) Query-based connector
    c) Fusion-based connector
    d) Contrastive encoder

38. In Flamingo, the perceiver resampler:
    a) Maps image embeddings to a fixed sequence size
    b) Generates transcriptions
    c) Splits text into tokens
    d) Quantizes audio

39. Gated cross-attention scales layer outputs by:
    a) softmax(α)
    b) sigmoid(α)
    c) α + 1
    d) tanh(α), with α initialized to 0

40. Which connector type tends to perform best?
    a) Projection-based
    b) Fusion-based
    c) Query-based
    d) All perform identically

---

## Section B: True/False (1 point each)

41. MLLMs can only process text inputs. (T/F)
42. Creating an MLLM generally involves separately training a base LLM and then adding multi-modality. (T/F)
43. ViT treats each patch of an image like a "word". (T/F)
44. CLIP embeddings are popular for MLLMs because the image embeddings have had exposure to the textual modality. (T/F)
45. wav2vec 2.0 uses strided audio parts processed by a CNN and then a Transformer Encoder. (T/F)
46. Whisper's decoder is used to create audio representations in MLLMs. (T/F)
47. CLAP always processes audio by simply padding it to exactly 10 seconds. (T/F)
48. Averaging sampled frame embeddings preserves the temporal structure of a video. (T/F)
49. TimeSformer uses separate Transformer models for spatial and temporal information. (T/F)
50. ImageBind only aligned pairs that were explicitly trained together. (T/F)
51. A projection-based connector can project embeddings down as well as up. (T/F)
52. The Q-Former compresses visual tokens into a smaller fixed number of vectors. (T/F)
53. Fusion-based connectors require changes to the LLM's architecture. (T/F)
54. Gemma 4 E4B processes both images and audio. (T/F)
55. In TinyAgent, adding image support requires rewriting the entire agent. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Name the three (plus optional fourth) components of an MLLM and state the role of each.
57. Explain the "common embedding space" problem and why a connector is needed.
58. Describe how ViT encodes an image (patches → projection → Transformer Encoder).
59. List the three training steps of wav2vec 2.0 and briefly explain each.
60. Contrast the two ViViT token-extraction methods: uniform frame sampling vs tubelet embeddings.
61. Explain Divided Space-Time Attention and why it helps video understanding.
62. Describe how ImageBind uses images to bind many modalities, and what "emergent alignment" means.
63. Compare projection-based and query-based connectors on scalability, expressiveness, and context usage.
64. Explain Flamingo's gated cross-attention (tanh(α), α initialized to 0) and why this initialization matters.
65. What are the two TinyAgent changes needed to enable image understanding, and how does MultimodalMemory handle URLs vs base64?

---

## Section D: Essay / Applied (5 points each)

66. **Encoding modalities.** Explain how each of the following modalities is encoded: text (tokenizer + embedding layer), images (ViT, CLIP), audio (wav2vec 2.0, HuBERT, Whisper, CLAP), and video (ViViT, TimeSformer, "just CLIP", VideoMamba). For each, identify the shared underlying principle.
67. **Connectors.** Compare the three connector types — projection-based, query-based, fusion-based — across the axes of performance, computational efficiency, and scalability. Give a concrete model example for each (LLaVA, BLIP-2/Qwen-VL, Flamingo) and explain the gating mechanism in Flamingo.
68. **ImageBind.** Explain how ImageBind creates a singular embedding space, the training pairs used, the role of linear projections, and why the emergent alignment of unseen pairs (e.g., Audio, Text) demonstrates the strength of images as a binding factor.
69. **Multi-modal in TinyAgent.** Describe how the book's TinyAgent gains image understanding: the MultimodalMemory change, the run function change, how URLs vs base64 images are handled, and why the chat template is handled by the inference engine. Walk through the kangaroo cover example.
70. **Design a multi-modal agent.** You are building an agent that must (a) listen to spoken instructions, (b) read text, and (c) judge screenshots of websites. Recommend an encoder for audio and one for images, choose a connector type (justify against the three axes), and describe how you would modify your agent's Memory class to accept the image input, using concepts from this chapter.

---

## ANSWER KEY

### Section A: Multiple Choice
1. c
2. a
3. d
4. b
5. a
6. c
7. b
8. d
9. a
10. c
11. b
12. d
13. a
14. c
15. b
16. d
17. a
18. c
19. b
20. d
21. a
22. c
23. b
24. d
25. c
26. a
27. d
28. b
29. c
30. a
31. d
32. b
33. c
34. a
35. d
36. b
37. c
38. a
39. d
40. b

### Section B: True/False
41. **F** — MLLMs process and/or generate more modalities than text (image, audio, video).
42. **T** — Train a base LLM first, then add multi-modality.
43. **T** — Each patch is considered a "word".
44. **T** — Image embeddings have had exposure to the textual modality.
45. **T** — Strided audio → CNN → Transformer Encoder.
46. **F** — Typically only the Encoder is used in MLLMs; the LLM handles generation.
47. **F** — Short audio is repeated + zero-padded; long audio is downsampled and three slices are randomly sampled.
48. **F** — Averaging destroys the temporal dimension.
49. **F** — TimeSformer uses a single Transformer with two attention mechanisms (Divided Space-Time Attention).
50. **F** — It demonstrated emergent alignment of unseen pairs.
51. **T** — Projection can go up or down depending on sizes.
52. **T** — It compresses visual tokens into a fixed number of query vectors.
53. **T** — Extra modules/cross-attention layers are added to the LLM.
54. **T** — It has a vision encoder and an audio encoder.
55. **F** — Only two small changes: run() and memory.add() accept image_data.

### Section C: Short Answer (model answers)
56. **MLLM components.** Encoder — encodes a modality (image, audio, etc.) to embeddings. Connector — converts encoded features so the LLM can use them. LLM — processes encoded features and generates text. Optional Generator — generates modalities aside from text.
57. **Common embedding space problem.** Encoders generate embeddings differing in size, dimensions, embedding space, positional encoding, and tokenization from what the LLM expects. A connector projects/aligns them to the same type as the LLM's token embeddings.
58. **ViT encoding.** Cuts the image into patches (each a "word"), flattens them, applies a projection, and feeds them to a regular Transformer Encoder that processes them as tokens; resulting embeddings are usable for classification, clustering, search.
59. **wav2vec 2.0 training.** (1) Masked language modeling — randomly masks part of the input for the Transformer to predict; (2) quantized embeddings — a codebook/vocabulary reduces the number of embeddings; (3) contrastive learning — embeddings close to similar inputs, far from dissimilar ones.
60. **ViViT methods.** Uniform frame sampling: frames sampled and passed through ViT to create tokens per patch. Tubelet embeddings: non-overlapping 3D patches from consecutive frames spanning spatial and temporal info.
61. **Divided Space-Time Attention.** Time Attention creates tubelets along the time dimension to capture motion; Space Attention operates within individual frames for spatial structures. Combined they beat Space-Attention-only or ViT.
62. **ImageBind.** Uses images as the basis for contrastive learning (one modality of each pair is always visual), applies linear projections to align pairs, and trains pairs like (Image, Text), (Image, Depth), (Video, Audio). Emergent alignment = unseen pairs (e.g., Audio, Text) aligning anyway, showing images' binding strength.
63. **Projection vs query connectors.** Projection: simple/efficient but passes many tokens (fills context) and limited expressiveness. Query: fixed number of queries → constant input length regardless of resolution (great for videos) and more expressive, but complex training and more latency; may skip fine-grained details.
64. **Gated cross-attention.** Layer outputs multiplied by tanh(α), α a learnable scalar per layer initialized to 0. Since tanh(0)=0 the layers initially contribute nothing, preserving pretrained LLM behavior; α opens the gate during training, improving stability and final performance.
65. **TinyAgent changes.** (1) Add `image_data` to the `run` function; (2) add `image_data` to `self.memory.add`. MultimodalMemory checks if image_data starts with http/https (kept as URL) else wraps as `data:image/png;base64,...`; content becomes image_url + text blocks — supporting different inference engines.

### Section D: Essay (grading notes)
66. **Expect** per-modality encoding: text (tokenizer splits into tokens, embedded); images (ViT patches; CLIP contrastive alignment with text); audio (wav2vec 2.0 strided waveforms → CNN → Transformer; HuBERT clustering; Whisper spectrogram + encoder-decoder; CLAP with HTS-AT); video (ViViT frames/tubelets; TimeSformer divided space-time attention; "just CLIP" lets the LLM handle temporal; VideoMamba positional embeddings). Shared principle: sample/segment input, create patch/token-like pieces, embed, process with a Transformer Encoder variant.
67. **Expect** axes definitions; projection (simple, efficient, fills context, limited power; LLaVA); query (fixed queries, scalable, more expressive, complex/latency; BLIP-2/Qwen-VL Q-Former); fusion (sees features at every layer, most expressive, needs architecture change + most fine-tuning; Flamingo with perceiver resampler + gated cross-attention, tanh(α) α=0).
68. **Expect** singular embedding space; visual-anchored contrastive pairs (Image-Text, Image-Depth, Image-Thermal, Video-IMU, Video-Audio); linear projections to align pairs; emergent alignment of unseen pairs (Audio, Text) because many modalities are processed through image encoders — evidence of images as binding factor.
69. **Expect** MultimodalMemory subclassing ch4 Memory; add() building image_url + text content; URL vs base64 handling (`data:image/png;base64,...`); run() passing image_data to memory.add; chat template built by the inference engine (70/140/280/560/1120 vision tokens); kangaroo cover example (without image → no answer; with base64 image → "kangaroo").
70. **Expect** audio encoder choice (Whisper or CLAP for speech understanding) and image encoder choice (ViT/CLIP); connector choice with justification against the three axes (e.g., query-based for constant input length, projection for simplicity, fusion for best performance at highest cost); Memory class modification to include image_url blocks exactly as in MultimodalMemory; noting inference-engine handling of chat templates.

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely connector types or encoding specifics).
- **<70%**: Re-read the chapter and study notes, then retry this exam in 2–3 days.
