### Question 1: 

Objective: Develop an AI-powered content generation platform that enables business users (e.g., marketing, finance, or product teams) to transform raw survey or analytical data into customer-ready deliverables with minimal engineering support. The solution should streamline the end-to-end workflow from data ingestion to publication while providing visibility into AI usage and associated costs.

Success Metrics: Reduce the time required to produce reports and interactive assets by at least 80%, lower production costs compared to traditional workflows, enable a single business user to complete the process independently, and provide transparent reporting of AI spend alongside measurable business outcomes.

Functional Requirements: The application should allow users to upload structured datasets (CSV, Excel, or survey exports), automatically identify key insights and trends, generate executive-ready reports with charts and narrative summaries, create interactive visualizations or calculators from the underlying data, and track AI usage and cost for each generated artifact. The platform should also support regenerating or repurposing existing datasets into new content formats (e.g., blog posts, infographics, executive summaries, or sales collateral) without requiring the data to be reprocessed.



**Core Challenge**: LLMs are expensive context engines, not data processing engines.

Step 1: What does an LLM actually "see"?

Suppose the user uploads `survey.csv` with `rows: 2,000` and `columns: 25`. An LLM never sees a high-level "CSV", it only sees the data within 
the rows, which becomes tokens. Suppose we take the example above with, on average, 8 tokens per cell. Then 25 x 2,000 x 8 = 400,000 tokens. And 
this is before adding "commas", "newlines", "headers", and "prompt". You're already well beyond the context window of many models. Even with a 
1M-token context model, sending 400k tokens is: (1) slow, (2) expensive, and (3) wasteful. 

Even if the model _could_ read everything.. Should it? Imagine asking: "What percentage of CFOs, increased AI spending?" The LLM doesn't need
2000 rows. A computer program can compute: `YES = 1423` and `TOTAL = 2000`, which equals `71.15%`. Then tell the LLM: `71.15% of respondents increased AI spending`. That might be 15 tokens instead of 20,000. 

The first principle: An LLM should rarely perform computation over structured data. **Traditional software computes. LLMs explain.**

LLM receives: 
* Healthcare spends 38% more on AI. 
* Enterprise companies spend 4x more. 
* Companies > 10,000 employees reported 92% adoption. 
* Budget growth correlates 0.71 with revenue. 

Instead of: 
- CSV -> LLM -> Report
Think: 
- CSV -> Parser -> Schema Discovery -> Statistical Engine -> Insight Objects -> LLM -> Report

In this problem, the general useful rule of thumb is: 
* Software ingests, validates, profiles, aggregates, and computes statistics.
* LLM (early in the pipeline) helps understand unfamiliar schemas and user intent.
* LLM (late in the pipeline) turns structured analytical results into polished reports, charts, executive summaries, or interactive calculator explanations.


### Entity Recognition Layer

The Entity Recognition layer acts as the foundational structural parser of incoming text. Its primary objective is to locate and classify specific, rigid elements—known as "named entities"—into predefined categories such as names of people, organizations, locations, dates, monetary values, or clinical codes. This layer operates largely at a surface token level, identifying what things are mentioned without necessarily caring about how they relate to one another or what the overarching sentence actually implies. It effectively isolates the essential nouns and data points out of raw text, turning unstructured words into labeled data points.

### Semantic Understanding Layer

The Semantic Understanding layer sits on top of entity recognition and is responsible for capturing the actual meaning, intent, and context of the communication. Instead of merely labeling individual words, this layer analyzes the relationships between entities, the syntactic structure of the sentence, and subtle nuances like sentiment, tone, or underlying intent. It determines why those entities were mentioned and what action or meaning is being conveyed. For instance, while the entity layer identifies a drug name and a date, the semantic layer deduces that the user is attempting to report a specific adverse claim denial event, mapping the disparate data points into a coherent, conceptual framework.

