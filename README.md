# AI Skills

Reusable skills for Claude Code and Codex.

Each folder under `skills/` is a portable skill with a `SKILL.md` entrypoint. Supporting references travel with it; `agents/openai.yaml` supplies optional Codex interface metadata.

## Repository structure

This repository is a collection of independent skills. Each catalog entry has its own folder; add a sibling folder for each new skill.

```text
skills/
  english-repository-content/
    SKILL.md
    agents/openai.yaml
  git-commit-rules/
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
  stateless-jwt-auth/
    SKILL.md
    agents/openai.yaml
    references/
  svg-icon-assets/
    SKILL.md
    agents/openai.yaml
    references/
  <another-skill>/
    SKILL.md
```

The angle-bracket names illustrate future skills. Each skill owns its instructions and supporting resources. Projects install only the folders they need, and can use different source revisions for different skills. The root README and contributor instructions belong to this collection and do not need to be installed with a skill.

## Available skills

| Skill | Purpose |
| --- | --- |
| [english-repository-content](skills/english-repository-content/SKILL.md) | Apply a shared English-language policy to repository content while preserving required names, external identifiers, and source data. |
| [git-commit-rules](skills/git-commit-rules/SKILL.md) | Review commit scope, verify configured author and committer identities, draft English messages, and keep commit, history-rewrite, and push authorization separate. |
| [project-documentation](skills/project-documentation/SKILL.md) | Create and maintain README, shared AGENTS/CLAUDE instructions, architecture, ADRs, topic guides, and plans while preserving project conventions and avoiding duplicated records. |
| [react-granular-store-subscriptions](skills/react-granular-store-subscriptions/SKILL.md) | Connect React components to only the state they need, using Redux, Zustand, or another store's native subscription model. Reduce unnecessary renders before adding component memoization. |
| [stateless-jwt-auth](skills/stateless-jwt-auth/SKILL.md) | Require salted password records, application-signed JWTs, validation before user-state lookups, and revocation tied to the user's current credentials, without per-session storage. |
| [svg-icon-assets](skills/svg-icon-assets/SKILL.md) | Generate icons and favicons from SVG masters, design for small display sizes, wire appropriate 1x/2x/3x outputs, and optimize delivered assets in the production build. |

## Use in another project

The current skills define reusable project practices. Keep the shared procedure in a skill and the project's technical facts, paths, and exceptions in its own documentation. A short conditional rule in `AGENTS.md` connects the two; it replaces repeated generic instructions rather than adding another copy of them.

For this collection, **project installation includes adoption**: make the complete skill available and integrate that rule into the project's instructions. This is one shared installation procedure, not a separate installer built into every skill.

| Stage | Review scope |
| --- | --- |
| Initial installation and adoption | Find the existing rules for the skill's subject across project instructions and relevant documentation, remove equivalent duplication, resolve conflicts, and record project exceptions. For documentation, also review the existing organization. |
| Ordinary use after adoption | Reuse those rules and exceptions. Read the context needed for the current task, apply the skill to affected work, and run the checks relevant to that result. Do not repeat the initial repository-wide review or reopen settled choices without new evidence. |

The adopted instruction link and recorded project exceptions carry the outcome of setup into later sessions; a separate adoption report or completion flag is unnecessary. A newly encountered conflict still needs attention, but it does not automatically justify reviewing the entire repository again. An explicit audit request can broaden the review. Updating a skill calls for reviewing the changed guidance and affected project rules, rather than repeating installation from scratch.

For example, ask the agent working in the destination project:

```text
Install project-documentation from tomviglini/skills (skills/project-documentation) in this project, following the collection's installation and adoption procedure.
```

The installing agent should:

1. Read the selected `SKILL.md` and find the existing rules for its subject in README, root and nested `AGENTS.md`/`CLAUDE.md`, contributor instructions, and relevant guides under `docs/` or equivalent locations. Copy the complete skill folder to the appropriate directory, or reuse the selected existing installation. Preserve intentional local changes.
2. Reconcile those rules using the boundaries below. Replace covered generic instructions wherever they occur with references as needed, and add or update one conditional `AGENTS.md` link to the installed `SKILL.md`. State when it must be applied; a list of available skills is insufficient. Preserve project-specific requirements and nested scope, and avoid duplicate rules on reinstall.
3. Keep shared rules reachable from the project's Claude Code instructions. Retain a working `CLAUDE.md` import or native `AGENTS.md` setup instead of duplicating the rules.
4. For `project-documentation`, review the current docs, retain adequate organization and content, and make necessary compatible updates. Present a concrete proposal and ask before a substantial migration not already chosen by the user. Adopting the other practices does not by itself request a code refactor, icon redesign, translation of old content, or Git operation.
5. Check files and references, then report the installed location, adopted rule, generic instructions replaced, exceptions retained, and any pending decision. Installation does not authorize commits, pushes, or unrelated project changes.

These are instructions for the installing agent, not a hook that runs when files are copied. When using a third-party installer, have the agent complete this adoption step after the download.

### Reconcile existing rules

This review applies to every adopted practice, not only `project-documentation`. Compare meaning and scope, rather than removing a section merely because its heading matches the skill. A mixed section may contain both covered rules and requirements the skill does not own.

