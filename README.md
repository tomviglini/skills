# AI Skills

Reusable skills for Claude Code and Codex.

Each folder under `skills/` is a portable skill with a `SKILL.md` entrypoint. Supporting references travel with it; `agents/openai.yaml` supplies optional Codex interface metadata.

## Repository structure

This repository is a collection of independent skills. Each catalog entry has its own folder; add a sibling folder for each new skill.

```text
skills/
  asset-versioning-and-cache/
    SKILL.md
    agents/openai.yaml
    references/
  checksum-named-artifacts/
    SKILL.md
    agents/openai.yaml
    references/
  concept-prototype/
    SKILL.md
    agents/openai.yaml
  design-tuning-lab/
    SKILL.md
    agents/openai.yaml
    references/
  english-repository-content/
    SKILL.md
    agents/openai.yaml
  git-commit-rules/
    SKILL.md
    agents/openai.yaml
    references/
  grilling/
    SKILL.md
    agents/openai.yaml
  installed-app-lifecycle/
    SKILL.md
    agents/openai.yaml
    references/
  mobile-ui-basics/
    SKILL.md
    agents/openai.yaml
    references/
  project-documentation/
    SKILL.md
    agents/openai.yaml
    references/
  react-granular-store-subscriptions/
    SKILL.md
    agents/openai.yaml
    references/
  shared-core-and-adapters/
    SKILL.md
    agents/openai.yaml
    references/
  stateless-jwt-auth/
    SKILL.md
    agents/openai.yaml
    references/
  svg-icon-assets/
    SKILL.md
    agents/openai.yaml
    references/
  task-lifecycle/
    SKILL.md
    agents/openai.yaml
  <another-skill>/
    SKILL.md
```

The angle-bracket names illustrate future skills. Each skill owns its instructions and supporting resources. Projects install only the folders they need, and can use different source revisions for different skills. The root README and contributor instructions belong to this collection and do not need to be installed with a skill.

## Available skills

| Skill | Purpose |
| --- | --- |
| [asset-versioning-and-cache](skills/asset-versioning-and-cache/SKILL.md) | Version static assets through the production build, recommend one-year immutable caching for content-derived URLs, and preserve project-specific mutable-resource and deployment policies. |
| [checksum-named-artifacts](skills/checksum-named-artifacts/SKILL.md) | Name installable development and release artifacts with a SHA-256 content prefix, print the full digest, and keep named copies where later builds do not delete them. |
| [concept-prototype](skills/concept-prototype/SKILL.md) | Run an explicitly invoked prototype-phase exploration: map an idea's real-use moments, weigh approaches by their architecture and trade-offs, implement it in the app for the owner to try, and defer documentation until the owner settles the direction. |
| [design-tuning-lab](skills/design-tuning-lab/SKILL.md) | Build interactive pages where the owner tunes and compares design values in real context, with one view per place, visible defaults, revert and press-to-compare controls, and returned values folded into a consistent system. |
| [english-repository-content](skills/english-repository-content/SKILL.md) | Apply a shared English-language policy to repository content while preserving required names, external identifiers, and source data. |
| [git-commit-rules](skills/git-commit-rules/SKILL.md) | Review commit scope, verify configured author and committer identities, draft English messages, and keep commit, history-rewrite, and push authorization separate. |
| [grilling](skills/grilling/SKILL.md) | Run an explicitly invoked interview, implement when the user asks to proceed, then update docs and ADRs from the actual implementation and verification. |
| [installed-app-lifecycle](skills/installed-app-lifecycle/SKILL.md) | Install, update, and uninstall desktop apps while keeping saved data across versions: per-item format versions and migrations, consent and a backup before any loss, TOML settings, opt-in start with the OS, and update checks against published releases. |
| [mobile-ui-basics](skills/mobile-ui-basics/SKILL.md) | Keep mobile text entry, layouts, scrolling, touch controls, feedback, and navigation usable while preserving the project's design and libraries. |
| [project-documentation](skills/project-documentation/SKILL.md) | Create and maintain README, shared AGENTS/CLAUDE instructions, architecture, ADRs, topic guides, and plans while preserving project conventions and avoiding duplicated records. |
| [react-granular-store-subscriptions](skills/react-granular-store-subscriptions/SKILL.md) | Connect React components to only the state they need, using Redux, Zustand, or another store's native subscription model. Reduce unnecessary renders before adding component memoization. |
| [shared-core-and-adapters](skills/shared-core-and-adapters/SKILL.md) | Share portable domain, protocol, and interaction logic through explicit platform adapters while preserving runtime ownership and independent UI and build choices. |
| [stateless-jwt-auth](skills/stateless-jwt-auth/SKILL.md) | Require salted password records, private-key JWT signing and public-key verification, validation before user-state lookups, and revocation tied to the user's current credentials, without per-session storage. |
| [svg-icon-assets](skills/svg-icon-assets/SKILL.md) | Generate icons and favicons from SVG masters, design for small display sizes, wire appropriate 1x/2x/3x outputs, and optimize delivered assets in the production build. |
| [task-lifecycle](skills/task-lifecycle/SKILL.md) | Treat everything the owner asks for until they close it as one task: focused checks and a Git-ignored log of decisions while working, then documentation and the full checks once, when the owner closes the task. |

