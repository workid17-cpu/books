# 📘 Chapter 6 Flashcards: Prompt Engineering
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 6

---

## Part 1: Terms → Definitions

**Q:** What is a generative pre-trained transformer (GPT)?
**A:** A model trained primarily for text generation that can generate text in response to user prompts.

**Q:** What is prompt engineering?
**A:** Designing prompts to enhance the quality of generated text; also a tool for evaluating output and designing safeguards — an iterative process of prompt optimization requiring experimentation.

**Q:** What is a foundation model?
**A:** An LLM pretrained on vast amounts of text data, often fine-tuned for specific applications; often released in several sizes.

**Q:** What is a proprietary model?
**A:** A closed model (generally more performant), e.g., GPT-4.

**Q:** What is an open source model?
**A:** A freely usable model offering more flexibility; the book's focus.

**Q:** Why does the book focus on open source models?
**A:** They offer more flexibility and are free to use.

**Q:** Which model is used in this chapter and why?
**A:** Phi-3-mini (3.8 billion parameters) — small enough for devices up to 8 GB VRAM; starting small is advised (scaling up is nicer than scaling down).

**Q:** What is a chat template?
**A:** The prompt template with special tokens (<|user|>, <|assistant|>, <|end|>) used during training that indicates who said what and when to stop generating.

**Q:** What do the special tokens <|user|> and <|assistant|> do?
**A:** They provide information about who said what in the conversation.

**Q:** What does the <|end|> token do?
**A:** Indicates when the model should stop generating text.

**Q:** What is temperature?
**A:** Controls the randomness/creativity of generated text; defines how likely less-probable tokens are chosen. 0 = always the most likely word; higher = more diverse.

**Q:** What does a temperature of 0 produce?
**A:** The same response every time, because it always chooses the most likely word.

**Q:** What is top_p (nucleus sampling)?
**A:** A sampling technique controlling which subset of tokens (the nucleus) the LLM can consider — tokens are considered until their cumulative probability reaches top_p.

**Q:** What is top_k?
**A:** Controls exactly how many tokens the LLM can consider (e.g., 100 = only the top 100 most probable tokens).

**Q:** What does `do_sample=False` do?
**A:** Disables sampling — only the most probable next token is selected (deterministic/consistent output).

**Q:** What does `do_sample=True` do?
**A:** Enables sampling so temperature and top_p can be used.

**Q:** What is a prompt?
**A:** The input to an LLM (question, statement, or instruction) used to elicit a useful response.

**Q:** What is an instruction-based prompt?
**A:** A prompt that has the LLM answer a specific question or resolve a certain task.

**Q:** What is hallucination?
**A:** LLMs generating incorrect information confidently.

**Q:** How is hallucination mitigated in prompts?
**A:** Ask the LLM to only generate an answer if it knows the answer; otherwise respond with "I don't know."

**Q:** What is the primacy effect?
**A:** LLMs tend to focus on information at the beginning of a prompt.

**Q:** What is the recency effect?
**A:** LLMs tend to focus on information at the end of a prompt.

**Q:** Why does information in the middle of a long prompt get forgotten?
**A:** Because LLMs tend to focus on the beginning (primacy effect) or the end (recency effect) of a prompt.

**Q:** What is specificity in prompting?
**A:** Accurately describing what you want to achieve (e.g., "in less than two sentences and use a formal tone"); the most important aspect of prompt design.

**Q:** What is a persona component?
**A:** Describes what role the LLM should take on (e.g., "You are an expert in astrophysics").

**Q:** What is an instruction component?
**A:** The task itself; make it as specific as possible.

**Q:** What is a context component?
**A:** Additional information describing the context of the problem/task; answers "What is the reason for the instruction?"

**Q:** What is a format component?
**A:** The format the LLM should use to output text; without it, the LLM picks its own format (troublesome for automated systems).

**Q:** What is an audience component?
**A:** The target of the generated text; describes the output level (e.g., ELI5 = "Explain it like I'm 5").

**Q:** What is a tone component?
**A:** The tone of voice the LLM should use in the generated text (e.g., formal vs. informal).

**Q:** What is a data component?
**A:** The main data related to the task itself.

**Q:** What is in-context learning?
**A:** Providing the LLM with correct examples of exactly what we want to achieve, instead of describing the task (Brown et al., 2020).

**Q:** What is zero-shot prompting?
**A:** Prompting without leveraging any examples.

**Q:** What is one-shot prompting?
**A:** Prompting with a single example.

**Q:** What is few-shot prompting?
**A:** Prompting with two or more examples.

