# A2A Handoff Format

Every OpenEmployee agent thread must end with this handoff block:

```yaml
from:
to:
repo:
branch:
pr:
status:
files_changed:
contracts_changed:
tests_run:
blockers:
risks:
next:
```

## Field guide

- `from`: agent, human, or system handing off.
- `to`: intended next owner.
- `repo`: affected repository.
- `branch`: working branch.
- `pr`: pull request URL or `none`.
- `status`: concise status such as `done`, `blocked`, `needs-review`, or `in-progress`.
- `files_changed`: changed files or `none`.
- `contracts_changed`: typed contracts or public interfaces changed.
- `tests_run`: exact checks run.
- `blockers`: blockers requiring human or upstream action.
- `risks`: known risks and mitigations.
- `next`: recommended next action.
