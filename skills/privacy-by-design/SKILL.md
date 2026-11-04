---
name: privacy-by-design
description: "Keep users' data private while building or changing an app: collect, keep, log, send, and show only what a feature needs; keep secrets and personal data out of logs, errors, crash reports, URLs, exports, and notifications; no telemetry by default; bounded retention; made-up data in tests, examples, and screenshots. Use when adding or changing logging, stored data, network calls, error or crash reporting, analytics, exports or sharing, permissions, or fixtures and screenshots; legal texts such as privacy policies and store disclosures belong to legal-compliance."
---

# Privacy by Design

Treat what an app learns about its user as theirs. A feature collects, keeps, and moves only what it needs, for as long as it needs it, and the user can tell what that is. Privacy is the default: a user who changes nothing shares nothing. These are engineering practices drawn from widely used guidance (see [sources](references/sources.md)); they are not legal advice. What a product must say publicly about its data, such as a privacy policy or a store's disclosure form, belongs to legal-compliance, which records it from what this practice leaves in the code.

## Scope the work

Apply the rules to the data a change touches: a new log line, setting, file, request, export, permission, or test fixture. For a review or explanation, report findings without changing code unless requested.

Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change.

## Know what the app handles

- **Personal and identifying data** includes names, email addresses, account and device identifiers, serial numbers, network addresses (IP and MAC addresses, Wi-Fi names), location, the user's name inside file paths, the names and contents of the user's files and media, history that reveals habits, and hardware details that fingerprint a machine. Passwords, tokens, keys, and session values are **secrets**.
- For each item a change touches, know why it is needed and where it goes: memory only, the app's data folder, logs, exports, the network, or places other apps can see, such as the clipboard, notifications, window titles, and recent-file lists.

## Collect and keep only what a feature needs

- Each stored or sent item serves a feature the user can see. Do not keep data "in case it is useful later".
- Prefer a form that identifies less: a count instead of a list, a hash or a random identifier created by the app instead of a hardware or account identifier, a rounded value instead of an exact one.
- Keep data in the app's own data location with the operating system's private defaults, never in places other apps or users can read. Secrets go in the operating system's credential store or a documented, ignored token file; never in settings that can be exported or shared.
- Bound how long and how much: logs, caches, and histories have a size or age limit, and removing an item removes its copies. Uninstalling removes the app's data, except backups the user chose to keep (see installed-app-lifecycle).

## Logs, errors, and crash reports

- Never log secrets: passwords, tokens, keys, connection strings, session values, or authorization headers. Never log the content of the user's files or messages, or precise location.
- Reduce what identifies the user: replace the user's profile folder in paths with a placeholder such as `%USERPROFILE%` or `~`, and log names, addresses, and file names only where the failure is about them.
- Error texts the user can copy or share follow the same rules as logs.
- Logs stay on the device. Release builds write no debug logging; on mobile, nothing personal goes to the system log (logcat or the console), which other apps or tools can read.
- An error or crash report leaves the device only through an action of the user, such as sending a prefilled report they can read first, and shows exactly what it contains.

## Network

- Every connection belongs to a feature that needs it. Know each one: the host, what is sent, and when.
- No analytics, telemetry, advertising, or tracking components by default. Usage data, if a project ever wants it, is opt-in, off by default, minimal, and documented.
- Use HTTPS on the internet. Bundle fonts, images, and scripts instead of loading them from third-party servers at run time, which tells those servers the user's address and what they opened.
- Requests carry only what their feature needs: an update check sends no identifiers beyond the request itself, and tokens are read-only and scoped to their purpose.
- URLs and query strings never carry secrets or personal data: they end up in logs, histories, and caches.

## Exports, sharing, and what other apps see

- An export, a shared file, or a file written next to the user's own files carries only what its purpose needs: no secrets, no machine paths, no device identifiers unless they are the point. Say what an export contains where the user creates it, and what a sidecar file reveals if it is shared.
- Notifications, window titles, the clipboard, and recent-file or jump lists show no secrets; notifications may appear on a lock screen.
- Importing restores only what the app recognizes and checks it before use.

## Permissions

- Ask for a permission only when a feature needs it, at the moment it is used, and keep working when the user says no, with a short explanation of what is missing.

## Tests, examples, screenshots, and documentation

- Use made-up data: invented names and titles, addresses from the documentation ranges (`192.0.2.x`, `198.51.100.x`, `203.0.113.x`, RFC 5737), MAC addresses from `00:00:5E:00:53:xx` (RFC 7042), emails at `example.com` (RFC 2606), and serial numbers padded with zeros. Never the user's real data, files, or captures.
- Screenshots and recordings meant for others show no real names, paths, addresses, tokens, or third-party content.

## Verify the affected data

| Changed area | Useful observations |
| --- | --- |
| Logging and errors | Run the feature, including a failure, then read the log and the error text: no secrets, and no user name, address, or file name that the diagnosis does not need. |
| Network | List each request the change makes (host, payload, when) and compare it with what the feature needs and what the project documents. |
| Stored data | Open the files the change writes: only the fields the feature needs, in the app's own location, within their limits. |
| Exports and sharing | Create one and read it as the person who will receive it. |
| Tests and screenshots | Search fixtures, examples, and pictures for real names, paths, addresses, and tokens. |

Record what the product reads, stores, and sends in the project's compliance record (legal-compliance), and keep it true to the code.
