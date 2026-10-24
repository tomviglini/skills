# Update Checks

Read this reference when adding or changing how an app finds a newer version. How its releases are published (tags, files, notes, records) is the release-publishing practice. Status: accepted design, reviewed on 2026-10-23 and extended on 2026-10-24, implemented in four consuming projects on Windows: Rust apps that read GitHub releases through WinHTTP, and apps whose local Node server shows a web UI.

## Behavior

- **App closed:** nothing runs. No service, scheduled task, or helper process checks for updates.
- **Settings:** automatic checks are on by default, at an interval the user picks from a short list (for example daily or weekly, weekly by default). A "Check now" button works even with automatic checks off. Next to these settings, show the time of the last check and what it found or what is missing.
- **Check now:** shows that the check is under way and then its result where the user asked for it. When the settings live in a menu that closes, a small dialog shows both. Automatic checks show nothing but the indicator.
- **Timing:** look whether a check is due shortly after start-up (the network may not be up yet when the OS starts the app), then about hourly. Store the time of the last check that read the releases with the app's state. A failed check moves nothing and is retried at the next look; its message appears only in the settings.
- **Newest version:** the highest stable `vX.Y.Z` tag above the running version, compared as numbers, never as text. Read every page of the release list; ignore drafts, pre-releases, and other tags. Do not rely on a host's "latest" mark: on GitHub it follows the last release published by default, so a fix for an older line can carry it. Major, minor, and patch updates get the same treatment unless the project defines release channels.
- **Indicator:** a small, clickable note in the main window, such as a download icon with "Version 1.4.0 available". Never a pop-up, and never installed on its own. Show the running version somewhere in the window too. Keep these texts short: the version found and how to get it.
- **Where:** only the installed desktop app shows update notices and settings, on the computer where it runs. Clients that update another way, such as mobile apps or pages opened from other devices, show none.
- **Installing:** the note opens the installer's download address in the default browser. The user opens the downloaded file, and the installer closes the running app as in [installer flow](installer-flow.md). Open only addresses on the release host, whatever a response contains. An unsigned installer downloaded this way meets the OS's and the browser's reputation warnings; code signing, or an in-app download checked against the release's published SHA-256, avoids them at a higher cost.

## Release location

- Read the project's own release list, for GitHub `GET /repos/{owner}/{repo}/releases?per_page=100&page=N` until a page comes back short. Send a User-Agent. Unauthenticated calls allow 60 requests per hour per IP address.
- A hand-edited settings key may name another repository and say whether it is private. Leave it commented in the template, so a default changed by a later build still applies.
- **Public repository:** no credentials.
- **Private repository:** never embed a token in the app or the installer; anyone can extract it. A person with access writes their own read-only token in a secrets file in the data folder, separate from the settings. On GitHub that is a fine-grained token limited to the repository with Contents read; there is no releases-only permission, so it can read the code too. A private repository without a token makes no request, and the settings say what is missing. A rejected token (401 or 403) and a repository the request cannot see (404) get their own messages.
- To keep a token from reading the code, publish into a separate releases-only repository. The pipeline then needs its own write token for it, because a CI token writes only to its own repository.

## What the app relies on in a release

- The installer has a fixed name prefix, so the app can pick it among the release's files.
- Release tags are `vX.Y.Z`, and the host's "latest" mark is not trusted (see Newest version above).
