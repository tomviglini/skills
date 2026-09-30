---
name: checksum-named-artifacts
description: "Name installable build artifacts, such as APKs, app bundles, and installers, with a SHA-256 content prefix and print the full digest. Use when creating, changing, or reviewing the commands that build development or release artifacts for manual installation; excludes web asset fingerprinting and product versioning."
---

# Checksum-Named Artifacts

Give every installable build artifact a filename derived from its own bytes, so any copy can be identified and verified without repository state. Reuse the project's build scripts, tools, and output conventions.

## Scope

- Apply to artifacts that people install, sideload, or hand over manually: Android APKs and app bundles, development clients, desktop installers, and similar packages. Cover both development and release variants the project builds this way.
- Do not apply to web asset URLs, intermediate build files, source archives, or store uploads that a distribution service renames. Content names do not replace version codes, signing, or release notes.
- For a review, check the naming contract and its integration. For implementation, change the project's existing build entry points; do not add a parallel manual step.

## Naming contract

- Build first, then compute SHA-256 over the final artifact bytes, after signing, alignment, and any other post-processing.
- Name the copy `<project>-<variant>[-<target>]-<hash>.<extension>`, where `<hash>` is the first eight lowercase hexadecimal characters of that digest. Use the project's own variant names, such as `debug`, `dev`, `release`, or `setup`. Add a target, such as `arm64`, when the build is limited to one architecture or platform. Examples: `example-release-arm64-18e782c2.apk`, `example-setup-4c0e9a1f.exe`.
- The suffix identifies content, not the Git commit or the product version. Identical bytes produce the same name; any packaging change produces a different name, even when the version is unchanged. Do not claim reproducible builds because of it.
- Copy the artifact rather than renaming it. Leave the build tool's conventional output, such as `app-release.apk`, in place for tools that expect it; both files contain the same bytes.
- Print the named path and the complete SHA-256 when the build finishes.
- Name only an artifact produced by the current run. Stop on build failure instead of copying a stale file left by an earlier build.

## Location and retention

- Keep named copies where the project's documented shortcuts and instructions expect them.
- If a clean step, native-project regeneration, or another variant's build deletes the conventional output directory, write the named copies to a stable directory outside it, so development and release artifacts can coexist.
- Keep artifacts out of version control unless the project explicitly requires otherwise. Do not delete older named copies automatically unless the project defines a retention rule.

## Integrate and document

- Implement naming inside each covered build command, using the project's existing language and tooling. A Node.js example is in [build script example](references/build-script-example.md); read it only when implementing in a Node-based build.
- Record project facts in its build documentation: commands, covered variants, output directory, and filename pattern. Link this skill for the general contract instead of repeating it.

## Verify

After an implementation change, run the affected build commands, or explain why a build cannot run in the environment. Check that:

| Check | Evidence |
| --- | --- |
| Name | The file matches the pattern for its variant and target. |
| Digest | An independent SHA-256 of the named file matches the printed digest and the suffix. |
| Identity | The named copy is byte-identical to the conventional output of the same run. |
| Retention | Building another covered variant, including any regeneration step, leaves earlier named copies in place. |

Report the named files and digests, and any variant that was not built.
