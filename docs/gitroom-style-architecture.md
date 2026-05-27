# Gitroom-Style Architecture for OpenEmployee

OpenEmployee borrows the "build-in-public" operating shape pioneered by Gitroom, but rewires it onto the OpenEmployee durable runtime. Postiz is *not* the system. Postiz is one connector among many; the operating system is feedback-to-roadmap.

## What we take from Gitroom

- Public, asynchronous feedback channels feed a single intake queue.
- Roadmap items are first-class artifacts derived from intake, not invented in private.
- Posting, scheduling, and replying are side effects of roadmap state changes, not the source of truth.
- Every public action is reviewable before it ships.

## What we change

- The intake queue, classifier, roadmap state, and posting are all expressed as Temporal workflows and Activities.
- Every external tool call (GitHub, Postiz, Slack, X, Discord, RSS, Pipedream) executes as a dedicated Temporal Activity boundary named `mcp__<server>__<tool>` — see [architecture.md](architecture.md).
- Raw secrets never enter workflow history. Connectors resolve `auth_ref` inside Activities only.
- Risky public actions (posting, mass-replying, closing issues, merging roadmap changes) require explicit human approval before the Activity is scheduled.
- Large Activity outputs (long threads, transcripts, RSS dumps) use claim-check artifact references.

## Layered view

1. **Signal layer** — GitHub issues, Discord, X mentions, RSS, support inbox. Each source is an MCP server exposing read tools.
2. **Intake workflow** — normalizes signals into a typed `FeedbackItem` and writes to the feedback store.
3. **Classification workflow** — tags items against the [PMF signal taxonomy](pmf-signal-taxonomy.md).
4. **Roadmap workflow** — promotes/merges/declines items; emits roadmap state transitions as durable events.
5. **Publication workflow** — composes public updates from roadmap transitions; requires human approval before any `mcp__postiz__*` or `mcp__github__*` write Activity is scheduled.

## Non-goals

- Becoming a social scheduler. Postiz is a connector; replacing it is not the goal.
- Auto-posting without human approval.
- Centralizing identity, secrets, or policy inside this repo — those live in `open-employee-identity`.

## Related docs

- [build-in-public-operating-system.md](build-in-public-operating-system.md)
- [community-feedback-loop.md](community-feedback-loop.md)
- [github-issue-intake-loop.md](github-issue-intake-loop.md)
- [pmf-signal-taxonomy.md](pmf-signal-taxonomy.md)
