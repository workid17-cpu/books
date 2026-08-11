# Comprehensive Study Notes — Chapter 9
**Source:** *An Illustrated Guide to AI Agents*, Chapter 9 "Multi-Modal Understanding"

---

## CHAPTER 9 — MULTI-MODAL UNDERSTANDING

### 9.1 The Big Picture

- Real-world agentic applications are **not limited to text-based information**. An agent may need to process a video, judge a website design from an image, or listen to a spoken request.
- **Multi-modal understanding** = understanding different types of information — not just written text, but also images, audio, and videos. When you watch a video and read the caption, your brain combines all those signals into one clear understanding; MLLMs do something similar.
- **Multi-modal LLMs (MLLMs)** take in multiple kinds of input (different modalities) and process them together to form a more accurate response.
- An agent that only reads/writes text will miss important parts of everyday situations (e.g., developing an app UI, or an agentic browser needing to process images and text of websites).
- Some MLLMs can not only process but also **output** different modalities (e.g., text + audio or images) — a model could "speak" to you or generate stock images.
- MLLMs are important in single- and multi-agent systems (MAS): since they process all kinds of information, they are often the main "brain" and are more capable of delegating tasks.
- Examples: **GPT-5, Google Gemini 3.0, Claude 4.5 family** — they process text, images, audio, and reason about them.

### 9.2 What Are Multi-Modal LLMs?

- **MLLM** = LLM that can process and/or generate more types of inputs (modalities) than text. Common modalities: **text, image, audio, video** (Figure 9-1).
- **Most MLLMs focus on encoding information (input) rather than generating information (output)** — encoding is cheaper and doesn't require large architectural changes (Figure 9-2). These input modalities are used to **guide textual generation** — contextual information for a given task.
- Example: an MLLM agent building a website could "see" the website it's optimizing (Figure 9-3).

#### Three main components (with an optional fourth) of an MLLM (Figure 9-4)
1. **Encoder** — encodes a modality (images, audio, etc.) to features (embeddings).
2. **Connector** — converts encoded features so the LLM can use them.
3. **LLM** — (pre-trained) model that processes encoded features and generates text.
4. **(Optional) Generator** — generates modalities aside from text.

- Creating an MLLM generally involves **separately training a base LLM, then adding multi-modality to it**.
- Chapter focus: **multi-modal understanding (input modalities)**, not multi-modal generation (output).

### 9.3 Encoding Modalities

- Encoding = converting a given input (text, image, audio, etc.) to **embeddings** (Figure 9-5). Vital because **the LLM expects embeddings as input** (Figure 9-6).
- The text modality is encoded inside the LLM itself (tokenizer + embedding layer); other modalities need external encoders.

#### Text
- LLM architecture: a **tokenizer** splits text into pieces (**tokens**), each embedded before being passed to a **decoder-only LLM** (Figure 9-7).
- Main encoding process of text = **tokenizer + embedding layer** (Figure 9-8). Token embeddings are the LLM's main input.
- This is one of the primary places we add other modalities: **by encoding other modalities into embeddings, the LLM now has the same type of information to process** (Figure 9-9).
- The tokenizer + embedding layer can be seen as the **text encoder** (Figure 9-10).
- **Problem:** embeddings from different modalities differ in structure and dimensions (Figure 9-11) — we need a model to **project embeddings onto a common set of characteristics** (same dimensions, similar value distributions).

#### Images
- **Vision-Transformer (ViT)** — an adaptation of the Transformer for images:
  - First step mimics textual tokens: creates **patches** of images, each patch considered a "word" (Figure 9-12).
  - After "tokenization", it **flattens the patches and applies a projection** so a regular Transformer Encoder processes them like tokens (Figure 9-13).
  - Resulting embeddings used like regular embeddings (classification, clustering, search, etc.).
- **CLIP (Contrastive Language-Image Pre-training)** — arguably the most used image encoder; encodes **both text and images**:
  - Uses ViT together with a regular transformer encoder to embed both images and text.
  - **Contrastive learning** on labeled similar/dissimilar image-text pairs aligns embeddings of both modalities (Figure 9-14).
  - Result: similar pairs → high embedding similarity; dissimilar pairs → low similarity (Figure 9-15).
  - **Why CLIP is popular for MLLMs:** image embeddings have already had "exposure" to the textual modality.

