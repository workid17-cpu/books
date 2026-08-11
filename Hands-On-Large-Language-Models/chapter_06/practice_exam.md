# 📘 Practice Exam — Chapter 6: Prompt Engineering
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 6
**Instructions:** Allow ~40–50 minutes. Sections A and B are quick checks; C is short answer; D is essay. Answers at the end — no peeking.

---

## Section A: Multiple Choice (1 point each)

1. Which model is used as the text generation model in this chapter?
   a) GPT-4
   b) google/flan-t5-small
   c) microsoft/Phi-3-mini-4k-instruct
   d) cardiffnlp/twitter-roberta-base

2. How many parameters does Phi-3-mini have?
   a) 3.8 billion
   b) 7 billion
   c) 13 billion
   d) 384 million

3. The book focuses on which type of model and why?
   a) Proprietary — better performance
   b) Open source — more flexibility and free to use
   c) Proprietary — easier to deploy
   d) Open source — more guardrails

4. What does `do_sample=False` do?
   a) Enables temperature and top_p
   b) Randomly samples tokens
   c) Selects only the most probable next token (deterministic)
   d) Disables the tokenizer

5. Which parameters require `do_sample=True` to be used?
   a) max_new_tokens and return_full_text
   b) device_map and torch_dtype
   c) n_gpu_layers and n_ctx
   d) temperature and top_p

6. A temperature of 0 produces:
   a) The same response every time
   b) The most creative output
   c) A completely random response
   d) Invalid JSON

7. top_p is also known as:
   a) Greedy sampling
   b) Top-k sampling
   c) Nucleus sampling
   d) Temperature sampling

8. With `top_p=0.1`, the LLM will consider tokens:
   a) Until their cumulative probability reaches 0.1
   b) For exactly 10 tokens
   c) Until their cumulative probability reaches 1
   d) None of the tokens

9. top_k controls:
   a) The cumulative probability of tokens
   b) The temperature
   c) The number of examples
   d) Exactly how many tokens the LLM can consider

10. Which Table 6-1 combination produces creative output that remains coherent?
    a) High temperature / High top_p (Brainstorming)
    b) Low temperature / Low top_p (Email generation)
    c) High temperature / Low top_p (Creative writing)
    d) Low temperature / High top_p (Translation)

11. Which special token indicates when the model should stop generating?
    a) <|user|>
    b) <|assistant|>
    c) <|end|>
    d) <s>

12. The two components of a basic instruction prompt are:
   a) The instruction and the data it refers to
   b) Persona and tone
   c) Format and audience
   d) Context and data

13. Which technique helps prevent the model from generating a full sentence in classification, forcing "negative"/"positive" only?
    a) Output indicators (e.g., "Text:" and "Sentiment:")
    b) Chain prompting
    c) Tree-of-thought
    d) Temperature 0.8

14. LLMs tend to forget information in the ______ of a long prompt.
   a) output indicators
   b) beginning
   c) end
   d) middle

15. Which prompting aspect is arguably the most important?
    a) Order
    b) Hallucination
    c) Specificity
    d) Tone

16. How is hallucination impact reduced in a prompt?
    a) Set temperature to 1
    b) Ask the model to only answer if it knows, else "I don't know"
    c) Use top_p=0.1
    d) Provide more data

17. "You are an expert in astrophysics" is an example of which prompt component?
    a) Instruction
    b) Tone
    c) Format
    d) Persona

18. Which component describes the target of the generated text (e.g., "Explain it like I'm 5")?
    a) Audience
    b) Context
    c) Data
    d) Format

19. In-context learning with a single example is called:
    a) Zero-shot
    b) One-shot
    c) Few-shot
    d) Multi-shot

20. In-context learning with two or more examples is called:
    a) Zero-shot
    b) One-shot
    c) Few-shot
    d) Chain-shot

21. Chain prompting breaks the problem:
    a) Within a single prompt
    b) Between prompts (output of one = input of next)
    c) By adding more output indicators
    d) By lowering temperature

