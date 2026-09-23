# Repository Instructions

- Write all skill content, examples, documentation, and contributor-facing text in English.
- Store each independent reusable skill under `skills/<skill-name>/SKILL.md`. Add sibling folders as the collection grows, and keep each folder portable on its own.
- Keep the shared skill compatible with Claude Code and Codex: use `name` and `description` frontmatter and plain Markdown instructions. Put optional Codex metadata in `agents/openai.yaml`.
- Keep each skill self-contained. Use relative links for supporting resources and avoid machine-specific paths or required agent-specific tools.
- Describe when a skill applies precisely. Preserve the consuming project's library choices and conventions.
- Put substantial library-specific details in references, loaded only when relevant.
- For version-sensitive code examples, state the target library versions, documentation review date, sources, and validation status. Distinguish API targets from exact versions actually tested; adapt to the consuming project's installed versions instead of requiring upgrades to match examples.
- Update the root README catalog when adding or renaming a skill.
- Check frontmatter, relative links, examples, and version-sensitive claims before finishing a change. Use an available skill validator when possible; this repository has no application build or test suite.
- When preparing commit messages or performing commit, history-rewrite, or push operations in this repository, read and apply [git-commit-rules](skills/git-commit-rules/SKILL.md). Creating or editing a skill does not itself authorize a commit or push.
