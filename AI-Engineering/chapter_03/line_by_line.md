# Chapter 3 Line-by-Line: Evaluation Methodology
**Source:** *AI Engineering* (Chip Huyen), Chapter 3
**Format:** Each numbered item quotes a passage (or closely paraphrases it), then gives a plain-English explanation, word meanings, and technical terms. Figures and tables annotated.

---

## Opening

1. **Quote:** "The more AI is used, the more opportunity there is for catastrophic failure. We've already seen many failures in the short time that foundation models have been around. A man committed suicide after being encouraged by a chatbot. Lawyers submitted false evidence hallucinated by AI. Air Canada was ordered to pay damages when its AI chatbot gave a passenger false information."
   - **Plain English:** Real AI failures have already caused serious harm; this motivates the need for quality control.
   - **Technical terms:** hallucination; quality control; catastrophic failure.

2. **Quote:** "Without a way to quality control AI outputs, the risk of AI might outweigh its benefits for many applications."
   - **Plain English:** No evaluation = risks bigger than rewards.
   - **Word meanings:** outweigh = exceed in importance.
   - **Technical terms:** quality control; evaluation.

3. **Quote:** "As teams rush to adopt AI, many quickly realize that the biggest hurdle to bringing AI applications to reality is evaluation. For some applications, figuring out evaluation can take up the majority of the development effort."
   - **Plain English:** Evaluation is often the hardest part of shipping an AI app.
   - **Word meanings:** hurdle = obstacle.
   - **Technical terms:** evaluation; development effort.

4. **Quote (footnote):** "In December 2023, Greg Brockman, an OpenAI cofounder, tweeted that 'evals are surprisingly often all you need.'"
   - **Plain English:** Evaluation work alone can unlock most of the value.
   - **Word meanings:** evals = evaluations.
   - **Technical terms:** evals.

5. **Quote:** "Evaluation aims to mitigate risks and uncover opportunities. To mitigate risks, you first need to identify the places where your system is likely to fail and design your evaluation around them."
   - **Plain English:** Find where your system breaks first; then evaluate around those weak points.
   - **Word meanings:** mitigate = reduce.
   - **Technical terms:** risk mitigation; evaluation design.

6. **Quote:** "Without a clear understanding of where your system fails, no amount of evaluation metrics or tools can make the system robust."
   - **Plain English:** Metrics can't fix a system you don't understand.
   - **Technical terms:** evaluation metrics; robustness.

7. **Quote (footnote):** "A 2023 study by a16z showed that 6 out of 70 decision makers evaluated models by word of mouth."
   - **Plain English:** Most teams pick models informally, not systematically.
   - **Technical terms:** word of mouth; vibe check (eyeballing results).

8. **Quote:** "Since many foundation models have a language model component, this chapter will provide a quick overview of the metrics used to evaluate language models, including cross entropy and perplexity."
   - **Plain English:** LM metrics are important because most foundation models contain an LM.
   - **Technical terms:** cross entropy; perplexity; language model.

9. **Quote:** "Evaluating foundation models is especially challenging because they are open-ended, and I'll cover best practices for how to tackle these."
   - **Plain English:** Open-endedness is the core evaluation challenge.
   - **Technical terms:** open-ended generation; best practices.

10. **Quote:** "Using human evaluators remains a necessary option for many applications. However, given how slow and expensive human annotations can be, the goal is to automate the process. This book focuses on automatic evaluation, which includes both exact and subjective evaluation."
    - **Plain English:** Humans are needed but slow/costly; automation is the goal.
    - **Technical terms:** human evaluation; automatic evaluation; exact vs subjective evaluation.

11. **Quote:** "The rising star of subjective evaluation is AI as a judge—the approach of using AI to evaluate AI responses. It's subjective because the score depends on what model and prompt the AI judge uses."
    - **Plain English:** AI-judging-AI is the hot new method; scores vary with judge and prompt.
    - **Technical terms:** AI as a judge; AI judge; subjective evaluation.

## Challenges of Evaluating Foundation Models

12. **Quote:** "First, the more intelligent AI models become, the harder it is to evaluate them. Most people can tell if a first grader's math solution is wrong. Few can do the same for a PhD-level math solution."
    - **Plain English:** Smarter outputs need smarter evaluators.
    - **Technical terms:** evaluation difficulty; expertise.

13. **Quote:** "To validate the quality of a summary, you might need to read the book first. This brings us to a corollary: evaluation can be so much more time-consuming for sophisticated tasks."
    - **Plain English:** Checking sophisticated outputs requires deep effort (read the book first).
    - **Word meanings:** corollary = natural consequence.
    - **Technical terms:** evaluation cost; fact-checking.

14. **Quote (footnote):** "When OpenAI's GPT-o1 came out in September 2024, the Fields medalist Terrence Tao compared the experience of working with this model to working with 'a mediocre, but not completely incompetent, graduate student.'"
    - **Plain English:** Even a top mathematician sees frontier AI as a mid-level grad student.
    - **Word meanings:** mediocre = average/below-average quality.
    - **Technical terms:** GPT-o1.

15. **Quote:** "Many people joked that if we're already at the point where we need the brightest human minds to evaluate AI models, we'll have no one qualified to evaluate future models."
    - **Plain English:** If evaluation needs the best humans now, future models will be unjudgeable.
    - **Technical terms:** evaluation bottleneck.

16. **Quote:** "Second, the open-ended nature of foundation models undermines the traditional approach of evaluating a model against ground truths."
    - **Plain English:** You can't compare open-ended outputs to a fixed answer key.
    - **Word meanings:** undermines = weakens.
    - **Technical terms:** ground truth; open-ended tasks.

17. **Quote:** "With traditional ML, most tasks are close-ended. For example, a classification model can only output among the expected categories... However, for an open-ended task, for a given input, there are so many possible correct responses."
    - **Plain English:** Classification has finite answers; open-ended generation has infinitely many.
    - **Technical terms:** close-ended tasks; classification; open-ended tasks.

18. **Quote:** "Third, most foundation models are treated as black boxes, either because model providers choose not to expose models' details, or because application developers lack the expertise to understand them."
    - **Plain English:** You usually can't see inside the model.
    - **Technical terms:** black box; model architecture; training data.

19. **Quote:** "At the same time, publicly available evaluation benchmarks have proven to be inadequate for evaluating foundation models. A benchmark becomes saturated for a model once the model achieves the perfect score."
    - **Plain English:** Public benchmarks run out — models hit perfect scores fast.
    - **Technical terms:** benchmark saturation; evaluation benchmarks.

20. **Quote:** "The benchmark GLUE (General Language Understanding Evaluation) came out in 2018 and became saturated in just a year, necessitating the introduction of SuperGLUE in 2019."
    - **Plain English:** GLUE lasted about a year before being topped.
    - **Technical terms:** GLUE; SuperGLUE; saturation.

21. **Quote:** "Similarly, NaturalInstructions (2021) was replaced by Super-NaturalInstructions (2022). MMLU (2020), a strong benchmark that many early foundation models relied on, was largely replaced by MMLU-Pro (2024)."
    - **Plain English:** Benchmark generations keep getting superseded as models improve.
    - **Technical terms:** NaturalInstructions; Super-NaturalInstructions; MMLU; MMLU-Pro.

22. **Quote:** "Last but not least, the scope of evaluation has expanded for general-purpose models... evaluation is not only about assessing a model's performance on known tasks but also about discovering new tasks that the model can do, and these might include tasks that extend beyond human capabilities."
    - **Plain English:** Evaluation must now find new capabilities, not just score known ones.
    - **Technical terms:** general-purpose models; capability discovery.

23. **Quote:** "Figure 3-1 shows that the number of published papers on LLM evaluation grew exponentially every month in the first half of 2023, from 2 papers a month to almost 35 papers a month."
    - **Plain English:** Evaluation research exploded in early 2023.
    - **Technical terms:** LLM evaluation; exponential growth.

24. **Quote:** "In my own analysis of the top 1,000 AI-related repositories on GitHub, as ranked by the number of stars, I found over 50 repositories dedicated to evaluation (as of May 2024)."
    - **Plain English:** More than 50 of the most popular AI repos are evaluation-focused.
    - **Technical terms:** open source; evaluation repositories.

25. **Quote:** "Balduzzi et al. from DeepMind noted in their paper that 'developing evaluations has received little systematic attention compared to developing algorithms.'"
    - **Plain English:** Algorithms get more research attention than evaluation.
    - **Technical terms:** evaluation research; algorithmic research.

26. **Quote:** "According to the paper, experiment results are almost exclusively used to improve algorithms and are rarely used to improve evaluation."
    - **Plain English:** Results improve models, not the eval methods themselves.
    - **Technical terms:** evaluation improvement.

27. **Quote:** "Recognizing the lack of investments in evaluation, Anthropic called on policymakers to increase government funding and grants both for developing new evaluation methodologies and analyzing the robustness of existing evaluations."
    - **Plain English:** Anthropic asked governments to fund evaluation research.
    - **Technical terms:** evaluation methodologies; robustness analysis.

28. **Quote:** "Inadequate investment leads to inadequate infrastructure, making it hard for people to carry out systematic evaluations."
    - **Plain English:** Few tools → few people evaluate systematically.
    - **Technical terms:** evaluation infrastructure.

29. **Quote:** "The process of curating these prompts is ad hoc, usually based on the curator's personal experience instead of based on the application's needs. You might be able to get away with this ad hoc approach when getting a project off the ground, but it won't be sufficient for application iteration."
    - **Plain English:** Go-to prompts are improvised; fine to start, not enough to iterate.
    - **Word meanings:** ad hoc = improvised; curate = select carefully.
    - **Technical terms:** prompt curation; systematic evaluation.

## Understanding Language Modeling Metrics

30. **Quote:** "Many foundation models still have language models as their main components. For these models, the performance of the language model component tends to be well correlated to the foundation model's performance on downstream applications."
    - **Plain English:** LM quality predicts downstream app quality.
    - **Technical terms:** language model component; correlation; downstream performance.

31. **Quote:** "The metrics used to guide the development of language models haven't changed much since then [Claude Shannon, 1951, 'Prediction and Entropy of Printed English']. Most autoregressive language models are trained using cross entropy or its relative, perplexity."
    - **Plain English:** We still train LMs on 70-year-old metric ideas.
    - **Technical terms:** cross entropy; perplexity; autoregressive language model.

32. **Quote:** "When reading papers and model reports, you might also come across bits-per-character (BPC) and bits-per-byte (BPB); both are variations of cross entropy."
    - **Plain English:** BPC and BPB are cross-entropy variants.
    - **Technical terms:** bits-per-character (BPC); bits-per-byte (BPB).

