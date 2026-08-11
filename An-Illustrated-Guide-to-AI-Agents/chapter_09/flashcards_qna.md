# Flashcards & Q&A — Chapter 9
**Source:** *An Illustrated Guide to AI Agents*, Chapter 9 "Multi-Modal Understanding"
**How to use:** Cover the answer, test yourself, then reveal. Great for spaced repetition.

## Part 1: Term → Definition

1. **MLLM** → Multi-modal LLM — an LLM that can process and/or generate more types of inputs (modalities) than text.
2. **Multi-modal understanding** → Processing multiple input modalities (text, image, audio, video) together to form a more accurate response.
3. **The four common modalities** → Text, image, audio, and video.
4. **Why do most MLLMs focus on encoding input rather than generating output?** → Encoding is much cheaper and doesn't necessarily require large architectural changes.
5. **The three main components of an MLLM (plus optional fourth)** → Encoder, Connector, LLM, and (optional) Generator.
6. **Encoder** → Encodes a modality (images, audio, etc.) to features (embeddings).
7. **Connector** → Converts encoded features so the LLM can use them.
8. **What does the LLM component do in an MLLM?** → Processes encoded features and generates text.
9. **Generator (optional)** → Generates modalities aside from text.
10. **How is a text-only LLM generally turned into an MLLM?** → Separately train a base LLM, then add multi-modality to it.
11. **What does "encoding a modality" mean?** → Converting a given input (text, image, audio, etc.) to embeddings.
12. **What does the LLM expect as input?** → Embeddings.
13. **Text encoder = ?** → Tokenizer + embedding layer.
14. **ViT (Vision-Transformer)** → Image encoder that cuts images into patches, each considered a "word", then flattens + projects them for a Transformer Encoder.
15. **What is a patch in ViT?** → A piece of an image treated like a token.
16. **CLIP** → Contrastive Language-Image Pre-training; encodes both text and images using ViT + a transformer encoder, aligned by contrastive learning.
17. **What does contrastive learning do?** → Trains encoders so similar pairs have high embedding similarity and dissimilar pairs have low similarity.
18. **Why is CLIP a popular image encoder for MLLMs?** → Its image embeddings have already had "exposure" to the textual modality.
19. **wav2vec 2.0** → Audio encoder that samples overlapping (strided) audio parts, processes them with a CNN, then a Transformer Encoder.
20. **The three training steps of wav2vec 2.0** → Masked language modeling, quantized embeddings (codebook), and contrastive learning.
21. **HuBERT** → Extension of wav2vec 2.0 that replaces the quantization step with a clustering algorithm.
22. **Whisper** → Popular audio encoder in LLMs for automatic speech recognition (ASR); encoder-decoder architecture using a spectrogram.
23. **Spectrogram** → An image of sound showing how pitch and loudness change over time.
24. **What task tokens does Whisper use?** → "TRANSCRIBE" for transcription, "TRANSLATE" for translation, plus language/time tokens.
25. **When Whisper is used in MLLMs, which part is typically used?** → Only the Encoder (the LLM handles generation).
26. **CLAP** → Contrastive Language-Audio Pre-training; CLIP-based, processes audio with the HTS-AT encoder.
27. **How does CLAP handle audio shorter/longer than the chunk duration (e.g., 10s)?** → Shorter: repeat then zero-pad. Longer: downsample to 10s and randomly sample three 10-second slices.
28. **HTS-AT** → Hierarchical Token Semantic-Audio Transformer; converts audio to a spectrogram, extracts patches as tokens, downscales via hierarchical transformers.
29. **What pairs does CLAP train on?** → Audio/text pairs (captions or descriptions of audio samples).
30. **Main challenge in video encoding** → Modeling the temporal dimension — how to keep track of the sequence of images.
31. **The most basic video approach** → Use ViT/CLIP, sample frames, embed, and average them (loses the temporal dimension).
32. **ViViT** → Video Vision Transformer; uses uniform frame sampling and/or tubelet embeddings, processed by spatial-focused Transformer Encoders.
33. **Tubelet** → A block of pixels from consecutive frames forming a 3D patch capturing both the image part and its changes over time.
34. **TimeSformer** → Single-Transformer video model using Divided Space-Time Attention.
35. **Time Attention** → Creates tubelets along the time dimension to capture motion patterns.
36. **Space Attention** → Operates on patches within individual frames to focus on spatial structures (e.g., object relationships).
37. **Divided Space-Time Attention** → Combining Time Attention and Space Attention; better representations than Space Attention alone or ViT.
38. **Other video models mentioned** → VideoBERT, Frozen in Time, VideoMAE.
39. **"Just CLIP" approach** → Skip the explicit temporal encoder and let the LLM figure out temporal dimensions by passing frame embeddings as a token sequence (like text).
40. **VideoMamba** → Uses positional embeddings to add spatial info (which part of the frame) then temporal info (frame order) to image embeddings.
41. **ImageBind** → Technique creating a singular embedding space binding many modalities, using images as the basis for contrastive learning.
42. **Why can a single image bind modalities?** → One image evokes many associations (e.g., a rainy park evokes sound of waves, cold mist, a quiet mood).
43. **ImageBind training pairs** → (Image, Text), (Image, Depth), (Image, Thermal), (Video, IMU), (Video, Audio).
44. **Emergent alignment in ImageBind** → Unseen modality pairs (e.g., Audio, Text) align in embedding space despite never being trained together.
45. **Why does a connector exist?** → Different encoders produce embeddings with different size, dimensions, embedding space, positional encoding, and tokenization than the LLM expects; the connector aligns them to the LLM's token embeddings.
46. **The three connector types** → Projection-based (MLP), query-based (learnable query tokens), fusion-based (cross-attention layers in the LLM).
47. **Projection-based connector** → An MLP (often a single linear layer) projects multi-modal embeddings into an embedding size compatible with the LLM.
48. **Can projection go down as well as up?** → Yes — down-projected if non-text embeddings are larger than the LLM's token embeddings.
49. **What happens after projection?** → Projected non-text embeddings are concatenated with the text token embeddings; the LLM treats every embedding as a token embedding.
50. **LLaVA** → Large Language and Vision Assistant; made text-only Vicuna (a Llama 2 variant) multi-modal.
51. **Why did LLaVA choose Vicuna?** → At the time (2023) it had the best instruction-following capabilities.
52. **LLaVA two-step training** → Step 1: pre-train only the projection layer (ViT + LLM frozen). Step 2: fine-tune projection + LLM end-to-end (only ViT frozen).
53. **Recent architectures using projection-based connectors** → Qwen2.5-VL, Phi-4-multimodal, Janus.
54. **Query-based connector** → Learnable query tokens (queries) extract the most relevant features from non-textual inputs before feeding the LLM.
55. **Q-Former** → The transformer that creates a fixed number of features the LLM can process; learns only the most relevant visual information.
56. **Which model introduced the Q-Former?** → BLIP-2.
57. **The Q-Former's two modes** → One with cross-attention (processing images), one without (processing text).
58. **The three stage-1 training tasks of the Q-Former** → Image-text contrastive learning (maximize mutual information), image-grounded text generation, image-text matching (binary classification).
59. **Stage 2 of Q-Former training** → Connect it to an LLM (OPT); project queries through an MLP; train only Q-Former + MLP, freeze LLM and image encoder.
60. **Which MLLM used the Q-Former technique?** → Qwen-VL (Qwen's first multi-modal LLM).
61. **Fusion-based connector** → Adds extra modules (cross-attention layers) inside the LLM's architecture for deep interaction between text and non-text features.
62. **Flamingo** → First fusion-based connector model: pretrained LLM + trainable cross-attention layers + perceiver resampler.
63. **Perceiver resampler** → Transformer (like Q-Former) mapping image embeddings to a fixed sequence size; extracts the most important vision features.
64. **Which parts of Flamingo are trained?** → Only the added cross-attention and perceiver resampler; the vision encoder and LLM are frozen.
65. **Gated cross-attention** → Cross-attention scaled by tanh(α), where α is a learnable scalar per layer initialized to 0 — the model behaves identically to the original LLM at initialization.
66. **Why does gating with tanh(0)=0 help?** → The new layers initially contribute nothing, preserving pretrained behavior; α learns to open the gate, enhancing training stability and final performance.
67. **Projection-based connector advantages** → Simple, lightweight, low latency, few parameters.
68. **Projection-based connector disadvantages** → Many tokens quickly fill the LLM's context window; limited expressive power (decoupled from the LLM, small model).
69. **Query-based connector advantages** → Fixed number of queries allows scaling with large inputs (e.g., videos); more expressive than projection.
70. **Query-based connector disadvantages** → Complex training procedures; larger models add more latency than projection.
71. **Fusion-based connector advantages** → LLM "sees" multi-modal features at every layer (not just as a prefix); most expressive.
72. **Fusion-based connector disadvantages** → Requires changes to the LLM's architecture; extensive (most) fine-tuning.
73. **The three axes for comparing connectors** → Performance, computational efficiency, scalability.
74. **What is Gemma 4 E4B?** → The book's running model; processes both images and audio via a vision encoder and an audio encoder.
75. **Vision tokens (image tokens)** → Embeddings generated by the vision encoder that the LLM can use.
76. **Gemma 4 E4B vision token counts** → Maximum of 70, 140, 280, 560, or 1,120; more tokens = higher resolution.
77. **What do `<|image>` and `<image|>` tokens mark?** → The start and end of a sequence of image tokens.
78. **What is `<|image|>`?** → A vision token.
79. **Who builds the chat template in practice?** → The inference engine (no need to manually create up to 1,120 vision tokens).
80. **Where is image input enabled in TinyAgent?** → In the Memory class — add `image_url` to the message content.

## Part 2: Short Answer

81. **Why would a text-only agent miss important information in everyday situations?** → Real tasks include visual/audio context (app UI development, agentic browsing, listening to spoken requests); a text-only agent can't process them.
82. **Explain the "common embedding space" problem.** → Different modality encoders produce embeddings with different dimensions, value distributions, positional encoding, and tokenization, so they cannot be directly compared or fed to the LLM; a projection/connector unifies them.
83. **Describe ViT's tokenization process.** → It mimics textual tokens by cutting the image into patches (each a "word"), flattening them, and applying a projection so a regular Transformer Encoder can process them as tokens.
84. **Walk through wav2vec 2.0's training.** → (1) Masked language modeling — randomly masks input for the Transformer to predict; (2) quantized embeddings — a codebook reduces the number of embeddings; (3) contrastive learning — similar inputs close, dissimilar far.
85. **Why is Whisper popular as an MLLM audio encoder?** → Its audio embeddings have already had "exposure" to the textual modality (like CLIP for images), and only its Encoder is used (the LLM generates).
86. **Contrast ViViT and TimeSformer.** → ViViT uses separate Transformer Encoders for spatial vs temporal information (uniform frames or tubelets). TimeSformer uses a single Transformer with two attention mechanisms (Time Attention and Space Attention) — Divided Space-Time Attention.
87. **Explain the "just CLIP" video approach and its rationale.** → Skip the explicit temporal encoder; treat video as a sequence of frame embeddings like text, since decoder LLMs already handle temporal dimensions in text; the connector adds temporal data or converts embeddings.
88. **How does ImageBind achieve emergent alignment?** → It trains only pairs involving images/video (with linear projections); because many modalities are processed through image encoders, unseen pairs (e.g., audio-text) end up aligned in the shared embedding space.
89. **Describe LLaVA's two-step training and why each step exists.** → Step 1 pre-trains only the projection layer (ViT + LLM frozen) to align embeddings to what Vicuna expects. Step 2 fine-tunes projection + LLM (ViT frozen) so the LLM learns to reason over text/image pairs.
90. **Compare the Q-Former's output to a projection-based connector's output.** → Projection passes (projected) all ViT features through; the Q-Former compresses visual tokens into a fixed, smaller number of learned query vectors containing the most relevant information.
91. **Explain Flamingo's gated cross-attention and its purpose.** → Layer outputs are multiplied by tanh(α) with α learnable per layer, initialized to 0, so the model behaves exactly like the pretrained LLM at initialization; α opens the gate during training, improving stability and final performance.
92. **Why might a query-based connector skip over details?** → It compresses input to a fixed number of queries and lacks the fine-grained representation of projection-based connectors.
93. **Why does the projection-based connector risk filling the context window?** → The input is not compressed, so it can produce a large number of tokens.
94. **What are the two TinyAgent changes needed for image understanding?** → (1) Add `image_data` to the `run` function; (2) add `image_data` to `self.memory.add`. The Memory class wraps it as an `image_url` (URL or base64) plus text.
95. **Why support both URL and base64 images in MultimodalMemory?** → Different inference engines handle images differently — some convert images themselves, others can only process base64-encoded images.

## Part 3: Fill-in-the-Blank

96. "MLLMs are LLMs that can process and/or ______ more types of inputs (modalities) than text." → generate
97. "The three main components of an MLLM are ______, ______, and ______, with an optional ______." → Encoder; Connector; LLM; Generator
98. "Most MLLMs focus on ______ information rather than generating it." → encoding
99. "The text encoder consists of the ______ and the ______ layer." → tokenizer; embedding
100. "The Vision-Transformer creates ______ of images, each considered a ______." → patches; "word"
101. "CLIP stands for ______." → Contrastive Language-Image Pre-training
102. "Contrastive learning makes similar pairs have ______ similarity and dissimilar pairs have ______ similarity." → high; low
103. "wav2vec 2.0 processes ______ parts of the audio with a ______ before a Transformer Encoder." → strided; CNN
104. "______ replaces wav2vec 2.0's quantization step with a clustering algorithm." → HuBERT
105. "Whisper converts audio to a ______, an image of sound." → spectrogram
106. "Whisper uses the tokens ______ and ______ to select transcription vs translation tasks." → TRANSCRIBE; TRANSLATE
107. "CLAP stands for ______." → Contrastive Language-Audio Pre-training
108. "A ______ is a 3D patch from consecutive frames capturing spatial and temporal info." → tubelet
109. "TimeSformer's two attention mechanisms are ______ and ______, together called ______." → Time Attention; Space Attention; Divided Space-Time Attention
110. "______ uses positional embeddings to add spatial and temporal information to image embeddings." → VideoMamba
111. "ImageBind uses ______ as the basis for contrastive learning." → images (the visual modality)
112. "ImageBind demonstrated ______ alignment of unseen modality pairs like (Audio, Text)." → emergent
113. "The three connector types are ______, ______, and ______." → projection-based; query-based; fusion-based
114. "LLaVA made ______, a Llama 2 variant, multi-modal." → Vicuna
115. "The Q-Former was introduced in ______ and used in ______." → BLIP-2; Qwen-VL
116. "The Q-Former's stage-1 tasks are ______, ______, and ______." → image-text contrastive learning; image-grounded text generation; image-text matching
117. "Flamingo uses a ______ to map image embeddings to a fixed sequence size, and ______ cross-attention gated by ______." → perceiver resampler; gated; tanh(α)
118. "Gemma 4 E4B has a ______ encoder for images and an ______ encoder for sound." → vision; audio
119. "In Gemma 4 E4B, more vision tokens mean the image is processed at ______ resolution." → higher
120. "In TinyAgent, image input is enabled in the ______ class by adding ______ to the message content." → Memory; image_url
