---
name: implementation-trade-offs
description: "Choose how to build a piece that shapes a product's performance, quality, or reliability: implement it in the project, use or improve a library, or work around the gap, weighing measured gains and losses with little weight on writing time, and matching the depth to the project's phase. Use when a constraint pushes a design toward a workaround, when a library is weak for the need or a clearly better result than existing tools looks possible, or when deciding between writing code and adopting a library; excludes ordinary use of libraries the project already chose and choices it already recorded."
---

# Implementation Trade-offs

Aim for the best product the context allows: fast, efficient, reliable, and good to use. Common practice often settles for a workaround or a weak library because writing code used to be the main cost. When an agent writes the code, that cost is small, and estimates of it are often far off: a piece estimated at days may take half an hour. Give writing time little weight, and weigh what the product and its users get: performance, quality, reliability, usability, security, and licensing.

A trade-off that degrades the product is the owner's choice, made knowingly, never a default reached to save effort. An option the owner never saw cannot be chosen.

## Match the phase

- **A prototype that tests whether something works or can be built:** take the quickest route that answers that question, and note what would change once it is settled.
- **A prototype that tests whether an optimization is worth it:** build the optimization as well as it can be built. A half-optimized test cannot show how much there is to gain.
- **A path users feel** (a core flow, a transfer, playback, startup): apply the whole practice.

When an explicitly invoked workflow such as concept-prototype already weighs approaches, apply this phase rule and the direct-route rule inside it rather than running a second comparison.

## Notice the gap

- A device or service offers a native route (its own download, export, API, or file format) that the project cannot consume as is.
- The design drifts toward emulating, accelerating, polling, converting, or proxying around a missing piece.
- The library at hand is weak for the need: slow, poorly integrated, unmaintained, too large, or missing the case, and a better result looks possible.

It does not apply to ordinary work with libraries the project already uses, or to a choice already recorded with its reasons, unless new evidence, such as a measurement or the owner's complaint, reopens it.

## Map the options

1. **Build it in the project:** the direct route (the requests the vendor's own clients make, the documented protocol, the native format), or a piece better than existing libraries offer, such as a focused HTTP or protocol client, a parser, or the part of a vendor SDK the product needs. Write it in a memory-safe systems language such as Rust unless the project's rules choose another: it performs close to C and rules out whole classes of mistakes.
2. **Use or improve a library:** as is, built with only the parts needed, or with fixes and improvements of its own. Check platform support, the license and its obligations, size, maintenance, and security record.
3. **Work around the gap:** what it gives up, what it depends on (behavior the vendor does not guarantee, a feature used for something else), and what would later replace it.

Add "defer" when the gap does not block the current goal.

## Weigh them

- Rank the criteria by what the product needs: performance, quality, reliability, usability, compatibility, security, license, size, maintenance. Writing time comes last.
- Test a doubtful gain instead of guessing it. When a route is dismissed as "little gain" or "too much work", build a proof of concept good enough to decide: it may show 1% that does not justify the change, or a difference far larger than expected. Label untested figures as estimates.
- Keep deep, mature libraries that are hard to match. FFmpeg for media is the model: build it with only the components needed and call it directly, for example from Rust through its C interface. Replacing such a library needs a rare, measured reason.
- Elsewhere, an existing library is not a reason to stop. When it is weak for the need, building the piece or improving the library is a real option.
- Push performance as far as it goes while nothing clearly pays for it. Where more speed costs stability, security, or something else, present each level as an option with its cost.

## Present and decide

- Before building a workaround or a large custom piece, present the options to the owner with the measurements, the costs, and a recommendation that defaults to the direct route when it is viable. When an improvement is clearly better and has no real downside, build it and report.
- Verify custom code against a reference: another implementation's output on the same input, the vendor's own client, and tests with made-up data.
- Record the chosen route and the rejected ones with their reasons where the project keeps its decisions. A workaround kept for now records what would replace it and when to reconsider.

## Examples

These cases show the pattern; the names are generic.

- **A recorder's file download.** A network video recorder's own clients download clips as files at the network's full speed. The file is an MPEG-2 program stream, which the mobile platform's media library reads only with old MPEG-2 video inside, not with the H.265 these cameras record. The first implementation avoided the format by playing the recording faster than real time over RTSP, which capped downloads at a third of the network's speed and relied on the recorder's fast playback. The direct route was a few hundred lines that read the program stream, checked byte for byte against a mature library's output on the same file; downloads then ran at the network's speed. It should have been offered from the start.
- **Media encoding.** Shrinking clips for sharing needed an encoder with constant-quality control. Writing one was never sensible: FFmpeg and its encoders already do it well. The real choice was between them, built with only the needed components, and the phone's hardware encoder, measured on the same clip for quality per byte and time; the encoder's copyleft license was a constraint the owner decided on knowingly.
- **A protocol client.** The platform's streaming client rejected the recorder's streams because their session description lacks parameter sets. Instead of converting the streams on a server, an established client library in another language handled them, with two small documented patches kept in the project.
