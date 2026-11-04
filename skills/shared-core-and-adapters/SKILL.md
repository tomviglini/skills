---
name: shared-core-and-adapters
description: "Design, extract, change, or review shared domain/protocol logic and its platform adapters. Use for shared-core boundaries and adapter integration; excludes ordinary UI or platform-only work that leaves those boundaries unchanged."
---

# Shared Core and Adapters

Keep behavior that must agree across consumers in portable shared logic, and connect it to each environment through explicit adapters. Prefer deterministic domain functions and small dependency contracts using the project's existing conventions. Shared orchestration may have state and effects; make their ownership and dependencies explicit instead of requiring the entire core to be stateless or purely functional.

Preserve the project's frameworks, libraries, repository layout, and release boundaries. This practice does not require a monorepo, microservices, a dependency-injection framework, a common UI, or a single store for every platform. Share behavior because its meaning and change requirements agree, not merely because two code fragments look similar.

## Scope adoption and ordinary use

- During initial adoption or an explicit architecture review, identify the relevant consumers, shared rules, contracts, runtime dependencies, and existing decisions. Compare the documented boundaries with actual imports and wiring. Reconcile duplicated generic instructions through the project's adoption procedure while retaining its paths, constraints, exceptions, and historical ADRs. Do not create a separate installer or adoption report.
- During ordinary use, reuse the established boundaries and inspect the affected rule, dependency contract, adapter, and necessary callers. An unrelated UI, styling, or platform implementation change does not require a repository-wide architecture audit.
- For review or explanation, report the boundary and any gaps without refactoring. Installing the skill does not itself authorize extracting packages or moving application code. For requested implementation, complete the relevant integration rather than adding an unused abstraction as evidence of adoption.
- Respect explicit project exceptions and resolved choices. If a requirement conflicts with an existing project rule, identify the exact conflict and ask whether to change that rule or record a scoped exception before the disputed change. Do not invent exceptions or reopen already settled decisions. Different package names, module layouts, and suitable libraries are ordinary adaptations.

## Put each responsibility at the right boundary

| Responsibility | Preferred home |
| --- | --- |
| Domain calculations, invariants, protocol encoding/decoding, and validation rules that consumers must agree on | Portable core, independent of UI and concrete I/O implementations. |
| Shared data contracts and runtime schemas | A portable module or entry point available to the consumers that need it. Preserve the project's schema library; avoid pulling in persistence or server initialization just to access a contract. |
| Common workflows and state transitions | Shared logic when semantics match, with effects supplied explicitly and state scoped to its owner. Sharing this logic does not require sharing a store instance. |
| Network connections, filesystem/database access, native APIs, and platform storage | Adapters that translate between the core's needs and the chosen platform facilities. |
| UI components, navigation, rendering subscriptions, permissions, foreground/background handling, and platform feedback | The relevant frontend or platform integration. Reuse an existing shared UI where appropriate without making it a dependency of domain logic. |

Keep one authoritative implementation of a rule that must remain identical where consumers can share executable code. If their languages or runtimes cannot share it, preserve one agreed contract and verify the separate implementations against common behavioral examples; do not add a service or migrate languages solely to eliminate duplication. Keep intentionally different behavior local or represent a real variation through a small explicit input or policy. Do not force unrelated features into a generic engine or add extension points for hypothetical future platforms.

## Keep dependencies and ownership explicit

- Let the core describe the capabilities it needs through narrow functions, callbacks, interfaces, or the language's equivalent. Platform code implements and supplies them where the application is assembled. Do not have portable rules import a concrete adapter and select it through environment checks.
- Prefer the smallest useful dependency boundary. Passing a value or function is often sufficient; do not add a service container, base class hierarchy, or generic repository layer solely to satisfy this skill. Keep existing abstractions that already express the required contract.
- Keep domain calculations deterministic by supplying variable context such as the current instant or business configuration when it affects the result. Read environment variables, initialize SDKs, and open resources in the owning runtime. Use existing testable runtime facilities for shared orchestration rather than wrapping every standard-library call by default.
- Define relevant inputs, outputs, failure behavior, and resource ownership at the boundary. Translate platform-specific errors when consumers need a common meaning; preserve enough cause information for diagnosis. Adapters may handle platform mechanics, but should not independently redefine shared business rules.
- Give stateful core instances the correct request, user, device, session, or application lifetime. Specify who starts, closes, cancels, or disposes their resources. Reusing a module must not accidentally share mutable state across independent owners or introduce duplicate connections and background work.
- Keep portable entry points free of platform-only imports and startup side effects, including through transitive dependencies and re-exports. Portable does not mean dependency-free: suitable calculation, parsing, or schema libraries can remain. A shared backend module can legitimately depend on a database without belonging in a browser-compatible core.
- Keep shared data contracts distinct from trust and execution authority. Sharing validation with a client does not replace server-side validation or authorization. Keep secrets and privileged implementations outside client-consumable dependencies; avoid exposing an entire persistence model just to share a few data fields.

## Change the boundary without changing the product accidentally

Preserve externally observable behavior, protocol rules, persisted formats, and intentional platform differences during extraction unless the task explicitly changes them. Reuse the relevant existing tests and examples to establish that baseline. Shared logic does not require identical platform lifecycles, build toolchains, dependency versions, or release schedules.

Read [extraction and verification](references/extraction-and-verification.md) when creating or moving a boundary, changing a dependency contract across consumers, or diagnosing a portability problem. A local rule change through an unchanged interface can use existing core checks without reviewing every adapter again.

For implementation, verify the changed rules and the affected real integrations using the project's tools. Pure-rule tests, tests with injected dependencies, and actual adapter checks establish different things; do not claim platform support from a fake transport alone. When a shared contract changes, identify its consumers and check the affected ones, including independently built or released components.

Update the existing architecture or topic guide only when ownership, contracts, dependencies, or meaningful limitations change. Record significant new decisions using the project's ADR conventions while preserving history. Report the resulting boundary, checks performed, and remaining platform verification without treating code sharing as measured performance improvement.
