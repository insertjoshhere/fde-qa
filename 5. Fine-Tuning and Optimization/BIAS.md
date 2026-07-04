The Limitations of Using LLM as a Judge

**1. Position Bias:** The judge prefers whichever response is shown first. GPT-4 flips its judgment 40% of the time when you swap the order.


**2. Verbosity Bias:** Longer answers score higher, even when they say the same thing. LLMs overvalue length way more than humans do.

**3. Self-Preference:** A judge rates its own model family's output 10-25% higher. The better it recognizes its own text, the more it favors it.

**4. Style Bias:** Markdown tables and formal tone beat casual answers, even when the casual one is more accurate.

**5. Calibration Drift:** Your judge model gets a version update. All your scores shift. Same rubric, same data, different results.

**6. Preference Leakage:** If the judge and the model being judged are from the same family, the judge plays favorites. Up to 28.7% bias on benchmarks (ICLR 2026).