33. **Quote:** "All four metrics—cross entropy, perplexity, BPC, and BPB—are closely related. If you know the value of one, you can compute the other three, given the necessary information."
    - **Plain English:** The four metrics are interchangeable with the right data.
    - **Technical terms:** cross entropy; perplexity; BPC; BPB.

34. **Quote:** "Statistically, given the context 'I like drinking __', the next word is more likely to be 'tea' than 'charcoal'. The more statistical information that a model can capture, the better it is at predicting the next token."
    - **Plain English:** Models encode how likely each token is in context.
    - **Technical terms:** token prediction; statistical information.

35. **Quote:** "A language model learns the distribution of its training data. The better this model learns, the better it is at predicting what comes next in the training data, and the lower its training cross entropy."
    - **Plain English:** Better learning → lower cross entropy.
    - **Technical terms:** data distribution; training cross entropy.

36. **Quote:** "In general, the closer your data is to a model's training data, the better the model can perform on your data."
    - **Plain English:** Match your data to the training data for best results.
    - **Technical terms:** data distribution shift; training data.

### Entropy

37. **Quote:** "Entropy measures how much information, on average, a token carries. The higher the entropy, the more information each token carries, and the more bits are needed to represent a token."
    - **Plain English:** Entropy = average information per token; higher = more bits.
    - **Technical terms:** entropy; bits; token.

38. **Quote (Figure 3-4 example):** "If your language has only two tokens, shown as (a) in Figure 3-4, each token can tell you whether the position is upper or lower. Since there are only two tokens, one bit is sufficient to represent them. The entropy of this language is, therefore, 1."
    - **Plain English:** 2 position words = 1 bit each = entropy 1.
    - **Technical terms:** entropy; bit.

39. **Quote (Figure 3-4 example):** "If your language has four tokens, shown as (b)... you need two bits to represent them. The entropy of this language is 2. This language has higher entropy, since each token carries more information."
    - **Plain English:** 4 tokens need 2 bits → entropy 2.
    - **Technical terms:** entropy; information.

40. **Quote:** "Intuitively, entropy measures how difficult it is to predict what comes next in a language. The lower a language's entropy (the less information a token of a language carries), the more predictable that language."
    - **Plain English:** Lower entropy = more predictable.
    - **Technical terms:** entropy; predictability.

41. **Quote (footnote, Shannon):** "The entropy is a statistical parameter which measures, in a certain sense, how much information is produced on the average for each letter of a text in the language. If the language is translated into binary digits (0 or 1) in the most efficient way, the entropy is the average number of binary digits required per letter of the original language."
    - **Plain English:** Shannon's definition: entropy = average bits per letter in optimal encoding.
    - **Technical terms:** entropy; bits per letter; Shannon.

### Cross Entropy

42. **Quote:** "A language model's cross entropy on a dataset measures how difficult it is for the language model to predict what comes next in this dataset."
    - **Plain English:** Cross entropy = how hard the model finds the data.
    - **Technical terms:** cross entropy.

43. **Quote:** "A model's cross entropy on the training data depends on two qualities: 1. The training data's predictability, measured by the training data's entropy. 2. How the distribution captured by the language model diverges from the true distribution of the training data."
    - **Plain English:** Cross entropy = data difficulty + model divergence.
    - **Technical terms:** entropy; distribution divergence.

44. **Quote:** "The training data's entropy is, therefore, H(P). The divergence of Q with respect to P can be measured using the Kullback–Leibler (KL) divergence, which is mathematically represented as D_KL(P‖Q). The model's cross entropy with respect to the training data is therefore: H(P, Q) = H(P) + D_KL(P‖Q)."
    - **Plain English:** Cross entropy = entropy + KL divergence.
    - **Technical terms:** KL divergence; H(P,Q); H(P).

45. **Quote:** "Cross entropy isn't symmetric. The cross entropy of Q with respect to P—H(P, Q)—is different from the cross entropy of P with respect to Q—H(Q, P)."
    - **Plain English:** Order matters; H(P,Q) ≠ H(Q,P).
    - **Technical terms:** asymmetry; cross entropy.

46. **Quote:** "A language model is trained to minimize its cross entropy with respect to the training data. If the language model learns perfectly from its training data, the model's cross entropy will be exactly the same as the entropy of the training data. The KL divergence of Q with respect to P will then be 0."
    - **Plain English:** Perfect learning → cross entropy = data entropy, KL = 0.
    - **Technical terms:** cross-entropy minimization; KL divergence.

### Bits-per-Character and Bits-per-Byte

47. **Quote:** "One unit of entropy and cross entropy is bits. If the cross entropy of a language model is 6 bits, this language model needs 6 bits to represent each token."
    - **Plain English:** Cross entropy in bits = bits per token.
    - **Technical terms:** bits; cross entropy.

48. **Quote:** "Since different models have different tokenization methods—for example, one model uses words as tokens and another uses characters as tokens—the number of bits per token isn't comparable across models. Some use the number of bits-per-character (BPC) instead."
    - **Plain English:** BPC makes metrics comparable across tokenizers.
    - **Technical terms:** tokenization; bits-per-character (BPC).

49. **Quote:** "If the number of bits per token is 6 and on average, each token consists of 2 characters, the BPC is 6/2 = 3."
    - **Plain English:** BPC = bits per token ÷ characters per token.
    - **Technical terms:** BPC.

50. **Quote:** "One complication with BPC arises from different character encoding schemes. For example, with ASCII, each character is encoded using 7 bits, but with UTF-8, a character can be encoded using anywhere between 8 and 32 bits."
    - **Plain English:** Character encodings vary, complicating BPC.
    - **Technical terms:** ASCII; UTF-8; character encoding.

51. **Quote:** "A more standardized metric would be bits-per-byte (BPB), the number of bits a language model needs to represent one byte of the original training data. If the BPC is 3 and each character is 7 bits, or 7/8 of a byte, then the BPB is 3 / (7/8) = 3.43."
    - **Plain English:** BPB = bits per byte; example 3 ÷ 7/8 ≈ 3.43.
    - **Technical terms:** bits-per-byte (BPB).

52. **Quote:** "Cross entropy tells us how efficient a language model will be at compressing text. If the BPB of a language model is 3.43... this language model can compress the original training text to less than half the text's original size."
    - **Plain English:** Lower BPB = better compression (3.43 bits/byte < half of 8).
    - **Technical terms:** compression; BPB.

### Perplexity

53. **Quote:** "Perplexity is the exponential of entropy and cross entropy. Perplexity is often shortened to PPL. Given a dataset with the true distribution P, its perplexity is defined as: PPL(P) = 2^H(P)."
    - **Plain English:** PPL = 2 raised to the cross entropy (in bits).
    - **Technical terms:** perplexity (PPL); exponential.

54. **Quote:** "If cross entropy measures how difficult it is for a model to predict the next token, perplexity measures the amount of uncertainty it has when predicting the next token. Higher uncertainty means there are more possible options for the next token."
    - **Plain English:** Perplexity = uncertainty about the next token.
    - **Technical terms:** perplexity; uncertainty.

55. **Quote (position-token example):** "The cross entropy of this language model is 2 bits. If this language model tries to predict a position in the square, it has to choose among 2^2 = 4 possible options. Thus, this language model has a perplexity of 4."
    - **Plain English:** 2-bit model → PPL = 2² = 4 (4 equally likely options).
    - **Technical terms:** perplexity; bits.

56. **Quote:** "Popular ML frameworks, including TensorFlow and PyTorch, use nat (natural log) as the unit for entropy and cross entropy. Nat uses the base of e... If you use nat as the unit, perplexity is the exponential of e: PPL(P,Q) = e^(H(P,Q))."
    - **Plain English:** TF/PyTorch use nats, so PPL = e^cross-entropy.
    - **Technical terms:** nat; natural log; base e.

57. **Quote (footnote):** "One reason many people might prefer natural log over log base 2 is because natural log has certain properties that makes its math easier. For example, the derivative of natural log ln(x) is 1/x."
    - **Plain English:** Natural log is preferred for its convenient calculus.
    - **Technical terms:** natural log; derivative.

58. **Quote:** "Due to the confusion around bit and nat, many people report perplexity, instead of cross entropy, when reporting their language models' performance."
    - **Plain English:** Teams report PPL to dodge the bit/nat confusion.
    - **Technical terms:** perplexity; cross entropy; bit; nat.

### Perplexity Interpretation and Use Cases

59. **Quote:** "What's considered a good value for perplexity depends on the data itself and how exactly perplexity is computed, such as how many previous tokens a model has access to."
    - **Plain English:** "Good PPL" depends on data and context length.
    - **Technical terms:** perplexity; context length.

60. **Quote (structured data rule):** "More structured data gives lower expected perplexity. For example, HTML code is more predictable than everyday text. If you see an opening HTML tag like <head>, you can predict that there should be a closing tag, </head>, nearby."
    - **Plain English:** Predictable (structured) data → lower PPL.
    - **Technical terms:** perplexity; structured data.

61. **Quote (vocabulary rule):** "The bigger the vocabulary, the higher the perplexity. Intuitively, the more possible tokens there are, the harder it is for the model to predict the next token... character-based perplexity will be lower than word-based perplexity."
    - **Plain English:** More candidate tokens → higher PPL.
    - **Technical terms:** vocabulary size; perplexity.

62. **Quote (context rule):** "The longer the context length, the lower the perplexity. The more context a model has, the less uncertainty it will have in predicting the next token. In 1951, Claude Shannon evaluated his model's cross entropy by using it to predict the next token conditioned on up to 10 previous tokens. As of this writing, a model's perplexity can typically be computed and conditioned on between 500 and 10,000 previous tokens."
    - **Plain English:** More context → lower PPL; we now use 500–10,000 tokens.
    - **Technical terms:** context length; perplexity.

63. **Quote:** "For reference, it's not uncommon to see perplexity values as low as 3 or even lower. If all tokens in a hypothetical language have an equal chance of happening, a perplexity of 3 means that this model has a 1 in 3 chance of predicting the next token correctly."
    - **Plain English:** PPL 3 = 1-in-3 odds of correct next token.
    - **Technical terms:** perplexity; token prediction.

64. **Quote:** "Given that a model's vocabulary is in the order of 10,000s and 100,000s, these odds are incredible."
    - **Plain English:** Guessing right 1 in 3 across ~100K options is remarkable.
    - **Technical terms:** vocabulary; perplexity.

65. **Quote:** "First, perplexity is a good proxy for a model's capabilities. If a model's bad at predicting the next token, its performance on downstream tasks will also likely be bad."
    - **Plain English:** PPL predicts downstream capability.
    - **Technical terms:** proxy; downstream tasks.