**Q:** What is chain prompting?
**A:** Breaking a problem up between prompts — taking the output of one prompt as input for the next, creating a chain of interactions.

**Q:** What is response validation (as a chain use case)?
**A:** Ask the LLM to double-check previously generated outputs.

**Q:** What are parallel prompts (as a chain use case)?
**A:** Create multiple prompts in parallel and do a final pass to merge them (e.g., multiple recipes → shopping list).

**Q:** What is system 1 thinking?
**A:** Automatic, intuitive, near-instantaneous process (Kahneman, 2011); resembles generative models generating tokens without self-reflection.

**Q:** What is system 2 thinking?
**A:** Conscious, slow, logical process; akin to brainstorming and self-reflection.

**Q:** What is chain-of-thought (CoT)?
**A:** A method that has the generative model "think" first — providing examples demonstrating reasoning ("thoughts") before the response (Wei et al., 2022).

**Q:** What is zero-shot chain-of-thought?
**A:** Asking the model to reason without examples, commonly via "Let's think step-by-step" (Kojima et al., 2022).

**Q:** What are two alternative zero-shot CoT phrases?
**A:** "Take a deep breath and think step-by-step" and "Let's work through this problem step-by-step."

**Q:** What is self-consistency?
**A:** Asking the generative model the same prompt multiple times and taking the majority result as the final answer (Wang et al., 2022); can add chain-of-thought, using only the answer for voting.

**Q:** What is the cost of self-consistency?
**A:** It becomes n times slower, where n is the number of output samples.

**Q:** What is tree-of-thought (ToT)?
**A:** Breaking a problem into pieces; at each step, the model explores different solutions, votes for the best, and continues (Yao et al., 2023).

**Q:** What is zero-shot tree-of-thought?
**A:** A single prompt emulating a conversation between multiple experts who question each other until they reach a consensus.

**Q:** Why does tree-of-thought slow down applications?
**A:** It requires many calls to the generative models.

**Q:** What is output verification?
**A:** Verifying/controlling model output to prevent breaking production applications and create robust generative AI applications.

**Q:** What are the four reasons for validating output?
**A:** Structured output (e.g., JSON), valid output (no third option), ethics (no profanity/PII/bias/stereotypes), and accuracy (factual, coherent, no hallucination).

**Q:** What are the three ways to control a generative model's output?
**A:** Examples, Grammar (constrained sampling), and Fine-tuning (Chapter 12).

**Q:** What is constrained sampling (grammar)?
**A:** Defining grammars/rules the LLM must adhere to when choosing its next token (e.g., only "positive"/"neutral"/"negative" for sentiment).

**Q:** What packages constrain/validate generative output?
**A:** Guidance, Guardrails, and LMQL.

**Q:** What is llama-cpp-python?
**A:** A library similar to transformers used to efficiently load compressed (GGUF/quantized) models; can also apply a JSON grammar.

**Q:** What is GGUF?
**A:** A model format generally used for compressed (quantized) models.

**Q:** What is quantization?
**A:** Compression of models (covered in detail in Chapter 12).

**Q:** What does `n_gpu_layers=-1` mean in llama-cpp-python?
**A:** All layers of the model are run from the GPU.

**Q:** What is `n_ctx` in llama-cpp-python?
**A:** The context size of the model (e.g., 2048).

---

## Part 2: Short Answer

**Q:** How is the Phi-3-mini model loaded?
**A:** `AutoModelForCausalLM.from_pretrained("microsoft/Phi-3-mini-4k-instruct", device_map="cuda", torch_dtype="auto", trust_remote_code=True)` plus its tokenizer, wrapped in a `pipeline("text-generation", ..., return_full_text=False, max_new_tokens=500, do_sample=False)`.

**Q:** How can you inspect the underlying prompt template?
**A:** `pipe.tokenizer.apply_chat_template(messages, tokenize=False)` prints the template, e.g., `<s><|user|>...<|end|><|assistant|>`.

**Q:** How do temperature and top_p affect output?
**A:** Higher temperature/top_p → more creative/diverse output; lower → more predictable/deterministic. Temperature controls likelihood of less-probable tokens; top_p controls the nucleus (subset of tokens by cumulative probability).

**Q:** What are the four use-case combinations in Table 6-1?
**A:** Brainstorming (High temperature / High top_p — diverse, creative, unexpected); Email generation (Low/Low — predictable, focused, conservative); Creative writing (High/Low — creative but coherent); Translation (Low/High — coherent with wider vocabulary/linguistic variety).

