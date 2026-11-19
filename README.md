# NiceKeys

![version](https://img.shields.io/badge/version-v1.0.0-5AD98C) ![macOS](https://img.shields.io/badge/macOS-13%2B-lightgrey) ![swift](https://img.shields.io/badge/Swift-5.9-orange)

**Current version: v1.0.0**

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
| **Appearance** | Key size, spacing, corner radius, border, opacity, and colors for idle and pressed keys, with a live preview. |
| **Position** | 9 snap points, drag-to-move mode, sliders, and a display picker for multiple monitors. |
| **General** | Launch at login, update checks, permission status, reset, quit. |

## Updates

NiceKeys checks <https://github.com/NiceSky-Studios/NiceKeys/releases> for a newer version.

- Automatic check at launch, at most **once every 24 hours** (one small request, no background polling).
  Turn it off in **General > Updates**.
- **Check now** in the hub, or **Check for Updates...** in the menu bar menu, checks on demand.
- When a newer release exists, the hub sidebar and menu bar menu show **Download <version>**, which opens
  the release page. NiceKeys doesn't install updates silently: you download the new `.dmg` yourself.
- The only data sent is a standard request to GitHub's public API with a `NiceKeys/<version>` user agent.

## Build from source

Needs macOS 13+ and the Xcode command line tools (`xcode-select --install`). No Xcode project required.

```bash
cd NiceKeys
./build.sh              # arm64 + x86_64 + universal .dmg files -> dist/
```

| Command | Result |
| --- | --- |
| `./build.sh` or `./build.sh all` | All three `.dmg` files |
| `./build.sh arm64` / `x86_64` / `universal` | One `.dmg` |
| `./build.sh run` | Build and launch for this Mac (no `.dmg`) |
| `./build.sh clean` | Remove `build/` and `dist/` |

`VERSION=1.1.0 ./build.sh` sets the version in the app and the `.dmg` names. VS Code tasks are included
(Cmd+Shift+B). If the script isn't executable, run `bash build.sh`.

Rebuilding changes the ad-hoc signature, so macOS may ask you to re-grant Input Monitoring after a rebuild.
For distribution, sign with `SIGN_IDENTITY="Developer ID Application: ..." ./build.sh` and notarize.

## Publishing a release

The update checker compares the **tag** of the latest GitHub release with the app's version, so tag releases
as `vMAJOR.MINOR.PATCH` and build with the same number.

```bash
VERSION=1.0.0 ./build.sh
gh release create v1.0.0 dist/*.dmg --title "NiceKeys v1.0.0" --notes-file RELEASE_NOTES_v1.0.0.md
```

## Why it's light

There is no polling and no per-frame redraw. A listen-only event tap wakes the app only for key and
mouse-button events, and each event updates just the affected Core Animation layers. The only timer runs
for about a second after clicking, to update CPS. Closing the hub frees its SwiftUI view tree.

## Project layout

```
build.sh                  build + .dmg packaging
Sources/NiceKeys/         Swift sources (overlay, key listener, hub, update checker)
scripts/icon/             app icon generator
.vscode/tasks.json        build tasks
```
