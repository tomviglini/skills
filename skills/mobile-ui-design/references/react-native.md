# React Native and Expo

Read this reference when building or reviewing screens of a React Native or Expo app. It collects behaviors of the framework and its common libraries that make a screen jump, scroll, or flash without their documentation warning about it, how to scale the app to its size range, and how to make room for a bar floating over the end of a page, each with the platform where it was seen and the fix that removed it. Keyboard handling has its own section in [keyboard handling](keyboard-handling.md#react-native-and-expo).

They were found on 2026-11-02 in an app built with Expo SDK 58, React Native 0.88 (New Architecture), and react-native-safe-area-context 5.10. They were seen on an Android 16 (API 36) emulator running a development build, and each fix was confirmed by recording the screen before and after it. iOS was not checked. Check them again after upgrading these libraries, and adapt the fixes to the project's components.

## A new screen drawn under the status bar for one frame

- **Seen on Android:** a screen that mounts after launch with its own `SafeAreaView` from react-native-safe-area-context draws its first frame without the top inset, under the status bar, then jumps down by the inset's height. Opening an item and coming back to the list both show it; tabs that stay mounted do not.
- **Why:** the native view reads the insets only once it is attached to the window and passes them to the layout as a state update, so the first layout has no padding and a second one adds it.
- **Fix:** pad each screen with the values of `useSafeAreaInsets()`, which the provider already holds when the screen mounts, through one shared screen container. Refuse `SafeAreaView` imports with ESLint's `no-restricted-imports`, naming the shared container in the message, so it does not come back. The hook's padding follows an insets change one render later, as on rotation or a switch of navigation mode, which shows only while that happens.

## Images that fade in

- **Seen on Android:** `Image` fades in over 300 ms each time it mounts, so a logo appears gradually every time its screen comes back.
- **Fix:** `fadeDuration={0}` on images that should already be there, such as a logo or an icon.

## Rounded corners lost

- **Seen on Android** on 2026-10-12, in an app built with Expo SDK 57 and React Native 0.86: a view with rounded corners whose background appears after it mounts, such as a tab's tile when it becomes active, can lose its rounded corners and draw square.
- **Fix:** give the view a transparent 1 dp border; it keeps the corners when the background arrives.

## A focus that scrolls the screen

- **Selectable text, seen on Android:** a `Text` with `selectable` takes focus even in touch mode, so a screen that opens in a `Modal` scrolls its `ScrollView` to that text. A settings screen opened scrolled down to its About section. Pass `selectable={false}` on values nobody needs to copy.
- **Focus before layout, seen on Android:** a `TextInput` focused when it mounts (`autoFocus`, or `focus()` right away) inside a `ScrollView` makes the scroll view jump to the field's provisional position at the top, then back once the keyboard opens. Focus it from its `onLayout`, once it has its place.
- **Revealing a field:** scroll a field into view with `scrollTo({ y, animated: false })`. An animated scroll right after the keyboard opens reads as the page going back and forth.

## A field focused in a modal without the keyboard

Found on 2026-11-06 in the same app and versions, first on a phone and then on the same emulator, where the input method's state and screenshots confirmed the fix.

- **Seen on Android:** a `TextInput` focused as a `Modal` appears, with `autoFocus` or with `focus()` from the modal's `onShow`, shows its cursor but no keyboard, so the user taps a field that already looks active. Outside a modal the same focus opens the keyboard.
- **Why:** React Native's focus asks for the keyboard through `InputMethodManager.showSoftInput`, which Android ignores while the field's window does not have the focus yet; logcat says `Ignoring showSoftInput() as view=… is not served`. A modal is a window of its own, and it gets the focus some tens of milliseconds after it appears, after `onShow` too. Calling `focus()` again does nothing, because React Native skips a field that already has the focus.
- **Fix:** ask through the field's window instead, as Android's guide to [showing the keyboard reliably](https://developer.android.com/develop/ui/views/touch-and-input/keyboard-input/visibility) recommends: `view.windowInsetsController.show(WindowInsets.Type.ime())` from Android 11, which Android keeps and carries out once the window has the focus (logcat shows the request, then `onShown` when the focus arrives); before Android 11, wait for that focus with a `ViewTreeObserver.OnWindowFocusChangeListener` and then call `showSoftInput`. React Native has no call for this, so a small native module function receives the field's view tag (`findNodeHandle`) and runs it on the main thread. The shared input calls `focus()` and then that function, once the field has its place and, inside a modal, after `onShow`, when the native view exists. ESLint's `no-restricted-syntax` with the selector `JSXAttribute[name.name='autoFocus']` keeps `autoFocus` from coming back.

## Scaling to the size range

Found on 2026-11-04 in the same app and versions, on the same emulator, at scales from 0.81 to 1.22 ([size range](proportions.md#size-range)).

- **Not through Android's density:** React Native reads the display's real metrics (`DisplayMetricsHolder` uses `getRealMetrics`), so overriding the app's density does not reach its layout. Scale in JavaScript instead: a viewport view at the root and inside every `Modal`, with its content at `(100 / scale)%` of the width and height, `transform: [{ scale }]`, and `transformOrigin: 'top left'`. Percentages need no measuring, so the first frame is already in place; the same tree at every scale keeps each screen's state when the size changes.
- **Insets:** each viewport passes the insets divided by the scale through `SafeAreaInsetsContext`. A modal opened from inside another viewport divides the phone's insets, not the parent's already divided ones.
- **Units:** `measureInWindow`, `Keyboard.metrics()`, and the keyboard events' height are in the phone's dp; `measureLayout` and scroll offsets are in the app's. Divide the first ones by the scale.
- **Fixed type:** `Text.defaultProps` no longer applies (React 19, function components), so use a shared `Text` with `allowFontScaling={false}`, the same on the shared input, and ESLint's `no-restricted-imports` refusing `Text` and `TextInput` from `react-native`. @expo/vector-icons already sets it on icons.
- **Touch:** Android maps touches through the transform: pressables, the community slider, and scroll views respond where they are drawn.

## Room for a bar floating over a scroll view

Found on 2026-11-04 in the same app, for bars laid over the end of a `ScrollView`.

- **Measure the bar:** its `onLayout` height, plus its offset from the edge and the gap, is the content container's `paddingBottom`. Start from the height its styles give (its buttons, padding and border), so the first frame is already right, and let the measure replace it. A fixed padding left a large blank at the end, or too little room once the bar changed.
- **The page at its end:** keep the scroll offset (`onScroll`), the visible height (`onLayout`) and the content height (`onContentSizeChange`) in refs. Decide whether the page is at its end in the action that makes the bar appear, before its room is added, then call `scrollToEnd({ animated: true })` from `onContentSizeChange` once the content has grown. A shared scroll view that owns its ref needs to forward one for this.