**Q:** Why does a temperature of 1 produce different output on each run?
**A:** It introduces stochastic behavior — the model randomly selects tokens.

**Q:** What is the most basic structured prompt, and how is it extended for classification?
**A:** Instruction + data. For classification, add output indicators (e.g., prefix "Text:" and add "Sentiment:") so the model outputs only "negative" or "positive."

**Q:** Why is thinking of prompts as puzzle pieces helpful?
**A:** It prompts reflection: "Have I described the context?" and "Does the prompt have an example of the output?"

**Q:** What are the seven advanced prompt components?
**A:** Persona, Instruction, Context, Format, Audience, Tone, and Data.

**Q:** How is the complex summary prompt constructed?
**A:** Via concatenated variables: `query = persona + instruction + context + data_format + audience + tone + data`.

**Q:** Why is experimentation vital in prompting?
**A:** Prompt components can be added/removed/reordered, and their order affects output quality (primacy/recency effects) — so an iterative cycle of experimentation finds the best prompt.

**Q:** What are emotional stimuli in prompting?
**A:** Creative components like "This is very important for my career." (from EmotionPrompt, Li et al., 2023) that can improve prompts.

**Q:** Why do some prompts work better for certain models?
**A:** Models' training data may differ or they are trained for different purposes.

**Q:** How does the made-up-word example work?
**A:** One-shot prompt: user defines "Gigamuru" and the assistant gives an example sentence; then user defines "screeg" and the model generates a correct sentence — demonstrating in-context learning with user/assistant role differentiation.

**Q:** Why must user and assistant roles be differentiated in the template?
**A:** Without role differentiation it would seem as if we were talking to ourselves; the model needs to see the pattern of question → answer.

**Q:** Can the model still ignore few-shot examples?
**A:** Yes — the model can still "choose," through random sampling, to ignore the instructions.

**Q:** How does chain prompting work for product features → sales pitch?
**A:** Prompt 1 creates a name and slogan from features; prompt 2 uses the name/slogan output to generate a sales pitch — a sequential pipeline.

**Q:** What is a benefit of making two chained calls instead of one?
**A:** Each call can use different parameters (e.g., short output for name/slogan, longer output for the pitch).

**Q:** Why is LLM behavior called reasoning capabilities rather than "true" reasoning?
**A:** It's generally considered emergent behavior through memorization of training data and pattern matching; we mimic reasoning via prompting to improve output.

**Q:** How does chain-of-thought help with math questions?
**A:** Providing reasoning examples distributes more compute over the reasoning process — each additional reasoning token stabilizes the output instead of computing the answer in a few tokens.

**Q:** How does the cafeteria/tennis-ball CoT example work?
**A:** The assistant shows a worked example (Roger: 5 + 2×3 = 11), then answers the new question with explanation first: 23 − 20 = 3, 3 + 6 = 9.

**Q:** How does self-consistency work?
**A:** Ask the same prompt multiple times (varying temperature/top_p for diversity), then take the majority result as the final answer. Optionally add chain-of-thought, using only the answer for voting.

**Q:** How does zero-shot tree-of-thought work?
**A:** A single prompt asks the model to imagine three experts answering the question, sharing one thinking step at a time, leaving if wrong, and discussing results until consensus.

**Q:** What went wrong with the zero-shot JSON character profile?
**A:** The output was truncated (stopped after "charisma") and was not valid JSON.

**Q:** How is a valid JSON character profile obtained?
**A:** Provide a one-shot format template with the expected keys (description, name, armor, weapon); the model follows it exactly.

**Q:** How does llama-cpp-python enforce JSON output?
**A:** Via `response_format={"type": "json_object"}` — under the hood it applies a JSON grammar; output is verified with `json.dumps(json.loads(output), indent=4)`.

**Q:** Why clear the VRAM before loading the GGUF model?
**A:** Loading a new model requires free VRAM — delete the old model/tokenizer/pipeline, then `gc.collect()` and `torch.cuda.empty_cache()`.

**Q:** Is constrained sampling affected by other parameters?
**A:** Yes — it is still affected by parameters such as top_p and temperature.

**Q:** What does the next chapter cover?
**A:** Going beyond prompt engineering — how LLMs can use external memory and tools.

---

## Part 3: Fill-in-the-Blank

**Q:** The chapter's model is ______, with ______ billion parameters, suitable for devices up to ______ GB VRAM.
**A:** Phi-3-mini; 3.8; 8.

**Q:** The model is loaded with `AutoModelForCausalLM.from_pretrained("______")`.
**A:** microsoft/Phi-3-mini-4k-instruct.