#### Audio
- **wav2vec 2.0** — one of the first successful audio encoders:
  - Samples **overlapping (strided) parts** of the audio to create tokens (strided waveforms) (Figure 9-16).
  - Strided parts processed by a **CNN** to create features, then a regular **Transformer Encoder** creates contextualized features (Figure 9-17).
  - Training: (1) **masked language modeling** (randomly masks input to predict), (2) **quantized embeddings** (codebook/vocabulary to reduce number of embeddings), (3) **contrastive learning** (similar inputs close, dissimilar far) (Figure 9-18).
- **HuBERT** — popular extension; **replaces the quantization step with a clustering algorithm** to create features for contrastive learning.
- **Whisper** — popular audio encoder in LLMs, for **automatic speech recognition (ASR)**:
  - **Encoder-decoder architecture**: encoder represents audio, decoder generates transcription.
  - Audio converted to a **spectrogram** (an image of sound showing pitch/loudness over time); features extracted via convolutional layers (Figure 9-19).
  - Trained on tasks (multilingual speech recognition, translation) each with its own tokens: "TRANSCRIBE", "TRANSLATE", plus language and time tokens.
  - In MLLMs, **typically only the Encoder is used** (LLM handles the generative part) (Figure 9-20). Popular because audio embeddings have "exposure" to text.
- **CLAP (Contrastive Language-Audio Pre-training)** — based on CLIP but for audio:
  - Audio waveforms processed based on chunk duration d (e.g., 10s): if shorter, repeat + zero-pad; if longer, downsample to 10s then randomly sample three 10-second slices (Figure 9-21).
  - Authors found **HTS-AT (Hierarchical Token Semantic-Audio Transformer)** worked best: converts audio to spectrogram, extracts patches as "tokens", processed by several hierarchical Transformer models to downscale embeddings (Figure 9-22).
  - Uses contrastive learning on **audio/text pairs** (captions or descriptions).

#### Video
- Significant overlap with images, but with a **temporal dimension** — a sequence of images. May also include audio and/or subtitles (Figure 9-23).
- **Main challenge: how to model the temporal dimension.**
- **Basic version:** use ViT/CLIP, sample frames, embed, **average** them (Figure 9-24). But averaging away the temporal dimension prevents understanding of how the video started and ended.
- **ViViT (Video Vision Transformer)** — early adaptation:
  - Two token-extraction methods: (1) **uniform frame sampling** — frames sampled and passed through ViT to create tokens per patch (Figure 9-25); (2) **tubelet embeddings** — a tubelet is a block of pixels from consecutive frames forming a **3D patch** capturing image content + changes over time (Figure 9-26).
  - Tokens passed to Transformer Encoders primarily processing **spatial** information (Figure 9-27).
- **TimeSformer** — separates temporal and spatial dimensions in a **single** Transformer:
  - Frames sampled and split into patches, flattened, passed directly to a Transformer (Figure 9-28).
  - Applies two attention mechanisms: **Time Attention** (creates tubelets along the time dimension to capture motion patterns) and **Space Attention** (patches within individual frames for spatial structures/object relationships) (Figure 9-29).
  - Called **Divided Space-Time Attention** — better representations than Space Attention alone or ViT (Figure 9-30).
  - Note: all video techniques share underlying principles: sample frames, create patch tokens, embed, use as token embeddings in a Transformer Encoder.
- Other examples: **VideoBERT, Frozen in Time, VideoMAE**.
- **"Just" CLIP approach:**
  - Redundancy: two Transformer Encoders before the LLM. **Skip the second** and let the **LLM figure out temporal dimensions** with the connector doing a bit more processing (Figures 9-31, 9-32).
  - Idea: decoder LLMs are good at temporal dimensions in text; videos can be seen as a sequence of tokens per frame (Figure 9-33).
  - Pass sampled frames as embeddings, **as a sequence (like text) to the LLM**; connector adds temporal data or converts embeddings (Figure 9-34).
  - **VideoMamba** example: uses **positional embeddings** to "enhance" image embeddings — adds **spatial** info (which part of frame) then **temporal** info (order of frames) (Figure 9-35).