| Skill | Generic guidance that can move to the skill | Information that stays in the project |
| --- | --- | --- |
| `english-repository-content` | English-language rules for code, documentation, UI, logs, and other covered content | Localization exceptions, conversation-language preferences, externally required text, and commit-message rules outside this skill's scope |
| `git-commit-rules` | Covered commit-review, identity-checking, and message-writing procedures | Required identities, signing/hooks, and project-specific authorization or message conventions |
| `project-documentation` | Document responsibilities, maintenance procedure, and ADR history rules | Actual architecture, decisions and their rationale, document paths, evidence, and pending work |
| `react-granular-store-subscriptions` | General subscription and memoization practices | Installed library/version, store ownership and lifetimes, domain constraints, and measured behavior |
| `stateless-jwt-auth` | Password storage, JWT validation order, credential binding, and revocation requirements | Existing client contracts, hashing parameters, key ownership, claim names, lifetimes, credential-update paths, and explicit migration exceptions |
| `svg-icon-assets` | General SVG-source, density, small-size review, and optimization practices | Canonical asset paths, chosen artwork, output requirements, generation commands, licenses, and verification limits |

Keep one current home for each retained rule and update references to any removed sections. A concise human-facing summary with a link can remain useful in README; the full procedure need not be repeated there. Preserve historical ADRs and evidence even when they describe a practice now supplied by a skill.

If an existing policy conflicts with the skill, identify the exact difference. Preserve explicit project exceptions or an already authorized choice. Otherwise, show the conflicting rules and ask whether to adjust the policy or record a scoped project exception before acting on that conflict. Do not invent an exception, silently discard a rule, or treat the conflict as duplication. Record an agreed exception in the relevant project instructions or guide, with its scope and reason. This conflict rule applies during ordinary skill use as well as adoption; unrelated work can continue while the decision is pending. Equivalent cleanup and reference updates do not need a separate confirmation. Routine skill use does not repeat the repository-wide adoption review.

### Copy the skill files

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

The installation paths follow the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) and [Claude Code skill documentation](https://code.claude.com/docs/en/skills#choose-where-skills-load).

## Versioned reuse

The copy commands copy local files. They do not resolve a version, record the source revision, or perform adoption by themselves.

For reproducible use, commit the skills and select a specific Git commit before copying. Release tags such as `v0.1.0` can name repository snapshots once published; resolve the tag to its full commit SHA and keep published tags unchanged. A repository tag covers all skills at that revision.

Record the source repository, skill path, and full commit SHA in the consuming project's documentation. Include project-installed files in version control when a commit is authorized. Each project can then retain its chosen revision. To upgrade, select a newer revision, review the changes to the skill and its supporting files, and update both the installed copy and recorded SHA together. Reconcile affected adoption rules and exceptions without repeating an unrelated policy audit or adding duplicate instructions.

Track each installed skill separately. A collection release does not require installing or upgrading every skill; a project can update one selected folder while retaining its other installed skills.

This is a manual workflow. There is no version-aware installer, lockfile, or automatic updater yet. A version label in skill metadata alone would not enforce a pinned installation.

## Invoke a skill

Each skill's description allows automatic selection for matching tasks, and Codex metadata explicitly keeps that selection enabled. Adoption makes the matching workflow a project instruction, so users do not need to name the skill for routine relevant work. The agent reads the full skill when the condition applies, not every skill on every task.

| Adopted skill | Required use | Does not trigger it by itself |
| --- | --- | --- |
| `english-repository-content` | Writing, editing, or reviewing repository content under the adopted policy | Conversation or reading files for context |
| `git-commit-rules` | Commit-message preparation or requested commit, history-rewrite, or push work | Ordinary coding, `git status`, or code review |
| `project-documentation` | Requested documentation work or necessary updates identified by the documentation-impact assessment | A code change with no documentation impact, or reading README for context |
| `react-granular-store-subscriptions` | React store subscription work or diagnosis of store-driven rerenders | Unrelated React layout, styling, or imports |
| `stateless-jwt-auth` | Password and application-issued JWT authentication work under the adopted model, including credential changes and revocation | Unrelated UI, ordinary endpoints using unchanged authentication, or external-provider-only authentication |
| `svg-icon-assets` | Icon, logo, favicon, tray/touch icon, and associated asset-pipeline work | Unrelated UI work or general image handling |

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

To prepare a commit with the Git skill in Codex:

```text
Use $git-commit-rules to review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

In Claude Code:

```text
/git-commit-rules Review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

Explicitly request a commit or push when you want that operation performed. Within this repository, the contributor instructions also point to the Git skill, so it can guide the collection's own commits without installing a second copy.

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

### Stateless JWT authentication

This skill defines an implementation contract: a complete password record with its salt embedded, application-signed JWTs, cryptographic and claim validation before user-state lookups, and a binding to the user's current stored credentials. Existing framework facilities can satisfy these requirements after verification; a framework name or skill installation alone does not establish compliance. The instructions are abstract and contain no JavaScript implementation.

The model avoids per-session or per-access-token storage, sticky sessions, per-request token writes, and unnecessary issuer calls. Replicas verify JWTs locally and reuse authenticated context within each request. It still reads current user credentials after the JWT passes its stateless checks. After a password change, an earlier JWT may retain a valid application signature but must no longer grant access because its credential binding fails; the shared signing key does not need to change. It does not require a second per-user salt field. Detailed cryptographic choices and lifecycle guidance load only when relevant.

During adoption, review existing authentication against the contract and retain project-specific decisions and explicit exceptions. Complete missing mechanisms within an authorized implementation scope; identify unresolved gaps and migration choices when adoption is limited to installing instructions. Routine use checks affected authentication paths without repeating the initial audit.

In Codex:

```text
Use $stateless-jwt-auth to implement password-based JWT authentication, rejecting invalid tokens before user-state lookups and invalidating tokens when stored credentials change.
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