66. **Quote (Table 3-1):** "OpenAI's GPT-2 report shows that larger models, which are also more powerful models, consistently give lower perplexity on a range of datasets."
    - **Plain English:** Bigger GPT-2 models → lower PPL across datasets.
    - **Technical terms:** model scaling; perplexity.

67. **Quote (Table 3-1 LAMBADA PPL values):** "SOTA 99.8; 117M 35.13; 345M 15.60; 762M 10.87; 1542M 8.63."
    - **Plain English:** Steady PPL drop from 35.13 (117M) to 8.63 (1542M).
    - **Technical terms:** LAMBADA; perplexity.

68. **Quote (post-training caveat):** "Perplexity might not be a great proxy to evaluate models that have been post-trained using techniques like SFT and RLHF. Post-training is about teaching models how to complete tasks. As a model gets better at completing tasks, it might get worse at predicting the next tokens. A language model's perplexity typically increases after post-training. Some people say that post-training collapses entropy."
    - **Plain English:** After SFT/RLHF the model is better at tasks but worse at next-token prediction → PPL rises.
    - **Technical terms:** post-training; SFT; RLHF; entropy collapse.

69. **Quote:** "Similarly, quantization—a technique that reduces a model's numerical precision and, with it, its memory footprint—can also change a model's perplexity in unexpected ways."
    - **Plain English:** Quantization shifts PPL unpredictably.
    - **Technical terms:** quantization; numerical precision.

70. **Quote (contamination):** "For a given model, perplexity is the lowest for texts that the model has seen and memorized during training. Therefore, perplexity can be used to detect whether a text was in a model's training data. This is useful for detecting data contamination—if a model's perplexity on a benchmark's data is low, this benchmark was likely included in the model's training data."
    - **Plain English:** Low PPL on benchmark data = likely data contamination.
    - **Technical terms:** data contamination; perplexity.

71. **Quote (deduplication):** "This can also be used for deduplication of training data: e.g., add new data to the existing training dataset only if the perplexity of the new data is high."
    - **Plain English:** Add new training data only if the model finds it surprising (high PPL).
    - **Technical terms:** deduplication; perplexity.

72. **Quote (abnormal text):** "Perplexity is the highest for unpredictable texts, such as texts expressing unusual ideas (like 'my dog teaches quantum physics in his free time') or gibberish (like 'home cat go eye'). Therefore, perplexity can be used to detect abnormal texts."
    - **Plain English:** High PPL flags unusual or gibberish text.
    - **Technical terms:** anomaly detection; perplexity.

73. **Quote (boxed):** "To compute perplexity, you need access to the probabilities (or logprobs) the language model assigns to each next token. Unfortunately, not all commercial models expose their models' logprobs."
    - **Plain English:** Computing PPL needs logprobs, which not all APIs give.
    - **Technical terms:** logprobs; perplexity.

## Exact Evaluation

74. **Quote:** "Exact evaluation produces judgment without ambiguity. For example, if the answer to a multiple-choice question is A and you pick B, your answer is wrong. There's no ambiguity around that."
    - **Plain English:** Exact = unambiguous, right-or-wrong.
    - **Technical terms:** exact evaluation.

75. **Quote:** "On the other hand, essay grading is subjective. An essay's score depends on who grades the essay... As you'll see in the next section, AI as a judge is subjective. The evaluation result can change based on the judge model and the prompt."
    - **Plain English:** Subjective = depends on grader/judge/prompt.
    - **Technical terms:** subjective evaluation; AI as a judge.

76. **Quote:** "I'll cover two evaluation approaches that produce exact scores: functional correctness and similarity measurements against reference data."
    - **Plain English:** Two exact approaches: functional correctness + similarity.
    - **Technical terms:** functional correctness; reference data.

77. **Quote:** "Note that this section focuses on evaluating open-ended responses (arbitrary text generation) as opposed to close-ended responses (such as classification)... This section focuses on open-ended evaluation because close-ended evaluation is already well understood."
    - **Plain English:** Chapter targets open-ended evaluation; classification is well understood.
    - **Technical terms:** open-ended vs close-ended evaluation.

### Functional Correctness

78. **Quote:** "Functional correctness evaluation means evaluating a system based on whether it performs the intended functionality. For example, if you ask a model to create a website, does the generated website meet your requirements? If you ask a model to make a reservation at a certain restaurant, does the model succeed?"
    - **Plain English:** Does the output actually do the job?
    - **Technical terms:** functional correctness; intended functionality.

79. **Quote:** "Functional correctness is the ultimate metric for evaluating the performance of any application, as it measures whether your application does what it's intended to do. However, functional correctness isn't always straightforward to measure, and its measurement can't be easily automated."
    - **Plain English:** It's the gold standard but hard to automate.
    - **Technical terms:** functional correctness; automation.

80. **Quote (code example):** "Code generation is an example of a task where functional correctness measurement can be automated. Functional correctness in coding is sometimes execution accuracy. Say you ask the model to write a Python function, gcd(num1, num2)... The generated code can then be input into a Python interpreter to check whether the code is valid and if it is, whether it outputs the correct result."
    - **Plain English:** Run the code; check the output — automated functional correctness.
    - **Technical terms:** execution accuracy; Python interpreter.

81. **Quote:** "Long before AI was used for writing code, automatically verifying code's functional correctness was standard practice in software engineering. Code is typically validated with unit tests... Functional correctness evaluation is how coding platforms like LeetCode and HackerRank validate the submitted solutions."
    - **Plain English:** Unit testing is an old, standard technique now applied to AI.
    - **Technical terms:** unit tests; functional correctness.

82. **Quote:** "Popular benchmarks for evaluating AI's code generation capabilities, such as OpenAI's HumanEval and Google's MBPP (Mostly Basic Python Problems Dataset) use functional correctness as their metrics."
    - **Plain English:** HumanEval and MBPP measure code by execution.
    - **Technical terms:** HumanEval; MBPP.

83. **Quote:** "Benchmarks for text-to-SQL (generating SQL queries from natural languages) like Spider (Yu et al., 2018), BIRD-SQL (Li et al., 2023), and WikiSQL (Zhong et al., 2017) also rely on functional correctness."
    - **Plain English:** Text-to-SQL benchmarks run the SQL to check correctness.
    - **Technical terms:** text-to-SQL; Spider; BIRD-SQL; WikiSQL.

84. **Quote (HumanEval example):** "A benchmark problem comes with a set of test cases. Each test case consists of a scenario the code should run and the expected output for that scenario."
    - **Plain English:** Problem + test cases (assert statements) = benchmark format.
    - **Technical terms:** test cases; expected output.

85. **Quote (pass@k):** "When evaluating a model, for each problem a number of code samples, denoted as k, are generated. A model solves a problem if any of the k code samples it generated pass all of that problem's test cases. The final score, called pass@k, is the fraction of the solved problems out of all problems. If there are 10 problems and a model solves 5 with k = 3, then that model's pass@3 score is 50%."
    - **Plain English:** pass@k = % of problems solved by ≥1 of k samples.
    - **Technical terms:** pass@k; code samples; test cases.

86. **Quote:** "The more code samples a model generates, the more chance the model has at solving each problem, hence the greater the final score. This means that in expectation, pass@1 score should be lower than pass@3, which, in turn, should be lower than pass@10."
    - **Plain English:** More samples → higher pass@k in expectation.
    - **Technical terms:** pass@1; pass@3; pass@10.

87. **Quote (game bots):** "Another category of tasks whose functional correctness can be automatically evaluated is game bots. If you create a bot to play Tetris, you can tell how good the bot is by the score it gets. Tasks with measurable objectives can typically be evaluated using functional correctness."
    - **Plain English:** Measurable objectives (scores, energy saved) enable functional correctness.
    - **Technical terms:** game bots; measurable objectives.

88. **Quote (footnote):** "Sometimes, evaluating a part of a solution is harder than evaluating the end outcome. Imagine you want to evaluate someone's ability to play chess. It's easier to evaluate the end game outcome (win/lose/draw) than to evaluate just one move."
    - **Plain English:** End outcomes are easier to judge than single moves.
    - **Technical terms:** evaluation granularity.

### Similarity Measurements Against Reference Data

89. **Quote:** "If the task you care about can't be automatically evaluated using functional correctness, one common approach is to evaluate AI's outputs against reference data."
    - **Plain English:** No functional correctness → compare to references.
    - **Technical terms:** reference data.

90. **Quote:** "Each example in the reference data follows the format (input, reference responses). An input can have multiple reference responses... Reference responses are also called ground truths or canonical responses. Metrics that require references are reference-based, and metrics that don't are reference-free."
    - **Plain English:** References come in sets; metrics are reference-based or free.
    - **Technical terms:** reference responses; ground truths; canonical responses; reference-based/free metrics.

91. **Quote:** "Since this evaluation approach requires reference data, it's bottlenecked by how much and how fast reference data can be generated. Reference data is generated typically by humans and increasingly by AIs."
    - **Plain English:** Reference generation is the bottleneck.
    - **Technical terms:** reference data; bottleneck.

92. **Quote:** "Using human-generated data as the reference means that we treat human performance as the gold standard, and AI's performance is measured against human performance."
    - **Plain English:** Human data = gold standard.
    - **Technical terms:** gold standard; human performance.

93. **Quote:** "AI-generated data might still need human reviews, but the labor needed to review it is much less than the labor needed to generate reference data from scratch."
    - **Plain English:** AI generates references; humans just review.
    - **Technical terms:** AI-generated reference data.

94. **Quote:** "There are four ways to measure the similarity between two open-ended texts: 1. Asking an evaluator to make the judgment whether two texts are the same. 2. Exact match: whether the generated response matches one of the reference responses exactly. 3. Lexical similarity: how similar the generated response looks to the reference responses. 4. Semantic similarity: how close the generated response is to the reference responses in meaning (semantics)."
    - **Plain English:** Four similarity approaches: judgment, exact match, lexical, semantic.
    - **Technical terms:** exact match; lexical similarity; semantic similarity.

95. **Quote:** "Scores by exact matching are binary (match or not), whereas the other two scores are on a sliding scale (such as between 0 and 1 or between –1 and 1)."
    - **Plain English:** Exact match is binary; lexical/semantic are continuous.
    - **Technical terms:** binary scores; sliding scale.

96. **Quote (boxed):** "Despite the ease of use and flexibility of the AI as a judge approach, hand-designed similarity measurements are still widely used in the industry for their exact nature."
    - **Plain English:** Industry still likes exact, hand-designed similarity metrics.
    - **Technical terms:** hand-designed metrics; similarity measurements.