#### Many-Modality: ImageBind
- Problem: modalities typically encoded in isolation/pairs → need different models per modality, and embeddings cannot be compared (Figure 9-36).
- **ImageBind** — finds a **singular embedding space** for many modalities:
  - Combines contrastive learning with the idea that **a single image can bind different modalities** (an image of a rainy park evokes sound of waves, cold mist, quiet mood).
  - Many modalities are processed through the lens of images (video and audio encoders use image encoders) (Figure 9-37).
  - **Primary objective: use images as the basis for contrastive learning** — one modality of each pair is always visual (images or video).
  - For each pair, a **linear projection** aligns embeddings across pairs (Figure 9-38).
  - Training pairs: (Image, Text), (Image, Depth Sensor Data), (Image, Thermal Data), (Video, IMU/accelerometer), (Video, Audio).
  - **Emergent alignment:** unseen pairs (e.g., Audio, Text) were aligned in the embedding space despite not being trained together (Figure 9-39) — showing the strength of images as a "binding" factor.

### 9.4 Connecting Modalities (Connectors)

- Problem: each modality's encoder creates embeddings differing in size, dimensions, embedding space, positional encoding, and tokenization from what the LLM expects. A **connector** transforms those embeddings into the same type as the LLM's token embeddings (Figure 9-40).
- **Three types of connectors (Figure 9-41):**
  1. **Projection-based** — a Multi-Layer Perceptron (MLP) projects multi-modal embeddings into an embedding size compatible with the LLM.
  2. **Query-based** — learnable groups of query tokens extract information in a query-based manner.
  3. **Fusion-based** — additional (cross-attention) layers added in the LLM to process both text and other multi-modal embeddings.

#### Projection-based Connector
- Most straightforward: **project the multi-modal embeddings** into the LLM's token-embedding space, typically with a **single linear layer** (Figure 9-42).
- Can project **up** (fewer→more values) or **down** (more→fewer values) depending on embedding sizes.
- Projected embeddings are **concatenated with the token embeddings** of text input; the LLM treats every embedding as if it were a token embedding (Figure 9-43).
- **Example: LLaVA (Large Language and Vision Assistant):**
  - Makes **Vicuna** (a Llama 2 variant, text-only) multi-modal; chosen because at the time (2023) it had the best instruction-following capabilities.
  - Created in **two steps** (Figure 9-44):
    - **Step 1 — Pre-training for feature alignment:** only the projection layer is trained; ViT and LLM weights are **frozen**.
    - **Step 2 — Fine-tuning end-to-end:** projection layer + LLM trained; **only ViT weights frozen**.
  - Step 1 aligns embedding types to what Vicuna expects; Step 2 lets the LLM learn to reason about text/image pairs.
  - Resulted in state-of-the-art multi-modal chat at the time; made creating MLLMs straightforward.
  - **Still used for recent architectures: Qwen2.5-VL, Phi-4-multimodal, Janus.** Can be used for any modality, not just images.

#### Query-based Connector
- More involved: extracts **relevant features** from all non-textual inputs before feeding the LLM (Figure 9-45). These learnable features are called **"queries"**, extracted by the **Q-Former**.
- Instead of projecting all ViT features, the Q-Former creates a **fixed number of features** the LLM can process — a major benefit is learning **only the most relevant visual information**.
- **Introduced in BLIP-2** — an MLLM using a transformer model to process image + text features. During training, the transformer receives **learnable queries** that get "updated" with relevant visual and textual information.
- Two modes: one **with cross-attention** for processing images, one **without** for text (Figure 9-46). Trained with contrastive learning on image/text pairs.
- Output = learned queries containing the images' most relevant info — a way to **compress visual tokens** into fewer representation vectors.
- **Two-stage training:**
  - **Stage 1:** Q-Former trained on three tasks (so queries learn the visual representation most informative of the text):
    1. **Image-text contrastive learning** — aligns image/text representations by maximizing mutual information.
    2. **Image-grounded text generation** — trains Q-Former to generate text conditioned on images.
    3. **Image-text matching** — binary classification predicting whether image-text pairs match.
  - **Stage 2:** trained Q-Former connected to an **LLM (OPT)**; query embeddings projected through an **MLP** to match LLM input dimensions. **Only Q-Former and MLP are trained; LLM and image encoder frozen** (Figure 9-47).
- Used in **Qwen's first multi-modal LLM, Qwen-VL**. Works for any modality.

