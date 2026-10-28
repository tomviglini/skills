# Android

Read this reference when changing an Android app's window, system bars, Back handling, permissions, notifications, or target SDK, or before raising the target SDK. It covers the platform's conventions beyond Material's components. Reviewed on 2026-10-27 against Android 16 (API 36) and the announced Android 17 (API 37) changes; check the behavior changes of each newer version before targeting it.

## Edge to edge

- An app that targets API 35 is drawn edge to edge on Android 15 and later; from API 36 the opt-out attribute is ignored. Treat edge to edge as required.
- **Draw behind the bars:** the top app bar or the screen's background extends behind the status bar, a bottom navigation bar behind the navigation bar, and scrolling content scrolls behind both.
- **Pad by the insets:** text and controls stay clear of the system bars, the display cutout, and the keyboard. Apply the insets once, at the container that owns the edge: a screen's top padding, a bottom bar's bottom padding, a list's bottom padding so its last item scrolls clear. Swipeable areas near the screen edges, such as carousels, stay clear of the system gesture insets.
- **Bars:** with gesture navigation the navigation bar is transparent; do not add a scrim behind it. With three-button navigation the system adds a translucent scrim unless the app turns contrast enforcement off and draws its own background. Status bar icons are light or dark to contrast with whatever is behind them.
- **Frameworks:** in React Native and Expo, edge to edge is on by default in current versions; read insets from `react-native-safe-area-context` instead of fixed heights, and check modals, which draw in their own window.

## Back

- System Back closes, in order: the keyboard, then an open menu, dialog, or sheet, then an editor (asking first when it has unsaved changes), then the current screen; from a tab other than the start tab it returns to the start tab; on the start screen it leaves the app. Every one of those layers also has a visible way to close.
- Do not add on-screen prompts that replace Back, and do not intercept Back to log or to run business logic. Register a handler only while there is something to close, and remove it after.
- **Predictive Back:** from API 36, Android shows the back-to-home and cross-activity animations by default and no longer calls `onBackPressed` or dispatches `KEYCODE_BACK`. Apps handle Back through `OnBackPressedCallback` (or Compose's `BackHandler` and `PredictiveBackHandler`). When the app's framework or navigation library does not support predictive Back yet (check its documentation for the installed version; React Native apps commonly keep it off), keep the opt-out (`android:enableOnBackInvokedCallback="false"`, Expo's `android.predictiveBackGestureEnabled: false`), record the reason, and check Back on every affected screen after each framework upgrade. The opt-out is described as temporary.

## Permissions

- Request only what the current feature needs, when the user starts that feature, never at launch. Check the permission each time before using it.
- When the platform reports that a rationale is due, explain in the app's own words why the feature needs it, with a way to decline. After a denial, keep the rest of the app working, show a specific message where the feature was, and do not ask again unprompted: two denials make the denial permanent, and pushing the user to Settings is discouraged.
- **Local network:** apps that connect to devices on the local network (TCP or UDP to private addresses, multicast and broadcast, mDNS and SSDP discovery) need `ACCESS_LOCAL_NETWORK` once they target API 37; until then the `INTERNET` permission grants it implicitly, and the app must not request it. When raising the target to 37: declare it, request it when the user first discovers or adds a device, explain it in context, and treat its denial as a known state (connections time out, sockets fail with a permission error) with a message that names the permission, not a generic network error. Discovery through the system's service picker needs no permission.
- **Notifications:** from Android 13, an app needs `POST_NOTIFICATIONS` to post notifications; request it from a user action that makes the purpose clear, such as turning on an alert. An app without notification features declares nothing. Notifications use channels by purpose, the right priority, and stay persistent only for ongoing activity.
- Keep the manifest to what the app uses: remove permissions that libraries or templates add but the app does not need, such as overlay or storage permissions in a release build.

## Window size and orientation

- From API 36, on displays at least 600 dp wide, Android ignores orientation locks, fixed aspect ratios, and non-resizable flags (games excepted): the app fills the window. Every screen must work in landscape and on tablets and foldables, and keep its state across rotation, folding, and resizing.
- Choose layouts by the window's width class: below 600 dp one column with 16 dp margins; from 600 dp 24 dp margins, and either a centered column with a maximum width or two panes. Cards and text keep a readable width instead of stretching across a tablet.

## Text size and display size

- Android 14 and later scale text up to 200 %, growing large text less than small text. Use sp for text and line heights, never sp for spacing, and check screens at the largest font size and the largest display size together.

## Themes, splash, haptics

- The core app quality guidelines ask for light and dark themes. An app that offers one theme records the decision; status bar icons and the splash screen match it.
- Android 12 and later show the system splash screen with the app's icon on a background color; do not add a second, custom splash screen.
- Use the system's haptic feedback types for discrete events, such as a slider's steps or a confirmed action, and offer a setting to turn them off when they are frequent.

## Quality checklist

Before a release that changes screens, go through the core app quality guidelines' visual, navigation, notification, permission, and accessibility items: consistent experience across sizes and orientations, no letterboxing, sharp graphics, light and dark support, standard Back and gesture navigation, state preserved when leaving and returning, minimal permissions requested in context, 48 dp targets, contrast, and content descriptions on non-text controls.

## Sources

| Source | What it gives | Reviewed |
| --- | --- | --- |
| [Android: Display content edge-to-edge](https://developer.android.com/develop/ui/views/layout/edge-to-edge) | Insets, what draws behind the bars, bar scrims and icon contrast | 2026-10-27 |
| [Android 16 behavior changes](https://developer.android.com/about/versions/16/behavior-changes-16) | No edge-to-edge opt-out, predictive Back by default, large screens ignore orientation and resizability | 2026-10-27 |
| [Android: Predictive back gesture](https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture) | Back callbacks, when to register them, the opt-out | 2026-10-27 |
| [Android: Request runtime permissions](https://developer.android.com/training/permissions/requesting) | Workflow, rationale, graceful degradation, permanent denial | 2026-10-27 |
| [Android: Local network permission](https://developer.android.com/privacy-and-security/local-network-permission) | `ACCESS_LOCAL_NETWORK`, covered traffic, timeline, failures without it | 2026-10-27 |
| [Android: Notification runtime permission](https://developer.android.com/develop/ui/views/notifications/notification-permission) | When to request `POST_NOTIFICATIONS`, exemptions | 2026-10-27 |
| [Android: Core app quality guidelines](https://developer.android.com/docs/quality-guidelines/core-app-quality) | The checklist above | 2026-10-27 |
| [Android 14: Non-linear font scaling](https://developer.android.com/about/versions/14/features#non-linear-font-scaling) | Text up to 200 % | 2026-10-27 |
| [React Native: BackHandler](https://reactnative.dev/docs/backhandler) | The framework's Back API | 2026-10-27 |
