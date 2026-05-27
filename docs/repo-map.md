# OpenEmployee Repo Map

## Repositories

- `open-employee`: coordination hub, docs, roadmap, quickstart, and local development entrypoint.
- `open-employee-core`: Temporal workflow and Activity runtime for durable MCP execution.
- `open-employee-connectors`: connector protocols and MCP/Pipedream/Exa/OpenManus integration skeletons.
- `open-employee-ui`: operator UI for chat, workflow timelines, MCP Activity cards, and approval flows.
- `open-employee-examples`: runnable examples and `employee.yaml` patterns.
- `open-employee-identity`: portable identity, policy, and secret-boundary design.

## Dependency direction

`examples` and `ui` depend on public contracts from `core`, `connectors`, and `identity`.

`core` should stay dependency-light and must not hard-depend on cloud-specific identity or secret providers in v0.