#### Fusion-based Connector
- Previous connectors don't change the LLM's architecture. **Fusion-based adds extra modules to the LLM's architecture** for deep interaction/fusion between text and non-text features.
- **Example: Flamingo:**
  - Uses pre-trained LLMs as basis, adds **trainable cross-attention layers** that process input images.
  - Images processed with a **vision encoder**; output processed through a **perceiver resampler** (a Transformer model, like the Q-Former) that maps image embeddings to a **fixed sequence size** (Figure 9-48).
  - Resampler output fed to **cross-attention layers added to the LLM**.
  - **Only the added cross-attention and perceiver resampler are trained; the pre-trained Vision Encoder and LLM are frozen** (Figure 9-49).
- **Perceiver resampler** — extracts the most important vision features at a fixed size (like Q-Former); learned queries match the shape expected by the LLM; fixed size reduces computation.
- Output embeddings passed to additional **cross-attention blocks intertwined with the LLM's regular blocks** — the LLM attends to both image and text features.
- **"Gated" cross-attention:** uses a gating trick so the conditioned model behaves identically to the original LLM at initialization. Layer outputs are multiplied by **tanh(α)**, where **α is a learnable scalar per layer initialized to 0**. Since tanh(0) = 0, these layers initially contribute nothing, preserving pretrained behavior; α learns to open the gate during training (Figure 9-50). Enhances training stability and final performance.
- **Drawback:** fusion-based requires adjusting the LLM's architecture → less flexible; but integrating attention into the architecture has potential for improved performance.

#### Advantages and Disadvantages (Table 9-1)
| Connector | Advantages | Disadvantages |
|---|---|---|
| **Projection-based** | Simple (lightweight to implement); computational efficiency (low latency, few parameters) | Scalability: can produce many tokens that quickly fill the LLM's context window; Performance: decoupled from LLM and small → limited expressive power |
| **Query-based** | Scalability: fixed number of queries allows scaling with large multi-modal inputs (e.g., videos); Performance: more expressive than projection | Complexity: relatively complex training procedures; Computational efficiency: larger models add more latency than projection |
| **Fusion-based** | Integration: LLM "sees" multi-modal features at every layer instead of just as a prefix; Performance: most expressive | Complexity: requires changes to the LLM's architecture; Computational efficiency: requires extensive fine-tuning, more than others |

- **Three axes:** performance, computational efficiency, scalability.
- Generally: **projection-based** → simplicity + computational efficiency (lightweight, easy to train) but may fill context window (input not compressed). **Query-based** → efficiency + scaling (constant input length regardless of image/video resolution) but may skip details (no fine-grained representation). **Fusion-based** → best performance (processing throughout the whole model, not just token embeddings) but most complex and expensive to train.

### 9.5 TinyAgent: Adding Multi-Modal Understanding

- The book's running model, **Gemma 4 E4B**, processes both images and audio — it has **two encoders**: a vision encoder and an audio encoder.
- **Vision tokens / image tokens:** the vision encoder generates embeddings (vision tokens) the LLM uses. In the raw chat template:
  - `<|image>` and `<image|>` = start and end of a sequence of image tokens.
  - `<|image|>` = a vision token.
  - Gemma 4 E4B typically uses a maximum of **70, 140, 280, 560, or 1,120 vision tokens**; more tokens = higher resolution processing.
