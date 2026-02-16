---
name: reddit-research
description: >
  This skill should be used when the user asks to "research Reddit",
  "find Reddit discussions", "look up a Reddit thread", "search Reddit for",
  "what does Reddit say about", "check Reddit comments", "find Reddit posts about",
  or any task involving fetching, reading, or analyzing content from reddit.com.
version: 0.1.0
---

# Reddit Research

Research Reddit content by fetching JSON data directly via curl. **Do not make anything up — report only what is actually found in the fetched data.**

## Core Rules

1. **Use curl, not WebFetch**, for all reddit.com requests.
2. **Append `.json`** to any Reddit URL to get structured JSON data.
3. **Always set the User-Agent header** on every request:
   ```
   curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
   ```
4. **Never fabricate content.** If a fetch fails or returns empty data, say so.

## Fetching a Reddit Thread (Post + Comments)

Given a thread URL like `https://www.reddit.com/r/SUBREDDIT/comments/POST_ID/slug/`:

```bash
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/comments/POST_ID.json?limit=500&depth=10"
```

The response is a JSON array with two elements:
- `[0]` — the post itself (in `.data.children[0].data`)
- `[1]` — the comment tree (in `.data.children[]`)

### Parsing Posts

Extract from `[0].data.children[0].data`:
- `title`, `selftext`, `author`, `score`, `num_comments`, `created_utc`, `url`, `subreddit`

### Parsing Comments

Each comment object at `[1].data.children[]` has `kind` and `data`:
- `kind: "t1"` — a comment. Key fields: `author`, `body`, `score`, `created_utc`
- `kind: "more"` — truncated comments that need a second fetch (see nested comments below)
- Replies are nested under `data.replies.data.children[]`

## Searching a Subreddit

```bash
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/search.json?q=QUERY&restrict_sr=on&sort=relevance&t=all&limit=25"
```

Sort options: `relevance`, `hot`, `top`, `new`, `comments`
Time filters (`t=`): `hour`, `day`, `week`, `month`, `year`, `all`

## Browsing a Subreddit

```bash
# Hot posts (default)
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/hot.json?limit=25"

# Top posts
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/top.json?t=week&limit=25"

# New posts
curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/new.json?limit=25"
```

## Fetching Nested / Truncated Comments

Reddit truncates deep or large comment trees. When `kind: "more"` nodes appear, fetch the missing comments. See `references/nested-comments.md` for the full recursive extraction technique.

**Quick version:**

1. Parse the thread JSON for `kind: "more"` nodes.
2. Extract their `data.children` arrays (these are comment IDs).
3. Fetch them via the `morechildren` endpoint:
   ```bash
   curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
     "https://www.reddit.com/api/morechildren.json?api_type=json&link_id=t3_POST_ID&children=ID1,ID2,ID3"
   ```
4. Repeat if the response itself contains more `kind: "more"` nodes.

## Presenting Results

- Always cite the subreddit, post title, author, and score when summarizing.
- Quote or closely paraphrase the actual comment text — do not invent opinions.
- When presenting multiple viewpoints, organize by theme or by most-upvoted.
- Include the URL of the original thread so the user can verify.
- Note when comments were truncated and whether nested fetching was performed.