97. **Quote (boxed, other uses):** "You can also use similarity measurements for many other use cases, including but not limited to the following: Retrieval and search, Ranking, Clustering, Anomaly detection, Data deduplication."
    - **Plain English:** Similarity powers search, ranking, clustering, anomaly detection, dedup.
    - **Technical terms:** retrieval; ranking; clustering; anomaly detection; deduplication.

### Exact Match

98. **Quote:** "It's considered an exact match if the generated response matches one of the reference responses exactly. Exact matching works for tasks that expect short, exact responses such as simple math problems, common knowledge queries, and trivia-style questions."
    - **Plain English:** Exact match works on short, unambiguous answers.
    - **Technical terms:** exact match.

99. **Quote (looser variant):** "One variation is to accept any output that contains the reference response as a match. Consider the question 'What's 2 + 3?' The reference response is '5'. This variation accepts all outputs that contain '5', including 'The answer is 5' and '2 + 3 is 5'."
    - **Plain English:** "Contains" matching accepts paraphrases that include the answer.
    - **Technical terms:** contains-match.

100. **Quote (trap):** "However, this variation can sometimes lead to the wrong solution being accepted. Consider the question 'What year was Anne Frank born?'... If the model outputs 'September 12, 1929', the correct year is included in the output, but the output is factually wrong."
     - **Plain English:** "Contains 1929" accepts a wrong full answer.
     - **Technical terms:** false positive; exact-match pitfall.

101. **Quote (translation):** "Given the original French sentence 'Comment ça va?', there are multiple possible English translations... If the reference data contains only these three translations and a model generates 'How is it going?', the model's response will be marked as wrong. The longer and more complex the original text, the more possible translations there are. It's impossible to create an exhaustive set of possible responses for an input."
     - **Plain English:** Exact match fails when many valid answers exist.
     - **Technical terms:** exact match; exhaustive reference set.

### Lexical Similarity

102. **Quote:** "Lexical similarity measures how much two texts overlap. You can do this by first breaking each text into smaller tokens."
     - **Plain English:** Lexical similarity = token overlap.
     - **Technical terms:** lexical similarity; tokens.

103. **Quote (word-overlap example):** "Consider the reference response 'My cats scare the mice' and two generated responses: 'My cats eat the mice' and 'Cats and mice fight all the time'... response A contains 4 out of 5 words in the reference response (the similarity score is 80%), whereas response B contains only 3 out of 5 (the similarity score is 60%). Response A is, therefore, considered more similar."
     - **Plain English:** 4/5 common words = 80% > 3/5 = 60%.
     - **Technical terms:** token overlap; lexical similarity.

104. **Quote (edit distance):** "One way to measure lexical similarity is approximate string matching, known colloquially as fuzzy matching. It measures the similarity between two texts by counting how many edits it'd need to convert from one text to another, a number called edit distance. The usual three edit operations are: 1. Deletion: 'brad' -> 'bad'. 2. Insertion: 'bad' -> 'bard'. 3. Substitution: 'bad' -> 'bed'."
     - **Plain English:** Edit distance = number of edits to transform one string into another.
     - **Technical terms:** fuzzy matching; edit distance; deletion; insertion; substitution.

105. **Quote (transposition):** "Some fuzzy matchers also treat transposition, swapping two letters (e.g., 'mats' -> 'mast'), to be an edit. However, some fuzzy matchers treat each transposition as two edit operations: one deletion and one insertion."
     - **Plain English:** Transposition counts as 1 or 2 edits depending on the matcher.
     - **Technical terms:** transposition; edit distance.

106. **Quote (n-grams):** "Another way to measure lexical similarity is n-gram similarity, measured based on the overlapping of sequences of tokens, n-grams, instead of single tokens. A 1-gram (unigram) is a token. A 2-gram (bigram) is a set of two tokens. 'My cats scare the mice' consists of four bigrams: 'my cats', 'cats scare', 'scare the', and 'the mice'."
     - **Plain English:** n-grams = token sequences; example sentence has 4 bigrams.
     - **Technical terms:** n-gram; unigram; bigram.

107. **Quote (metrics):** "Common metrics for lexical similarity are BLEU, ROUGE, METEOR++, TER, and CIDEr. They differ in exactly how the overlapping is calculated. Before foundation models, BLEU, ROUGE, and their relatives were common, especially for translation tasks."
     - **Plain English:** BLEU/ROUGE/METEOR++/TER/CIDEr are lexical-similarity metrics, once dominant.
     - **Technical terms:** BLEU; ROUGE; METEOR++; TER; CIDEr.

108. **Quote (benchmarks):** "Examples of benchmarks that use these metrics are WMT, COCO Captions, and GEMv2."
     - **Plain English:** WMT/COCO Captions/GEMv2 still use lexical metrics.
     - **Technical terms:** WMT; COCO Captions; GEMv2.

109. **Quote (drawback 1 — missing references):** "A drawback of this method is that it requires curating a comprehensive set of reference responses. A good response can get a low similarity score if the reference set doesn't contain any response that looks like it. On some benchmark examples, Adept found that its model Fuyu performed poorly not because the model's outputs were wrong, but because some correct answers were missing in the reference data."
     - **Plain English:** Good answers score low when the reference set is incomplete (Fuyu example).
     - **Technical terms:** reference coverage; Fuyu.

110. **Quote (drawback 2 — wrong references):** "Not only that, but references can be wrong. For example, the organizers of the WMT 2023 Metrics shared task... reported that they found many bad reference translations in their data. Low-quality reference data is one of the reasons that reference-free metrics were strong contenders for reference-based metrics in terms of correlation to human judgment."
     - **Plain English:** Bad references exist; this boosted reference-free metrics.
     - **Technical terms:** reference quality; reference-free metrics.

111. **Quote (drawback 3 — BLEU ≠ correctness):** "Another drawback of this measurement is that higher lexical similarity scores don't always mean better responses. For example, on HumanEval, a code generation benchmark, OpenAI found that BLEU scores for incorrect and correct solutions were similar. This indicates that optimizing for BLEU scores isn't the same as optimizing for functional correctness."
     - **Plain English:** BLEU doesn't distinguish correct from incorrect code.
     - **Technical terms:** BLEU; functional correctness.

### Semantic Similarity

112. **Quote:** "Lexical similarity measures whether two texts look similar, not whether they have the same meaning. Consider the two sentences 'What's up?' and 'How are you?' Lexically, they are different—there's little overlapping in the words and letters they use. However, semantically, they are close."
     - **Plain English:** Different words, same meaning — lexical misses it.
     - **Technical terms:** lexical similarity; semantic similarity.

113. **Quote:** "Conversely, similar-looking texts can mean very different things. 'Let's eat, grandma' and 'Let's eat grandma' mean two completely different things."
     - **Plain English:** Same-looking text, different meaning — the comma changes everything.
     - **Technical terms:** semantic ambiguity.

114. **Quote:** "Semantic similarity aims to compute the similarity in semantics. This first requires transforming a text into a numerical representation, which is called an embedding. For example, the sentence 'the cat sits on a mat' might be represented using an embedding that looks like this: [0.11, 0.02, 0.54]. Semantic similarity is, therefore, also called embedding similarity."
     - **Plain English:** Convert text to a vector, then measure distance.
     - **Technical terms:** embedding; embedding similarity.

115. **Quote:** "The similarity between two embeddings can be computed using metrics such as cosine similarity. Two embeddings that are exactly the same have a similarity score of 1. Two opposite embeddings have a similarity score of –1."
     - **Plain English:** Cosine similarity: same = 1, opposite = –1.
     - **Technical terms:** cosine similarity.

116. **Quote:** "I'm using text examples, but semantic similarity can be computed for embeddings of any data modality, including images and audio. Semantic similarity for text is sometimes called semantic textual similarity."
     - **Plain English:** Applies to any modality; text version = STS.
     - **Technical terms:** modality; semantic textual similarity (STS).

117. **Quote (boxed):** "While I put semantic similarity in the exact evaluation category, it can be considered subjective, as different embedding algorithms can produce different embeddings. However, given two embeddings, the similarity score between them is computed exactly."
     - **Plain English:** Embedding choice is subjective; the score itself is exact.
     - **Technical terms:** exact vs subjective; embedding algorithms.

118. **Quote (cosine formula):** "Mathematically, let A be an embedding of the generated response, and B be an embedding of a reference response. The cosine similarity between A and B is computed as A·B / (‖A‖‖B‖), with A·B being the dot product of A and B, and ‖A‖ being the Euclidean norm (also known as L2 norm) of A."
     - **Plain English:** cos(A,B) = dot product ÷ (norms product).
     - **Technical terms:** cosine similarity; dot product; Euclidean (L2) norm.

119. **Quote (metrics):** "Metrics for semantic textual similarity include BERTScore (embeddings are generated by BERT) and MoverScore (embeddings are generated by a mixture of algorithms)."
     - **Plain English:** BERTScore (BERT) and MoverScore (mixed algorithms) measure semantic similarity.
     - **Technical terms:** BERTScore; MoverScore; BERT.

120. **Quote (drawbacks):** "Semantic textual similarity doesn't require a set of reference responses as comprehensive as lexical similarity does. However, the reliability of semantic similarity depends on the quality of the underlying embedding algorithm. Two texts with the same meaning can still have a low semantic similarity score if their embeddings are bad."
     - **Plain English:** Needs fewer references but depends on embedding quality.
     - **Technical terms:** embedding quality.

### Introduction to Embedding

121. **Quote:** "An embedding is a numerical representation that aims to capture the meaning of the original data. An embedding is a vector. For example, the sentence 'the cat sits on a mat' might be represented using an embedding vector that looks like this: [0.11, 0.02, 0.54]. Here, I use a small vector as an example. In reality, the size of an embedding vector (the number of elements in the embedding vector) is typically between 100 and 10,000."
     - **Plain English:** Embeddings = vectors (100–10,000 elements) capturing meaning.
     - **Technical terms:** embedding; vector.

122. **Quote:** "Models trained especially to produce embeddings include the open source models BERT, CLIP (Contrastive Language–Image Pre-training), and Sentence Transformers. There are also proprietary embedding models provided as APIs."
     - **Plain English:** Embedding models: BERT, CLIP, Sentence Transformers + proprietary APIs.
     - **Technical terms:** BERT; CLIP; Sentence Transformers.

123. **Quote (Table 3-2 sizes):** "Google's BERT: BERT base 768, BERT large 1024. OpenAI's CLIP: Image 512, Text 512. OpenAI Embeddings API: text-embedding-3-small 1536, text-embedding-3-large 3072. Cohere's Embed v3: embed-english-v3.0 1024, embed-english-light-3.0 384."
     - **Plain English:** Common embedding sizes: BERT 768/1024, CLIP 512, OpenAI 1536/3072, Cohere 1024/384.
     - **Technical terms:** embedding size; BERT; CLIP; Cohere.

