# Nested Comment Extraction

Reddit's JSON API truncates comment trees. Deep threads and popular posts will contain `kind: "more"` nodes instead of actual comments. Use this recursive technique to fetch them all.

## How Reddit Comment Truncation Works

When fetching a thread with `.json?limit=500&depth=10`, Reddit returns as many comments as it can within those limits. Remaining comments are represented as stub nodes:

```json
{
  "kind": "more",
  "data": {
    "count": 42,
    "name": "t1_abc123",
    "id": "abc123",
    "parent_id": "t1_xyz789",
    "depth": 3,
    "children": ["id1", "id2", "id3", "..."]
  }
}
```

- `count` — approximate number of hidden comments in this branch
- `children` — array of comment IDs that were not returned
- `depth` — nesting level of this stub

## Step-by-Step Recursive Extraction

### Step 1: Fetch the Main Thread

```bash
THREAD_JSON=$(curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/r/SUBREDDIT/comments/POST_ID.json?limit=500&depth=10")
```

### Step 2: Extract "more" Comment IDs

Use `jq` to find all `kind: "more"` nodes and collect their children IDs:

```bash
MORE_IDS=$(echo "$THREAD_JSON" | jq -r '
  .. | select(.kind? == "more") | .data.children[]
' | head -100 | tr '\n' ',')
```

The `head -100` limits to 100 IDs per batch (Reddit's API limit for `morechildren`).

### Step 3: Fetch Additional Comments

```bash
MORE_JSON=$(curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
  "https://www.reddit.com/api/morechildren.json?api_type=json&link_id=t3_POST_ID&children=${MORE_IDS}")
```

The response contains `json.data.things[]` — an array of comment objects.

### Step 4: Parse the Additional Comments

```bash
echo "$MORE_JSON" | jq '.json.data.things[] | select(.kind == "t1") | {
  author: .data.author,
  body: .data.body,
  score: .data.score,
  parent_id: .data.parent_id,
  depth: .data.depth
}'
```

### Step 5: Check for Further Truncation

The `morechildren` response may itself contain `kind: "more"` stubs. Check and repeat:

```bash
REMAINING=$(echo "$MORE_JSON" | jq -r '
  .json.data.things[] | select(.kind == "more") | .data.children[]
' | head -100 | tr '\n' ',')
```

If `REMAINING` is non-empty, go back to Step 3 with these new IDs.

## Batch Processing for Large Threads

Reddit limits `morechildren` to ~100 IDs per request. For very large threads, batch the IDs:

```bash
# Collect ALL more IDs
ALL_IDS=$(echo "$THREAD_JSON" | jq -r '.. | select(.kind? == "more") | .data.children[]')

# Process in batches of 100
echo "$ALL_IDS" | paste -sd, - | fold -s -w 1000 | while read BATCH; do
  curl -s -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36" \
    "https://www.reddit.com/api/morechildren.json?api_type=json&link_id=t3_POST_ID&children=${BATCH}"
  sleep 1  # Rate limiting: be polite to Reddit's servers
done
```

## Rate Limiting

Reddit rate-limits unauthenticated requests. Best practices:
- Wait at least 1 second between requests
- Limit to ~30 requests per minute
- If you get a 429 response, back off for 30-60 seconds
- Fetch only what the user actually needs — don't speculatively grab every comment

## Common Issues

- **Empty `children` array**: The `count` may be > 0 but `children` is empty. This means Reddit has collapsed the thread — there's nothing more to fetch.
- **`[deleted]` or `[removed]` authors**: These comments exist but their content was deleted by the user or removed by moderators. Report them as such.
- **Rate limiting (HTTP 429)**: Wait and retry. Don't hammer the endpoint.
- **`jq` not available**: Fall back to Python's `json` module or manual parsing as needed.
