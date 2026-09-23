# Decision and Plan Patterns

Read this reference only when a project needs a new ADR or plan format, or when its existing format is being revised. Preserve established formats otherwise. The outlines describe useful content; omit fields or sections that add no information.

## Architecture and ADRs

A combined architecture document can start with system context, component boundaries, invariants, and runtime flows, followed by a numbered decision log. Keep detailed setup or API contracts in linked guides. Split a large log only when navigation or independent editing justifies the migration, preserving decision IDs and updating references.

For each new decision, record:

- **Identifier and title:** a stable ID and the actual choice.
- **Status:** use existing terminology. Defaults are Proposed, Accepted, Provisional, Rejected, and Superseded; provisional choices should state their unresolved condition.
- **Date:** when the decision was made, if known. Label a retrospective recording date explicitly.
- **Context:** the problem, constraints, and meaningful alternatives.
- **Decision:** the selected approach and why it fits those constraints.
- **Consequences:** benefits, costs, and remaining limitations.
- **Related records:** relevant guides, evidence, and superseded or superseding ADRs.

Acceptance, implementation, and verification are separate facts. A decision may be accepted with implementation pending; tests may pass locally while production remains unverified. Link the maintained work inventory for outstanding steps instead of duplicating its checklist in the ADR.

For example, an accepted queue architecture can have pending worker implementation. Document the chosen boundary in the ADR, show the actual current runtime in the architecture overview, and track the unfinished worker in the roadmap. Do not describe the proposed worker as already running.

## Roadmap or work inventory

Prefer the project's existing tracker or roadmap. Each active item needs a clear outcome, a status, and its remaining work or acceptance evidence. Link to a detailed plan only when one is needed.

Useful statuses distinguish pending work, work in progress, deferred work, unresolved proposals, and implemented work awaiting verification. Use the project's vocabulary; do not introduce a parallel status system just for documentation.

Remove completed items from the active queue or mark them complete according to project conventions. Preserve meaningful deferred or rejected choices where their rationale helps prevent repeated discussion. Do not turn the roadmap into a chronological transcript of every edit.

## Task or feature plan

A focused plan can contain:

1. **Outcome and scope:** the intended behavior, constraints, and explicit exclusions.
2. **Current state:** relevant implementation and accepted decisions, with links.
3. **Proposed work:** coherent steps and dependencies, separating settled choices from open questions.
4. **Acceptance criteria:** observable outcomes and how they will be checked.
5. **Progress and remaining questions:** enough to resume the effort without copying the roadmap or durable guides.

Do not treat a draft plan as a product decision or a work order. When implementation changes an assumption, update the active plan and record any consequential accepted decision in the ADR log. At completion, leave lasting behavior and evidence in the guides, then close or archive the plan so it cannot be mistaken for current architecture.
