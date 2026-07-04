### 1. Self-Consistency: 

In AI, Self-Consistency is a decoding strategy and evaluation metric based on the idea that a complex reasoning problem often has multiple paths to the correct answer, but they should all converge on the same final conclusion.

If you ask a model a math problem 512 times with a bit of randomness enabled (temperature > 0), a standard model might give you 5 or 6 different final answers across those attempts. "100% self-consistency @ 512 samples" means that when sampled 512 times, every single attempt converged on the exact same final answer.

#### High vs. Low

* High Self-Consistency (e.g., 100%): The model is incredibly reliable and stable. No matter how many times or ways it reasons through the problem, it consistently lands on the exact same conclusion.
* Low Self-Consistency: The model is erratic. Minor changes in its internal reasoning path cause it to spit out different final answers, signaling low confidence or ambiguity.

#### How it is measured

1. Sample Generation: The model is prompted with a question multiple times (in this case, `N = 512`) using a non-zero temperature to allow for diverse reasoning paths.
2. Parsing: An automated script extracts the final answer (e.g., the final number in a math problem or the multiple-choice letter) from each of the 512 outputs.
3. Majority Voting: The algorithm counts the frequency of each unique final answer. The self-consistency score is the percentage of samples that match the most common answer.

### 2. Output Entropy:

In information theory, Entropy is a measure of randomness, uncertainty, or unpredictability. In the context of LLMs, Output Entropy usually refers to the average Shannon entropy of the token probability distributions during generation.When a model predicts the next word, it assigns probabilities to thousands of possible tokens. "0.00 bits output entropy" means there was absolutely zero uncertainty. At every single step of generation, the model assigned a $100\%$ probability ($1.0$) to one specific token and $0\%$ to all others.

#### High vs. Low

* High Output Entropy: The model is "unsure" and sees many viable next words. This allows for creativity, variety, and open-ended writing (like brainstorming or poetry).
* Low Output Entropy (e.g., 0.00 bits): The model is entirely deterministic and rigid. It behaves less like a creative writer and more like a hard-coded calculator executing a single, unyielding path.

#### How it is measured

* At every single token generation step t, the model outputs a probability distribution vector P. The entropy H for that step is calculated using the formula:

$$H(X) = -\sum_{i} P(x_i) \log_2 P(x_i)$$

To get the overall output entropy, researchers average $H(X)$ across all tokens in the generated response. If a model always assigns a probability of $1.0$ to the top token, $\log_2(1) = 0$, resulting in $0.00$ bits of entropy.

### 3. Hallucination Variance: 

Hallucination occurs when an AI generates factually incorrect or ungrounded information. Variance measures how much a set of data points differs from the mean.

Therefore, Hallucination Variance refers to how wildly the model flits between being accurate and hallucinating across different runs or slight prompt variations. "Zero hallucination variance" means the model’s truthfulness is completely static. Combined with 100% self-consistency, it implies the model either always hallucinates the exact same way, or (more likely in the context of a successful distillation boast) it has completely eliminated hallucination fluctuations and is perfectly grounded across all runs.

#### High vs. Low
* High Hallucination Variance: The model is unpredictable. On the first run, it might give you a perfectly accurate medical diagnosis. On the second run, it might invent a fake chemical compound. You can't trust it without verifying every single output. 
* Low/Zero Hallucination Variance: The model is perfectly predictable. If it knows the answer, it gives it to you every time. If it doesn't, it consistently fails or says "I don't know" in the exact same manner. 

#### How it is measured

1. Truthfulness Scoring: The model is run against a benchmark dataset (like TruthfulQA or MuSR) across multiple seeds or minor prompt perturbations. Each output is scored for factual correctness (often using a stronger judge LLM like GPT-4, or programmatic fact-checking tools).
2. Statistical Variance Calculation: Researchers take the accuracy scores across all those iterations and calculate the statistical variance ($\sigma^2$):

$$\sigma^2 = \frac{\sum (x_i - \mu)^2}{N}$$

Where $x_i$ is the hallucination score of a specific run, and $\mu$ is the average score. A variance of 0.00 means $x_i$ never deviates from the average.