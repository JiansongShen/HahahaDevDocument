# ADR: Architecture Decision Records

ADRs capture **what we finally decided** and **why we decided it**. Their value is that future readers can quickly understand the constraints and trade-offs at the time, rather than repeating the same discussions again.

## What belongs here

- Key decisions that the team has agreed on (e.g. adopting a protocol / storage / architectural split)
- Context, constraints, alternatives, and rationale
- Impact and follow-up actions (migration, compatibility, risks)

## What does not belong here

- Drafts that are still undecided (put them in `en/design/`)
- Raw process notes and review comments (put them in `en/discussions/`)

## Numbering and naming

- File name: `NNNN-title.md` (e.g. `0001-cpp23.md`)
- Title format: `ADR-NNNN: ...`
- Directory organization: Related ADRs can be organized in subdirectories (e.g. `1-cpp23/`, `2-basic-project-design/`)

## ADR List

### ADR-0001: Adopt Modern C++23 Features
- Location: [0001-cpp23.md](0001-cpp23.md)
- Status: Accepted
- Content: Decision to fully embrace C++23 standard, using modern features like Concepts and stacktrace

### ADR-0002: Basic Project Design
- Location: [2-basic-project-design/](2-basic-project-design/)
- Status: Accepted
- Content: Includes basic design decisions such as project structure, CI/CD requirements, dependency management, testing requirements, feature addition requirements, documentation and comment requirements, contribution guidelines, and error handling guidelines
