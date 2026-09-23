# Documentation Layout

Read this reference when establishing or reorganizing documentation, choosing a home for missing information, or creating or restructuring README. Preserve existing paths when they serve these roles; this is not a required file inventory.

| Document | Responsibility | Create or expand when |
| --- | --- | --- |
| `README.md` | Purpose, current capabilities, essential setup/use, and links to deeper documentation | A reader needs an entry point to the project |
| `AGENTS.md` | Working rules, constraints, verification requirements, and conditions for consulting other documents | Agents need project-specific guidance |
| `CLAUDE.md` | Access to shared instructions, plus Claude-specific guidance if needed | The project's Claude Code setup needs it; follow [shared agent instructions](agent-instructions.md) |
| `docs/architecture-and-decisions.md` | Current architecture, boundaries, invariants, and numbered ADRs | The system has structure or consequential choices worth explaining |
| `docs/<topic>.md` | Detailed behavior, development or operational procedures, and relevant evidence | The detail has a distinct audience or would obscure the entry point |
| `docs/roadmap.md` or an existing tracker | Maintained inventory of pending, deferred, or undecided work | There is actual work to track |
| A task or feature plan at the project's established location | Scope, intended outcome, acceptance criteria, and execution steps | A particular effort needs detail beyond a roadmap entry |

Keep small projects small. Combine related material when it remains readable. Do not create empty guides, speculative roadmaps, a glossary, or an ADR directory merely to fill out a structure. A feature plan and roadmap can coexist if the roadmap links to the plan instead of maintaining a competing status inventory.

Make the README useful to someone opening the repository for the first time. Show the shortest supported setup and verification path, prerequisites, meaningful limitations, and links to detailed guides. Separate optional platform workflows from the common path. A short overview may summarize a guide without copying its full procedure or decision rationale.

Derive commands from the project's scripts and tooling. State platform requirements and material side effects. Do not invent a successful run, expose local credentials, or execute a deployment just to document its command.

Keep agent instructions focused on project constraints, required checks, and what to read for a given kind of change. Keep the documentation-impact assessment as a short standing rule; require the full skill only for initial organization or affected documentation updates. Preserve the project's Git authorization rules, language exceptions, and component scope.
