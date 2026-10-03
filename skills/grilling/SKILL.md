---
name: grilling
description: "Run an explicitly invoked interview to challenge and clarify an idea or decision. Implement when the user asks to proceed, verify the result, then update the affected docs and ADRs from the actual outcome. Never activate for ordinary planning or implementation requests."
disable-model-invocation: true
---

# Grilling

Use this workflow only when the user explicitly invokes it. Installation, a standing documentation rule, or a request to plan a feature does not start an interview. Keep it self-contained and usable with or without a repository.

Follow three stages: discuss the subject until the user is satisfied; implement and verify when the user asks to proceed; then document the actual outcome. Agreement with an individual answer is not authorization to implement. By default, keep the proposal in the conversation during discussion instead of writing speculative project docs or ADRs before the code exists. Honor an explicit request for a written plan or design-only record as a separate deliverable.

## Establish the subject and evidence

- Take the subject, scope, and constraints from the invocation and conversation. Ask for the missing subject if necessary; do not make the user restate information already available. Keep the interview about the selected decision rather than expanding it into a review of the entire product.
- For repository work, read the relevant instructions, implementation, tests, and docs/ADRs. Use them to answer factual questions and understand established decisions. For a standalone idea, proceed without requiring a repository or scaffolding files.
- Distinguish observed behavior, previously accepted decisions, user requirements, agent recommendations, and unresolved assumptions. A plausible recommendation does not become a requirement merely because the user did not reject it.
- Respect existing exceptions and resolved choices. If a relevant contradiction appears, show the conflicting statements or evidence and ask which rule to change or whether to record a scoped exception. Do not invent an exception, silently override a decision, or reopen unrelated settled choices.

## Conduct the interview

- Prioritize questions that can change the result: the intended outcome, scope, user-visible behavior, constraints, alternatives, failure cases, compatibility, and meaningful trade-offs. Follow the branches relevant to this subject; do not mechanically ask about every category.
- Ask one consequential question at a time by default. A short group of independent questions is useful when it reduces back-and-forth; never group a question with one whose answer it depends on. Adjust to the user's preferred pace.
- Explain why each question matters and give a recommendation with its reason and main trade-off when the evidence supports one. Keep options clear and permit a different answer. Do not present an unsupported preference as a fact or steer every question toward the first design proposed.
- Investigate facts the available code, documentation, or read-only checks can establish instead of outsourcing that investigation to the user. Reserve questions for intent, priorities, and choices that the evidence cannot settle. Do not start prototypes, implementation, or external changes as part of fact gathering.
- Wait for answers to material choices. Use them to revise the next questions and challenge inconsistencies constructively. Summarize a corrected understanding when needed without requesting approval for every sentence or implementation detail.
- Treat uncertainty honestly. If an answer needs measurement, a prototype, or an unavailable fact, identify that need and its consequence. A decision to defer or accept a stated limitation can close that branch; further rephrasing cannot replace missing evidence.

## Close the interview and establish the next action

When the essential choices are coherent, or the user asks to wrap up, present a concise proposed agreement: the intended behavior, scope and exclusions, decisions and reasons, exact constraints that matter, and unresolved or deliberately deferred items. Preserve precise limits and negative requirements rather than weakening them into generic summaries.

The user decides when the proposal is satisfactory and whether to implement it. If that intent is still unclear, ask whether to keep discussing or proceed with the proposed implementation. Acceptance of the design alone does not request implementation or documentation. Silence, elapsed time, or the absence of further questions is not authorization. Reuse a clear instruction to proceed with the agreed implementation rather than adding a separate formal approval step.

Do not require every imaginable uncertainty to disappear. Make remaining assumptions and deferred work visible so the user can decide whether they are acceptable. If the user stops after the discussion or postpones implementation, keep the result in the conversation unless they ask for a written artifact. Finishing the interview does not itself create documentation work.

## Implement, verify, and incorporate discoveries

Once implementation is authorized, complete the agreed work using the project's existing conventions and relevant adopted skills. Preserve the agreed behavior, exact constraints, exclusions, and independently authorized scope. This command does not replace the project's implementation or testing practices and requires no other skill to be installed.

- Make the code changes and run the checks appropriate to their behavior. Establish what was actually implemented and observed rather than using the agreed design as evidence of success.
- Resolve routine implementation details through engineering judgment within the accepted constraints. New information is expected; it does not automatically justify restarting the interview or asking about every adjustment.
- If code or tests reveal a material conflict with agreed behavior, scope, compatibility, or an established project rule, explain the evidence and trade-off. Recommend a correction and ask about the affected decision before making the disputed change. Continue independent authorized work where possible; do not silently replace a user requirement with a more convenient implementation.
- Carry accepted corrections forward and verify the resulting candidate. Preserve the useful reason for a changed approach without turning every experiment into a historical decision.
- If implementation or verification cannot be completed, report the actual partial state and blocker. Do not describe the feature as delivered or turn an untested assumption into a confirmed outcome.

## Document the result after implementation and verification

Before finishing the authorized work, assess its documentation impact and complete the necessary updates from the final code, checks, and decisions. When the project's workflow sets when a task completes, such as a close step after the owner approves the work, make these updates then. This maintenance is part of the implementation workflow; do not request another routine approval to update affected docs. A commit, push, deployment, or unrelated scope expansion still requires its own authorization.

- Reuse the project's documentation conventions and an adopted documentation skill when available. For a separately requested design-only deliverable, use the same conventions but label the result as an unimplemented proposal or accepted design, as appropriate.
- Put durable information in its existing home: current contracts or guidance in topic docs, significant trade-offs in ADRs, and pending implementation in the maintained plan when useful. Do not require `CONTEXT.md`, a new ADR directory, a plan per session, or an interview transcript.
- Record the final decisions and relevant rationale, including meaningful discoveries during implementation. Keep proposals, accepted decisions, implemented behavior, and observed verification distinct. A passing local check does not establish deployment or native-device behavior. Retain unresolved items as unresolved and preserve superseded ADR history.
- Avoid duplicating existing material or changing unrelated documents. If recording the agreement would require a substantial documentation migration that has not been chosen, prepare the concrete mapping and ask about that migration while completing compatible updates.
- Verify changed links and factual claims, then report the implementation, checks, documentation changes, and anything still open. Accurate existing docs need no edit, and a routine change need not produce an ADR. For incomplete work, keep records aligned with the actual partial state rather than presenting the original plan as delivered.
