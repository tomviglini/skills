# Logs

Read this reference when adding or changing what an installed app writes to its logs, or how it keeps them. What a log line may contain (no secrets, the user's profile folder as a placeholder, no file contents) is the subject of privacy-by-design when the project installs it; this reference covers the files themselves. Reviewed on 2026-10-26; the example uses only Rust's standard library.

## What to log

| Log | When | Where |
| --- | --- | --- |
| Errors | Always | `error.log` in the data folder: one line per failure of the app itself, crashes included |
| Activity | Only where it helps diagnose something concrete, such as one long job or one device session | One file per job or session in a `logs\` folder |
| Debug | Never in release builds | A development build's own output |

- **Crashes:** install the language's crash hook before anything else runs, such as a panic hook in Rust (it also runs with `panic = "abort"`, before the process ends) or an uncaught-exception handler in Node.js, and write the crash to `error.log` before the default handling.
- **Repeats:** write a failure that repeats once, then again only when the outcome changes, for example a device that stops answering and later answers with a different error. Keeping the messages already written during this run is enough; a restart may write them again.
- **Name:** new apps call the file `error.log`. An existing app can keep its older name.

## Format

One line per entry: the UTC time, the context, and the message, for example `[2026-10-26 04:59:00 UTC] Checking for updates failed: timed out`. A message with several lines, such as a crash's, continues on indented lines, so every entry starts with a time. Write the text a person can act on; codes and identifiers only where they help find the cause.

## Size and rotation

- **Limit:** about 1 MiB per file. Another value needs a reason recorded in the project's decisions, for example a smaller file for a tray app that writes a line per device poll, or separate limits for one job's log and for the whole `logs\` folder.
- **Rotation, not truncation:** when an entry would take a file past its limit, rename the file to `error.1.log`, replacing the previous one, and start a new file. The pair never takes more than twice the limit, and the entries just before a failure survive. Never empty a file in place.
- **Per-job folders:** cap each file, and delete the oldest files until the folder fits its total. Names that start with the date (`2026-10-26_045900_job.log`) sort by age.

```rust
use std::io::Write;
use std::path::Path;

/// Append one entry to `error.log`, moving the file to `error.1.log` first when the entry would take it past
/// `limit`. Every failure here is ignored: logging never stops the app.
fn append_rotating(data: &Path, entry: &str, limit: u64) {
    let path = data.join("error.log");
    if std::fs::metadata(&path).is_ok_and(|m| m.len() + entry.len() as u64 > limit) {
        let _ = std::fs::rename(&path, data.join("error.1.log"));
    }
    if let Ok(mut file) = std::fs::OpenOptions::new().create(true).append(true).open(&path) {
        let _ = file.write_all(entry.as_bytes());
    }
}
```

## Writers

- **One process per file.** When a launcher and a server, or an app and a helper, both log, each writes its own file with the same rules, such as `error.log` for the app and `server-error.log` for the server. Two writers with different rotation rules lose or interleave each other's entries.
- **Logging never fails the app:** ignore errors while writing a log, never retry in a loop, and never log a failure to log.

## Install and uninstall

- Logs are **drop** items: list `error.log`, `error.1.log`, and any `logs\` folder in the item table. An update may remove them; uninstalling removes them.
- The app's installer modes, which the Setup runs while the app is closed, write their failures to `error.log` through the same code.

## Tests

- Rotation: write entries past the limit and check that `error.log` restarts with the newest entry, `error.1.log` holds the previous file, and neither is over the limit.
- Repeats: the same failure twice writes one line; a different one writes another.
- Pruning, where there are per-job folders: the folder ends within its total, and the newest files are the ones kept.
- A data folder that cannot be written: the app keeps working.
