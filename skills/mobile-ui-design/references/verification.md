# Verification

Read this reference when checking a changed mobile screen: capturing it in each state and setting, checking targets and labels, and measuring contrast. The aim is to see the screens the user will see without opening windows on the user's desktop, driving the user's phone, or operating their real devices. The commands target the Android emulator and `adb` from the Android SDK, written on 2026-10-27 for API 36 images. They were first run end to end on 2026-10-27 (KEF Controller): headless boot, `screencap`, `uiautomator dump`, `input`, `font_scale`, and a simulated device at `10.0.2.2`; confirm the others on first use.

## Run the app where it disturbs no one

- **Headless emulator:** start the virtual device without a window, `emulator -avd <name> -no-window -no-audio -no-boot-anim -no-snapshot-save`, and wait for `adb shell getprop sys.boot_completed` to print 1. It runs on the machine that has the SDK and hardware acceleration, also when that is the user's desktop computer, without showing anything there. If the only way is a visible emulator window or the user's phone, ask first.
- **Install the build under review** with `adb install -r`, and keep the previous build's APK for the before pictures.
- **Data and devices:** use made-up names and sample data, never the user's. An app that controls hardware talks to a simulated device instead of the user's real ones: a small server on the host that answers the device's protocol, which the emulator reaches at `10.0.2.2`. The simulated device can report every state the screens need (on, off, each mode, slow answers, errors, offline). An emulator or device may still hold the app's saved data from earlier use, such as a real device's address: clear the app's data before its first launch (`adb shell pm clear <package>`) and again when done, so it starts as a fresh install. Do not edit an app's stored data by hand.

## Capture and drive

- **Screenshots:** `adb exec-out screencap -p > shot.png` captures the whole screen, system bars included, so insets show.
- **Input:** `adb shell input tap <x> <y>`, `input swipe`, `input text`, and `input keyevent KEYCODE_BACK` drive the app on the emulator, never on the user's desktop. Coordinates are in pixels; divide by the density (`adb shell wm density`) over 160 to get dp.
- **Layout and labels:** `adb shell uiautomator dump /sdcard/ui.xml`, then `adb pull`, gives every node's bounds, text, content description, and whether it is clickable. Check from it that every clickable node is at least 48 × 48 dp, that icon-only controls have a content description, and that nothing clickable overlaps.

## Settings to cover

| Setting | Command | Reset |
| --- | --- | --- |
| Dark or light theme | `adb shell cmd uimode night yes` or `no` | `cmd uimode night auto` |
| Largest text | `adb shell settings put system font_scale 2.0` | `font_scale 1.0` |
| Larger display size | `adb shell wm density <dpi>`, about 1.2 times the default | `wm density reset` |
| Smallest phone width | `adb shell wm size 720x1280` with `wm density 320` (360 × 640 dp) | `wm size reset` |
| Three-button navigation | `adb shell cmd overlay enable com.android.internal.systemui.navbar.threebutton` | the same with `navbar.gestural` |
| Landscape | `adb shell settings put system accelerometer_rotation 0`, then `user_rotation 1` | `user_rotation 0` |
| Tablet width | `adb shell wm size 1600x2560` with `wm density 320` (800 dp wide) | `wm size reset` |

Some apps read the configuration only at start; restart the app after changing a setting (`adb shell am force-stop <package>`, then launch it again).

## The keyboard

The emulator shows the real software keyboard: focus each changed field by tapping it, capture, switch fields with the keyboard open, and close it with Back. Mobile browser emulation does not show a keyboard, so claims about keyboard behavior need the emulator, a simulator, or a device.

## Web UIs on a phone

A headless browser with a phone profile (viewport about 360 to 412 CSS px wide, touch enabled, the phone's device scale factor) checks layout, text at 200 % zoom, and the computed size of every clickable element. It does not show the keyboard, system bars, or real touch.

## Measure contrast

Compute the WCAG contrast ratio from the sRGB colors of the theme: `(L1 + 0.05) / (L2 + 0.05)`, where L is the relative luminance of the lighter (L1) and darker (L2) color. Check each text color against each background it appears on, and each control edge or state marker against the colors next to it, in every theme. Translucent colors are measured after blending them over their actual background.

## What stays for a real device

Touch feel, haptics, Wi-Fi or Bluetooth discovery, and timing against real hardware need a physical phone. List them in the handover as unchecked instead of claiming them from the emulator.

## Before and after

For a visual change the owner reviews, capture the same steps with the previous build and with the new one, and put the pictures side by side with a label over each.
