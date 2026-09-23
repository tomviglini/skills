# Git Identity Checks

CLI baseline: **Git 2.34.1**. Documentation reviewed: **2026-09-23**. The read-only commands below were exercised with that Git version in temporary repositories covering missing configuration, explicit configuration, role-specific settings, and environment overrides. No commit or push was needed for those checks.

## Resolve configuration and effective identities

Run these commands in the target repository, using the same environment as the intended commit:

```bash
git config --show-origin --show-scope --get-regexp '^(user|author|committer)\.(name|email)$'
git -c user.useConfigOnly=true var GIT_AUTHOR_IDENT
git -c user.useConfigOnly=true var GIT_COMMITTER_IDENT
```

Inspect each result independently. The config query exits with status 1 when no matching keys exist; explicit identity environment variables may still provide the values. The two `git var` commands must resolve successfully. The command-scoped `user.useConfigOnly=true` disables guessed defaults without modifying any configuration file.

The config output shows origins and scopes. `git var` shows the effective identity after applicable overrides. Its trailing timestamp and timezone are not part of the name or email. Inspect only identity-related environment variables if an override needs explaining; do not dump the entire environment or Git configuration.

Sources: [git config identity settings](https://git-scm.com/docs/git-config#Documentation/git-config.txt-username), [user.useConfigOnly](https://git-scm.com/docs/git-config#Documentation/git-config.txt-useruseConfigOnly), and [git var](https://git-scm.com/docs/git-var).

## Assess whether the identity is intentional

Git accepting an identity establishes that it can record it; it does not establish that the identity is appropriate. Check both resolved names and email addresses for:

- Missing or empty values, template placeholders, and obvious dummy addresses.
- Default operating-system, container, or assistant identities used without an explicit designation from the user or repository policy.
- An unexpected environment override that replaces a configured personal identity with a generic one.

Do not use a rigid full-name rule or require a particular email provider. Personal handles, single names, Unicode names, and account-specific privacy/no-reply addresses can be valid. Do not require public personal details or proof of email ownership. A known, deliberately designated automation account can also be valid.

These are review criteria, not an automatic identity-verification algorithm. A syntactically valid placeholder still requires correction; a legitimate unfamiliar name does not itself justify blocking a commit. Ask only when missing information or a concrete ambiguity prevents establishing the intended attribution.

## Operations that preserve an existing author

`git var GIT_AUTHOR_IDENT` resolves the default author for a new commit. It does not identify every author an amend, cherry-pick, rebase, or author-reuse option will preserve. For such an authorized operation, also inspect the raw author of the source commit and account for the intended command's options. Check the actual committer separately.

Do not add `--author` or `--reset-author` merely to conceal configuration problems or make author and committer match. Preserve the original author unless the user explicitly requests correcting or replacing that attribution. See [git commit](https://git-scm.com/docs/git-commit).