22. A benefit of making two chained calls instead of one:
   a) Each call can use different parameters
   b) No examples needed
   c) Deterministic output guaranteed
   d) Lower VRAM usage

23. Which is NOT a use case of chain prompting mentioned in the chapter?
    a) Response validation
    b) Parallel prompts (merged with a final pass)
    c) Writing stories by breaking down the problem
    d) Translating between languages

24. System 1 thinking is:
    a) Conscious, slow, and logical
    b) Automatic, intuitive, and near-instantaneous
    c) A form of chain-of-thought
    d) The same as tree-of-thought

25. LLM "reasoning" is generally considered:
   a) True logical reasoning
   b) System 1 and 2 switching automatically
   c) Impossible to mimic
   d) Emergent behavior from memorization and pattern matching

26. Chain-of-thought prompting provides:
   a) Output indicators
   b) Multiple temperature values
   c) A JSON grammar
   d) Examples demonstrating reasoning ("thoughts") before the response

27. The common zero-shot chain-of-thought phrase is:
    a) "Explain it like I'm 5"
    b) "Think carefully"
    c) "Let's think step-by-step"
    d) "Show your work"

28. Which is an alternative zero-shot CoT phrase mentioned in the chapter?
    a) "Take a deep breath and think step-by-step"
    b) "You are an expert"
    c) "Be more specific"
    d) "Answer directly"

29. Self-consistency works by:
   a) Using one sample with high temperature
   b) Pruning the lowest-rated thoughts
   c) Voting between experts in one prompt
   d) Asking the same prompt multiple times and taking the majority result

30. Self-consistency becomes ______ slower as the number of samples increases.
    a) 2 times
    b) n times
    c) 10 times
    d) never

31. Tree-of-thought at each step:
   a) Generates one reasoning path
   b) Stops generating
   c) Removes all intermediate thoughts
   d) Explores different solutions, votes for the best, and continues

32. The disadvantage of tree-of-thought is:
    a) It cannot handle math
    b) It requires many calls to the model (slower)
    c) It needs labeled data
    d) It only works with GPT-4

33. Zero-shot tree-of-thought emulates:
    a) A single expert's monologue
    b) A conversation between multiple experts reaching consensus
    c) Chain-of-thought examples
    d) A JSON grammar

34. Which is NOT a reason for validating output?
    a) Structured output
    b) Valid output
    c) Ethics
    d) Increasing temperature

35. The three ways to control a generative model's output are:
    a) Examples, grammar, fine-tuning
    b) Temperature, top_p, top_k
    c) Prompt, tokenizer, pipeline
    d) Persona, tone, data

36. The zero-shot JSON character profile output was:
    a) Perfectly formatted
    b) Truncated and not valid JSON
    c) Valid but too short
    d) A Python dictionary

37. Which package is used in the chapter to apply a JSON grammar?
    a) Guidance
    b) LMQL
    c) llama-cpp-python
    d) Guardrails

38. The GGUF model loaded had repo_id:
    a) microsoft/Phi-3-mini-4k-instruct-gguf
    b) microsoft/Phi-3-mini-4k-instruct
    c) google/flan-t5-small
    d) sentence-transformers/all-mpnet-base-v2

39. `n_gpu_layers=-1` in llama-cpp-python means:
    a) No layers on GPU
    b) One layer on GPU
    c) All layers of the model run from the GPU
    d) Layers are quantized

40. Which parameters still affect constrained sampling?
    a) n_gpu_layers and n_ctx
    b) top_p and temperature
    c) repo_id and filename
    d) return_full_text and max_new_tokens

---

## Section B: True/False (1 point each)