124. **Quote:** "Because models typically require their inputs to first be transformed into vector representations, many ML models, including GPTs and Llamas, also involve a step to generate embeddings... If you have access to the intermediate layers of these models, you can use them to extract embeddings. However, the quality of these embeddings might not be as good as the embeddings generated by specialized embedding models."
     - **Plain English:** GPTs/Llamas have embeddings too, but intermediate-layer ones are weaker.
     - **Technical terms:** intermediate layers; specialized embedding models.

125. **Quote:** "While a 10,000-element vector space seems high-dimensional, it's much lower than the dimensionality of the raw data. An embedding is, therefore, considered a representation of complex data in a lower-dimensional space."
     - **Plain English:** Embeddings compress complex data into lower dimensions.
     - **Technical terms:** dimensionality; lower-dimensional representation.

126. **Quote (word embeddings, footnote):** "There are also models that generate word embeddings, as opposed to document embeddings, such as word2vec (Mikolov et al., 2013) and GloVe."
     - **Plain English:** word2vec/GloVe produce word-level embeddings.
     - **Technical terms:** word2vec; GloVe; document embeddings.

127. **Quote:** "At a high level, an embedding algorithm is considered good if more-similar texts have closer embeddings, measured by cosine similarity or related metrics. The embedding of the sentence 'the cat sits on a mat' should be closer to the embedding of 'the dog plays on the grass' than the embedding of 'AI research is super fun'."
     - **Plain English:** Good embeddings keep similar meanings close together.
     - **Technical terms:** embedding quality; cosine similarity.

128. **Quote:** "You can also evaluate the quality of embeddings based on their utility for your task. Embeddings are used in many tasks, including classification, topic modeling, recommender systems, and RAG. An example of benchmarks that measure embedding quality on multiple tasks is MTEB, Massive Text Embedding Benchmark (Muennighoff et al., 2023)."
     - **Plain English:** Embeddings are evaluated by task utility; MTEB is the benchmark.
     - **Technical terms:** MTEB; recommender systems; RAG.

129. **Quote:** "Any data can have embedding representations. For example, ecommerce solutions like Criteo and Coveo have embeddings for products. Pinterest has embeddings for images, graphs, queries, and even users."
     - **Plain English:** Embeddings exist for products, images, queries, users, etc.
     - **Technical terms:** product embeddings; graph embeddings.

130. **Quote (joint embeddings):** "A new frontier is to create joint embeddings for data of different modalities. CLIP (Radford et al., 2021) was one of the first major models that could map data of different modalities, text and images, into a joint embedding space. ULIP (Xue et al., 2022) aims to create unified representations of text, images, and 3D point clouds. ImageBind (Girdhar et al., 2023) learns a joint embedding across six different modalities."
     - **Plain English:** CLIP (text+images), ULIP (+3D point clouds), ImageBind (6 modalities) map different data types into shared spaces.
     - **Technical terms:** joint embedding; multimodal; CLIP; ULIP; ImageBind.

131. **Quote (CLIP training):** "CLIP is trained using (image, text) pairs... For each (image, text) pair, CLIP uses a text encoder to convert the text to a text embedding, and an image encoder to convert the image to an image embedding. It then projects both these embeddings into a joint embedding space. The training goal is to get the embedding of an image close to the embedding of the corresponding text in this joint space."
     - **Plain English:** CLIP aligns matching image/text embeddings in a shared space.
     - **Technical terms:** text encoder; image encoder; joint embedding space.

132. **Quote (text-based image search):** "In a text–image joint embedding space, the embedding of an image of a man fishing should be closer to the embedding of the text 'a fisherman' than the embedding of the text 'fashion show'... this enables text-based image search. Given a text, it helps you find images closest to this text."
     - **Plain English:** Joint spaces enable searching images by text.
     - **Technical terms:** text-based image search; multimodal embedding space.

## AI as a Judge

133. **Quote:** "The approach of using AI to evaluate AI is called AI as a judge or LLM as a judge. An AI model that is used to evaluate other AI models is called an AI judge."
     - **Plain English:** AI-as-a-judge = AI evaluating AI; the model is the AI judge.
     - **Technical terms:** AI as a judge; LLM as a judge; AI judge.

134. **Quote (history):** "It only became practical when AI models became capable of doing so, which was around 2020 with the release of GPT-3. As of this writing, AI as a judge has become one of the most, if not the most, common methods for evaluating AI models in production."
     - **Plain English:** GPT-3 (2020) made AI judges practical; now ubiquitous.
     - **Technical terms:** AI as a judge; production evaluation.

135. **Quote (usage stat):** "LangChain's State of AI report in 2023 noted that 58% of evaluations on their platform were done by AI judges."
     - **Plain English:** 58% of LangChain evals used AI judges in 2023.
     - **Technical terms:** AI judges; evaluation platform.

### Why AI as a Judge?

136. **Quote:** "AI judges are fast, easy to use, and relatively cheap compared to human evaluators. They can also work without reference data, which means they can be used in production environments where there is no reference data."
     - **Plain English:** Fast, cheap, reference-free — production-friendly.
     - **Technical terms:** AI judge; reference data.

137. **Quote:** "You can ask AI models to judge an output based on any criteria: correctness, repetitiveness, toxicity, wholesomeness, hallucinations, and more."
     - **Plain English:** Judge on any criterion you can prompt.
     - **Technical terms:** evaluation criteria.

138. **Quote (masses):** "However, as each AI model is an aggregation of the masses, it's possible for AI models to make judgments representative of the masses. With the right prompt for the right model, you can get reasonably good judgments on a wide range of topics."
     - **Plain English:** Models aggregate human opinions → mass-representative judgments.
     - **Technical terms:** aggregation of the masses.

139. **Quote (human correlation):** "In 2023, Zheng et al. found that on their evaluation benchmark, MT-Bench, the agreement between GPT-4 and humans reached 85%, which is even higher than the agreement among humans (81%)."
     - **Plain English:** GPT-4 agrees with humans (85%) more than humans agree with each other (81%).
     - **Technical terms:** MT-Bench; inter-annotator agreement.

140. **Quote (AlpacaEval):** "AlpacaEval authors (Dubois et al., 2023) also found that their AI judges have a near perfect (0.98) correlation with LMSYS's Chat Arena leaderboard, which is evaluated by humans."
     - **Plain English:** AlpacaEval's AI judge correlates 0.98 with human-run Chatbot Arena.
     - **Technical terms:** AlpacaEval; LMSYS Chatbot Arena.

141. **Quote (explanations):** "Not only can AI evaluate a response, but it can also explain its decision, which can be especially useful when you want to audit your evaluation results."
     - **Plain English:** AI judges can explain their scores → auditable.
     - **Technical terms:** explainability; audit.

142. **Quote:** "Even when AI judgments aren't as good as human judgments, they might still be good enough to guide an application's development and provide sufficient confidence to get a project off the ground."
     - **Plain English:** Good-enough judgments suffice to start projects.
     - **Technical terms:** good-enough evaluation.

### How to Use AI as a Judge

143. **Quote (three modes):** "For example, you can use AI to evaluate the quality of a response by itself, compare that response to reference data, or compare that response to another response."
     - **Plain English:** Three judge modes: self-score, vs reference, vs another response.
     - **Technical terms:** evaluation modes.

144. **Quote (mode 3 → preference data):** "Compare two generated responses and determine which one is better or predict which one users will likely prefer. This is helpful for generating preference data for post-training alignment, test-time compute, and ranking models using comparative evaluation."
     - **Plain English:** Comparing two responses creates preference data for alignment, test-time compute, ranking.
     - **Technical terms:** preference data; post-training alignment; test-time compute; comparative evaluation.

145. **Quote (Table 3-3):** "Table 3-3 shows common built-in AI as a judge criteria offered by some AI tools... Azure AI Studio: Groundedness, relevance, coherence, fluency, similarity. MLflow.metrics: Faithfulness, relevance. LangChain Criteria Evaluation: Conciseness, relevance, correctness, coherence, harmfulness... Ragas: Faithfulness, answer relevance."
     - **Plain English:** Tools ship built-in criteria (relevance, faithfulness, etc.).
     - **Technical terms:** built-in criteria; Azure AI Studio; MLflow; LangChain; Ragas.

146. **Quote (not standardized):** "It's essential to remember that AI as a judge criteria aren't standardized. Azure AI Studio's relevance scores might be very different from MLflow's relevance scores. These scores depend on the judge's underlying model and prompt."
     - **Plain English:** Same-named criteria differ across tools.
     - **Technical terms:** criteria standardization; relevance.

147. **Quote (prompt structure):** "In general, a judge's prompt should clearly explain the following: 1. The task the model is to perform. 2. The criteria the model should follow to evaluate... The more detailed the instruction, the better. 3. The scoring system."
     - **Plain English:** Judge prompts need: task, criteria (detailed), scoring system.
     - **Technical terms:** judge prompt; scoring system.

148. **Quote (scoring systems):** "The scoring system, which can be one of these: Classification, such as good/bad or relevant/irrelevant/neutral. Discrete numerical values, such as 1 to 5. Discrete numerical values can be considered a special case of classification... Continuous numerical values, such as between 0 and 1."
     - **Plain English:** Scoring = classification | discrete numeric | continuous numeric.
     - **Technical terms:** classification; discrete values; continuous values.

149. **Quote (text > numbers):** "Language models are generally better with text than with numbers. It's been reported that AI judges work better with classification than with numerical scoring systems. For numerical scoring systems, discrete scoring seems to work better than continuous scoring. Empirically, the wider the range for discrete scoring, the worse the model seems to get."
     - **Plain English:** Use classification, then discrete; avoid wide ranges.
     - **Technical terms:** classification; discrete scoring; continuous scoring.

150. **Quote (examples help):** "Prompts with examples have been shown to perform better. If you use a scoring system between 1 and 5, include examples of what a response with a score of 1, 2, 3, 4, or 5 looks like, and if possible, why a response receives a certain score."
     - **Plain English:** Show the judge example responses per score.
     - **Technical terms:** few-shot prompting; scoring rubric.

151. **Quote (system view):** "An AI judge is not just a model—it's a system that includes both a model and a prompt. Altering the model, the prompt, or the model's sampling parameters results in a different judge."
     - **Plain English:** Judge = model + prompt + sampling parameters.
     - **Technical terms:** AI judge system; sampling parameters.

### Limitations of AI as a Judge

152. **Quote (tautology/uncertainty):** "Using AI to evaluate AI seems tautological. The probabilistic nature of AI makes it seem too unreliable to act as an evaluator. AI judges can potentially introduce nontrivial costs and latency to an application."
     - **Plain English:** Skepticism: circular, probabilistic, costly, slow.
     - **Technical terms:** tautology; probabilistic; latency.

