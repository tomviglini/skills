# Legal compliance

This repository publishes Markdown instructions and YAML metadata for AI coding agents. This file records what applies to publishing them, what is still open, and every check, following [legal-compliance](../skills/legal-compliance/SKILL.md). The checks are done by the owner with an AI assistant, in good faith against the sources listed here; they are not a legal opinion.

## Status

As of 2026-10-26:

| Area | Status | Summary |
| --- | --- | --- |
| Licenses | Done | MIT [LICENSE](../LICENSE). Every file was written for this collection, including the code examples in the references; nothing is copied from third parties. |
| Names | Done | The collection's and the skills' names are descriptive. Claude Code, Codex, and the libraries, services, and products the skills mention are named only to say what a skill works with, or, in [desktop-ui-design](../skills/desktop-ui-design/references/proportions.md) and [privacy-by-design](../skills/privacy-by-design/references/sources.md), to cite the public guidance they summarize. |
| Disclaimer | Done | The [README](../README.md#disclaimer-and-license) states that the collection is independent of Anthropic and OpenAI and that the names belong to their owners. |
| Exposure | Done | The whole history has no secrets, tokens, or personal data. Commits carry the owner's name and address, as in the owner's other public repositories. |
| Terms, privacy, store policies | Not applicable | Nothing here runs, collects data, or is sold through a store. |

## Decisions

None yet.

## Open items

None.

## Sources

| Source | Read | Version |
| --- | --- | --- |
| [The MIT License, Open Source Initiative](https://opensource.org/license/mit) | 2026-10-24 | SPDX `MIT`; the page shows no update date |

## Review log

### 2026-10-28: mobile-ui-design, logs, pipeline speed and commit boundaries

- **Reviewers:** the owner, with Claude (Opus 5.5) in Claude Code.
- **Reviewed:** the change that renames `mobile-ui-basics` to `mobile-ui-design` and adds its Android, proportions, patterns, and verification references; the Logs section of installed-app-lifecycle; the pipeline-speed reference of release-publishing; the Commit boundaries section of git-commit-rules; and legal-compliance's Licenses wording.
- **Scope:** those files, their README entries, and the sources they cite.
- **Findings and actions:**
  - mobile-ui-design cites public guidance (Android's developer pages and core app quality guidelines, the Material 3 tokens published with Compose, Apple's Human Interface Guidelines, and WCAG 2.2), summarized in the skill's own words and tables with a link and review date; no text, image, or code is copied. Its patterns name no other company's apps.
  - pipeline-speed names GitHub and its documentation only as the host the examples work with, without prices; its examples come from the owner's own release runs, written generically.
  - The Logs section and the Commit boundaries section are the owner's own standards and cite nothing.
- **Not checked:** the cited publishers' brand guidelines, since their names appear only in plain text as sources.

### 2026-10-26: desktop-ui-design and privacy-by-design

- **Reviewers:** the owner, with Claude (Opus 5.5) in Claude Code.
- **Reviewed:** the change that renames `desktop-ui-basics` to `desktop-ui-design` and adds its proportions and verification references, and the change that adds `privacy-by-design` and adopts it here.
- **Scope:** both skills' files, their README entries, and the sources they cite.
- **Findings and actions:**
  - privacy-by-design cites public guidance in [its sources](../skills/privacy-by-design/references/sources.md) (GDPR Articles 5 and 25, the Privacy by Design principles, Mozilla's Lean Data Practices, two OWASP cheat sheets, MITRE CWE-532, Android's security tips, and RFCs 5737, 7042, and 2606), summarized in the skill's own words with a link and review date; no text is copied. It is not legal advice, as the skill says.
  - The proportions reference cites public design guidelines and documentation (Microsoft's Windows and Fluent 2 guidelines, shadcn/ui's components, WCAG 2.2, web.dev, Nielsen Norman Group, Atlassian, Vercel's Web Interface Guidelines, and the book Refactoring UI). Their values are summarized in the skill's own words and tables, with a link and review date for each source; no text, image, or code is copied. The names are used only to say where a value comes from.
- **Not checked:** the cited publishers' brand guidelines, since their names appear only in plain text as sources.

### 2026-10-24: complete check before going public

- **Reviewers:** the owner, with Claude (Opus 5.5) in Claude Code.
- **Reviewed:** the change that adds this record, on top of `b3b8688`.
- **Scope:** every tracked file, the whole Git history (27 commits on every ref), and the license.
- **Findings and actions:**
  - The repository had no license. The MIT [LICENSE](../LICENSE) was added; its text matches the Open Source Initiative's.
  - The README named Claude Code and Codex without a disclaimer. One was added.
  - History: searched for tokens, keys, passwords, email addresses, private-network addresses, and local paths. The only private-network address, `192.168.1.20`, is an example in a settings sample. The only email address is the commit identity.
- **Not checked:** Anthropic's and OpenAI's brand guidelines, since their names appear only in plain text to say which agents the skills work with; a trademark search for the collection's generic name.
