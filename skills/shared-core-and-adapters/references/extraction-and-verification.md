# Extraction and Verification

Read this when introducing or moving shared logic, changing a dependency contract across consumers, or investigating a platform dependency leak. Reuse settled boundaries for ordinary maintenance.

## Choose the smallest useful shared unit

Start with the behavior that needs consistent meaning and its actual consumers. Read the relevant implementation, tests, and accepted decisions before deciding what to extract. A candidate can be a function, a state transition, a codec, or an orchestration object; it need not become a separately published package.

| Situation | Useful boundary |
| --- | --- |
| Desktop and mobile encode the same device commands but use different socket APIs | Share protocol rules and command behavior; supply the transport separately. Keep permissions and platform connection mechanics in their adapters. |
| Browser, API, and worker derive the same time ranges or eligibility display | Share the agreed calculations and safe data contracts. Keep authoritative enforcement in the trusted backend and database access outside browser imports. |
| Two frontends use the same pending/confirmed state rules but different stores and storage | Share the transitions or interaction primitive. Let each frontend own its store, persistence, rendering subscriptions, and feedback. |
| Similar-looking operations have different product rules or incompatible lifetimes | Preserve those differences. Extract only a genuinely common subproblem when useful, rather than forcing the operations through one configurable workflow. |

Keep architecture-specific numbers, timing policies, commands, and storage choices in the consuming project. A known future consumer may justify a boundary, but do not implement speculative adapters or claim support for a platform that has not been integrated.

## Make the adapter contract usable

Define the operations the core actually needs instead of copying an entire SDK into a new interface. Specify only relevant behavior: data representation, success and failure results, cancellation or timeout semantics, and who owns resource release. Reuse suitable existing contracts.

When retries, queues, or connection recovery cross the boundary, make ownership explicit. Avoid a core and adapter independently retrying the same operation without accounting for the combined behavior. Do not introduce retries for operations whose repetition may change the result. Preserve the project's actual protocol and idempotency rules.

Allow platform capabilities to differ where the product already permits it. Represent unsupported operations explicitly rather than silently reporting success. An adapter's transport implementation can differ while still satisfying the shared operation's meaning.

## Integrate an extraction completely

- Keep the smallest coherent change that moves the rule and connects its intended consumers. Preserve the public behavior before combining extraction with unrelated protocol, UI, persistence, or performance changes.
- Verify how each consumer resolves the shared module in its actual build. Inspect entry points and transitive imports: an export of a portable type or helper can accidentally make a platform SDK part of another runtime's dependency graph.
- Keep concrete adapter construction in the owning application. Avoid imports that open connections, read required environment configuration, or initialize a global mutable instance merely to load a shared calculation.
- Move tests with the rule when appropriate, and keep integration checks where the adapter is used. Remove replaced copies and obsolete wiring only after consumers use the new path. If migration is intentionally partial, record which path is authoritative and what remains; do not describe divergent copies as completed sharing.
- Preserve independent build and release constraints. A contract change may need backward-compatible evolution when consumers update at different times. Extracting a module does not authorize changing package managers, merging lockfiles, or synchronizing framework upgrades.

## Verify behavior at the boundary that owns it

Select relevant checks, using the existing runners and infrastructure:

| Changed area | Evidence |
| --- | --- |
| Domain rule, calculation, or codec | Observable outputs and error cases for representative inputs and actual domain boundaries, without requiring unrelated infrastructure. |
| Shared orchestration | Controlled dependencies demonstrate the operation sequence and outcomes, including relevant failures and cancellation. Assert interactions only when their order or count belongs to the contract. |
| Stateful instance or ownership | Independent owners do not leak state into one another; affected cleanup and replacement paths release their resources and cannot apply obsolete results to a new owner. |
| Concrete adapter | The real integration maps data, errors, and resource ownership as promised. Use an isolated local integration where available and identify any native/device behavior it cannot establish. |
| Public contract or entry point | Affected consumers still compile/build or pass their integration checks. The portable entry point can load in its intended environment without unavailable runtime APIs or accidental initialization. |
| Extraction across consumers | Established behavior remains consistent where required and documented platform differences remain intact. Tests should not encode only the new implementation's own assumptions. |

Passing tests with a fake database, transport, or native bridge verifies the core's use of that contract, not the concrete adapter. Conversely, one successful device operation does not cover domain edge cases. Report those scopes separately and identify unverified targets.

Do not add tests that merely mirror method names or folder structure. Reuse existing evidence for unchanged adapters; a narrow domain fix does not require a new architecture harness or every supported device. Reviews can describe missing verification without starting an implementation task.