## Skills used in this repository

This repository adopts `english-repository-content`, `git-commit-rules`, `project-documentation`, and `task-lifecycle` for its maintenance, and makes `grilling` available for explicit invocation. Their use conditions live in [AGENTS.md](AGENTS.md); [CLAUDE.md](CLAUDE.md) imports those same instructions. The remaining catalog entries are reusable source material, not practices adopted by this collection merely because their files are present.

Each of these five skills has a relative directory symlink in both `.agents/skills/` for Codex and `.claude/skills/` for Claude Code, pointing to `../../skills/<skill-name>`. Both agents therefore discover the complete local skill, including references and invocation metadata, without maintaining duplicate content. Edit the canonical files under `skills/`. These links use the current working tree, including uncommitted edits; they are for maintaining this collection, not pinned installations in consuming projects.

Keep these links and their targets together in version control when committing is authorized, and verify that the checkout preserves working symlinks. This setup is contained in the repository and requires no personal skill installation or global configuration. Other projects should use the installation and adoption procedure below.

## Use in another project

This collection contains reusable project practices and explicitly invoked workflows. Keep the shared procedure in a skill and the project's technical facts, paths, and exceptions in its own documentation. For a project practice, a short conditional rule in `AGENTS.md` connects the two; it replaces repeated generic instructions rather than adding another copy of them.

For this collection, **project installation requires both native discovery and adoption**, for automatic practices and manual commands alike:

| Part | Required result |
| --- | --- |
| Native discovery | The complete skill folder is accessible in a location recognized by each target agent, preserving its frontmatter, supporting files, and invocation metadata. This makes it available to the agent's skill system and selector. |
| Project adoption | `AGENTS.md` points to the installed skill and states when to apply it, with existing project rules reconciled and exceptions retained. Claude Code can reach those shared instructions through the project's existing setup. |

A README entry documents the setup; it does not install the skill. An `AGENTS.md` link can instruct an agent to read a file outside its skill directories, but that alone does not make it discoverable as a native skill. Conversely, copying the folder does not perform the project's policy reconciliation. Keep this shared installation procedure here, rather than building an installer into every skill.

For an explicitly invoked workflow such as `grilling`, adoption records its availability and invocation boundary, preserving its manual controls. Do not turn it into a required step before every change or run it as part of installation. Reconcile only existing rules for that workflow; the user's invocation starts its discussion, and a subsequent instruction to proceed authorizes implementation and its necessary documentation updates.

| Stage | Review scope |
| --- | --- |
| Initial installation and adoption | Find the existing rules for the skill's subject across project instructions, relevant guides, and ADRs or equivalent decision records. Distinguish effective decisions from historical or proposed ones, remove equivalent duplication, resolve conflicts, and record project exceptions. For documentation, also review the existing organization. |
| Ordinary use after adoption | Reuse those rules and exceptions. Read the context needed for the current task, apply the skill to affected work, and run the checks relevant to that result. Do not repeat the initial repository-wide review or reopen settled choices without new evidence. |

The adopted instruction link and recorded project exceptions carry the outcome of setup into later sessions; a separate adoption report or completion flag is unnecessary. A newly encountered conflict still needs attention, but it does not automatically justify reviewing the entire repository again. An explicit audit request can broaden the review. Updating a skill calls for reviewing the changed guidance and affected project rules, rather than repeating installation from scratch.

For example, ask the agent working in the destination project:

```text
Install project-documentation from tomviglini/skills (skills/project-documentation) in this project, following the collection's installation and adoption procedure.
```

The installing agent should:

