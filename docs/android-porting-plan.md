# LibreCAD Android porting plan

## Target

- Android 9 (API 28) and newer
- ARMv7 (`armeabi-v7a`) and ARM64 (`arm64-v8a`)
- Private development repository: `ih532268-cell/librecad-android`

## Baseline audit (2026-09-22)

- Upstream snapshot: `a3ee08bd71c7a59e719c9b61c06ff93d58fea513`
- Upstream build uses Qt 6 modules: Core, Gui, Widgets, PrintSupport, Svg, Network, LinguistTools.
- The application is currently a Qt Widgets desktop application, not a Qt Quick/Kirigami app.
- The main drawing surface is `QG_GraphicView`, whose input path is primarily mouse events.
- Desktop assumptions requiring explicit Android work include dock widgets/toolbars, native file dialogs, printing, clipboard, desktop settings, keyboard shortcuts, and cursor handling.
- The geometry/document/file libraries are the best candidates for reuse without a UI rewrite.

## Strategy

1. Establish a reproducible Linux build and test baseline before changing behavior.
2. Add an Android build lane in CI for both ARM ABIs; target API 28 minimum.
3. First produce a functional APK with the existing Widgets UI wherever Qt Android supports it.
4. Add a mobile input adapter around `QG_GraphicView`: single-finger selection, two-finger pan, pinch zoom, and long-press context actions. Keep CAD coordinates and snapping in the existing engine.
5. Replace the desktop-first shell incrementally: mobile toolbar, bottom action bar, mobile file open/save through Android document URIs, and touch-sized controls.
6. Keep desktop builds and tests green on every change.

## Touch/UI references

- Qt for Android: https://doc.qt.io/qt-6/android.html
- Qt touch input guidance: https://doc.qt.io/qt-6/touchinputexamples.html
- KDE Kirigami adaptive controls: https://github.com/KDE/kirigami
- Kasts, a convergent Qt/Kirigami app with Android packaging: https://github.com/KDE/kasts
- Qt Android JNI integration: https://doc.qt.io/qt-6/android-services.html

## Non-goals for the first milestone

- Full feature parity with every desktop dialog.
- DWG support changes.
- 3D rendering.
- Replacing all Widgets with QML in one step.

## Milestones

### M0 — baseline

- Linux configure/build/test documented and reproducible.
- Static inventory of desktop-only APIs and Android blockers.
- CI skeleton for Android toolchain validation.

### M1 — Android shell

- Qt Android application package builds for `arm64-v8a` and `armeabi-v7a`.
- App launches, opens bundled sample DXF, renders, and exits cleanly.
- API 28 minimum is enforced.

### M2 — touch drawing MVP

- Pan, pinch zoom, tap selection, long press, and undo/redo.
- Touch-friendly primary toolbar.
- Open/save through Android document picker.

### M3 — usable CAD MVP

- Lines, circles, trim, move/copy, layers, snapping, dimensions.
- Import/export DXF and SVG/PNG where supported.
- Release APKs and regression fixtures.

## Risk controls

- Do not modify geometry algorithms while solving packaging or input.
- Add focused tests for every coordinate-transform change.
- Keep Android-specific code behind small interfaces and compile guards.
- Validate both ARM ABIs in CI; never assume arm64 success implies armv7 success.
