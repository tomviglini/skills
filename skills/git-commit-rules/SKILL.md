---
name: git-commit-rules
description: "Prepare and review Git commits, check configured author and committer identities, and write concise English commit messages. Use when asked to draft a commit message, prepare or create commits, amend or reorganize commits, or push branches or tags. Keep commit, history-rewrite, and push authorization separate."
---

# Git Commit Rules

Make the intended change, attribution, and authorization clear before recording or publishing Git history. Follow the user's explicit instructions and the repository's applicable requirements.

## Authorization and scope

- Create a commit only when the user explicitly authorizes committing the changes in the active task. Editing files, completing an implementation, invoking this skill, or requesting a message does not itself authorize a commit.
- Authorization remains valid while completing that same task, including follow-up clarifications. It does not carry over to unrelated tasks or additional changes outside the authorized scope. Do not request the same authorization again when it is already clear.
- A normal commit request authorizes a new commit. Amending, squashing, rebasing, or otherwise rewriting existing commits requires explicit authorization for that operation and scope.
- Push a branch or tag only when the user explicitly authorizes that push for the active task. A commit request alone does not authorize a push, and a push request alone does not authorize creating additional commits.
- Force-push only when explicitly authorized for that operation and target. Do not use force to work around an ordinary push rejection.
- A message-only request authorizes reviewing relevant changes and drafting text. Leave the index, history, configuration, and remotes unchanged.

## Review the intended changes

1. Inspect the repository state and the applicable instructions. Review staged and unstaged diffs, and inspect the contents of relevant untracked files. Initial repositories may have no `HEAD`; use checks that work before the first commit.
2. Determine which changes belong to the requested commit. Preserve unrelated work and existing partial staging. Stage explicit paths or hunks only after establishing their scope; do not include unrelated staged changes merely because they are already in the index.
3. For an authorized commit, review the final staged diff, including added and deleted files. Draft the message from that content. For a message-only request, use the requested diff or staged changes by default, and state the assumed scope if the user supplied none.
4. Run checks appropriate to the change and required by the repository. Report failures or unavailable checks accurately. Avoid repeating successful checks unless the candidate commit changed or a failure needs investigation.

If commit boundaries are materially ambiguous, complete the available review and draft before asking which changes to include. Do not silently discard or unstage someone else's work to resolve the ambiguity.

## Verify identity before creating or rewriting a commit

Read [identity checks](references/identity.md) and inspect the effective author and committer for the intended operation. For a new commit, both need an explicitly configured, non-placeholder name and email. Validate the resolved identities, including environment overrides, rather than relying only on `user.name` and `user.email`. For an operation that preserves an existing author, inspect that source attribution as described in the reference.

Do not hardcode a person's identity, invent an identity, infer one from repository ownership or past commits, or silently replace configuration. Accept a clearly configured personal identity without requiring legal-name verification. An automation identity is appropriate only when explicitly designated by the user or applicable repository policy.

If an identity is missing or generic, finish the independent review and message draft, then request the missing identity or configuration. Change identity settings only using values the user has supplied or approved for that purpose; prefer repository-local settings when configuring this repository, and change global settings only when requested. Recheck after any correction.

Author and committer need not be identical. Preserve legitimate original authorship during an authorized history operation. Do not replace an original author with the person performing the operation merely to make the fields match. A push-only request does not need a new-commit identity check.

## Write the commit message

- Write a concise English subject describing the actual change, starting with a lowercase imperative verb.
- Preserve the capitalization of proper names, product names, identifiers, and acronyms.
- Aim for 72 characters or fewer and omit a trailing period.
- Describe the meaningful change or resulting behavior. Avoid vague subjects and generic summaries of activity.
- Do not add Conventional Commits prefixes unless the user requests them or the repository explicitly requires them. In that case, apply the lowercase imperative guidance to the description after the prefix.
- Add a body when the reason, tradeoff, or relevant validation needs explanation beyond the subject. Include only claims supported by the work performed.
- Do not invent co-authors or append assistant attribution. Include attribution and other trailers only when requested or required by applicable repository rules, using accurate identities.

## Execute and verify authorized operations

Before execution, ensure the reviewed scope, identity, and message still match the intended commit. When invoking `git commit`, include the command-scoped `-c user.useConfigOnly=true` Git option to prevent guessed identity defaults at execution as well. Use safe argument passing; for a multiline message, pass its literal contents through a file or standard input rather than interpolating it into shell code.

Respect configured hooks and signing requirements. If a check or hook fails, inspect the result before retrying. Review any hook-generated changes before staging them, and confirm whether a commit was created before repeating the command. Do not bypass a failing hook or signing requirement merely to finish the task.

After a commit, inspect the recorded message, raw author and committer fields, and changed files, then check the remaining working-tree status. Report the commit ID and subject. If the recorded result is wrong, report it and establish authorization for any needed history correction instead of silently amending.

For an authorized push, verify the destination and intended branches or tags, publish only that scope, and report the outcome. Keep the final status explicit about whether work was drafted, committed, or pushed.
