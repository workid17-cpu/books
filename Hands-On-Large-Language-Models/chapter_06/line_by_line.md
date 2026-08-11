# 📘 Chapter 6 Line-by-Line: Prompt Engineering
**Source:** *Hands-On Large Language Models* (Alammar & Grootendorst), Chapter 6
**Format:** Each numbered item quotes a paragraph (or closely paraphrases it), then gives plain-English explanation + word meanings + technical terms. Code listings annotated.

---

## Opening

1. **Quote:** "In the first chapters of this book, we took our first steps into the world of large language models (LLMs). We delved into various applications, such as supervised and unsupervised classification, employing models that focus on representing text, like BERT and its derivatives."
   - **Plain English:** Earlier chapters covered LLMs and classification using representation models like BERT.
   - **Word meanings:** delved = explored deeply; derivatives = variants.
   - **Technical terms:** LLMs; supervised/unsupervised classification; representation models; BERT.

2. **Quote:** "As we progressed, we used models trained primarily for text generation, models that are often referred to as generative pre-trained transformers (GPT). These models have the remarkable ability to generate text in response to prompts from the user. Through prompt engineering, we can design these prompts in a way that enhances the quality of the generated text."
   - **Plain English:** GPT models generate text from user prompts; prompt engineering improves output quality.
   - **Word meanings:** remarkable = extraordinary.
   - **Technical terms:** generative pre-trained transformers (GPT); prompts; prompt engineering.

3. **Quote:** "In this chapter, we will explore these generative models in more detail and dive into the realm of prompt engineering, reasoning with generative models, verification, and even evaluating their output."
   - **Plain English:** Chapter covers generative models, prompt engineering, reasoning, verification, and output evaluation.
   - **Word meanings:** realm = domain/field.
   - **Technical terms:** prompt engineering; reasoning; verification.

### Using Text Generation Models

4. **Quote:** "Before we start with the fundamentals of prompt engineering, it is essential to explore the basics of utilizing a text generation model. How do we select the model to use? Do we use a proprietary or open source model? How can we control the generated output?"
   - **Plain English:** Basics: model selection, proprietary vs open source, and output control.
   - **Word meanings:** utilizing = using; proprietary = closed/owned.
   - **Technical terms:** text generation model; proprietary model; open source model.

### Choosing a Text Generation Model

5. **Quote:** "Choosing a text generation model starts with choosing between proprietary models or open source models. Although proprietary models are generally more performant, we focus in this book more on open source models as they offer more flexibility and are free to use."
   - **Plain English:** Proprietary models perform better, but the book uses open source models (flexible, free).
   - **Word meanings:** performant = high-performing.
   - **Technical terms:** proprietary models; open source models.

6. **Quote:** "Figure 6-1 shows a small selection of impactful foundation models, LLMs that have been pretrained on vast amounts of text data and are often fine-tuned for specific applications. From those foundation models, hundreds if not thousands of models have been fine-tuned, one more suitable for certain tasks than another. Choosing the model to use can be a daunting task!"
    - **Plain English:** Foundation models are pretrained LLMs fine-tuned into thousands of task-specific variants; choosing among them is hard.
    - **Word meanings:** daunting = intimidating.
    - **Technical terms:** foundation models; pretrained; fine-tuned.

7. **Quote:** "We advise starting with a small foundation model. So let's continue using Phi-3-mini, which has 3.8 billion parameters. This makes it suitable for running with devices up to 8 GB of VRAM. Overall, scaling up to larger models tends to be a nicer experience than scaling down. Smaller models provide a great introduction and lay a solid foundation for progressing to larger models."
    - **Plain English:** Start small — Phi-3-mini (3.8B params, 8 GB VRAM); scaling up is nicer than down; small models are a good introduction.
    - **Technical terms:** Phi-3-mini; parameters; VRAM; scaling.

### Loading a Text Generation Model

### Code Block: Loading the model and pipeline
```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline
# Load model and tokenizer
model = AutoModelForCausalLM.from_pretrained(
    "microsoft/Phi-3-mini-4k-instruct",
    device_map="cuda",
    torch_dtype="auto",
    trust_remote_code=True,
)
tokenizer = AutoTokenizer.from_pretrained("microsoft/Phi-3-mini-4k-instruct")
# Create a pipeline
pipe = pipeline(
    "text-generation",
    model=model,
    tokenizer=tokenizer,
    return_full_text=False,
    max_new_tokens=500,
    do_sample=False,
)
```
- **Explanation:** Loads Phi-3-mini-4k-instruct on GPU, loads its tokenizer, and creates a text-generation pipeline (return_full_text=False, max 500 new tokens, do_sample=False).
- **Fits the architecture:** Standard loading of an open source causal LM via the transformers library.

8. **Quote:** "Compared to previous chapters, we will take a closer look at developing and using the prompt template."
    - **Plain English:** This chapter focuses on prompt templates.
    - **Technical terms:** prompt template.

### Code Block: The basic prompt and output
```python
# Prompt
messages = [
    {"role": "user", "content": "Create a funny joke about chickens."}
]
# Generate the output
output = pipe(messages)
print(output[0]["generated_text"])
```
- **Explanation:** Passes a single user message; the model replies with a chicken joke.
- **Fits the architecture:** Role-based messages (user/assistant) are the chat interface format.

9. **Quote:** "Under the hood, transformers.pipeline first converts our messages into a specific prompt template. We can explore this process by accessing the underlying tokenizer:"
    - **Plain English:** The pipeline converts messages into a chat template using the tokenizer.
    - **Technical terms:** transformers.pipeline; tokenizer; chat template.

### Code Block: Inspecting the chat template
```python
# Apply prompt template
prompt = pipe.tokenizer.apply_chat_template(messages, tokenize=False)
print(prompt)
```
```
<s><|user|>
Create a funny joke about chickens.<|end|>
<|assistant|>
```
- **Explanation:** Shows the template: `<s>` start, `<|user|>`/`<|assistant|>` role tokens, `<|end|>` stop token.
- **Fits the architecture:** The template encodes who said what and where to stop.

10. **Quote:** "You may recognize the special tokens <|user|> and <|assistant|> from Chapter 2. This prompt template, further illustrated in Figure 6-2, was used during the training of the model. Not only does it provide information about who said what, but it is also used to indicate when the model should stop generating text (see the <|end|> token). This prompt is passed directly to the LLM and processed all at once."
    - **Plain English:** The template (from training) tells the model who said what and where to stop; the prompt is processed all at once.
    - **Technical terms:** special tokens; <|user|>; <|assistant|>; <|end|>; chat template.

