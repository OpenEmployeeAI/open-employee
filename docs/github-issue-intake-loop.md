# GitHub Issue Intake Loop

The first concrete slice of the [community feedback loop](community-feedback-loop.md). GitHub is the only source in phase one.

## Goal

Turn GitHub issues, discussions, and PR review comments into typed `FeedbackItem`s that feed the roadmap workflow, without any autonomous writes to GitHub.

## Surface in this repo

- Issue templates under `.github/ISSUE_TEMPLATE/` capture structured fields up front so the classifier has clean input.
- This doc defines the read Activities and the human-approval boundary for any write.
- No code lives here. The implementation lives in `open-employee-core` and `open-employee-connectors`.

## Read Activities (no approval required)

Each runs as its own Temporal Activity:

- `mcp__github__list_issues` — paged list of issues by repo, label, and time window.
- `mcp__github__get_issue` — single issue with body and metadata.
- `mcp__github__list_issue_comments` — comments on a single issue.
- `mcp__github__list_discussions` — discussions (when enabled on the repo).
- `mcp__github__list_pull_request_review_comments` — PR review comments tagged as feedback.

Outputs that exceed the claim-check size threshold are stored externally and referenced by artifact id.

## Write Activities (approval required, `risk: public`)

- `mcp__github__create_issue_comment`
- `mcp__github__add_labels`
- `mcp__github__remove_labels`
- `mcp__github__close_issue`
- `mcp__github__reopen_issue`

A write Activity is only scheduled after a human approval event is recorded on the workflow. The approval payload carries the reviewer identity and the target `external_id`. The Activity resolves `auth_ref` to a usable credential at execution time; the credential never appears in workflow history.

## Intake fields from the issue template

The community feedback template (added alongside this doc) collects:

- one-line summary
- source channel (where the user first hit this)
- expected vs actual behavior or desired capability
- whether the reporter would pay for, adopt, or contribute to a fix
- links to related threads

The classifier maps these fields to the [PMF signal taxonomy](pmf-signal-taxonomy.md).

## Failure handling

- Rate limits surface as structured `MCPToolError`s with retry hints; the workflow waits and resumes.
- Deleted or moved issues are recorded as a terminal state on the `FeedbackItem`, not silently dropped.
- Duplicate `external_id`s are deduplicated by the intake workflow before classification runs.

## Out of scope for this slice

- Non-GitHub sources.
- Auto-labeling or auto-closing.
- Posting roadmap updates back to issues (deferred to a later slice with explicit approval gating).

## Related docs

- [community-feedback-loop.md](community-feedback-loop.md)
- [gitroom-style-architecture.md](gitroom-style-architecture.md)
- [pmf-signal-taxonomy.md](pmf-signal-taxonomy.md)
