---
name: task-lifecycle
description: "Run the changes requested in a project through one lifecycle: work with focused checks while logging what the diff will not show in .pending-docs.md, hand over for the owner to try, and when the owner approves, close the task by writing the pending documentation and running the full checks. Pipelines that build or publish what the project ships run those same checks and build scripts. Apply to implementation work in an adopting project, including creating or changing such pipelines; reading, explaining, and answering questions do not trigger it."
---

# Task Lifecycle

A task is everything the owner asks for until they close it, however many requests it spans. Documentation and the full checks run once, when the task closes, instead of after every change. Closing leaves the repository as if every change had been documented and fully checked as it was made. The project's instructions supply the concrete check commands and documentation rules; this skill decides when they run.

## Work

- Implement each request as part of the open task. A request after a close opens a new task.
- With the first change of a task, create `.pending-docs.md` at the repository root, ignored by Git, if it does not exist. Its existence means a task is open.
- Log in it, one line each and as they happen, what the diff will not show: the owner's decisions and their reasons, alternatives considered and discarded, discoveries such as platform limits or measurements, what remains unverified, and the documents expected to change. When a decision changes, add the change instead of editing the earlier line. When there is nothing of that kind, a line saying what was requested is enough.
- After each change, run the project's focused checks for the affected area. Do not update documentation or run the full checks yet.
- Hand over: say what changed, how to try it, which checks ran, and that documentation and the full checks wait for the close.

## Close

The owner closes a task by making clear they are satisfied with it; no fixed phrase is needed. "OK, but change X" is more work, not a close. When unsure, ask. Closing by mistake only brings the documentation pass forward, so close whenever the owner's satisfaction is clear. A request such as "do it and close it" runs the whole lifecycle at once, and a change with nothing to try, such as a refactor, can be proposed for closing directly.

To close:

1. Update the documentation from the task's diff and `.pending-docs.md`, following the project's documentation rules, such as its ADRs, guides, and README files. Update derived material only as the project's rules say.
2. Run the project's full checks.
3. If a check fails, tell the owner what failed. Fix it when possible, rerun the checks, and update the documentation if the fix affects it; say so if the fix changes behavior the owner already tried. If it cannot be fixed, report it and leave the task open.
4. Delete `.pending-docs.md`, then report the documents changed and the checks run. The task is ready to commit.

## Commit

A commit still needs the owner's explicit request and follows the project's Git rules. If `.pending-docs.md` exists when a commit is requested, close the task first. Otherwise the last close already documented and checked everything, so commit without repeating the checks.

## Pipelines

A pipeline that builds, publishes, or deploys what the project ships (a release workflow, a deployment, a store build) produces what users get, so it runs what a close runs locally:

- It calls the project's own scripts for the full checks and for the release build, the ones the project's instructions name, with the same pinned toolchain, shell, and options. It does not copy their commands into its own steps.
- It adds only what the runner itself needs, such as checking out the code, and what has no local counterpart, such as publishing the result and recording the outcome.
- When the runner needs a different path, change the shared script so both take it, instead of adding a pipeline-only step.
- Checks that need a device, an emulator, or other hardware stay local. The project's instructions list each such exception and its reason.

## Across sessions

Before working in a repository, check for `.pending-docs.md`. If it exists, a task is open: read it, continue that task, and carry its notes into the close.
