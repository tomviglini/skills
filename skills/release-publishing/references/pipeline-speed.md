# Pipeline Speed

Read this reference when a release run is slow, or before adding caching, prebuilt tools, or another runner to a pipeline. The goal is a shorter run within the host's limits on runner time. The examples come from real GitHub Actions runs of a desktop app with a Rust build, a Windows installer, and a media tool, described without the project; GitHub details reviewed on 2026-10-26.

## Measure before changing

- Read the finished run's job logs, not the run's total. Every log line carries a time: the gaps between lines show where a step waited, and the step list shows each step's duration. On GitHub, the jobs API gives each step's start and end while the run is still going; the full log can be downloaded only after the job ends.
- Split each slow step into its parts: download (size and speed), install, compile (which crates or packages), tests, packaging. A download speed far below the rest of the run points at a mirror, not at the work.
- Compare with the same command on the development machine, with as many parallel jobs as the runner has cores, so the difference between the runner and the work itself is known. Note the runner's cores and memory from the log.
- Record the measured times with the date and the runner in the project's decisions or build guide, so the next change has a baseline.

## Limits

- Hosted runner time is limited, typically a monthly allowance shared by every private repository of the account, and Windows and macOS runners use it faster than Linux ones. Run on Windows or macOS only what needs that system: building and testing what ships there. Checks that run the same on Linux go to Linux.
- Parallel jobs shorten the wait but each adds its own setup time; split a job only when the wait matters more than the allowance.

## Do by default

These make a run faster without trading trust, cleanliness, or the product, so apply them when the measurements show the need, and say what changed:

- **Remove repeated work.** Count the builds a job makes: each profile, target, or feature set compiles the dependency tree again. A test that builds the product a second time only to inspect it can usually inspect the build that ships instead.
- **Install only what is missing.** Check for a tool the runner image already has before installing it from a package mirror, whose speed varies from run to run.
- **Keep small generated inputs in the repository.** A test that generates its input files with a large tool on every run can keep those files as fixtures and generate them again only when one is missing. The fixtures must be made up and free of third-party content.
- **Run a test where its real dependency is.** A test that needs a tool the product ships can run on the job that has that pinned tool, instead of installing a distribution's copy elsewhere; it then also tests what ships.
- **Pin downloads by version and checksum** from sources that keep old versions; a link to a source that deletes old builds breaks later.

## Ask the maintainer first

These trade something for speed. Present the measured gain, the cost, and the risk, and let the maintainer decide; record the decision either way.

| Option | Gain | Cost and risk |
| --- | --- | --- |
| Cache between runs (package or build caches) | Skips downloads and compiling unchanged dependencies | Uses the repository's cache storage; a stale or tampered cache can reach a release; a release built from a cache is not a clean build |
| Prebuilt binaries of tools instead of compiling them | Minutes per tool | Trusting someone else's build, even when checked by checksum |
| A prebuilt runner image or container | Fastest setup | Another workflow and image to keep updated |
| Self-hosted runner, such as the developer's PC | Everything already installed, no allowance used | Depends on that machine being on; lets the CI host run code on it |
| Larger or paid runners | Shorter runs | Money |
| Lower release optimization, such as less link-time optimization | Shorter builds | Changes the product's speed or size |

## Examples

- **Slow mirror:** a Linux job installed a distribution's 94 MB media tool for one test. The package index downloaded at 7.6 MB/s, then the packages at 85 kB/s, and that step took 18 of the job's 29 minutes. The test's three synthetic clips (428 KB) became fixtures, and the test moved to the Windows job with the tool the installer ships. The Linux job went to 10 minutes, the Windows job grew by 2.
- **Repeated build:** a Windows test built the whole app in another profile only to read one field of its executable header, 16 minutes. The installer script now reads that field from the executable it ships. The Windows job went from 56 to 42 minutes.
- **Kept as is:** compiling a license checker from source took 2.5 minutes. A prebuilt binary would take seconds, but the maintainer preferred compiling it to trusting a prebuilt one.
- **Runner versus desktop:** the same release build took about twice as long on a 2-core hosted runner as on the developer's PC with 2 parallel jobs, and turning off the PC's antivirus scanning changed nothing measurable. What remains is compiling, which only caching or more cores would shorten.
