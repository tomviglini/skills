---
name: legal-compliance
description: "Check in good faith what a repository publishes for legal issues, at a depth that fits the moment, and keep a record so later checks only look at what changed: its license and third-party notices, names and other companies' trademarks, the terms of products it works with, privacy, store policies, and what it exposes before becoming public. Use when naming a new project, when closing a task whose change touches those areas, before a release, and before making a repository public, submitting to a store, or charging; it is not legal advice."
---

# Legal Compliance

Check that what a repository publishes respects other people's rights, the licenses and terms it depends on, and its users, and record each check so the next one only looks at what changed. This is a careful check against primary sources, not legal advice: never state that something is legal. State what a source says, what the project does, and how they compare.

## When and how deep

| Moment | Depth |
| --- | --- |
| Naming a new project or product | Recommend a name as [Names](#names) describes, before the owner chooses. |
| Closing a task | Only when the change touches an area below, such as a new dependency or asset, network access, stored data, a permission, or another company's name or material: check that area. Otherwise nothing. |
| Before a release | Short: the changes since the last check, the files the release ships, the licenses and notices included in them, and whether the disclaimer and privacy texts are still true to the code. |
| Before making a repository public, submitting to a store, or charging | Complete: every area below, with the primary sources read at that moment and the whole Git history. |

Reuse the record. A finding, decision, or source stays settled until a change, the moment, or the source itself affects it. Read sources again only in a complete check or when the change relates to them.

## Areas

- **Licenses:** a license file at the repository root, MIT unless the owner chooses another, with the year and the copyright holder. Every dependency, font, icon, or other asset that ships allows that use, and the notices its license requires travel in every package that contains it, such as a third-party notices file next to the license. An app with its own interface also shows them inside the app: a Licenses entry in its settings or About, not on the main screens, that shows the product's license and every third-party notice of what that build contains, on a page of its own or by opening the installed notices file. Every surface users run gets one, such as the desktop window, the Android app and a web view, so that a mention a license asks to see in the program, such as FFmpeg's in an About box, is there. Installed programs also keep the license and the notices file next to the executable.
- **Names and images:** names follow [Names](#names). Icons, screenshots, and store images show no other company's logo or product design, and no third-party content such as frames of a film.
- **Disclaimer:** when the product works with another company's product, brand, or service, the README, the release notes, and any store listing say that the project is independent and not affiliated with or endorsed by that company; that its trademarks are its own and are named only to say what the product works with; what the product does not do when it matters, such as bypassing DRM or logins; and that it is provided as is, as the license states. State what the code shows. Avoid broad claims such as "this infringes nothing", and do not say a review found it legal.
- **Terms of what it works with:** the product does not circumvent DRM, output protection, region checks, logins, subscriptions, or payments, and does not download or record protected content. When it changes another company's software in a way that company's terms forbid, the disclaimer says so. Of a protocol learned by studying another company's app or firmware, the repository keeps only the facts needed to interoperate, in its own words, never that company's code, assets, or binaries.
- **Privacy:** what the product reads, stores, and sends, taken from the code's actual network and storage calls. When it handles user data, or a store requires it, a privacy policy says exactly that and changes in the same change as the code. How the code keeps that data minimal and out of logs, requests, and exports is the practice of privacy-by-design, when the project installs it.
- **Store policies:** before a submission, the store's current policies, the permissions requested, and the disclosures it requires.
- **Exposure before going public:** secrets and tokens; personal data such as real addresses on a home network, emails, names, or account identifiers; another company's material; local dumps and captures. Check the whole Git history, not only the current files: deleting a file does not remove it from history.

## Names

- When naming a new project or product, recommend a name of its own, with another company's brand only after it, in a phrase that says what it works with, such as "Player Tweaks for Apple TV". Model names are trademarks too. Say why a brand in or at the start of the name is a risk, then let the owner choose.
- Once the owner has chosen, record the decision once: the name, what it refers to, how the product is distributed, and what would reopen it, such as a store submission, charging for it, or a request from the brand's owner. Do not raise it again unless one of those happens.
- Any other risk the owner decides to take works the same way: record it with its reason and the condition that reopens it.
- Write decisions as facts and reasons, for example "the name says which speakers it works with; free, distributed only through this repository's releases". Do not phrase them as admissions of a fault.

## Ground rules

- Base each requirement on a primary source read during the check: quote the sentence relied on, with its address, the date it was read, and the source's own update date when it shows one.
- Base what the project does on the code and the shipped files, not on its documentation: cite files and lines, or the command run and its output.
- Separate facts, interpretation, and recommendations. When in doubt, take the more conservative reading and mark the item for the owner.
- Propose changes to names, code, assets, and texts with their exact wording, and make them only when the owner agrees. Updating the record is part of the check.
- Suggest a lawyer's advice before charging for a product that works with another company's product, when a company objects, or when a store rejects the product on legal grounds.

## Record

Keep one record per repository, such as `docs/compliance.md`, in the repository's language:

1. **Status:** one row per area that applies, with its state and a one-line summary.
2. **Decisions:** what the owner chose and why, with the condition that would reopen each one.
3. **Open items:** what is still to do, and the moment it must be done by, such as "before going public".
4. **Sources:** each source used, with the date it was read and its version.
5. **Review log,** newest first. A check with no findings is one line: date, version or commit, depth, and reviewers. Otherwise list the scope, each finding with its evidence and the action taken or proposed, and what was not checked. The reviewers are the owner and the AI tool and model that helped.

The project's own rules, such as what its product must never do, stay in its `AGENTS.md`.

## Report

Summarize for the owner, most important first: what blocks the release or publication, the proposed changes with their exact wording, and the questions only the owner or a lawyer can answer.