**Q:** The chat template tokens are ______, ______, and ______.
**A:** <|user|>; <|assistant|>; <|end|>.

**Q:** The pipeline used return_full_text=______, max_new_tokens=______, do_sample=______.
**A:** False; 500; False.

**Q:** A temperature of ______ generates the same response every time.
**A:** 0.

**Q:** A ______ temperature (e.g., 0.8) gives more diverse output; a ______ temperature (e.g., 0.2) gives deterministic output.
**A:** higher; lower.

**Q:** top_p is also known as ______ sampling.
**A:** nucleus.

**Q:** top_p=______ considers all tokens; top_p=0.1 considers tokens until cumulative probability ______.
**A:** 1; 0.1.

**Q:** top_k=100 means the LLM only considers the top ______ most probable tokens.
**A:** 100.

**Q:** In Table 6-1, Brainstorming uses ______ temperature / ______ top_p; Email generation uses ______ / ______; Creative writing uses ______ / ______; Translation uses ______ / ______.
**A:** High/High; Low/Low; High/Low; Low/High.

**Q:** The two components of a basic instruction prompt are the ______ and the ______ it refers to.
**A:** instruction; data.

**Q:** Prefixing with "______" and adding "______" are output indicators that force "negative"/"positive".
**A:** Text:; Sentiment:.

**Q:** Information in the ______ of a prompt is often forgotten.
**A:** middle.

**Q:** LLMs focus on the ______ (primacy effect) or ______ (recency effect) of a prompt.
**A:** beginning; end.

**Q:** The ______ aspect is arguably the most important in prompt design.
**A:** specificity.

**Q:** Asking the LLM to respond with "______" reduces hallucination impact.
**A:** I don't know.

**Q:** The seven advanced prompt components are ______, ______, ______, ______, ______, ______, and ______.
**A:** Persona; Instruction; Context; Format; Audience; Tone; Data.

**Q:** For education purposes, it is helpful to use ______ ("Explain it like I'm 5").
**A:** ELI5.

**Q:** In-context learning with 0 examples = ______, 1 example = ______, 2+ examples = ______.
**A:** zero-shot; one-shot; few-shot.

**Q:** The made-up-word example used "______" (musical instrument) and "______" (swing a sword).
**A:** Gigamuru; screeg.

**Q:** Chain prompting creates a ______ of interactions where one prompt's output becomes the next prompt's input.
**A:** chain.

**Q:** The chain example produced the product name "______" with slogan "______".
**A:** MindMeld Messenger; Unleashing Intelligent Conversations, One Response at a Time.

**Q:** ______ thinking is automatic and intuitive; ______ thinking is conscious and logical.
**A:** System 1; System 2.

**Q:** The CoT worked example: Roger had 5 balls + 2 cans of 3 = ______.
**A:** 11.

**Q:** The cafeteria example: 23 − 20 + 6 = ______.
**A:** 9.

**Q:** The common zero-shot CoT phrase is "______".
**A:** Let's think step-by-step.

**Q:** Self-consistency takes the ______ result as the final answer.
**A:** majority (majority vote).

**Q:** Self-consistency is ______ times slower, where n is the number of output samples.
**A:** n.

**Q:** Tree-of-thought keeps the most promising thoughts and ______ the lowest.
**A:** prunes.

**Q:** Zero-shot tree-of-thought emulates a conversation between multiple ______.
**A:** experts.

**Q:** The four reasons to validate output are structured output, ______ output, ______, and ______.
**A:** valid; ethics; accuracy.

**Q:** The three ways to control output are ______, ______ (constrained sampling), and ______ (Chapter 12).
**A:** examples; grammar; fine-tuning.

**Q:** The packages for constraining/validating output are ______, ______, and ______.
**A:** Guidance; Guardrails; LMQL.

**Q:** The zero-shot JSON output was not ______ (truncated).
**A:** valid.

**Q:** The one-shot JSON template had keys description, ______, ______, and ______.
**A:** name; armor; weapon.

**Q:** llama-cpp-python loads models in the ______ format.
**A:** GGUF.

**Q:** The GGUF model repo is "______" with filename "______".
**A:** microsoft/Phi-3-mini-4k-instruct-gguf; *fp16.gguf.

**Q:** `n_gpu_layers=______` means all layers run on the GPU; `n_ctx=______` was the context size.
**A:** -1; 2048.

**Q:** JSON output is enforced via `response_format={"type": "______"}`.
**A:** json_object.

**Q:** The next chapter explores how LLMs can use external ______ and ______.
**A:** memory; tools.