41. Proprietary models are generally more performant than open source models. (T/F)
42. Scaling down to larger models is a nicer experience than scaling up. (T/F)
43. `do_sample=False` means only the most probable next token is selected. (T/F)
44. A higher temperature generally results in more deterministic output. (T/F)
45. top_p=1 considers all tokens. (T/F)
46. In Table 6-1, Creative writing uses High temperature and Low top_p. (T/F)
47. The <|user|> and <|assistant|> tokens were used during the training of the model. (T/F)
48. A prompt cannot be more complex than instruction + data + output indicators. (T/F)
49. Few-shot prompts use a single example. (T/F)
50. In chain prompting, the output of one prompt is used as input for the next. (T/F)
51. LLM reasoning is considered true logical reasoning by the authors. (T/F)
52. Zero-shot chain-of-thought requires reasoning examples in the prompt. (T/F)
53. Self-consistency asks the model the same prompt multiple times and takes the majority result. (T/F)
54. Tree-of-thought requires only a single call to the generative model. (T/F)
55. The one-shot JSON template produced valid, well-structured JSON. (T/F)

---

## Section C: Short Answer (2–3 points each)

56. Why does the book use open source models, and which model is chosen?
57. Explain how temperature, top_p, and top_k control output, and the four Table 6-1 combinations.
58. What is the chat template, and what do <|user|>, <|assistant|>, and <|end|> do?
59. Describe the basic components of an instruction prompt and how output indicators work.
60. What are the seven advanced prompt components, and what is the role of each?
61. Explain specificity, hallucination mitigation, and prompt ordering (primacy/recency effects).
62. Define zero-shot, one-shot, and few-shot prompting, and describe the made-up-word example.
63. How does chain prompting work, and what are its benefits and use cases?
64. Explain chain-of-thought, zero-shot chain-of-thought, and self-consistency.
65. How is output verification done via examples and grammar (constrained sampling)?

---

## Section D: Essay / Applied (5 points each)

66. **Controlling generation.** Explain how the chapter controls a generative model's output via sampling parameters: `do_sample`, temperature (0 → deterministic, higher → diverse), top_p/nucleus sampling (cumulative probability), and top_k (exact token count). Include the Table 6-1 use cases (Brainstorming, Email generation, Creative writing, Translation) and why temperature introduces stochastic behavior.
67. **The anatomy of a prompt.** Describe the basic ingredients of a prompt (LLM as prediction machine; instruction + data; output indicators like "Text:"/"Sentiment:"), instruction-based prompting, the prompting techniques (specificity as most important, hallucination mitigation, ordering with primacy/recency effects), and the seven advanced components (persona, instruction, context, format, audience, tone, data) using the complex summary-prompt example. Discuss the modular, iterative, experimental nature of prompting.
68. **In-context learning and chain prompting.** Explain in-context learning (zero-shot/one-shot/few-shot), the user/assistant role differentiation in the chat template (made-up-word Gigamuru/screeg example), and chain prompting (product features → name+slogan → sales pitch). Include benefits (per-call parameters), use cases (response validation, parallel prompts, story writing), and how chain prompting foreshadows reasoning techniques.
69. **Reasoning with generative models.** Contrast system 1 vs system 2 thinking and how prompt engineering mimics system 2. Explain chain-of-thought (reasoning examples, more compute over reasoning, tennis-ball/cafeteria examples), zero-shot chain-of-thought ("Let's think step-by-step" and alternatives), self-consistency (multiple samples, majority vote, n× slower), and tree-of-thought (explore/vote/prune; disadvantage of many calls) plus its zero-shot multi-expert variant.
70. **Output verification and control.** Explain why output verification matters in production (structured output, valid output, ethics, accuracy), the three ways to control output (examples, grammar, fine-tuning — with fine-tuning deferred to Chapter 12), the zero-shot vs one-shot JSON example (truncation problem solved by format templates), grammar packages (Guidance, Guardrails, LMQL), and the llama-cpp-python JSON grammar example (GGUF loading, n_gpu_layers=-1, n_ctx, response_format={"type": "json_object"}).

---

## ANSWER KEY

