# NiceKeys

![version](https://img.shields.io/badge/version-v1.2.0-5AD98C) ![macOS](https://img.shields.io/badge/macOS-13%2B-lightgrey) ![swift](https://img.shields.io/badge/Swift-5.9-orange)

**Current version: v1.2.0**  ([release notes](RELEASE_NOTES_v1.2.0.md), [changelog](#changelog))

A tiny macOS keystroke overlay in the style of Lunar Client's keystrokes mod. Dark, semi-transparent keys
(WASD, Shift, Space, mouse buttons with clicks per second, and more) float above every app, including
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

1. The hub window opens. macOS asks for **Input Monitoring**: allow NiceKeys in
   System Settings > Privacy & Security > Input Monitoring.
2. If the hub still says it needs permission after you allow it, quit and reopen NiceKeys once.
3. The keyboard icon in the menu bar reopens the hub, toggles the overlay, checks for updates, or quits.

Builds are ad-hoc signed. If macOS blocks the first open, right-click the app and choose **Open**, or run:

```bash
xattr -dr com.apple.quarantine /Applications/NiceKeys.app
```

## The hub

| Tab | What you can do |
| --- | --- |
| **Keys** | Toggle W A S D, Q E R F, Shift / Ctrl / Alt, Space, LMB / RMB (with CPS), hotbar 1-5, arrows. Presets, plus custom keys: press any key to add it. |
| **Layout** | Drag keys anywhere on a grid to build your own arrangement. Resize keys, rename them, nudge with steppers, snap to grid, and give any key its own colors (for example a green glow on W). The overlay updates live. One click resets to the automatic layout. |
| **Appearance** | Key size, spacing, corner radius, border, opacity, and colors for idle and pressed keys, with a live preview. Effects: press animation, glow on pressed keys, and fade-out when idle. |
| **Position** | 9 snap points, drag-to-move mode, sliders, and a display picker for multiple monitors. |
| **Profiles** | Save complete setups (keys, layout, colors, effects, position), such as "Minecraft", "Recording" and "Minimal", and switch between them here or from the menu bar. |
| **General** | Global show/hide shortcut (default Control+Option+K), launch at login, update checks, permission status, reset, quit. |

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
you turn it on, and a 0.3s check that runs only while a key is physically held. Closing the hub frees its SwiftUI view tree.

## Changelog

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
