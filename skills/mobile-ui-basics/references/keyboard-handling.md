# Keyboard Handling on Mobile

Read the relevant runtime section when changing editable layouts or diagnosing keyboard overlap. The intended outcome is visible, usable text entry, not a particular wrapper or dependency.

## Establish ownership and geometry

Trace the focused control through its scroll container, screen, navigation chrome, and any modal or sheet host. An overlay rendered elsewhere may need its own keyboard handling even when the underlying screen works. Inspect the actual supported runtime and installed dependency versions before choosing APIs.

First establish which layer resizes or moves for the keyboard and which layer scrolls the field into view. Avoid adding keyboard height on top of an already resized viewport. Measure in a consistent coordinate space, including the appropriate safe-area or header offset once. Derive adjustments from current layout and keyboard/viewport information, not a guessed device height.

Reveal only as much as needed, leaving reasonable breathing room. Respond to focus changes even without a new keyboard-open event, and reevaluate when multiline content or the available area changes. Prefer the installed library's focus/caret support. For custom handlers, cancel stale measurements and pending scrolls when focus changes or the screen closes, and remove listeners. Dismissal should remove temporary compensation without resetting the whole form or leaving an unusable scroll position.

Identify the action associated with the active field explicitly instead of assuming the next button or the last button on screen is its action. Measure the span from the field to that action, including any intervening controls. If the span fits the available area with the usual clearance, reveal it as a unit and place the clearance after the action. Otherwise reveal the active text without forcing the form to its bottom. Reevaluate when the action, validation text, editor size, or viewport changes; keep the submit action reachable even when it cannot remain visible alongside the editor.

## React Native and Expo

Documentation reviewed on **2026-09-26**. API references target **React Native 0.86**; the Expo guide is unversioned. This reference contains integration guidance, not an executed implementation or a tested compatibility range. Adapt it to the consuming project's resolved versions.

- Inspect Android window-resize/pan behavior, edge-to-edge insets, and the navigation or sheet library's existing keyboard support before adding another layer. Changes to native configuration may require a new build; a JavaScript refresh does not establish that they took effect.
- `KeyboardAvoidingView` can change layout to make room. Choose its behavior and `keyboardVerticalOffset` from the actual container and platform; its presence does not prove that a long form scrolls to the active field. [React Native reference](https://reactnative.dev/docs/0.86/keyboardavoidingview).
- Ensure the scroll view and its ancestors have bounded layout. Coordinate iOS `automaticallyAdjustKeyboardInsets` with other compensation. `keyboardShouldPersistTaps` controls tap delivery/dismissal, not field visibility; choose it so submit and other intended actions remain usable. [ScrollView reference](https://reactnative.dev/docs/0.86/scrollview).
- Keyboard event availability differs by platform and Android configuration; do not assume iOS timing events everywhere. Prefer supported layout/inset mechanisms and clean up any event subscriptions. [Keyboard reference](https://reactnative.dev/docs/0.86/keyboard).
- Preserve an existing keyboard-aware library when it fits. Before adding or replacing one, check its installed/versioned APIs, native prerequisites, and compatibility with the project's Expo Go or development-build workflow. Do not introduce a native rebuild merely to copy an example. [Expo keyboard guide](https://docs.expo.dev/guides/keyboard-handling/).

## Mobile web, including React Native Web

Web references reviewed on **2026-09-26**. No browser/OS compatibility matrix was executed for this skill. Inspect support in the project's target browsers instead of treating native React Native behavior as web evidence.

Start with native input focus behavior and correctly bounded scroll containers. Inspect fixed headers/footers, nested overflow, and any body scroll lock before adding global focus handlers.

The visual viewport can shrink for a keyboard without the layout viewport doing so. If custom positioning needs `window.visualViewport`, feature-detect it, account for its offsets, and respond to relevant resize/scroll changes. These changes can also come from zoom and browser UI; a reduced viewport height alone does not identify an open keyboard. [VisualViewport reference](https://developer.mozilla.org/en-US/docs/Web/API/VisualViewport).

Prefer minimal reveal within the intended scroller instead of forcing the page to its bottom on every focus. Keep user zoom and text scaling available. Verify actual mobile keyboard opening/closing when claiming that an input stays visible; desktop device emulation alone is insufficient.
