# Community Feedback Loop

This document describes the end-to-end loop from a community signal to a roadmap decision, and back to a public update.

## Sources (initial set)

- GitHub issues and discussions on `OpenEmployeeAI/open-employee` and sibling repos.
- Pull request review comments tagged as feedback.
- Optional: Discord, X mentions, RSS, support inbox — added later as MCP servers.

Initial phase is GitHub-only. Other sources are deferred until the GitHub loop is stable.

## FeedbackItem (typed contract sketch)

```yaml
feedback_item:
  id: string                # internal id
  source: string            # e.g., github, discord, x, rss, support
  external_id: string       # e.g., gh:OpenEmployeeAI/open-employee#42
  actor: string             # external handle; never a raw token
  captured_at: timestamp
  text_ref: artifact_ref    # claim-check ref to full text
  signal_type: enum         # see pmf-signal-taxonomy.md
  severity: enum            # low | medium | high | critical
  confidence: float         # 0..1
  roadmap_link: optional    # roadmap node id, if any
  auth_ref: string          # resolved inside Activity only
```

This is a sketch for `open-employee-core` to formalize. It is intentionally not implemented in this coordination repo.

## Activity boundaries

Every external call below is a Temporal Activity named `mcp__<server>__<tool>`. Read-only Activities do not require approval. Write Activities are tagged `risk: public` and require approval.

Read examples:

- `mcp__github__list_issues`
- `mcp__github__get_issue`
- `mcp__github__list_issue_comments`

Write examples (approval required):

- `mcp__github__create_issue_comment`
- `mcp__github__add_labels`
- `mcp__github__close_issue`
- `mcp__postiz__create_post`

## Approval gates

- Promote feedback to roadmap → human approval.
- Decline feedback → human approval (with reason).
- Any `risk: public` write Activity → human approval before scheduling.
- Bulk operations (≥ N items, threshold TBD in `open-employee-core`) → human approval even if individual actions are normally auto.

## State transitions

Roadmap nodes move through: `proposed → accepted → in_progress → shipped` or `proposed → declined`. Each transition is a durable workflow event. Public updates derive from transitions, not from ad-hoc messaging.

## Related docs

- [gitroom-style-architecture.md](gitroom-style-architecture.md)
- [build-in-public-operating-system.md](build-in-public-operating-system.md)
- [github-issue-intake-loop.md](github-issue-intake-loop.md)
- [pmf-signal-taxonomy.md](pmf-signal-taxonomy.md)
