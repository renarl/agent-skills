# Verbalized Sampling Plugin

Generate diverse, creative responses using the Verbalized Sampling (VS) technique — a research-backed prompting method that mitigates mode collapse in LLMs.

## What It Does

Instead of getting one typical response, the `/vs` command generates 15 candidate responses across three diversity strategies, deduplicates them, and returns a clean set of unique outputs.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/vs` | Generate diverse responses for any prompt |

## Usage

```
/vs Write a joke about coffee
/vs Write a short story starting with "The door opened slowly"
/vs Name a creative startup idea for pet owners
```

## How It Works

The command runs three VS variants on your prompt:

- **VS-Standard** — Generates 5 diverse responses sampled from the full distribution
- **VS-CoT** — Uses chain-of-thought reasoning to explore diverse angles before generating 5 responses
- **VS-Multi** — Targets the tails of the distribution for 5 unusual/surprising responses

All 15 responses are then deduplicated and presented as a single numbered list.

## Based On

Zhang, J., Yu, S., Chong, D., et al. "Verbalized Sampling: How to Mitigate Mode Collapse and Unlock LLM Diversity." 2025.

## No Setup Required

This plugin works out of the box — no API keys, tools, or external dependencies needed.
