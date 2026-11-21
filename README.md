# NiceKeys

![version](https://img.shields.io/badge/version-v1.3.0-5AD98C) ![macOS](https://img.shields.io/badge/macOS-13%2B-lightgrey) ![swift](https://img.shields.io/badge/Swift-5.9-orange)

**Current version: v1.3.0**  ([release notes](RELEASE_NOTES_v1.3.0.md), [changelog](#changelog))

A tiny macOS keystroke overlay in the style of Lunar Client's keystrokes mod. Dark, semi-transparent keys
(WASD, Shift, Space, mouse buttons with clicks per second, scroll wheel, and more) float above every app, including
full-screen ones. The overlay is click-through, event-driven, and light on CPU, GPU and memory.

Made by [NiceSky Studios](https://github.com/NiceSky-Studios).

## Download

Grab the latest `.dmg` from the [Releases page](https://github.com/NiceSky-Studios/NiceKeys/releases):

| File | For |
| --- | --- |
| `NiceKeys-<version>-universal.dmg` | Any Mac (recommended) |
| `NiceKeys-<version>-arm64.dmg` | Apple Silicon (M1 and later) |
| `NiceKeys-<version>-x86_64.dmg` | Intel Macs |

Open the `.dmg`, drag NiceKeys to Applications, and launch it.

## First launch

1. The hub opens with a short walkthrough. macOS asks for **Input Monitoring**: allow NiceKeys in
   System Settings > Privacy & Security > Input Monitoring. The walkthrough shows when it's granted.
2. If it still says it needs permission after you allow it, use **Relaunch NiceKeys** (in the walkthrough or General).
3. The keyboard icon in the menu bar reopens the hub, toggles the overlay, checks for updates, or quits.

Builds are ad-hoc signed. If macOS blocks the first open, right-click the app and choose **Open**, or run:

```bash
xattr -dr com.apple.quarantine /Applications/NiceKeys.app
```

## The hub

| Tab | What you can do |
| --- | --- |
| **Keys** | Toggle W A S D, Q E R F, Shift / Ctrl / Alt, Space, mouse buttons (left, right, middle, back, forward, with CPS), scroll wheel up/down, hotbar 1-5, arrows. Presets, plus custom keys: press any key to add it. |
| **Layout** | Drag keys anywhere on a grid to build your own arrangement. Resize keys, rename them, nudge with steppers, snap to grid, and give any key its own colors (for example a green glow on W). The overlay updates live. Shift-click to multi-select and drag a group, nudge with the arrow keys, duplicate, align and distribute, undo and redo (Cmd+Z), and show a per-key press counter. One click resets to the automatic layout. |
| **Appearance** | Key size, spacing, corner radius, border, opacity, and colors for idle and pressed keys, with a live preview. Built-in themes (NiceKeys Dark, Lunar Glass, Neon Green, Ice Blue, Sunset, Minimal Light) plus theme import/export as JSON. Effects: press animation, glow, a typing and shortcut strip (off by default), and fade-out when idle. |
| **Position** | 9 snap points, drag-to-move mode, sliders, and a display picker for multiple monitors, including **Follow active display**. |
| **Profiles** | Save complete setups (keys, layout, colors, effects, position), such as "Minecraft", "Recording" and "Minimal", and switch between them here or from the menu bar. Optionally assign apps to a profile and NiceKeys switches automatically when you focus them. |
| **General** | Global show/hide shortcut (default Control+Option+K), apps where the overlay is hidden (privacy), launch at login, optional Dock icon, report a bug, update checks, permission status, reset, quit. |

## Updates

NiceKeys checks <https://github.com/NiceSky-Studios/NiceKeys/releases> for a newer version.

- Automatic check at launch, at most **once every 24 hours** (one small request, no background polling).
  Turn it off in **General > Updates**.
- **Check now** in the hub, or **Check for Updates...** in the menu bar menu, checks on demand.
- When a newer release exists, the hub sidebar and menu bar menu show **Download <version>**, which opens
  the release page. NiceKeys doesn't install updates silently: you download the new `.dmg` yourself.
- The only data sent is a standard request to GitHub's public API with a `NiceKeys/<version>` user agent.

## Why it's light

There is no polling and no per-frame redraw. A listen-only event tap wakes the app only for key and
mouse-button events, and each event updates just the affected Core Animation layers. Timers are
rare and short-lived: one for about a second after clicking (CPS), one lazy timer for fade-when-idle if
you turn it on, a 0.3s check that runs only while a key is physically held, a brief pulse timer only while you scroll, and a one-shot timer that clears the typing strip. Scroll events are only subscribed to when a scroll key is enabled, and typed text is only read when the typing strip is on. Per-app features use one notification per app switch, not polling. Closing the hub frees its SwiftUI view tree.

## Changelog

**v1.3.0**
- **Per-app profiles**: assign apps to a profile and NiceKeys switches automatically, then switches back.
- **Hide in specific apps**: a blocklist for password managers, banking apps and anything else private.
- **Typing and shortcut strip**: optional line above the keys showing recent text and shortcuts like Cmd+Shift+4.
- **More inputs**: middle click, mouse back/forward buttons, scroll wheel up/down, and optional per-key press counters.
- **Follow active display** for multi-monitor setups.
- **Layout editor**: multi-select, undo/redo, arrow-key nudging, duplicate, align and distribute.
- **Themes**: six built-in themes and import/export of your own.
- **First-run walkthrough** for the Input Monitoring permission.
- Optional Dock icon, relaunch button, report-a-bug link.
- `build.sh`: signing, notarization (`NOTARIZE=1`) and a Homebrew cask generator (`./build.sh cask`).
- Settings from earlier versions are carried over automatically.

**v1.2.0**
- **Profiles**: save and switch complete setups from the hub or the menu bar.
- **Global shortcut** to show or hide the overlay (default Control+Option+K, changeable in General).
- **Fade when idle**: the overlay dims or hides after a few seconds without input.
- **Press animation and glow**: keys dip slightly when pressed, with an optional glow in the pressed color.
- **Stuck-key protection**: keys can no longer stay lit after a missed key-up (Cmd+Tab, lock screen, sleep).
- Settings from v1.1.0 and v1.0.0 are carried over automatically.

**v1.1.0**
- New **Layout** tab: drag keys anywhere, resize and rename them, snap to grid, reset to automatic.
- Per-key colors: override background, pressed, text and pressed-text color on any key.
- Settings from v1.0.0 are carried over automatically.

**v1.0.0**
- First release: always-on-top click-through overlay, selectable keys, the NiceKeys hub, appearance and
  position controls, launch at login, and update checks.

## Project layout

```
build.sh                  build + .dmg packaging
Sources/NiceKeys/         Swift sources (overlay, key listener, hub, update checker)
scripts/icon/             app icon generator
.vscode/tasks.json        build tasks
```
