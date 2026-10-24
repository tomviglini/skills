---
name: repository-language
description: "Write repository content in the one language the project declares, from code and documentation to interface text, release notes, and commit messages. Use when writing, editing, or reviewing repository content, or when a repository has not declared its language yet; conversational replies and reading files for context do not trigger it."
---

# Repository Language

Each repository declares one language, and what is written in it uses that language. The language of a request does not change it. Conversation follows the user's own language separately.

Apply this to content being written, changed, or reviewed. Reading files for context or answering a question does not need a language check.

## Where the language is declared

- The root `AGENTS.md` has a language section whose first line names it, such as `Repository language: English.`, or `Idioma del repositorio: español.` in a file written in Spanish. Codex reads that file directly, and Claude Code reads it through a `CLAUDE.md` that imports it.
- One language covers everything listed below. An area that needs another, such as a website published in two languages, gets its own line in that section with its scope, for example `Website content: English and Spanish.` Add one only when the owner chooses it.
- When a repository declares no language, ask the owner and add the section before writing content. Existing files can suggest an answer, but they may mix languages by accident.

## Rules

- Write in the declared language: identifiers, comments, documentation, interface text, logs and error messages, tests, scripts, configuration descriptions, release notes, commit messages, and the names of new files, branches, and tags.
- Write new or replacement text in that language even when the request uses another. Do not copy prose from the request in its original language.
- Keep proper names, product names, identifiers defined elsewhere (an API, a protocol field, another site's markup), and text whose exact form matters, such as a saved key that users' data depends on. Write the explanations and labels around them in the declared language.
- Keep changes within the task. Do not translate unrelated content or rename existing public identifiers, branches, or tags to normalize their language unless the owner asks for that migration.
- The format of commit messages, such as their capitalization, belongs to the project's Git rules.

Honor the recorded areas and the choices the owner already made. If a rule still conflicts with the project's instructions, show both and ask whether to change the declaration or add an area line before changing the affected content.

## Review

Before finishing, check the changed identifiers and text for unintended additions in another language. Reuse the declaration and its area lines; revisit them only when a new conflict appears or the owner asks.
