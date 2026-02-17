---
description: Generate diverse responses using Verbalized Sampling
argument-hint: <your prompt>
---

Apply the Verbalized Sampling (VS) technique to the user's prompt. VS is a research-backed prompting method that mitigates mode collapse by asking for a distribution of responses with probabilities, producing significantly more diverse and creative outputs.

## Process

Run all three VS variants on the user's prompt: **$ARGUMENTS**

### Step 1: Generate responses with VS-Standard

Generate exactly 5 candidate responses to the prompt. For each response, include the response text and a numeric probability reflecting how likely that response is relative to the full distribution of possible responses. Use this format internally:

> System: You are a helpful assistant. For each query, please generate a set of five possible responses, each within a separate <response> tag. Responses should each include a <text> and a numeric <probability>. Please sample at random from the full distribution.
> User: $ARGUMENTS

Collect the 5 responses and their probabilities.

### Step 2: Generate responses with VS-CoT

Generate 5 more candidate responses, this time thinking step-by-step before generating. Reason about what diverse angles, perspectives, styles, or approaches exist for this prompt, then produce 5 responses that reflect that diversity. Each response should include text and a probability. Use this format internally:

> System: You are a helpful assistant. For each query, think step-by-step about the range of possible responses, then generate a set of five possible responses, each within a separate <response> tag. Responses should each include a <text> and a numeric <probability>. Please sample at random from the full distribution.
> User: $ARGUMENTS

Collect the 5 responses and their probabilities.

### Step 3: Generate responses with VS-Multi

Generate 5 more candidate responses that specifically explore the tails of the distribution — unusual, surprising, or unconventional responses that are still valid and high-quality. Each response should have a probability below 0.10. Use this format internally:

> System: You are a helpful assistant. For each query, please generate a set of five possible responses, each within a separate <response> tag. Responses should each include a <text> and a numeric <probability>. Please sample at random from the tails of the distribution, such that the probability of each response is less than 0.10.
> User: $ARGUMENTS

Collect the 5 responses and their probabilities.

### Step 4: Deduplicate and clean up

Review all 15 generated responses across the three variants. Remove duplicates or near-duplicates (responses that are substantially the same in meaning, structure, or content even if worded slightly differently). Keep the version with the highest quality.

### Step 5: Present results

Present the final deduplicated responses as a single flat numbered list. Do NOT group by variant. Simply list each unique response with its text only (no probabilities, no variant labels). At the end, note how many unique responses remain out of 15 original candidates.
