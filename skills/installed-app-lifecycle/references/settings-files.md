# Settings Files

Read this reference when creating or changing a settings file, a state file, or a file that holds a user list.

## Format

| Content | Format |
| --- | --- |
| The user's settings, and lists a person reads or edits (devices, scenes, cameras, favorites) | TOML |
| What the app records for itself (an active port, a detection result) | TOML, in a separate `state.toml` |
| Traffic between processes, and large or machine-only data | JSON, a log format, or a database with its own migrations |

Avoid INI for new files: it has no lists or nesting, so each project invents its own conventions. Do not use YAML for settings.

## Rules

- **Keys:** kebab-case (`preferred-port`), grouped in sections. At most two levels; data that needs deeper trees belongs to the machine-only category.
- **Missing keys:** a missing key means its default. TOML has no null; do not use an empty string as one, because an empty string can be a real value. Express a deliberate "none" with an explicit value, such as `mode = "off"`.
- **Reading:** read key by key. An invalid value falls back to that key's default only; the rest of the file stays in effect.
- **Saving:** change only the values the app owns. Keep comments, layout, and keys this version does not know, so going back to an older version loses nothing.
- **Key names:** never reuse a key with a new meaning or unit; add a new name instead. Renaming or converting a key is a migration and raises the item's version.
- **New options:** one default, decided when the feature is built, applies to new installs and updates alike. The file is not rewritten just because a key is missing.
- **First run:** create the file from a template that documents each key in a trailing comment.
- **Hand edits:** if the file cannot be parsed at all, keep a copy in `backup\` before starting from the defaults.

## Example

```toml
# Example App settings.
# Unknown keys are kept; an invalid value falls back to its default.

[appearance]
theme = "system"        # system | day | night

[server]
preferred-port = 4567
allow-lan = true

[[devices]]
name = "Ceiling Light"
host = "192.168.1.20"
port = 55443
supports = ["toggle", "set_bright"]
```

## Libraries

- **Rust:** `toml_edit` reads and writes while preserving comments, layout, and unknown keys. API target 0.25 (0.25.17 used), with `default-features = false` and the `parse` and `display` features. Executed in one consuming project, reviewed on 2026-10-23. Replace a value through its existing item and keep the old value's decoration, so its trailing comment survives.
- **JavaScript or TypeScript:** most TOML libraries rewrite the whole file and drop comments. When such a process owns a file, regenerate the standard header comments on each save and document that hand-written comments there are not kept; the data is.

Adapt to the project's resolved versions instead of upgrading to match these targets.
