# Verification

Read this reference when checking a changed desktop screen: capturing it in each theme and state, checking the pointer, and measuring contrast. The aim is to see the screens the user will see, without opening windows on the user's desktop or driving it with simulated input.

## Capture screens without the user's desktop

- **Scripted runs:** give the app test hooks that open its window at a fixed size, select a theme and a data folder, and run a list of steps (open a tab, add sample files, start a job, wait for a state). At each step that needs a picture, the app asks for a capture and waits until it is saved. Keep the hooks behind environment variables so they never change the product's behavior.
- **Off the desktop:** run the capture where it cannot disturb the user. On Windows, start the real build on a separate, hidden desktop (`CreateDesktop`, then the process with that desktop in its startup information), capture its windows and open menus with `PrintWindow`, and drive it with posted mouse and key messages; nothing appears on the user's screen. Make the capture process per-monitor DPI aware, or a scaled display returns cropped pictures. For a native app whose code also builds for Linux, a virtual X display (Xvfb with software rendering) works from WSL; point fontconfig at the Windows fonts so text renders as it will for the user. For a web UI, use a headless browser. If the only way is a real window on the user's machine, ask first and use a secondary monitor.
- **The real build first:** capture the app itself rather than a separate render or demo mode. Keep such a mode only for states the capture cannot reach, such as connected hardware or a device's error, and give it made-up values.
- **Capture the window itself.** Some renderers present through Vulkan or composition, so a capture of the whole screen can come out black; capture the window by its handle or id.
- **Fixtures:** sample data with very long names and paths, many items, and every status. Use made-up names, never real user data.
- **Each pass:** every theme the app offers, the narrowest supported window size, and every state of the content that changed (idle, working, finished, each error, empty).

## Check the pointer

- Where a scripted capture runs on a virtual display, move the virtual pointer over each clickable element and over the informative parts next to it, and read the cursor the display shows. On X11, XFixes reports the current cursor's image; comparing its size and hot spot with the cursor theme's files tells the hand from the arrow and the I-beam, because X keeps no cursor names.
- In a web UI, check the computed `cursor` style of each clickable element and its disabled state.
- When neither is possible, list the elements whose pointer was not checked in the handover.

## Measure contrast

Compute the WCAG contrast ratio from the sRGB colors of the theme: `(L1 + 0.05) / (L2 + 0.05)`, where L is the relative luminance of the lighter (L1) and darker (L2) color. Check each text color against each background it appears on, and each control edge or state marker against the colors next to it, in every theme. Translucent colors are measured after blending them over their actual background.

## Before and after

For a visual change the owner reviews, capture the same steps with the previous build and with the new one, and put the pictures side by side with a label over each. Keep the previous build's executable until the review is over, so the before pictures can be repeated.