11. **Quote:** "In the next chapter, we will customize parts of this template ourselves. Throughout this chapter, we can use transformers.pipeline to handle chat template processing for us. Next, let us explore how we can control the output of the model."
    - **Plain English:** Pipeline handles template processing here; next chapter customizes it.
    - **Technical terms:** chat template processing.

### Controlling Model Output

12. **Quote:** "Other than prompt engineering, we can control the kind of output we want by adjusting the model parameters. In our previous example, you might have noticed that we used several parameters in the pipe function, including temperature and top_p. These parameters control the randomness of the output."
    - **Plain English:** temperature and top_p control output randomness.
    - **Technical terms:** temperature; top_p.

13. **Quote:** "A part of what makes LLMs exciting technology is that it can generate different responses for the exact same prompt. Each time an LLM needs to generate a token, it assigns a likelihood number to each possible token."
    - **Plain English:** LLMs can produce different responses; each token gets a likelihood score.
    - **Word meanings:** likelihood = probability.
    - **Technical terms:** token generation; likelihood scores.

14. **Quote:** "As illustrated in Figure 6-3, in the sentence 'I am driving a…' the likelihood of that sentence being followed by tokens like 'car' or 'truck' is generally higher than a token like 'elephant.' However, there is still a possibility of 'elephant' being generated but it is much lower."
    - **Plain English:** More likely continuations ("car") have higher likelihood than unlikely ones ("elephant"), but unlikely ones are still possible.
    - **Technical terms:** next-token prediction; likelihood.

15. **Quote:** "When we loaded our model, we purposefully set do_sample=False to make sure the output is somewhat consistent. This means that no sampling will be done and only the most probable next token is selected. However, to use the temperature and top_p parameters, we will set do_sample=True in order to make use of them."
    - **Plain English:** do_sample=False → always pick most probable token (consistent); do_sample=True needed for temperature/top_p.
    - **Technical terms:** do_sample; greedy selection; sampling.

### Temperature

16. **Quote:** "The temperature controls the randomness or creativity of the text generated. It defines how likely it is to choose tokens that are less probable. The underlying idea is that a temperature of 0 generates the same response every time because it always chooses the most likely word. As illustrated in Figure 6-4, a higher value allows less probable words to be generated."
    - **Plain English:** Temperature 0 → same response always (most likely word); higher → less probable words allowed.
    - **Technical terms:** temperature; token probability.

17. **Quote:** "As a result, a higher temperature (e.g., 0.8) generally results in a more diverse output while a lower temperature (e.g., 0.2) creates a more deterministic output."
    - **Plain English:** Higher temperature (0.8) = diverse; lower (0.2) = deterministic.
    - **Word meanings:** diverse = varied; deterministic = predictable/fixed.
    - **Technical terms:** temperature.

### Code Block: High temperature generation
```python
# Using a high temperature
output = pipe(messages, do_sample=True, temperature=1)
print(output[0]["generated_text"])
```
- **Explanation:** Enables sampling at temperature 1 → creative output that changes each run.
- **Fits the architecture:** Temperature introduces stochastic behavior via random token selection.

18. **Quote:** "Note that every time you rerun this piece of code, the output will change! temperature introduces stochastic behavior since the model now randomly selects tokens."
    - **Plain English:** High temperature → output changes every run (stochastic).
    - **Word meanings:** stochastic = random.
    - **Technical terms:** stochastic behavior; sampling.

### top_p

19. **Quote:** "top_p, also known as nucleus sampling, is a sampling technique that controls which subset of tokens (the nucleus) the LLM can consider. It will consider tokens until it reaches their cumulative probability. If we set top_p to 0.1, it will consider tokens until it reaches that value. If we set top_p to 1, it will consider all tokens."
    - **Plain English:** top_p (nucleus sampling) limits tokens by cumulative probability; 0.1 = small nucleus, 1 = all tokens.
    - **Technical terms:** top_p; nucleus sampling; cumulative probability.

20. **Quote:** "As shown in Figure 6-5, by lowering the value, it will consider fewer tokens and generally give less 'creative' output, while increasing the value allows the LLM to choose from more tokens."
    - **Plain English:** Lower top_p = fewer tokens, less creative; higher = more tokens.
    - **Technical terms:** nucleus size.

21. **Quote:** "Similarly, the top_k parameter controls exactly how many tokens the LLM can consider. If you change its value to 100, the LLM will only consider the top 100 most probable tokens."
    - **Plain English:** top_k sets an exact token count (e.g., top 100).
    - **Technical terms:** top_k.

### Code Block: High top_p generation
```python
# Using a high top_p
output = pipe(messages, do_sample=True, top_p=1)
print(output[0]["generated_text"])
```
- **Explanation:** top_p=1 considers all tokens — maximal flexibility.
- **Fits the architecture:** top_p controls the pool of selectable tokens.

22. **Quote:** "As shown in Table 6-1, these parameters allow the user to have a sliding scale between being creative (high temperature and top_p) and being predictable (lower temperature and top_p)."
    - **Plain English:** temperature/top_p form a creativity ↔ predictability slider.
    - **Technical terms:** sampling parameters.

23. **Quote (Table 6-1):** "Brainstorming session: High temperature / High top_p — High randomness with large pool of potential tokens. The results will be highly diverse, often leading to very creative and unexpected results. Email generation: Low / Low — Deterministic output with high probable predicted tokens. This results in predictable, focused, and conservative outputs. Creative writing: High / Low — High randomness with a small pool of potential tokens. This combination produces creative outputs but still remains coherent. Translation: Low / High — Deterministic output with high probable predicted tokens. Produces coherent output with a wider range of vocabulary, leading to outputs with linguistic variety."
    - **Plain English:** Four combos: Brainstorming (High/High, creative+unexpected), Email (Low/Low, predictable+conservative), Creative writing (High/Low, creative+coherent), Translation (Low/High, coherent+linguistic variety).
    - **Technical terms:** temperature; top_p; deterministic vs. diverse output.

### Intro to Prompt Engineering

24. **Quote:** "An essential part of working with text-generative LLMs is prompt engineering. By carefully designing our prompts we can guide the LLM to generate desired responses. Whether the prompts are questions, statements, or instructions, the main goal of prompt engineering is to elicit a useful response from the model."
    - **Plain English:** Prompt engineering guides LLMs toward useful responses.
    - **Word meanings:** elicit = draw out.
    - **Technical terms:** prompt engineering.