1. Read the selected `SKILL.md` and find the existing rules for its subject in README, root and nested `AGENTS.md`/`CLAUDE.md`, contributor instructions, relevant guides, and ADRs or equivalent decision records, including decisions in combined architecture documents. Determine the target agents and project or personal scope from the request and existing setup; default a project installation to repository scope. Make the complete folder discoverable in each target agent's supported location, using the paths below or reusing a suitable existing installation. Preserve intentional local changes and automatic/manual invocation controls. Do not stop at adding an instruction link, and do not install every catalog entry when only selected skills are needed.
2. Reconcile those rules using the boundaries below, including [ADR reconciliation](#reconcile-adrs-during-adoption). Replace covered generic instructions in current guidance with references as needed, preserve historical decision records, and add or update one `AGENTS.md` link to the installed `SKILL.md`. For project practices, state the condition that requires applying it; a list of available skills is insufficient. Record the project-specific values the skill relies on, such as check commands, paths, or ignored files, where the skill expects them. For explicitly invoked workflows, state that they run only on user invocation. Preserve project-specific requirements and nested scope, and avoid duplicate rules on reinstall.
3. Keep shared rules reachable from the project's Claude Code instructions. Retain a working `CLAUDE.md` import or native `AGENTS.md` setup instead of duplicating the rules. This instruction bridge does not replace Claude Code's own skill-discovery location when Claude Code is a target agent.
4. For `project-documentation`, review the current docs, retain adequate organization and content, and make necessary compatible updates. Present a concrete proposal and ask before a substantial migration not already chosen by the user. Adopting the other practices does not by itself request a code refactor, icon redesign, translation of old content, or Git operation.
5. Check every installed folder's `SKILL.md`, supporting links, invocation controls, and project instruction links. Verify symlink targets when used. Check discovery in each target client's skill list or selector when accessible, without invoking a workflow just to test installation; otherwise report that native discovery remains unverified. Report the installed locations and scope, adopted rule, generic instructions replaced, exceptions retained, and any pending decision. Installation does not authorize commits, pushes, or unrelated project changes.

These are instructions for the installing agent, not a hook that runs when files are copied. When using a third-party installer, verify its actual destination, supported discovery scope, and copied metadata, then complete adoption after the download. A download alone is not a completed project installation under this procedure.

### Reconcile existing rules

This review applies to every adopted practice, not only `project-documentation`. Compare meaning and scope, rather than removing a section merely because its heading matches the skill. A mixed section may contain both covered rules and requirements the skill does not own.

| Skill | Generic guidance that can move to the skill | Information that stays in the project |
| --- | --- | --- |
| `asset-versioning-and-cache` | Asset fingerprinting, reference rewriting, immutable-cache defaults, and delivery verification practices | Mutable-resource lifetimes, asset paths and origins, build/hosting configuration, retention and recovery decisions, and explicit exceptions |
| `checksum-named-artifacts` | Content-hash filename format, digest printing, copy-versus-rename, stale-output, and retention practices | Build commands, covered variants and targets, output directories and shortcuts, and explicit exceptions |
| `concept-prototype` | An existing explicit procedure for trying product ideas in the app before documenting them | Product decisions, demo or simulation controls, and the documentation and testing rules applied once a direction is chosen |
| `design-tuning-lab` | Conventions for interactive design-decision pages: real context, per-view controls, visible defaults, revert, compare, and settings exchange | Design decisions and their evidence, capture sources, chosen values, archive locations, and explicit exceptions |
| `english-repository-content` | English-language rules for code, documentation, UI, logs, and other covered content | Localization exceptions, conversation-language preferences, externally required text, and commit-message rules outside this skill's scope |
| `git-commit-rules` | Covered commit-review, identity-checking, and message-writing procedures | Required identities, signing/hooks, and project-specific authorization or message conventions |
| `grilling` | An existing explicit interview procedure, transition to authorized implementation, and documentation of the verified outcome | Actual requirements, agreed decisions, pending questions, document destinations, and any separately adopted automatic planning rules |
| `installed-app-lifecycle` | Data-folder item rules, per-item versions, migration and consent before loss, backup and uninstall handling, settings-file rules, start-with-OS default, update-check behavior, and release publishing | Data location, the actual items and their versions, migrations, installer tool and paths, release location, and explicit exceptions |
| `mobile-ui-basics` | Keyboard visibility, focus, scrolling, touch interaction, feedback, navigation, and focused usability checks | Supported platforms and devices, design system, installed libraries, navigation semantics, native configuration, verified behavior, and explicit exceptions |
| `project-documentation` | Document responsibilities, maintenance procedure, and ADR history rules | Actual architecture, decisions and their rationale, document paths, evidence, and pending work |
| `react-granular-store-subscriptions` | General subscription and memoization practices | Installed library/version, store ownership and lifetimes, domain constraints, and measured behavior |
| `shared-core-and-adapters` | Shared-rule placement, dependency boundaries, adapter integration, and boundary verification practices | Actual domain rules, module paths, contracts, runtime lifetimes, supported platforms, toolchains, release constraints, and explicit exceptions |
| `stateless-jwt-auth` | Password storage, asymmetric signing and public-key verification, JWT validation order, credential binding, and revocation requirements | Existing client contracts, hashing parameters, key ownership, claim names, lifetimes, credential-update paths, and explicit migration exceptions |
| `svg-icon-assets` | General SVG-source, density, small-size review, and optimization practices | Canonical asset paths, chosen artwork, output requirements, generation commands, licenses, and verification limits |
| `task-lifecycle` | When documentation updates and the full checks run, the pending-notes file, and committing after a close | The focused and full check commands, documentation rules and derived-material policies, and Git authorization rules |

Keep one current home for each retained rule and update references to any removed sections. A concise human-facing summary with a link can remain useful in README; the full procedure need not be repeated there. Preserve historical ADRs and evidence even when they describe a practice now supplied by a skill.

If an existing policy conflicts with the skill, identify the exact difference. Preserve explicit project exceptions or an already authorized choice. Otherwise, show the conflicting rules and ask whether to adjust the policy or record a scoped project exception before acting on that conflict. Do not invent an exception, silently discard a rule, or treat the conflict as duplication. Record an agreed exception in the relevant project instructions or guide, with its scope and reason. This conflict rule applies during ordinary skill use as well as adoption; unrelated work can continue while the decision is pending. Equivalent cleanup and reference updates do not need a separate confirmation. Routine skill use does not repeat the repository-wide adoption review.

#### Reconcile ADRs during adoption

During installation and adoption of any skill, compare its requirements with the ADRs relevant to its subject. This includes explicitly invoked workflows, within their own scope. Reuse the project's decision index, statuses, and supersession links to identify which decisions remain effective. Preserve superseded or rejected records as history and keep proposals distinct from accepted decisions. An accepted decision may still await implementation; existing code alone does not cancel it. If a relevant decision's status or applicability remains unclear after reviewing the available evidence, ask for clarification before the disputed action.

| Relationship to the skill | Adoption action |
| --- | --- |
| Fully covered, with the same decision | Keep the ADR's status, rationale, and history. Add a concise reference to the installed skill for the current reusable procedure; remove equivalent duplication from current instructions and guides where appropriate. |
| Partially covered | Reference the covered practice and retain the project's specific requirements, parameters, constraints, rationale, and exceptions. Make the boundary clear without rewriting the historical decision. |
| Contradicts an effective decision, with no resolved exception | Identify the ADR and the exact skill requirement, explain the consequence, recommend an option, and ask whether to change the decision or retain it as a scoped project exception. Continue independent adoption work while the disputed part remains pending. |

Reuse explicit exceptions and user choices that already resolve the same difference. An accepted ADR's existence does not by itself establish an exception to a newly adopted skill. Record an agreed exception with its scope and reason in the project's maintained instructions or guide, linking the ADR when useful.

Do not delete or mark an ADR as superseded merely because a skill now covers its practice. Supersede it only when the underlying decision actually changes: record the replacement using the project's ADR conventions, link both directions, and identify the affected scope for a partial change. Skill installation alone does not require a new ADR. If a file labeled as an ADR contains only procedural instructions, propose moving those instructions to the skill while preserving any decision context and updating references; do not silently remove a decision record as duplicate content.

Keep this review in initial adoption. Ordinary use and skill updates revisit only decisions affected by the task or changed guidance, unless new evidence or an explicit audit justifies a broader review. A skill update never silently overrides a project decision.

### Copy the skill files

Choose a supported location for each target agent:

| Scope | Codex | Claude Code | Portability |
| --- | --- | --- | --- |
| Project | `<repo>/.agents/skills/<skill-name>/` | `<repo>/.claude/skills/<skill-name>/` | Travels with the repository when the complete files and any link targets are included in version control. |
| Personal | `~/.agents/skills/<skill-name>/` | `~/.claude/skills/<skill-name>/` | Applies across projects for that user in that environment; install separately in other machines, containers, or remote environments as needed. |

Install only for the agents and scope the project uses. For a portable project setup, keep all skill files inside the consuming repository. If both agents are targets, use complete copies in both locations or a complete canonical folder plus relative directory symlinks supported by the clients and checkout. Keep both locations on the selected revision. Do not use a link to the author's external checkout as a portable installation.

For manual copying, run the following from this repository, replacing `/path/to/project` with the target project directory. These commands cover the file-copy step only; complete adoption afterward. The examples use `git-commit-rules` and assume a fresh destination. Substitute another catalog entry as needed.

```bash
# Codex
mkdir -p /path/to/project/.agents/skills
cp -R skills/git-commit-rules /path/to/project/.agents/skills/

# Claude Code
mkdir -p /path/to/project/.claude/skills
cp -R skills/git-commit-rules /path/to/project/.claude/skills/
```

For personal use across projects, copy to the corresponding home directory instead:

```bash
# Codex
mkdir -p "$HOME/.agents/skills"
cp -R skills/git-commit-rules "$HOME/.agents/skills/"

# Claude Code
mkdir -p "$HOME/.claude/skills"
cp -R skills/git-commit-rules "$HOME/.claude/skills/"
```

Choose project or personal installation for each agent to avoid duplicate copies. Copies are snapshots: update them when you change the source skill. Restart your agent if the skill does not appear.

A personal copy makes the skill available across projects. Complete adoption in each project that should require it; a personal installation does not authorize modifying every repository. For project copies, use relative instruction links. For personal copies, register a by-name rule when appropriate rather than a machine-specific path.

The installation paths and directory-symlink support follow the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) and [Claude Code skill documentation](https://code.claude.com/docs/en/skills#choose-where-skills-load), reviewed on 2026-09-23. Local file and link checks do not establish that a running client's selector has refreshed; restart the client if a skill does not appear.

## Versioned reuse

The copy commands copy local files. They do not resolve a version, record the source revision, or perform adoption by themselves.

For reproducible use, commit the skills and select a specific Git commit before copying. Release tags such as `v0.1.0` can name repository snapshots once published; resolve the tag to its full commit SHA and keep published tags unchanged. A repository tag covers all skills at that revision.

Record the source repository, skill path, and full commit SHA in the consuming project's documentation. Include project-installed files in version control when a commit is authorized. Each project can then retain its chosen revision. To upgrade, select a newer revision, review the changes to the skill and its supporting files, and update both the installed copy and recorded SHA together. Reconcile affected adoption rules and exceptions without repeating an unrelated policy audit or adding duplicate instructions.

Track each installed skill separately. A collection release does not require installing or upgrading every skill; a project can update one selected folder while retaining its other installed skills.

This is a manual workflow. There is no version-aware installer, lockfile, or automatic updater yet. A version label in skill metadata alone would not enforce a pinned installation.

## Invoke a skill

Reusable project practices keep automatic selection enabled. Adoption makes their matching workflow a project instruction, so users do not need to name the skill for routine relevant work. Explicitly invoked workflows such as `grilling` disable automatic selection and run only when the user invokes them. The agent reads the relevant skill rather than every installed skill on every task.

| Adopted skill | Required use | Does not trigger it by itself |
| --- | --- | --- |
| `asset-versioning-and-cache` | Static-asset versioning, cache-header or delivery changes, and stale/missing assets across deployments | Ordinary artwork, content, or styling changes using an unchanged delivery pipeline; unrelated API caching |
| `checksum-named-artifacts` | Creating, changing, or reviewing commands that build installable development or release artifacts | Running an unchanged build command; web asset URLs or product version numbers |
| `concept-prototype` | Explicit user invocation of the concept-prototype command | Ordinary feature requests, planning, design-value tuning, installation, or another skill suggesting it |
| `design-tuning-lab` | Building or revising an interactive page for the owner to explore or decide visual design values | Implementing a chosen design in the product, static galleries or contact sheets, and ordinary UI fixes |
| `english-repository-content` | Writing, editing, or reviewing repository content under the adopted policy | Conversation or reading files for context |
| `git-commit-rules` | Commit-message preparation or requested commit, history-rewrite, or push work | Ordinary coding, `git status`, or code review |
| `grilling` | Explicit user invocation of the grilling command | Ordinary planning, ambiguous requirements, architecture changes, installation, or another skill suggesting an interview |
| `installed-app-lifecycle` | Changing an installer or uninstaller, anything the app keeps between runs, update checks or release publishing, and preparing a release that changes saved data | Ordinary features that leave saved data and the installer unchanged; web deployments or app-store releases |
| `mobile-ui-basics` | Creating, changing, or reviewing mobile screens and their input, layout, or interaction behavior | Backend-only changes, desktop-only UI, or asset generation without changing the mobile interaction |
| `project-documentation` | Requested documentation work or necessary updates identified by the documentation-impact assessment | A code change with no documentation impact, or reading README for context |
| `react-granular-store-subscriptions` | React store subscription work or diagnosis of store-driven rerenders | Unrelated React layout, styling, or imports |
| `shared-core-and-adapters` | Shared domain/protocol logic, extraction or review of core boundaries, and adapter integration | Ordinary UI or platform-only work using unchanged boundaries; reading a shared module for context |
| `stateless-jwt-auth` | Password and application-issued JWT authentication work under the adopted model, including credential changes and revocation | Unrelated UI, ordinary endpoints using unchanged authentication, or external-provider-only authentication |
| `svg-icon-assets` | Icon, logo, favicon, tray/touch icon, and associated asset-pipeline work | Unrelated UI work or general image handling |
| `task-lifecycle` | Implementation work in the project: making and handing over changes, closing a task, and preparing a commit | Reading code, explaining, answering questions, or reviewing without changes |

For example, a project adopting documentation and icon practices could use the following rules. Adjust the links to actual installed locations and include only skills the project adopts:

```markdown
- Before finishing every project change, assess documentation impact. When
  setting up documentation or updating affected records, read and apply
  [project-documentation](.agents/skills/project-documentation/SKILL.md).
- When working on icons, logos, favicons, tray or touch icons, or their asset
  pipelines, read and apply
  [svg-icon-assets](.agents/skills/svg-icon-assets/SKILL.md).
```

Explicit invocation is also available; using a skill for one task does not by itself install or adopt it.

A skill can also describe a workflow for a specific deliverable, such as exporting the current task's context to a file. Such workflows may be invoked on demand rather than made a standing project rule. Choose the adoption trigger for the skill's purpose instead of requiring every installed skill on every task.

For workflows explicitly designated as manual, this collection sets `policy.allow_implicit_invocation: false` in `agents/openai.yaml` for [Codex](https://learn.chatgpt.com/docs/build-skills), and `disable-model-invocation: true` in `SKILL.md` frontmatter for [Claude Code](https://code.claude.com/docs/en/skills#control-who-invokes-a-skill). Preserve both controls when copying or updating them. The Claude Code field is a host extension; the portable instructions and required `name`/`description` remain shared. Controls reviewed against official documentation on 2026-09-23; interactive loading has not been exercised in both clients in this repository.

To prepare a commit with the Git skill in Codex:

```text
Use $git-commit-rules to review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

In Claude Code:

```text
/git-commit-rules Review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

Explicitly request a commit or push when you want that operation performed. Within this repository, the Git skill is discoverable through the local links described above, and the contributor instructions require applying it to matching Git work.

### Repository language

The [language skill](skills/english-repository-content/SKILL.md) owns the repository-content language rules. This repository's `AGENTS.md` points to it, and `CLAUDE.md` imports those instructions. Commit-message rules remain in the Git skill.

In Codex:

```text
Use $english-repository-content to keep the repository content in this task in English.
```

In Claude Code:

```text
/english-repository-content Keep the repository content in this task in English.
```

Project installation includes the language rule and its installed `SKILL.md` reference in the project's agent instructions; retain explicit language exceptions.

### Grilling

`grilling` is a manual command. It first investigates the relevant context and interviews the user about the chosen idea, requirement, or decision. It separates facts, existing decisions, recommendations, and open assumptions. By default, the proposal stays in the conversation during the discussion.

When the proposal is coherent, it presents the agreed direction and remaining questions. The user decides when to proceed with implementation. The agent then writes and verifies the code, resolves routine details, and returns to the user only for material changes to agreed decisions. After that work, it updates the affected guides, ADRs, or plan from the actual outcome, including relevant discoveries. Routine documentation maintenance needs no second approval; finishing the interview alone does not generate files or authorize implementation.

In Codex:

```text
Use $grilling to challenge this proposal. Discuss it with me first; when I ask you to proceed, implement and verify it, then update the affected docs and ADRs from the result.
```

In Claude Code:

```text
/grilling Review this design with me. Implement when I tell you to proceed, then document the resulting behavior and decisions after verification.
```

The user can stop after the conversation or explicitly request a written plan or design-only record instead of implementation. Such a record remains visibly unimplemented. A standalone idea does not require a repository. This skill needs no other skill installed; when the project has adopted `project-documentation`, its conventions govern the resulting updates. Implementation authorization does not by itself authorize a commit, push, or deployment.

### Concept prototype

`concept-prototype` is a manual command for a product in its prototype phase, when the owner has an idea of how something should work and wants to decide by trying it in the app. The agent restates the question, checks how the underlying technology actually behaves, and maps the real-use moments the idea must handle, such as first use, returning after minutes or hours, bad conditions, refused permissions, and changes during use. It weighs two or three approaches, including the owner's idea as stated, by their architecture and trade-offs, and identifies the parameters that are product decisions.

It then implements the behavior in the app, exposing hard-to-reproduce conditions and decision parameters through the project's demo controls, and puts the two most promising approaches behind a switch when that is cheap. When trying the idea needs something costly, such as a native dependency and a new build, it presents the approaches and a recommendation and lets the owner choose first. The report describes the concept, meaning the behavior in each moment, the architecture, the trade-offs, suggestions, and open questions, rather than the diff.

While it runs, the project's quick checks still run, but documentation, decision records, and new tests wait until the owner settles the direction. The agent then removes the discarded approaches and brings the change up to the project's standards. Invoking the command is itself the owner's instruction to defer that upkeep, even where a project requires documentation with every change, so projects need no rule or exception for it and a personal installation is enough.

In Codex:

```text
Use $concept-prototype to work out how location acquisition should fit into opening and returning to the app, and let me try it.
```

In Claude Code:

```text
/concept-prototype The first time, show "acquiring location" and fetch posts only when it finishes; coming back two minutes later should not show the same thing.
```

Choosing a direction does not authorize a commit, push, or deployment.

### Project documentation

Use the documentation skill to organize a new or existing project, or to maintain the affected documents after a change. It respects existing paths and decision history, creates only useful documents, and keeps proposals, accepted decisions, implementation, and verification distinct. It does not generate handoffs or routine session logs.

In Codex:

```text
Use $project-documentation to organize this project's README, shared AGENTS/CLAUDE instructions, architecture, ADRs, guides, and planning documents, preserving existing conventions.
```

In Claude Code:

```text
/project-documentation Update the documentation affected by this change, preserving existing paths, decisions, and verification limits.
```

Installation includes an initial documentation review and an `AGENTS.md` rule requiring an impact assessment before finishing every project change, followed by any necessary documentation updates. Substantially different structures require a concrete migration choice from the user; different filenames or equivalent layouts do not. See its [shared instruction guidance](skills/project-documentation/references/agent-instructions.md) for AGENTS/CLAUDE compatibility and portable references.

### Task lifecycle

`task-lifecycle` sets when documentation and the full checks run. A task is everything the owner asks for until they close it, however many requests it spans. While it is open, the agent makes the changes, runs the project's focused checks, and logs in a Git-ignored `.pending-docs.md` what the diff will not show: decisions and their reasons, discarded alternatives, discoveries, unverified points, and the documents expected to change. It hands the work over for the owner to try.

When the owner makes clear they are satisfied, the agent closes the task: it updates the documentation from the diff and the notes, runs the full checks, reports and fixes failures when it can, and deletes the notes. A commit requested after a close needs no further checks; a commit requested while notes exist closes the task first. The notes file also tells a new session that a task is open.

A project adopting it lists its focused and full checks in `AGENTS.md`, ignores `.pending-docs.md`, and moves rules that update documentation after every change, or run full suites at commit time, to the close. `project-documentation` decides what to document; this skill decides when.

In Codex:

```text
Use $task-lifecycle to close this task: update the documentation from the pending notes and run the full checks.
```

In Claude Code:

```text
/task-lifecycle Close this task: update the documentation from the pending notes and run the full checks.
```

### Asset versioning and cache

Use this skill when changing static-asset URLs, production asset delivery, cache policies, or compatibility across releases. It recommends one year plus `immutable` for public content-derived asset URLs; root pages, HTML, and other mutable URLs retain project-specific freshness policies. Production review establishes whether the CDN caches each affected resource, its effective lifetime, and the rule that controls it, then verifies the policy reaching the browser. It complements the icon skill without owning artwork, density, or compression choices.

Adoption reconciles existing delivery rules and records project decisions and exceptions. Ordinary use checks only affected behavior and loads header or deployment details when needed. An accepted limitation for older clients remains explicit; installation does not itself authorize deployment or a retention-system migration.

In Codex:

```text
Use $asset-versioning-and-cache to review this production asset pipeline and its cache policies, preserving the project's HTML freshness and deployment decisions.
```

In Claude Code:

```text
/asset-versioning-and-cache Implement content-derived asset URLs and appropriate cache headers using this project's existing build and hosting tools.
```

### Checksum-named artifacts

Use this practice when creating or changing the commands that build APKs, app bundles, development clients, installers, or similar artifacts installed by hand. Each build copies its output to `<project>-<variant>[-<target>]-<first eight SHA-256 hex characters>.<extension>` and prints the full digest, so a file identifies and verifies its own content independently of Git state. Development and release variants are both covered. When native-project regeneration or another variant's build deletes the conventional output directory, named copies go to a stable directory instead.

Adoption reconciles existing artifact-naming rules and keeps project facts, such as commands, output directories, and shortcuts, in the project's build documentation. Running an unchanged build does not activate the skill, and it does not replace version codes or signing.

In Codex:

```text
Use $checksum-named-artifacts to add SHA-256 filenames to this project's debug and release APK builds.
```

In Claude Code:

```text
/checksum-named-artifacts Review this installer build script against the checksum naming contract.
```

### Design tuning lab

Use this practice when the owner needs to decide visual values, such as a logo treatment, palette, type scale, or spacing, by trying them in context. The page shows each place under review in its own view with only the controls that affect it; every control shows its default, reverts to it, and compares against it while pressed. Values the owner returns are read as intent and folded into a consistent system before they become the new defaults. The skill is deliberately conceptual: it fixes what the owner must be able to see and do, not a page template or library.

A project adopting it records where its design evidence lives and any exceptions, such as the language of review pages. It does not replace the project's implementation, asset, or documentation workflows for the chosen design.

In Codex:

```text
Use $design-tuning-lab to build a page where I can tune the logo grid and the title treatment on real captures of the landing and the app.
```

In Claude Code:

```text
/design-tuning-lab Turn these spacing and type-scale options into a lab with one tab per screen, visible defaults, revert, and compare.
```

### Mobile UI basics

Use this practice when creating or changing a mobile screen, editable control, layout, or interaction, or when reviewing a mobile usability defect. It covers native apps and mobile web only where the project supports them. Ordinary use checks the affected behavior; it does not require a redesign, new dependency, or full-app audit. Keyboard details load separately for the relevant runtime.

A project adopting it can add a conditional instruction to read the installed `mobile-ui-basics` skill for that work, while retaining its supported platforms, design system, navigation rules, and existing exceptions. Adding this catalog entry does not install it in another project or adopt it in this collection.

In Codex:

```text
Use $mobile-ui-basics to fix this mobile form so its focused input stays visible above the keyboard and its submit action remains reachable.
```

In Claude Code:

```text
/mobile-ui-basics Review this mobile sheet's keyboard, scrolling, touch targets, and Back behavior without changing its visual design.
```

### React subscriptions

The React skill uses the project's existing state library; Redux and Zustand are examples, not prerequisites.

In Codex:

```text
Use $react-granular-store-subscriptions to refactor this screen so each component subscribes only to the state it renders.
```

In Claude Code:

```text
/react-granular-store-subscriptions Refactor this screen so each component subscribes only to the state it renders.
```

### Shared core and adapters

Use this skill when extracting or changing logic shared across consumers, defining its dependency boundaries, or integrating platform adapters. It prefers portable rules and explicit effects while allowing stateful shared workflows. UI, store ownership, native facilities, and concrete persistence remain in the appropriate consuming components; existing libraries and repository layout are preserved.

Adoption reconciles existing architecture rules and retains project decisions and history. Ordinary use examines only the affected boundary and consumers. Detailed extraction and verification guidance loads when relevant. Installation alone does not request an architecture migration, and sharing code does not require a shared store, a monorepo, or synchronized framework upgrades.

In Codex:

```text
Use $shared-core-and-adapters to extract the common protocol behavior while preserving each platform's transport and lifecycle.
```

In Claude Code:

```text
/shared-core-and-adapters Review this shared module's dependency boundaries and its adapters, preserving the project's existing frameworks and contracts.
```

### Stateless JWT authentication

This skill defines an implementation contract: a complete password record with its salt embedded, asymmetrically signed application JWTs, cryptographic and claim validation before user-state lookups, and a binding to the user's current stored credentials. Issuers hold the private signing key; other services can verify signatures and applicable claims with trusted public keys without gaining issuance authority. This is signing, not encryption of the claims. Existing framework facilities can satisfy these requirements after verification; a framework name or skill installation alone does not establish compliance. The instructions are abstract and contain no JavaScript implementation.

An edge proxy can use only public keys to filter invalid bearer tokens before forwarding requests, without private keys or user-state lookups. The backend completes authentication and authorization before granting protected access.

The model avoids per-session or per-access-token storage, sticky sessions, per-request token writes, and unnecessary issuer calls. Replicas verify JWTs locally and reuse authenticated context within each request. It still reads current user credentials after the JWT passes its stateless checks. Public-key verification alone does not enforce credential-change revocation; downstream services must rely on a trusted boundary for that check and secure its propagation. After a password change, an earlier JWT may retain a valid application signature but must no longer grant access because its credential binding fails; the shared signing key does not need to change. It does not require a second per-user salt field. Detailed cryptographic choices and lifecycle guidance load only when relevant.

During adoption, review existing authentication against the contract and retain project-specific decisions and explicit exceptions. Complete missing mechanisms within an authorized implementation scope; identify unresolved gaps and migration choices when adoption is limited to installing instructions. Routine use checks affected authentication paths without repeating the initial audit.

In Codex:

```text
Use $stateless-jwt-auth to implement password-based JWT authentication with private-key signing and public-key verification, rejecting invalid tokens before user-state lookups and invalidating tokens when stored credentials change.
```

In Claude Code:

```text
/stateless-jwt-auth Review this authentication flow against the adopted password, JWT, validation-order, and credential-revocation requirements.
```

## Add a skill

Create `skills/<skill-name>/SKILL.md` with a lowercase, hyphenated name and YAML frontmatter:

```markdown
---
name: skill-name
description: Describe the capability and the concrete tasks that should trigger it.
---

# Skill Title

Task-specific instructions.
```

Keep the entrypoint concise. Add references only when they help with a particular library or workflow, and link them from the entrypoint. Keep shared instructions independent of agent-specific tools and local machine paths.

Decide whether a new skill defines a reusable practice or a workflow for a particular deliverable. For a practice, document a concrete conditional adoption trigger here and keep automatic selection enabled. Keep common installation mechanics in this README and substantive task instructions in the portable skill folder. Add skill-specific setup only when that skill actually needs it.

Use manual-only invocation when explicitly requested by the user, as with `grilling`. Set the native controls for both supported agents described above, retain an explicit-invocation boundary in the instructions, and check that installation or another skill cannot turn it into an automatic prerequisite. Frontmatter validators that only accept standard fields may reject Claude Code's documented `disable-model-invocation` extension; check that field separately from the standard fields rather than removing the invocation control to satisfy a narrower validator.

### Keep ordinary use lightweight

- **Make selection precise.** Describe the task that needs the skill, not every related topic. Keep the description short and put the procedure in the body. Add exclusions for likely false matches, such as ordinary React styling for a store-subscription skill.
- **Separate adoption from use.** Adoption reconciles existing policies and records exceptions. Later tasks reuse that result and inspect only relevant context. A reference in `AGENTS.md` must say when to apply the skill rather than require all installed skills for every task.
- **Match work to the request.** Distinguish explanation, review, and implementation when they need different actions. Drafting a commit message does not inherently require a test run; reviewing an icon does not inherently require rebuilding the application.
- **Keep necessary verification.** Check the current change's behavior, output, or authorization when relevant. Reuse successful checks when the candidate and relevant inputs are unchanged; broaden or repeat them for new evidence, changed inputs, failures, or explicit requirements.
- **Load detail only when needed.** Keep the common workflow in `SKILL.md`; move substantial setup, platform, library, or optimization guidance into linked references with specific reading conditions. Preserve the actual requirements when moving them.
- **Reuse decisions and exceptions.** Ask about a newly encountered unresolved conflict. Do not rediscover or request approval for an exception already recorded by the project. A local conflict does not automatically require a repository-wide audit.
- **Avoid work created by the skill itself.** Do not require new reports, plans, document edits, refactors, or status files merely to show that a skill ran. Use existing project records and create an artifact only when the task needs it.

Before finishing a new or revised skill, review representative requests for initial adoption, ordinary matching work, and unrelated work that should not activate it. Include the relevant lighter modes (such as review-only or message-only), an existing exception, and a newly encountered conflict. Check that the intended result follows without an unnecessary audit or unrelated action. Use behavioral trials when warranted, and distinguish static review from observed execution; a valid frontmatter check alone does not demonstrate reliable activation or lower runtime cost.

### Library versions in examples

Keep general principles in the skill entrypoint and library-specific API targets beside the examples. Each code reference should identify its target versions, documentation review date, official sources, and whether the examples were only reviewed or also executed. Record exact tested versions when executable validation exists; a major-version API target does not claim a tested compatibility range.

Agents should compare those targets with the consuming project's resolved dependencies and adapt the code. These library versions are separate from the Git revision used to version the skill itself.