### Section A: Multiple Choice
1. c
2. a
3. b
4. c
5. d
6. a
7. c
8. a
9. d
10. c
11. c
12. a
13. a
14. d
15. c
16. b
17. d
18. a
19. b
20. c
21. b
22. a
23. d
24. b
25. d
26. d
27. c
28. a
29. d
30. b
31. d
32. b
33. b
34. d
35. a
36. b
37. c
38. a
39. c
40. b

### Section B: True/False
41. **T** — Proprietary models are generally more performant than open source ones.
42. **F** — Scaling UP is a nicer experience than scaling down.
43. **T** — do_sample=False selects only the most probable next token.
44. **F** — A HIGHER temperature gives more diverse (less deterministic) output.
45. **T** — top_p=1 considers all tokens.
46. **T** — Creative writing = High temperature / Low top_p.
47. **T** — The chat template tokens were used during training.
48. **F** — Prompts can be built up to be as complex as you want (no limit to three components).
49. **F** — Few-shot uses two or more examples; a single example is one-shot.
50. **T** — Chain prompting uses one prompt's output as the next prompt's input.
51. **F** — The authors consider it emergent behavior via memorization/pattern matching, not "true" reasoning.
52. **F** — Zero-shot CoT uses no examples (e.g., "Let's think step-by-step").
53. **T** — Self-consistency takes the majority result over multiple samples.
54. **F** — Tree-of-thought requires many calls; the zero-shot variant needs only one.
55. **T** — The one-shot JSON template produced valid, well-structured JSON.

### Section C: Short Answer (model answers)
56. **Open source choice.** The book uses open source models because they offer more flexibility and are free to use (proprietary are generally more performant). The chosen model is Phi-3-mini (3.8 billion parameters, fits devices up to 8 GB VRAM) — start small; scaling up is nicer than down.
57. **Sampling parameters.** do_sample=False → greedy (most probable token); True enables temperature/top_p. Temperature controls randomness (0 = same response every time; higher = more diverse; lower = deterministic). top_p (nucleus sampling) considers tokens until cumulative probability reaches top_p (0.1 → small nucleus; 1 → all tokens). top_k limits to exactly k tokens. Table 6-1: Brainstorming High/High (diverse, creative); Email Low/Low (predictable, conservative); Creative writing High/Low (creative but coherent); Translation Low/High (coherent with wider vocabulary).
58. **Chat template.** The template used during training with special tokens: <|user|> and <|assistant|> tell the model who said what; <|end|> indicates when to stop generating; <s> starts the sequence. Inspect via `pipe.tokenizer.apply_chat_template(messages, tokenize=False)`.
59. **Basic prompt.** A basic instruction prompt = instruction + data it refers to. Output indicators (e.g., prefix "Text:" and add "Sentiment:") constrain output to "negative"/"positive" instead of a full sentence; the model generalizes to this structure even without being trained on it directly.
60. **Seven components.** Persona (role, e.g., "expert in astrophysics"); Instruction (the task, specific); Context (background, reason for instruction); Format (output format; avoids arbitrary formats in automated systems); Audience (target/level, e.g., ELI5); Tone (voice, e.g., formal); Data (the main task data).
61. **Techniques.** Specificity = accurately describe what you want (most important — restricts output, reduces off-target generation). Hallucination mitigation = ask the model to answer only if it knows, else "I don't know." Ordering = place the instruction at the beginning (primacy) or end (recency); middle info is often forgotten.
62. **In-context learning.** Zero-shot = 0 examples; one-shot = 1; few-shot = 2+. Made-up-word example: user defines "Gigamuru", assistant gives an example sentence, user defines "screeg"; the model generates a correct sentence — showing the value of differentiating user/assistant roles.
63. **Chain prompting.** Output of one prompt becomes input of the next (features → name+slogan → sales pitch). Benefits: LLM spends more time per question; each call can use different parameters. Use cases: response validation, parallel prompts (final merge pass), writing stories.
64. **Reasoning methods.** CoT = provide reasoning examples ("thoughts") so the model thinks before answering; each reasoning token stabilizes output. Zero-shot CoT = no examples, append "Let's think step-by-step" (alternatives: "Take a deep breath...", "Let's work through..."). Self-consistency = sample the same prompt multiple times (vary temperature/top_p), take majority result; n× slower; can combine with CoT.
65. **Output verification.** Examples: few-shot templates guide structure (one-shot JSON template produced valid JSON; zero-shot was truncated/invalid). Grammar: constrained sampling defines rules for token selection (only "positive"/"neutral"/"negative", or JSON via response_format); packages: Guidance, Guardrails, LMQL; llama-cpp-python with GGUF and `response_format={"type": "json_object"}`. Fine-tuning is deferred to Chapter 12.

