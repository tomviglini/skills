---
name: concept-prototype
description: "Run an explicitly invoked prototype-phase exploration of how a product idea should work in real use: map the real-use situations, weigh approaches by their architecture and trade-offs, implement the chosen behavior in the app so the owner can try it, and report on the concept with ideas and suggestions. Never activate for ordinary implementation, planning, or design-value tuning."
disable-model-invocation: true
---

# Concept Prototype

Use this workflow only when the user explicitly invokes it. It is for a product in its prototype phase: the owner has an idea of how something should work and wants to decide by trying it in the app. The focus is the idea, meaning how it behaves in real use, its architecture, and its trade-offs, rather than the code. The code follows the project's conventions, but the project's documentation and test upkeep waits until the owner settles the direction; the thinking and the report are about the concept.

Invoking this command is the owner's instruction to defer that upkeep for this work, even where the project's instructions require it with every change, so the project needs no rule or exception of its own for it. Every other project rule still applies.

## Frame the concept

- Take the subject and the owner's initial idea from the invocation and conversation. Restate the question being decided in one or two sentences, for example "when and how the app shows that it is acquiring a location, and when it fetches content".
- Read the product definitions, decision records, and the current implementation relevant to the subject. Separate settled decisions, owner directions, open questions, and the agent's assumptions. Do not reopen settled choices; if the idea conflicts with one, show both and ask which changes.
- Ask only when the answer would change the whole exploration. Otherwise state the assumption and proceed; trying the result is a faster way to surface disagreement than a questionnaire.

## Ground it in reality

- Establish how the underlying technology and environment actually behave: platform APIs and permission models, typical latencies, failure modes, operating-system limits, costs. Use official documentation for the project's actual versions and cite it. Label estimates as estimates; never present an invented number as a fact.
- Pay attention to what the platform already provides, such as cached values, system dialogs, or lifecycle events. They often change which approaches are possible.

## Map the real-use moments

This is the core of the work. List the situations a real person goes through with the concept, not only the happy path:

- Time: first use ever, return after seconds, minutes, hours, or days, the app closed by the system versus kept in the background.
- Environment: good and bad conditions for the technology involved, slow or absent network, interruptions.
- User choices: granting, partially granting, refusing, or revoking something; acting before the app is ready.
- Change during use: the person moves, the data changes, a timer expires.

For each moment, state what the person should see and feel, what the system knows at that point, and what it must not do. Name the variable that actually drives the behavior; a rule keyed on the wrong variable, such as "first time" instead of "how old is the data", is a common finding.

## Choose an approach

- Weigh two or three genuinely different approaches, including the owner's idea as stated. For each, consider its states and transitions, the architecture (which component owns which state, what triggers each request, what is cached and where), and its trade-offs: perceived speed, correctness, failure handling, battery or cost, privacy, server load, implementation and testing complexity.
- Identify the parameters that are really product decisions, such as thresholds, timeouts, and cache lifetimes. Give each a default with its reason.
- Decide whether to ask before building. When trying the idea needs something costly, such as a native dependency and a new build, a data or API contract change, or a conflict with a settled decision, the owner cannot cheaply try the alternatives: present the real-use moments, the approaches, and a recommendation, and let them choose. Otherwise build without waiting. Ask anything else whose answer the code and documents cannot settle.

## Build it in the app

- Implement it in the project's code with its conventions, and keep the app working: run the project's quick checks, such as type checking and the existing unit tests. Leave documentation, decision records, and new test coverage until the owner settles the direction, and say in the report what was deferred.
- Make the real-use moments reachable. Conditions such as time passing, bad signal, or a revoked permission are hard to reproduce, so expose them, and the decision parameters, through the project's existing simulation or demo controls when it has them.
- When building them is cheap, implement the two most promising approaches behind a switch in those controls so the owner can compare them in the app. Otherwise implement the recommended one and keep the others in the report.

## Report on the concept

Write the summary about the idea, not the diff:

- What the person sees and what the app does in each real-use moment.
- The architecture: components, who owns which state, what triggers requests, what is cached.
- The approaches weighed, their trade-offs, and why this one.
- The decision parameters and their defaults.
- Ideas, suggestions, and risks that came up.
- Open questions ranked by impact, and what remains unverified, such as behavior that only a real device can show.
- How to try it: which moments to play first and where the controls are.

Keep the code part short: what areas changed and which checks ran.

## Iterate and close

- Treat the owner's reactions, such as "this message feels repetitive", as intent. Update the behavior and say what changed in the concept.
- When the owner settles on a direction, remove the approach switch and the code of the approaches not chosen, then bring the change up to the project's standards: the documentation, decision records, and tests its rules require, with the decisions recorded where the project keeps product decisions. If the owner drops the idea, revert it.
- Commits and pushes still need their own authorization.
