---
name: local-device-control
description: "Build, change, or review how an app finds, identifies, commands, and reads devices on the local network, such as speakers, lights, microphones, and recorders: local-only control, a stable identity instead of an address, discovery that works on real networks, read-back until the device confirms a change, one command at a time, polling only while someone looks, and a simulated device for tests and screenshots. Use when adding or changing discovery, a device's protocol client, its commands, or how its state is read; cloud APIs and screens that only show data already read do not trigger it."
---

# Local Device Control

Control devices on the user's network directly and predictably: find them where they are, keep them by who they are, and show only what they report. Preserve the project's protocol client, transports, and recorded decisions. What the device does, measured on it, wins over the defaults below.

## Scope the work

Apply the parts a change touches: a new command needs the command rules, a new search the discovery rules. For a review or explanation, report findings without changing code.

Respect explicit project exceptions and resolved user choices. If an unresolved project requirement conflicts with this guidance, identify the difference and ask whether to adjust it or record a scoped exception before the disputed change.

## Local only

- Talk to the device on the local network, with no account or cloud service unless the device itself requires one. Nothing about the device leaves the network.
- A device protocol is often unencrypted and unauthenticated: its ports must not be reachable from the internet, and the project's security notes say so.

## Identity, not address

- Keep each device by a stable identity it reports, such as its serial number or UPnP device id. Its DHCP address can change: an address says where the device is now, not which device it is.
- A search that finds a saved device at a new address updates the saved address. A device added by its address learns its identity the first time it answers.

## Discovery

- Search with the device's own discovery protocol, such as SSDP or mDNS, and check each answer before offering it: its maker and model in the device's description, and an answer on its control port.
- Offer adding a device by its IP address, for networks that block multicast.
- On Windows, send a multicast search on every IPv4 adapter. Windows sends multicast through one adapter, which with VPN or virtual adapters is often not the one on the device's network.
- When a saved device stops answering, search for it again by its identity, at most once a minute, so a new address is found without flooding the network.
- Searching can need a platform permission or lock, such as Android's multicast lock or local network permission; ask for it when the search starts, not at launch.

## Commands and state

- Send one command at a time to each device, through one owner of its connection. A setting that shares a byte or register with others is changed by reading it, changing only its own bits, and writing it back.
- Devices often acknowledge a command before they apply it and keep reporting the old value for a while. After a write, read the value back at a short interval until it matches, within a time limit, then return what the device reports instead of failing because it was slow. Measure the interval and the limit on the device.
- The screen keeps showing the user's choice while the read-back runs, and never shows a value the device has not reported, such as a volume of 0 before the first answer. desktop-ui-design and mobile-ui-design cover that side when the project installs them.
- Poll a device's state only while a screen that shows it is visible, and stop when none is.

## Describe the device

Write protocol facts as what the device does, measured or observed: its registers, values, timings, and limits, with the model and firmware version they were seen on. Name no other app, firmware file, or third-party project as their source; legal-compliance covers what a repository keeps of a protocol learned elsewhere.

## Verify

Tests and screenshots use a simulated device on the host that answers the protocol in every state the screens need: on, off, each mode, slow answers, a delayed confirmation, errors, and offline. Never use the owner's real device, and give the simulated one made-up names, serials, and addresses (privacy-by-design).

| Changed area | Useful observations |
| --- | --- |
| Discovery | A search finds the simulated device on a machine with several adapters; a saved device at a new address is updated, not added twice; a device that stops answering starts at most one search a minute. |
| Commands | With a delayed confirmation, the screen keeps the chosen value and settles on the reported one; a device that never confirms ends on what it reports, within the time limit. |
| State | Hiding every screen that shows the device stops its polling; showing one starts it again. |

Report what was checked against the simulated device and what still needs the real one, such as its timing and discovery on the owner's network.
