# Reddit Research Plugin

Research Reddit threads, subreddits, and discussions directly from Claude. Fetches real data via Reddit's JSON API using curl — no API key required.

## What It Does

- Fetch and summarize Reddit threads (posts + full comment trees)
- Search subreddits for relevant discussions
- Browse subreddit feeds (hot, top, new)
- Extract nested/truncated comments using Reddit's `morechildren` API
- Reports only what's actually found — never fabricates content

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/reddit-research` | Research a URL, subreddit, or topic on demand |
| Skill | `reddit-research` | Auto-activates when Reddit research is mentioned in conversation |

## Usage

### Slash Command

```
/reddit-research https://www.reddit.com/r/programming/comments/abc123/some_post/
/reddit-research r/machinelearning transformer architectures
/reddit-research what does reddit think about Rust vs Go
```

### Automatic

Just mention Reddit research in conversation — the skill activates automatically:

> "Can you find what people on Reddit are saying about the new MacBook Pro?"

## Technical Details

- Uses `curl` with a browser User-Agent header (not WebFetch)
- Appends `.json` to Reddit URLs for structured data
- Supports recursive nested comment extraction via `morechildren` API
- Includes rate-limiting best practices (1s delay between requests)
- Requires `jq` for JSON parsing (falls back to Python if unavailable)

## No Setup Required

This plugin uses Reddit's public JSON endpoints — no API key or authentication needed.
