---
name: english-repository-content
description: "Apply an adopted English-language policy when writing, editing, or reviewing repository content. Excludes conversational replies and commit-message rules."
---

# English Repository Content

Keep repository content in English within the project's adopted policy. The language of the user's request does not change the repository's language policy. Follow conversation-language preferences separately.

Apply this policy to authored or reviewed content. Reading files for context or answering a general question does not require a language audit.

## Language rules

- Use English for source code identifiers, comments, documentation, UI copy, logs, error messages, tests, scripts, and configuration descriptions.
- Honor explicit project language exceptions, such as localized UI or bilingual published content, and choices already resolved by the user. If a conflict remains, show the conflicting rules and ask whether to adjust the language policy or record a scoped exception before changing the affected content. Do not invent an exception; keep agreed exceptions in the project's instructions with their scope and reason.
- Use English when naming new branches and tags.
- For content covered by the English policy, write new or replacement text in English. Do not introduce non-English prose by copying it from the request.
- Preserve proper names, externally defined identifiers, and source data whose exact spelling or language is required for correctness. Write surrounding explanations and labels in English.
- Keep changes within the requested task. Do not translate unrelated content or rename existing public identifiers, branches, or tags solely to normalize their language.

## Review

During ordinary use, reuse the adopted policy and its recorded exceptions. Review only content being authored, changed, or explicitly reviewed; do not repeat a repository-wide language-policy audit. Revisit a policy choice only when a relevant new conflict appears or the user requests a review.

Check the changed identifiers and human-readable text for unintended non-English additions before finishing. Commit-message language, formatting, and Git operations remain governed by the project's applicable Git instructions.
