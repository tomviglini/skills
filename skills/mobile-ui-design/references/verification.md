# Verification

Read this reference when checking a changed mobile screen: capturing it in each state and setting, checking targets and labels, and measuring contrast. The aim is to see the screens the user will see without opening windows on the user's desktop, driving the user's phone, or operating their real devices. The commands target the Android emulator and `adb` from the Android SDK, written on 2026-10-27 for API 36 images. They were first run end to end on 2026-10-27, on an Expo app: headless boot, `screencap`, `uiautomator dump`, `input`, `font_scale`, and a simulated device at `10.0.2.2`, and `wm density` across the size range on 2026-11-04; confirm the others on first use.

## Run the app where it disturbs no one

- **Headless emulator:** start the virtual device without a window, `emulator -avd <name> -no-window -no-audio -no-boot-anim -no-snapshot-save`, and wait for `adb shell getprop sys.boot_completed` to print 1. It runs on the machine that has the SDK and hardware acceleration, also when that is the user's desktop computer, without showing anything there. If the only way is a visible emulator window or the user's phone, ask first.
- **Install the build under review** with `adb install -r`, and keep the previous build's APK for the before pictures.
- **Data and devices:** use made-up names and sample data, never the user's. An app that controls hardware talks to a simulated device instead of the user's real ones: a small server on the host that answers the device's protocol, which the emulator reaches at `10.0.2.2`. The simulated device can report every state the screens need (on, off, each mode, slow answers, errors, offline). An emulator or device may still hold the app's saved data from earlier use, such as a real device's address: clear the app's data before its first launch (`adb shell pm clear <package>`) and again when done, so it starts as a fresh install. Do not edit an app's stored data by hand.

## Several projects on one machine

- Give each project its own development server port and app id, so their development builds and servers run side by side without reinstalling to switch.
- The emulator image matches the host's architecture, x86_64 on a PC; the development build for it targets that architecture, apart from the arm64 builds for phones.
- With the development server in WSL and the emulator on Windows, `adb reverse tcp:<port> tcp:<port>` lets the app reach the server.
- Launchers, shortcuts, and the emulator's name are the workstation's, not the project's: they stay in the owner's notes for that machine.

## Capture and drive

- **Screenshots:** `adb exec-out screencap -p > shot.png` captures the whole screen, system bars included, so insets show.
- **Transitions:** a jump that lasts one frame does not show in screenshots. Record the transition, starting the recording and the input in the device's shell so they run in order: `adb shell "screenrecord --time-limit 4 /sdcard/clip.mp4 & sleep 1; input tap <x> <y>; wait"`. Pull the file and extract every recorded frame, `ffmpeg -i clip.mp4 -fps_mode passthrough frame%03d.png`, then look at them side by side. To hold a loading state, have the simulated device stop answering.
- **Input:** `adb shell input tap <x> <y>`, `input swipe`, `input text`, and `input keyevent KEYCODE_BACK` drive the app on the emulator, never on the user's desktop. Coordinates are in pixels; divide by the density (`adb shell wm density`) over 160 to get dp.
- **Layout and labels:** `adb shell uiautomator dump /sdcard/ui.xml`, then `adb pull`, gives every node's bounds, text, content description, and whether it is clickable. Check from it that every clickable node is at least 48 × 48 dp, that icon-only controls have a content description, and that nothing clickable overlaps.

## Settings to cover

| Setting | Command | Reset |
| --- | --- | --- |
| Dark or light theme | `adb shell cmd uimode night yes` or `no` | `cmd uimode night auto` |
| Size range ends | `adb shell wm density <dpi>`, where dpi = screen width in px × 160 / width in dp (on a 1080 px screen: 440 for 393 dp, 360 for 480 dp), or the app's size setting | `wm density reset` |
| Outside the range | the same, for 320 and 540 dp (540 and 320 on a 1080 px screen) | `wm density reset` |
| Largest font | `adb shell settings put system font_scale 2.0`: nothing in the app changes | `font_scale 1.0` |
| Three-button navigation | `adb shell cmd overlay enable com.android.internal.systemui.navbar.threebutton` | the same with `navbar.gestural` |
| Landscape | `adb shell settings put system accelerometer_rotation 0`, then `user_rotation 1` | `user_rotation 0` |
| Tablet width, where the project supports wider windows | `adb shell wm size 1600x2560` with `wm density 320` (800 dp wide) | `wm size reset` |

Some apps read the configuration only at start; restart the app after changing a setting (`adb shell am force-stop <package>`, then launch it again).

## The size range

Capture each changed screen at both ends of the range, scrolled to its end. Check the scaling itself once per app and after a change to the shared viewport or text component, with the settings above: outside the range on both sides, a size chosen in the app away from the phone's, and the largest font. `uiautomator dump` reports bounds after the app's scale, so taps taken from it land on the control.

## The keyboard

The emulator shows the real software keyboard: focus each changed field by tapping it, capture, switch fields with the keyboard open, and close it with Back. Mobile browser emulation does not show a keyboard, so claims about keyboard behavior need the emulator, a simulator, or a device.

## Web UIs on a phone

A headless browser with a phone profile (touch enabled, the phone's device scale factor) checks layout at the viewport widths of the size range's two ends, and the computed size of every clickable element. The browser's own zoom and text settings stay the user's. It does not show the keyboard, system bars, or real touch.

## Measure contrast

Compute the WCAG contrast ratio from the sRGB colors of the theme: `(L1 + 0.05) / (L2 + 0.05)`, where L is the relative luminance of the lighter (L1) and darker (L2) color. Check each text color against each background it appears on, and each control edge or state marker against the colors next to it, in every theme. Translucent colors are measured after blending them over their actual background.

## What stays for a real device

Touch feel, haptics, Wi-Fi or Bluetooth discovery, and timing against real hardware need a physical phone. List them in the handover as unchecked instead of claiming them from the emulator.

## Before and after

For a visual change the owner reviews, capture the same steps with the previous build and with the new one, and put the pictures side by side with a label over each.