153. **Quote (inconsistency):** "Yet AI judges, like all AI applications, are probabilistic. The same judge, on the same input, can output different scores if prompted differently. Even the same judge, prompted with the same instruction, can output different scores if run twice. This inconsistency makes it hard to reproduce or trust evaluation results."
     - **Plain English:** Same input can yield different scores → reproducibility problem.
     - **Technical terms:** inconsistency; reproducibility.

154. **Quote (examples raise consistency):** "Zheng et al. (2023) showed that including evaluation examples in the prompt can increase the consistency of GPT-4 from 65% to 77.5%. However, they acknowledged that high consistency may not imply high accuracy—the judge might consistently make the same mistakes. On top of that, including more examples makes prompts longer, and longer prompts mean higher inference costs. In Zheng et al.'s experiment, including more examples in their prompts caused their GPT-4 spending to quadruple."
     - **Plain English:** Examples: consistency 65%→77.5%, but cost ×4; consistency ≠ accuracy.
     - **Technical terms:** consistency; accuracy; inference cost.

155. **Quote (criteria ambiguity):** "Unlike many human-designed metrics, AI as a judge metrics aren't standardized, making it easy to misinterpret and misuse them. As of this writing, the open source tools MLflow, Ragas, and LlamaIndex all have the built-in criterion faithfulness... but their instructions and scoring systems are all different."
     - **Plain English:** Same "faithfulness" name, different instructions and scales.
     - **Technical terms:** criteria ambiguity; faithfulness.

156. **Quote (Table 3-4):** "MLflow uses a scoring system from 1 to 5, Ragas uses 0 and 1, whereas LlamaIndex's prompt asks the judge to output YES and NO... The faithfulness scores outputted by these three tools won't be comparable."
     - **Plain English:** 1–5 vs 0/1 vs YES/NO — scores are not comparable.
     - **Technical terms:** scoring systems; comparability.

157. **Quote (monitoring):** "An application evolves over time, but the way it's evaluated ideally should be fixed. This way, evaluation metrics can be used to monitor the application's changes. However, AI judges are also AI applications, which means that they also can change over time."
     - **Plain English:** Changing judges break monitoring.
     - **Technical terms:** evaluation monitoring; judge drift.

158. **Quote (misattribution):** "The AI judge team might change the judges without informing the application team. As a result, the application team might mistakenly attribute the changes in the evaluation results to changes in the application, rather than the changes in the judges."
     - **Plain English:** Judge changes get blamed on the app.
     - **Technical terms:** misattribution; evaluation drift.

159. **Quote (rule of thumb):** "Do not trust any AI judge if you can't see the model and the prompt used for the judge."
     - **Plain English:** Only trust transparent judges.
     - **Technical terms:** judge transparency.

160. **Quote (costs):** "If you use GPT-4 to both generate and evaluate responses, you'll do twice as many GPT-4 calls, approximately doubling your API costs. If you have three evaluation prompts... you'll increase your number of API calls four times."
     - **Plain English:** Generate + evaluate = 2×; generation + 3 criteria = 4× calls.
     - **Technical terms:** API cost; evaluation budget.

161. **Quote (spot-checking):** "You can also reduce costs with spot-checking: evaluating only a subset of responses. Spot-checking means you might fail to catch some failures. The larger the percentage of samples you evaluate, the more confidence you will have in your evaluation results, but also the higher the costs."
     - **Plain English:** Spot-checking (sampling) trades confidence for cost.
     - **Technical terms:** spot-checking; sampling.

162. **Quote (latency):** "Implementing AI judges in your production pipeline can add latency. If you evaluate responses before returning them to users, you face a trade-off: reduced risk but increased latency. The added latency might make this option a nonstarter for applications with strict latency requirements."
     - **Plain English:** Pre-response judging adds latency; bad for strict-latency apps.
     - **Technical terms:** latency; guardrails.

163. **Quote (self-bias):** "AI judges tend to have self-bias, where a model favors its own responses over the responses generated by other models. The same mechanism that helps a model compute the most likely response to generate will also give this response a high score. In Zheng et al.'s 2023 experiment, GPT-4 favors itself with a 10% higher win rate, while Claude-v1 favors itself with a 25% higher win rate."
     - **Plain English:** Judges favor their own outputs (GPT-4 +10%, Claude-v1 +25%).
     - **Technical terms:** self-bias; win rate.

164. **Quote (position bias):** "Many AI models have first-position bias. An AI judge may favor the first answer in a pairwise comparison or the first in a list of options. This can be mitigated by repeating the same test multiple times with different orderings or with carefully crafted prompts. The position bias of AI is the opposite of that of humans. Humans tend to favor the answer they see last, which is called recency bias."
     - **Plain English:** AI favors first answers; humans favor last (recency bias).
     - **Technical terms:** first-position bias; recency bias.

165. **Quote (verbosity bias):** "Some AI judges have verbosity bias, favoring lengthier answers, regardless of their quality. Wu and Aji (2023) found that both GPT-4 and Claude-1 prefer longer responses (~100 words) with factual errors over shorter, correct responses (~50 words)."
     - **Plain English:** Longer-but-wrong beats shorter-but-right.
     - **Technical terms:** verbosity bias.

166. **Quote (length ratio):** "Saito et al. (2023) studied this bias for creative tasks and found that when the length difference is large enough (e.g., one response is twice as long as the other), the judge almost always prefers the longer one."
     - **Plain English:** 2× longer → almost always preferred.
     - **Technical terms:** verbosity bias; length difference.

167. **Quote (bias may fade):** "Both Zheng et al. (2023) and Saito et al. (2023), however, discovered that GPT-4 is less prone to this bias than GPT-3.5, suggesting that this bias might go away as models become stronger."
     - **Plain English:** Stronger models show less verbosity bias.
     - **Technical terms:** verbosity bias; model scaling.

168. **Quote (privacy/IP):** "On top of all these biases, AI judges have the same limitations as all AI applications, including privacy and IP. If you use a proprietary model as your judge, you'd need to send your data to this model."
     - **Plain English:** Judge APIs get your data; IP/privacy concerns apply.
     - **Technical terms:** privacy; intellectual property.

169. **Quote (supplement):** "However, AI judges should be supplemented with exact evaluation methods and/or human evaluation."
     - **Plain English:** Don't rely on AI judges alone.
     - **Technical terms:** exact evaluation; human evaluation.

### What Models Can Act as Judges?

170. **Quote:** "The judge can either be stronger, weaker, or the same as the model being judged. Each scenario has its pros and cons."
     - **Plain English:** Judge strength relative to the judged model matters.
     - **Technical terms:** judge strength.

171. **Quote (stronger judge):** "Not only can stronger models make better judgments, but they can also help improve weaker models by guiding them to generate better responses."
     - **Plain English:** Strong judges judge better and can coach weaker models.
     - **Technical terms:** stronger judge.

172. **Quote (why weaker generates):** "You might not have the budget to use the stronger model to generate all responses, so you use it to evaluate a subset of responses. For example, you may use a cheap in-house model to generate responses and GPT-4 to evaluate 1% of the responses."
     - **Plain English:** Cost drives using a strong judge on a sample of weak-model outputs.
     - **Technical terms:** cost optimization; subset evaluation.

173. **Quote (stronger judge challenges):** "Using the stronger model as a judge leaves us with two challenges. First, the strongest model will be left with no eligible judge. Second, we need an alternative evaluation method to determine which model is the strongest."
     - **Plain English:** The best model can't judge itself as an oracle.
     - **Technical terms:** strongest-model problem.

174. **Quote (self-evaluation):** "Using a model to judge itself, self-evaluation or self-critique, sounds like cheating, especially because of self-bias. However, self-evaluation can be great for sanity checks. If a model thinks its own response is incorrect, the model might not be that reliable."
     - **Plain English:** Self-judging is for sanity checks, not final verdicts.
     - **Technical terms:** self-evaluation; self-critique; sanity check.

175. **Quote (self-evaluation example):** "Prompt [from user]: What's 10+3? First response [from AI]: 30. Self-critique [from AI]: Is this answer correct? Final response [from AI]: No it's not. The correct answer is 13."
     - **Plain English:** Self-critique catches and corrects the model's own error.
     - **Technical terms:** self-critique; self-ask.

176. **Quote (weaker judge):** "Some argue that judging is an easier task than generating. Anyone can have an opinion about whether a song is good, but not everyone can write a song. Weaker models should be able to judge the outputs of stronger models."
     - **Plain English:** Judging is easier than generating → weaker judges can work.
     - **Technical terms:** weaker judge; evaluation difficulty.

177. **Quote (specialized judges):** "Because there are many possible ways to use AI judges, there are many possible specialized AI judges... A small, specialized judge can be more reliable than larger, general-purpose judges for specific judgments."
     - **Plain English:** Small specialized judges beat big generalists on specific tasks.
     - **Technical terms:** specialized judges.

178. **Quote (reward model):** "A reward model takes in a (prompt, response) pair and scores how good the response is given the prompt. Reward models have been successfully used in RLHF for many years. Cappy is an example of a reward model developed by Google (2023)... Cappy is a lightweight scorer with 360 million parameters, much smaller than general-purpose foundation models."
     - **Plain English:** Reward model scores (prompt, response); Cappy = 360M-param example.
     - **Technical terms:** reward model; RLHF; Cappy.

179. **Quote (reference-based judge):** "A reference-based judge evaluates the generated response with respect to one or more reference responses... For example, BLEURT (Sellam et al., 2020) takes in a (candidate response, reference response) pair and outputs a similarity score... Prometheus (Kim et al., 2023) takes in (prompt, generated response, reference response, scoring rubric) and outputs a quality score between 1 and 5, assuming that the reference response gets a 5."
     - **Plain English:** Reference-based judges: BLEURT (similarity), Prometheus (1–5 vs reference).
     - **Technical terms:** reference-based judge; BLEURT; Prometheus.

180. **Quote (BLEURT range, footnote):** "The BLEURT score range is confusing. It's approximately between –2.5 and 1.0. This highlights the challenge of criteria ambiguity with AI judges: the score range can be arbitrary."
     - **Plain English:** BLEURT's odd range shows criteria ambiguity.
     - **Technical terms:** BLEURT; criteria ambiguity.

181. **Quote (preference model):** "A preference model takes in (prompt, response 1, response 2) as input and outputs which of the two responses is better (preferred by users) for the given prompt. This is perhaps one of the more exciting directions for specialized judges."
     - **Plain English:** Preference model predicts which response users prefer.
     - **Technical terms:** preference model.