### Section D: Essay (grading notes)
66. **Expect** do_sample=False (greedy/deterministic) vs True (sampling); temperature 0 → same response always, higher → less-probable tokens (diverse), stochastic behavior (output changes each run); top_p nucleus sampling (tokens until cumulative probability; 1 = all); top_k exact token count; Table 6-1 combos (Brainstorming High/High, Email Low/Low, Creative writing High/Low, Translation Low/High) with descriptions.
67. **Expect** LLM as prediction machine; instruction + data; output indicators (Text:/Sentiment:) forcing "negative"/"positive"; instruction-based prompting; specificity (most important), hallucination mitigation ("I don't know"), ordering (primacy/recency; middle forgotten); seven components (persona, instruction, context, format, audience, tone, data); complex summary-prompt code (variables concatenated into query); modularity, iteration, experimentation; prompts vary by model.
68. **Expect** in-context learning definition (show vs describe; Brown et al.); zero/one/few-shot counts; user/assistant role differentiation (Gigamuru/screeg example; template with alternating roles); chain prompting (between prompts; features → name+slogan "MindMeld Messenger" → sales pitch); benefits (more time per question, per-call parameters like token counts); use cases (response validation, parallel prompts/merge, story writing with summary/characters/beats/dialogue); leads to reasoning techniques.
69. **Expect** system 1 (automatic/intuitive/instant) vs system 2 (conscious/slow/logical; brainstorming/self-reflection); emulating system 2 via prompting; "reasoning" as emergent behavior from memorization + pattern matching (not "true" reasoning); CoT (reasoning examples/thoughts; more compute over reasoning; tennis-ball 5+2×3=11 and cafeteria 23−20+6=9 examples; explanation before answer); zero-shot CoT ("Let's think step-by-step"; alternatives); self-consistency (same prompt multiple times, majority vote, vary temperature/top_p, n× slower, CoT combined); ToT (break into steps, explore solutions, vote, prune lowest; many calls disadvantage); zero-shot ToT (multi-expert consensus conversation).
70. **Expect** production robustness; four reasons (structured output/JSON, valid output/no third option, ethics/no profanity-PII-bias-stereotypes, accuracy/no hallucination); three control ways (examples, grammar, fine-tuning — Ch 12); zero-shot JSON truncated/invalid vs one-shot template (description/name/armor/weapon) valid; grammar packages Guidance/Guardrails/LMQL; LLM self-validation against guardrails; constrained sampling during token selection (still affected by top_p/temperature); llama-cpp-python (GGUF, quantized models; clear VRAM via del/gc.collect()/torch.cuda.empty_cache(); Llama.from_pretrained repo_id microsoft/Phi-3-mini-4k-instruct-gguf, filename *fp16.gguf, n_gpu_layers=-1, n_ctx=2048; response_format={"type": "json_object"}, temperature=0; json.dumps(json.loads(output), indent=4)).

### Scoring Guide
- Section A: 40 pts | Section B: 15 pts | Section C: ~20 pts (choose any ~5) | Section D: 20–25 pts.
- **85–100%**: Strong. Review only missed items.
- **70–84%**: Good. Re-read study notes for weak areas (likely the sampling parameters/Table 6-1, prompt components, or the reasoning techniques).
