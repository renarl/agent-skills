---
description: Research Reddit threads, subreddits, or topics
allowed-tools: Bash(curl:*), Bash(jq:*), Bash(echo:*), Read, Write
argument-hint: [url, subreddit, or search query]
---

Research Reddit content based on the user's input: `$ARGUMENTS`

Load the reddit-research skill for full technical details, then follow these steps:

## 1. Determine the Request Type

- **If the input is a Reddit URL** (contains `reddit.com/r/`): Fetch that specific thread or subreddit.
- **If the input is a subreddit name** (like `r/golang` or just `golang`): Browse or search that subreddit.
- **If the input is a search query**: Search across the most relevant subreddit(s), or Reddit broadly.

## 2. Fetch Data

Use curl with the required User-Agent for ALL requests:
```
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

Always append `.json` to Reddit URLs. Never use WebFetch for reddit.com.

### For a specific thread:
Fetch the thread JSON, extract the post details and top comments. If there are `kind: "more"` stubs with significant comment counts, fetch nested comments using the `morechildren` API (see the skill's `references/nested-comments.md`).

### For a subreddit browse:
Fetch hot/top/new posts and summarize the most relevant or popular ones.

### For a search:
Use the subreddit search endpoint with appropriate sort and time filters.

## 3. Present Findings

- **Report only what was actually found.** Do not invent, embellish, or speculate.
- Summarize the post title, author, score, and key content.
- For threads: organize the most notable comments by theme or by score.
- Quote or closely paraphrase actual comment text.
- Include the original Reddit URL for verification.
- Note if any comments were truncated and whether nested fetching was performed.
- If a fetch failed, explain clearly what happened instead of guessing at the content.
