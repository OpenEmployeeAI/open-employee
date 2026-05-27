# PMF Signal Taxonomy

A small, opinionated taxonomy for classifying community feedback. Keep it short on purpose — taxonomies that grow unbounded stop being useful.

## Signal types

- **pain** — user reports something is broken, painful, or blocking. High weight for PMF.
- **wish** — user asks for a capability they do not have. Medium weight; aggregate before acting.
- **praise** — user expresses positive sentiment. Low weight individually; useful as trend signal.
- **confusion** — user misunderstands or cannot find something. Often a docs/UX gap.
- **comparison** — user compares to an alternative (named tool, internal build, status quo). High weight when paired with `pain` or `wish`.
- **intent** — user states they will adopt, pay, contribute, or leave. Highest weight; rare.
- **noise** — off-topic, spam, or unactionable. Recorded and dropped from prioritization.

## Severity

- `low` — minor inconvenience or aesthetic.
- `medium` — degraded workflow, workaround exists.
- `high` — blocks a primary use case.
- `critical` — data loss, security, or trust impact.

## Confidence

Float in `[0, 1]`. Classifier records confidence; items below a threshold (TBD in core) are queued for human review before they influence roadmap weights.

## Aggregation

Roadmap weight is a function of `signal_type`, `severity`, `confidence`, and the count of distinct `actor`s. Exact formula is owned by `open-employee-core`; this doc only fixes the vocabulary.

## What this is not

- Not a sentiment score. Sentiment is a byproduct, not the goal.
- Not a ticket priority system. Severity here describes the *signal*, not internal engineering priority.

## Related docs

- [community-feedback-loop.md](community-feedback-loop.md)
- [build-in-public-operating-system.md](build-in-public-operating-system.md)
