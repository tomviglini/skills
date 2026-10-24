---
name: installed-app-lifecycle
description: "Install, update, and uninstall a desktop app while keeping its saved data across versions: per-item format versions, migrations, consent and a backup before any loss, TOML settings, opt-in start with the OS, and update checks against published releases. Use when changing an installer or uninstaller, adding or changing anything the app keeps between runs, adding or changing update checks, or preparing a release that changes saved data; excludes web deployments and app-store mobile releases."
---

# Installed App Lifecycle

Make every release state what it does to the user's saved data, so any build installs with confidence. Data that still works stays untouched. What changed is migrated or reset one file or folder at a time. Nothing the user would miss is lost without their agreement.

Preserve the project's installer tool, languages, data location, and recorded decisions. For a review or explanation, inspect the affected items and flows; change files only when implementing.

## Inventory the data folder

- Give each file or folder the app keeps one role and list it as an **item**. Each item has an id, its paths (current name first, older names with the version that used them), a format version, the oldest version it migrates from, and a rule for a version the build cannot use:
  - **migrate:** the user's settings or content. Convert it; reset it only after the user agrees.
  - **ask:** something the user would miss but cannot be converted, such as browser sign-ins. Reset it only after the user agrees.
  - **drop:** something the app rebuilds by itself (caches, detection results, temporary files, logs). Remove it without asking.
- Keep roles apart: `settings.toml` for the user's choices, `state.toml` for what the app records (an active port, a detection result), one file per user list (devices, scenes, cameras), and separate folders for third-party stores.
- Reserve `backup\` for copies the user chose to keep; no update or reset touches it. Write every file through a `.tmp\` folder inside the data folder, then rename it into place.
- Do not version what another component owns, such as a browser engine's own profile and cache formats. Version only what the project's code defines. Let the app repair such stores at run time, for example by clearing the cache folders when the engine fails to start, never the sign-ins.

## Version items, not the whole folder

- Record each item's version and an `epoch` in `versions.toml` in the data folder. Without that file (data from before it existed), an item counts at the version of the name it is found under.
- Raise an item's version only for a change older files cannot follow, and write its migration. A new key with a default is not such a change. Raise the epoch only to reset every item after a redesign; the same consent and backup rules apply.
- Keep a sample of every released version of each migrate item in the repository, and test that each one upgrades to the expected result. Before a release, check whether saved data changed, which item it affects, and whether the version, migration, and sample are all in place.

## Write settings files

Read [settings files](references/settings-files.md) when creating or changing a settings or list file. In short:
- Use TOML for anything a person may read or edit, with kebab-case keys and at most two levels.
- A missing key means its default; there is no null. An invalid value resets only its own key.
- Unknown keys and comments survive saving. A key name is never reused with a new meaning.
- A new option has one default, the same for a new install and an update. A dependency on another setting can still keep it inactive.

## Install, update, and uninstall

1. **Running app:** detect a running copy and its helper processes and close it without asking; installing, updating, or removing the app implies it.
2. **Plan:** the new build reports per item whether it keeps, migrates, or resets it.
3. **Warning:** when something the user would miss is lost, list it, offer a backup copy with its size, and let Cancel keep the installed version. Offer no silent path that resets user data.
4. **Apply:** after the files are installed, the new build applies the plan, performing the resets the user agreed to. At start-up the app itself applies only what loses nothing, so copies run without the installer still upgrade.
5. **Start with the OS:** an installer option, unchecked for a new install and showing the current choice on an update. The app's settings change it later. The OS's own entries are the only record: read them each time instead of saving a copy, and on Windows read both the Run value and the Startup apps mark.
6. **Size:** report the installed files plus a bounded data estimate where the OS lists installed apps.
7. **Uninstall:** close a running copy the same way, remove everything except `backup\`, and ask whether to delete the copies when there are any.

The migration logic lives in the app's own code, where it is tested; the installer only shows the plan and asks. Read [installer flow](references/installer-flow.md) when implementing these steps, and [NSIS](references/nsis.md) for an NSIS installer.

## Check for updates

Read [update checks](references/update-checks.md) when adding or changing update checks. Nothing runs while the app is closed. A running app checks its release location at an interval the user can change (on and weekly by default) and shows when it last checked. The newest version is the highest stable version number above the running one, not the host's "latest" mark. It appears as a small clickable note that opens the installer's download, only in the installed desktop app, and the running version is always visible. Never install automatically, and never embed credentials: a private release location uses a read-only token the user writes in the data folder.

## Verify

After changing an item, a migration, or the installer, run the sample upgrade tests. Exercise the affected installer paths when possible: a new install, an update over the previous release with and without data loss, a downgrade warning, and an uninstall with and without backups. Report which paths ran on a real system and which were only built.