182. **Quote (preference data value):** "As discussed in Chapter 2, preference data is essential for aligning AI models to human preference, and it's challenging and expensive to obtain. Having a good human preference predictor can generally make evaluation easier and models safer to use."
     - **Plain English:** Predicting preference is valuable because preference data is costly.
     - **Technical terms:** preference data; alignment.

183. **Quote (initiatives):** "There have been many initiatives in building preference models, including PandaLM (Wang et al., 2023) and JudgeLM (Zhu et al., 2023)."
     - **Plain English:** Preference models: PandaLM, JudgeLM.
     - **Technical terms:** PandaLM; JudgeLM.

## Ranking Models with Comparative Evaluation

184. **Quote:** "Often, you evaluate models not because you care about their scores, but because you want to know which model is the best for you. What you want is a ranking of these models. You can rank models using either pointwise evaluation or comparative evaluation."
     - **Plain English:** Goal is ranking; two methods: pointwise or comparative.
     - **Technical terms:** ranking; pointwise evaluation; comparative evaluation.

185. **Quote (pointwise):** "With pointwise evaluation, you evaluate each model independently, then rank them by their scores. For example, if you want to find out which dancer is the best... you evaluate each dancer individually, give them a score, then pick the dancer with the highest score."
     - **Plain English:** Score each one, then order by score.
     - **Technical terms:** pointwise evaluation; Likert scale.

186. **Quote (comparative):** "With comparative evaluation, you evaluate models against each other and compute a ranking from comparison results. For the same dancing contest, you can ask all candidates to dance side-by-side and ask the judges which candidate's dancing they like the most."
     - **Plain English:** Compare candidates directly, then derive ranking.
     - **Technical terms:** comparative evaluation; pairwise comparison.

187. **Quote:** "For responses whose quality is subjective, comparative evaluation is typically easier to do than pointwise evaluation. For example, it's easier to tell which song of the two songs is better than to give each song a concrete score."
     - **Plain English:** Relative judgments are easier than absolute scores.
     - **Technical terms:** comparative vs pointwise evaluation.

188. **Quote (history):** "In AI, comparative evaluation was first used in 2021 by Anthropic to rank different models. It also powers the popular LMSYS's Chatbot Arena leaderboard that ranks models using scores computed from pairwise model comparisons from the community."
     - **Plain English:** Anthropic (2021) pioneered it; Chatbot Arena runs on it.
     - **Technical terms:** Anthropic; LMSYS Chatbot Arena; pairwise comparison.

189. **Quote (matches):** "For each request, two or more models are selected to respond. An evaluator, which can be human or AI, picks the winner. Many developers allow for ties to avoid a winner being picked at random when drafts are equally good or bad."
     - **Plain English:** Each request = a match; winner picked; ties allowed.
     - **Technical terms:** match; evaluator; tie.

190. **Quote (preference vs correctness):** "A very important thing to keep in mind is that not all questions should be answered by preference. Many questions should be answered by correctness instead. Imagine asking the model 'Is there a link between cell phone radiation and brain tumors?' and the model presents two options, 'Yes' and 'No'... Preference-based voting can lead to wrong signals that, if used to train your model, can result in misaligned behaviors."
     - **Plain English:** Factual questions need correctness, not preference, votes.
     - **Technical terms:** preference voting; correctness; alignment.

191. **Quote (user frustration):** "Asking users to pick can also cause user frustration. Imagine asking the model a math question because you don't know the answer, and the model gives you two different answers and asks you to pick the one you prefer. If you had known the right answer, you wouldn't have asked the model in the first place."
     - **Plain English:** Forcing a choice frustrates users who lack the answer.
     - **Technical terms:** user experience; preference collection.

192. **Quote (voter knowledge):** "Preference-based voting only works if the voters are knowledgeable in the subject. This approach generally works in applications where AI serves as an intern or assistant, helping users speed up tasks they know how to do—and not where users ask AI to perform tasks they themselves don't know how to do."
     - **Plain English:** Voting works when users already know the task (AI as intern).
     - **Technical terms:** voter knowledge; AI as intern/assistant.

193. **Quote (vs A/B testing):** "Comparative evaluation shouldn't be confused with A/B testing. In A/B testing, a user sees the output from one candidate model at a time. In comparative evaluation, a user sees outputs from multiple models at the same time."
     - **Plain English:** A/B = one output at a time; comparative = side-by-side.
     - **Technical terms:** A/B testing; comparative evaluation.

194. **Quote (win rate):** "The probability that model A is preferred over model B is the win rate of A over B. We can compute this win rate by looking at all matches between A and B and calculating the percentage in which A wins."
     - **Plain English:** Win rate = A's wins ÷ A-vs-B matches.
     - **Technical terms:** win rate; matches.

195. **Quote (two models easy):** "If there are only two models, ranking them is straightforward. The model that wins more often ranks higher. The more models there are, the more challenging ranking becomes."
     - **Plain English:** 2 models easy; many models hard.
     - **Technical terms:** ranking difficulty.

196. **Quote (Table 3-6):** "Let's say that we have five models with the empirical win rates between model pairs... It's not obvious, from looking at the data, how these five models should be ranked."
     - **Plain English:** Pairwise win rates alone don't yield an obvious ranking.
     - **Technical terms:** empirical win rates; ranking.

197. **Quote (rating algorithms):** "Given comparative signals, a rating algorithm is then used to compute a ranking of models. Typically, this algorithm first computes a score for each model from the comparative signals and then ranks models by their scores."
     - **Plain English:** Rating algorithm: signals → per-model score → ranking.
     - **Technical terms:** rating algorithm; comparative signals.

198. **Quote (sports origin):** "Comparative evaluation is new in AI but has been around for almost a century in other industries. It's especially popular in sports and video games. Many rating algorithms developed for these other domains can be adapted to evaluating AI models, such as Elo, Bradley–Terry, and TrueSkill."
     - **Plain English:** Elo/Bradley–Terry/TrueSkill come from sports & games.
     - **Technical terms:** Elo; Bradley–Terry; TrueSkill.

199. **Quote (Elo → Bradley–Terry):** "LMSYS's Chatbot Arena originally used Elo to compute models' ranking but later switched to the Bradley–Terry algorithm because they found Elo sensitive to the order of evaluators and prompts."
     - **Plain English:** Elo is order-sensitive; Arena switched to Bradley–Terry.
     - **Technical terms:** Elo; Bradley–Terry; order sensitivity.

200. **Quote (scaling footnote):** "They scaled the resulting Bradley-Terry scores to make them look like Elo scores... Each score is multiplied by 400 (the scale used in Elo) and added to 1,000 (the initial Elo score). Then this score is rescaled so that the model Llama-13b has a score of 800."
     - **Plain English:** Bradley–Terry scores were rescaled to look like Elo (×400 + 1000; Llama-13b = 800).
     - **Technical terms:** score scaling; Elo scale.

201. **Quote (ranking = prediction):** "A ranking is correct if, for any model pair, the higher-ranked model is more likely to win in a match against the lower-ranked model. If model A ranks higher than model B, users should prefer model A to model B more than half the time."
     - **Plain English:** Good ranking = higher-ranked wins >50% of the time.
     - **Technical terms:** ranking correctness; win probability.

202. **Quote (predictive problem):** "Through this lens, model ranking is a predictive problem. We compute a ranking from historical match outcomes and use it to predict future match outcomes. Different ranking algorithms can produce different rankings, and there's no ground truth for what the correct ranking is. The quality of a ranking is determined by how good it is in predicting future match outcomes."
     - **Plain English:** Rankings are judged by predictive power, not ground truth.
     - **Technical terms:** predictive ranking; no ground truth.

### Challenges of Comparative Evaluation

203. **Quote:** "With pointwise evaluation, the heavy-lifting part of the process is in designing the benchmark and metrics to gather the right signals. Computing scores to rank models is easy. With comparative evaluation, both signal gathering and model ranking are challenging."
     - **Plain English:** Comparative is hard on both signal gathering and ranking.
     - **Technical terms:** signal gathering; model ranking.

204. **Quote (scalability):** "Comparative evaluation is data-intensive. The number of model pairs to compare grows quadratically with the number of models. In January 2024, LMSYS evaluated 57 models using 244,000 comparisons. Even though this sounds like a lot of comparisons, this averages only 153 comparisons per model pair (57 models correspond to 1,596 model pairs)."
     - **Plain English:** 57 models → 1,596 pairs; only ~153 comparisons each.
     - **Technical terms:** quadratic scaling; model pairs.

205. **Quote (transitivity):** "Ranking algorithms typically assume transitivity. If model A ranks higher than B, and B ranks higher than C, then with transitivity, you can infer that A ranks higher than C. This means that if the algorithm is certain that A is better than B and B is better than C, it doesn't need to compare A against C."
     - **Plain English:** Transitivity lets you skip direct A-vs-C matches.
     - **Technical terms:** transitivity assumption.

206. **Quote (transitivity doubt):** "However, it's unclear if this transitivity assumption holds for AI models. Many papers that analyze Elo for AI evaluation cite transitivity assumption as a limitation (Boubdir et al.; Balduzzi et al.; and Munos et al.). They argued that human preference is not necessarily transitive. In addition, non-transitivity can happen because different model pairs are evaluated by different evaluators and on different prompts."
     - **Plain English:** Human preference may be intransitive → ranking assumptions break.
     - **Technical terms:** transitivity; non-transitivity.

207. **Quote (new models):** "With independent evaluation, only the new model needs to be evaluated. With comparative evaluation, the new model has to be evaluated against existing models, which can change the ranking of existing models."
     - **Plain English:** Adding a model reshuffles everyone's ranking.
     - **Technical terms:** new-model evaluation.

208. **Quote (private models):** "This also makes it hard to evaluate private models. Imagine you've built a model for your company, using internal data... If you want to use comparative evaluation for your model, you'll likely have to collect your own comparative signals and create your own leaderboard or pay one of those public leaderboards to run private evaluation for you."
     - **Plain English:** Private models need private leaderboards or paid evaluations.
     - **Technical terms:** private models; private leaderboard.

209. **Quote (matching algorithms):** "The scaling bottleneck can be mitigated with better matching algorithms. So far, we've assumed that models are selected randomly for each match... However, not all model pairs need to be equally compared. Once we're confident about the outcome of a model pair, we can stop matching them against each other. An efficient matching algorithm should sample matches that reduce the most uncertainty in the overall ranking."
     - **Plain English:** Match only uncertain pairs; sample to cut uncertainty most.
     - **Technical terms:** matching algorithm; uncertainty reduction.

