# AI Skills

Reusable skills for Claude Code and Codex. All instructions, examples, and documentation are written in English.

Each folder under `skills/` is a portable skill with a `SKILL.md` entrypoint. Supporting references travel with it; `agents/openai.yaml` supplies optional Codex interface metadata.

## Repository structure

This repository is a collection of independent skills. Each catalog entry has its own folder; add a sibling folder for each new skill.

```text
skills/
  git-commit-rules/
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
| [git-commit-rules](skills/git-commit-rules/SKILL.md) | Review commit scope, verify configured author and committer identities, draft English messages, and keep commit, history-rewrite, and push authorization separate. |

## Use in another project

Copy the complete skill folder into the destination for your agent. Run these commands from this repository, replacing `/path/to/project` with the target project directory. These examples assume a fresh installation.

The examples install `git-commit-rules`. Substitute another catalog entry to install that skill instead.

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

The installation paths follow the official [Codex skill documentation](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills) and [Claude Code skill documentation](https://code.claude.com/docs/en/skills#choose-where-skills-load).

## Versioned reuse

The current copy commands install the local files. They do not resolve a version or record where the installed copy came from.

For reproducible use, commit the skills and select a specific Git commit before copying. Release tags such as `v0.1.0` can name repository snapshots once published; resolve the tag to its full commit SHA and keep published tags unchanged. A repository tag covers all skills at that revision.

Record the source repository, skill path, and full commit SHA in the consuming project's documentation, and commit the installed skill folder there. Each project can then retain its chosen revision. To upgrade, select a newer revision, review the skill and its supporting files, and update both the installed copy and recorded SHA together.

Track each installed skill separately. A collection release does not require installing or upgrading every skill; a project can update one selected folder while retaining its other installed skills.

This is a manual workflow. There is no version-aware installer, lockfile, or automatic updater yet. A version label in skill metadata alone would not enforce a pinned installation.

## Invoke a skill

Each skill's description allows automatic selection for matching tasks. You can also invoke an installed skill by name.

To prepare a commit with the Git skill in Codex:

```text
Use $git-commit-rules to review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

In Claude Code:

```text
/git-commit-rules Review the intended changes, check Git identity, and draft a commit message without committing or pushing.
```

Explicitly request a commit or push when you want that operation performed. Within this repository, the contributor instructions also point to the Git skill, so it can guide the collection's own commits without installing a second copy.

## Add a skill

Create `skills/<skill-name>/SKILL.md` with a lowercase, hyphenated name and YAML frontmatter:

```markdown
---
name: skill-name
description: Describe the capability and the concrete tasks that should trigger it.
---

# Skill Title

Task-specific instructions in English.
```

Keep the entrypoint concise. Add references only when they help with a particular library or workflow, and link them from the entrypoint. Keep shared instructions independent of agent-specific tools and local machine paths.

### Library versions in examples

Keep general principles in the skill entrypoint and library-specific API targets beside the examples. Each code reference should identify its target versions, documentation review date, official sources, and whether the examples were only reviewed or also executed. Record exact tested versions when executable validation exists; a major-version API target does not claim a tested compatibility range.

Agents should compare those targets with the consuming project's resolved dependencies and adapt the code. These library versions are separate from the Git revision used to version the skill itself.