25. **Quote:** "Prompt engineering is more than designing effective prompts. It can be used as a tool to evaluate the output of a model as well as to design safeguards and safety mitigation methods. This is an iterative process of prompt optimization and requires experimentation. There is not and unlikely will ever be a perfect prompt design."
    - **Plain English:** Prompt engineering also evaluates output, designs safeguards; it's iterative and experimental — no perfect prompt.
    - **Word meanings:** iterative = repeated; mitigation = reduction of harm.
    - **Technical terms:** safeguards; prompt optimization; experimentation.

### The Basic Ingredients of a Prompt

26. **Quote:** "An LLM is a prediction machine. Based on a certain input, the prompt, it tries to predict the words that might follow it. At its core (illustrated in Figure 6-6), the prompt does not need to be more than a few words to elicit a response from the LLM."
    - **Plain English:** An LLM predicts what follows the prompt; even a few words work.
    - **Technical terms:** prediction machine; prompt.

27. **Quote:** "However, although the illustration works as a basic example, it fails to complete a specific task. Instead, we generally approach prompt engineering by asking a specific question or task the LLM should complete. To elicit the desired response, we need a more structured prompt."
    - **Plain English:** For specific tasks, we need a structured prompt, not just sentence completion.
    - **Technical terms:** structured prompt.

28. **Quote:** "For example, and as shown in Figure 6-7, we could ask the LLM to classify a sentence into either having positive or negative sentiment. This extends the most basic prompt to one consisting of two components—the instruction itself and the data that relates to the instruction."
    - **Plain English:** A classification prompt = instruction + data.
    - **Technical terms:** instruction; data; sentiment classification.

29. **Quote:** "More complex use cases might require more components in a prompt. For instance, to make sure the model only outputs 'negative' or 'positive' we can introduce output indicators that help guide the model. In Figure 6-8, we prefix the sentence with 'Text:' and add 'Sentiment:' to prevent the model from generating a complete sentence. Instead, this structure indicates that we expect either 'negative' or 'positive.' Although the model might not have been trained on these components directly, it was fed enough instructions to be able to generalize to this structure."
    - **Plain English:** Output indicators ("Text:", "Sentiment:") constrain output to "negative"/"positive"; the model generalizes to this structure.
    - **Technical terms:** output indicators; generalization.

30. **Quote:** "We can continue adding or updating the elements of a prompt until we elicit the response we are looking for. We could add additional examples, describe the use case in more detail, provide additional context, etc. These components are merely examples and not a limited set of possibilities. The creativity that comes with designing these components is key."
    - **Plain English:** Keep iterating on prompt elements; creativity in component design is key.
    - **Technical terms:** prompt components; iteration.

31. **Quote:** "Although a prompt is a single piece of text, it is tremendously helpful to think of prompts as pieces of a larger puzzle. Have I described the context of my question? Does the prompt have an example of the output?"
    - **Plain English:** Think of prompts as puzzle pieces — ask if context and output examples are included.
    - **Word meanings:** tremendously = very.
    - **Technical terms:** prompt design.

### Instruction-Based Prompting

32. **Quote:** "Although prompting comes in many flavors, from discussing philosophy with the LLM to role-playing with your favorite superhero, prompting is often used to have the LLM answer a specific question or resolve a certain task. This is referred to as instruction-based prompting."
    - **Plain English:** Instruction-based prompting = prompting to answer a specific question/resolve a task.
    - **Technical terms:** instruction-based prompting.

33. **Quote:** "Each of these tasks requires different prompting formats and more specifically, asking different questions of the LLM. Asking the LLM to summarize a piece of text will not suddenly result in classification. To illustrate, examples of prompts for some of these use cases can be found in Figure 6-10."
    - **Plain English:** Different tasks need different prompt formats/questions; summarization ≠ classification.
    - **Technical terms:** prompting formats; use cases.

34. **Quote:** "Although these tasks require different instructions, there is actually a lot of overlap in the prompting techniques used to improve the quality of the output. A non-exhaustive list of these techniques includes: Specificity; Hallucination; Order."
    - **Plain English:** Techniques overlap across tasks: specificity, hallucination mitigation, and ordering.
    - **Word meanings:** non-exhaustive = not complete.
    - **Technical terms:** specificity; hallucination; primacy/recency.

35. **Quote:** "Specificity: Accurately describe what you want to achieve. Instead of asking the LLM to 'Write a description for a product' ask it to 'Write a description for a product in less than two sentences and use a formal tone.'"
    - **Plain English:** Be specific — include constraints like length and tone.
    - **Technical terms:** specificity.

36. **Quote:** "Hallucination: LLMs may generate incorrect information confidently, which is referred to as hallucination. To reduce its impact, we can ask the LLM to only generate an answer if it knows the answer. If it does not know the answer, it can respond with 'I don't know.'"
    - **Plain English:** Reduce hallucination by letting the model say "I don't know."
    - **Technical terms:** hallucination.

37. **Quote:** "Order: Either begin or end your prompt with the instruction. Especially with long prompts, information in the middle is often forgotten. LLMs tend to focus on information either at the beginning of a prompt (primacy effect) or the end of a prompt (recency effect)."
    - **Plain English:** Place instructions at start/end; middle info is forgotten (primacy/recency effects).
    - **Technical terms:** primacy effect; recency effect.

38. **Quote:** "Here, specificity is arguably the most important aspect. By restricting and specifying what the model should generate, there is a smaller chance of having it generate something not related to your use case. For instance, if we were to skip the instruction 'in two to three sentences' it might generate complete paragraphs. Like human conversations, without any specific instructions or additional context, it is difficult to derive what the task at hand actually is."
    - **Plain English:** Specificity is the most important aspect — it restricts output and reduces off-target generation.
    - **Word meanings:** arguably = debatably/possibly.
    - **Technical terms:** specificity.

### Advanced Prompt Engineering

39. **Quote:** "On the surface, creating a good prompt might seem straightforward. Ask a specific question, be accurate, add some examples, and you are done! However, prompting can grow complex quite quickly and as a result is an often-underestimated component of leveraging LLMs."
    - **Plain English:** Good prompts seem easy but are complex and underestimated.
    - **Word meanings:** underestimated = undervalued.
    - **Technical terms:** (none) — general point.

40. **Quote:** "Here, we will go through several advanced techniques for building up your prompts, starting with the iterative workflow of building up complex prompts all the way to using LLMs sequentially to get improved results. Eventually, we will even build up to advanced reasoning techniques."
    - **Plain English:** Covers iterative prompt building, sequential LLM use, and advanced reasoning.
    - **Technical terms:** iterative workflow; reasoning techniques.

### The Potential Complexity of a Prompt

