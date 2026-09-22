# Android touch research notes

## Decisions

- Prefer Qt Quick for newly written mobile shell controls because Qt documents it as the fluid, GPU-accelerated path for touch UIs.
- Keep the existing CAD viewport initially. A full QML scene rewrite would mix rendering, coordinate transforms, and feature migration in one risky change.
- Use a gesture state machine rather than mapping every touch to a synthetic mouse event. CAD actions need explicit handling for one-finger drawing versus two-finger navigation.
- Preserve a desktop-friendly input path so Linux mouse and tablet behavior remains unchanged.

## Initial gesture contract

| Gesture | CAD action |
| --- | --- |
| One finger tap | Select / snap point |
| One finger drag after tool activation | Draw or edit current entity |
| Two finger drag | Pan |
| Pinch | Zoom about gesture centroid |
| Long press | Context actions / entity info |
| Two-finger tap | Cancel current action |

This contract is provisional and must be validated on a real device before being treated as stable UX.