210. **Quote (crowdsourcing):** "One way to collect comparative signals is to crowdsource comparisons to the community the way LMSYS Chatbot Arena does. Anyone can go to the website, enter a prompt, get back two responses from two anonymous models, and vote for the better one. Only after voting is done are the model names revealed."
     - **Plain English:** Anonymous side-by-side voting; names revealed after.
     - **Technical terms:** crowdsourcing; anonymous models.

211. **Quote (crowdsourcing pros):** "The benefit of this approach is that it captures a wide range of signals and is relatively difficult to game."
     - **Plain English:** Broad signals; hard to game.
     - **Technical terms:** signal diversity; gaming.

212. **Quote (crowdsourcing cons):** "First, anyone with internet access can use any prompt to evaluate these models, and there's no standard on what should constitute a better response. It might be a lot to expect volunteers to fact-check the responses, so they might unknowingly prefer responses that sound better but are factually incorrect."
     - **Plain English:** Unchecked volunteers can prefer wrong-but-sounding-good answers.
     - **Technical terms:** quality control; fact-checking.

213. **Quote (preference in the wild):** "Some people might prefer polite and moderate responses, while others might prefer responses without a filter. This is both good and bad. It's good because it helps capture human preference in the wild. It's bad because human preference in the wild might not be appropriate for all use cases. For example, if a user asks a model to tell an inappropriate joke and a model refuses, the user might downvote it. However, as an application developer, you might prefer that the model refuses."
     - **Plain English:** Wild preferences conflict with developer goals (refusing bad jokes).
     - **Technical terms:** preference in the wild.

214. **Quote (malicious votes):** "Some users might even maliciously pick the toxic responses as the preferred ones, polluting the ranking."
     - **Plain English:** Malicious voters can poison rankings.
     - **Technical terms:** ranking pollution.

215. **Quote (context gap):** "Second, crowdsourcing comparisons require users to evaluate models outside of their working environments. Without real-world grounding, test prompts might not reflect how these models are being used in the real world. People might just use the first prompts that come to mind and are unlikely to use sophisticated prompting techniques."
     - **Plain English:** Off-work prompts ≠ real-world usage.
     - **Technical terms:** test prompts; real-world grounding.

216. **Quote (simple prompts):** "Among 33,000 prompts published by LMSYS Chatbot Arena in 2023, 180 of them are 'hello' and 'hi', which account for 0.55% of the data, and this doesn't yet count variations... The question 'X has 3 sisters, each has a brother. How many brothers does X have?' was asked 44 times."
     - **Plain English:** Simple/repeated prompts pollute leaderboards (180 hellos; one brainteaser ×44).
     - **Technical terms:** prompt distribution; pollution.

217. **Quote (simple prompts hurt):** "Simple prompts are easy to respond to, making it hard to differentiate models' performance. Evaluating models using too many simple prompts can pollute the ranking."
     - **Plain English:** Easy prompts don't separate strong from weak models.
     - **Technical terms:** model differentiation; ranking pollution.

218. **Quote (RAG gap):** "If a public leaderboard doesn't support sophisticated context construction, such as augmenting the context with relevant documents retrieved from your internal databases, its ranking won't reflect how well a model might work for your RAG system. The ability to generate good responses is different from the ability to retrieve the most relevant documents."
     - **Plain English:** Leaderboards without RAG context misjudge RAG performance.
     - **Technical terms:** RAG; retrieval vs generation.

219. **Quote (hard-prompt filtering):** "LMSYS instead lets users use any prompts but then filter out hard prompts using their internal model and rank models using only these hard prompts."
     - **Plain English:** Arena ranks models on hard prompts only.
     - **Technical terms:** hard-prompt filtering.

220. **Quote (trained evaluators):** "Another way is to use only evaluators that we can trust. We can train evaluators on the criteria to compare two responses or train them to use practical prompts and sophisticated prompting techniques. This is the approach that Scale uses with their private comparative leaderboard. The downside of this approach is that it's expensive and it can severely reduce the number of comparisons we can get."
     - **Plain English:** Scale trains trusted evaluators; expensive, fewer comparisons.
     - **Technical terms:** trained evaluators; private leaderboard.

221. **Quote (product-embedded evaluation):** "Another option is to incorporate comparative evaluation into your products and let users evaluate models during their workflows. For example, for the code generation task, you can suggest users two code snippets inside the user's code editor and let them pick the better one."
     - **Plain English:** Evaluate inside the product (two code snippets in an editor).
     - **Technical terms:** product-embedded evaluation; in-workflow evaluation.

222. **Quote (random clicks):** "On top of that, users might not read both options and just randomly click on one. This can introduce a lot of noise to the results. However, the signals from the small percentage of users who vote correctly can sometimes be sufficient to help determine which model is better."
     - **Plain English:** Random clicks add noise, but correct voters may dominate.
     - **Technical terms:** noise; signal.

223. **Quote (AI vs internet users):** "Some teams prefer AI to human evaluators. AI might not be as good as trained human experts but it might be more reliable than random internet users."
     - **Plain English:** AI judges beat random internet voters.
     - **Technical terms:** AI evaluators; reliability.

### From Comparative Performance to Absolute Performance

224. **Quote:** "Comparative evaluation tells us which model is better. It doesn't tell us how good a model is or whether this model is good enough for our use case."
     - **Plain English:** Relative ranking ≠ absolute quality.
     - **Technical terms:** relative vs absolute performance.

225. **Quote (three scenarios):** "Let's say we obtained the ranking that model B is better than model A. Any of the following scenarios could be valid: 1. Model B is good, but model A is bad. 2. Both model A and model B are bad. 3. Both model A and model B are good. You need other forms of evaluation to determine which scenario is true."
     - **Plain English:** "B > A" is compatible with all-good, all-bad, or mixed.
     - **Technical terms:** absolute performance; evaluation complementarity.

226. **Quote (win-rate conversion):** "Imagine that we're using model A for customer support, and model A can resolve 70% of all the tickets. Consider model B, which wins against A 51% of the time. It's unclear how this 51% win rate will be converted to the number of requests model B can resolve."
     - **Plain English:** 51% win rate doesn't translate cleanly to ticket resolution.
     - **Technical terms:** win rate; absolute conversion.

227. **Quote (1% win-rate variance):** "Several people have told me that in their experience, a 1% change in the win rate can induce a huge performance boost in some applications but just a minimal boost in other applications."
     - **Plain English:** 1% win-rate gains have unpredictable application impact.
     - **Technical terms:** win-rate sensitivity.

228. **Quote (cost-benefit):** "When deciding to swap out A for B, human preference isn't everything. We also care about other factors like cost. Not knowing what performance boost to expect makes it hard to do the cost–benefit analysis. If model B costs twice as much as A, comparative evaluation isn't sufficient to help us determine if the performance boost from B will be worth the added cost."
     - **Plain English:** Comparative alone can't answer cost-benefit questions.
     - **Technical terms:** cost–benefit analysis.

### The Future of Comparative Evaluation

229. **Quote (human limits):** "As models become stronger, surpassing human performance, it might become impossible for human evaluators to give model responses concrete scores. However, human evaluators might still be able to detect the difference, and comparative evaluation might remain the only option."
     - **Plain English:** Beyond human skill, we can still tell which is better.
     - **Technical terms:** human evaluation limits; comparative evaluation.

230. **Quote (Llama 2):** "The Llama 2 paper shared that when the model ventures into the kind of writing beyond the ability of the best human annotators, humans can still provide valuable feedback when comparing two answers (Touvron et al., 2023)."
     - **Plain English:** Humans stay useful at comparing, even beyond their writing skill.
     - **Technical terms:** Llama 2; comparative feedback.

231. **Quote (never saturates):** "Second, comparative evaluation aims to capture the quality we care about: human preference. It reduces the pressure to have to constantly create more benchmarks to catch up with AI's ever-expanding capabilities. Unlike benchmarks that become useless when model performance achieves perfect scores, comparative evaluations will never get saturated as long as newer, stronger models are introduced."
     - **Plain English:** Comparative never saturates; no need for endless new benchmarks.
     - **Technical terms:** saturation; human preference.

232. **Quote (hard to game):** "Comparative evaluation is relatively hard to game, as there's no easy way to cheat, like training your model on reference data. For this reason, many trust the results of public comparative leaderboards more than any other public leaderboards."
     - **Plain English:** Can't cheat by training on references; trusted leaderboards.
     - **Technical terms:** gaming; public leaderboards.

233. **Quote (offline/online role):** "For offline evaluation, it can be a great addition to evaluation benchmarks. For online evaluation, it can be complementary to A/B testing."
     - **Plain English:** Comparative complements benchmarks (offline) and A/B (online).
     - **Technical terms:** offline evaluation; online evaluation; A/B testing.

## Summary

234. **Quote:** "The stronger AI models become, the higher the potential for catastrophic failures, which makes evaluation even more important. At the same time, evaluating open-ended, powerful models is challenging."
     - **Plain English:** More powerful AI → more risk → more evaluation needed.
     - **Technical terms:** catastrophic failure; evaluation importance.

235. **Quote:** "Having humans in the loop for sanity checks is always helpful, and in many cases, human evaluation is essential. However, this chapter focused on different approaches to automatic evaluation."
     - **Plain English:** Humans stay necessary; chapter covers automation.
     - **Technical terms:** human evaluation; automatic evaluation.

236. **Quote:** "Unlike exact evaluation, subjective metrics are highly dependent on the judge. Their scores need to be interpreted in the context of what judges are being used. Scores aimed to measure the same quality by different AI judges might not be comparable."
     - **Plain English:** Subjective scores only make sense relative to their judges.
     - **Technical terms:** subjective metrics; judge dependence.

237. **Quote:** "AI judges, like all AI applications, should be iterated upon, meaning their judgments change. This makes them unreliable as benchmarks to track an application's changes over time. While promising, AI judges should be supplemented with exact evaluation, human evaluation, or both."
     - **Plain English:** Judging changes over time → unreliable benchmarks; supplement with exact/human eval.
     - **Technical terms:** judge drift; exact evaluation; human evaluation.

238. **Quote:** "Both comparative evaluation and the post-training alignment process need preference signals, which are expensive to collect. This motivated the development of preference models: specialized AI judges that predict which response users prefer."
     - **Plain English:** Expensive preference data motivated preference models.
     - **Technical terms:** preference signals; preference models.

239. **Quote:** "While language modeling metrics and hand-designed similarity measurements have existed for some time, AI as a judge and comparative evaluation have only gained adoption with the emergence of foundation models. Many teams are figuring out how to incorporate them into their evaluation pipelines. Figuring out how to build a reliable evaluation pipeline to evaluate open-ended applications is the topic of the next chapter."
     - **Plain English:** New methods came with foundation models; building the pipeline is Chapter 4.
     - **Technical terms:** evaluation pipeline; AI as a judge; comparative evaluation.