41. **Quote:** "As we explored in the intro to prompt engineering, a prompt generally consists of multiple components. In our very first example, our prompt consisted of instruction, data, and output indicators. As we mentioned before, no prompt is limited to just these three components and you can build it up to be as complex as you want."
    - **Plain English:** Prompts can be arbitrarily complex beyond instruction/data/output indicators.
    - **Technical terms:** prompt components.

42. **Quote:** "These advanced components can quickly make a prompt quite complex. Some common components are: Persona — Describe what role the LLM should take on. For example, use 'You are an expert in astrophysics' if you want to ask a question about astrophysics. Instruction — The task itself. Make sure this is as specific as possible. We do not want to leave much room for interpretation. Context — Additional information describing the context of the problem or task. It answers questions like 'What is the reason for the instruction?' Format — The format the LLM should use to output the generated text. Without it, the LLM will come up with a format itself, which is troublesome in automated systems. Audience — The target of the generated text. This also describes the level of the generated output. For education purposes, it is often helpful to use ELI5 ('Explain it like I'm 5'). Tone — The tone of voice the LLM should use in the generated text. If you are writing a formal email to your boss, you might not want to use an informal tone of voice. Data — The main data related to the task itself."
    - **Plain English:** Seven common components: Persona, Instruction, Context, Format, Audience, Tone, Data — each with a specific role.
    - **Word meanings:** troublesome = problematic.
    - **Technical terms:** persona; instruction; context; format; audience; tone; data; ELI5.

### Code Block: The complex prompt (all components)
```python
# Prompt components
persona = "You are an expert in Large Language models. You excel at breaking 
down complex papers into digestible summaries.\n"
instruction = "Summarize the key findings of the paper provided.\n"
context = "Your summary should extract the most crucial points that can help 
researchers quickly understand the most vital information of the paper.\n"
data_format = "Create a bullet-point summary that outlines the method. Follow 
this up with a concise paragraph that encapsulates the main results.\n"
audience = "The summary is designed for busy researchers that quickly need to 
grasp the newest trends in Large Language Models.\n"
tone = "The tone should be professional and clear.\n"
text = "MY TEXT TO SUMMARIZE"
data = f"Text to summarize: {text}"
# The full prompt - remove and add pieces to view its impact on the generated 
output
query = persona + instruction + context + data_format + audience + tone + data
```
- **Explanation:** Builds a summary prompt from persona, instruction, context, data_format, audience, tone, and data components concatenated into a query.
- **Fits the architecture:** Prompt components are modular — add/remove to test their effect.

43. **Quote:** "This complex prompt demonstrates the modular nature of prompting. We can add and remove components freely and judge their effect on the output. As illustrated in Figure 6-12, we can slowly build up our prompt and explore the effect of each change. The changes are not limited to simply introducing or removing components. Their order, as we saw before with the recency and primacy effects, can affect the quality of the LLM's output. In other words, experimentation is vital when finding the best prompt for your use case. With prompting, we essentially have ourselves in an iterative cycle of experimentation."
    - **Plain English:** Prompting is modular and iterative — add/remove/reorder components and experiment.
    - **Technical terms:** modular prompting; iterative cycle; primacy/recency effects.

44. **Quote (advice box):** "There is all manner of components that we could add and creative components like using emotional stimuli (e.g., 'This is very important for my career.'). Part of the fun in prompt engineering is that you can be as creative as possible to figure out which combination of prompt components contribute to your use case. There are few constraints to developing a format that works for you. In a way, it is an attempt to reverse engineer what the model has learned and how it responds to certain prompts. However, note that some prompts work better for certain models compared to others as their training data might be different or they are trained for different purposes."
    - **Plain English:** Be creative (e.g., emotional stimuli); prompting reverse-engineers what the model learned; prompts behave differently per model.
    - **Technical terms:** emotional stimuli; reverse engineering.

### In-Context Learning: Providing Examples

45. **Quote:** "In the previous sections, we tried to accurately describe what the LLM should do. Although accurate and specific descriptions help the LLM to understand the use case, we can go one step further. Instead of describing the task, why do we not just show the task? We can provide the LLM with examples of exactly the thing that we want to achieve. This is often referred to as in-context learning, where we provide the model with correct examples."
    - **Plain English:** In-context learning = show examples instead of describing the task.
    - **Technical terms:** in-context learning.

46. **Quote:** "As illustrated in Figure 6-13, this comes in a number of forms depending on how many examples you show the LLM. Zero-shot prompting does not leverage examples, one-shot prompts use a single example, and few-shot prompts use two or more examples."
    - **Plain English:** zero-shot (0 examples), one-shot (1), few-shot (2+).
    - **Technical terms:** zero-shot; one-shot; few-shot.

47. **Quote:** "Adopting the original phrase, we believe that 'an example is worth a thousand words.' These examples provide a direct example of what and how the LLM should achieve."
    - **Plain English:** Examples directly demonstrate what/how the model should produce.
    - **Word meanings:** adopting = borrowing.
    - **Technical terms:** (none) — principle.

48. **Quote:** "We can illustrate this method with a simple example taken from the original paper describing this method. The goal of the prompt is to generate a sentence with a made-up word. To improve the quality of the resulting sentence, we can show the generative model an example of what a proper sentence with a made-up word would be. To do so, we will need to differentiate between our question (user) and the answers that were provided by the model (assistant). We additionally showcase how this interaction is processed using the template:"
    - **Plain English:** Made-up-word example (Gigamuru/screeg) demonstrates user/assistant role differentiation in the template.
    - **Technical terms:** user/assistant roles; chat template.

