# Legal compliance

This repository publishes Markdown instructions and YAML metadata for AI coding agents. This file records what applies to publishing them, what is still open, and every check, following [legal-compliance](../skills/legal-compliance/SKILL.md). The checks are done by the owner with an AI assistant, in good faith against the sources listed here; they are not a legal opinion.

## Status

As of 2026-10-24:

| Area | Status | Summary |
| --- | --- | --- |
| Licenses | Done | MIT [LICENSE](../LICENSE). Every file was written for this collection, including the code examples in the references; nothing is copied from third parties. |
| Names | Done | The collection's and the skills' names are descriptive. Claude Code, Codex, and the libraries, services, and products the skills mention are named only to say what a skill works with. |
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

### 2026-10-24: complete check before going public

- **Reviewers:** the owner, with Claude (Opus 5.5) in Claude Code.
- **Reviewed:** the change that adds this record, on top of `b3b8688`.
- **Scope:** every tracked file, the whole Git history (27 commits on every ref), and the license.
- **Findings and actions:**
  - The repository had no license. The MIT [LICENSE](../LICENSE) was added; its text matches the Open Source Initiative's.
  - The README named Claude Code and Codex without a disclaimer. One was added.
  - History: searched for tokens, keys, passwords, email addresses, private-network addresses, and local paths. The only private-network address, `192.168.1.20`, is an example in a settings sample. The only email address is the commit identity.
- **Not checked:** Anthropic's and OpenAI's brand guidelines, since their names appear only in plain text to say which agents the skills work with; a trademark search for the collection's generic name.
