---
name: release-publishing
description: "Publish versioned releases from a tag: one product version, CI that runs the project's own checks and builds, files named by version, a dated title, the host's latest mark only on the highest version, notes that introduce the product and list the release's changes, a license and disclaimer, a record of each outcome, and cleanup when a tag is deleted. Use when creating or changing a release pipeline or its notes template, or when preparing, publishing, editing, or deleting a release; app-store submissions and web deployments do not trigger it."
---

# Release Publishing

Publish every release the same way, so a tag is all it takes and a reader of the release page understands the product, gets the right file, and sees what changed. Preserve the project's host, pipeline tool, and conventions; the examples use GitHub and Actions.

## Scope the work

Apply the parts a task touches: a notes change needs the notes rules, a new pipeline needs all of it. Publishing or deleting a tag, and editing a published release, happen only when the maintainer asks. For a review, report findings without changing the pipeline.

Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change.

## Version and tag

- The repository has one product version, kept in one place and read by every build. A release is a tag `vX.Y.Z` that matches it, and the pipeline refuses a tag that does not.
- Every installable file in a release carries that version in its name, without the tag's `v`, for example `example-setup-1.4.0.exe` and `example-release-arm64-1.4.0.apk`. Local builds keep their own naming.
- Several installable files, such as a desktop installer and an Android APK, are separate files of one release under that one version. A store version code derives from it so it rises with every release, for example `major × 10000 + minor × 100 + patch`.

## Pipeline

- A pushed tag runs the project's full checks and its release builds through the same scripts and pinned toolchains as local work. The pipeline adds only what the runner needs and what has no local counterpart: publishing and recording.
- One publication at a time per repository. The run's title names the release, such as "Release v1.4.0".
- The release title starts with the version and adds the date, `vX.Y.Z (YYYY-MM-DD)`, so a list of releases shows the version first.
- Publish with the pipeline's own token, then download each published file and compare it with the build.
- Mark a release as the host's latest only when it carries the highest version, so a fix to an older line does not take the mark.
- Record each outcome where the maintainer can read it without the host's web interface, such as a git note on the tagged commit: on success the release address and each file's name, SHA-256, and size; on failure the step or job that failed and the end of its output.
- Deleting the tag deletes its release, its runs, and its record. A run cannot delete itself, so the next run removes finished cleanup runs.
- To test an update path, publish a throwaway higher version from a commit that never reaches the main branch, and delete its tag afterwards.

## Notes

Keep the notes' fixed text in a template next to the pipeline, such as `.github/RELEASE_NOTES.md`, with placeholders for the version and the changes. Update it when what the product offers changes. The sections, in this order:

1. **Opening:** one sentence on what the product is and what it works with.
2. **Download and install:** which file to download from the release's assets for each platform, how to install it, and how to update. When the host adds source archives, say they are not the product.
3. **Features:** the main capabilities, four to six short bullets.
4. **Changes in this release:** the subjects of the commits since the previous release tag, filled in by the pipeline. The first release says "First release."
5. **Disclaimer:** when the product works with another company's product, brand, or service: independent and not affiliated, plus any limit a user needs to know.
6. **License:** the repository's license, and that the package includes it.

Keep every text short: what the reader gets and does, without the product's internals or reassurances users already expect. Write the notes in the repository's content language. Editing a published release's notes by hand follows the same template, filled in for that release.

## License and disclaimer

Before a repository's first release, cover the basics:

- A license file at the repository root: MIT unless the maintainer chooses another, with the year and the copyright holder. Archives and other packages that carry files include it.
- When the product works with another company's product, brand, or service, a disclaimer in the README and in the notes: an independent project, not affiliated with or endorsed by that company; its trademarks are theirs and are named only to say what the product works with; what the product does not do when it matters, such as not bypassing DRM or logins; and that it is provided as is, as the license states.
- A deeper legal review, such as a written compliance prompt an agent runs, is worth it before making a repository public or publishing in a store, and when a change adds third-party code or assets, network access, or data handling. It is not required for every release.

## Verify

| Changed area | Useful observations |
| --- | --- |
| Pipeline | A tag publishes the release; its record shows success; the release's title, files, digests, and latest mark are as expected. A deliberately failing step leaves its name and output in the record. |
| Notes template | A published release shows every section filled in, no placeholder left, the right file names for its version, and only that release's changes. |
| Deletion | Deleting the tag removes the release, its runs, and its record. |

Report what was published or changed and what was verified on the host.