### Code Block: One-shot in-context learning
```python
# Use a single example of using the made-up word in a sentence
one_shot_prompt = [
    {
        "role": "user",
        "content": "A 'Gigamuru' is a type of Japanese musical instrument. An 
example of a sentence that uses the word Gigamuru is:"
    },
    {
        "role": "assistant",
        "content": "I have a Gigamuru that my uncle gave me as a gift. I love 
to play it at home."
    },
    {
        "role": "user",
        "content": "To 'screeg' something is to swing a sword at it. An example 
of a sentence that uses the word screeg is:"
    }
]
print(tokenizer.apply_chat_template(one_shot_prompt, tokenize=False))
# Generate the output
outputs = pipe(one_shot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** One-shot prompt: user defines "Gigamuru", assistant gives an example sentence, then user defines "screeg"; the model correctly generates a sentence using "screeg".
- **Fits the architecture:** Alternating user/assistant turns show the model the expected pattern.

49. **Quote:** "The prompt illustrates the need to differentiate between the user and the assistant. If we did not, it would seem as if we were talking to ourselves. Using these interactions, we can generate output as follows: ... It correctly generated the answer!"
    - **Plain English:** Distinguishing user/assistant roles is essential for the model to learn from the example.
    - **Technical terms:** user/assistant message roles.

50. **Quote:** "As with all prompt components, one- or few-shot prompting is not the be all and end all of prompt engineering. We can use it as one piece of the puzzle to further enhance the descriptions that we gave it. The model can still 'choose,' through random sampling, to ignore the instructions."
    - **Plain English:** Few-shot prompting is one tool; the model can still ignore instructions via random sampling.
    - **Word meanings:** be all and end all = the only thing that matters.
    - **Technical terms:** random sampling.

### Chain Prompting: Breaking up the Problem

51. **Quote:** "In previous examples, we explored splitting up prompts into modular components to improve the performance of LLMs. Although this works well for many use cases, this might not be feasible for highly complex prompts or use cases. Instead of breaking the problem within a prompt, we can do so between prompts. Essentially, we take the output of one prompt and use it as input for the next, thereby creating a continuous chain of interactions that solves our problem."
    - **Plain English:** Chain prompting breaks the problem between prompts — output of one becomes input of the next.
    - **Word meanings:** feasible = practical/possible.
    - **Technical terms:** chain prompting.

52. **Quote:** "To illustrate, let us say we want to use an LLM to create a product name, slogan, and sales pitch for us based on a number of product features. Although we can ask the LLM to do this in one go, we can instead break up the problem into pieces. As a result, and as illustrated in Figure 6-14, we get a sequential pipeline that first creates the product name, uses that with the product features as input to create the slogan, and finally, uses the features, product name, and slogan to create the sales pitch."
    - **Plain English:** Features → name → slogan → sales pitch is a sequential pipeline of chained prompts.
    - **Technical terms:** sequential pipeline; chained prompts.

53. **Quote:** "This technique of chaining prompts allows the LLM to spend more time on each individual question instead of tackling the whole problem."
    - **Plain English:** Chaining lets the LLM focus on each sub-problem.
    - **Technical terms:** (none) — benefit of chaining.

### Code Block: Chain prompting — name and slogan
```python
# Create name and slogan for a product
product_prompt = [
    {"role": "user", "content": "Create a name and slogan for a chatbot that 
leverages LLMs."}
]
outputs = pipe(product_prompt)
product_description = outputs[0]["generated_text"]
print(product_description)
```
- **Explanation:** First prompt creates name + slogan ("MindMeld Messenger" / "Unleashing Intelligent Conversations...").
- **Fits the architecture:** Output feeds the next prompt in the chain.

### Code Block: Chain prompting — sales pitch
```python
# Based on a name and slogan for a product, generate a sales pitch
sales_prompt = [
    {"role": "user", "content": f"Generate a very short sales pitch for the 
following product: '{product_description}'"}
]
outputs = pipe(sales_prompt)
sales_pitch = outputs[0]["generated_text"]
print(sales_pitch)
```
- **Explanation:** Uses the previous name/slogan output to generate a sales pitch.
- **Fits the architecture:** Each call can use different parameters (short vs. long output).

54. **Quote:** "Although we need two calls to the model, a major benefit is that we can give each call different parameters. For instance, the number of tokens created was relatively small for the name and slogan whereas the pitch can be much longer."
    - **Plain English:** Benefit: each chained call can have different parameters (e.g., token counts).
    - **Technical terms:** per-call parameters.

55. **Quote:** "This can be used for a variety of use cases, including: Response validation — Ask the LLM to double-check previously generated outputs. Parallel prompts — Create multiple prompts in parallel and do a final pass to merge them. For example, ask multiple LLMs to generate multiple recipes in parallel and use the combined result to create a shopping list. Writing stories — Leverage the LLM to write books or stories by breaking down the problem into components. For example, by first writing a summary, developing characters, and building the story beats before diving into creating the dialogue."
    - **Plain English:** Use cases: response validation, parallel prompts (merge), story writing (break down into components).
    - **Technical terms:** response validation; parallel prompts; story beats.

56. **Quote:** "In the next chapter, we will automate this process and go beyond chaining LLMs. We will chain other pieces of technology together, like memory, tool use, and more! Before that, this idea of prompt chaining will be explored further in the following sections, which describe more complex prompt chaining methods like self-consistency, chain-of-thought, and tree-of-thought."
    - **Plain English:** Next chapter adds memory/tool use; this chapter's remaining sections cover self-consistency, chain-of-thought, tree-of-thought.
    - **Technical terms:** self-consistency; chain-of-thought; tree-of-thought.

### Reasoning with Generative Models

57. **Quote:** "In the previous sections, we focused mostly on the modular component of prompts, building them up through iteration. These advanced prompt engineering techniques, like prompt chaining, proved to be the first step toward enabling complex reasoning with generative models."
    - **Plain English:** Prompt chaining is the first step toward complex reasoning.
    - **Technical terms:** reasoning.

58. **Quote:** "Reasoning is a core component of human intelligence and is often compared to the emergent behavior of LLMs that often resembles reasoning. We highlight 'resemble' as these models, at the time of writing, are generally considered to demonstrate this behavior through memorization of training data and pattern matching."
    - **Plain English:** LLM "reasoning" is actually emergent behavior from memorization + pattern matching, not true reasoning.
    - **Word meanings:** emergent = arising from complexity.
    - **Technical terms:** emergent behavior; memorization; pattern matching.

59. **Quote:** "The output that they showcase, however, can demonstrate complex behavior and although it might not be 'true' reasoning, they are still referred to as reasoning capabilities. In other words, we work together with the LLM through prompt engineering so we can mimic reasoning processes in order to improve the output of the LLM."
    - **Plain English:** We mimic reasoning via prompting to improve output, even though it's not "true" reasoning.
    - **Technical terms:** reasoning capabilities; mimic.

60. **Quote:** "To allow for this reasoning behavior, it is a good moment to step back and explore what reasoning entails in human behavior. To simplify, our methods of reasoning can be divided into system 1 and 2 thinking processes. System 1 thinking represents an automatic, intuitive, and near-instantaneous process. It shares similarities with generative models that automatically generate tokens without any self-reflective behavior. In contrast, system 2 thinking is a conscious, slow, and logical process, akin to brainstorming and self-reflection."
    - **Plain English:** System 1 = automatic/intuitive/instant (like token generation); System 2 = conscious/slow/logical (brainstorming, self-reflection).
    - **Word meanings:** entails = involves; akin = similar to.
    - **Technical terms:** system 1 thinking; system 2 thinking.

61. **Quote:** "If we could give a generative model the ability to mimic a form of self-reflection, we would essentially be emulating the system 2 way of thinking, which tends to produce more thoughtful responses than system 1 thinking. In this section, we will explore several techniques that attempt to mimic these kinds of thought processes of human reasoners with the aim of improving the output of the model."
    - **Plain English:** Prompting techniques emulate System 2 (self-reflection) for more thoughtful responses.
    - **Word meanings:** emulating = imitating.
    - **Technical terms:** system 2 emulation; self-reflection.

### Chain-of-Thought: Think Before Answering

62. **Quote:** "The first and major step toward complex reasoning in generative models was through a method called chain-of-thought. Chain-of-thought aims to have the generative model 'think' first rather than answering the question directly without any reasoning. As illustrated in Figure 6-15, it provides examples in a prompt that demonstrate the reasoning the model should do before generating its response. These reasoning processes are referred to as 'thoughts.' This helps tremendously for tasks that involve a higher degree of complexity, like mathematical questions. Adding this reasoning step allows the model to distribute more compute over the reasoning process. Instead of calculating the entire solution based on a few tokens, each additional token in this reasoning process allows the LLM to stabilize its output."
    - **Plain English:** CoT = provide reasoning examples ("thoughts") so the model thinks before answering; each reasoning token stabilizes the output.
    - **Word meanings:** tremendously = greatly.
    - **Technical terms:** chain-of-thought; thoughts; reasoning tokens.

### Code Block: Chain-of-thought prompting
```python
# Answering with chain-of-thought
cot_prompt = [
    {"role": "user", "content": "Roger has 5 tennis balls. He buys 2 more cans 
of tennis balls. Each can has 3 tennis balls. How many tennis balls does he 
have now?"},
    {"role": "assistant", "content": "Roger started with 5 balls. 2 cans of 3 
tennis balls each is 6 tennis balls. 5 + 6 = 11. The answer is 11."},
    {"role": "user", "content": "The cafeteria had 23 apples. If they used 20 
to make lunch and bought 6 more, how many apples do they have?"}
]
# Generate the output
outputs = pipe(cot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Shows a worked example (Roger's tennis balls), then asks the cafeteria question; the model explains (23 − 20 = 3, + 6 = 9) before answering.
- **Fits the architecture:** Reasoning examples persuade the model to reason.

63. **Quote:** "Note how the model doesn't generate only the answer but provides an explanation before doing so. By doing so, it can leverage the knowledge it has generated thus far to compute the final answer."
    - **Plain English:** The model explains first, using that explanation to compute the answer.
    - **Technical terms:** explanation-first reasoning.

64. **Quote:** "Although chain-of-thought is a great method for enhancing the output of a generative model, it does require one or more examples of reasoning in the prompt, which the user might not have access to. Instead of providing examples, we can simply ask the generative model to provide the reasoning (zero-shot chain-of-thought). There are many different forms that work but a common and effective method is to use the phrase 'Let's think step-by-step,' which is illustrated in Figure 6-16."
    - **Plain English:** CoT needs examples; zero-shot CoT instead asks for reasoning — commonly via "Let's think step-by-step."
    - **Technical terms:** zero-shot chain-of-thought.

### Code Block: Zero-shot chain-of-thought
```python
# Zero-shot chain-of-thought
zeroshot_cot_prompt = [
    {"role": "user", "content": "The cafeteria had 23 apples. If they used 20 
to make lunch and bought 6 more, how many apples do they have? Let's think 
step-by-step."}
]
# Generate the output
outputs = pipe(zeroshot_cot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Appends "Let's think step-by-step" to get stepwise reasoning without examples.
- **Fits the architecture:** A single phrase primes chain-of-thought-like reasoning.

65. **Quote:** "Without needing to provide examples, we again got the same reasoning behavior. This is why it is so important to 'show your work' when doing calculations. By addressing the reasoning process the LLM can use the previously generated information as a guide through generating the final answer."
    - **Plain English:** Zero-shot CoT works; showing work lets the model use prior info as a guide.
    - **Word meanings:** (none).
    - **Technical terms:** reasoning process.

66. **Quote (advice box):** "Although the prompt 'Let's think step by step' can improve the output, you are not constrained by this exact formulation. Alternatives exist like 'Take a deep breath and think step-by-step' and 'Let's work through this problem step-by-step.'"
    - **Plain English:** Alternatives to "Let's think step-by-step" also prime reasoning.
    - **Technical terms:** (none) — prompt variants.

### Self-Consistency: Sampling Outputs

67. **Quote:** "Using the same prompt multiple times can lead to different results if we allow for a degree of creativity through parameters like temperature and top_p. As a result, the quality of the output might improve or degrade depending on the random selection of tokens. In other words, luck!"
    - **Plain English:** Sampling makes output random — quality depends on token-selection luck.
    - **Word meanings:** degrade = worsen.
    - **Technical terms:** sampling; randomness.

68. **Quote:** "To counteract this degree of randomness and improve the performance of generative models, self-consistency was introduced. This method asks the generative model the same prompt multiple times and takes the majority result as the final answer. During this process, each answer can be affected by different temperature and top_p values to increase the diversity of sampling."
    - **Plain English:** Self-consistency = ask the same prompt multiple times, take the majority result; vary temperature/top_p for diversity.
    - **Word meanings:** counteract = counter/neutralize.
    - **Technical terms:** self-consistency; majority voting.

69. **Quote:** "As illustrated in Figure 6-17, this method can further be improved by adding chain-of-thought prompting to improve its reasoning while only using the answer for the voting procedure."
    - **Plain English:** Self-consistency + chain-of-thought; only the answer is used for voting.
    - **Technical terms:** chain-of-thought + self-consistency.

70. **Quote:** "However, this does require a single question to be asked multiple times. As a result, although the method can improve performance, it becomes n times slower where n is the number of output samples."
    - **Plain English:** Self-consistency is n times slower (n = number of samples).
    - **Technical terms:** n× slowdown.

### Tree-of-Thought: Exploring Intermediate Steps

71. **Quote:** "The ideas of chain-of-thought and self-consistency are meant to enable more complex reasoning. By sampling from multiple 'thoughts' and making them more thoughtful, we aim to improve the output of generative models. These techniques only scratch the surface of what is currently being done to mimic complex reasoning. An improvement to these approaches can be found in tree-of-thought, which allows for an in-depth exploration of several ideas."
    - **Plain English:** Tree-of-thought improves on CoT/self-consistency by exploring multiple ideas in depth.
    - **Word meanings:** scratch the surface = only begin to explore.
    - **Technical terms:** tree-of-thought.

72. **Quote:** "The method works as follows. When faced with a problem that requires multiple reasoning steps, it often helps to break it down into pieces. At each step, and as illustrated in Figure 6-18, the generative model is prompted to explore different solutions to the problem at hand. It then votes for the best solution and continues to the next step."
    - **Plain English:** At each step, the model explores multiple solutions, votes for the best, and continues.
    - **Technical terms:** tree-based reasoning; voting.

73. **Quote:** "This method is tremendously helpful when needing to consider multiple paths, like when writing a story or coming up with creative ideas. A disadvantage of this method is that it requires many calls to the generative models, which slows the application significantly. Fortunately, there has been a successful attempt to convert the tree-of-thought framework into a simple prompting technique. Instead of calling the generative model multiple times, we ask the model to mimic that behavior by emulating a conversation between multiple experts. These experts will question each other until they reach a consensus."
    - **Plain English:** ToT is helpful for multi-path problems but needs many calls; zero-shot variant = one prompt emulating a multi-expert consensus discussion.
    - **Word meanings:** consensus = agreement.
    - **Technical terms:** zero-shot tree-of-thought; multi-expert emulation.

### Code Block: Zero-shot tree-of-thought
```python
# Zero-shot tree-of-thought
zeroshot_tot_prompt = [
    {"role": "user", "content": "Imagine three different experts are answering 
this question. All experts will write down 1 step of their thinking, then share 
it with the group. Then all experts will go on to the next step, etc. If any 
expert realizes they're wrong at any point then they leave. The question is 
'The cafeteria had 23 apples. If they used 20 to make lunch and bought 6 more, 
how many apples do they have?' Make sure to discuss the results."}
]
# Generate the output
outputs = pipe(zeroshot_tot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** A single prompt emulates a discussion among three experts who share steps, verify each other, and reach consensus (9 apples).
- **Fits the architecture:** Mimics tree-of-thought exploration without many model calls.

74. **Quote:** "We again get the correct answer but instead through a 'discussion between experts.' It is interesting to see such a conversation between 'experts' that demonstrates the creativity that comes with prompt engineering."
    - **Plain English:** The expert-discussion framing still yields the right answer — prompt engineering is creative.
    - **Technical terms:** (none) — observation.

### Output Verification

75. **Quote:** "Systems and applications built with generative models might eventually end up in production. When that happens, it is important that we verify and control the output of the model to prevent breaking the application and to create a robust generative AI application."
    - **Plain English:** Production apps need verified, controlled model output for robustness.
    - **Word meanings:** robust = reliable/strong.
    - **Technical terms:** output verification; production.

76. **Quote:** "Reasons for validating the output might include: Structured output — By default, most generative models create free-form text without adhering to specific structures other than those defined by natural language. Some use cases require their output to be structured in certain formats, like JSON. Valid output — Even if we allow the model to generate structured output, it still has the capability to freely generate its content. For instance, when a model is asked to output either one of two choices, it should not come up with a third. Ethics — Some open source generative models have no guardrails and will generate outputs that do not consider safety or ethical considerations. For instance, use cases might require the output to be free of profanity, personally identifiable information (PII), bias, cultural stereotypes, etc. Accuracy — Many use cases require the output to adhere to certain standards or performance. The aim is to double-check whether the generated information is factually accurate, coherent, or free from hallucination."
    - **Plain English:** Four validation reasons: structured output (e.g., JSON), valid output (no third option), ethics (no profanity/PII/bias/stereotypes), accuracy (factual, coherent, no hallucination).
    - **Technical terms:** structured output; valid output; ethics; PII; accuracy; hallucination; guardrails.

77. **Quote:** "Controlling the output of a generative model, as we explored with parameters like top_p and temperature, is not an easy feat. These models require help to generate consistent output conforming to certain guidelines. Generally, there are three ways of controlling the output of a generative model: Examples — Provide a number of examples of the expected output. Grammar — Control the token selection process. Fine-tuning — Tune a model on data that contains the expected output. In this section, we will go through the first two methods. The third, fine-tuning a model, is left for Chapter 12."
    - **Plain English:** Three control methods: Examples, Grammar (token selection), Fine-tuning (Ch 12). Chapter covers the first two.
    - **Word meanings:** feat = achievement.
    - **Technical terms:** examples; grammar (constrained sampling); fine-tuning.

### Providing Examples

78. **Quote:** "A simple and straightforward method to fix the output is to provide the generative model with examples of what the output should look like. As we explored before, few-shot learning is a helpful technique that guides the output of the generative model. This method can be generalized to guide the structure of the output as well."
    - **Plain English:** Few-shot examples guide both content and structure of output.
    - **Technical terms:** few-shot learning.

### Code Block: Zero-shot JSON (no examples)
```python
# Zero-shot learning: Providing no examples
zeroshot_prompt = [
    {"role": "user", "content": "Create a character profile for an RPG game in 
JSON format."}
]
# Generate the output
outputs = pipe(zeroshot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Asks for JSON without an example; output is truncated mid-attribute and invalid.
- **Fits the architecture:** Demonstrates why guidance is needed for structured output.

79. **Quote:** "The preceding truncated output is not valid JSON since the model stopped generating tokens after starting the 'charisma' attribute. Moreover, we might not want certain attributes. Instead, we can provide the model with a number of examples that indicate the expected format:"
    - **Plain English:** Zero-shot JSON was truncated/invalid; examples fix the format.
    - **Word meanings:** truncated = cut short.
    - **Technical terms:** valid JSON; format examples.

### Code Block: One-shot JSON (format template)
```python
# One-shot learning: Providing an example of the output structure
one_shot_template = """Create a short character profile for an RPG game. Make 
sure to only use this format:
{
  "description": "A SHORT DESCRIPTION",
  "name": "THE CHARACTER'S NAME",
  "armor": "ONE PIECE OF ARMOR",
  "weapon": "ONE OR MORE WEAPONS"
}
"""
one_shot_prompt = [
    {"role": "user", "content": one_shot_template}
]
# Generate the output
outputs = pipe(one_shot_prompt)
print(outputs[0]["generated_text"])
```
- **Explanation:** Provides a one-shot format template; the model follows it perfectly.
- **Fits the architecture:** Few-shot learning improves output structure, not just content.

80. **Quote:** "The model perfectly followed the example we gave it, which allows for more consistent behavior. This also demonstrates the importance of leveraging few-shot learning to improve the structure of the output and not only its content."
    - **Plain English:** Example templates produce consistent structured output.
    - **Technical terms:** consistency; few-shot learning.

81. **Quote:** "An important note here is that it is still up to the model whether it will adhere to your suggested format or not. Some models are better than others at following instructions."
    - **Plain English:** Models may not follow the suggested format — instruction-following varies by model.
    - **Word meanings:** adhere = follow/obey.
    - **Technical terms:** instruction-following.

### Grammar: Constrained Sampling

82. **Quote:** "Few-shot learning has a big disadvantage: we cannot explicitly prevent certain output from being generated. Although we guide the model and give it instructions, it might still not follow it entirely. Instead, packages have been rapidly developed to constrain and validate the output of generative models, like Guidance, Guardrails, and LMQL. In part, they leverage generative models to validate their own output, as illustrated in Figure 6-19. The generative models retrieve the output as new prompts and attempt to validate it based on a number of predefined guardrails."
    - **Plain English:** Few-shot can't prevent unwanted output; packages like Guidance, Guardrails, LMQL constrain/validate, sometimes using the LLM to check its own output against guardrails.
    - **Technical terms:** Guidance; Guardrails; LMQL; guardrails; validation.

83. **Quote:** "Similarly, as illustrated in Figure 6-20, this validation process can also be used to control the formatting of the output by generating parts of its format ourselves as we already know how it should be structured."
    - **Plain English:** Validation can control formatting by pre-generating known structure parts.
    - **Technical terms:** formatting control.

84. **Quote:** "This process can be taken one step further and instead of validating the output we can already perform validation during the token sampling process. When sampling tokens, we can define a number of grammars or rules that the LLM should adhere to when choosing its next token. For instance, if we ask the model to either return 'positive,' 'negative,' or 'neutral' when performing sentiment classification, it might still return something else. As illustrated in Figure 6-21, by constraining the sampling process, we can have the LLM only output what we are interested in. Note that this is still affected by parameters such as top_p and temperature."
    - **Plain English:** Constrained sampling = define grammars/rules during token selection (e.g., only positive/neutral/negative); still affected by top_p/temperature.
    - **Technical terms:** constrained sampling; grammars; token selection.

85. **Quote:** "Let us illustrate this phenomenon with llama-cpp-python, a library similar to transformers that we can use to load in our language model. It is generally used to efficiently load and use compressed models (through quantization; see Chapter 12) but we can also use it to apply a JSON grammar. We load the same model we used throughout this chapter but use a different format instead, namely GGUF. llama-cpp-python expects this format, which is generally used for compressed (quantized) models."
    - **Plain English:** llama-cpp-python loads compressed (GGUF/quantized) models and can apply JSON grammars.
    - **Technical terms:** llama-cpp-python; quantization; GGUF; JSON grammar.

### Code Block: Clearing VRAM before loading GGUF
```python
import gc
import torch
del model, tokenizer, pipe
# Flush memory
gc.collect()
torch.cuda.empty_cache()
```
- **Explanation:** Deletes the loaded model/tokenizer/pipeline and flushes GPU memory before loading the new GGUF model.
- **Fits the architecture:** Loading a new model requires free VRAM.

86. **Quote:** "Now that we have cleared the memory, we can load Phi-3. We set n_gpu_layers to -1 to indicate that we want all layers of the model to be run from the GPU. The n_ctx refers to the context size of the model. The repo_id and filename refer to the Hugging Face repository where the model resides:"
    - **Plain English:** n_gpu_layers=-1 = all layers on GPU; n_ctx = context size; repo_id/filename locate the model.
    - **Technical terms:** n_gpu_layers; n_ctx; repo_id; GGUF.

### Code Block: Loading Phi-3 GGUF with llama-cpp-python
```python
from llama_cpp.llama import Llama
# Load Phi-3
llm = Llama.from_pretrained(
    repo_id="microsoft/Phi-3-mini-4k-instruct-gguf",
    filename="*fp16.gguf",
    n_gpu_layers=-1,
    n_ctx=2048,
    verbose=False
)
```
- **Explanation:** Loads the GGUF Phi-3 model via llama-cpp-python with all layers on GPU and context size 2048.
- **Fits the architecture:** Efficient loading of a compressed model for constrained generation.

87. **Quote:** "To generate the output using the internal JSON grammar, we only need to specify the response_format as a JSON object. Under the hood, it will apply a JSON grammar to make sure the output adheres to that format."
    - **Plain English:** response_format={"type": "json_object"} applies a JSON grammar internally.
    - **Technical terms:** response_format; JSON grammar.

### Code Block: JSON grammar constrained sampling
```python
# Generate output
output = llm.create_chat_completion(
    messages=[
        {"role": "user", "content": "Create a warrior for an RPG in JSON for
mat."},
    ],
    response_format={"type": "json_object"},
    temperature=0,
)['choices'][0]['message']["content"]
import json
# Format as json
json_output = json.dumps(json.loads(output), indent=4)
print(json_output)
```
- **Explanation:** Generates with a JSON grammar (response_format) and temperature 0, then parses and pretty-prints the JSON.
- **Fits the architecture:** Constrained sampling guarantees format-valid output.

88. **Quote:** "The output is properly formatted as JSON. This allows us to more confidently use generative models in applications where we expect the output to adhere to certain formats."
    - **Plain English:** JSON grammar gives format-valid output, enabling production use.
    - **Technical terms:** format adherence.

### Summary

89. **Quote:** "In this chapter, we explored the basics of using generative models through prompt engineering and output verification. We focused on the creativity and potential complexity that comes with prompt engineering. These components of a prompt are key in generating and optimizing output appropriate for different use cases."
    - **Plain English:** Chapter covered prompt engineering basics + output verification; prompt components are key for optimizing output.
    - **Technical terms:** prompt components; output verification.

90. **Quote:** "We further explored advanced prompt engineering techniques such as in-context learning and chain-of-thought. These methods involve guiding generative models to reason through complex problems by providing examples or phrases that encourage step-by-step thinking thereby mimicking human reasoning processes."
    - **Plain English:** Advanced techniques (in-context learning, chain-of-thought) mimic human reasoning via examples/phrases.
    - **Technical terms:** in-context learning; chain-of-thought.

91. **Quote:** "Overall, this chapter demonstrated that prompt engineering is a crucial aspect of working with LLMs, as it allows us to effectively communicate our needs and preferences to the model. By mastering prompt engineering techniques, we can unlock some of the potential of LLMs and generate high-quality responses that meet our requirements."
    - **Plain English:** Prompt engineering is crucial for communicating with LLMs and unlocking their potential.
    - **Word meanings:** crucial = essential.
    - **Technical terms:** (none) — takeaway.

92. **Quote:** "The next chapter will build upon these concepts by exploring more advanced techniques for leveraging generative models. We will go beyond prompt engineering and explore how LLMs can use external memory and tools."
    - **Plain English:** Next chapter: LLMs with external memory and tools.
    - **Technical terms:** external memory; tools.
