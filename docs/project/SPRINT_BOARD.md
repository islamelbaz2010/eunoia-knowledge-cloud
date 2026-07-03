# Sprint Board — Eunoia Knowledge Cloud

This board tracks active work in the current sprint. After KC-0 completion (2026-07-03), sprint planning begins for KC-1.

## Sprint KC-1 Planning

Sprint KC-1 is scheduled to begin after KC-0 freeze approval. The following reflects the recommended work allocation and dependencies.

### Sprint Goals

- Deliver working reference implementation of registry, generator, validator, and publisher.
- Establish CLI interfaces and end-to-end demo flow.
- Create contract tests and validation tooling.
- Document all APIs and operational procedures.

### Active Components (recommended prioritization for KC-1)

#### Phase 1 (Critical path)

- [ ] Registry layer: file-backed implementations (Source, Dataset, Pack, Template, Schema, Taxonomy).
- [ ] Filesystem layer: canonical layout and utilities.
- [ ] Configuration: loader and schema.

#### Phase 2 (Dependent on Phase 1)

- [ ] Generator: input resolution, template rendering, staging.
- [ ] Validator: schema checks, policy checks, quality heuristics.
- [ ] Pack Builder: manifest generation, checksums, pack assembly.

#### Phase 3 (Dependent on Phases 1-2)

- [ ] Publisher: channel promotion, signing stub, publish receipt.
- [ ] CLI: command implementations.
- [ ] Orchestration: end-to-end runner.

#### Phase 4 (Parallel with earlier phases)

- [ ] Contract tests for manifest and registry.
- [ ] Demo pack and test data.
- [ ] Documentation and runbooks.

### Sprint Metrics

- Definition of Done: all contract tests pass, demo pack builds end-to-end, documentation complete.
- Code review: all PRs require 1 approval and pass linting/tests.
- Backlog management: sprint backlog refined weekly, impediments tracked.

### Current Sprint (pre-KC-1)

- Status: Planning phase.
- Next: Architecture Board approval of KC-0 freeze, then sprint planning kickoff.
