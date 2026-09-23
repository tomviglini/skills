---
name: project-documentation
description: "Set up, consolidate, review, or update project documentation and decision records. Use for requested documentation work or changes needing documented behavior or decisions updated."
---

# Project Documentation

Keep project knowledge in a small, connected set of documents with clear responsibilities. Support both initial organization and maintenance after a change. Preserve the consuming project's conventions, language policy, libraries, and architecture; standardize how knowledge is recorded, not how the application is built.

A brief documentation-impact assessment belongs in the project's standing instructions. Load this skill when documentation work is requested or an impact is identified; merely reading README for context does not require the documentation workflow.

## Establish the current state

- For setup, adoption, or requested consolidation, read applicable agent instructions, the README, and the existing documentation index. Locate architectural decisions, topic guides, and the maintained source of pending work, then choose the smallest useful structure.
- For ordinary maintenance after adoption, start from the requested change, applicable instructions, and the documents it affects. Use the README or index as needed to locate them. Reuse recorded paths and exceptions without repeating the initial repository-wide review; broaden the review only when requested or justified by relevant new evidence. Inspect nested instructions for the affected components.
- Check relevant code, manifests, scripts, configuration, and tests before describing behavior or commands. Existing prose and code can disagree; identify whether the difference is a defect, an outdated description, or an accepted but unfinished change. Do not silently resolve a product decision by choosing whichever source is easier to copy.
- Distinguish confirmed requirements, proposals, accepted decisions, implemented behavior, and observed verification. Preserve evidence and limitations. Ask only about unresolved intent or choices that the repository and conversation cannot establish.
- Respect documented project exceptions and choices already resolved by the user. For an unresolved conflict with this guidance, show the conflicting requirements and ask whether to adjust the project rule or record a scoped exception before the disputed change. Do not invent an exception; record an agreed one in the relevant project instructions or guide with its scope and reason.
- Reconcile existing material during setup or consolidation. During maintenance, update only affected documents; a small correction does not require reorganizing the repository.
- If the existing organization differs substantially and adoption would replace its workflow, move many documents, or merge or split decision histories, first present a concrete mapping of current documents to the proposed arrangement and ask whether to adapt the existing structure or migrate it. Reuse an explicit choice already made by the user. Different filenames, paths, or headings alone do not require a migration or another question; continue compatible maintenance while a migration decision is pending.

## Assign each kind of information a home

Reuse the existing homes for orientation, working rules, architecture, decisions, detailed guides, and pending work. Keep one authoritative home for each fact or status and link to it instead of copying procedures, rationale, or progress checklists.

- Read [documentation layout](references/documentation-layout.md) when establishing or reorganizing the structure, choosing where missing documentation belongs, or creating or restructuring README. A routine update to an existing guide does not require this reference.
- Read [shared agent instructions](references/agent-instructions.md) when creating or reorganizing `AGENTS.md` and `CLAUDE.md`, or adding a reference to this skill. Preserve project constraints, Git authorization rules, language exceptions, and component scope.

Complete necessary documentation updates within the task. Create missing documents only when warranted; no documentation impact means no edit or ADR. Do not generate empty scaffolding, handoffs, or routine session logs.

## Maintain architecture and decisions

- Describe the current system through component responsibilities, dependencies, runtime/data flows, and invariants. Include diagrams only when they clarify those relationships. Keep proposed architecture visibly separate from implemented architecture.
- Add an ADR when a significant choice affects boundaries, lifecycle, persistence, networking, security, protocols, platform support, packaging, or release strategy, and its rationale or trade-offs will help future work. Routine implementation details and facts obvious from code belong elsewhere or need no documentation.
- Reuse the repository's ADR location, numbering, statuses, and template. A combined architecture document and a directory of individual ADRs are both valid; do not introduce a second decision log. Read [record patterns](references/record-patterns.md) only when establishing or changing the format of ADRs or plans.
- Preserve accepted decision history. Record a changed decision as a new ADR and mark the earlier record as superseded, linking both directions and stating the affected part if the replacement is partial. Correct factual errors transparently without inventing a historical rationale.
- Keep the current architecture overview aligned with the resulting system while retaining superseded decisions as history. Decision acceptance does not establish implementation, deployment, or verification. When documenting an older choice retrospectively, distinguish the recording date from a known decision date.

## Maintain guides and planning

- Put detailed contracts, edge cases, operational steps, and validation evidence in the relevant topic guide. Explain reasons and limits where they affect use; keep raw reports or baselines as linked evidence instead of copying their contents into every summary.
- Preserve distinctions between synthetic checks, local execution, real-device observations, and production validation. Identify the environment and date when they matter to interpreting evidence.
- Track only actual pending work. Separate proposals, accepted but unfinished work, deferred items, and outstanding validation. A roadmap or plan records intent; it does not authorize implementation or deployment beyond the user's request.
- Use a feature plan when scope, dependencies, or acceptance criteria need their own document. Link it from the existing work inventory. After delivery, transfer lasting behavior to guides and significant decisions to ADRs, then close or archive the plan according to project conventions.
- During consolidation, preserve unique rationale, constraints, evidence, and open questions before retiring a document. Update incoming links and retain historical records that still explain current behavior. Keep file moves within the requested scope.

## Verify the documentation change

- Check changed links, anchors, agent imports, ADR identifiers, supersession references, and incoming links affected by moves. Ensure required-reading pointers name files that exist and explain when to read them. Do not leave links to removed documents or competing sources of current status.
- Compare new or changed commands and behavior claims with their implementation sources; reuse established evidence for unaffected details. For version-sensitive examples, record the API or tool target, documentation review date, official sources, and validation status; distinguish reviewed targets from exact versions actually tested. Adapt to installed versions rather than requiring upgrades to match an example.
- Check the applicable language policy, preservation of project-specific constraints, and the distinction between facts and proposals. Keep personal machine details out of shared setup instructions unless the project explicitly documents that environment.
- Run relevant documentation checks and any checks required by the project. Application builds or device tests are useful only when required or relevant to the change; a prose edit does not establish fresh runtime evidence.

Report which documents changed, what checks ran, and any unresolved questions or unverified claims. If the requested change has no documentation impact, leave the documents alone and state that conclusion briefly.