- In practice, chat template processing is handled by the **inference engine** (as in Ch 5); no need to create templates with up to 1,120 vision tokens.
- The OpenAI endpoint expects content with **two fields**: one for the image (`image_url`) and one for the text. In some cases the URL must be the image itself in **base64** rather than a link (depends on the inference engine).
- **Enabling image inputs is done in the Memory class!** Messages function both as memory and as the way to communicate with the model.
- **MultimodalMemory** (subclass of ch4's Memory) — only minor change: add `image_url` to messages if there's an input image:
  - Adds an `image_data` parameter to `add()`.
  - If `image_data` starts with http/https → treated as URL; otherwise wrapped as `data:image/png;base64,{image_data}`.
  - Content becomes `[{"type": "image_url", "image_url": {"url": url}}, {"type": "text", "text": content}]`.
  - Supports both URL and base64 for different inference engines.
- **TinyAgent run function update:** add `image_data` parameter to `run()` and pass it to `self.memory.add("user", task, image_data=image_data)`. That's all needed to use image-understanding capabilities.
- **Demo:** asking "Which animal is on the cover of 'Hands-On Large Language Models'?" without an image → agent can't answer ("I do not have the ability to view specific commercial book covers..."). With the image downloaded and base64-encoded (`base64.b64encode(httpx.get(image_url).content).decode("utf-8")`) → "The animal on the cover of 'Hands-On Large Language Models' is a **kangaroo**."
- **What We Built:** TinyAgent/ with `agent.py` (updated — processes images), `memory.py` (updated — tracks images in messages), plus evaluator.py, llm.py, planning.py, toolbox.py, tools.py, trajectory.py.

### Chapter 9 Key Takeaways
- MLLMs process and/or generate more input/output modalities than text (text, image, audio, video).
- Most MLLMs focus on input encoding to guide text generation (cheaper, fewer architectural changes).
- MLLM components: Encoder, Connector, LLM, (optional) Generator.
- Encoding: text = tokenizer + embedding layer; images = ViT (patches) and CLIP (contrastive); audio = wav2vec 2.0, HuBERT, Whisper (ASR, encoder-decoder, spectrogram), CLAP; video = ViViT (tubelets), TimeSformer (divided space-time attention), "just CLIP" (LLM handles temporal); many-modality = ImageBind (image-binding, emergent alignment).
- Connectors: projection (simple/efficient but fills context window; LLaVA), query (Q-Former; BLIP-2, Qwen-VL; fixed queries), fusion (cross-attention in LLM; Flamingo with perceiver resampler + gated cross-attention).
- TinyAgent: adding `image_url` to Memory enables image understanding; base64 or URL support per inference engine.

---

## High-Yield Vocabulary (Ch 9)

| Term | Meaning |
|---|---|
| MLLM | Multi-modal LLM — processes and/or generates more modalities than text |
| Modality | A type of input/output (text, image, audio, video) |
| Multi-modal understanding | Processing multiple input modalities together to form accurate responses |
| Encoder | Encodes a modality to features (embeddings) |
| Connector | Converts encoded features so the LLM can use them |
| Generator | (Optional) generates modalities aside from text |
| Embedding | Numeric representation of an input token/feature |
| Tokenizer | Splits text into tokens for the embedding layer |
| ViT | Vision-Transformer — image encoder using patches as "words" |
| Patch | A piece of an image treated like a token by ViT |
| CLIP | Contrastive Language-Image Pre-training — aligns image/text embeddings |
| Contrastive learning | Trains embeddings so similar pairs are close, dissimilar far |
| wav2vec 2.0 | Audio encoder: strided waveforms + CNN + Transformer Encoder |
| HuBERT | wav2vec 2.0 variant replacing quantization with clustering |
| Whisper | ASR audio encoder (encoder-decoder, spectrogram-based) |
| Spectrogram | Image of sound showing pitch/loudness over time |
| CLAP | Contrastive Language-Audio Pre-training (HTS-AT encoder) |
| HTS-AT | Hierarchical Token Semantic-Audio Transformer |
| ViViT | Video Vision Transformer (uniform frames + tubelets) |
| Tubelet | 3D patch from consecutive frames capturing spatial + temporal info |
| TimeSformer | Single-Transformer video model with Divided Space-Time Attention |
| Divided Space-Time Attention | Separate Time Attention (motion) + Space Attention (structure) |
| VideoMamba | Uses positional embeddings to add spatial + temporal info to frames |
| ImageBind | Singular embedding space binding many modalities via images |
| Emergent alignment | Unseen modality pairs aligning without being trained together |
| Projection-based connector | MLP/linear layer projecting embeddings to LLM size (LLaVA) |
| Query-based connector | Learnable query tokens extracting features (Q-Former, BLIP-2) |
| Q-Former | Transformer extracting a fixed set of relevant visual features |
| Fusion-based connector | Cross-attention layers added inside the LLM (Flamingo) |
| Perceiver resampler | Maps image embeddings to a fixed sequence size (Flamingo) |
| Gated cross-attention | Cross-attention gated by tanh(α), α initialized to 0 |
| Image-text contrastive learning | Aligns image/text representations via mutual information |
| Image-grounded text generation | Generates text conditioned on images |
| Image-text matching | Binary classification of whether image-text pairs match |
| LLM-as-vision tokens | Image embeddings treated as token embeddings the LLM processes |
| Base64 image | Image encoded as a string for APIs that can't fetch URLs |
| Vision token | An image embedding token the LLM can process |
