# Shared Agent Instructions

Read this reference when creating or reorganizing agent instruction files or connecting documentation maintenance to an installed skill.

## Keep one source of shared rules

For a new shared setup, put tool-neutral working rules in `AGENTS.md`. Keep architecture, operational detail, and decision history in their own documents, reached through conditional reading instructions. Preserve an existing working arrangement instead of reversing its references solely to match this default.

If both `AGENTS.md` and `CLAUDE.md` already contain rules, reconcile their unique requirements before removing duplicated text. Preserve nested component instructions at their current scope; moving them into the root can accidentally make a component-specific rule apply everywhere. Do not erase tool-specific instructions during consolidation.

When a compatibility bridge is appropriate, a sibling `CLAUDE.md` can contain:

```text
@AGENTS.md
```

This is the file's actual content, without a code fence. Adjust the relative path when the files are in different directories. Avoid circular imports. Import the shared rules, not the entire architecture document or all topic guides: those should remain conditional reads.

Claude Code supports `@path` imports relative to the importing file. Its current documentation also describes direct `AGENTS.md` loading in v2.1.277 and later, subject to instruction-file settings and the presence of Claude instruction files. Preserve a working direct-loading setup; do not assume every installation needs a bridge or that every version loads `AGENTS.md` automatically. Check nested-file behavior when component instructions matter. [Official memory documentation](https://code.claude.com/docs/en/memory#import-additional-files), [AGENTS.md support](https://code.claude.com/docs/en/memory#agentsmd).

Compatibility target: the documented Claude Code import mechanism and v2.1.277+ direct-loading feature, reviewed September 23, 2026. No exact Claude Code version was executed to validate these examples. Check the consuming environment's installed version and configuration; do not upgrade it or change personal settings merely to match this example.

## Write conditional reading instructions

Adapt these examples to real files and requirements in the consuming repository:

```markdown
## Documentation

- Before architectural or cross-cutting changes, read
  `docs/architecture-and-decisions.md` and the relevant topic guides.
- Before finishing a change, assess whether README, architecture, decisions,
  guides, or the maintained work plan need updates. Apply the installed
  project-documentation skill and complete affected updates in the same task.
  Create missing documents only when warranted; leave adequate records unchanged.
- Ask before a substantial documentation migration unless it was already chosen.
- Keep accepted decisions distinct from implementation and verification status.
  Supersede changed decisions explicitly and preserve their original rationale.
```

Keep critical project constraints explicit in agent instructions. Link detailed procedures with a concrete trigger instead of requiring every document to be read for every task. Add pointers only for documents that exist.

## Register the installed skill as a project rule

When adopting this practice in a project, locate generic documentation-maintenance rules in README, agent instructions, contributor guides, and relevant docs. Replace rules already covered by the skill with references, including a conditional link to its installed `SKILL.md` in `AGENTS.md`. Retain unique requirements from mixed sections, project-specific paths, constraints, exceptions, and historical ADRs. The resulting `AGENTS.md` must require a documentation-impact assessment before completing every project change and application of the skill for affected updates. Resolve the actual installed path before writing the link; do not point to the author's checkout.

Review the existing documentation during adoption. Leave adequate organization and content unchanged, make necessary compatible updates, and ask before a substantial migration not already chosen by the user. Ordinary use afterward maintains only the affected documents; it does not repeat the initial review of the whole repository.

Prefer a repository-relative link for a project-installed skill. For a personal installation, a by-name instruction may be appropriate when the project intentionally depends on that setup; keep essential project rules understandable without it. A reference neither installs the skill nor guarantees that another environment can discover it. Do not copy the skill's full text into `AGENTS.md`.
