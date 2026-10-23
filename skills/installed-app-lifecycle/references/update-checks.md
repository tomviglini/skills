# Update Checks

Read this reference when adding or changing how an app finds and installs a newer version. Status: accepted design, reviewed on 2026-10-23; not yet implemented or executed in a consuming project.

## Behavior

- **App closed:** nothing runs. No service, scheduled task, or helper process checks for updates.
- **App running:** it checks at most about once a week (store the time of the last check with the app's state), and immediately when the user presses "Check for updates". It shows when it last checked.
- **Where it checks:** a predefined location, such as a release host's "latest release" URL. It compares the published version with its own, and says nothing when no release exists or the check fails.
- **New version:** shown as a small, non-intrusive indicator in the app's window or menu. Never a pop-up, and never installed on its own.
- **Installing:** a button downloads the installer, verifies it against a published SHA-256 (or a signed manifest), and starts it. The installer then closes the running app as in [installer flow](installer-flow.md), and the data rules decide what is kept.

## Releases

- The release files must be downloadable without credentials. Never embed a token in the app: anyone can extract it.
- When the source repository is private, publish installers somewhere public, such as a separate public releases repository or the project's website.
- Publish beside each installer its version, download URL, and SHA-256, and optionally a note when the update asks to reset data.
