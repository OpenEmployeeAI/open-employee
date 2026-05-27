# Build-in-Public Operating System

The build-in-public loop is an operating system, not a marketing tactic. Its job is to convert public signal into roadmap state and roadmap state back into public updates, durably and reviewably.

## Loop

1. **Listen** — pull feedback from GitHub, Discord, X, RSS, and support sources via read-only MCP tools.
2. **Normalize** — write each signal as a typed `FeedbackItem` with `source`, `external_id`, `actor`, `captured_at`, `text_ref`, and `auth_ref`.
3. **Classify** — apply the [PMF signal taxonomy](pmf-signal-taxonomy.md) to assign signal type, severity, and confidence.
4. **Route** — link the item to an existing roadmap node, open a new one, or drop with a recorded reason.
5. **Decide** — humans promote, merge, decline, or defer. Decisions are durable events on the roadmap workflow.
6. **Communicate** — roadmap state transitions can trigger draft public updates. Drafts require human approval before any write Activity is scheduled.
7. **Close** — when shipped, the workflow notifies the originating thread(s) via the same MCP write tools, again gated by approval.

## Invariants

- Every external read or write executes as a Temporal Activity named `mcp__<server>__<tool>`.
- No raw secrets in workflow history; Activities resolve `auth_ref` at execution time.
- All write-side public actions (post, reply, edit, close, label, schedule) are tagged `risk: public` and require human approval before scheduling.
- All decisions are durable events; the roadmap is reconstructible from event history.
- Large payloads (long threads, attachments) use claim-check artifact references.

## Roles

- **Listener agent** — reads sources. Read-only. No approval needed.
- **Classifier agent** — applies taxonomy. No external writes.
- **Router agent** — proposes roadmap links. Writes only to internal feedback store.
- **Publisher agent** — drafts public messages. *Never* posts without human approval.
- **Operator (human)** — approves promotions, declines, and public posts.

## What this is not

- Not a CMS. The roadmap is workflow state, not a wiki page.
- Not a scheduler. Postiz is a connector that *can* schedule, but scheduling is an effect of a roadmap transition, not a primary action.
- Not autonomous posting. The system can draft; only humans approve.

## Related docs

- [gitroom-style-architecture.md](gitroom-style-architecture.md)
- [community-feedback-loop.md](community-feedback-loop.md)
- [github-issue-intake-loop.md](github-issue-intake-loop.md)
