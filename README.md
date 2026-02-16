# Agent Skills

A collection of custom skills for Claude — reusable frameworks that help Claude handle complex, multi-step work more effectively.

## Skills

### agent-brief

A task delegation and project scoping framework for Claude. When you hand Claude a non-trivial project, this skill teaches it to behave like a well-managed employee: clarify goals before starting, break work into checkpoints, save progress persistently, and stop at natural decision points for your approval.

**Inspired by** Ethan Mollick's [Management as AI Superpower](https://www.oneusefulthing.org/p/management-as-ai-superpower), which argues that the bottleneck with AI agents isn't the AI's capability — it's our ability to delegate well. Mollick's delegation framework depends on three variables: how long the task would take you, the probability of AI success on each attempt, and the overhead of requesting and evaluating output. The skills that matter most turn out to be the "soft" ones: scoping problems, defining deliverables, and recognizing when output is off.

This skill operationalizes that insight. It gives Claude a structured protocol for:

- **Scoping** — clarifying purpose, authority boundaries, and definition of done before starting
- **Checkpointing** — breaking work into 2–5 resumable phases with persistent outputs
- **Briefing** — creating multi-file project briefs so work survives session boundaries
- **Stopping** — pausing at decision points for human review instead of barreling ahead

#### Usage

Drop the `agent-brief` folder into your Claude skills directory. The skill triggers on phrases like "scope this out", "let's plan this", "brief me", or any request involving project kickoff and multi-phase work.

#### Structure

```
agent-brief/
├── SKILL.md              # Core skill instructions
├── assets/               # Supporting files
├── references/
│   ├── api_reference.md  # Tool API reference
│   └── templates.md      # Copy-paste brief templates
└── scripts/              # Helper scripts
```

### reddit-research

A Reddit research plugin for Claude. Fetches and analyzes Reddit threads, subreddits, and discussions using Reddit's public JSON API — no API key or authentication required.

Includes both a `/reddit-research` slash command for on-demand use and an auto-activating skill that triggers when Reddit research comes up in conversation. Handles the full complexity of Reddit's data: post metadata, comment trees, nested/truncated comment extraction via the `morechildren` API, subreddit search, and feed browsing (hot/top/new).

Key design decisions:

- **curl over WebFetch** — uses curl with a browser User-Agent for reliable JSON fetching
- **Never fabricates** — reports only what was actually found in the fetched data
- **Recursive comment extraction** — handles Reddit's comment truncation with batched fetching and rate limiting

#### Usage

Drop the `reddit-research` folder into your Claude plugins directory. Use the slash command directly or let the skill auto-activate:

```
/reddit-research https://www.reddit.com/r/programming/comments/abc123/some_post/
/reddit-research r/machinelearning transformer architectures
/reddit-research what does reddit think about Rust vs Go
```

Or just ask naturally — *"What are people on Reddit saying about the new MacBook Pro?"*

#### Structure

```
reddit-research/
├── .claude-plugin/
│   └── plugin.json           # Plugin metadata
├── commands/
│   └── reddit-research.md    # Slash command definition
├── skills/
│   └── reddit-research/
│       ├── SKILL.md           # Core skill instructions
│       └── references/
│           └── nested-comments.md  # Recursive comment extraction guide
└── README.md
```

## License

MIT