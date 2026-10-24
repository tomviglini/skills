# Installer Flow

Read this reference when implementing the plan, warning, apply, start-up, or uninstall steps.

## Item table

Declare the items once, in the app's code:

```rust
pub enum Rule { Migrate, Ask, Drop }

pub struct Item {
    pub id: &'static str,
    /// Current name first; each older name with the version that used it.
    pub paths: &'static [(&'static str, u32)],
    pub rule: Rule,
    pub version: u32,
    /// The oldest version the migration code converts.
    pub oldest: u32,
    /// What a reset costs the user, for the warning.
    pub loss: &'static str,
}

pub const EPOCH: u32 = 1;
pub const ITEMS: &[Item] = &[
    Item { id: "settings", paths: &[("settings.toml", 2), ("config.txt", 1)], rule: Rule::Migrate,
           version: 2, oldest: 1, loss: "Your settings go back to their defaults" },
    Item { id: "error-log", paths: &[("error.log", 1)], rule: Rule::Drop,
           version: 1, oldest: 1, loss: "" },
];
```

Each migration is a function from one version to the next. It reports what it would drop, such as unknown keys of an old format, so a lossy migration also asks first.

## Decision per item

| Found | Action |
| --- | --- |
| Not present | Keep (nothing to do) |
| Recorded epoch differs from the build's | Reset (asking unless the rule is drop) |
| Same version under its current name | Keep, byte for byte |
| Newer than the build knows (a downgrade) | Reset (asking unless the rule is drop) |
| Older, at least `oldest`, rule migrate | Migrate; ask first if the migration drops something |
| Anything else | Reset (asking unless the rule is drop) |

Under its current name an item's version is the recorded one, defaulting to that name's version. Under an older name it is that name's version, whatever is recorded.

## versions.toml

```toml
# Format version of each file and folder here, written by Setup and the app.
epoch = 1

[versions]
settings = 2
error-log = 1
```

After applying, record the build's version for every item handled, including absent ones. Keep the recorded version, and the old epoch, for anything the user did not agree to reset, so the next install asks again. Rewrite the file whole; it is not meant for editing.

## Commands the installer runs

The new build exposes two modes. They start no window, take no single-instance lock, and never touch devices:

- **`--data-plan <data> <out>`** writes the backup checkbox label on the first line (for example `Keep a copy in data\backup (293 MB)`) and one line per loss after it. It exits 2 when the user must agree, 0 when nothing would be lost, and 1 on failure.
- **`--data-apply <data> [--accept] [--backup]`** applies every step that loses nothing. Steps that lose something run only with `--accept`, after copying what they remove to `backup\<UTC time>\` when `--backup` is given. It exits 0 on success and 1 on failure, logging the cause.

The installer runs the plan from a temporary copy of the new executable before installing (the target folder is known by then). It shows the warning only on exit code 2, and runs the apply mode from the installed executable after copying the files. Without the warning page, for instance in a silent run, it never passes `--accept`.

## Start-up

Before reading any data, the app runs the same plan and applies only what loses nothing. Data that needs consent stays as it is, and the app works with what it can read. This covers copies run without the installer and data restored by hand.

## Start with the OS

Keep no copy of the choice in the data folder: the OS's entries decide whether the app starts, a copy can disagree with them, and on Windows they are per user while the data folder is shared by every account.

On Windows, two places decide it, and the app and the installer read and write both:

| Entry | Meaning |
| --- | --- |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`, a string value with the app's command line | The app is registered to start |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\StartupApproved\Run`, a binary value with the same name | How Startup apps (Settings, Task Manager) set it: an odd first byte means turned off; Windows writes `02` and eleven zero bytes for on. Missing means on |

- **Read:** on when the Run value exists and its mark is missing or has an even first byte.
- **Turn on:** write the Run value and the `02` mark, so an earlier "off" from Startup apps does not keep it from starting.
- **Turn off and uninstall:** delete both values.

This mark layout comes from community documentation and observed registry values; Microsoft does not document it. Reviewed on 2026-10-23.

## Running copies

Detect a running copy by its window class or process name, and include helper processes such as a local server or browser processes. Close it without asking, since installing, updating, or removing the app implies it. Close it through the app's own exit path first, so tray icons disappear, then force-close what remains. When the app has no window to message, give it a named exit event that the new build can signal, and keep the force-close for versions without it.

## Uninstall

Remove the files, shortcuts, start-up entry, and registration. In the data folder, delete every entry except `backup\`. When `backup\` holds copies, ask whether to delete them too, defaulting to keeping them. Remove the empty folders that remain.
